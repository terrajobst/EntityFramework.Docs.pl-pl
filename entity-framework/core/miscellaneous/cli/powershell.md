---
title: Konsola Menedżera pakietów (Visual Studio) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 0799b0cb7c5d837fdbb7a4af510a9a4d9d34ec1a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949041"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="a8527-102">Narzędzia konsoli Menedżera pakietów programu EF Core</span><span class="sxs-lookup"><span data-stu-id="a8527-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="a8527-103">Uruchomione narzędzia konsoli Menedżera pakietów (PMC) EF Core w programie Visual Studio za pomocą NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="a8527-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="a8527-104">Te narzędzia działają w projektach .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a8527-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="a8527-105">Nie można za pomocą programu Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="a8527-105">Not using Visual Studio?</span></span> <span data-ttu-id="a8527-106">[Narzędzi wiersza polecenia programu EF Core] [ 1] dla wielu platform i wykonywania w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="a8527-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a8527-107">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="a8527-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="a8527-108">Zainstaluj narzędzia konsoli Menedżera pakietów programu EF Core, instalując pakiet Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="a8527-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="a8527-109">Można ją zainstalować, wykonując następujące polecenie z poziomu [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="a8527-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="a8527-110">Jeśli wszystko działało poprawnie, należy uruchomić to polecenie:</span><span class="sxs-lookup"><span data-stu-id="a8527-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="a8527-111">Jeśli Twój projekt startowy jest przeznaczony dla .NET Standard [cross-target framework obsługiwane] [ 3] przed rozpoczęciem korzystania z narzędzia.</span><span class="sxs-lookup"><span data-stu-id="a8527-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8527-112">Jeśli używasz **Universal Windows** lub **Xamarin**, Przenieś swój kod programem EF do biblioteki klas .NET Standard i [cross-target framework obsługiwane] [ 3] przed rozpoczęciem korzystania z narzędzia.</span><span class="sxs-lookup"><span data-stu-id="a8527-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="a8527-113">Określ bibliotekę klas jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="a8527-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="a8527-114">Przy użyciu narzędzi</span><span class="sxs-lookup"><span data-stu-id="a8527-114">Using the tools</span></span>
---------------
<span data-ttu-id="a8527-115">Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="a8527-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a8527-116">Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="a8527-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a8527-117">Wartością domyślną jest projekt docelowy **domyślny projekt** wybrany w konsoli Menedżera pakietów, ale można również określić za pomocą parametru - projekt.</span><span class="sxs-lookup"><span data-stu-id="a8527-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="a8527-118">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="a8527-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a8527-119">Jego wartość domyślna to jeden **Ustaw jako projekt startowy** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a8527-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="a8527-120">Można również określić, za pomocą parametru - projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="a8527-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="a8527-121">Wspólne parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="a8527-122">-Kontekstu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-122">-Context \<String></span></span>        | <span data-ttu-id="a8527-123">Kontekst DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="a8527-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="a8527-124">-Projekt \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-124">-Project \<String></span></span>        | <span data-ttu-id="a8527-125">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="a8527-125">The project to use.</span></span>         |
| <span data-ttu-id="a8527-126">Projekt startowy - \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-126">-StartupProject \<String></span></span> | <span data-ttu-id="a8527-127">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="a8527-127">The startup project to use.</span></span> |
| <span data-ttu-id="a8527-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="a8527-128">-Verbose</span></span>                  | <span data-ttu-id="a8527-129">Wyświetlić pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="a8527-129">Show verbose output.</span></span>        |

<span data-ttu-id="a8527-130">Aby wyświetlić Pomoc dotyczącą polecenia, użyj programu PowerShell `Get-Help` polecenia.</span><span class="sxs-lookup"><span data-stu-id="a8527-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="a8527-131">Parametry kontekstu, projekt i projekt startowy obsługują rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="a8527-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="a8527-132">Ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem do określania środowiska ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8527-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="a8527-133">Polecenia</span><span class="sxs-lookup"><span data-stu-id="a8527-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="a8527-134">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="a8527-134">Add-Migration</span></span>

<span data-ttu-id="a8527-135">Dodaje nową migrację.</span><span class="sxs-lookup"><span data-stu-id="a8527-135">Adds a new migration.</span></span>

<span data-ttu-id="a8527-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a8527-137">***— Nazwa*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-137">***-Name*** \<String></span></span>             | <span data-ttu-id="a8527-138">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="a8527-139"><nobr>-OutputDir \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="a8527-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="a8527-140">Katalog (i podrzędnej przestrzeni nazw) do użycia.</span><span class="sxs-lookup"><span data-stu-id="a8527-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a8527-141">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a8527-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="a8527-142">Wartość domyślna to "Migracja".</span><span class="sxs-lookup"><span data-stu-id="a8527-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="a8527-143">Parametry w **bold** są wymagane i te w *Kursywa* są pozycyjnych.</span><span class="sxs-lookup"><span data-stu-id="a8527-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="a8527-144">Baza danych listy</span><span class="sxs-lookup"><span data-stu-id="a8527-144">Drop-Database</span></span>

