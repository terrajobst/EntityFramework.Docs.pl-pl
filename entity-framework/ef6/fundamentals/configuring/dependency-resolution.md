---
title: Rozpoznawanie zależności — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 76bb350f2407e0e33a20d0c4f6961ba57d646818
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914216"
---
# <a name="dependency-resolution"></a><span data-ttu-id="f17a2-102">Rozpoznawanie zależności</span><span class="sxs-lookup"><span data-stu-id="f17a2-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="f17a2-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f17a2-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f17a2-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f17a2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f17a2-105">Począwszy od platformy EF6 Entity Framework zawiera mechanizm ogólnego przeznaczenia do uzyskania implementacji usług, które są wymagane.</span><span class="sxs-lookup"><span data-stu-id="f17a2-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="f17a2-106">Oznacza to kiedy EF używa wystąpienia niektóre interfejsy lub klas bazowych zostanie wyświetlony monit dla konkretnej implementacji interfejsu lub klasy bazowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="f17a2-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="f17a2-107">Jest to osiągane przy użyciu interfejsu IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="f17a2-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="f17a2-108">Metoda GetService zwykle jest wywoływana przez EF i jest obsługiwane przez implementację IDbDependencyResolver EF lub aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="f17a2-109">Gdy zostanie wywołana, argument typu jest typem klasy interfejsu lub base żądanej usługi i klucz obiektu jest wartość null lub obiekt dostarczający informacje kontekstowe o żądanej usługi.</span><span class="sxs-lookup"><span data-stu-id="f17a2-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="f17a2-110">Ten artykuł zawiera szczegółowe informacje o sposobie implementacji IDbDependencyResolver, ale zamiast tego działa jako odwołanie dla typów usługi (np. interfejsu i typów klasy bazowej), dla których EF wywołuje GetService i semantyka obiekt klucza dla każdego z tych wywołań.</span><span class="sxs-lookup"><span data-stu-id="f17a2-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (i.e. interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="f17a2-111">Ten dokument będzie aktualizowany podczas dodawania dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="f17a2-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="f17a2-112">Usługi rozwiązane</span><span class="sxs-lookup"><span data-stu-id="f17a2-112">Services resolved</span></span>  

