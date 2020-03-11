---
title: Rozpoznawanie zależności — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417863"
---
# <a name="dependency-resolution"></a><span data-ttu-id="fa43b-102">Rozwiązywanie zależności</span><span class="sxs-lookup"><span data-stu-id="fa43b-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="fa43b-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fa43b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fa43b-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="fa43b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fa43b-105">Począwszy od EF6, Entity Framework zawiera mechanizm ogólnego przeznaczenia do uzyskiwania implementacji wymaganych usług.</span><span class="sxs-lookup"><span data-stu-id="fa43b-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="fa43b-106">Oznacza to, że gdy EF używa wystąpienia niektórych interfejsów lub klas bazowych, będzie pytał o konkretną implementację interfejsu lub klasy bazowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="fa43b-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="fa43b-107">Jest to realizowane za pomocą interfejsu IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="fa43b-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="fa43b-108">Metoda GetService jest zazwyczaj wywoływana przez EF i jest obsługiwana przez implementację IDbDependencyResolver dostarczonej przez EF lub przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="fa43b-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="fa43b-109">Gdy wywoływana, argument typu jest interfejsem lub typem klasy bazowej żądanej usługi, a obiekt klucza ma wartość null lub obiekt zawierający informacje kontekstowe dotyczące żądanej usługi.</span><span class="sxs-lookup"><span data-stu-id="fa43b-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="fa43b-110">O ile nie określono inaczej, wszelkie zwracane obiekty muszą być bezpieczne wątkowo, ponieważ mogą być używane jako pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="fa43b-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="fa43b-111">W wielu przypadkach zwracanym obiektem jest fabryka, w której przypadku sama fabryka musi być bezpieczna wątkowo, ale obiekt zwrócony z fabryki nie musi być bezpieczny wątkowo, ponieważ nowe wystąpienie jest wymagane od fabryki dla każdego użycia.</span><span class="sxs-lookup"><span data-stu-id="fa43b-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="fa43b-112">Ten artykuł nie zawiera pełnych informacji na temat implementowania IDbDependencyResolver, ale zamiast tego działa jako odwołanie do typów usług (czyli interfejsów i typów klas podstawowych), dla których EF wywołuje metodę GetService i semantykę obiektu klucza dla każdej z tych elementów Rozmowa.</span><span class="sxs-lookup"><span data-stu-id="fa43b-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="fa43b-113">System. Data. Entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="fa43b-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="fa43b-114">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-115">**Zwrócony obiekt**: inicjator bazy danych dla danego typu kontekstu</span><span class="sxs-lookup"><span data-stu-id="fa43b-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="fa43b-116">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="fa43b-117">Func < System. Data. Entity. migrations. SQL. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="fa43b-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="fa43b-118">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="fa43b-119">**Zwrócony obiekt**: Fabryka do tworzenia generatora SQL, który może służyć do migracji i innych akcji, które powodują utworzenie bazy danych, takich jak tworzenie bazy danych z inicjatorami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fa43b-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="fa43b-120">**Klucz**: ciąg zawierający niezmienną nazwę dostawcy ADO.NET określającą typ bazy danych, dla której zostanie wygenerowany program SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fa43b-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="fa43b-121">Na przykład SQL Server Generator SQL jest zwracany dla klucza "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="fa43b-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-122">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="fa43b-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="fa43b-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="fa43b-124">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-125">**Zwrócony obiekt**: dostawca EF do użycia dla danej niezmiennej nazwy dostawcy</span><span class="sxs-lookup"><span data-stu-id="fa43b-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="fa43b-126">**Klucz**: ciąg zawierający nazwę niezmiennej dostawcy ADO.NET określającą typ bazy danych, dla której jest wymagany dostawca.</span><span class="sxs-lookup"><span data-stu-id="fa43b-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="fa43b-127">Na przykład dostawca SQL Server jest zwracany dla klucza "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="fa43b-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-128">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="fa43b-129">System. Data. Entity. Infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="fa43b-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="fa43b-130">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-131">**Zwrócony obiekt**: Fabryka połączeń, która będzie używana podczas tworzenia połączenia z bazą danych za pomocą Konwencji EF.</span><span class="sxs-lookup"><span data-stu-id="fa43b-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="fa43b-132">Oznacza to, że w przypadku braku połączenia lub parametrów połączenia z EF nie można odnaleźć parametrów połączenia w pliku App. config lub Web. config. Ta usługa jest używana do tworzenia połączenia według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="fa43b-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="fa43b-133">Zmiana fabryki połączeń może pozwolić, aby EF domyślnie używała innego typu bazy danych (na przykład SQL Server Compact Edition).</span><span class="sxs-lookup"><span data-stu-id="fa43b-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="fa43b-134">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-135">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="fa43b-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="fa43b-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="fa43b-137">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-138">**Zwrócony obiekt**: usługa, która może generować token manifestu dostawcy z połączenia.</span><span class="sxs-lookup"><span data-stu-id="fa43b-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="fa43b-139">Ta usługa jest zwykle używana na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="fa43b-139">This service is typically used in two ways.</span></span> <span data-ttu-id="fa43b-140">Po pierwsze, można go użyć, aby uniknąć Code First łączenia się z bazą danych podczas kompilowania modelu.</span><span class="sxs-lookup"><span data-stu-id="fa43b-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="fa43b-141">Następnie można go użyć do wymuszenia Code First tworzenia modelu dla określonej wersji bazy danych — na przykład, aby wymusić model dla SQL Server 2005, nawet jeśli czasami SQL Server 2008 jest używany.</span><span class="sxs-lookup"><span data-stu-id="fa43b-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="fa43b-142">**Okres istnienia obiektu**: pojedynczy — ten sam obiekt może być wielokrotnie używany i jednocześnie przez różne wątki</span><span class="sxs-lookup"><span data-stu-id="fa43b-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="fa43b-143">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="fa43b-144">System. Data. Entity. Infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="fa43b-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="fa43b-145">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-146">**Zwrócony obiekt**: usługa, która może uzyskać fabrykę dostawcy z danego połączenia.</span><span class="sxs-lookup"><span data-stu-id="fa43b-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="fa43b-147">W przypadku programu .NET 4,5 dostawca jest publicznie dostępny w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="fa43b-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="fa43b-148">W programie .NET 4 domyślna implementacja tej usługi używa pewnych algorytmów heurystycznych w celu znalezienia pasującego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="fa43b-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="fa43b-149">Jeśli te błędy zakończą się niepowodzeniem, można zarejestrować nową implementację tej usługi, aby zapewnić odpowiednie rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="fa43b-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="fa43b-150">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="fa43b-151">Func < DbContext, system. Data. Entity. Infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="fa43b-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="fa43b-152">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-153">**Zwrócony obiekt**: Fabryka, która będzie generować klucz pamięci podręcznej modelu dla danego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="fa43b-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="fa43b-154">Domyślnie EF zapisuje w pamięci podręcznej jeden model na typ kontekstu dbdla każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="fa43b-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="fa43b-155">Inna implementacja tej usługi może służyć do dodawania innych informacji, takich jak nazwa schematu, do klucza pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="fa43b-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="fa43b-156">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="fa43b-157">System. Data. Entity. przestrzenny. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="fa43b-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="fa43b-158">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-159">**Zwrócony obiekt**: dostawca przestrzenny EF, który dodaje obsługę podstawowego dostawcy EF dla typów przestrzennych i geograficznych.</span><span class="sxs-lookup"><span data-stu-id="fa43b-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="fa43b-160">**Klucz**: DbSptialServices jest proszony na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="fa43b-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="fa43b-161">Najpierw żąda się usług przestrzennych specyficznych dla dostawcy przy użyciu obiektu DbProviderInfo (który zawiera nazwę niezmienną i token manifestu) jako klucz.</span><span class="sxs-lookup"><span data-stu-id="fa43b-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="fa43b-162">Po drugie, DbSpatialServices może być poproszony o brak klucza.</span><span class="sxs-lookup"><span data-stu-id="fa43b-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="fa43b-163">Służy do rozpoznawania globalnego dostawcy przestrzennego, który jest używany podczas tworzenia autonomicznych typów DbGeography lub DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="fa43b-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-164">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="fa43b-165">Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="fa43b-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="fa43b-166">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-167">**Zwrócony obiekt**: Fabryka do tworzenia usługi, która umożliwia dostawcy wdrożenie ponownych prób lub inne zachowanie w przypadku wykonywania zapytań i poleceń względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fa43b-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="fa43b-168">Jeśli nie podano implementacji, EF po prostu wykona polecenia i propaguje wszystkie zgłoszone wyjątki.</span><span class="sxs-lookup"><span data-stu-id="fa43b-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="fa43b-169">W przypadku SQL Server ta usługa jest używana w celu zapewnienia zasad ponawiania, która jest szczególnie przydatna w przypadku uruchamiania na serwerach bazy danych opartych na chmurze, takich jak SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="fa43b-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="fa43b-170">**Klucz**: obiekt ExecutionStrategyKey, który zawiera niezmienną nazwę dostawcy i opcjonalnie nazwę serwera, dla którego zostanie użyta strategia wykonywania.</span><span class="sxs-lookup"><span data-stu-id="fa43b-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-171">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="fa43b-172">Func < DbConnection, String, system. Data. Entity. migrations. history. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="fa43b-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="fa43b-173">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-174">**Zwrócony obiekt**: Fabryka, która pozwala dostawcy konfigurować mapowanie HistoryContext do tabeli `__MigrationHistory` używanej przez migracje EF.</span><span class="sxs-lookup"><span data-stu-id="fa43b-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="fa43b-175">HistoryContext to Code First DbContext i można go skonfigurować przy użyciu normalnego interfejsu API Fluent, aby zmienić elementy, takie jak nazwa tabeli i specyfikacje mapowania kolumn.</span><span class="sxs-lookup"><span data-stu-id="fa43b-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="fa43b-176">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-177">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="fa43b-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="fa43b-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="fa43b-179">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-180">**Zwrócony obiekt**: dostawca ADO.NET do użycia dla danej niezmiennej nazwy dostawcy.</span><span class="sxs-lookup"><span data-stu-id="fa43b-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="fa43b-181">**Klucz**: ciąg zawierający niezmienną nazwę dostawcy ADO.NET</span><span class="sxs-lookup"><span data-stu-id="fa43b-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-182">Ta usługa nie jest zwykle zmieniana bezpośrednio od czasu implementacji domyślnej przy użyciu standardowej rejestracji dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="fa43b-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="fa43b-183">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="fa43b-184">System. Data. Entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="fa43b-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="fa43b-185">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-186">**Zwrócony obiekt**: usługa, która jest używana do określenia niezmiennej nazwy dostawcy dla danego typu DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="fa43b-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="fa43b-187">Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="fa43b-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="fa43b-188">Oznacza to, że jeśli dostawca ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozpoznawany przez EF, konieczne będzie również rozwiązanie tej usługi.</span><span class="sxs-lookup"><span data-stu-id="fa43b-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="fa43b-189">**Key**: Wystąpienie DbProviderFactory, dla którego wymagana jest nazwa niezmienna.</span><span class="sxs-lookup"><span data-stu-id="fa43b-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="fa43b-190">Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="fa43b-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="fa43b-191">System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="fa43b-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="fa43b-192">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-193">**Zwrócony obiekt**: pamięć podręczna zestawów, które zawierają wstępnie wygenerowane widoki.</span><span class="sxs-lookup"><span data-stu-id="fa43b-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="fa43b-194">Zastąpienie jest zwykle używane do informowania EF o tym, które zestawy zawierają wstępnie wygenerowane widoki, bez konieczności odnajdywania.</span><span class="sxs-lookup"><span data-stu-id="fa43b-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="fa43b-195">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="fa43b-196">System. Data. Entity. Infrastructure. pluralizacja. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="fa43b-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="fa43b-197">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-198">**Zwrócony obiekt**: Usługa używana przez EF do pluralize i nazw nazwom.</span><span class="sxs-lookup"><span data-stu-id="fa43b-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="fa43b-199">Domyślnie używana jest usługa pluralizacja w języku angielskim.</span><span class="sxs-lookup"><span data-stu-id="fa43b-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="fa43b-200">**Klucz**: nieużywany; będzie mieć wartość null</span><span class="sxs-lookup"><span data-stu-id="fa43b-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="fa43b-201">System. Data. Entity. Infrastructure. przechwytując. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="fa43b-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="fa43b-202">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="fa43b-203">**Zwrócone obiekty**: wszystkie Interceptory, które powinny być zarejestrowane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fa43b-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="fa43b-204">Należy zauważyć, że te obiekty są żądane przy użyciu wywołania GetServices i wszystkich przechwyceń zwracanych przez dowolny program rozpoznawania zależności zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="fa43b-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="fa43b-205">**Klucz**: nieużywany; będzie mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="fa43b-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="fa43b-206">Func < System. Data. Entity. DbContext, Akcja < ciąg\>, system. Data. Entity. Infrastructure. przechwytując. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="fa43b-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="fa43b-207">**Wprowadzona wersja**: Dr 6.0.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fa43b-208">**Zwrócony obiekt**: Fabryka, która zostanie użyta do utworzenia programu formatującego dziennika bazy danych, który będzie używany w przypadku kontekstu. Właściwość Database. log jest ustawiona w danym kontekście.</span><span class="sxs-lookup"><span data-stu-id="fa43b-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="fa43b-209">**Klucz**: nieużywany; będzie mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="fa43b-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="fa43b-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="fa43b-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="fa43b-211">**Wprowadzona wersja**: Dr 6.1.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fa43b-212">**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia wystąpień kontekstu dla migracji, gdy kontekst nie ma dostępnego konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="fa43b-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="fa43b-213">**Key**: obiekt Type dla typu pochodnego DbContext, dla którego jest wymagana fabryka.</span><span class="sxs-lookup"><span data-stu-id="fa43b-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="fa43b-214">Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="fa43b-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="fa43b-215">**Wprowadzona wersja**: Dr 6.1.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fa43b-216">**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia serializatorów do serializacji niestandardowych adnotacji o jednoznacznie określonym typie, które mogą być serializowane i desterylizowane w kodzie XML do użycia w migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="fa43b-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="fa43b-217">**Key**: nazwa adnotacji, która jest serializowana lub deserializowana.</span><span class="sxs-lookup"><span data-stu-id="fa43b-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="fa43b-218">Func < System. Data. Entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="fa43b-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="fa43b-219">**Wprowadzona wersja**: Dr 6.1.0</span><span class="sxs-lookup"><span data-stu-id="fa43b-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fa43b-220">**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia programów obsługi dla transakcji, dzięki czemu można zastosować obsługę specjalną w przypadku sytuacji, takich jak obsługa niepowodzeń zatwierdzeń.</span><span class="sxs-lookup"><span data-stu-id="fa43b-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="fa43b-221">**Klucz**: obiekt ExecutionStrategyKey, który zawiera niezmienną nazwę dostawcy i opcjonalnie nazwę serwera, dla którego zostanie użyta procedura obsługi transakcji.</span><span class="sxs-lookup"><span data-stu-id="fa43b-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
