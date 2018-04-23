---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="60db8-102">Narzędzia wiersza polecenia platformy .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="60db8-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="60db8-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="60db8-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="60db8-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="60db8-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="60db8-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="60db8-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="60db8-106">Zainstaluj narzędzia wiersza polecenia platformy .NET Core EF trzy kroki:</span><span class="sxs-lookup"><span data-stu-id="60db8-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="60db8-107">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="60db8-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="60db8-108">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="60db8-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="60db8-109">Projekt wynikowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="60db8-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="60db8-110">Odwołanie pakietu z `PrivateAssets="All"` oznacza, że nie jest on ujawniony dla projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="60db8-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="60db8-111">Jeśli tak, nie wszystkie prawa należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="60db8-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="60db8-112">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="60db8-112">Using the tools</span></span>
---------------
<span data-ttu-id="60db8-113">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="60db8-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="60db8-114">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="60db8-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="60db8-115">Docelowy projekt domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **--projektu** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="60db8-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="60db8-116">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="60db8-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="60db8-117">On również domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.</span><span class="sxs-lookup"><span data-stu-id="60db8-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="60db8-118">Na przykład uaktualnienie bazy danych aplikacji sieci web, który został zainstalowany w innym projekcie Core EF będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)</span><span class="sxs-lookup"><span data-stu-id="60db8-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="60db8-119">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="60db8-120">--json</span><span class="sxs-lookup"><span data-stu-id="60db8-120">--json</span></span>                           | <span data-ttu-id="60db8-121">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="60db8-121">Show JSON output.</span></span>           |
| <span data-ttu-id="60db8-122">-c</span><span class="sxs-lookup"><span data-stu-id="60db8-122">-c</span></span> | <span data-ttu-id="60db8-123">--kontekstu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="60db8-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="60db8-124">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="60db8-125">-p</span><span class="sxs-lookup"><span data-stu-id="60db8-125">-p</span></span> | <span data-ttu-id="60db8-126">--projektu \<projektu ></span><span class="sxs-lookup"><span data-stu-id="60db8-126">--project \<PROJECT></span></span>             | <span data-ttu-id="60db8-127">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-127">The project to use.</span></span>         |
| <span data-ttu-id="60db8-128">-s</span><span class="sxs-lookup"><span data-stu-id="60db8-128">-s</span></span> | <span data-ttu-id="60db8-129">--Projekt startowy \<projektu ></span><span class="sxs-lookup"><span data-stu-id="60db8-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="60db8-130">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="60db8-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="60db8-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="60db8-132">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="60db8-132">The target framework.</span></span>       |
|    | <span data-ttu-id="60db8-133">--Konfiguracja \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="60db8-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="60db8-134">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="60db8-135">środowisko uruchomieniowe — \<identyfikator ></span><span class="sxs-lookup"><span data-stu-id="60db8-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="60db8-136">Środowiska uruchomieniowego do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-136">The runtime to use.</span></span>         |
| <span data-ttu-id="60db8-137">-h</span><span class="sxs-lookup"><span data-stu-id="60db8-137">-h</span></span> | <span data-ttu-id="60db8-138">--help</span><span class="sxs-lookup"><span data-stu-id="60db8-138">--help</span></span>                           | <span data-ttu-id="60db8-139">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="60db8-139">Show help information.</span></span>      |
| <span data-ttu-id="60db8-140">-v</span><span class="sxs-lookup"><span data-stu-id="60db8-140">-v</span></span> | <span data-ttu-id="60db8-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="60db8-141">--verbose</span></span>                        | <span data-ttu-id="60db8-142">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="60db8-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="60db8-143">— Brak koloru</span><span class="sxs-lookup"><span data-stu-id="60db8-143">--no-color</span></span>                       | <span data-ttu-id="60db8-144">Nie kolorowanie danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="60db8-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="60db8-145">--dane wyjściowe prefiksu</span><span class="sxs-lookup"><span data-stu-id="60db8-145">--prefix-output</span></span>                  | <span data-ttu-id="60db8-146">Dane wyjściowe z poziomu prefiks.</span><span class="sxs-lookup"><span data-stu-id="60db8-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="60db8-147">Aby w środowisku ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="60db8-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="60db8-148">Polecenia</span><span class="sxs-lookup"><span data-stu-id="60db8-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="60db8-149">Usuń bazę danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-149">dotnet ef database drop</span></span>

<span data-ttu-id="60db8-150">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60db8-150">Drops the database.</span></span>

<span data-ttu-id="60db8-151">Opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="60db8-152">-f</span><span class="sxs-lookup"><span data-stu-id="60db8-152">-f</span></span> | <span data-ttu-id="60db8-153">--wymusić</span><span class="sxs-lookup"><span data-stu-id="60db8-153">--force</span></span>   | <span data-ttu-id="60db8-154">Nie potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="60db8-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="60db8-155">--przebiegu próbnego</span><span class="sxs-lookup"><span data-stu-id="60db8-155">--dry-run</span></span> | <span data-ttu-id="60db8-156">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="60db8-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="60db8-157">Aktualizacja bazy danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-157">dotnet ef database update</span></span>

