---
title: .NET core interfejsu wiersza polecenia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152417"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="14178-102">Narzędzia wiersza polecenia programu EF Core platformy .NET</span><span class="sxs-lookup"><span data-stu-id="14178-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="14178-103">Narzędzia wiersza polecenia programu Entity Framework Core .NET to rozszerzenie dla wielu platform **dotnet** polecenia, który jest częścią programu [zestawu .NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="14178-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="14178-104">Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.</span><span class="sxs-lookup"><span data-stu-id="14178-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="14178-105">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="14178-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="14178-106">.NET Core SDK w wersji 2.1.300 oraz nowszych **dotnet ef** poleceń, które są zgodne z programem EF Core 2.0 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="14178-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="14178-107">W związku z tym jeśli używane są nowe wersje programu .NET Core SDK i środowiska uruchomieniowego EF Core, instalacja nie jest wymagana, i można zignorować pozostałej części tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="14178-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="14178-108">Z drugiej strony **dotnet ef** narzędzie zawarte w wersji zestawu .NET Core SDK 2.1.300 i nowszych nie jest zgodny z wersją programu EF Core 1.0 i 1.1.</span><span class="sxs-lookup"><span data-stu-id="14178-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="14178-109">Zanim można pracować z projektu, który używa tych starszych wersji programu EF Core na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania tej starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku.</span><span class="sxs-lookup"><span data-stu-id="14178-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="14178-110">Ten plik zwykle znajduje się w katalogu rozwiązania (po jednym powyżej projektu).</span><span class="sxs-lookup"><span data-stu-id="14178-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="14178-111">Następnie możesz przejść za pomocą instrukcji na temat poniżej.</span><span class="sxs-lookup"><span data-stu-id="14178-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="14178-112">We wcześniejszych wersjach programu .NET Core SDK można zainstalować narzędzia wiersza polecenia platformy .NET Core EF wykonując następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="14178-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="14178-113">Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)</span><span class="sxs-lookup"><span data-stu-id="14178-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="14178-114">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="14178-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="14178-115">Projekt wynikowy powinien wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="14178-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="14178-116">Odwołania do pakietu przy użyciu `PrivateAssets="All"` oznacza, że nie jest widoczne projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="14178-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="14178-117">Jeśli tak, nie wszystko bezpośrednio należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="14178-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="14178-118">Przy użyciu narzędzi</span><span class="sxs-lookup"><span data-stu-id="14178-118">Using the tools</span></span>
---------------
<span data-ttu-id="14178-119">Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="14178-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="14178-120">Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="14178-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="14178-121">Projekt docelowy domyślnie do projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **`--project`** </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="14178-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="14178-122">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="14178-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="14178-123">On również wartość domyślna to projekt w bieżącym katalogu, ale można zmienić za pomocą **`--startup-project`** opcji.</span><span class="sxs-lookup"><span data-stu-id="14178-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="14178-124">Na przykład aktualizowanie bazy danych zawierającej programu EF Core zainstalowany w innym projekcie aplikacji sieci web będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)</span><span class="sxs-lookup"><span data-stu-id="14178-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="14178-125">Typowe opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="14178-126">Pokaż dane wyjściowe JSON.</span><span class="sxs-lookup"><span data-stu-id="14178-126">Show JSON output.</span></span>           |
| <span data-ttu-id="14178-127">-c</span><span class="sxs-lookup"><span data-stu-id="14178-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="14178-128">Kontekst DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="14178-129">-p</span><span class="sxs-lookup"><span data-stu-id="14178-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="14178-130">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-130">The project to use.</span></span>         |
| <span data-ttu-id="14178-131">-s</span><span class="sxs-lookup"><span data-stu-id="14178-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="14178-132">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="14178-133">Platforma docelowa.</span><span class="sxs-lookup"><span data-stu-id="14178-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="14178-134">Konfiguracja do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="14178-135">Środowisko uruchomieniowe do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-135">The runtime to use.</span></span>         |
| <span data-ttu-id="14178-136">-h</span><span class="sxs-lookup"><span data-stu-id="14178-136">-h</span></span> | `--help`                           | <span data-ttu-id="14178-137">Pokaż informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="14178-137">Show help information.</span></span>      |
| <span data-ttu-id="14178-138">-v</span><span class="sxs-lookup"><span data-stu-id="14178-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="14178-139">Wyświetlić pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="14178-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="14178-140">Nie kolorować danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="14178-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="14178-141">Prefiks danych wyjściowych z poziomu.</span><span class="sxs-lookup"><span data-stu-id="14178-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="14178-142">Aby określić środowiska ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="14178-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="14178-143">Polecenia</span><span class="sxs-lookup"><span data-stu-id="14178-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="14178-144">docelowej bazie danych ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-144">dotnet ef database drop</span></span>

<span data-ttu-id="14178-145">Porzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14178-145">Drops the database.</span></span>

