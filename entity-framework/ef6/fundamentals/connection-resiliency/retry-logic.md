---
title: Połączenie odporności logikę ponawiania prób i - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997488"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="df85a-102">Logika połączenia odporności, a następnie spróbuj ponownie</span><span class="sxs-lookup"><span data-stu-id="df85a-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="df85a-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="df85a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="df85a-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="df85a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="df85a-105">Aplikacje nawiązywania połączenia z serwerem bazy danych zawsze były podatne na podziały połączenia z powodu niepowodzenia zaplecza i niestabilności sieci.</span><span class="sxs-lookup"><span data-stu-id="df85a-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="df85a-106">Jednak w środowisku sieci LAN, na podstawie działa z serwerów dedykowanych bazy danych te błędy są wystarczająco rzadkie, że dodatkowa logika do obsługi tych błędów nie jest często wymagane.</span><span class="sxs-lookup"><span data-stu-id="df85a-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="df85a-107">Za pomocą w chmurze oparte mniej niezawodnej sieci, z którymi jest teraz bardziej powszechne podziału połączenia wystąpienia serwerów baz danych, takich jak Windows Azure SQL Database oraz połączeń.</span><span class="sxs-lookup"><span data-stu-id="df85a-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="df85a-108">Może to być spowodowane obrony technik, które w chmurze Użyj bazy danych, aby zapewnić sprawiedliwe usługi, takie jak ograniczanie przepustowości połączenia lub do niestabilności w sieci, powodują sporadyczne przekroczeń limitu czasu i innych błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="df85a-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="df85a-109">Elastyczność połączenia odnosi się do możliwości EF do automatycznego ponawiania próby wykonania dowolne polecenia, które się nie powieść z powodu przerwy te połączenia.</span><span class="sxs-lookup"><span data-stu-id="df85a-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="df85a-110">Strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="df85a-110">Execution Strategies</span></span>  

<span data-ttu-id="df85a-111">Ponów próbę połączenia są wykonywane przez implementację interfejsu IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="df85a-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="df85a-112">Implementacje IDbExecutionStrategy będzie odpowiedzialny za akceptować operacji, a jeśli wystąpi wyjątek, określająca, czy ponowienie próby jest odpowiednia i ponawianie próby, jeśli jest.</span><span class="sxs-lookup"><span data-stu-id="df85a-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="df85a-113">Istnieją cztery strategii wykonywania, które są dostarczane z programem EF:</span><span class="sxs-lookup"><span data-stu-id="df85a-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="df85a-114">**DefaultExecutionStrategy**: Ta strategia wykonywania nie ponawia próby żadnych operacji, jest ustawieniem domyślnym dla baz danych innych niż sql server.</span><span class="sxs-lookup"><span data-stu-id="df85a-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="df85a-115">**DefaultSqlExecutionStrategy**: jest to strategii wykonywania wewnętrzny używany domyślnie.</span><span class="sxs-lookup"><span data-stu-id="df85a-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="df85a-116">Ta strategia nie ponawia próby w ogóle, jednak opakować wszystkie wyjątki, które mogą być przejściowe poinformować użytkowników, którzy chcą włączyć elastyczność połączenia.</span><span class="sxs-lookup"><span data-stu-id="df85a-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="df85a-117">**DbExecutionStrategy**: Ta klasa jest odpowiednie, jako klasę bazową dla innych strategii wykonywania, w tym własne niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="df85a-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="df85a-118">Implementuje zasady ponawiania wykładniczego, gdzie początkowej ponawiania się dzieje z zero opóźnienia i opóźnienia rośnie wykładniczo, dopóki nie zostanie osiągnięty jako maksymalna liczba ponowień.</span><span class="sxs-lookup"><span data-stu-id="df85a-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="df85a-119">Ta klasa posiada metodę ShouldRetryOn abstrakcyjną, która może być implementowany w strategii wykonywania pochodnych do kontrolowania, wyjątki, które należy wykonać ponownie.</span><span class="sxs-lookup"><span data-stu-id="df85a-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="df85a-120">**SqlAzureExecutionStrategy**: Ta strategia wykonywania dziedziczy DbExecutionStrategy i spróbuje ponowić operację na wyjątki, które są znane jako prawdopodobnie przejściowy podczas pracy z usługą Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="df85a-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="df85a-121">Strategii wykonywania 2 i 4 są uwzględnione w dostawcy programu Sql Server, który jest dostarczany z programem EF, który znajduje się w zestawie EntityFramework.SqlServer i zostały zaprojektowane do pracy z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="df85a-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="df85a-122">Włączanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="df85a-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="df85a-123">Najprostszym sposobem Poinformuj EF, aby użyć strategii wykonywania jest przy użyciu metody SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) klasy:</span><span class="sxs-lookup"><span data-stu-id="df85a-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="df85a-124">Ten kod informuje EF, aby użyć SqlAzureExecutionStrategy podczas nawiązywania połączenia z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="df85a-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="df85a-125">Konfigurowanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="df85a-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="df85a-126">Konstruktor obiektu SqlAzureExecutionStrategy może akceptować dwa parametry: wartość MaxRetryCount i MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="df85a-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="df85a-127">Liczba MaxRetry jest maksymalna liczba strategii ponownych prób.</span><span class="sxs-lookup"><span data-stu-id="df85a-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="df85a-128">MaxDelay jest element TimeSpan reprezentujący Maksymalne opóźnienie między kolejnymi próbami używających strategii wykonywania.</span><span class="sxs-lookup"><span data-stu-id="df85a-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="df85a-129">Aby ustawić maksymalną liczbę ponownych prób 1 i maksymalne opóźnienie do 30 sekund będzie się execue następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="df85a-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="df85a-130">SqlAzureExecutionStrategy ponowi próbę natychmiast po raz pierwszy błąd przejściowy, który występuje, ale będzie już opóźnienie między kolejnymi próbami, dopóki albo maksymalny limit ponownych prób zostanie przekroczony lub łącznego czasu trafienia Maksymalne opóźnienie.</span><span class="sxs-lookup"><span data-stu-id="df85a-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="df85a-131">Strategii wykonywania ponowi tylko ograniczoną liczbę wyjątków, które są zwykle tansient, nadal konieczne będzie obsługiwać inne błędy, a także przechwytywanie wyjątku RetryLimitExceeded w przypadku których błąd nie jest przejściowy lub trwa zbyt długo rozwiązać samego siebie.</span><span class="sxs-lookup"><span data-stu-id="df85a-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="df85a-132">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="df85a-132">Limitations</span></span>  

