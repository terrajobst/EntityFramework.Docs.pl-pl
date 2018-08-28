---
title: Instalowanie programu EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996931"
---
# <a name="installing-ef-core"></a><span data-ttu-id="ba224-102">Instalowanie programu EF Core</span><span class="sxs-lookup"><span data-stu-id="ba224-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba224-103">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ba224-103">Prerequisites</span></span>

<span data-ttu-id="ba224-104">W celu opracowywania aplikacji platformy .NET Core 2.1 (w tym aplikacji platformy ASP.NET Core 2.1, przeznaczonych dla platformy .NET Core), należy pobrać i zainstalować wersję [zestawu SDK programu .NET Core 2.1](https://www.microsoft.com/net/download/core) odpowiednie dla danej platformy.</span><span class="sxs-lookup"><span data-stu-id="ba224-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="ba224-105">**Ta zasada obowiązuje, nawet jeśli zainstalowano program Visual Studio 2017 w wersji 15.7.**</span><span class="sxs-lookup"><span data-stu-id="ba224-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="ba224-106">Aby można było używać programu EF Core 2.1 lub inne biblioteki .NET Standard 2.0 przy użyciu platformy .NET poza platformy .NET Core 2.1 (na przykład za pomocą programu .NET Framework 4.6.1 lub nowszej) potrzebna jest wersja pakietu nuget, rozpoznający .NET Standard 2.0 i jego struktury zgodne.</span><span class="sxs-lookup"><span data-stu-id="ba224-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="ba224-107">Oto kilka sposobów, można uzyskać, to:</span><span class="sxs-lookup"><span data-stu-id="ba224-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="ba224-108">Instalowanie programu Visual Studio 2017 w wersji 15.7</span><span class="sxs-lookup"><span data-stu-id="ba224-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="ba224-109">Jeśli używasz programu Visual Studio 2015 [pobierania i uaktualnić klienta programu NuGet do wersji 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="ba224-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="ba224-110">Projekty utworzone we wcześniejszych wersjach programu Visual Studio i przeznaczonych dla platformy .NET Framework może być konieczne dodatkowe zmiany, aby był zgodny z biblioteki .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="ba224-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="ba224-111">Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="ba224-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="ba224-112">Dla projektów testowych również upewnij się, że występuje następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="ba224-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="ba224-113">Pobieranie usługi bits</span><span class="sxs-lookup"><span data-stu-id="ba224-113">Getting the bits</span></span>
<span data-ttu-id="ba224-114">Zalecanym sposobem dodawania bibliotek środowiska uruchomieniowego EF Core w aplikacji jest również zainstalować dostawcy bazy danych programu EF Core z pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="ba224-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="ba224-115">Oprócz bibliotek środowiska uruchomieniowego można zainstalować narzędzia, które ułatwiają wykonać kilka zadań związanych z programem EF Core w projekcie, w czasie projektowania, takie jak tworzenie i stosowanie migracje i tworzenie modelu, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ba224-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="ba224-116">Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="ba224-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="ba224-117">Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z środowiska uruchomieniowego EF Core w wersji 2.1.</span><span class="sxs-lookup"><span data-stu-id="ba224-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="ba224-118">Aplikacje przeznaczone na platformy ASP.NET Core 2.1 można użyć programu EF Core 2.1 bez dodatkowe zależności oprócz dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba224-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="ba224-119">Aplikacje przeznaczone na poprzednie wersje platformy ASP.NET Core konieczne uaktualnienie do platformy ASP.NET Core 2.1, aby można było używać programu EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ba224-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="ba224-120">Programowanie dla wielu platform przy użyciu konsoli .NET Core interfejsu wiersza polecenia (CLI)</span><span class="sxs-lookup"><span data-stu-id="ba224-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="ba224-121">Do tworzenia aplikacji, których platformą docelową [platformy .NET Core](https://www.microsoft.com/net/download/core) możesz użyć [ `dotnet` poleceń interfejsu wiersza polecenia](https://docs.microsoft.com/dotnet/core/tools/) w połączeniu z ulubionego edytora tekstu lub tworzenia środowiska IDE (Integrated) takie Program Visual Studio, Visual Studio for Mac lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ba224-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ba224-122">Aplikacji przeznaczonych dla platformy .NET Core wymagają określonych wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba224-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="ba224-123">Na przykład .NET Core 1.x rozwoju wymaga programu Visual Studio 2017, podczas gdy rozwój platformy .NET Core 2.1 wymaga programu Visual Studio 2017 w wersji 15.7.</span><span class="sxs-lookup"><span data-stu-id="ba224-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="ba224-124">Aby zainstalować lub uaktualnić dostawcę programu SQL Server w aplikacji platformy .NET Core dla wielu platform, przejdź do katalogu aplikacji, a następnie uruchom następujące polecenie w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="ba224-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="ba224-125">Można wskazać instalowanie określonej wersji, w `dotnet add package` polecenia przy użyciu `-v` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ba224-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="ba224-126">Na przykład, aby zainstalować pakiety programu EF Core 2.1, należy dołączyć `-v 2.1.0` do polecenia.</span><span class="sxs-lookup"><span data-stu-id="ba224-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="ba224-127">EF Core obejmuje zestaw [dodatkowe polecenia do `dotnet` interfejsu wiersza polecenia](../../miscellaneous/cli/dotnet.md), począwszy od `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="ba224-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="ba224-128">Narzędzia wiersza polecenia platformy .NET Core dla platformy EF Core wymagają pakiet o nazwie `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="ba224-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="ba224-129">Można go dodać do projektu przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="ba224-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="ba224-130">Zawsze używaj wersji zgodnej wersji głównej pakiety środowiska uruchomieniowego pakietu Narzędzia.</span><span class="sxs-lookup"><span data-stu-id="ba224-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="ba224-131">Programowanie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba224-131">Visual Studio development</span></span>

<span data-ttu-id="ba224-132">Możesz tworzyć wiele różnych typów aplikacji, których platformą docelową .NET Core, .NET Framework lub na innych platformach obsługiwanych przez platformę EF Core przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba224-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="ba224-133">Istnieją dwa sposoby, można zainstalować dostawcy bazy danych programu EF Core w aplikacji z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ba224-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="ba224-134">Za pomocą NuGet [interfejsu użytkownika Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="ba224-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="ba224-135">Wybierz z menu **Projekt > Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="ba224-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="ba224-136">Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę</span><span class="sxs-lookup"><span data-stu-id="ba224-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="ba224-137">Wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i wymaganej wersji i upewnij się,</span><span class="sxs-lookup"><span data-stu-id="ba224-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="ba224-138">Za pomocą NuGet [Konsola Menedżera pakietów (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="ba224-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="ba224-139">Wybierz z menu **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="ba224-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ba224-140">Wpisz i uruchom następujące polecenie w konsoli zarządzania Pakietami:</span><span class="sxs-lookup"><span data-stu-id="ba224-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="ba224-141">Możesz użyć `Update-Package` zamiast tego polecenia, aby zaktualizować pakiet, który jest już zainstalowany na nowszą wersję</span><span class="sxs-lookup"><span data-stu-id="ba224-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="ba224-142">Aby określić określonej wersji, można użyć `-Version` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ba224-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="ba224-143">Na przykład, aby zainstalować pakiety programu EF Core 2.1, należy dołączyć `-Version 2.1.0` poleceń</span><span class="sxs-lookup"><span data-stu-id="ba224-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="ba224-144">Narzędzia</span><span class="sxs-lookup"><span data-stu-id="ba224-144">Tools</span></span>

<span data-ttu-id="ba224-145">Jest również wersja programu PowerShell z [programu EF Core polecenia uruchomienia, które w konsoli zarządzania Pakietami](../../miscellaneous/cli/powershell.md) w programie Visual Studio, podobne możliwości `dotnet ef` poleceń.</span><span class="sxs-lookup"><span data-stu-id="ba224-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="ba224-146">Chociaż istnieje możliwość użycia `dotnet ef` polecenia z konsoli zarządzania Pakietami w programie Visual Studio, jest to o wiele bardziej wygodne do korzystania z wersji programu PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ba224-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="ba224-147">Działają automatycznie z bieżącego projektu wybrany w konsoli zarządzania Pakietami, bez konieczności ręcznie zmienić katalog.</span><span class="sxs-lookup"><span data-stu-id="ba224-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="ba224-148">One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.</span><span class="sxs-lookup"><span data-stu-id="ba224-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ba224-149">**Przestarzałe pakiety w programie EF Core 2.1:** Jeśli aktualizujesz istniejącą aplikację platformy EF Core 2.1 niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie:</span><span class="sxs-lookup"><span data-stu-id="ba224-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="ba224-150">Bazy danych dostawcy projektowania pakietów, takich jak `Microsoft.EntityFrameworkCore.SqlServer.Design` nie jest już wymagane lub obsługiwanych w programie EF Core 2.1, ale nie zostaną automatycznie usunięte po uaktualnieniu innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="ba224-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="ba224-151">Narzędzia wiersza polecenia platformy .NET zawiera teraz zestaw .NET SDK, odwołanie do tego pakietu może zostać usunięty z *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="ba224-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
