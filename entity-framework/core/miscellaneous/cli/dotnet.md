---
title: .NET core interfejsu wiersza polecenia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995300"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a02a7-102">Narzędzia wiersza polecenia programu EF Core platformy .NET</span><span class="sxs-lookup"><span data-stu-id="a02a7-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a02a7-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET to rozszerzenie dla wielu platform **dotnet** polecenia, który jest częścią programu [zestawu .NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="a02a7-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a02a7-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.</span><span class="sxs-lookup"><span data-stu-id="a02a7-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a02a7-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="a02a7-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="a02a7-106">.NET Core SDK w wersji 2.1.300 oraz nowszych **dotnet ef** poleceń, które są zgodne z programem EF Core 2.0 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="a02a7-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="a02a7-107">W związku z tym jeśli używane są nowe wersje programu .NET Core SDK i środowiska uruchomieniowego EF Core, instalacja nie jest wymagana, i można zignorować pozostałej części tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="a02a7-108">Z drugiej strony **dotnet ef** narzędzie zawarte w wersji zestawu .NET Core SDK 2.1.300 i nowszych nie jest zgodny z wersją programu EF Core 1.0 i 1.1.</span><span class="sxs-lookup"><span data-stu-id="a02a7-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="a02a7-109">Zanim można pracować z projektu, który używa tych starszych wersji programu EF Core na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania tej starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku.</span><span class="sxs-lookup"><span data-stu-id="a02a7-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="a02a7-110">Ten plik zwykle znajduje się w katalogu rozwiązania (po jednym powyżej projektu).</span><span class="sxs-lookup"><span data-stu-id="a02a7-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="a02a7-111">Następnie możesz przejść za pomocą instrukcji na temat poniżej.</span><span class="sxs-lookup"><span data-stu-id="a02a7-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="a02a7-112">We wcześniejszych wersjach programu .NET Core SDK można zainstalować narzędzia wiersza polecenia platformy .NET Core EF wykonując następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="a02a7-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a02a7-113">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="a02a7-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a02a7-114">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a02a7-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="a02a7-115">Projekt wynikowy powinien wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="a02a7-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="a02a7-116">Odwołania do pakietu przy użyciu `PrivateAssets="All"` oznacza, że nie jest widoczne projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="a02a7-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a02a7-117">Jeśli tak, nie wszystko bezpośrednio należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a02a7-118">Przy użyciu narzędzi</span><span class="sxs-lookup"><span data-stu-id="a02a7-118">Using the tools</span></span>
---------------
<span data-ttu-id="a02a7-119">Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="a02a7-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a02a7-120">Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="a02a7-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a02a7-121">Projekt docelowy domyślnie do projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **— projekt** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a02a7-122">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="a02a7-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a02a7-123">On również wartość domyślna to projekt w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="a02a7-124">Na przykład aktualizowanie bazy danych zawierającej programu EF Core zainstalowany w innym projekcie aplikacji sieci web będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)</span><span class="sxs-lookup"><span data-stu-id="a02a7-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="a02a7-125">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a02a7-126">--json</span><span class="sxs-lookup"><span data-stu-id="a02a7-126">--json</span></span>                           | <span data-ttu-id="a02a7-127">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="a02a7-127">Show JSON output.</span></span>           |
| <span data-ttu-id="a02a7-128">-c</span><span class="sxs-lookup"><span data-stu-id="a02a7-128">-c</span></span> | <span data-ttu-id="a02a7-129">--kontekstu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a02a7-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a02a7-130">Kontekst DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="a02a7-131">-p</span><span class="sxs-lookup"><span data-stu-id="a02a7-131">-p</span></span> | <span data-ttu-id="a02a7-132">--projektu \<projektu ></span><span class="sxs-lookup"><span data-stu-id="a02a7-132">--project \<PROJECT></span></span>             | <span data-ttu-id="a02a7-133">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-133">The project to use.</span></span>         |
| <span data-ttu-id="a02a7-134">-s</span><span class="sxs-lookup"><span data-stu-id="a02a7-134">-s</span></span> | <span data-ttu-id="a02a7-135">--Projekt startowy \<projektu ></span><span class="sxs-lookup"><span data-stu-id="a02a7-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a02a7-136">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="a02a7-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="a02a7-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a02a7-138">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="a02a7-138">The target framework.</span></span>       |
|    | <span data-ttu-id="a02a7-139">— Konfiguracja \<konfiguracji ></span><span class="sxs-lookup"><span data-stu-id="a02a7-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a02a7-140">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a02a7-141">środowisko uruchomieniowe — \<identyfikator ></span><span class="sxs-lookup"><span data-stu-id="a02a7-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a02a7-142">Środowisko uruchomieniowe do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-142">The runtime to use.</span></span>         |
| <span data-ttu-id="a02a7-143">-h</span><span class="sxs-lookup"><span data-stu-id="a02a7-143">-h</span></span> | <span data-ttu-id="a02a7-144">--help</span><span class="sxs-lookup"><span data-stu-id="a02a7-144">--help</span></span>                           | <span data-ttu-id="a02a7-145">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="a02a7-145">Show help information.</span></span>      |
| <span data-ttu-id="a02a7-146">-v</span><span class="sxs-lookup"><span data-stu-id="a02a7-146">-v</span></span> | <span data-ttu-id="a02a7-147">— pełne</span><span class="sxs-lookup"><span data-stu-id="a02a7-147">--verbose</span></span>                        | <span data-ttu-id="a02a7-148">Wyświetlić pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="a02a7-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a02a7-149">--nie-color</span><span class="sxs-lookup"><span data-stu-id="a02a7-149">--no-color</span></span>                       | <span data-ttu-id="a02a7-150">Nie kolorować danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a02a7-151">--dane wyjściowe prefiksu</span><span class="sxs-lookup"><span data-stu-id="a02a7-151">--prefix-output</span></span>                  | <span data-ttu-id="a02a7-152">Prefiks danych wyjściowych z poziomu.</span><span class="sxs-lookup"><span data-stu-id="a02a7-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a02a7-153">Aby określić środowiska ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="a02a7-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a02a7-154">Polecenia</span><span class="sxs-lookup"><span data-stu-id="a02a7-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a02a7-155">docelowej bazie danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-155">dotnet ef database drop</span></span>

