---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="3aaf5-102">Narzędzia wiersza polecenia platformy .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="3aaf5-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="3aaf5-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="3aaf5-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="3aaf5-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="3aaf5-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="3aaf5-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="3aaf5-106">Zainstaluj narzędzia wiersza polecenia platformy .NET Core EF trzy kroki:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="3aaf5-107">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="3aaf5-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="3aaf5-108">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="3aaf5-109">Projekt wynikowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="3aaf5-110">Odwołanie pakietu z `PrivateAssets="All"` oznacza, że nie jest on ujawniony dla projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="3aaf5-111">Jeśli tak, nie wszystkie prawa należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="3aaf5-112">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="3aaf5-112">Using the tools</span></span>
---------------
<span data-ttu-id="3aaf5-113">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="3aaf5-114">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="3aaf5-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="3aaf5-115">Docelowy projekt domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **--projektu** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="3aaf5-116">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="3aaf5-117">On również domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="3aaf5-118">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="3aaf5-119">--json</span><span class="sxs-lookup"><span data-stu-id="3aaf5-119">--json</span></span>                           | <span data-ttu-id="3aaf5-120">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-120">Show JSON output.</span></span>           |
| <span data-ttu-id="3aaf5-121">-c</span><span class="sxs-lookup"><span data-stu-id="3aaf5-121">-c</span></span> | <span data-ttu-id="3aaf5-122">--kontekstu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="3aaf5-123">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="3aaf5-124">-p</span><span class="sxs-lookup"><span data-stu-id="3aaf5-124">-p</span></span> | <span data-ttu-id="3aaf5-125">--projektu \<projektu ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-125">--project \<PROJECT></span></span>             | <span data-ttu-id="3aaf5-126">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-126">The project to use.</span></span>         |
| <span data-ttu-id="3aaf5-127">-s</span><span class="sxs-lookup"><span data-stu-id="3aaf5-127">-s</span></span> | <span data-ttu-id="3aaf5-128">--Projekt startowy \<projektu ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="3aaf5-129">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="3aaf5-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="3aaf5-131">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-131">The target framework.</span></span>       |
|    | <span data-ttu-id="3aaf5-132">--Konfiguracja \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="3aaf5-133">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="3aaf5-134">środowisko uruchomieniowe — \<identyfikator ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="3aaf5-135">Środowiska uruchomieniowego do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-135">The runtime to use.</span></span>         |
| <span data-ttu-id="3aaf5-136">-h</span><span class="sxs-lookup"><span data-stu-id="3aaf5-136">-h</span></span> | <span data-ttu-id="3aaf5-137">--help</span><span class="sxs-lookup"><span data-stu-id="3aaf5-137">--help</span></span>                           | <span data-ttu-id="3aaf5-138">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-138">Show help information.</span></span>      |
| <span data-ttu-id="3aaf5-139">-v</span><span class="sxs-lookup"><span data-stu-id="3aaf5-139">-v</span></span> | <span data-ttu-id="3aaf5-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="3aaf5-140">--verbose</span></span>                        | <span data-ttu-id="3aaf5-141">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="3aaf5-142">— Brak koloru</span><span class="sxs-lookup"><span data-stu-id="3aaf5-142">--no-color</span></span>                       | <span data-ttu-id="3aaf5-143">Nie kolorowanie danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="3aaf5-144">--dane wyjściowe prefiksu</span><span class="sxs-lookup"><span data-stu-id="3aaf5-144">--prefix-output</span></span>                  | <span data-ttu-id="3aaf5-145">Dane wyjściowe z poziomu prefiks.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="3aaf5-146">Aby w środowisku ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="3aaf5-147">Polecenia</span><span class="sxs-lookup"><span data-stu-id="3aaf5-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="3aaf5-148">Usuń bazę danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-148">dotnet ef database drop</span></span>

<span data-ttu-id="3aaf5-149">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-149">Drops the database.</span></span>

<span data-ttu-id="3aaf5-150">Opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="3aaf5-151">-f</span><span class="sxs-lookup"><span data-stu-id="3aaf5-151">-f</span></span> | <span data-ttu-id="3aaf5-152">--wymusić</span><span class="sxs-lookup"><span data-stu-id="3aaf5-152">--force</span></span>   | <span data-ttu-id="3aaf5-153">Nie potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="3aaf5-154">--przebiegu próbnego</span><span class="sxs-lookup"><span data-stu-id="3aaf5-154">--dry-run</span></span> | <span data-ttu-id="3aaf5-155">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="3aaf5-156">Aktualizacja bazy danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-156">dotnet ef database update</span></span>

