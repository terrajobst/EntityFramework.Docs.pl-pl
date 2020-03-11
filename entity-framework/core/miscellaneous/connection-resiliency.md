---
title: Odporność połączenia — EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416642"
---
# <a name="connection-resiliency"></a>Elastyczność połączenia

Odporność połączenia automatycznie ponawia próby nieudane polecenia bazy danych. Funkcja może być używana z dowolną bazą danych, dostarczając "strategię wykonywania", która hermetyzuje logikę niezbędną do wykrywania błędów i ponawiania prób. Dostawcy EF Core mogą dostarczać strategie wykonywania dostosowane do ich określonych warunków awarii bazy danych i optymalne zasady ponawiania.

Na przykład dostawca SQL Server obejmuje strategię wykonywania, która jest specjalnie dostosowana do SQL Server (w tym SQL Azure). Jest on świadomy typów wyjątków, które mogą być ponawiane i ma rozsądne wartości domyślne dla maksymalnej liczby ponownych prób, opóźnienia między kolejnymi próbami itd.

Podczas konfigurowania opcji dla kontekstu określono strategię wykonywania. Zwykle jest to metoda `OnConfiguring` w kontekście pochodnym:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

lub w `Startup.cs` dla aplikacji ASP.NET Core:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Niestandardowa strategia wykonywania

Istnieje mechanizm, aby zarejestrować własną niestandardową strategię wykonywania, jeśli chcesz zmienić ustawienia domyślne.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Strategie wykonywania i transakcje

Strategia wykonywania, która automatycznie ponawia próbę w przypadku awarii, musi być w stanie odtworzyć każdą operację w bloku ponawiania, który zakończy się niepowodzeniem. Po włączeniu ponownych prób każda operacja wykonywana za pośrednictwem EF Core będzie własną operacją wywołały. Oznacza to, że każde zapytanie i każde wywołanie `SaveChanges()` zostanie ponowione w przypadku wystąpienia błędu przejściowego.

Jeśli jednak kod inicjuje transakcję przy użyciu `BeginTransaction()` definiujesz własną grupę operacji, która musi być traktowana jako jednostka, a wszystkie elementy wewnątrz transakcji byłyby potrzebne do odtworzenia. Jeśli podjęto próbę wykonania tej czynności podczas korzystania z strategii wykonywania, zostanie wyświetlony wyjątek:

> InvalidOperationException: skonfigurowana strategia wykonywania "SqlServerRetryingExecutionStrategy" nie obsługuje transakcji inicjowanych przez użytkownika. Użyj strategii wykonywania zwróconej przez obiekt "DbContext. Database. CreateExecutionStrategy ()", aby wykonać wszystkie operacje w transakcji jako jednostkę wywołały.

Rozwiązaniem jest ręczne wywołanie strategii wykonywania z delegatem reprezentującym wszystkie elementy, które należy wykonać. Jeśli wystąpi błąd przejściowy, strategia wykonywania ponownie wywoła delegata.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Takie podejście może być również używane z transakcjami otoczenia.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Błąd zatwierdzania transakcji i problem z idempotentności

Ogólnie rzecz biorąc, gdy występuje błąd połączenia, bieżąca transakcja jest wycofywana. Jeśli jednak połączenie zostanie usunięte podczas zatwierdzania transakcji, stan wynikający z transakcji jest nieznany. 

Domyślnie strategia wykonywania ponowi próbę wykonania operacji tak, jakby transakcja została wycofana, ale jeśli nie jest to przypadek, spowoduje to wyjątek, jeśli nowy stan bazy danych jest niezgodny lub może doprowadzić do **uszkodzenia danych** , jeśli operacja nie zależy od określonego stanu, na przykład podczas wstawiania nowego wiersza z automatycznie generowanymi wartościami klucza.

Można to zrobić na kilka sposobów.

### <a name="option-1---do-almost-nothing"></a>Opcja 1 — do (prawie) nic

Prawdopodobieństwo błędu połączenia podczas zatwierdzania transakcji jest niskie, dlatego może być akceptowalne, aby aplikacja mogła zakończyć się niepowodzeniem, jeśli ten stan rzeczywiście wystąpi.

Należy jednak unikać używania kluczy generowanych przez magazyn w celu zapewnienia, że wyjątek jest zgłaszany zamiast dodawania zduplikowanego wiersza. Rozważ użycie wygenerowanej przez klienta wartości GUID lub generatora wartości po stronie klienta.

### <a name="option-2---rebuild-application-state"></a>Opcja 2 — ponowne kompilowanie stanu aplikacji

1. Odrzuć bieżącą `DbContext`.
2. Utwórz nowy `DbContext` i Przywróć stan aplikacji z bazy danych.
3. Poinformuj użytkownika, że Ostatnia operacja mogła nie zostać ukończona pomyślnie.

### <a name="option-3---add-state-verification"></a>Opcja 3 — Dodawanie weryfikacji stanu

W przypadku większości operacji, które zmieniają stan bazy danych, można dodać kod, który sprawdza, czy zakończył się pomyślnie. EF udostępnia metodę rozszerzenia, aby ułatwić `IExecutionStrategy.ExecuteInTransaction`.

Ta metoda rozpoczyna i zatwierdza transakcję, a także akceptuje funkcję w `verifySucceeded` parametr, który jest wywoływany, gdy wystąpi błąd przejściowy podczas zatwierdzania transakcji.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> W tym miejscu `SaveChanges` jest wywoływana z `acceptAllChangesOnSuccess` ustawionym na `false`, aby uniknąć zmiany stanu jednostki `Blog` na `Unchanged`, jeśli `SaveChanges` się powiedzie. Pozwala to na ponowienie próby wykonania tej operacji, jeśli zatwierdzenie zakończy się niepowodzeniem, a transakcja zostanie wycofana.

### <a name="option-4---manually-track-the-transaction"></a>Opcja 4 — ręcznie Śledź transakcję

Jeśli konieczne jest użycie kluczy generowanych przez magazyn lub konieczny jest ogólny sposób obsługi niepowodzeń zatwierdzeń, które nie zależą od operacji wykonywanej przez każdą transakcję, można przypisać identyfikator, który jest sprawdzany, gdy zatwierdzenie zakończy się niepowodzeniem.

1. Dodaj tabelę do bazy danych używanej do śledzenia stanu transakcji.
2. Wstaw wiersz do tabeli na początku każdej transakcji.
3. Jeśli połączenie zakończy się niepowodzeniem podczas zatwierdzania, sprawdź obecność odpowiedniego wiersza w bazie danych.
4. Jeśli zatwierdzenie zakończy się pomyślnie, Usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Upewnij się, że kontekst używany do weryfikacji ma strategię wykonywania zdefiniowaną z chwilą, że połączenie będzie prawdopodobnie kończyć się niepowodzeniem, jeśli wystąpił błąd podczas zatwierdzania transakcji.