<span data-ttu-id="a02a7-156">Porzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-156">Drops the database.</span></span>

<span data-ttu-id="a02a7-157">Opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a02a7-158">-f</span><span class="sxs-lookup"><span data-stu-id="a02a7-158">-f</span></span> | <span data-ttu-id="a02a7-159">--force</span><span class="sxs-lookup"><span data-stu-id="a02a7-159">--force</span></span>   | <span data-ttu-id="a02a7-160">Nie Potwierdź.</span><span class="sxs-lookup"><span data-stu-id="a02a7-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a02a7-161">--uruchomienia próbnego</span><span class="sxs-lookup"><span data-stu-id="a02a7-161">--dry-run</span></span> | <span data-ttu-id="a02a7-162">Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej.</span><span class="sxs-lookup"><span data-stu-id="a02a7-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a02a7-163">Aktualizacja bazy danych programu ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-163">dotnet ef database update</span></span>

<span data-ttu-id="a02a7-164">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a02a7-165">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="a02a7-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a02a7-166">\<MIGRACJA &GT;</span><span class="sxs-lookup"><span data-stu-id="a02a7-166">\<MIGRATION></span></span> | <span data-ttu-id="a02a7-167">Migracja docelowego.</span><span class="sxs-lookup"><span data-stu-id="a02a7-167">The target migration.</span></span> <span data-ttu-id="a02a7-168">Jeśli jest to 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a02a7-169">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a02a7-170">informacje kontekstu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a02a7-171">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="a02a7-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a02a7-172">Lista typu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a02a7-173">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="a02a7-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a02a7-174">Tworzenie szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a02a7-175">Szkielety mechanizmów DbContext i jednostek typów dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a02a7-176">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="a02a7-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="a02a7-177">\<POŁĄCZENIA &GT;</span><span class="sxs-lookup"><span data-stu-id="a02a7-177">\<CONNECTION></span></span> | <span data-ttu-id="a02a7-178">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="a02a7-179">\<DOSTAWCA &GT;</span><span class="sxs-lookup"><span data-stu-id="a02a7-179">\<PROVIDER></span></span>   | <span data-ttu-id="a02a7-180">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-180">The provider to use.</span></span> <span data-ttu-id="a02a7-181">(na przykład Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a02a7-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a02a7-182">Opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a02a7-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a02a7-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="a02a7-184">--adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="a02a7-184">--data-annotations</span></span>                      | <span data-ttu-id="a02a7-185">Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a02a7-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a02a7-186">W przypadku pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="a02a7-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a02a7-187">-c</span><span class="sxs-lookup"><span data-stu-id="a02a7-187">-c</span></span>              | <span data-ttu-id="a02a7-188">--kontekstu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="a02a7-188">--context \<NAME></span></span>                       | <span data-ttu-id="a02a7-189">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="a02a7-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="a02a7-190">-dir kontekstu \<ŚCIEŻKA ></span><span class="sxs-lookup"><span data-stu-id="a02a7-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="a02a7-191">Katalog, który można umieścić plik typu DbContext w.</span><span class="sxs-lookup"><span data-stu-id="a02a7-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="a02a7-192">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a02a7-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a02a7-193">-f</span><span class="sxs-lookup"><span data-stu-id="a02a7-193">-f</span></span>              | <span data-ttu-id="a02a7-194">--force</span><span class="sxs-lookup"><span data-stu-id="a02a7-194">--force</span></span>                                 | <span data-ttu-id="a02a7-195">Nadpisz istniejące pliki.</span><span class="sxs-lookup"><span data-stu-id="a02a7-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a02a7-196">-o</span><span class="sxs-lookup"><span data-stu-id="a02a7-196">-o</span></span>              | <span data-ttu-id="a02a7-197">--dane wyjściowe dir \<ŚCIEŻKA ></span><span class="sxs-lookup"><span data-stu-id="a02a7-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a02a7-198">Katalog, który można umieścić pliki w.</span><span class="sxs-lookup"><span data-stu-id="a02a7-198">The directory to put files in.</span></span> <span data-ttu-id="a02a7-199">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a02a7-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a02a7-200"><nobr>--schematu \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="a02a7-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a02a7-201">Schematów tabel w celu wygenerowania typów jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="a02a7-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a02a7-202">-t</span><span class="sxs-lookup"><span data-stu-id="a02a7-202">-t</span></span>              | <span data-ttu-id="a02a7-203">--tabeli \<nazwa_tabeli >...</span><span class="sxs-lookup"><span data-stu-id="a02a7-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a02a7-204">Tabele, aby wygenerować typy jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="a02a7-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a02a7-205">— nazwy w przypadku baz danych użycia</span><span class="sxs-lookup"><span data-stu-id="a02a7-205">--use-database-names</span></span>                    | <span data-ttu-id="a02a7-206">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a02a7-207">Dodaj migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-207">dotnet ef migrations add</span></span>