<span data-ttu-id="14178-146">Opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="14178-147">-f</span><span class="sxs-lookup"><span data-stu-id="14178-147">-f</span></span> | `--force`   | <span data-ttu-id="14178-148">Nie Potwierdź.</span><span class="sxs-lookup"><span data-stu-id="14178-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="14178-149">Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej.</span><span class="sxs-lookup"><span data-stu-id="14178-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="14178-150">Aktualizacja bazy danych programu ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-150">dotnet ef database update</span></span>

<span data-ttu-id="14178-151">Aktualizuje bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="14178-152">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="14178-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="14178-153">Migracja docelowego.</span><span class="sxs-lookup"><span data-stu-id="14178-153">The target migration.</span></span> <span data-ttu-id="14178-154">Jeśli jest to 0, będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="14178-155">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="14178-156">informacje kontekstu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="14178-157">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="14178-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="14178-158">Lista typu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="14178-159">Wyświetla listę dostępnych typów DbContext.</span><span class="sxs-lookup"><span data-stu-id="14178-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="14178-160">Tworzenie szkieletu dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="14178-161">Szkielety mechanizmów DbContext i jednostek typów dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14178-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="14178-162">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="14178-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="14178-163">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="14178-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="14178-164">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-164">The provider to use.</span></span> <span data-ttu-id="14178-165">(na przykład Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="14178-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="14178-166">Opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="14178-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="14178-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="14178-168">Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów.</span><span class="sxs-lookup"><span data-stu-id="14178-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="14178-169">W przypadku pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="14178-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="14178-170">-c</span><span class="sxs-lookup"><span data-stu-id="14178-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="14178-171">Nazwa typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="14178-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="14178-172">Katalog, który można umieścić plik typu DbContext w.</span><span class="sxs-lookup"><span data-stu-id="14178-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="14178-173">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="14178-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="14178-174">-f</span><span class="sxs-lookup"><span data-stu-id="14178-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="14178-175">Nadpisz istniejące pliki.</span><span class="sxs-lookup"><span data-stu-id="14178-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="14178-176">-o</span><span class="sxs-lookup"><span data-stu-id="14178-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="14178-177">Katalog, który można umieścić pliki w.</span><span class="sxs-lookup"><span data-stu-id="14178-177">The directory to put files in.</span></span> <span data-ttu-id="14178-178">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="14178-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="14178-179">Schematów tabel w celu wygenerowania typów jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="14178-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="14178-180">-t</span><span class="sxs-lookup"><span data-stu-id="14178-180">-t</span></span>              | <span data-ttu-id="14178-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="14178-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="14178-182">Tabele, aby wygenerować typy jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="14178-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="14178-183">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14178-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="14178-184">Dodaj migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-184">dotnet ef migrations add</span></span>

<span data-ttu-id="14178-185">Dodaje nową migrację.</span><span class="sxs-lookup"><span data-stu-id="14178-185">Adds a new migration.</span></span>

<span data-ttu-id="14178-186">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="14178-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="14178-187">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-187">The name of the migration.</span></span> |

<span data-ttu-id="14178-188">Opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="14178-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="14178-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="14178-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="14178-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="14178-191">Katalog (i podrzędnej przestrzeni nazw) do użycia.</span><span class="sxs-lookup"><span data-stu-id="14178-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="14178-192">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="14178-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="14178-193">Wartość domyślna to "Migracja".</span><span class="sxs-lookup"><span data-stu-id="14178-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="14178-194">Lista migracje ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-194">dotnet ef migrations list</span></span>

<span data-ttu-id="14178-195">Wyświetla listę dostępnych migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="14178-196">Usuń migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="14178-197">Usuwa ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-197">Removes the last migration.</span></span>

<span data-ttu-id="14178-198">Opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="14178-199">-f</span><span class="sxs-lookup"><span data-stu-id="14178-199">-f</span></span> | `--force` | <span data-ttu-id="14178-200">Przywróć migracji, jeśli został zastosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14178-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="14178-201">skrypt migracji ef DotNet</span><span class="sxs-lookup"><span data-stu-id="14178-201">dotnet ef migrations script</span></span>

<span data-ttu-id="14178-202">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="14178-203">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="14178-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="14178-204">Począwszy od migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-204">The starting migration.</span></span> <span data-ttu-id="14178-205">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="14178-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="14178-206">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-206">The ending migration.</span></span> <span data-ttu-id="14178-207">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="14178-208">Opcje:</span><span class="sxs-lookup"><span data-stu-id="14178-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="14178-209">-o</span><span class="sxs-lookup"><span data-stu-id="14178-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="14178-210">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="14178-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="14178-211">-i</span><span class="sxs-lookup"><span data-stu-id="14178-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="14178-212">Generowanie skryptu, który może służyć w bazie danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="14178-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
