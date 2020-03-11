---
title: Instalowanie Entity Framework Core-EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 987b6f38954c291f88b5167fa9b061853b15a6cb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416881"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="1c5f1-102">Instalowanie Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c5f1-103">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1c5f1-103">Prerequisites</span></span>

* <span data-ttu-id="1c5f1-104">EF Core jest biblioteką [.NET Standard 2,1](/dotnet/standard/net-standard) .</span><span class="sxs-lookup"><span data-stu-id="1c5f1-104">EF Core is a [.NET Standard 2.1](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="1c5f1-105">Dlatego EF Core wymaga implementacji platformy .NET obsługującej .NET Standard 2,1 do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-105">So EF Core requires a .NET implementation that supports .NET Standard 2.1 to run.</span></span> <span data-ttu-id="1c5f1-106">EF Core można także odwoływać się do innych bibliotek .NET Standard 2,1.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-106">EF Core can also be referenced by other .NET Standard 2.1 libraries.</span></span>

* <span data-ttu-id="1c5f1-107">Na przykład można użyć EF Core do tworzenia aplikacji przeznaczonych dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="1c5f1-108">Tworzenie aplikacji .NET Core wymaga [zestaw .NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="1c5f1-109">Opcjonalnie możesz również użyć środowiska programistycznego, takiego jak [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac)lub [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-109">Optionally, you can also use a development environment like [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac), or [Visual Studio Code](https://code.visualstudio.com).</span></span> <span data-ttu-id="1c5f1-110">Aby uzyskać więcej informacji, sprawdź [wprowadzenie z platformą .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="1c5f1-111">Za pomocą programu Visual Studio EF Core można opracowywać aplikacje w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-111">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="1c5f1-112">Zalecana jest Najnowsza wersja programu [Visual Studio](https://visualstudio.microsoft.com/vs) .</span><span class="sxs-lookup"><span data-stu-id="1c5f1-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="1c5f1-113">EF Core można uruchamiać na innych implementacjach platformy .NET, takich jak [Xamarin](https://dotnet.microsoft.com/apps/xamarin) i .NET Native.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-113">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="1c5f1-114">Jednak w ramach tych implementacji obowiązują ograniczenia środowiska uruchomieniowego, które mogą mieć wpływ na działanie EF Core w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-114">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="1c5f1-115">Aby uzyskać więcej informacji, zobacz [implementacje platformy .NET obsługiwane przez EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-115">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="1c5f1-116">Na koniec różni dostawcy baz danych mogą wymagać określonych wersji aparatu bazy danych, implementacji platformy .NET lub systemów operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-116">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="1c5f1-117">Upewnij się, że [dostawca bazy danych EF Core](xref:core/providers/index) jest dostępny, który obsługuje odpowiednie środowisko dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-117">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="1c5f1-118">Pobierz środowisko uruchomieniowe Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-118">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="1c5f1-119">Aby dodać EF Core do aplikacji, zainstaluj pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-119">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="1c5f1-120">W przypadku kompilowania aplikacji ASP.NET Core nie trzeba instalować dostawców znajdujących się w pamięci i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-120">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="1c5f1-121">Te dostawcy są dołączane do bieżących wersji ASP.NET Core wraz z EF Core środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-121">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="1c5f1-122">Aby zainstalować lub zaktualizować pakiety NuGet, można użyć interfejsu wiersza polecenia platformy .NET Core (CLI), okna dialogowego Menedżera pakietów programu Visual Studio lub konsoli Menedżera pakietów programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-122">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="1c5f1-123">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-123">.NET Core CLI</span></span>

* <span data-ttu-id="1c5f1-124">Użyj następującego polecenia interfejs wiersza polecenia platformy .NET Core w wierszu polecenia systemu operacyjnego, aby zainstalować lub zaktualizować dostawcę EF Core SQL Server:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-124">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="1c5f1-125">Możesz wskazać określoną wersję w poleceniu `dotnet add package`, używając modyfikatora `-v`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-125">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="1c5f1-126">Aby na przykład zainstalować EF Core pakiety 2.2.0, Dołącz `-v 2.2.0` do polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-126">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="1c5f1-127">Aby uzyskać więcej informacji, zobacz [Narzędzia interfejsu wiersza polecenia (CLI) platformy .NET](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="1c5f1-128">Okno dialogowe Menedżera pakietów NuGet programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c5f1-128">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="1c5f1-129">Z menu programu Visual Studio wybierz pozycję **projekt > Zarządzaj pakietami NuGet** .</span><span class="sxs-lookup"><span data-stu-id="1c5f1-129">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="1c5f1-130">Kliknij kartę **Przeglądaj** lub **aktualizacje**</span><span class="sxs-lookup"><span data-stu-id="1c5f1-130">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="1c5f1-131">Aby zainstalować lub zaktualizować dostawcę SQL Server, wybierz pakiet `Microsoft.EntityFrameworkCore.SqlServer` i potwierdź.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-131">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="1c5f1-132">Aby uzyskać więcej informacji, zobacz [okno dialogowe Menedżera pakietów NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-132">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="1c5f1-133">Konsola Menedżera pakietów NuGet programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c5f1-133">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="1c5f1-134">Z menu programu Visual Studio wybierz kolejno pozycje **narzędzia > Menedżer pakietów NuGet > konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="1c5f1-134">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="1c5f1-135">Aby zainstalować dostawcę SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-135">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="1c5f1-136">Aby zaktualizować dostawcę, użyj polecenia `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-136">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="1c5f1-137">Aby określić określoną wersję, użyj modyfikatora `-Version`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-137">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="1c5f1-138">Na przykład aby zainstalować EF Core pakiety 2.2.0, Dołącz `-Version 2.2.0` do poleceń</span><span class="sxs-lookup"><span data-stu-id="1c5f1-138">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="1c5f1-139">Aby uzyskać więcej informacji, zobacz [konsola Menedżera pakietów](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-139">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="1c5f1-140">Pobierz narzędzia Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-140">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="1c5f1-141">Możesz zainstalować narzędzia do wykonywania zadań związanych z EF Core w projekcie, takich jak tworzenie i stosowanie migracji baz danych lub tworzenie modelu EF Core na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-141">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="1c5f1-142">Dostępne są dwa zestawy narzędzi:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-142">Two sets of tools are available:</span></span>

* <span data-ttu-id="1c5f1-143">[Narzędzia interfejsu wiersza polecenia platformy .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) mogą być używane w systemie Windows, Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-143">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="1c5f1-144">Te polecenia zaczynają się od `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-144">These commands begin with `dotnet ef`.</span></span>

* <span data-ttu-id="1c5f1-145">[Narzędzia konsoli Menedżera pakietów (PMC)](xref:core/miscellaneous/cli/powershell) działają w programie Visual Studio w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-145">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="1c5f1-146">Te polecenia zaczynają się od zlecenia, na przykład `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-146">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="1c5f1-147">Mimo że można również użyć poleceń `dotnet ef` z konsoli Menedżera pakietów, zaleca się korzystanie z narzędzi konsoli Menedżera pakietów w przypadku korzystania z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-147">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="1c5f1-148">Automatycznie pracują z bieżącym projektem wybranym w ramach dyrektywy PMC w programie Visual Studio, bez konieczności ręcznego przełączania katalogów.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-148">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="1c5f1-149">Po zakończeniu tego polecenia automatycznie otwierane są pliki wygenerowane przez polecenia w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-149">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="1c5f1-150">Pobierz narzędzia interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-150">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="1c5f1-151">Narzędzia interfejs wiersza polecenia platformy .NET Core wymagają zestaw .NET Core SDK, wymienione wcześniej w [wymaganiach wstępnych](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="1c5f1-151">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="1c5f1-152">Polecenia `dotnet ef` są zawarte w bieżących wersjach zestaw .NET Core SDK, ale aby włączyć polecenia w określonym projekcie, należy zainstalować pakiet `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-152">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> <span data-ttu-id="1c5f1-153">Zawsze używaj wersji pakietu narzędzi, która jest zgodna z wersją główną pakietów środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-153">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="1c5f1-154">Pobierz narzędzia konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="1c5f1-154">Get the Package Manager Console tools</span></span>

<span data-ttu-id="1c5f1-155">Aby uzyskać narzędzia konsoli Menedżera pakietów dla EF Core, zainstaluj pakiet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-155">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="1c5f1-156">Na przykład w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-156">For example, from Visual Studio:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="1c5f1-157">W przypadku aplikacji ASP.NET Core ten pakiet jest uwzględniany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-157">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="1c5f1-158">Uaktualnianie do najnowszej EF Core</span><span class="sxs-lookup"><span data-stu-id="1c5f1-158">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="1c5f1-159">Za każdym razem, gdy udostępnimy nową wersję EF Core, wybraliśmy również nową wersję dostawców, którzy są częścią projektu EF Core, takich jak Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. SQLite, i Microsoft. EntityFrameworkCore. inMemory.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-159">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="1c5f1-160">Po prostu możesz przeprowadzić uaktualnienie do nowej wersji dostawcy, aby uzyskać wszystkie ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-160">You can just upgrade to the new version of the provider to get all the improvements.</span></span>

* <span data-ttu-id="1c5f1-161">EF Core, wraz z SQL Server i dostawcami znajdującymi się w pamięci, są dołączane do bieżących wersji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-161">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="1c5f1-162">Aby uaktualnić istniejącą aplikację ASP.NET Core do nowszej wersji EF Core, należy zawsze uaktualnić wersję programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-162">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="1c5f1-163">Jeśli trzeba zaktualizować aplikację, która korzysta z dostawcy bazy danych innej firmy, należy zawsze sprawdzić, czy jest to aktualizacja dostawcy, która jest zgodna z wersją EF Core, która ma być używana.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-163">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="1c5f1-164">Na przykład dostawcy bazy danych dla poprzednich wersji nie są zgodni z wersją 2,0 środowiska uruchomieniowego EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-164">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="1c5f1-165">Dostawcy innych firm dla EF Core zwykle nie wydawania wersji poprawek wraz z EF Core środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-165">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="1c5f1-166">Aby uaktualnić aplikację, która używa dostawcy innej firmy do wersji poprawki EF Core, może być konieczne dodanie bezpośredniego odwołania do poszczególnych składników środowiska uruchomieniowego EF Core, takich jak Microsoft. EntityFrameworkCore i Microsoft. EntityFrameworkCore. relacyjny.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-166">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="1c5f1-167">W przypadku uaktualniania istniejącej aplikacji do najnowszej wersji EF Core niektóre odwołania do starszych pakietów EF Core mogą wymagać usunięcia ręcznie:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-167">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="1c5f1-168">Pakiety czasu projektowania dostawcy bazy danych, takie jak `Microsoft.EntityFrameworkCore.SqlServer.Design`, nie są już wymagane ani obsługiwane przez program EF Core 2,0 i nowsze, ale nie są automatycznie usuwane podczas uaktualniania innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="1c5f1-168">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="1c5f1-169">Narzędzia interfejsu wiersza polecenia platformy .NET są dołączone do zestawu .NET SDK od wersji 2,1, więc odwołanie do tego pakietu można usunąć z pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="1c5f1-169">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