<span data-ttu-id="f17a2-113">Jeżeli nie określono inaczej, dowolny obiekt zwrócony musi być metodą o bezpiecznych wątkach, ponieważ może służyć jako pojedynczą.</span><span class="sxs-lookup"><span data-stu-id="f17a2-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="f17a2-114">W wielu przypadkach, w których obiekt zwrócony, w którym to przypadku to fabryka sama fabryka musi być metodą o bezpiecznych wątkach, ale obiekt zwrócony z fabryki nie musi być metodą o bezpiecznych wątkach, ponieważ zażądano nowe wystąpienie z fabryki dla każdego zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f17a2-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="f17a2-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="f17a2-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="f17a2-116">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-117">**Obiekt zwrócony**: Inicjator bazy danych dla typu podanym kontekście</span><span class="sxs-lookup"><span data-stu-id="f17a2-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="f17a2-118">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="f17a2-119">FUNC < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="f17a2-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="f17a2-120">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="f17a2-121">**Obiekt zwrócony**: fabrykę do tworzenia generator SQL, który może służyć do migracji i inne akcje, które powodują bazy danych ma zostać utworzony, takie jak tworzenie bazy danych z inicjatorami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f17a2-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="f17a2-122">**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET, określanie typu bazy danych, dla którego zostanie wygenerowany SQL.</span><span class="sxs-lookup"><span data-stu-id="f17a2-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="f17a2-123">Na przykład generator programu SQL Server SQL jest zwracana dla klucza "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="f17a2-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-124">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="f17a2-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="f17a2-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="f17a2-126">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-127">**Obiekt zwrócony**: Dostawca EF do użycia dla danego dostawcy o niezmiennej nazwie</span><span class="sxs-lookup"><span data-stu-id="f17a2-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="f17a2-128">**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET, określanie typu bazy danych, dla której dostawca jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="f17a2-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="f17a2-129">Na przykład dostawca programu SQL Server jest zwracana dla klucza "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="f17a2-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-130">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="f17a2-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="f17a2-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="f17a2-132">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-133">**Obiekt zwrócony**: fabryka połączenia, który będzie używany podczas EF, tworzenia połączenia z bazą danych według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="f17a2-134">Oznacza to gdy nie połączenia lub parametry połączenia znajduje się do programu EF, a nie ciągu połączenia można znaleźć w pliku app.config lub web.config, następnie ta usługa służy do tworzenia połączenia zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="f17a2-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="f17a2-135">Zmiana fabryka połączenia można zezwolić EF użyć innego typu bazy danych (np. SQL Server Compact Edition) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="f17a2-135">Changing the connection factory can allow EF to use a different type of database (e.g. SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="f17a2-136">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-137">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="f17a2-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="f17a2-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="f17a2-139">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-140">**Obiekt zwrócony**: usługi, który można wygenerować token manifestu dostawcy z połączenia.</span><span class="sxs-lookup"><span data-stu-id="f17a2-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="f17a2-141">Ta usługa jest zwykle używana na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="f17a2-141">This service is typically used in two ways.</span></span> <span data-ttu-id="f17a2-142">Po pierwsze może służyć w celu uniknięcia Code First połączenie z bazą danych, podczas tworzenia modelu.</span><span class="sxs-lookup"><span data-stu-id="f17a2-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="f17a2-143">Po drugie może służyć do wymuszenia Code First na budowanie modelu na potrzeby wersji konkretnej bazy danych — na przykład, aby wymusić model dla programu SQL Server 2005, nawet jeśli czasami jest używany program SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="f17a2-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="f17a2-144">**Okres istnienia obiektu**: pojedyncze — ten sam obiekt może być używana wiele razy, a jednocześnie przez inne wątki</span><span class="sxs-lookup"><span data-stu-id="f17a2-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="f17a2-145">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="f17a2-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="f17a2-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="f17a2-147">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-148">**Obiekt zwrócony**: usługi, który można uzyskać fabryki dostawcy z danego połączenia.</span><span class="sxs-lookup"><span data-stu-id="f17a2-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="f17a2-149">W .NET 4.5 dostawca jest publicznie dostępny z poziomu połączenia.</span><span class="sxs-lookup"><span data-stu-id="f17a2-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="f17a2-150">W .NET 4 Domyślna implementacja tej usługi używa niektóre heurystyki w celu odnalezienia pasującego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="f17a2-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="f17a2-151">Jeśli te nie powiodą się następnie nową metodę implementacji tej usługi można zarejestrować zapewniające odpowiednie rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="f17a2-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="f17a2-152">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="f17a2-153">FUNC < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="f17a2-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="f17a2-154">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-155">**Obiekt zwrócony**: fabryki, który zostanie wygenerowany klucz pamięci podręcznej modelu dla danego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="f17a2-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="f17a2-156">Domyślnie program EF buforuje jednego modelu dla typu DbContext dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="f17a2-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="f17a2-157">Inną implementację tej usługi można dodać inne informacje, takie jak nazwa schematu do klucza pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f17a2-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="f17a2-158">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="f17a2-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="f17a2-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="f17a2-160">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-161">**Obiekt zwrócony**: EF przestrzenne dostawcy, który dodaje obsługę podstawowych EF dostawcy typów przestrzennych geometry i położenia geograficznego.</span><span class="sxs-lookup"><span data-stu-id="f17a2-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="f17a2-162">**Klucz**: DbSptialServices zostanie poproszony o na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="f17a2-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="f17a2-163">Po pierwsze, właściwe dla dostawcy usług przestrzennych, są żądane przy użyciu obiektu DbProviderInfo (zawierający niezmiennej token nazwy, a manifestu) jako klucza.</span><span class="sxs-lookup"><span data-stu-id="f17a2-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="f17a2-164">Po drugie DbSpatialServices można monit o wpisanie bez klucza.</span><span class="sxs-lookup"><span data-stu-id="f17a2-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="f17a2-165">Służy to rozwiązać "globalne przestrzenne dostawcę" który jest używany podczas tworzenia autonomicznego DbGeography lub DbGeometry typów.</span><span class="sxs-lookup"><span data-stu-id="f17a2-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-166">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="f17a2-167">FUNC < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="f17a2-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="f17a2-168">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-169">**Obiekt zwrócony**: fabryki, aby utworzyć usługę, która umożliwia dostawcy zaimplementować ponownych prób lub inne zachowanie, gdy zapytań i poleceń, które są wykonywane względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f17a2-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="f17a2-170">Jeśli nie dostarczono żadnej implementacji, EF będzie po prostu wykonaj polecenia i propagowanie wyjątki zgłaszane.</span><span class="sxs-lookup"><span data-stu-id="f17a2-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="f17a2-171">Dla programu SQL Server ta usługa służy do zapewnienia zasady ponawiania, co jest szczególnie przydatne, jeśli działających w odniesieniu do serwerów bazy danych opartej na chmurze, takich jak SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="f17a2-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="f17a2-172">**Klucz**: obiekt ExecutionStrategyKey, który zawiera nazwę niezmienną dostawcy i opcjonalnie nazwy serwera, dla której będzie służyć strategii wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f17a2-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-173">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="f17a2-174">FUNC < DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="f17a2-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="f17a2-175">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-176">**Obiekt zwrócony**: fabrykę, która umożliwia dostawcy skonfigurować mapowanie HistoryContext do `__MigrationHistory` tabeli używanej przez migracje EF.</span><span class="sxs-lookup"><span data-stu-id="f17a2-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="f17a2-177">HistoryContext jest pierwszy typu DbContext kodu i można skonfigurować przy użyciu normalnych wygodnego interfejsu API można zmienić elementów, takich jak nazwy tabeli i specyfikacji mapowania kolumn.</span><span class="sxs-lookup"><span data-stu-id="f17a2-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="f17a2-178">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-179">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="f17a2-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="f17a2-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="f17a2-181">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-182">**Obiekt zwrócony**: Dostawca ADO.NET do użycia dla danego dostawcy o niezmiennej nazwie.</span><span class="sxs-lookup"><span data-stu-id="f17a2-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="f17a2-183">**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET</span><span class="sxs-lookup"><span data-stu-id="f17a2-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-184">Ta usługa nie jest zazwyczaj zmieniany bezpośrednio od implementacji domyślnej jest używana normalnej rejestracji dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f17a2-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="f17a2-185">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="f17a2-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="f17a2-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="f17a2-187">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-188">**Obiekt zwrócony**: usługa, która jest używana do określenia dla danego typu DbProviderFactory nazwę niezmienną dostawcy.</span><span class="sxs-lookup"><span data-stu-id="f17a2-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="f17a2-189">Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f17a2-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="f17a2-190">Oznacza to, że jeśli dostawcy ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozwiązywany za EF, następnie również będzie wymagany do rozwiązania tej usługi.</span><span class="sxs-lookup"><span data-stu-id="f17a2-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="f17a2-191">**Klucz**: DbProviderFactory wystąpienia, dla których niezmienna nazwa jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="f17a2-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="f17a2-192">Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="f17a2-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="f17a2-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="f17a2-194">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-195">**Obiekt zwrócony**: pamięć podręczna zestawów, które zawierają wstępnie wygenerowanych widoków.</span><span class="sxs-lookup"><span data-stu-id="f17a2-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="f17a2-196">Zazwyczaj służy zamiennika umożliwiające EF wiedzieć, zestawy, które zawierają wstępnie wygenerowanych widoków bez żadnych odnajdywania.</span><span class="sxs-lookup"><span data-stu-id="f17a2-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="f17a2-197">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="f17a2-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="f17a2-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="f17a2-199">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-200">**Obiekt zwrócony**: Usługa używana przez EF w liczbie mnogiej do nazw końcówek.</span><span class="sxs-lookup"><span data-stu-id="f17a2-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="f17a2-201">Domyślnie usługa angielskiej pluralizacja jest używana.</span><span class="sxs-lookup"><span data-stu-id="f17a2-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="f17a2-202">**Klucz**: nie jest używany; będzie miał wartość null</span><span class="sxs-lookup"><span data-stu-id="f17a2-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="f17a2-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="f17a2-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="f17a2-204">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="f17a2-205">**Obiekty zwrócone**: wszelkie interceptory, które powinny być rejestrowane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="f17a2-206">Należy zauważyć, że te obiekty są żądane za pomocą wywołania funkcji GetServices i interceptory wszystkie zwrócone przez dowolnego mechanizmu rozpoznawania zależności zostanie zarejestrowana.</span><span class="sxs-lookup"><span data-stu-id="f17a2-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="f17a2-207">**Klucz**: nie jest używany; będzie miał wartość null.</span><span class="sxs-lookup"><span data-stu-id="f17a2-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="f17a2-208">FUNC < System.Data.Entity.DbContext, akcja < ciąg\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="f17a2-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="f17a2-209">**Wprowadzona w wersji**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="f17a2-210">**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenia elementu formatującego dziennika bazy danych, który będzie używany podczas kontekstu. Database.Log właściwość jest ustawiona w danym kontekście.</span><span class="sxs-lookup"><span data-stu-id="f17a2-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="f17a2-211">**Klucz**: nie jest używany; będzie miał wartość null.</span><span class="sxs-lookup"><span data-stu-id="f17a2-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="f17a2-212">FUNC < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="f17a2-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="f17a2-213">**Wprowadzona w wersji**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="f17a2-214">**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenia wystąpień kontekstu dla migracji, gdy kontekst nie jest dostępny konstruktor bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="f17a2-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="f17a2-215">**Klucz**: typ obiektu dla typu pochodnego typu DbContext potrzeby fabrykę.</span><span class="sxs-lookup"><span data-stu-id="f17a2-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="f17a2-216">FUNC < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="f17a2-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="f17a2-217">**Wprowadzona w wersji**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="f17a2-218">**Obiekt zwrócony**: fabrykę, która będzie służyć do utworzyć serializatorów serializacji silnie typizowane niestandardowe adnotacje taki sposób, że może być serializowany i desterilized do formatu XML do użycia w migracji Code First.</span><span class="sxs-lookup"><span data-stu-id="f17a2-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="f17a2-219">**Klucz**: Nazwa adnotacji, który jest serializowany lub deserializowany.</span><span class="sxs-lookup"><span data-stu-id="f17a2-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="f17a2-220">FUNC < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="f17a2-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="f17a2-221">**Wprowadzona w wersji**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="f17a2-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="f17a2-222">**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenie programów do obsługi transakcji, tak aby specjalnej obsługi można zastosować w sytuacjach, takich jak obsługa zatwierdzenia awarie.</span><span class="sxs-lookup"><span data-stu-id="f17a2-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="f17a2-223">**Klucz**: obiekt ExecutionStrategyKey, który zawiera nazwę niezmienną dostawcy i opcjonalnie nazwy serwera, dla której będzie używany program obsługi transakcji.</span><span class="sxs-lookup"><span data-stu-id="f17a2-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
