---
title: Instalowanie Entiy Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250325"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="6ff3a-102">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6ff3a-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ff3a-103">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6ff3a-103">Prerequisites</span></span>

* <span data-ttu-id="6ff3a-104">Do tworzenia aplikacji przeznaczonych dla platformy .NET Core 2.1, należy zainstalować [zestawu SDK programu .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6ff3a-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="6ff3a-105">Zestaw SDK został zainstalowany, nawet jeśli masz najnowszą wersję programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="6ff3a-106">Aby użyć programu Visual Studio do tworzenia aplikacji przeznaczonych dla platformy .NET Core 2.1, należy zainstalować Visual Studio 2017 w wersji 15.7 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="6ff3a-107">Aby używać programu Entity Framework 2.1 w aplikacji platformy ASP.NET Core, należy użyć platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="6ff3a-108">Aplikacje, które używają starszych wersji platformy ASP.NET Core, musi zostać zaktualizowany do wersji 2.1.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="6ff3a-109">Można użyć programu Visual Studio 2015 dla aplikacji przeznaczonych dla platformy .NET Framework 4.6.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="6ff3a-110">Jednak potrzebna jest wersja pakietu nuget, rozpoznający .NET Standard 2.0 i jego struktury zgodne.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="6ff3a-111">Aby uzyskać tę w programie Visual Studio 2015 [uaktualnić klienta programu NuGet w wersji 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="6ff3a-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="6ff3a-112">Pobierz środowisko uruchomieniowe programu Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6ff3a-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="6ff3a-113">Aby dodać biblioteki środowiska uruchomieniowego EF Core do aplikacji, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="6ff3a-114">Aby uzyskać listę obsługiwanych dostawców i ich nazwy pakietów NuGet, zobacz [bazy danych dostawcy](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6ff3a-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="6ff3a-115">Instalowanie lub aktualizowanie pakietów NuGet, przy użyciu interfejsu wiersza polecenia platformy .NET Core, okno dialogowe programu Visual Studio pakietu Menedżera lub konsoli Menedżera pakietów Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="6ff3a-116">W przypadku aplikacji platformy ASP.NET Core 2.1 w pamięci i dostawcy programu SQL Server są automatycznie dołączane, więc nie trzeba instalować ich osobno.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="6ff3a-117">Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="6ff3a-118">Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z środowiska uruchomieniowego EF Core w wersji 2.1.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="6ff3a-119">.NET core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6ff3a-119">.NET Core CLI</span></span>

<span data-ttu-id="6ff3a-120">Następujące polecenie interfejsu wiersza polecenia platformy .NET Core instaluje lub aktualizuje dostawcy programu SQL Server:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="6ff3a-121">Można wskazać określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="6ff3a-122">Na przykład, aby zainstalować pakiety programu EF Core 2.1.0, należy dołączyć `-v 2.1.0` do polecenia.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="6ff3a-123">Okno Menedżera pakietów NuGet programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ff3a-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="6ff3a-124">W menu, wybierz opcję **Projekt > Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="6ff3a-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="6ff3a-125">Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę</span><span class="sxs-lookup"><span data-stu-id="6ff3a-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="6ff3a-126">Aby zainstalować lub zaktualizować dostawcy programu SQL Server, wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i potwierdź.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="6ff3a-127">Aby uzyskać więcej informacji, zobacz [okno Menedżera pakietów NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="6ff3a-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="6ff3a-128">Konsola Menedżera pakietów NuGet dla programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ff3a-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="6ff3a-129">W menu, wybierz opcję **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="6ff3a-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="6ff3a-130">Aby zainstalować dostawcę programu SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="6ff3a-131">Aby zaktualizować dostawcę, użyj `Update-Package` polecenia.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="6ff3a-132">Aby określić określonej wersji, użyj `-Version` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="6ff3a-133">Na przykład, aby zainstalować pakiety programu EF Core 2.1.0, należy dołączyć `-Version 2.1.0` poleceń</span><span class="sxs-lookup"><span data-stu-id="6ff3a-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="6ff3a-134">Aby uzyskać więcej informacji, zobacz [Konsola Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6ff3a-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="6ff3a-135">Pobierz narzędzia platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6ff3a-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="6ff3a-136">Oprócz bibliotek środowiska uruchomieniowego można zainstalować narzędzia, które mogą wykonywać niektóre zadania związane z programem EF Core w projekcie w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="6ff3a-137">Na przykład można utworzyć migracje, zastosować migracje i utworzyć model, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="6ff3a-138">Dostępne są dwa zestawy narzędzi:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="6ff3a-139">.NET Core [narzędzi interfejsu wiersza polecenia (CLI)](../../miscellaneous/cli/dotnet.md) może służyć w Windows, Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="6ff3a-140">Te polecenia zaczynają się od `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="6ff3a-141">[Narzędzia Konsola Menedżera pakietów](../../miscellaneous/cli/powershell.md) systemem Windows w programie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="6ff3a-142">Te polecenia rozpoczynać czasownika, na przykład `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="6ff3a-143">Chociaż można używać `dotnet ef` poleceń z poziomu konsoli Menedżera pakietów jest bardziej wygodne do korzystania z narzędzi Konsola Menedżera pakietów, podczas korzystania z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="6ff3a-144">Działają automatycznie z bieżącego projektu wybrany w konsoli Menedżera pakietów, bez konieczności ręcznie zmienić katalog.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="6ff3a-145">One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="6ff3a-146">Pobierz narzędzia interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6ff3a-146">Get the CLI tools</span></span>

<span data-ttu-id="6ff3a-147">`dotnet ef` Poleceń są zawarte w zestawie SDK programu .NET Core, ale aby włączyć polecenia należy zainstalować `Microsoft.EntityFrameworkCore.Design` pakietu:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="6ff3a-148">W przypadku aplikacji platformy ASP.NET Core 2.1 Ten pakiet jest uwzględniane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="6ff3a-149">Jak wyjaśniono wcześniej w [wymagania wstępne](#prerequisites), należy także zainstalować zestaw SDK programu .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="6ff3a-150">Zawsze używaj wersji zgodnej wersji głównej pakiety środowiska uruchomieniowego pakietu Narzędzia.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="6ff3a-151">Pobierz narzędzia w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="6ff3a-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="6ff3a-152">Aby uzyskać Konsola Menedżera pakietów narzędzi dla platformy EF Core, należy zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="6ff3a-153">W przypadku aplikacji platformy ASP.NET Core 2.1 Ten pakiet jest uwzględniane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="6ff3a-154">Uaktualnianie do EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="6ff3a-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="6ff3a-155">Jeśli aktualizujesz istniejącą aplikację platformy EF Core 2.1 niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="6ff3a-156">Bazy danych dostawcy projektowania pakietów, takich jak `Microsoft.EntityFrameworkCore.SqlServer.Design` nie jest już wymagane lub obsługiwanych w programie EF Core 2.1, ale nie są automatycznie usuwane po uaktualnieniu innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="6ff3a-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="6ff3a-157">Narzędzia wiersza polecenia platformy .NET zawiera teraz zestaw .NET SDK, odwołanie do tego pakietu może zostać usunięty z *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="6ff3a-158">W przypadku aplikacji dla środowiska .NET Framework, które zostały utworzone we wcześniejszych wersjach programu Visual Studio upewnij się, że są one zgodne z bibliotekami .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="6ff3a-159">Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="6ff3a-160">Dla projektów testowych również upewnij się, że występuje następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="6ff3a-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
