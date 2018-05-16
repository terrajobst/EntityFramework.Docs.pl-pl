---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="e19aa-102">Narzędzia wiersza polecenia platformy .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="e19aa-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="e19aa-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="e19aa-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="e19aa-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="e19aa-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="e19aa-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="e19aa-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="e19aa-106">.NET Core SDK w wersji 2.1.300 i zawiera nowszą **dotnet ef** poleceń, które są zgodne z EF Core w wersji 2.0 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="e19aa-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="e19aa-107">Dlatego jeśli używane są nowe wersje zestawu SDK .NET Core i EF podstawowego środowiska wykonawczego, instalacja nie jest wymagana i pozostałej części tej sekcji można zignorować.</span><span class="sxs-lookup"><span data-stu-id="e19aa-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="e19aa-108">Z drugiej strony **dotnet ef** narzędzie zawartych w wersji zestawu SDK programu .NET Core 2.1.300 i nowszych nie jest zgodny z wersją EF Core 1.0 i 1.1.</span><span class="sxs-lookup"><span data-stu-id="e19aa-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="e19aa-109">Przed możesz pracować z projektu, który używa tych wersji EF rdzeni na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku.</span><span class="sxs-lookup"><span data-stu-id="e19aa-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="e19aa-110">Ten plik znajduje się zwykle w katalogu rozwiązania (jeden nad projektu).</span><span class="sxs-lookup"><span data-stu-id="e19aa-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="e19aa-111">Następnie można przejść z poniższych instrukcji na temat.</span><span class="sxs-lookup"><span data-stu-id="e19aa-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="e19aa-112">W poprzednich wersjach zestawu SDK .NET Core można zainstalować narzędzi wiersza polecenia platformy .NET Core EF przy użyciu następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="e19aa-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="e19aa-113">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="e19aa-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="e19aa-114">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e19aa-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="e19aa-115">Projekt wynikowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="e19aa-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="e19aa-116">Odwołanie pakietu z `PrivateAssets="All"` oznacza, że nie jest on ujawniony dla projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="e19aa-117">Jeśli tak, nie wszystkie prawa należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="e19aa-118">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="e19aa-118">Using the tools</span></span>
---------------
<span data-ttu-id="e19aa-119">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="e19aa-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="e19aa-120">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="e19aa-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="e19aa-121">Docelowy projekt domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **--projektu** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="e19aa-122">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="e19aa-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="e19aa-123">On również domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="e19aa-124">Na przykład uaktualnienie bazy danych aplikacji sieci web, który został zainstalowany w innym projekcie Core EF będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)</span><span class="sxs-lookup"><span data-stu-id="e19aa-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="e19aa-125">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="e19aa-126">--json</span><span class="sxs-lookup"><span data-stu-id="e19aa-126">--json</span></span>                           | <span data-ttu-id="e19aa-127">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="e19aa-127">Show JSON output.</span></span>           |
| <span data-ttu-id="e19aa-128">-c</span><span class="sxs-lookup"><span data-stu-id="e19aa-128">-c</span></span> | <span data-ttu-id="e19aa-129">--kontekstu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="e19aa-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="e19aa-130">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="e19aa-131">-p</span><span class="sxs-lookup"><span data-stu-id="e19aa-131">-p</span></span> | <span data-ttu-id="e19aa-132">--projektu \<projektu ></span><span class="sxs-lookup"><span data-stu-id="e19aa-132">--project \<PROJECT></span></span>             | <span data-ttu-id="e19aa-133">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-133">The project to use.</span></span>         |
| <span data-ttu-id="e19aa-134">-s</span><span class="sxs-lookup"><span data-stu-id="e19aa-134">-s</span></span> | <span data-ttu-id="e19aa-135">--Projekt startowy \<projektu ></span><span class="sxs-lookup"><span data-stu-id="e19aa-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="e19aa-136">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="e19aa-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="e19aa-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="e19aa-138">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="e19aa-138">The target framework.</span></span>       |
|    | <span data-ttu-id="e19aa-139">--Konfiguracja \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="e19aa-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="e19aa-140">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="e19aa-141">środowisko uruchomieniowe — \<identyfikator ></span><span class="sxs-lookup"><span data-stu-id="e19aa-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="e19aa-142">Środowiska uruchomieniowego do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-142">The runtime to use.</span></span>         |
| <span data-ttu-id="e19aa-143">-h</span><span class="sxs-lookup"><span data-stu-id="e19aa-143">-h</span></span> | <span data-ttu-id="e19aa-144">--help</span><span class="sxs-lookup"><span data-stu-id="e19aa-144">--help</span></span>                           | <span data-ttu-id="e19aa-145">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="e19aa-145">Show help information.</span></span>      |
| <span data-ttu-id="e19aa-146">-v</span><span class="sxs-lookup"><span data-stu-id="e19aa-146">-v</span></span> | <span data-ttu-id="e19aa-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="e19aa-147">--verbose</span></span>                        | <span data-ttu-id="e19aa-148">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="e19aa-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="e19aa-149">— Brak koloru</span><span class="sxs-lookup"><span data-stu-id="e19aa-149">--no-color</span></span>                       | <span data-ttu-id="e19aa-150">Nie kolorowanie danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="e19aa-151">--dane wyjściowe prefiksu</span><span class="sxs-lookup"><span data-stu-id="e19aa-151">--prefix-output</span></span>                  | <span data-ttu-id="e19aa-152">Dane wyjściowe z poziomu prefiks.</span><span class="sxs-lookup"><span data-stu-id="e19aa-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="e19aa-153">Aby w środowisku ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="e19aa-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="e19aa-154">Polecenia</span><span class="sxs-lookup"><span data-stu-id="e19aa-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="e19aa-155">Usuń bazę danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-155">dotnet ef database drop</span></span>

