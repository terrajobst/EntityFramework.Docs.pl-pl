---
title: Dostawcy w programie Entity Framework - EF6
author: divega
ms.date: 2018-06-27
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
caps.latest.revision: 3
ms.openlocfilehash: 8bd5a5a420d741accd1167845575e23c09579ae1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914300"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="64bc3-102">Dostawcy programu Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="64bc3-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="64bc3-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="64bc3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="64bc3-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="64bc3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="64bc3-105">Entity Framework jest obecnie opracowywany w ramach licencji open source i EF6 i powyżej nie zostanie dostarczona w ramach programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="64bc3-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="64bc3-106">Ma wiele zalet, ale wymaga również odbudowany EF dostawców dla zestawów platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="64bc3-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="64bc3-107">Oznacza to, że EF dostawców dla EF5, jak i poniżej nie będą działać przy użyciu platformy EF6, dopóki ich ponownej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="64bc3-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="64bc3-108">Które są dostępni dostawcy dla platformy EF6?</span><span class="sxs-lookup"><span data-stu-id="64bc3-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="64bc3-109">Sobie sprawę z dostawcami, ponownej kompilacji dla platformy EF6 obejmują:</span><span class="sxs-lookup"><span data-stu-id="64bc3-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="64bc3-110">**Dostawca programu Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="64bc3-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="64bc3-111">Utworzona na podstawie [Entity Framework Otwórz bazy kodu źródłowego](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="64bc3-111">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="64bc3-112">Dostarczany jako część [pakiet NuGet platformy EntityFramework](http://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="64bc3-112">Shipped as part of the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="64bc3-113">**Dostawca programu Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="64bc3-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="64bc3-114">Utworzona na podstawie [Entity Framework Otwórz bazy kodu źródłowego](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="64bc3-114">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="64bc3-115">Dostarczana z [pakietu EntityFramework.SqlServerCompact NuGet](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="64bc3-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="64bc3-116">**Devart dotConnect dostawców danych**</span><span class="sxs-lookup"><span data-stu-id="64bc3-116">**Devart dotConnect Data Providers**</span></span>](http://www.devart.com/dotconnect/)
    *   <span data-ttu-id="64bc3-117">Brak dostawców innych firm [Devart](http://www.devart.com/) dla wielu baz danych w tym Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 i programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="64bc3-117">There are third-party providers from [Devart](http://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="64bc3-118">**Dostawców oprogramowania CData**</span><span class="sxs-lookup"><span data-stu-id="64bc3-118">**CData Software providers**</span></span>](http://www.cdata.com/ado/)
    *   <span data-ttu-id="64bc3-119">Brak dostawców innych firm [oprogramowania CData](http://www.cdata.com/ado/) z różnych magazynów danych, w tym Salesforce, usługa Azure Table Storage, MySql i wielu innych</span><span class="sxs-lookup"><span data-stu-id="64bc3-119">There are third-party providers from [CData Software](http://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="64bc3-120">**Dostawca firebird**</span><span class="sxs-lookup"><span data-stu-id="64bc3-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="64bc3-121">Dostępne jako [pakietu NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span><span class="sxs-lookup"><span data-stu-id="64bc3-121">Available as a [NuGet Package](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span></span>
*   <span data-ttu-id="64bc3-122">**Visual Fox Pro dostawcy**</span><span class="sxs-lookup"><span data-stu-id="64bc3-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="64bc3-123">Dostępne jako [pakietu NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="64bc3-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="64bc3-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="64bc3-124">**MySQL**</span></span>
    *   [<span data-ttu-id="64bc3-125">MySQL Connector/Net</span><span class="sxs-lookup"><span data-stu-id="64bc3-125">MySQL Connector/Net</span></span>](http://dev.mysql.com/downloads/connector/net/)
*   <span data-ttu-id="64bc3-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="64bc3-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="64bc3-127">Npgsql jest dostępna jako [pakietu NuGet](http://www.nuget.org/packages/Npgsql.EF6/)</span><span class="sxs-lookup"><span data-stu-id="64bc3-127">Npgsql is available as a [NuGet package](http://www.nuget.org/packages/Npgsql.EF6/)</span></span>
*   <span data-ttu-id="64bc3-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="64bc3-128">**Oracle**</span></span>
    *   <span data-ttu-id="64bc3-129">ODP.NET jest dostępna jako [pakietu NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="64bc3-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="64bc3-130">Należy pamiętać, włączenia na tej liście nie wskazuje poziom funkcjonalności lub pomocy technicznej dla danego dostawcy, tylko że kompilacji dla platformy EF6 została udostępniona.</span><span class="sxs-lookup"><span data-stu-id="64bc3-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="64bc3-131">Rejestrowanie dostawców EF</span><span class="sxs-lookup"><span data-stu-id="64bc3-131">Registering EF providers</span></span>

<span data-ttu-id="64bc3-132">Począwszy od dostawcy programu Entity Framework 6 EF można zarejestrować za pomocą konfiguracji albo oparte na kodzie lub w pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64bc3-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="64bc3-133">Rejestracja pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="64bc3-133">Config file registration</span></span>

<span data-ttu-id="64bc3-134">Rejestracja dostawcy EF w pliku app.config lub web.config ma następujący format:</span><span class="sxs-lookup"><span data-stu-id="64bc3-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="64bc3-135">Należy pamiętać, że często Jeśli dostawca programu EF jest instalowany z pakietów NuGet, a następnie pakietu NuGet spowoduje automatyczne dodanie tej rejestracji do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="64bc3-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="64bc3-136">Po zainstalowaniu pakietu NuGet do projektu, który nie jest projektem startowym aplikacji może należy skopiować rejestracji do pliku konfiguracji dla Twój projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="64bc3-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="64bc3-137">"Invatiantname" w tym rejestracji jest taka sama nazwa niezmienna, używany do identyfikowania dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="64bc3-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="64bc3-138">Ten znajduje się jako atrybut "niezmiennej" w rejestracji DbProviderFactories, a atrybut "providerName" podczas rejestracji ciąg połączenia.</span><span class="sxs-lookup"><span data-stu-id="64bc3-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="64bc3-139">Niezmienna Nazwa do użycia powinny też obejmować w dokumentacji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="64bc3-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="64bc3-140">Przykłady nazw niezmiennej są "System.Data.SqlClient" dla programu SQL Server i "System.Data.SqlServerCe.4.0" dla programu SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="64bc3-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="64bc3-141">"Type" w tym rejestracji jest nazwa kwalifikowanego dla zestawu typu dostawcy, która dziedziczy po "System.Data.Entity.Core.Common.DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="64bc3-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="64bc3-142">Na przykład ciąg do użycia dla programu SQL Compact jest "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="64bc3-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="64bc3-143">Typ do użycia w tym miejscu powinny być objęte dokumentacji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="64bc3-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="64bc3-144">Oparte na kodzie rejestracji</span><span class="sxs-lookup"><span data-stu-id="64bc3-144">Code-based registration</span></span>

<span data-ttu-id="64bc3-145">Począwszy od platformy Entity Framework 6 konfiguracji całej aplikacji na platformie EF można określić w kodzie.</span><span class="sxs-lookup"><span data-stu-id="64bc3-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="64bc3-146">Szczegółowe informacje można znaleźć  _[konfiguracji Entity Framework Code-Based](https://msdn.microsoft.com/en-us/data/jj680699)_.</span><span class="sxs-lookup"><span data-stu-id="64bc3-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/en-us/data/jj680699)_.</span></span> <span data-ttu-id="64bc3-147">Normalny sposób, aby zarejestrować dostawcę EF przy użyciu konfiguracji na podstawie kodu jest Utwórz nową klasę dziedziczącą po System.Data.Entity.DbConfiguration i umieść go w tym samym zestawie jako swojej klasy DbContext.</span><span class="sxs-lookup"><span data-stu-id="64bc3-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="64bc3-148">Klasa DbConfiguration należy następnie zarejestruj dostawcę w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="64bc3-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="64bc3-149">Na przykład aby zarejestrować SQL Compact dostawcy klasy DbConfiguration wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="64bc3-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="64bc3-150">W tym kodzie "SqlCeProviderServices.ProviderInvariantName" to wygodne dla programu SQL Server Compact ciągu niezmienna Nazwa dostawcy ("System.Data.SqlServerCe.4.0") i SqlCeProviderServices.Instance zwraca pojedyncze wystąpienie SQL Compact Dostawca EF.</span><span class="sxs-lookup"><span data-stu-id="64bc3-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="64bc3-151">Co się stanie, jeśli dostawca potrzebuję nie jest dostępna?</span><span class="sxs-lookup"><span data-stu-id="64bc3-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="64bc3-152">Jeśli dostawca jest dostępny we wcześniejszych wersjach programu EF, następnie zachęcamy Cię do skontaktuj się z właścicielem dostawcy i poproś go o utworzenia wersji EF6.</span><span class="sxs-lookup"><span data-stu-id="64bc3-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="64bc3-153">Należy dołączyć odwołanie do [dokumentacji dla modelu dostawcy EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="64bc3-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="64bc3-154">Można napisać dostawcę samodzielnie?</span><span class="sxs-lookup"><span data-stu-id="64bc3-154">Can I write a provider myself?</span></span>

<span data-ttu-id="64bc3-155">Oczywiście prawdopodobnie utworzyć dostawcę EF samodzielnie, mimo że nie należy rozważyć trivial przedsiębiorstwa.</span><span class="sxs-lookup"><span data-stu-id="64bc3-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="64bc3-156">Powyższe łącze o modelu dostawcy EF6 jest dobrym miejscem do rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="64bc3-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="64bc3-157">Możesz również przydatne może okazać się do użycia kodu dla programu SQL Server i SQL CE dostawcy uwzględnione w [codebase "open source" EF](https://github.com/aspnet/EntityFramework6) jako punktu wyjścia lub odwołania.</span><span class="sxs-lookup"><span data-stu-id="64bc3-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="64bc3-158">Należy pamiętać, że począwszy od platformy EF6 dostawcy EF jest mniej ściśle powiązany źródłowy dostawca ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="64bc3-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="64bc3-159">Ta funkcja ułatwia pisanie dostawcy programu EF bez konieczności pisania lub opakowywania klas ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="64bc3-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
