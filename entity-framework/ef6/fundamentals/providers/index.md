---
title: Dostawcy struktury podmiotów — EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419364"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="aa4d3-102">Dostawcy struktury podmiotów 6</span><span class="sxs-lookup"><span data-stu-id="aa4d3-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="aa4d3-103">**Ef6 Tylko —** funkcje, interfejsy API, itp omówione na tej stronie zostały wprowadzone w entity framework 6.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aa4d3-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie mają zastosowania.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="aa4d3-105">Entity Framework jest obecnie opracowywany w ramach licencji open-source i EF6 i powyżej nie zostaną wysłane w ramach programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="aa4d3-106">Ma to wiele zalet, ale wymaga również, aby dostawcy EF zostać przebudowany względem zestawów EF6.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="aa4d3-107">Oznacza to, że ef dostawców EF5 i poniżej nie będzie działać z EF6, dopóki nie zostały przebudowane.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="aa4d3-108">Którzy dostawcy są dostępni dla EF6?</span><span class="sxs-lookup"><span data-stu-id="aa4d3-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="aa4d3-109">Dostawcy, których jesteśmy świadomi, że zostały przebudowane dla EF6 obejmują:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="aa4d3-110">**Dostawca programu Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="aa4d3-111">Zbudowany z [bazy kodu open source Entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="aa4d3-112">Dostarczany jako część [pakietu EntityFramework NuGet](https://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="aa4d3-113">**Dostawca programu Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="aa4d3-114">Zbudowany z [bazy kodu open source Entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="aa4d3-115">Dostarczane w [pakiecie EntityFramework.SqlServerCompact NuGet](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="aa4d3-116">**Dostawcy danych Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="aa4d3-117">Istnieją dostawcy zewnętrzni z [Devart](https://www.devart.com/) dla różnych baz danych, w tym Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 i SQL Server</span><span class="sxs-lookup"><span data-stu-id="aa4d3-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="aa4d3-118">**Dostawcy oprogramowania CData**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="aa4d3-119">Istnieją dostawcy zewnętrzni z [CData Software](https://www.cdata.com/ado/) dla różnych magazynów danych, w tym Salesforce, Azure Table Storage, MySql i wielu innych</span><span class="sxs-lookup"><span data-stu-id="aa4d3-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="aa4d3-120">**Dostawca firebird**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="aa4d3-121">Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="aa4d3-122">**Dostawca visual fox pro**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="aa4d3-123">Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="aa4d3-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-124">**MySQL**</span></span>
    *   [<span data-ttu-id="aa4d3-125">Łącznik MySQL/NET dla entity framework</span><span class="sxs-lookup"><span data-stu-id="aa4d3-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="aa4d3-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="aa4d3-127">Npgsql jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="aa4d3-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-128">**Oracle**</span></span>
    *   <span data-ttu-id="aa4d3-129">ODP.NET jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="aa4d3-130">Należy zauważyć, że włączenie do tej listy nie wskazuje poziomu funkcjonalności lub obsługi dla danego dostawcy, tylko, że kompilacja dla EF6 została udostępniona.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="aa4d3-131">Rejestrowanie dostawców EF</span><span class="sxs-lookup"><span data-stu-id="aa4d3-131">Registering EF providers</span></span>

<span data-ttu-id="aa4d3-132">Począwszy od entity framework 6 EF dostawców mogą być rejestrowane przy użyciu konfiguracji opartej na kodzie lub w pliku konfiguracyjnym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="aa4d3-133">Rejestracja pliku konfiguracyjnego</span><span class="sxs-lookup"><span data-stu-id="aa4d3-133">Config file registration</span></span>

<span data-ttu-id="aa4d3-134">Rejestracja dostawcy EF w app.config lub web.config ma następujący format:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="aa4d3-135">Należy zauważyć, że często, jeśli dostawca EF jest zainstalowany z NuGet, a następnie pakiet NuGet automatycznie dodać tę rejestrację do pliku konfiguracyjnego.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="aa4d3-136">Jeśli zainstalujesz pakiet NuGet w projekcie, który nie jest projektem startowym dla aplikacji, może być konieczne skopiowanie rejestracji do pliku konfiguracyjnego dla projektu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="aa4d3-137">"InvariantName" w tej rejestracji jest taka sama niezmienna nazwa używana do identyfikowania dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="aa4d3-138">Można to znaleźć jako atrybut "niezmienny" w DbProviderFactories rejestracji i jako "providerName" atrybut w rejestracji ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="aa4d3-139">Niezmienna nazwa do użycia powinna być również uwzględniona w dokumentacji dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="aa4d3-140">Przykładami niezmiennych nazw są "System.Data.SqlClient" dla programu SQL Server i "System.Data.SqlServerCe.4.0" dla programu SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="aa4d3-141">"Typ" w tej rejestracji jest nazwą kwalifikowaną do zestawu typu dostawcy, która pochodzi od "System.Data.Entity.Core.Common.DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="aa4d3-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="aa4d3-142">Na przykład ciąg używany dla SQL Compact to "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="aa4d3-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="aa4d3-143">Typ do użycia w tym miejscu powinny być zawarte w dokumentacji dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="aa4d3-144">Rejestracja oparta na kodzie</span><span class="sxs-lookup"><span data-stu-id="aa4d3-144">Code-based registration</span></span>

<span data-ttu-id="aa4d3-145">Począwszy od entity framework 6 konfiguracji całej aplikacji dla EF można określić w kodzie.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="aa4d3-146">Aby uzyskać szczegółowe informacje, zobacz _[Konfiguracja oparta na kodzie entity framework](https://msdn.microsoft.com/data/jj680699)_.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="aa4d3-147">Normalnym sposobem zarejestrowania dostawcy ef przy użyciu konfiguracji opartej na kodzie jest utworzenie nowej klasy, która wywodzi się z System.Data.Entity.DbConfiguration i umieścić go w tym samym zestawie co klasa DbContext.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="aa4d3-148">Klasa DbConfiguration powinna następnie zarejestrować dostawcę w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="aa4d3-149">Na przykład, aby zarejestrować dostawcę sql compact klasa DbConfiguration wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="aa4d3-150">W tym kodzie "SqlCeProviderServices.ProviderInvariantName" jest wygoda dla dostawcy SQL Server Compact niezmienny ciąg nazwy ("System.Data.SqlServerCe.4.0") i SqlCeProviderServices.Instance zwraca pojedyncze wystąpienie dostawcy SQL Compact EF.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="aa4d3-151">Co zrobić, jeśli potrzebny mi dostawca nie jest dostępny?</span><span class="sxs-lookup"><span data-stu-id="aa4d3-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="aa4d3-152">Jeśli dostawca jest dostępny dla poprzednich wersji EF, firma Microsoft zachęcamy do skontaktowania się z właścicielem dostawcy i poprosić o utworzenie wersji EF6.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="aa4d3-153">Należy dołączyć odwołanie do [dokumentacji dla modelu dostawcy EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="aa4d3-154">Czy mogę napisać dostawcę samodzielnie?</span><span class="sxs-lookup"><span data-stu-id="aa4d3-154">Can I write a provider myself?</span></span>

<span data-ttu-id="aa4d3-155">Z pewnością możliwe jest samodzielne utworzenie dostawcy ef, chociaż nie należy go uważać za trywialne przedsięwzięcie.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="aa4d3-156">Powyższe łącze dotyczące modelu dostawcy EF6 jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="aa4d3-157">Może się również okazać przydatne użycie kodu dla dostawcy SQL Server i SQL CE zawarte w [bazie kodu open source EF](https://github.com/aspnet/EntityFramework6) jako punkt wyjścia lub w celach informacyjnych.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="aa4d3-158">Należy zauważyć, że począwszy od EF6 dostawcy EF jest mniej ściśle powiązane z podstawowym dostawcą ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="aa4d3-159">Ułatwia to pisanie dostawcy EF bez konieczności pisania lub zawijania klas ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
