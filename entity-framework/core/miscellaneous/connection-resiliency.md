---
title: Elastyczność połączenia - EF Core
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
ms.locfileid: "26054557"
---
# <a name="connection-resiliency"></a><span data-ttu-id="62423-102">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="62423-102">Connection Resiliency</span></span>

<span data-ttu-id="62423-103">Elastyczność połączenia automatycznie ponowi próbę polecenia bazy danych nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="62423-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="62423-104">Funkcja może być używana z żadną bazą danych, podając "Strategia wykonywania", który hermetyzuje logiki niezbędne do wykrywania błędów i ponów próbę wykonania polecenia.</span><span class="sxs-lookup"><span data-stu-id="62423-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="62423-105">EF Core dostawców można podać dostosowane do ich awarię określonej bazy danych i zasady ponawiania optymalnej strategii wykonywania.</span><span class="sxs-lookup"><span data-stu-id="62423-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="62423-106">Na przykład dostawcy programu SQL Server zawiera strategii wykonanie, specjalnie dostosowane do programu SQL Server (w tym usług SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="62423-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="62423-107">Jest znane typy wyjątków, które można ponowić i ma za pośrednictwem ustawień domyślnych, maksymalna liczba ponownych prób, opóźnienie między ponownych prób itp.</span><span class="sxs-lookup"><span data-stu-id="62423-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="62423-108">Strategia wykonywania została określona podczas konfigurowania opcji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="62423-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="62423-109">Jest to zazwyczaj w `OnConfiguring` metodę kontekst pochodnej, albo w `Startup.cs` dla aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62423-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="62423-110">Strategia wykonywania niestandardowych</span><span class="sxs-lookup"><span data-stu-id="62423-110">Custom execution strategy</span></span>

<span data-ttu-id="62423-111">Brak mechanizmu zarejestrować strategii wykonywania niestandardowych swoje własne, jeśli chcesz zmienić dowolne z ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="62423-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="62423-112">Strategie wykonywania i transakcji</span><span class="sxs-lookup"><span data-stu-id="62423-112">Execution strategies and transactions</span></span>