<span data-ttu-id="e19aa-156">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-156">Drops the database.</span></span>

<span data-ttu-id="e19aa-157">Opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="e19aa-158">-f</span><span class="sxs-lookup"><span data-stu-id="e19aa-158">-f</span></span> | <span data-ttu-id="e19aa-159">--wymusić</span><span class="sxs-lookup"><span data-stu-id="e19aa-159">--force</span></span>   | <span data-ttu-id="e19aa-160">Nie potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="e19aa-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="e19aa-161">--przebiegu próbnego</span><span class="sxs-lookup"><span data-stu-id="e19aa-161">--dry-run</span></span> | <span data-ttu-id="e19aa-162">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="e19aa-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="e19aa-163">Aktualizacja bazy danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-163">dotnet ef database update</span></span>

<span data-ttu-id="e19aa-164">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="e19aa-165">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="e19aa-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="e19aa-166">\<MIGRACJA &GT;</span><span class="sxs-lookup"><span data-stu-id="e19aa-166">\<MIGRATION></span></span> | <span data-ttu-id="e19aa-167">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-167">The target migration.</span></span> <span data-ttu-id="e19aa-168">Jeśli 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="e19aa-169">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="e19aa-170">informacje o dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="e19aa-171">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="e19aa-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="e19aa-172">Lista dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="e19aa-173">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="e19aa-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="e19aa-174">szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="e19aa-175">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="e19aa-176">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="e19aa-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="e19aa-177">\<POŁĄCZENIA &GT;</span><span class="sxs-lookup"><span data-stu-id="e19aa-177">\<CONNECTION></span></span> | <span data-ttu-id="e19aa-178">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="e19aa-179">\<DOSTAWCA &GT;</span><span class="sxs-lookup"><span data-stu-id="e19aa-179">\<PROVIDER></span></span>   | <span data-ttu-id="e19aa-180">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-180">The provider to use.</span></span> <span data-ttu-id="e19aa-181">(Np.</span><span class="sxs-lookup"><span data-stu-id="e19aa-181">(E.g.</span></span> <span data-ttu-id="e19aa-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="e19aa-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="e19aa-183">Opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e19aa-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="e19aa-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="e19aa-185">--adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="e19aa-185">--data-annotations</span></span>                      | <span data-ttu-id="e19aa-186">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="e19aa-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="e19aa-187">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="e19aa-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="e19aa-188">-c</span><span class="sxs-lookup"><span data-stu-id="e19aa-188">-c</span></span>              | <span data-ttu-id="e19aa-189">--kontekstu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="e19aa-189">--context \<NAME></span></span>                       | <span data-ttu-id="e19aa-190">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="e19aa-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="e19aa-191">-dir kontekstu \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="e19aa-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="e19aa-192">Katalog mają zostać umieszczone w pliku DbContext.</span><span class="sxs-lookup"><span data-stu-id="e19aa-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="e19aa-193">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e19aa-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="e19aa-194">-f</span><span class="sxs-lookup"><span data-stu-id="e19aa-194">-f</span></span>              | <span data-ttu-id="e19aa-195">--wymusić</span><span class="sxs-lookup"><span data-stu-id="e19aa-195">--force</span></span>                                 | <span data-ttu-id="e19aa-196">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="e19aa-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="e19aa-197">-o</span><span class="sxs-lookup"><span data-stu-id="e19aa-197">-o</span></span>              | <span data-ttu-id="e19aa-198">--katalog wyjściowy \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="e19aa-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="e19aa-199">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="e19aa-199">The directory to put files in.</span></span> <span data-ttu-id="e19aa-200">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e19aa-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="e19aa-201"><nobr>--schematu \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="e19aa-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="e19aa-202">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="e19aa-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="e19aa-203">-t</span><span class="sxs-lookup"><span data-stu-id="e19aa-203">-t</span></span>              | <span data-ttu-id="e19aa-204">--tabeli \<nazwa_tabeli >...</span><span class="sxs-lookup"><span data-stu-id="e19aa-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="e19aa-205">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="e19aa-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="e19aa-206">--nazwy w przypadku baz danych użycia</span><span class="sxs-lookup"><span data-stu-id="e19aa-206">--use-database-names</span></span>                    | <span data-ttu-id="e19aa-207">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="e19aa-208">Dodaj migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-208">dotnet ef migrations add</span></span>

<span data-ttu-id="e19aa-209">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-209">Adds a new migration.</span></span>

<span data-ttu-id="e19aa-210">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="e19aa-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="e19aa-211">\<NAME></span><span class="sxs-lookup"><span data-stu-id="e19aa-211">\<NAME></span></span> | <span data-ttu-id="e19aa-212">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-212">The name of the migration.</span></span> |

<span data-ttu-id="e19aa-213">Opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e19aa-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="e19aa-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="e19aa-215"><nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr></span><span class="sxs-lookup"><span data-stu-id="e19aa-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="e19aa-216">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="e19aa-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="e19aa-217">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e19aa-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="e19aa-218">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="e19aa-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="e19aa-219">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-219">dotnet ef migrations list</span></span>

<span data-ttu-id="e19aa-220">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="e19aa-221">Usuń migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="e19aa-222">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-222">Removes the last migration.</span></span>

<span data-ttu-id="e19aa-223">Opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="e19aa-224">-f</span><span class="sxs-lookup"><span data-stu-id="e19aa-224">-f</span></span> | <span data-ttu-id="e19aa-225">--wymusić</span><span class="sxs-lookup"><span data-stu-id="e19aa-225">--force</span></span> | <span data-ttu-id="e19aa-226">Przywróć migracji, jeśli została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e19aa-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="e19aa-227">skrypt migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="e19aa-227">dotnet ef migrations script</span></span>

<span data-ttu-id="e19aa-228">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="e19aa-229">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="e19aa-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="e19aa-230">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="e19aa-230">\<FROM></span></span> | <span data-ttu-id="e19aa-231">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-231">The starting migration.</span></span> <span data-ttu-id="e19aa-232">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="e19aa-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="e19aa-233">\<ABY &GT;</span><span class="sxs-lookup"><span data-stu-id="e19aa-233">\<TO></span></span>   | <span data-ttu-id="e19aa-234">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-234">The ending migration.</span></span> <span data-ttu-id="e19aa-235">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="e19aa-236">Opcje:</span><span class="sxs-lookup"><span data-stu-id="e19aa-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="e19aa-237">-o</span><span class="sxs-lookup"><span data-stu-id="e19aa-237">-o</span></span> | <span data-ttu-id="e19aa-238">--dane wyjściowe \<pliku ></span><span class="sxs-lookup"><span data-stu-id="e19aa-238">--output \<FILE></span></span> | <span data-ttu-id="e19aa-239">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="e19aa-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="e19aa-240">-i</span><span class="sxs-lookup"><span data-stu-id="e19aa-240">-i</span></span> | <span data-ttu-id="e19aa-241">--idempotentności</span><span class="sxs-lookup"><span data-stu-id="e19aa-241">--idempotent</span></span>     | <span data-ttu-id="e19aa-242">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="e19aa-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
