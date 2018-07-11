---
title: Elastyczność połączenia — EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 34ca1908257ed5544f2e134fa7686c9802fcebea
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949301"
---
# <a name="connection-resiliency"></a>Elastyczność połączenia

Elastyczność połączenia automatycznie ponawia próbę polecenia bazy danych nie powiodło się. Funkcja może być używana z żadną bazą danych, podając "strategii wykonywania", która hermetyzuje logikę niezbędną do wykrywania błędów i ponów próbę wykonania polecenia. EF Core dostawców można podać strategii wykonywania dopasowane do swoich warunków błędów konkretnej bazy danych i zasady ponawiania optymalne.

Na przykład dostawca programu SQL Server zawiera strategii wykonywania, który jest w szczególny sposób dopasowane do programu SQL Server (w tym usługi SQL Azure). Ją rozpoznaje rodzaje wyjątków, które mogą być ponawiane i posiada odpowiednie ustawienia domyślne, aby uzyskać maksymalną liczbę ponownych prób, opóźnienie między ponownych prób.

Strategia wykonywania jest określony podczas konfigurowania opcji dla kontekstu. Jest to zazwyczaj w `OnConfiguring` metody pochodnej kontekstu lub w `Startup.cs` dla aplikacji platformy ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Strategia wykonywania niestandardowych

Istnieje mechanizm do zarejestrowania strategii wykonywania niestandardowych samodzielnie, jeśli chcesz zmienić dowolne z ustawień domyślnych.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Strategii wykonywania i transakcji

Strategii wykonywania, który automatycznie ponawia próbę na błędy musi być w stanie odtworzyć każdej operacji w bloku ponawiania, która kończy się niepowodzeniem. Po włączeniu ponownych prób każdej operacji wykonywanych przy użyciu programu EF Core staje się własną wywołały operacji. Oznacza to, każde zapytanie i każde wywołanie `SaveChanges()` zostanie ponowiona jako jednostki, jeśli wystąpi błąd przejściowy.

Jednakże jeżeli Twój kod inicjuje transakcji przy użyciu `BeginTransaction()` definiujesz własną grupę działań, które muszą być traktowane jako jednostka i wszystko wewnątrz transakcji musi być odtwarzane ma miejsce awaria. Jeśli spróbuje to zrobić, korzystając z strategii wykonywania, zostanie wyświetlony następujący wyjątek:

> InvalidOperationException: Strategia wykonywania skonfigurowany "SqlServerRetryingExecutionStrategy" nie obsługuje transakcji zainicjowanej przez użytkownika. Strategia wykonywania zwróconych przez "DbContext.Database.CreateExecutionStrategy()" umożliwia wykonywanie wszystkich operacji w transakcji jako jednostka z możliwością ponowienia próby.

To rozwiązanie jest ręcznie wywołać strategii wykonywania z delegatem reprezentujący wszystko, co ma zostać wykonana. Jeśli wystąpi błąd przejściowy, strategia wykonywania wywoła delegata ponownie.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Błąd zatwierdzania transakcji i problem idempotentności

Ogólnie rzecz biorąc gdy wystąpi awaria połączenia bieżąca transakcja zostanie wycofana. Jednakże, jeśli połączenie zostało przerwane, gdy transakcja jest zwrócenia zatwierdzone Wynikowy stan transakcji jest nieznany. Zobacz ten [wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) Aby uzyskać więcej informacji.

Domyślnie strategii wykonywania ponowi operację tak, jakby transakcja została wycofana, ale jeśli nie jest to wynikiem będzie wyjątek nowy stan bazy danych jest niezgodny lub może prowadzić do **uszkodzenie danych** Jeśli Operacja nie zależą od określonego stanu, na przykład podczas wstawiania nowego wiersza przy użyciu automatycznego generowania wartości klucza.

Istnieje kilka sposobów, aby poradzić sobie z tym.

### <a name="option-1---do-almost-nothing"></a>Opcja 1 — czy (prawie) nothing

Prawdopodobieństwo awarii połączenia podczas zatwierdzania transakcji jest niska, dlatego może być akceptowalne, aby aplikacja została właśnie się niepowodzeniem, jeśli rzeczywiście występuje ten problem.

Jednak należy unikać używania generowane przez magazyn kluczy w celu zapewnienia, że zamiast opcji dodawania zduplikowany wiersz jest zgłaszany wyjątek. Należy rozważyć użycie wartość identyfikatora GUID generowany przez klienta lub generator wartości po stronie klienta.

### <a name="option-2---rebuild-application-state"></a>Opcja 2 — stan aplikacji ponownej kompilacji

1. Odrzucenie aktualnego `DbContext`.
2. Utwórz nową `DbContext` i przywrócenia stanu aplikacji z bazy danych.
3. Informuje użytkownika, że ostatnia operacja może nie zostały zakończone pomyślnie.

### <a name="option-3---add-state-verification"></a>Opcja 3 — Dodawanie weryfikacji do stanu

Dla większości działań, które zmieniają stan bazy danych jest można dodać kod, który sprawdza, czy powiodła się. EF udostępnia metodę rozszerzenia, aby to ułatwić - `IExecutionStrategy.ExecuteInTransaction`.

Ta metoda rozpoczyna się i zatwierdzeń transakcji i akceptuje także funkcję `verifySucceeded` parametrów, które jest wywoływane, gdy wystąpi błąd przejściowy podczas zatwierdzania transakcji.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> W tym miejscu `SaveChanges` jest wywoływana z `acceptAllChangesOnSuccess` równa `false` Aby uniknąć zmieniania stanu `Blog` jednostki do `Unchanged` Jeśli `SaveChanges` zakończy się pomyślnie. Dzięki temu, aby ponowić próbę wykonania tej samej operacji, jeśli zatwierdzenie zakończy się niepowodzeniem, a transakcja zostanie wycofana.

### <a name="option-4---manually-track-the-transaction"></a>Opcja 4 — ręcznie śledzić transakcji

Jeśli musisz używać generowane przez magazyn kluczy lub potrzebujesz ogólny sposób obsługi błędów zatwierdzania, który nie są zależne od operacji wykonywanych każdej transakcji można przypisać Identyfikatora, który jest sprawdzany, jeśli zatwierdzenie zakończy się niepowodzeniem.

1. Dodaj tabelę w bazie danych używane do śledzenia stanu transakcji.
2. Wstaw wiersz do tabeli na początku każdej transakcji.
3. Jeśli połączenie nie powiedzie się podczas zatwierdzania, sprawdź, czy obecność odpowiedni wiersz w bazie danych.
4. Jeśli zatwierdzenie zakończy się pomyślnie, usuń odpowiedni wiersz w celu uniknięcia wzrostu tabeli.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Upewnij się, że kontekst użyty w celu weryfikacji ma zdefiniowany jako połączenie jest prawdopodobnie nastąpi ich awaria ponownie podczas weryfikacji w przypadku niepowodzenia podczas zatwierdzania transakcji strategii wykonywania.