<span data-ttu-id="df85a-133">Korzystając z Trwa ponawianie próby strategii wykonywania są niektóre znane ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="df85a-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="df85a-134">Zapytania przesyłania strumieniowego nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="df85a-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="df85a-135">Domyślnie programy EF6 i nowszym będzie buforować wyniki zapytania, a nie ich streaming.</span><span class="sxs-lookup"><span data-stu-id="df85a-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="df85a-136">Jeśli chcesz mieć wyniki przesyłane strumieniowo można metoda AsStreaming służy do zmiany LINQ zapytania jednostki w celu przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="df85a-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="df85a-137">Przesyłania strumieniowego nie jest obsługiwana, gdy Trwa ponawianie próby strategii wykonywania jest zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="df85a-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="df85a-138">To ograniczenie istnieje, ponieważ połączenie można porzucić część sposób wyniki są zwracane.</span><span class="sxs-lookup"><span data-stu-id="df85a-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="df85a-139">W takiej sytuacji EF musi zostać ponownie uruchomiony całe zapytanie, ale nie ma niezawodne możliwości informacji o tym, co powoduje już zostały zwrócone (dane mogły ulec zmianie od momentu początkowego zapytania została wysłana, wyniki mogą wrócić w innej kolejności, wyniki nie może mieć unikatowy identyfikator itp.).</span><span class="sxs-lookup"><span data-stu-id="df85a-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="df85a-140">Użytkownik zainicjował transakcji nie jest obsługiwane</span><span class="sxs-lookup"><span data-stu-id="df85a-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="df85a-141">Po skonfigurowaniu strategii wykonywania, który skutkuje ponownych prób, istnieją pewne ograniczenia dotyczące użycia transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="df85a-142">Jakie operacje są obsługiwane: firmy EF domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="df85a-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="df85a-143">Domyślnie program EF będzie wykonywać żadnych aktualizacji bazy danych w obrębie transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="df85a-144">Nie trzeba nic robić, aby włączyć tę opcję, EF zawsze wykonuje to automatycznie.</span><span class="sxs-lookup"><span data-stu-id="df85a-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="df85a-145">Na przykład w poniższym kodzie SaveChanges jest wykonywana automatycznie w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="df85a-146">Gdyby SaveChanges się niepowodzeniem po Wstawianie jedną nową lokację, a następnie będzie można z powrotem obniżyć transakcji i nie zmiany zostały wprowadzone do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="df85a-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="df85a-147">Kontekst jest również pozostanie w stanie umożliwiającym Funkcja SaveChanges zapisuje do ponownie wywołany, aby ponowić próbę zastosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="df85a-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="df85a-148">Co to jest nieobsługiwana: użytkownik zainicjował transakcji</span><span class="sxs-lookup"><span data-stu-id="df85a-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="df85a-149">Bez korzystania z Trwa ponawianie próby strategii wykonywania może zawijać się wiele operacji w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="df85a-150">Na przykład poniższy kod opakowuje dwóch wywołań funkcji SaveChanges w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="df85a-151">Jeśli którejkolwiek którejkolwiek z tych operacji nie powiedzie się następnie zmiany zostaną zastosowane.</span><span class="sxs-lookup"><span data-stu-id="df85a-151">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="df85a-152">To nie jest obsługiwana, gdy za pomocą Trwa ponawianie próby strategii wykonywania, ponieważ EF nie jest świadomy żadnych poprzedniej operacji i sposób ponowić próbę ich wykonania.</span><span class="sxs-lookup"><span data-stu-id="df85a-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="df85a-153">Na przykład jeśli drugi SaveChanges nie powiodła się następnie EF już ma wymagane informacje, aby ponowić próbę wykonania pierwszego wywołania funkcji SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="df85a-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="df85a-154">Możliwe obejścia</span><span class="sxs-lookup"><span data-stu-id="df85a-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="df85a-155">Wstrzymywanie strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="df85a-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="df85a-156">Jednym możliwym obejściem jest wstrzymania Trwa ponawianie próby strategia wykonywania dla fragmentu kodu, który musi używać użytkownik zainicjował transakcji.</span><span class="sxs-lookup"><span data-stu-id="df85a-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="df85a-157">Najprostszym sposobem, w tym celu jest dodać flagę SuspendExecutionStrategy w kodzie na podstawie klasy konfiguracji, a także zmienić lambda strategii wykonywania do zwrócenia strategii wykonywania (inne niż retying) domyślny, jeśli flaga jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="df85a-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="df85a-158">Należy pamiętać, że używasz CallContext do przechowywania wartości flagi.</span><span class="sxs-lookup"><span data-stu-id="df85a-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="df85a-159">Zapewnia funkcje podobne do pamięci lokalnej wątku, ale jest bezpieczne, za pomocą kodu asynchronicznego — w tym zapytania asynchroniczne i zapisywać przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df85a-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="df85a-160">Firma Microsoft jest teraz wstrzymywanie strategii wykonywania dla sekcji kodu, który używa transakcji zainicjowanej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="df85a-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="df85a-161">Ręcznego wywoływania strategii wykonywania</span><span class="sxs-lookup"><span data-stu-id="df85a-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="df85a-162">Inną opcją jest ręczne Użyj strategii wykonywania i nadaj cały zestaw logiki, aby uruchomić, dzięki czemu można ponów wszystko, jeśli jedna z operacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="df85a-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="df85a-163">Nadal trzeba wstrzymywanie strategii wykonywania - korzystające z techniki powyżej — tak aby kontekstów używana wewnątrz blok kodu powtarzający operację nie należy podejmować próbę.</span><span class="sxs-lookup"><span data-stu-id="df85a-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="df85a-164">Należy pamiętać, że w bloku kodu, ponowienie próby powinien być konstruowany kontekstów.</span><span class="sxs-lookup"><span data-stu-id="df85a-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="df85a-165">Dzięki temu rozpoczynamy przy użyciu czystego stanu przeznaczonego do każdego ponownych prób.</span><span class="sxs-lookup"><span data-stu-id="df85a-165">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

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

MyConfiguration.SuspendExecutionStrategy = false;
```  
