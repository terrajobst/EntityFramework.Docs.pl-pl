---
title: Dostawcy Entity Framework — EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419364"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="497ae-102">Entity Framework 6 dostawców</span><span class="sxs-lookup"><span data-stu-id="497ae-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="497ae-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="497ae-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="497ae-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="497ae-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="497ae-105">Entity Framework jest teraz opracowywany w ramach licencji Open Source i EF6, a powyższe nie będą wysyłane jako część .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="497ae-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="497ae-106">Ma wiele korzyści, ale wymaga również, aby dostawcy EF mogli odbudować zestawy EF6.</span><span class="sxs-lookup"><span data-stu-id="497ae-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="497ae-107">Oznacza to, że dostawcy EF dla EF5 i poniżej nie będą współpracować z EF6, dopóki nie zostaną ponownie skompilowane.</span><span class="sxs-lookup"><span data-stu-id="497ae-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="497ae-108">Którzy dostawcy są dostępni dla EF6?</span><span class="sxs-lookup"><span data-stu-id="497ae-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="497ae-109">Dostawcy wie, że zostały skompilowane dla EF6:</span><span class="sxs-lookup"><span data-stu-id="497ae-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="497ae-110">**Dostawca Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="497ae-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="497ae-111">Kompilacja oparta na [Entity Framework bazie kodu open source](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="497ae-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="497ae-112">Dostarczane jako część [pakietu NuGet EntityFramework](https://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="497ae-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="497ae-113">**Dostawca wersji Microsoft SQL Server Compact**</span><span class="sxs-lookup"><span data-stu-id="497ae-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="497ae-114">Kompilacja oparta na [Entity Framework bazie kodu open source](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="497ae-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="497ae-115">Dostarczone w [pakiecie NuGet EntityFramework. SqlServerCompact](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="497ae-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="497ae-116">**Dostawcy danych Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="497ae-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="497ae-117">Istnieją dostawcy innych firm od [Devart](https://www.devart.com/) dla różnych baz danych, takich jak Oracle, MySQL, PostgreSQL, SQLite, SALESFORCE, DB2 i SQL Server</span><span class="sxs-lookup"><span data-stu-id="497ae-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="497ae-118">**CData dostawcy oprogramowania**</span><span class="sxs-lookup"><span data-stu-id="497ae-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="497ae-119">Istnieją dostawcy innych firm z [CDATA oprogramowania](https://www.cdata.com/ado/) dla różnych magazynów danych, w tym usług Salesforce, Azure Table Storage, MySQL i wielu innych</span><span class="sxs-lookup"><span data-stu-id="497ae-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="497ae-120">**Dostawca Firebird**</span><span class="sxs-lookup"><span data-stu-id="497ae-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="497ae-121">Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)</span><span class="sxs-lookup"><span data-stu-id="497ae-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="497ae-122">**Dostawca programu Visual Fox Pro**</span><span class="sxs-lookup"><span data-stu-id="497ae-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="497ae-123">Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="497ae-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="497ae-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="497ae-124">**MySQL**</span></span>
    *   [<span data-ttu-id="497ae-125">Łącznik MySQL/NET dla Entity Framework</span><span class="sxs-lookup"><span data-stu-id="497ae-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="497ae-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="497ae-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="497ae-127">Npgsql jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span><span class="sxs-lookup"><span data-stu-id="497ae-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="497ae-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="497ae-128">**Oracle**</span></span>
    *   <span data-ttu-id="497ae-129">ODP.NET jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="497ae-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="497ae-130">Należy pamiętać, że włączenie na tej liście nie wskazuje poziomu funkcjonalności ani wsparcia dla danego dostawcy, tylko wtedy, gdy udostępniono kompilację dla EF6.</span><span class="sxs-lookup"><span data-stu-id="497ae-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="497ae-131">Rejestrowanie dostawców EF</span><span class="sxs-lookup"><span data-stu-id="497ae-131">Registering EF providers</span></span>

<span data-ttu-id="497ae-132">Począwszy od Entity Frameworkych dostawców EF można zarejestrować przy użyciu konfiguracji opartej na kodzie lub w pliku konfiguracyjnym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="497ae-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="497ae-133">Rejestracja pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="497ae-133">Config file registration</span></span>

<span data-ttu-id="497ae-134">Rejestracja dostawcy EF w pliku App. config lub Web. config ma następujący format:</span><span class="sxs-lookup"><span data-stu-id="497ae-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="497ae-135">Należy pamiętać, że często Jeśli dostawca EF jest instalowany z narzędzia NuGet, pakiet NuGet automatycznie doda tę rejestrację do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="497ae-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="497ae-136">W przypadku zainstalowania pakietu NuGet w projekcie, który nie jest projektem startowym dla aplikacji, może być konieczne skopiowanie rejestracji do pliku konfiguracji dla projektu startowego.</span><span class="sxs-lookup"><span data-stu-id="497ae-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="497ae-137">"Invariantname" w tej rejestracji jest tą samą niezmienną nazwą, która jest używana do identyfikowania dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="497ae-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="497ae-138">Można go znaleźć jako atrybut "Invariant" w rejestracji DbProviderFactories i jako atrybut "ProviderName" w rejestracji parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="497ae-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="497ae-139">Niezmienna nazwa do użycia powinna również być uwzględniona w dokumentacji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="497ae-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="497ae-140">Przykłady nazw niezmiennych to "System. Data. SqlClient" dla SQL Server i "System. Data. SqlServerCe. 4.0" dla SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="497ae-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="497ae-141">"Typ" w tej rejestracji to kwalifikowana dla zestawu nazwa typu dostawcy, który pochodzi od "System. Data. Entity. Core. Common. DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="497ae-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="497ae-142">Na przykład ciąg używany na potrzeby języka SQL Compact to "System. Data. Entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="497ae-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="497ae-143">Typ, który ma być używany w tym miejscu, powinien zostać uwzględniony w dokumentacji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="497ae-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="497ae-144">Rejestracja oparta na kodzie</span><span class="sxs-lookup"><span data-stu-id="497ae-144">Code-based registration</span></span>

<span data-ttu-id="497ae-145">Począwszy od Entity Framework 6 Konfiguracja całej aplikacji dla EF można określić w kodzie.</span><span class="sxs-lookup"><span data-stu-id="497ae-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="497ae-146">Aby uzyskać szczegółowe informacje, zobacz _[Entity Framework konfiguracji opartej na kodzie](https://msdn.microsoft.com/data/jj680699)_ .</span><span class="sxs-lookup"><span data-stu-id="497ae-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="497ae-147">Normalny sposób zarejestrowania dostawcy EF przy użyciu konfiguracji opartej na kodzie polega na utworzeniu nowej klasy, która pochodzi od klasy System. Data. Entity. dbconfiguration i umieścić ją w tym samym zestawie co Klasa DbContext.</span><span class="sxs-lookup"><span data-stu-id="497ae-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="497ae-148">Klasa dbconfiguration powinna następnie zarejestrować dostawcę w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="497ae-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="497ae-149">Na przykład, aby zarejestrować dostawcę programu SQL Compact, Klasa dbconfiguration wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="497ae-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="497ae-150">W tym kodzie "SqlCeProviderServices. ProviderInvariantName" jest wygodnym ciągiem nazw niezmiennej dostawcy SQL Server Compact ("System. Data. SqlServerCe. 4.0") i SqlCeProviderServices. Instance zwraca pojedyncze wystąpienie programu SQL Compact Dostawca EF.</span><span class="sxs-lookup"><span data-stu-id="497ae-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="497ae-151">Co zrobić, jeśli potrzebny dostawca nie jest dostępny?</span><span class="sxs-lookup"><span data-stu-id="497ae-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="497ae-152">Jeśli dostawca jest dostępny dla poprzednich wersji EF, zachęcamy do skontaktowania się z właścicielem dostawcy i poproszenia o utworzenie wersji EF6.</span><span class="sxs-lookup"><span data-stu-id="497ae-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="497ae-153">Należy dołączyć odwołanie do [dokumentacji modelu dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="497ae-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="497ae-154">Czy mogę napisać dostawcę samodzielnie?</span><span class="sxs-lookup"><span data-stu-id="497ae-154">Can I write a provider myself?</span></span>

<span data-ttu-id="497ae-155">Z pewnością można utworzyć dostawcę EF, chociaż nie należy go traktować jako prostego przedsiębiorstwa.</span><span class="sxs-lookup"><span data-stu-id="497ae-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="497ae-156">Powyższy link dotyczący modelu dostawcy EF6 jest dobrym miejscem do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="497ae-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="497ae-157">Przydatne może być również użycie kodu dla dostawcy SQL Server i programu SQL CE zawartego w bazie [kodu "open source" EF](https://github.com/aspnet/EntityFramework6) jako punktu początkowego lub do odwołania.</span><span class="sxs-lookup"><span data-stu-id="497ae-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="497ae-158">Należy pamiętać, że począwszy od EF6 dostawca EF jest mniej ściśle połączony z podstawowym dostawcą ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="497ae-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="497ae-159">Ułatwia to pisanie dostawcy EF bez konieczności pisania ani zawijania klas ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="497ae-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
