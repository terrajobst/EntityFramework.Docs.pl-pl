---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754499"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="fc1f1-102">Narzędzia wiersza polecenia platformy .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="fc1f1-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="fc1f1-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="fc1f1-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="fc1f1-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="fc1f1-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="fc1f1-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="fc1f1-106">.NET Core SDK w wersji 2.1.300 i zawiera nowszą **dotnet ef** poleceń, które są zgodne z EF Core w wersji 2.0 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="fc1f1-107">Dlatego jeśli używane są nowe wersje zestawu SDK .NET Core i EF podstawowego środowiska wykonawczego, instalacja nie jest wymagana i pozostałej części tej sekcji można zignorować.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="fc1f1-108">Z drugiej strony **dotnet ef** narzędzie zawartych w wersji zestawu SDK programu .NET Core 2.1.300 i nowszych nie jest zgodny z wersją EF Core 1.0 i 1.1.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="fc1f1-109">Przed możesz pracować z projektu, który używa tych wersji EF rdzeni na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="fc1f1-110">Ten plik znajduje się zwykle w katalogu rozwiązania (jeden nad projektu).</span><span class="sxs-lookup"><span data-stu-id="fc1f1-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="fc1f1-111">Następnie można przejść z poniższych instrukcji na temat.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="fc1f1-112">W poprzednich wersjach zestawu SDK .NET Core można zainstalować narzędzi wiersza polecenia platformy .NET Core EF przy użyciu następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="fc1f1-113">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="fc1f1-114">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="fc1f1-115">Projekt wynikowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="fc1f1-116">Odwołanie pakietu z `PrivateAssets="All"` oznacza, że nie jest on ujawniony dla projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="fc1f1-117">Jeśli tak, nie wszystkie prawa należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="fc1f1-118">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="fc1f1-118">Using the tools</span></span>
---------------
<span data-ttu-id="fc1f1-119">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="fc1f1-120">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="fc1f1-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="fc1f1-121">Docelowy projekt domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **--projektu** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="fc1f1-122">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="fc1f1-123">On również domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="fc1f1-124">Na przykład uaktualnienie bazy danych aplikacji sieci web, który został zainstalowany w innym projekcie Core EF będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="fc1f1-125">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="fc1f1-126">--json</span><span class="sxs-lookup"><span data-stu-id="fc1f1-126">--json</span></span>                           | <span data-ttu-id="fc1f1-127">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-127">Show JSON output.</span></span>           |
| <span data-ttu-id="fc1f1-128">-c</span><span class="sxs-lookup"><span data-stu-id="fc1f1-128">-c</span></span> | <span data-ttu-id="fc1f1-129">--kontekstu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="fc1f1-130">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="fc1f1-131">-p</span><span class="sxs-lookup"><span data-stu-id="fc1f1-131">-p</span></span> | <span data-ttu-id="fc1f1-132">--projektu \<projektu ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-132">--project \<PROJECT></span></span>             | <span data-ttu-id="fc1f1-133">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-133">The project to use.</span></span>         |
| <span data-ttu-id="fc1f1-134">-s</span><span class="sxs-lookup"><span data-stu-id="fc1f1-134">-s</span></span> | <span data-ttu-id="fc1f1-135">--Projekt startowy \<projektu ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="fc1f1-136">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="fc1f1-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="fc1f1-138">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-138">The target framework.</span></span>       |
|    | <span data-ttu-id="fc1f1-139">--Konfiguracja \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="fc1f1-140">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="fc1f1-141">środowisko uruchomieniowe — \<identyfikator ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="fc1f1-142">Środowiska uruchomieniowego do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-142">The runtime to use.</span></span>         |
| <span data-ttu-id="fc1f1-143">-h</span><span class="sxs-lookup"><span data-stu-id="fc1f1-143">-h</span></span> | <span data-ttu-id="fc1f1-144">--help</span><span class="sxs-lookup"><span data-stu-id="fc1f1-144">--help</span></span>                           | <span data-ttu-id="fc1f1-145">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-145">Show help information.</span></span>      |
| <span data-ttu-id="fc1f1-146">-v</span><span class="sxs-lookup"><span data-stu-id="fc1f1-146">-v</span></span> | <span data-ttu-id="fc1f1-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="fc1f1-147">--verbose</span></span>                        | <span data-ttu-id="fc1f1-148">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="fc1f1-149">— Brak koloru</span><span class="sxs-lookup"><span data-stu-id="fc1f1-149">--no-color</span></span>                       | <span data-ttu-id="fc1f1-150">Nie kolorowanie danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="fc1f1-151">--dane wyjściowe prefiksu</span><span class="sxs-lookup"><span data-stu-id="fc1f1-151">--prefix-output</span></span>                  | <span data-ttu-id="fc1f1-152">Dane wyjściowe z poziomu prefiks.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="fc1f1-153">Aby w środowisku ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="fc1f1-154">Polecenia</span><span class="sxs-lookup"><span data-stu-id="fc1f1-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="fc1f1-155">Usuń bazę danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-155">dotnet ef database drop</span></span>

