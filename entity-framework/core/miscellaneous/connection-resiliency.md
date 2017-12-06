---
title: "Elastyczność połączenia - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a>Elastyczność połączenia

Elastyczność połączenia automatycznie ponowi próbę polecenia bazy danych nie powiodło się. Funkcja może być używana z żadną bazą danych, podając "Strategia wykonywania", który hermetyzuje logiki niezbędne do wykrywania błędów i ponów próbę wykonania polecenia. EF Core dostawców można podać dostosowane do ich awarię określonej bazy danych i zasady ponawiania optymalnej strategii wykonywania.

Na przykład dostawcy programu SQL Server zawiera strategii wykonanie, specjalnie dostosowane do programu SQL Server (w tym usług SQL Azure). Jest znane typy wyjątków, które można ponowić i ma za pośrednictwem ustawień domyślnych, maksymalna liczba ponownych prób, opóźnienie między ponownych prób itp.

Strategia wykonywania została określona podczas konfigurowania opcji dla kontekstu. Jest to zazwyczaj w `OnConfiguring` metodę kontekst pochodnej, albo w `Startup.cs` dla aplikacji platformy ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Strategia wykonywania niestandardowych

Brak mechanizmu zarejestrować strategii wykonywania niestandardowych swoje własne, jeśli chcesz zmienić dowolne z ustawień domyślnych.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Strategie wykonywania i transakcji

Strategii wykonywania, która ma automatycznie ponawiać próbę na awarie musi mieć możliwość odtwarzania każdej operacji w bloku ponownych prób, który zakończy się niepowodzeniem. Po włączeniu ponownych prób każdej operacji wykonywania za pośrednictwem EF Core staje się możliwością ponowienia próby działania, np. każde zapytanie i każde wywołanie `SaveChanges()` zostanie ponowiona jako jednostki, jeśli wystąpi błąd przejściowy.

Jednak jeśli inicjuje kodu przy użyciu transakcji `BeginTransaction()` definiowania grupy działań, które muszą być traktowane jako jednostka, tj. wszystkie elementy wewnątrz transakcji musiałaby zostać odtworzone następuje awaria. Otrzymasz wyjątek w następujący, jeśli użytkownik spróbuje to zrobić, korzystając z strategia wykonywania.

> InvalidOperationException: Skonfigurowana strategia wykonywania "SqlServerRetryingExecutionStrategy" nie obsługuje transakcji inicjowanych przez użytkownika. Strategia wykonywania zwróconych przez "DbContext.Database.CreateExecutionStrategy()" umożliwia wykonywanie wszystkich operacji w transakcji jako jednostki można spróbować ponownie.

Rozwiązanie to ręczne wywoływanie strategia wykonywania z delegatem reprezentujący wszystko, co ma zostać wykonana. Jeśli wystąpi błąd przejściowy, strategia wykonywania zostaną wywołane delegat ponownie.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Błąd zatwierdzania transakcji, a problem idempotency

Ogólnie rzecz biorąc po awarii połączenia bieżąca transakcja zostanie wycofana. Jednak jeśli połączenie zostało przerwane, gdy transakcja jest jest zatwierdzona powstałe w ten sposób stan transakcji jest nieznany. Zobacz to [wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) więcej szczegółów.

Domyślnie strategia wykonywania ponowi operację tak, jakby transakcja została wycofana, ale jeśli nie jest to spowoduje wyjątek nowy stan bazy danych jest niezgodny lub może prowadzić do **uszkodzenie danych** Jeśli Operacja nie bazuje na określonym stanie, na przykład podczas usuwania wierszy z automatycznego generowania wartości klucza.

Istnieje kilka sposobów postępowania z nią.

### <a name="option-1---do-almost-nothing"></a>Opcja 1 — czy nothing (prawie)

Prawdopodobieństwo błąd połączenia podczas zatwierdzania transakcji jest niska, dlatego może być dopuszczalne tylko się niepowodzeniem, jeśli ten stan występuje, faktycznie aplikacji.

Należy jednak unikać generowanych przez magazyn kluczy w celu zapewnienia, jest zwracany wyjątek, zamiast dodawania zduplikowany wiersz. Należy rozważyć użycie wartość identyfikatora GUID generowany przez klienta lub generator wartości po stronie klienta.

### <a name="option-2---rebuild-application-state"></a>Opcja 2 — stan aplikacji Skompiluj ponownie

1. Odrzucenie bieżącej `DbContext`.
2. Utwórz nową `DbContext` i przywrócenia stanu aplikacji z bazy danych.
3. Poinformuj użytkownika, że ostatnia operacja może nie zostały zakończone pomyślnie.

### <a name="option-3---add-state-verification"></a>Opcja 3 — Dodawanie stanu weryfikacji

Dla większości działań, które spowodują zmianę stanu bazy danych jest możliwe, Dodaj kod, który sprawdza, czy powiodło się. EF udostępnia metodę rozszerzenia, aby ułatwić - `IExecutionStrategy.ExecuteInTransaction`.

Ta metoda rozpoczyna się i zatwierdza transakcji i akceptuje również funkcję `verifySucceeded` parametr, który jest wywoływany po wystąpieniu błędu przejściowego podczas zatwierdzania transakcji.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> W tym miejscu `SaveChanges` została wywołana z `acceptAllChangesOnSuccess` ustawioną `false` Aby uniknąć zmieniania stanu `Blog` jednostki do `Unchanged` Jeśli `SaveChanges` zakończy się pomyślnie. Dzięki temu Ponów tę samą operację, jeśli zatwierdzanie nie powiedzie się i transakcja zostanie wycofana.

### <a name="option-4---manually-track-the-transaction"></a>Opcja 4 - ręcznie śledzić transakcji

Jeśli musisz używać generowanych przez magazyn kluczy lub potrzebujesz ogólny sposób obsługi niepowodzeń zatwierdzenia, który nie jest zależny od operacje wykonywane każdej transakcji może zostać przypisana Identyfikatora, który jest sprawdzana podczas zatwierdzenia nie powiedzie się.

1. Dodawanie tabeli do bazy danych używane do śledzenia stanu transakcji.
2. Wstaw wiersz do tabeli na początku każdej transakcji.
3. Jeśli połączenie zatwierdzenia zakończy się niepowodzeniem, sprawdź obecność odpowiedni wiersz w bazie danych.
4. Jeśli zatwierdzenia zakończy się pomyślnie, usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Upewnij się, że kontekst używany do weryfikacji ma strategia wykonywania zdefiniowany jako połączenia prawdopodobnie może zakończyć się niepowodzeniem podczas weryfikacji, jeśli go nie powiodła się podczas zatwierdzania transakcji.
