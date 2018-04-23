---
title: Konsola Menedżera pakietów (Visual Studio) — podstawowe EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="72d16-102">Narzędzia konsoli Menedżera pakietów Core EF</span><span class="sxs-lookup"><span data-stu-id="72d16-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="72d16-103">Uruchom narzędzia konsoli Menedżera pakietów EF Core (PMC) w programie Visual Studio przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="72d16-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="72d16-104">Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="72d16-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="72d16-105">Nie używasz programu Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="72d16-105">Not using Visual Studio?</span></span> <span data-ttu-id="72d16-106">[EF podstawowe narzędzia wiersza polecenia] [ 1] są międzyplatformowa i uruchamiane w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="72d16-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="72d16-107">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="72d16-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="72d16-108">Zainstaluj narzędzia konsoli Menedżera pakietów Core EF, instalując pakiet Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="72d16-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="72d16-109">Można ją zainstalować, wykonując następujące polecenie w [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="72d16-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="72d16-110">Jeśli wszystko działało poprawnie, należy uruchomić to polecenie:</span><span class="sxs-lookup"><span data-stu-id="72d16-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="72d16-111">Jeśli Twój projekt startowy jest przeznaczony dla platformy .NET Standard [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia.</span><span class="sxs-lookup"><span data-stu-id="72d16-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72d16-112">Jeśli używasz **uniwersalnych systemu Windows** lub **Xamarin**, Przenieś swój kod EF do .NET Standard biblioteki klas i [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia.</span><span class="sxs-lookup"><span data-stu-id="72d16-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="72d16-113">Określ biblioteki klas jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="72d16-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="72d16-114">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="72d16-114">Using the tools</span></span>
---------------
<span data-ttu-id="72d16-115">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="72d16-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="72d16-116">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="72d16-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="72d16-117">Domyślnie projektu docelowego **domyślny projekt** wybrany w konsoli Menedżera pakietów, ale można również określić za pomocą parametru - projektu.</span><span class="sxs-lookup"><span data-stu-id="72d16-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="72d16-118">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="72d16-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="72d16-119">Domyślnie jedną **Ustaw jako projekt startowy** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="72d16-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="72d16-120">Można również określić, za pomocą parametru - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="72d16-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="72d16-121">Wspólne parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="72d16-122">-Kontekst \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-122">-Context \<String></span></span>        | <span data-ttu-id="72d16-123">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="72d16-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="72d16-124">-Projektu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-124">-Project \<String></span></span>        | <span data-ttu-id="72d16-125">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="72d16-125">The project to use.</span></span>         |
| <span data-ttu-id="72d16-126">-StartupProject \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-126">-StartupProject \<String></span></span> | <span data-ttu-id="72d16-127">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="72d16-127">The startup project to use.</span></span> |
| <span data-ttu-id="72d16-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="72d16-128">-Verbose</span></span>                  | <span data-ttu-id="72d16-129">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="72d16-129">Show verbose output.</span></span>        |

<span data-ttu-id="72d16-130">Aby wyświetlić Pomoc informacje dotyczące polecenia, za pomocą programu PowerShell w `Get-Help` polecenia.</span><span class="sxs-lookup"><span data-stu-id="72d16-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="72d16-131">Parametry kontekstu, projektu i StartupProject obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="72d16-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="72d16-132">Ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem do określenia środowiska ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72d16-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="72d16-133">Polecenia</span><span class="sxs-lookup"><span data-stu-id="72d16-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="72d16-134">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="72d16-134">Add-Migration</span></span>

<span data-ttu-id="72d16-135">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-135">Adds a new migration.</span></span>

<span data-ttu-id="72d16-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72d16-137">***-Nazwa*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-137">***-Name*** \<String></span></span>             | <span data-ttu-id="72d16-138">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="72d16-139"><nobr>-OutputDir \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="72d16-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="72d16-140">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="72d16-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="72d16-141">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="72d16-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="72d16-142">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="72d16-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="72d16-143">Parametry w **bold** są wymagane i w *italics* są pozycyjnych.</span><span class="sxs-lookup"><span data-stu-id="72d16-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="72d16-144">Baza danych listy</span><span class="sxs-lookup"><span data-stu-id="72d16-144">Drop-Database</span></span>

<span data-ttu-id="72d16-145">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72d16-145">Drops the database.</span></span>

<span data-ttu-id="72d16-146">Parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="72d16-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="72d16-147">-WhatIf</span></span> | <span data-ttu-id="72d16-148">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="72d16-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="72d16-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="72d16-149">Get-DbContext</span></span>

<span data-ttu-id="72d16-150">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="72d16-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="72d16-151">Usuń migracji</span><span class="sxs-lookup"><span data-stu-id="72d16-151">Remove-Migration</span></span>

<span data-ttu-id="72d16-152">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-152">Removes the last migration.</span></span>

<span data-ttu-id="72d16-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="72d16-154">-Force.</span><span class="sxs-lookup"><span data-stu-id="72d16-154">-Force</span></span> | <span data-ttu-id="72d16-155">Przywróć migracji, jeśli została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72d16-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="72d16-156">Szkieletu DbContext</span><span class="sxs-lookup"><span data-stu-id="72d16-156">Scaffold-DbContext</span></span>

<span data-ttu-id="72d16-157">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72d16-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="72d16-158">Parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72d16-159"><nobr>***-Połączenia*** \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="72d16-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="72d16-160">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="72d16-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="72d16-161">***-Dostawca*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="72d16-162">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="72d16-162">The provider to use.</span></span> <span data-ttu-id="72d16-163">(Np.</span><span class="sxs-lookup"><span data-stu-id="72d16-163">(E.g.</span></span> <span data-ttu-id="72d16-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="72d16-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="72d16-165">-OutputDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="72d16-166">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="72d16-166">The directory to put files in.</span></span> <span data-ttu-id="72d16-167">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="72d16-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="72d16-168">-ContextDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="72d16-169">Katalog mają zostać umieszczone w pliku DbContext.</span><span class="sxs-lookup"><span data-stu-id="72d16-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="72d16-170">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="72d16-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="72d16-171">-Kontekst \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-171">-Context \<String></span></span>                       | <span data-ttu-id="72d16-172">Nazwa typu DbContext w celu wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="72d16-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="72d16-173">-Schematy \<String [] ></span><span class="sxs-lookup"><span data-stu-id="72d16-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="72d16-174">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="72d16-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="72d16-175">-Tabele \<String [] ></span><span class="sxs-lookup"><span data-stu-id="72d16-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="72d16-176">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="72d16-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="72d16-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="72d16-177">-DataAnnotations</span></span>                         | <span data-ttu-id="72d16-178">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="72d16-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="72d16-179">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="72d16-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="72d16-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="72d16-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="72d16-181">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72d16-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="72d16-182">-Force.</span><span class="sxs-lookup"><span data-stu-id="72d16-182">-Force</span></span>                                   | <span data-ttu-id="72d16-183">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="72d16-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="72d16-184">Skrypt migracji</span><span class="sxs-lookup"><span data-stu-id="72d16-184">Script-Migration</span></span>

<span data-ttu-id="72d16-185">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="72d16-186">Parametry:</span><span class="sxs-lookup"><span data-stu-id="72d16-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="72d16-187">*-From* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-187">*-From* \<String></span></span> | <span data-ttu-id="72d16-188">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-188">The starting migration.</span></span> <span data-ttu-id="72d16-189">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="72d16-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="72d16-190">*— Do* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-190">*-To* \<String></span></span>   | <span data-ttu-id="72d16-191">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-191">The ending migration.</span></span> <span data-ttu-id="72d16-192">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="72d16-193">-Idempotentności</span><span class="sxs-lookup"><span data-stu-id="72d16-193">-Idempotent</span></span>       | <span data-ttu-id="72d16-194">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="72d16-195">-Output \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="72d16-195">-Output \<String></span></span> | <span data-ttu-id="72d16-196">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="72d16-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="72d16-197">Do z, i parametry wyjściowe obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="72d16-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="72d16-198">Update-Database</span><span class="sxs-lookup"><span data-stu-id="72d16-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="72d16-199"><nobr>*-Migration* \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="72d16-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="72d16-200">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="72d16-200">The target migration.</span></span> <span data-ttu-id="72d16-201">W przypadku wartości "0" wszystkich migracji zostaną cofnięte.</span><span class="sxs-lookup"><span data-stu-id="72d16-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="72d16-202">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="72d16-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="72d16-203">Parametr migracji obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="72d16-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
