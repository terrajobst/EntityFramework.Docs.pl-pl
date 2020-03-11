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
# <a name="connection-resiliency"></a><span data-ttu-id="aaec5-102">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="aaec5-102">Connection Resiliency</span></span>

<span data-ttu-id="aaec5-103">Odporność połączenia automatycznie ponawia próby nieudane polecenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aaec5-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="aaec5-104">Funkcja może być używana z dowolną bazą danych, dostarczając "strategię wykonywania", która hermetyzuje logikę niezbędną do wykrywania błędów i ponawiania prób.</span><span class="sxs-lookup"><span data-stu-id="aaec5-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="aaec5-105">Dostawcy EF Core mogą dostarczać strategie wykonywania dostosowane do ich określonych warunków awarii bazy danych i optymalne zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="aaec5-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="aaec5-106">Na przykład dostawca SQL Server obejmuje strategię wykonywania, która jest specjalnie dostosowana do SQL Server (w tym SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="aaec5-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="aaec5-107">Jest on świadomy typów wyjątków, które mogą być ponawiane i ma rozsądne wartości domyślne dla maksymalnej liczby ponownych prób, opóźnienia między kolejnymi próbami itd.</span><span class="sxs-lookup"><span data-stu-id="aaec5-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="aaec5-108">Podczas konfigurowania opcji dla kontekstu określono strategię wykonywania.</span><span class="sxs-lookup"><span data-stu-id="aaec5-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="aaec5-109">Zwykle jest to metoda `OnConfiguring` w kontekście pochodnym:</span><span class="sxs-lookup"><span data-stu-id="aaec5-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="aaec5-110">lub w `Startup.cs` dla aplikacji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="aaec5-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="aaec5-111">Niestandardowa strategia wykonywania</span><span class="sxs-lookup"><span data-stu-id="aaec5-111">Custom execution strategy</span></span>

<span data-ttu-id="aaec5-112">Istnieje mechanizm, aby zarejestrować własną niestandardową strategię wykonywania, jeśli chcesz zmienić ustawienia domyślne.</span><span class="sxs-lookup"><span data-stu-id="aaec5-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="aaec5-113">Strategie wykonywania i transakcje</span><span class="sxs-lookup"><span data-stu-id="aaec5-113">Execution strategies and transactions</span></span>

<span data-ttu-id="aaec5-114">Strategia wykonywania, która automatycznie ponawia próbę w przypadku awarii, musi być w stanie odtworzyć każdą operację w bloku ponawiania, który zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="aaec5-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="aaec5-115">Po włączeniu ponownych prób każda operacja wykonywana za pośrednictwem EF Core będzie własną operacją wywołały.</span><span class="sxs-lookup"><span data-stu-id="aaec5-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="aaec5-116">Oznacza to, że każde zapytanie i każde wywołanie `SaveChanges()` zostanie ponowione w przypadku wystąpienia błędu przejściowego.</span><span class="sxs-lookup"><span data-stu-id="aaec5-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="aaec5-117">Jeśli jednak kod inicjuje transakcję przy użyciu `BeginTransaction()` definiujesz własną grupę operacji, która musi być traktowana jako jednostka, a wszystkie elementy wewnątrz transakcji byłyby potrzebne do odtworzenia.</span><span class="sxs-lookup"><span data-stu-id="aaec5-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="aaec5-118">Jeśli podjęto próbę wykonania tej czynności podczas korzystania z strategii wykonywania, zostanie wyświetlony wyjątek:</span><span class="sxs-lookup"><span data-stu-id="aaec5-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="aaec5-119">InvalidOperationException: skonfigurowana strategia wykonywania "SqlServerRetryingExecutionStrategy" nie obsługuje transakcji inicjowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="aaec5-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="aaec5-120">Użyj strategii wykonywania zwróconej przez obiekt "DbContext. Database. CreateExecutionStrategy ()", aby wykonać wszystkie operacje w transakcji jako jednostkę wywołały.</span><span class="sxs-lookup"><span data-stu-id="aaec5-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="aaec5-121">Rozwiązaniem jest ręczne wywołanie strategii wykonywania z delegatem reprezentującym wszystkie elementy, które należy wykonać.</span><span class="sxs-lookup"><span data-stu-id="aaec5-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="aaec5-122">Jeśli wystąpi błąd przejściowy, strategia wykonywania ponownie wywoła delegata.</span><span class="sxs-lookup"><span data-stu-id="aaec5-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="aaec5-123">Takie podejście może być również używane z transakcjami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="aaec5-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="aaec5-124">Błąd zatwierdzania transakcji i problem z idempotentności</span><span class="sxs-lookup"><span data-stu-id="aaec5-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="aaec5-125">Ogólnie rzecz biorąc, gdy występuje błąd połączenia, bieżąca transakcja jest wycofywana.</span><span class="sxs-lookup"><span data-stu-id="aaec5-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="aaec5-126">Jeśli jednak połączenie zostanie usunięte podczas zatwierdzania transakcji, stan wynikający z transakcji jest nieznany.</span><span class="sxs-lookup"><span data-stu-id="aaec5-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="aaec5-127">Domyślnie strategia wykonywania ponowi próbę wykonania operacji tak, jakby transakcja została wycofana, ale jeśli nie jest to przypadek, spowoduje to wyjątek, jeśli nowy stan bazy danych jest niezgodny lub może doprowadzić do **uszkodzenia danych** , jeśli operacja nie zależy od określonego stanu, na przykład podczas wstawiania nowego wiersza z automatycznie generowanymi wartościami klucza.</span><span class="sxs-lookup"><span data-stu-id="aaec5-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="aaec5-128">Można to zrobić na kilka sposobów.</span><span class="sxs-lookup"><span data-stu-id="aaec5-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="aaec5-129">Opcja 1 — do (prawie) nic</span><span class="sxs-lookup"><span data-stu-id="aaec5-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="aaec5-130">Prawdopodobieństwo błędu połączenia podczas zatwierdzania transakcji jest niskie, dlatego może być akceptowalne, aby aplikacja mogła zakończyć się niepowodzeniem, jeśli ten stan rzeczywiście wystąpi.</span><span class="sxs-lookup"><span data-stu-id="aaec5-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="aaec5-131">Należy jednak unikać używania kluczy generowanych przez magazyn w celu zapewnienia, że wyjątek jest zgłaszany zamiast dodawania zduplikowanego wiersza.</span><span class="sxs-lookup"><span data-stu-id="aaec5-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="aaec5-132">Rozważ użycie wygenerowanej przez klienta wartości GUID lub generatora wartości po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="aaec5-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="aaec5-133">Opcja 2 — ponowne kompilowanie stanu aplikacji</span><span class="sxs-lookup"><span data-stu-id="aaec5-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="aaec5-134">Odrzuć bieżącą `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="aaec5-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="aaec5-135">Utwórz nowy `DbContext` i Przywróć stan aplikacji z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aaec5-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="aaec5-136">Poinformuj użytkownika, że Ostatnia operacja mogła nie zostać ukończona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="aaec5-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="aaec5-137">Opcja 3 — Dodawanie weryfikacji stanu</span><span class="sxs-lookup"><span data-stu-id="aaec5-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="aaec5-138">W przypadku większości operacji, które zmieniają stan bazy danych, można dodać kod, który sprawdza, czy zakończył się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="aaec5-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="aaec5-139">EF udostępnia metodę rozszerzenia, aby ułatwić `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="aaec5-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="aaec5-140">Ta metoda rozpoczyna i zatwierdza transakcję, a także akceptuje funkcję w `verifySucceeded` parametr, który jest wywoływany, gdy wystąpi błąd przejściowy podczas zatwierdzania transakcji.</span><span class="sxs-lookup"><span data-stu-id="aaec5-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="aaec5-141">W tym miejscu `SaveChanges` jest wywoływana z `acceptAllChangesOnSuccess` ustawionym na `false`, aby uniknąć zmiany stanu jednostki `Blog` na `Unchanged`, jeśli `SaveChanges` się powiedzie.</span><span class="sxs-lookup"><span data-stu-id="aaec5-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="aaec5-142">Pozwala to na ponowienie próby wykonania tej operacji, jeśli zatwierdzenie zakończy się niepowodzeniem, a transakcja zostanie wycofana.</span><span class="sxs-lookup"><span data-stu-id="aaec5-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="aaec5-143">Opcja 4 — ręcznie Śledź transakcję</span><span class="sxs-lookup"><span data-stu-id="aaec5-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="aaec5-144">Jeśli konieczne jest użycie kluczy generowanych przez magazyn lub konieczny jest ogólny sposób obsługi niepowodzeń zatwierdzeń, które nie zależą od operacji wykonywanej przez każdą transakcję, można przypisać identyfikator, który jest sprawdzany, gdy zatwierdzenie zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="aaec5-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="aaec5-145">Dodaj tabelę do bazy danych używanej do śledzenia stanu transakcji.</span><span class="sxs-lookup"><span data-stu-id="aaec5-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="aaec5-146">Wstaw wiersz do tabeli na początku każdej transakcji.</span><span class="sxs-lookup"><span data-stu-id="aaec5-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="aaec5-147">Jeśli połączenie zakończy się niepowodzeniem podczas zatwierdzania, sprawdź obecność odpowiedniego wiersza w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="aaec5-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="aaec5-148">Jeśli zatwierdzenie zakończy się pomyślnie, Usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.</span><span class="sxs-lookup"><span data-stu-id="aaec5-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="aaec5-149">Upewnij się, że kontekst używany do weryfikacji ma strategię wykonywania zdefiniowaną z chwilą, że połączenie będzie prawdopodobnie kończyć się niepowodzeniem, jeśli wystąpił błąd podczas zatwierdzania transakcji.</span><span class="sxs-lookup"><span data-stu-id="aaec5-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