<span data-ttu-id="fc1f1-156">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-156">Drops the database.</span></span>

<span data-ttu-id="fc1f1-157">Opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="fc1f1-158">-f</span><span class="sxs-lookup"><span data-stu-id="fc1f1-158">-f</span></span> | <span data-ttu-id="fc1f1-159">--wymusić</span><span class="sxs-lookup"><span data-stu-id="fc1f1-159">--force</span></span>   | <span data-ttu-id="fc1f1-160">Nie potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="fc1f1-161">--przebiegu próbnego</span><span class="sxs-lookup"><span data-stu-id="fc1f1-161">--dry-run</span></span> | <span data-ttu-id="fc1f1-162">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="fc1f1-163">Aktualizacja bazy danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-163">dotnet ef database update</span></span>

<span data-ttu-id="fc1f1-164">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="fc1f1-165">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-166">\<MIGRACJA &GT;</span><span class="sxs-lookup"><span data-stu-id="fc1f1-166">\<MIGRATION></span></span> | <span data-ttu-id="fc1f1-167">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-167">The target migration.</span></span> <span data-ttu-id="fc1f1-168">Jeśli 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="fc1f1-169">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="fc1f1-170">informacje o dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="fc1f1-171">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="fc1f1-172">Lista dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="fc1f1-173">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="fc1f1-174">szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="fc1f1-175">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="fc1f1-176">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-177">\<POŁĄCZENIA &GT;</span><span class="sxs-lookup"><span data-stu-id="fc1f1-177">\<CONNECTION></span></span> | <span data-ttu-id="fc1f1-178">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="fc1f1-179">\<DOSTAWCA &GT;</span><span class="sxs-lookup"><span data-stu-id="fc1f1-179">\<PROVIDER></span></span>   | <span data-ttu-id="fc1f1-180">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-180">The provider to use.</span></span> <span data-ttu-id="fc1f1-181">(na przykład Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="fc1f1-182">Opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="fc1f1-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="fc1f1-184">--adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="fc1f1-184">--data-annotations</span></span>                      | <span data-ttu-id="fc1f1-185">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="fc1f1-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="fc1f1-186">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="fc1f1-187">-c</span><span class="sxs-lookup"><span data-stu-id="fc1f1-187">-c</span></span>              | <span data-ttu-id="fc1f1-188">--kontekstu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-188">--context \<NAME></span></span>                       | <span data-ttu-id="fc1f1-189">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="fc1f1-190">-dir kontekstu \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="fc1f1-191">Katalog mają zostać umieszczone w pliku DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="fc1f1-192">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="fc1f1-193">-f</span><span class="sxs-lookup"><span data-stu-id="fc1f1-193">-f</span></span>              | <span data-ttu-id="fc1f1-194">--wymusić</span><span class="sxs-lookup"><span data-stu-id="fc1f1-194">--force</span></span>                                 | <span data-ttu-id="fc1f1-195">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="fc1f1-196">-o</span><span class="sxs-lookup"><span data-stu-id="fc1f1-196">-o</span></span>              | <span data-ttu-id="fc1f1-197">--katalog wyjściowy \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="fc1f1-198">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-198">The directory to put files in.</span></span> <span data-ttu-id="fc1f1-199">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="fc1f1-200"><nobr>--schematu \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="fc1f1-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="fc1f1-201">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="fc1f1-202">-t</span><span class="sxs-lookup"><span data-stu-id="fc1f1-202">-t</span></span>              | <span data-ttu-id="fc1f1-203">--tabeli \<nazwa_tabeli >...</span><span class="sxs-lookup"><span data-stu-id="fc1f1-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="fc1f1-204">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="fc1f1-205">--nazwy w przypadku baz danych użycia</span><span class="sxs-lookup"><span data-stu-id="fc1f1-205">--use-database-names</span></span>                    | <span data-ttu-id="fc1f1-206">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="fc1f1-207">Dodaj migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-207">dotnet ef migrations add</span></span>

<span data-ttu-id="fc1f1-208">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-208">Adds a new migration.</span></span>

<span data-ttu-id="fc1f1-209">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="fc1f1-210">\<NAME></span><span class="sxs-lookup"><span data-stu-id="fc1f1-210">\<NAME></span></span> | <span data-ttu-id="fc1f1-211">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-211">The name of the migration.</span></span> |

<span data-ttu-id="fc1f1-212">Opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="fc1f1-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="fc1f1-214"><nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr></span><span class="sxs-lookup"><span data-stu-id="fc1f1-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="fc1f1-215">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="fc1f1-216">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="fc1f1-217">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="fc1f1-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="fc1f1-218">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-218">dotnet ef migrations list</span></span>

<span data-ttu-id="fc1f1-219">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="fc1f1-220">Usuń migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="fc1f1-221">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-221">Removes the last migration.</span></span>

<span data-ttu-id="fc1f1-222">Opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-223">-f</span><span class="sxs-lookup"><span data-stu-id="fc1f1-223">-f</span></span> | <span data-ttu-id="fc1f1-224">--wymusić</span><span class="sxs-lookup"><span data-stu-id="fc1f1-224">--force</span></span> | <span data-ttu-id="fc1f1-225">Przywróć migracji, jeśli została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="fc1f1-226">skrypt migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="fc1f1-226">dotnet ef migrations script</span></span>

<span data-ttu-id="fc1f1-227">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="fc1f1-228">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="fc1f1-229">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="fc1f1-229">\<FROM></span></span> | <span data-ttu-id="fc1f1-230">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-230">The starting migration.</span></span> <span data-ttu-id="fc1f1-231">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="fc1f1-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="fc1f1-232">\<ABY &GT;</span><span class="sxs-lookup"><span data-stu-id="fc1f1-232">\<TO></span></span>   | <span data-ttu-id="fc1f1-233">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-233">The ending migration.</span></span> <span data-ttu-id="fc1f1-234">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="fc1f1-235">Opcje:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="fc1f1-236">-o</span><span class="sxs-lookup"><span data-stu-id="fc1f1-236">-o</span></span> | <span data-ttu-id="fc1f1-237">--dane wyjściowe \<pliku ></span><span class="sxs-lookup"><span data-stu-id="fc1f1-237">--output \<FILE></span></span> | <span data-ttu-id="fc1f1-238">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="fc1f1-239">-i</span><span class="sxs-lookup"><span data-stu-id="fc1f1-239">-i</span></span> | <span data-ttu-id="fc1f1-240">--idempotentności</span><span class="sxs-lookup"><span data-stu-id="fc1f1-240">--idempotent</span></span>     | <span data-ttu-id="fc1f1-241">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