<span data-ttu-id="3aaf5-157">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="3aaf5-158">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-159">\<MIGRACJA ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-159">\<MIGRATION></span></span> | <span data-ttu-id="3aaf5-160">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-160">The target migration.</span></span> <span data-ttu-id="3aaf5-161">Jeśli 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="3aaf5-162">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="3aaf5-163">informacje o dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="3aaf5-164">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="3aaf5-165">Lista dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="3aaf5-166">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="3aaf5-167">szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="3aaf5-168">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="3aaf5-169">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-170">\<POŁĄCZENIA ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-170">\<CONNECTION></span></span> | <span data-ttu-id="3aaf5-171">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="3aaf5-172">\<DOSTAWCA ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-172">\<PROVIDER></span></span>   | <span data-ttu-id="3aaf5-173">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-173">The provider to use.</span></span> <span data-ttu-id="3aaf5-174">(Np.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-174">(E.g.</span></span> <span data-ttu-id="3aaf5-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="3aaf5-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="3aaf5-176">Opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="3aaf5-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="3aaf5-178">--adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="3aaf5-178">--data-annotations</span></span>                      | <span data-ttu-id="3aaf5-179">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="3aaf5-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="3aaf5-180">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="3aaf5-181">-c</span><span class="sxs-lookup"><span data-stu-id="3aaf5-181">-c</span></span>              | <span data-ttu-id="3aaf5-182">--kontekstu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-182">--context \<NAME></span></span>                       | <span data-ttu-id="3aaf5-183">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="3aaf5-184">-f</span><span class="sxs-lookup"><span data-stu-id="3aaf5-184">-f</span></span>              | <span data-ttu-id="3aaf5-185">--wymusić</span><span class="sxs-lookup"><span data-stu-id="3aaf5-185">--force</span></span>                                 | <span data-ttu-id="3aaf5-186">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="3aaf5-187">-o</span><span class="sxs-lookup"><span data-stu-id="3aaf5-187">-o</span></span>              | <span data-ttu-id="3aaf5-188">--katalog wyjściowy \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="3aaf5-189">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-189">The directory to put files in.</span></span> <span data-ttu-id="3aaf5-190">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="3aaf5-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="3aaf5-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="3aaf5-192">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="3aaf5-193">-t</span><span class="sxs-lookup"><span data-stu-id="3aaf5-193">-t</span></span>              | <span data-ttu-id="3aaf5-194">--tabeli \<nazwa_tabeli >...</span><span class="sxs-lookup"><span data-stu-id="3aaf5-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="3aaf5-195">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="3aaf5-196">--nazwy w przypadku baz danych użycia</span><span class="sxs-lookup"><span data-stu-id="3aaf5-196">--use-database-names</span></span>                    | <span data-ttu-id="3aaf5-197">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="3aaf5-198">Dodaj migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-198">dotnet ef migrations add</span></span>

<span data-ttu-id="3aaf5-199">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-199">Adds a new migration.</span></span>

<span data-ttu-id="3aaf5-200">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="3aaf5-201">\<NAME></span><span class="sxs-lookup"><span data-stu-id="3aaf5-201">\<NAME></span></span> | <span data-ttu-id="3aaf5-202">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-202">The name of the migration.</span></span> |

<span data-ttu-id="3aaf5-203">Opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="3aaf5-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="3aaf5-205"><nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr></span><span class="sxs-lookup"><span data-stu-id="3aaf5-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="3aaf5-206">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="3aaf5-207">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="3aaf5-208">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="3aaf5-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="3aaf5-209">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-209">dotnet ef migrations list</span></span>

<span data-ttu-id="3aaf5-210">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="3aaf5-211">Usuń migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="3aaf5-212">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-212">Removes the last migration.</span></span>

<span data-ttu-id="3aaf5-213">Opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-214">-f</span><span class="sxs-lookup"><span data-stu-id="3aaf5-214">-f</span></span> | <span data-ttu-id="3aaf5-215">--wymusić</span><span class="sxs-lookup"><span data-stu-id="3aaf5-215">--force</span></span> | <span data-ttu-id="3aaf5-216">Nie Sprawdź, czy migracja zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="3aaf5-217">skrypt migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="3aaf5-217">dotnet ef migrations script</span></span>

<span data-ttu-id="3aaf5-218">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="3aaf5-219">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="3aaf5-220">\<Z ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-220">\<FROM></span></span> | <span data-ttu-id="3aaf5-221">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-221">The starting migration.</span></span> <span data-ttu-id="3aaf5-222">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="3aaf5-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="3aaf5-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="3aaf5-223">\<TO></span></span>   | <span data-ttu-id="3aaf5-224">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-224">The ending migration.</span></span> <span data-ttu-id="3aaf5-225">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="3aaf5-226">Opcje:</span><span class="sxs-lookup"><span data-stu-id="3aaf5-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="3aaf5-227">-o</span><span class="sxs-lookup"><span data-stu-id="3aaf5-227">-o</span></span> | <span data-ttu-id="3aaf5-228">--dane wyjściowe \<pliku ></span><span class="sxs-lookup"><span data-stu-id="3aaf5-228">--output \<FILE></span></span> | <span data-ttu-id="3aaf5-229">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="3aaf5-230">-i</span><span class="sxs-lookup"><span data-stu-id="3aaf5-230">-i</span></span> | <span data-ttu-id="3aaf5-231">--idempotentności</span><span class="sxs-lookup"><span data-stu-id="3aaf5-231">--idempotent</span></span>     | <span data-ttu-id="3aaf5-232">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="3aaf5-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