<span data-ttu-id="a02a7-208">Dodaje nową migrację.</span><span class="sxs-lookup"><span data-stu-id="a02a7-208">Adds a new migration.</span></span>

<span data-ttu-id="a02a7-209">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="a02a7-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a02a7-210">\<NAME></span><span class="sxs-lookup"><span data-stu-id="a02a7-210">\<NAME></span></span> | <span data-ttu-id="a02a7-211">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-211">The name of the migration.</span></span> |

<span data-ttu-id="a02a7-212">Opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a02a7-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a02a7-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="a02a7-214"><nobr>--dane wyjściowe dir \<ŚCIEŻKA ></nobr></span><span class="sxs-lookup"><span data-stu-id="a02a7-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a02a7-215">Katalog (i podrzędnej przestrzeni nazw) do użycia.</span><span class="sxs-lookup"><span data-stu-id="a02a7-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a02a7-216">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a02a7-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="a02a7-217">Wartość domyślna to "Migracja".</span><span class="sxs-lookup"><span data-stu-id="a02a7-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a02a7-218">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-218">dotnet ef migrations list</span></span>

<span data-ttu-id="a02a7-219">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a02a7-220">Usuń migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="a02a7-221">Usuwa ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-221">Removes the last migration.</span></span>

<span data-ttu-id="a02a7-222">Opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a02a7-223">-f</span><span class="sxs-lookup"><span data-stu-id="a02a7-223">-f</span></span> | <span data-ttu-id="a02a7-224">--force</span><span class="sxs-lookup"><span data-stu-id="a02a7-224">--force</span></span> | <span data-ttu-id="a02a7-225">Przywróć migracji, jeśli został zastosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a02a7-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a02a7-226">skrypt migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a02a7-226">dotnet ef migrations script</span></span>

<span data-ttu-id="a02a7-227">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a02a7-228">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="a02a7-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a02a7-229">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="a02a7-229">\<FROM></span></span> | <span data-ttu-id="a02a7-230">Począwszy od migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-230">The starting migration.</span></span> <span data-ttu-id="a02a7-231">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="a02a7-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a02a7-232">\<ABY &GT;</span><span class="sxs-lookup"><span data-stu-id="a02a7-232">\<TO></span></span>   | <span data-ttu-id="a02a7-233">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-233">The ending migration.</span></span> <span data-ttu-id="a02a7-234">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a02a7-235">Opcje:</span><span class="sxs-lookup"><span data-stu-id="a02a7-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a02a7-236">-o</span><span class="sxs-lookup"><span data-stu-id="a02a7-236">-o</span></span> | <span data-ttu-id="a02a7-237">--dane wyjściowe \<pliku ></span><span class="sxs-lookup"><span data-stu-id="a02a7-237">--output \<FILE></span></span> | <span data-ttu-id="a02a7-238">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="a02a7-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a02a7-239">-i</span><span class="sxs-lookup"><span data-stu-id="a02a7-239">-i</span></span> | <span data-ttu-id="a02a7-240">--idempotentne</span><span class="sxs-lookup"><span data-stu-id="a02a7-240">--idempotent</span></span>     | <span data-ttu-id="a02a7-241">Generowanie skryptu, który może służyć w bazie danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="a02a7-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