<span data-ttu-id="60db8-158">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="60db8-159">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="60db8-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="60db8-160">\<MIGRACJA &GT;</span><span class="sxs-lookup"><span data-stu-id="60db8-160">\<MIGRATION></span></span> | <span data-ttu-id="60db8-161">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="60db8-161">The target migration.</span></span> <span data-ttu-id="60db8-162">Jeśli 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="60db8-163">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="60db8-164">informacje o dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="60db8-165">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="60db8-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="60db8-166">Lista dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="60db8-167">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="60db8-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="60db8-168">szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="60db8-169">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60db8-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="60db8-170">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="60db8-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="60db8-171">\<POŁĄCZENIA &GT;</span><span class="sxs-lookup"><span data-stu-id="60db8-171">\<CONNECTION></span></span> | <span data-ttu-id="60db8-172">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="60db8-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="60db8-173">\<DOSTAWCA &GT;</span><span class="sxs-lookup"><span data-stu-id="60db8-173">\<PROVIDER></span></span>   | <span data-ttu-id="60db8-174">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-174">The provider to use.</span></span> <span data-ttu-id="60db8-175">(Np.</span><span class="sxs-lookup"><span data-stu-id="60db8-175">(E.g.</span></span> <span data-ttu-id="60db8-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="60db8-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="60db8-177">Opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="60db8-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="60db8-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="60db8-179">--adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="60db8-179">--data-annotations</span></span>                      | <span data-ttu-id="60db8-180">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="60db8-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="60db8-181">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="60db8-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="60db8-182">-c</span><span class="sxs-lookup"><span data-stu-id="60db8-182">-c</span></span>              | <span data-ttu-id="60db8-183">--kontekstu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="60db8-183">--context \<NAME></span></span>                       | <span data-ttu-id="60db8-184">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="60db8-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="60db8-185">-dir kontekstu \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="60db8-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="60db8-186">Katalog mają zostać umieszczone w pliku DbContext.</span><span class="sxs-lookup"><span data-stu-id="60db8-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="60db8-187">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="60db8-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="60db8-188">-f</span><span class="sxs-lookup"><span data-stu-id="60db8-188">-f</span></span>              | <span data-ttu-id="60db8-189">--wymusić</span><span class="sxs-lookup"><span data-stu-id="60db8-189">--force</span></span>                                 | <span data-ttu-id="60db8-190">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="60db8-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="60db8-191">-o</span><span class="sxs-lookup"><span data-stu-id="60db8-191">-o</span></span>              | <span data-ttu-id="60db8-192">--katalog wyjściowy \<ŚCIEŻKĘ ></span><span class="sxs-lookup"><span data-stu-id="60db8-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="60db8-193">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="60db8-193">The directory to put files in.</span></span> <span data-ttu-id="60db8-194">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="60db8-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="60db8-195"><nobr>--schematu \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="60db8-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="60db8-196">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="60db8-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="60db8-197">-t</span><span class="sxs-lookup"><span data-stu-id="60db8-197">-t</span></span>              | <span data-ttu-id="60db8-198">--tabeli \<nazwa_tabeli >...</span><span class="sxs-lookup"><span data-stu-id="60db8-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="60db8-199">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="60db8-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="60db8-200">--nazwy w przypadku baz danych użycia</span><span class="sxs-lookup"><span data-stu-id="60db8-200">--use-database-names</span></span>                    | <span data-ttu-id="60db8-201">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60db8-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="60db8-202">Dodaj migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-202">dotnet ef migrations add</span></span>

<span data-ttu-id="60db8-203">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-203">Adds a new migration.</span></span>

<span data-ttu-id="60db8-204">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="60db8-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="60db8-205">\<NAME></span><span class="sxs-lookup"><span data-stu-id="60db8-205">\<NAME></span></span> | <span data-ttu-id="60db8-206">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-206">The name of the migration.</span></span> |

<span data-ttu-id="60db8-207">Opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="60db8-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="60db8-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="60db8-209"><nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr></span><span class="sxs-lookup"><span data-stu-id="60db8-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="60db8-210">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="60db8-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="60db8-211">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="60db8-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="60db8-212">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="60db8-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="60db8-213">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-213">dotnet ef migrations list</span></span>

<span data-ttu-id="60db8-214">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="60db8-215">Usuń migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="60db8-216">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-216">Removes the last migration.</span></span>

<span data-ttu-id="60db8-217">Opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="60db8-218">-f</span><span class="sxs-lookup"><span data-stu-id="60db8-218">-f</span></span> | <span data-ttu-id="60db8-219">--wymusić</span><span class="sxs-lookup"><span data-stu-id="60db8-219">--force</span></span> | <span data-ttu-id="60db8-220">Przywróć migracji, jeśli została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60db8-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="60db8-221">skrypt migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="60db8-221">dotnet ef migrations script</span></span>

<span data-ttu-id="60db8-222">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="60db8-223">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="60db8-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="60db8-224">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="60db8-224">\<FROM></span></span> | <span data-ttu-id="60db8-225">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-225">The starting migration.</span></span> <span data-ttu-id="60db8-226">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="60db8-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="60db8-227">\<ABY &GT;</span><span class="sxs-lookup"><span data-stu-id="60db8-227">\<TO></span></span>   | <span data-ttu-id="60db8-228">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-228">The ending migration.</span></span> <span data-ttu-id="60db8-229">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="60db8-230">Opcje:</span><span class="sxs-lookup"><span data-stu-id="60db8-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="60db8-231">-o</span><span class="sxs-lookup"><span data-stu-id="60db8-231">-o</span></span> | <span data-ttu-id="60db8-232">--dane wyjściowe \<pliku ></span><span class="sxs-lookup"><span data-stu-id="60db8-232">--output \<FILE></span></span> | <span data-ttu-id="60db8-233">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="60db8-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="60db8-234">-i</span><span class="sxs-lookup"><span data-stu-id="60db8-234">-i</span></span> | <span data-ttu-id="60db8-235">--idempotentności</span><span class="sxs-lookup"><span data-stu-id="60db8-235">--idempotent</span></span>     | <span data-ttu-id="60db8-236">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="60db8-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
