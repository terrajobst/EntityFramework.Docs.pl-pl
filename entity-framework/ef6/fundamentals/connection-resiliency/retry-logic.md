---
title: Odporność połączeń i logika ponawiania — EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402160"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="8e201-102">Odporność połączeń i logika ponowień</span><span class="sxs-lookup"><span data-stu-id="8e201-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="8e201-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8e201-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8e201-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="8e201-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8e201-105">Aplikacje łączące się z serwerem bazy danych zawsze były podatne na przerwy z połączeniami z powodu awarii zaplecza i niestabilności sieci.</span><span class="sxs-lookup"><span data-stu-id="8e201-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="8e201-106">Jednak w środowisku opartym na sieci LAN pracującym z serwerami dedykowanych baz danych te błędy są wystarczająco rzadko, ponieważ dodatkowa logika obsługi tych awarii nie jest często wymagana.</span><span class="sxs-lookup"><span data-stu-id="8e201-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="8e201-107">Ze wzrostem liczby serwerów baz danych opartych na chmurze, takich jak Azure SQL Database systemu Windows i połączeń za pośrednictwem mniej niezawodnych sieci, jest teraz bardziej często spotykane w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="8e201-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="8e201-108">Może to być spowodowane technikami obronnymi używanymi przez bazy danych w chmurze w celu zapewnienia sprawiedliwości usług, takich jak ograniczanie połączenia lub niestabilność w sieci, powodującej sporadyczne przekroczenia limitu czasu i innych błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="8e201-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="8e201-109">Odporność połączenia odnosi się do zdolności programu EF do automatycznego ponawiania prób wszelkich poleceń, które kończą się niepowodzeniem z powodu tych przerw połączeń.</span><span class="sxs-lookup"><span data-stu-id="8e201-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="8e201-110">Strategie wykonywania</span><span class="sxs-lookup"><span data-stu-id="8e201-110">Execution Strategies</span></span>  

