---
title: Instalowanie EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054647"
---
# <a name="installing-ef-core"></a><span data-ttu-id="6fd5b-102">Instalowanie EF Core</span><span class="sxs-lookup"><span data-stu-id="6fd5b-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fd5b-103">Wstępnie wymagane składniki</span><span class="sxs-lookup"><span data-stu-id="6fd5b-103">Prerequisites</span></span>

<span data-ttu-id="6fd5b-104">Aby tworzyć aplikacje .NET Core 2.0 (w tym aplikacje ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core), należy pobrać i zainstalować wersję [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) odpowiednie dla danej platformy.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="6fd5b-105">**Dotyczy to nawet po zainstalowaniu programu Visual Studio 2017 wersji 15 ustęp 3.**</span><span class="sxs-lookup"><span data-stu-id="6fd5b-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="6fd5b-106">Aby można było używać EF Core w wersji 2.0 lub inne biblioteki .NET 2.0 standardowe z platformami .NET, oprócz .NET Core 2.0 (np. z platformy .NET Framework 4.6.1 lub nowszej) będzie mieć wersję programu NuGet, który zna .NET 2.0 standardowe i jego struktury zgodne.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="6fd5b-107">Poniżej przedstawiono kilka sposobów, możesz uzyskać, to:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="6fd5b-108">Zainstaluj program Visual Studio 2017 wersji 15 ustęp 3</span><span class="sxs-lookup"><span data-stu-id="6fd5b-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="6fd5b-109">Jeśli używasz programu Visual Studio 2015, [pobierania i uaktualnić klienta NuGet w wersji 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="6fd5b-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="6fd5b-110">Projektów utworzonych w poprzednich wersjach programu Visual Studio i przeznaczonych dla platformy .NET Framework może być konieczne dodatkowe zmiany, aby był zgodny z bibliotekami .NET 2.0 standardowe:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="6fd5b-111">Edytuj plik projektu i upewnij się, że w grupie właściwości początkowej pojawia się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="6fd5b-112">Dla projektów testów upewnij się, że występuje następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="6fd5b-113">Pobieranie usługi bits</span><span class="sxs-lookup"><span data-stu-id="6fd5b-113">Getting the bits</span></span>
<span data-ttu-id="6fd5b-114">Zalecanym sposobem Dodawanie bibliotek środowiska uruchomieniowego EF Core do aplikacji jest do zainstalowania dostawcy bazy danych programu EF Core z NuGet.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="6fd5b-115">Oprócz bibliotek środowiska uruchomieniowego należy zainstalować narzędzia, które ułatwiają wykonać kilka zadań związanych z EF Core w projekcie w czasie projektowania, takie jak tworzenie i stosowanie migracji i tworzenia modelu na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="6fd5b-116">Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych z innych firm, zawsze Wyszukaj aktualizację dostawcy, który jest zgodny z wersją Core EF, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="6fd5b-117">Np.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-117">E.g.</span></span> <span data-ttu-id="6fd5b-118">Baza danych dostawców w poprzednich wersjach nie są zgodne z EF podstawowego środowiska wykonawczego w wersji 2.0.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="6fd5b-119">Aplikacji przeznaczonych dla platformy ASP.NET Core 2.0 służy EF Core 2.0 bez dodatkowe zależności oprócz dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="6fd5b-120">Aplikacji na poprzednie wersje platformy ASP.NET Core trzeba uaktualnić do programu ASP.NET 2.0 Core aby można było używać EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="6fd5b-121">Programowanie wieloplatformowych przy użyciu platformy .NET Core interfejsu wiersza polecenia (CLI)</span><span class="sxs-lookup"><span data-stu-id="6fd5b-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="6fd5b-122">Do tworzenia aplikacji przeznaczonych [.NET Core](https://www.microsoft.com/net/download/core) mogą być używane [ `dotnet` polecenia interfejsu wiersza polecenia](https://docs.microsoft.com/dotnet/core/tools/) w połączeniu z edytora tekstu lub programowanie środowiska IDE (Integrated) takie Program Visual Studio, programu Visual Studio for Mac lub kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="6fd5b-123">Aplikacji przeznaczonych dla platformy .NET Core wymagają określonych wersji programu Visual Studio, np. .NET Core 1.x programowanie wymaga programu Visual Studio 2017 r, podczas programowania .NET Core 2.0 wymaga programu Visual Studio 2017 wersji 15 ustęp 3.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="6fd5b-124">Aby zainstalować lub uaktualnić dostawcę usługi programu SQL Server w aplikacji platformy .NET Core i platform, przełącz się do katalogu aplikacji i uruchom następujące polecenie w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="6fd5b-125">Można wskazać, instalowanie określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="6fd5b-126">Np.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-126">E.g.</span></span> <span data-ttu-id="6fd5b-127">Aby zainstalować pakiety EF Core 2.0, dołącz `-v 2.0.0` do polecenia.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="6fd5b-128">Podstawowe EF zawiera zestaw [dodatkowych poleceń dla `dotnet` CLI](../../miscellaneous/cli/dotnet.md)począwszy `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="6fd5b-129">Aby można było używać `dotnet ef` polecenia interfejsu wiersza polecenia aplikacji `.csproj` plik musi zawierać następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="6fd5b-130">Narzędzia .NET Core CLI podstawowych EF również wymagać oddzielny pakiet o nazwie Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="6fd5b-131">Można po prostu Dodaj do projektu przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="6fd5b-132">Należy zawsze używać wersji pakiety narzędzia, które odpowiada wersji głównej pakietów środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="6fd5b-133">Tworzenia programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fd5b-133">Visual Studio development</span></span>

<span data-ttu-id="6fd5b-134">Możesz utworzyć wiele różnych typów aplikacji .NET Core obiektu docelowego, .NET Framework lub innych platform obsługiwanych przez EF Core za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="6fd5b-135">Istnieją dwa sposoby dostawcy bazy danych programu EF Core można zainstalować w aplikacji z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="6fd5b-136">Przy użyciu narzędzia NuGet [interfejsu użytkownika Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="6fd5b-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="6fd5b-137">Wybierz z menu **projektu > Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="6fd5b-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="6fd5b-138">Polecenie **Przeglądaj** lub **aktualizacje** kartę</span><span class="sxs-lookup"><span data-stu-id="6fd5b-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="6fd5b-139">Wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i wymaganej wersji i upewnij się,</span><span class="sxs-lookup"><span data-stu-id="6fd5b-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="6fd5b-140">Przy użyciu narzędzia NuGet [Konsola Menedżera pakietów (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="6fd5b-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="6fd5b-141">Wybierz z menu **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="6fd5b-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="6fd5b-142">Wpisz i uruchom następujące polecenie w kryterium:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="6fd5b-143">Można użyć `Update-Package` polecenia do zaktualizowania pakietu, który jest już zainstalowana na nowszą wersję</span><span class="sxs-lookup"><span data-stu-id="6fd5b-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="6fd5b-144">Aby określić określonej wersji, można użyć `-Version` modyfikator, np. Aby zainstalować pakiety EF Core 2.0, dołącz `-Version 2.0.0` do poleceń</span><span class="sxs-lookup"><span data-stu-id="6fd5b-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="6fd5b-145">Narzędzia</span><span class="sxs-lookup"><span data-stu-id="6fd5b-145">Tools</span></span>

<span data-ttu-id="6fd5b-146">Jest także wersja środowiska PowerShell [EF Core poleceń Uruchom, które wewnątrz PMC](../../miscellaneous/cli/powershell.md) w programie Visual Studio i podobnych funkcji `dotnet ef` poleceń.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="6fd5b-147">Aby można było ich używać, zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu przy użyciu interfejsu użytkownika Menedżera pakietów lub PMC.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="6fd5b-148">Należy zawsze używać wersji pakiety narzędzia, które odpowiada wersji głównej pakietów środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="6fd5b-149">Chociaż można używać `dotnet ef` poleceń PMC w programie Visual Studio jest znacznie bardziej wygodne korzysta z wersji programu PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6fd5b-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="6fd5b-150">Automatycznie działają z bieżącego projektu wybranego w PMC bez konieczności ręcznego przełączania katalogów.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="6fd5b-151">Zostaną one automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="6fd5b-152">**Przestarzałe pakietów w programie EF Core 2.0:** uaktualniania istniejącej aplikacji do EF Core 2.0 niektórych odwołań do starszych pakietów EF Core może muszą zostać usunięte ręcznie.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="6fd5b-153">W szczególności, takie jak bazy danych dostawcy czasu projektowania pakietów `Microsoft.EntityFrameworkCore.SqlServer.Design` już wymagane ani obsługiwane w programie EF Core 2.0, ale nie zostaną automatycznie usunięte podczas uaktualniania z innymi pakietami.</span><span class="sxs-lookup"><span data-stu-id="6fd5b-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