<span data-ttu-id="a8527-145">Porzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8527-145">Drops the database.</span></span>

<span data-ttu-id="a8527-146">Parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="a8527-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="a8527-147">-WhatIf</span></span> | <span data-ttu-id="a8527-148">Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej.</span><span class="sxs-lookup"><span data-stu-id="a8527-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="a8527-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="a8527-149">Get-DbContext</span></span>

<span data-ttu-id="a8527-150">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="a8527-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="a8527-151">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="a8527-151">Remove-Migration</span></span>

<span data-ttu-id="a8527-152">Usuwa ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-152">Removes the last migration.</span></span>

<span data-ttu-id="a8527-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="a8527-154">-Force</span><span class="sxs-lookup"><span data-stu-id="a8527-154">-Force</span></span> | <span data-ttu-id="a8527-155">Przywróć migracji, jeśli został zastosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8527-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="a8527-156">Tworzenie szkieletu DbContext</span><span class="sxs-lookup"><span data-stu-id="a8527-156">Scaffold-DbContext</span></span>

<span data-ttu-id="a8527-157">Szkielety mechanizmów DbContext i jednostek typów dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8527-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a8527-158">Parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a8527-159"><nobr>***-Connection*** \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="a8527-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="a8527-160">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="a8527-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="a8527-161">***-Provider*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="a8527-162">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="a8527-162">The provider to use.</span></span> <span data-ttu-id="a8527-163">(na przykład Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a8527-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="a8527-164">-OutputDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="a8527-165">Katalog, który można umieścić pliki w.</span><span class="sxs-lookup"><span data-stu-id="a8527-165">The directory to put files in.</span></span> <span data-ttu-id="a8527-166">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a8527-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="a8527-167">-ContextDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="a8527-168">Katalog, który można umieścić plik typu DbContext w.</span><span class="sxs-lookup"><span data-stu-id="a8527-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="a8527-169">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a8527-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a8527-170">-Kontekstu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-170">-Context \<String></span></span>                       | <span data-ttu-id="a8527-171">Nazwa typu DbContext do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="a8527-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="a8527-172">-Schematów \<String [] ></span><span class="sxs-lookup"><span data-stu-id="a8527-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="a8527-173">Schematów tabel w celu wygenerowania typów jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="a8527-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a8527-174">-Tabele \<String [] ></span><span class="sxs-lookup"><span data-stu-id="a8527-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="a8527-175">Tabele, aby wygenerować typy jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="a8527-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="a8527-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="a8527-176">-DataAnnotations</span></span>                         | <span data-ttu-id="a8527-177">Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a8527-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a8527-178">W przypadku pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="a8527-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a8527-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="a8527-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="a8527-180">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8527-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="a8527-181">-Force</span><span class="sxs-lookup"><span data-stu-id="a8527-181">-Force</span></span>                                   | <span data-ttu-id="a8527-182">Nadpisz istniejące pliki.</span><span class="sxs-lookup"><span data-stu-id="a8527-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="a8527-183">Skrypt migracji</span><span class="sxs-lookup"><span data-stu-id="a8527-183">Script-Migration</span></span>

<span data-ttu-id="a8527-184">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a8527-185">Parametry:</span><span class="sxs-lookup"><span data-stu-id="a8527-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a8527-186">*-From* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-186">*-From* \<String></span></span> | <span data-ttu-id="a8527-187">Począwszy od migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-187">The starting migration.</span></span> <span data-ttu-id="a8527-188">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="a8527-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="a8527-189">*— Do* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-189">*-To* \<String></span></span>   | <span data-ttu-id="a8527-190">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-190">The ending migration.</span></span> <span data-ttu-id="a8527-191">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="a8527-192">-Idempotentne</span><span class="sxs-lookup"><span data-stu-id="a8527-192">-Idempotent</span></span>       | <span data-ttu-id="a8527-193">Generowanie skryptu, który może służyć w bazie danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="a8527-194">-Dane wyjściowe \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="a8527-194">-Output \<String></span></span> | <span data-ttu-id="a8527-195">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="a8527-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="a8527-196">To, z danych wyjściowych z obsługą i parametrów rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="a8527-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="a8527-197">Aktualizuj bazy danych</span><span class="sxs-lookup"><span data-stu-id="a8527-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="a8527-198"><nobr>*— Migracja* \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="a8527-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="a8527-199">Migracja docelowego.</span><span class="sxs-lookup"><span data-stu-id="a8527-199">The target migration.</span></span> <span data-ttu-id="a8527-200">Jeśli jest to "0", będzie można przywrócić wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="a8527-201">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="a8527-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="a8527-202">Parametr migracji obsługuje rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="a8527-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
