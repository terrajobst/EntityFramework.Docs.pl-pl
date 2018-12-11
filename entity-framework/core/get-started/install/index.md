---
title: Instalowanie platformy Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 58c79d477d590eea355a922b3e1233bbecb305cc
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181984"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="c7be2-102">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7be2-103">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c7be2-103">Prerequisites</span></span>

* <span data-ttu-id="c7be2-104">EF Core jest [.NET Standard 2.0](/dotnet/standard/net-standard) biblioteki.</span><span class="sxs-lookup"><span data-stu-id="c7be2-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="c7be2-105">EF Core wymaga implementacji .NET, która obsługuje .NET Standard 2.0 do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="c7be2-106">EF Core również mogą być przywoływane przez inne biblioteki .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c7be2-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="c7be2-107">Na przykład można użyć programu EF Core programować aplikacje, których platformą docelową .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="c7be2-108">Tworzenie aplikacji .NET Core wymaga [zestawu .NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c7be2-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="c7be2-109">Opcjonalnie umożliwia także środowiska programowania, takich jak Visual Studio, Visual Studio for Mac lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c7be2-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="c7be2-110">Aby uzyskać więcej informacji, sprawdź [wprowadzenie do platformy .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="c7be2-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="c7be2-111">EF Core służy do tworzenia aplikacji, których platformą docelową .NET Framework 4.6.1 lub później na Windows, za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7be2-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="c7be2-112">Najnowszą wersję programu Visual Studio jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="c7be2-112">The latest version of Visual Studio is recommended.</span></span> <span data-ttu-id="c7be2-113">Jeśli chcesz używać starszej wersji, takich jak Visual Studio 2015, upewnij się, że [uaktualnić klienta programu NuGet w wersji 3.6.0](https://www.nuget.org/downloads) do pracy z biblioteki .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c7be2-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="c7be2-114">EF Core można uruchamiać na inne implementacje platformy .NET, takich jak Xamarin i .NET Native.</span><span class="sxs-lookup"><span data-stu-id="c7be2-114">EF Core can run on other .NET implementations like Xamarin and .NET Native.</span></span> <span data-ttu-id="c7be2-115">Ale w praktyce te implementacje ograniczenia środowiska uruchomieniowego, które mogą dotyczyć, jak dobrze działa programu EF Core w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7be2-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="c7be2-116">Aby uzyskać więcej informacji, zobacz [implementacji platformy .NET obsługiwanych przez platformę EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="c7be2-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="c7be2-117">Na koniec dostawców innej bazy danych może wymagać konkretnej bazy danych aparatu wersje, implementacje platformy .NET lub systemów operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="c7be2-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="c7be2-118">Upewnij się, że [dostawcy bazy danych programu EF Core](xref:core/providers/index) jest dostępna, która obsługuje odpowiednim środowisku dla danej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7be2-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="c7be2-119">Pobierz środowisko uruchomieniowe programu Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="c7be2-120">Do programu EF Core można dodać do aplikacji, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="c7be2-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="c7be2-121">Jeśli tworzysz aplikację ASP.NET Core, nie trzeba zainstalować w pamięci i dostawców programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c7be2-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="c7be2-122">Te dostawcy są uwzględnieni w bieżących wersjach platformy ASP.NET Core, wraz z środowiska uruchomieniowego EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="c7be2-123">Aby zainstalować lub zaktualizować pakiety NuGet, należy użyć [platformy .NET Core interfejsu wiersza polecenia (CLI), okno Menedżera pakietów Visual Studio lub konsoli Menedżera pakietów Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7be2-123">To install or update NuGet packages, you can use the [.NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="c7be2-124">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c7be2-124">.NET Core CLI</span></span>

* <span data-ttu-id="c7be2-125">Użyj następującego polecenia interfejsu wiersza polecenia platformy .NET Core z wiersza polecenia systemu operacyjnego, aby zainstalować lub zaktualizować dostawcy EF Core programu SQL Server:</span><span class="sxs-lookup"><span data-stu-id="c7be2-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="c7be2-126">Można wskazać określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c7be2-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="c7be2-127">Na przykład, aby zainstalować pakiety programu EF Core 2.2.0, należy dołączyć `-v 2.2.0` do polecenia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="c7be2-128">Aby uzyskać więcej informacji, zobacz [narzędzia interfejsu wiersza polecenia (CLI) platformy .NET](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c7be2-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="c7be2-129">Okno Menedżera pakietów NuGet programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7be2-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="c7be2-130">Wybierz z menu programu Visual Studio **Projekt > Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="c7be2-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="c7be2-131">Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę</span><span class="sxs-lookup"><span data-stu-id="c7be2-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="c7be2-132">Aby zainstalować lub zaktualizować dostawcy programu SQL Server, wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i potwierdź.</span><span class="sxs-lookup"><span data-stu-id="c7be2-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="c7be2-133">Aby uzyskać więcej informacji, zobacz [okno Menedżera pakietów NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="c7be2-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="c7be2-134">Konsola Menedżera pakietów NuGet dla programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7be2-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="c7be2-135">Wybierz z menu programu Visual Studio **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="c7be2-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c7be2-136">Aby zainstalować dostawcę programu SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="c7be2-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="c7be2-137">Aby zaktualizować dostawcę, użyj `Update-Package` polecenia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="c7be2-138">Aby określić określonej wersji, użyj `-Version` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c7be2-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="c7be2-139">Na przykład, aby zainstalować pakiety programu EF Core 2.2.0, należy dołączyć `-Version 2.2.0` poleceń</span><span class="sxs-lookup"><span data-stu-id="c7be2-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="c7be2-140">Aby uzyskać więcej informacji, zobacz [Konsola Menedżera pakietów](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="c7be2-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="c7be2-141">Pobierz narzędzia platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="c7be2-142">Możesz zainstalować narzędzia do wykonywania zadań związanych z programem EF Core w projekcie, takich jak tworzenie i stosowanie migracje baz danych lub tworzenie modelu platformy EF Core oparte na istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c7be2-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="c7be2-143">Dostępne są dwa zestawy narzędzi:</span><span class="sxs-lookup"><span data-stu-id="c7be2-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="c7be2-144">[Narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core](xref:core/miscellaneous/cli/dotnet) może służyć w Windows, Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="c7be2-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="c7be2-145">Te polecenia zaczynają się od `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="c7be2-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="c7be2-146">[Narzędzia konsoli Menedżera pakietów (PMC)](xref:core/miscellaneous/cli/powershell) systemem Windows w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7be2-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="c7be2-147">Te polecenia rozpoczynać czasownika, na przykład `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="c7be2-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="c7be2-148">Mimo że można również użyć `dotnet ef` poleceń z poziomu konsoli Menedżera pakietów, zaleca się korzystania z narzędzi Konsola Menedżera pakietów, podczas korzystania z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c7be2-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="c7be2-149">Działają automatycznie z bieżącego projektu wybrany w konsoli zarządzania Pakietami w programie Visual Studio, bez konieczności ręcznie zmienić katalog.</span><span class="sxs-lookup"><span data-stu-id="c7be2-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="c7be2-150">One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="c7be2-151">Pobierz narzędzia wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="c7be2-152">Narzędzia interfejsu wiersza polecenia platformy .NET core wymagają .NET Core SDK, wymienione wcześniej w [wymagania wstępne](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c7be2-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="c7be2-153">`dotnet ef` Polecenia są zawarte w aktualnych wersjach programu .NET Core SDK, ale aby włączyć polecenia dla danego projektu, musisz zainstalować `Microsoft.EntityFrameworkCore.Design` pakietu:</span><span class="sxs-lookup"><span data-stu-id="c7be2-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="c7be2-154">W przypadku aplikacji platformy ASP.NET Core, ten pakiet jest uwzględniane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c7be2-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="c7be2-155">Zawsze używaj wersji zgodnej wersji głównej pakiety środowiska uruchomieniowego pakietu Narzędzia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="c7be2-156">Pobierz narzędzia w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="c7be2-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="c7be2-157">Aby uzyskać Konsola Menedżera pakietów narzędzi dla platformy EF Core, należy zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="c7be2-158">Na przykład z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c7be2-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="c7be2-159">W przypadku aplikacji platformy ASP.NET Core, ten pakiet jest uwzględniane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c7be2-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="c7be2-160">Uaktualnianie do najnowszej EF Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="c7be2-161">Każdym wydaniu nowej wersji programu EF Core, wydaniu nowej wersji dostawców, które są częścią projektu programu EF Core, takich jak Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, i Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="c7be2-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="c7be2-162">Możesz po prostu uaktualnić do nowej wersji dostawcy, aby uzyskać wszystkie ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="c7be2-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="c7be2-163">EF Core wraz z programu SQL Server i dostawców w pamięci są uwzględnione w aktualnych wersjach programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="c7be2-164">Aby przeprowadzić uaktualnienie do nowszej wersji programu EF Core istniejącej aplikacji platformy ASP.NET Core, zawsze należy uaktualnić wersję platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="c7be2-165">Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="c7be2-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="c7be2-166">Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z wersji 2.0 środowiska uruchomieniowego EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="c7be2-167">Innych dostawców dla platformy EF Core zwykle nie zwalniaj wersji poprawki wraz z środowiska uruchomieniowego EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7be2-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="c7be2-168">Aby uaktualnić aplikację, która używa dostawcy innej firmy, do wersji poprawki programu EF Core, może być konieczne dodanie bezpośrednie odwołanie do poszczególnych składników środowiska uruchomieniowego EF Core, takie jak Microsoft.EntityFrameworkCore i Microsoft.EntityFrameworkCore.Relational.</span><span class="sxs-lookup"><span data-stu-id="c7be2-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="c7be2-169">Jeśli aktualizujesz istniejącą aplikację do najnowszej wersji programu EF Core niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie:</span><span class="sxs-lookup"><span data-stu-id="c7be2-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="c7be2-170">Bazy danych dostawcy projektowania pakietów, takich jak `Microsoft.EntityFrameworkCore.SqlServer.Design` nie są już wymagane lub obsługiwane z programem EF Core 2.0 lub nowszy, ale nie są automatycznie usuwane po uaktualnieniu innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="c7be2-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="c7be2-171">Narzędzia wiersza polecenia platformy .NET obejmuje zestawu .NET SDK od wersji 2.1, dzięki czemu można usunąć odwołania do tego pakietu z pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c7be2-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="c7be2-172">Aplikacji przeznaczonych dla środowiska .NET Framework może być konieczne zmiany, aby pracować z biblioteki .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="c7be2-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="c7be2-173">Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="c7be2-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="c7be2-174">Dla projektów testowych również upewnij się, że występuje następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="c7be2-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