<span data-ttu-id="62423-113">Strategii wykonywania, która ma automatycznie ponawiać próbę na awarie musi mieć możliwość odtwarzania każdej operacji w bloku ponownych prób, który zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="62423-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="62423-114">Po włączeniu ponownych prób każdej operacji wykonywania za pośrednictwem EF Core staje się możliwością ponowienia próby działania, np. każde zapytanie i każde wywołanie `SaveChanges()` zostanie ponowiona jako jednostki, jeśli wystąpi błąd przejściowy.</span><span class="sxs-lookup"><span data-stu-id="62423-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="62423-115">Jednak jeśli inicjuje kodu przy użyciu transakcji `BeginTransaction()` definiowania grupy działań, które muszą być traktowane jako jednostka, tj. wszystkie elementy wewnątrz transakcji musiałaby zostać odtworzone następuje awaria.</span><span class="sxs-lookup"><span data-stu-id="62423-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="62423-116">Otrzymasz wyjątek w następujący, jeśli użytkownik spróbuje to zrobić, korzystając z strategia wykonywania.</span><span class="sxs-lookup"><span data-stu-id="62423-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="62423-117">InvalidOperationException: Skonfigurowana strategia wykonywania "SqlServerRetryingExecutionStrategy" nie obsługuje transakcji inicjowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="62423-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="62423-118">Strategia wykonywania zwróconych przez "DbContext.Database.CreateExecutionStrategy()" umożliwia wykonywanie wszystkich operacji w transakcji jako jednostki można spróbować ponownie.</span><span class="sxs-lookup"><span data-stu-id="62423-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="62423-119">Rozwiązanie to ręczne wywoływanie strategia wykonywania z delegatem reprezentujący wszystko, co ma zostać wykonana.</span><span class="sxs-lookup"><span data-stu-id="62423-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="62423-120">Jeśli wystąpi błąd przejściowy, strategia wykonywania zostaną wywołane delegat ponownie.</span><span class="sxs-lookup"><span data-stu-id="62423-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="62423-121">Błąd zatwierdzania transakcji, a problem idempotency</span><span class="sxs-lookup"><span data-stu-id="62423-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="62423-122">Ogólnie rzecz biorąc po awarii połączenia bieżąca transakcja zostanie wycofana.</span><span class="sxs-lookup"><span data-stu-id="62423-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="62423-123">Jednak jeśli połączenie zostało przerwane, gdy transakcja jest jest zatwierdzona powstałe w ten sposób stan transakcji jest nieznany.</span><span class="sxs-lookup"><span data-stu-id="62423-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="62423-124">Zobacz to [wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="62423-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="62423-125">Domyślnie strategia wykonywania ponowi operację tak, jakby transakcja została wycofana, ale jeśli nie jest to spowoduje wyjątek nowy stan bazy danych jest niezgodny lub może prowadzić do **uszkodzenie danych** Jeśli Operacja nie bazuje na określonym stanie, na przykład podczas usuwania wierszy z automatycznego generowania wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="62423-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="62423-126">Istnieje kilka sposobów postępowania z nią.</span><span class="sxs-lookup"><span data-stu-id="62423-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="62423-127">Opcja 1 — czy nothing (prawie)</span><span class="sxs-lookup"><span data-stu-id="62423-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="62423-128">Prawdopodobieństwo błąd połączenia podczas zatwierdzania transakcji jest niska, dlatego może być dopuszczalne tylko się niepowodzeniem, jeśli ten stan występuje, faktycznie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="62423-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="62423-129">Należy jednak unikać generowanych przez magazyn kluczy w celu zapewnienia, jest zwracany wyjątek, zamiast dodawania zduplikowany wiersz.</span><span class="sxs-lookup"><span data-stu-id="62423-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="62423-130">Należy rozważyć użycie wartość identyfikatora GUID generowany przez klienta lub generator wartości po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="62423-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="62423-131">Opcja 2 — stan aplikacji Skompiluj ponownie</span><span class="sxs-lookup"><span data-stu-id="62423-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="62423-132">Odrzucenie bieżącej `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="62423-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="62423-133">Utwórz nową `DbContext` i przywrócenia stanu aplikacji z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="62423-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="62423-134">Poinformuj użytkownika, że ostatnia operacja może nie zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="62423-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="62423-135">Opcja 3 — Dodawanie stanu weryfikacji</span><span class="sxs-lookup"><span data-stu-id="62423-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="62423-136">Dla większości działań, które spowodują zmianę stanu bazy danych jest możliwe, Dodaj kod, który sprawdza, czy powiodło się.</span><span class="sxs-lookup"><span data-stu-id="62423-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="62423-137">EF udostępnia metodę rozszerzenia, aby ułatwić - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="62423-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="62423-138">Ta metoda rozpoczyna się i zatwierdza transakcji i akceptuje również funkcję `verifySucceeded` parametr, który jest wywoływany po wystąpieniu błędu przejściowego podczas zatwierdzania transakcji.</span><span class="sxs-lookup"><span data-stu-id="62423-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="62423-139">W tym miejscu `SaveChanges` została wywołana z `acceptAllChangesOnSuccess` ustawioną `false` Aby uniknąć zmieniania stanu `Blog` jednostki do `Unchanged` Jeśli `SaveChanges` zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="62423-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="62423-140">Dzięki temu Ponów tę samą operację, jeśli zatwierdzanie nie powiedzie się i transakcja zostanie wycofana.</span><span class="sxs-lookup"><span data-stu-id="62423-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="62423-141">Opcja 4 - ręcznie śledzić transakcji</span><span class="sxs-lookup"><span data-stu-id="62423-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="62423-142">Jeśli musisz używać generowanych przez magazyn kluczy lub potrzebujesz ogólny sposób obsługi niepowodzeń zatwierdzenia, który nie jest zależny od operacje wykonywane każdej transakcji może zostać przypisana Identyfikatora, który jest sprawdzana podczas zatwierdzenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="62423-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="62423-143">Dodawanie tabeli do bazy danych używane do śledzenia stanu transakcji.</span><span class="sxs-lookup"><span data-stu-id="62423-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="62423-144">Wstaw wiersz do tabeli na początku każdej transakcji.</span><span class="sxs-lookup"><span data-stu-id="62423-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="62423-145">Jeśli połączenie zatwierdzenia zakończy się niepowodzeniem, sprawdź obecność odpowiedni wiersz w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="62423-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="62423-146">Jeśli zatwierdzenia zakończy się pomyślnie, usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.</span><span class="sxs-lookup"><span data-stu-id="62423-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="62423-147">Upewnij się, że kontekst używany do weryfikacji ma strategia wykonywania zdefiniowany jako połączenia prawdopodobnie może zakończyć się niepowodzeniem podczas weryfikacji, jeśli go nie powiodła się podczas zatwierdzania transakcji.</span><span class="sxs-lookup"><span data-stu-id="62423-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