<span data-ttu-id="8e201-111">Ponowienie połączenia jest wykonywane przez implementację interfejsu IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="8e201-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="8e201-112">Implementacje IDbExecutionStrategy będą odpowiedzialne za zaakceptowanie operacji, a jeśli wystąpi wyjątek, określenie, czy ponowienie próby jest odpowiednie i ponawianie próby, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="8e201-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="8e201-113">Istnieją cztery strategie wykonywania dostarczane z EF:</span><span class="sxs-lookup"><span data-stu-id="8e201-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="8e201-114">**DefaultExecutionStrategy**: ta strategia wykonywania nie ponawia żadnej operacji, jest to wartość domyślna dla baz danych innych niż SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e201-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="8e201-115">**DefaultSqlExecutionStrategy**: jest to wewnętrzna strategia wykonywania, która jest używana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="8e201-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="8e201-116">Ta strategia nie ponawia żadnej próby, jednak spowoduje zawinięcie wszelkich wyjątków, które mogą być przejściowe, aby poinformować użytkowników, że mogą chcieć włączyć odporność połączenia.</span><span class="sxs-lookup"><span data-stu-id="8e201-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="8e201-117">**DbExecutionStrategy**: Ta klasa jest odpowiednia jako klasa bazowa dla innych strategii wykonywania, w tym własnych niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="8e201-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="8e201-118">Implementuje on zasady ponowień wykładniczych, w przypadku których początkowa ponowna próba nastąpi z opóźnieniem równym zero, a opóźnienie wzrasta wykładniczo, dopóki Maksymalna liczba ponownych prób zostanie osiągnięta</span><span class="sxs-lookup"><span data-stu-id="8e201-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="8e201-119">Ta klasa ma abstrakcyjną metodę ShouldRetryOn, która może być implementowana w pochodnych strategiach wykonywania w celu kontrolowania, które wyjątki powinny być ponawiane.</span><span class="sxs-lookup"><span data-stu-id="8e201-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="8e201-120">**SqlAzureExecutionStrategy**: ta strategia wykonywania dziedziczy z DbExecutionStrategy i ponowi próbę po wyjątkach, które są prawdopodobnie przejściowe podczas pracy z Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="8e201-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="8e201-121">Strategie wykonywania 2 i 4 są zawarte w dostawcy programu SQL Server, który jest dostarczany z dr, który znajduje się w zestawie EntityFramework. SqlServer i jest przeznaczony do pracy z SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e201-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="8e201-122">Włączanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="8e201-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="8e201-123">Najprostszym sposobem, aby poinformować Dr o korzystanie z strategii wykonywania, jest metoda SetExecutionStrategy klasy [dbconfiguration](~/ef6/fundamentals/configuring/code-based.md) :</span><span class="sxs-lookup"><span data-stu-id="8e201-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="8e201-124">Ten kod nakazuje programowi Dr użycie SqlAzureExecutionStrategy podczas nawiązywania połączenia z SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e201-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="8e201-125">Konfigurowanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="8e201-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="8e201-126">Konstruktor elementu SqlAzureExecutionStrategy może przyjmować dwa parametry, wartość MaxRetryCount i MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="8e201-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="8e201-127">Liczba MaxRetry jest maksymalną liczbą ponownych prób w strategii.</span><span class="sxs-lookup"><span data-stu-id="8e201-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="8e201-128">MaxDelay to przedział czasu reprezentujący maksymalne opóźnienie między ponownymi próbami, które będą używane przez strategię wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8e201-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="8e201-129">Aby ustawić maksymalną liczbę ponownych prób na 1 i maksymalne opóźnienie na 30 sekund, należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8e201-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="8e201-130">SqlAzureExecutionStrategy ponowi próbę natychmiast po pierwszym wystąpieniu błędu przejściowego, ale wydłuża czas oczekiwania na przekroczenie limitu maksymalnej liczby ponownych prób lub łącznego czasu osiągnie maksymalne opóźnienie.</span><span class="sxs-lookup"><span data-stu-id="8e201-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="8e201-131">W strategiach wykonywania będzie można ponowić tylko ograniczoną liczbę wyjątków, które zwykle są przejściowe. nadal trzeba będzie obsługiwać inne błędy, a także przechwycić wyjątek RetryLimitExceeded w przypadku, gdy błąd nie jest przejściowy lub trwa zbyt długo, aby rozwiązać ten problem. sam.</span><span class="sxs-lookup"><span data-stu-id="8e201-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="8e201-132">Istnieją pewne znane ograniczenia związane z ponowną próbą wykonania:</span><span class="sxs-lookup"><span data-stu-id="8e201-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="8e201-133">Zapytania przesyłania strumieniowego nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="8e201-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="8e201-134">Domyślnie EF6 i nowsze wersje będą buforować wyniki zapytania zamiast przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="8e201-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="8e201-135">Jeśli chcesz uzyskać strumieniowe wyniki, możesz użyć metody AsStreaming, aby zmienić zapytanie LINQ to Entities do przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="8e201-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="8e201-136">Przesyłanie strumieniowe nie jest obsługiwane w przypadku zarejestrowania strategii wykonywania ponownych prób.</span><span class="sxs-lookup"><span data-stu-id="8e201-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="8e201-137">To ograniczenie istnieje, ponieważ połączenie może zostać porzucane w formie częściowej przez zwracane wyniki.</span><span class="sxs-lookup"><span data-stu-id="8e201-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="8e201-138">W takim przypadku EF musi ponownie uruchomić całe zapytanie, ale nie ma niezawodnego sposobu na poznanie wyników, które zostały już zwrócone (dane mogły zostać zmienione od czasu wysłania zapytania, wyniki mogą być ponownie w innej kolejności, wyniki mogą nie mieć unikatowego identyfikatora itp.).</span><span class="sxs-lookup"><span data-stu-id="8e201-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="8e201-139">Transakcje zainicjowane przez użytkownika nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="8e201-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="8e201-140">Po skonfigurowaniu strategii wykonywania, która powoduje ponowną próbę, istnieją pewne ograniczenia dotyczące korzystania z transakcji.</span><span class="sxs-lookup"><span data-stu-id="8e201-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="8e201-141">Domyślnie program EF wykona wszystkie aktualizacje bazy danych w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="8e201-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="8e201-142">Nie musisz nic robić, aby go włączyć. EF zawsze robi to automatycznie.</span><span class="sxs-lookup"><span data-stu-id="8e201-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="8e201-143">Na przykład w poniższym kodzie metody SaveChanges jest automatycznie wykonywane w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="8e201-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="8e201-144">Jeśli metody SaveChanges nie powiedzie się po wstawieniu jednej z nowych witryn, transakcja zostanie wycofana i nie zostaną zastosowane żadne zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8e201-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="8e201-145">Kontekst jest również pozostawiony w stanie, który umożliwia ponowne wywołanie metody SaveChanges, aby ponowić próbę zastosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="8e201-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="8e201-146">Gdy nie korzystasz z kolejnej strategii wykonywania, możesz otoczyć wiele operacji w jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8e201-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="8e201-147">Na przykład poniższy kod otacza dwa wywołania metody SaveChanges w jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8e201-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="8e201-148">Jeśli jakakolwiek część jednej operacji nie powiedzie się, żadne zmiany nie zostaną zastosowane.</span><span class="sxs-lookup"><span data-stu-id="8e201-148">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="8e201-149">Nie jest to obsługiwane w przypadku korzystania z metody ponawiania próby, ponieważ EF nie wie o żadnych poprzednich operacjach i sposobach ich ponowienia.</span><span class="sxs-lookup"><span data-stu-id="8e201-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="8e201-150">Na przykład jeśli druga metody SaveChanges nie powiodła się, EF nie ma już wymaganych informacji, aby ponowić próbę wykonania pierwszego wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="8e201-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="8e201-151">Rozwiązanie: ręczne wywoływanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="8e201-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="8e201-152">Rozwiązaniem jest ręczne Użycie strategii wykonywania i nadanie jej całego zestawu logiki, dzięki czemu można ponowić próbę wszystkiego, jeśli jedna z operacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8e201-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="8e201-153">Po uruchomieniu strategii wykonywania pochodzącej od DbExecutionStrategy zostanie ona zawiesić niejawną strategię wykonywania używaną w metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="8e201-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="8e201-154">Należy zauważyć, że wszystkie konteksty należy skonstruować w bloku kodu, aby można było ponowić próbę.</span><span class="sxs-lookup"><span data-stu-id="8e201-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="8e201-155">Gwarantuje to, że rozpoczynamy od stanu czystego dla każdej ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="8e201-155">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });
```  
