---
title: Konsola Menedżera pakietów (Visual Studio) — podstawowe EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="5d546-102">Narzędzia konsoli Menedżera pakietów Core EF</span><span class="sxs-lookup"><span data-stu-id="5d546-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="5d546-103">Uruchom narzędzia konsoli Menedżera pakietów EF Core (PMC) w programie Visual Studio przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="5d546-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="5d546-104">Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d546-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="5d546-105">Nie używasz programu Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="5d546-105">Not using Visual Studio?</span></span> <span data-ttu-id="5d546-106">[EF podstawowe narzędzia wiersza polecenia] [ 1] są międzyplatformowa i uruchamiane w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="5d546-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="5d546-107">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="5d546-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="5d546-108">Zainstaluj narzędzia konsoli Menedżera pakietów Core EF, instalując pakiet Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d546-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="5d546-109">Można ją zainstalować, wykonując następujące polecenie w [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="5d546-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="5d546-110">Jeśli wszystko działało poprawnie, należy uruchomić to polecenie:</span><span class="sxs-lookup"><span data-stu-id="5d546-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="5d546-111">Jeśli Twój projekt startowy jest przeznaczony dla platformy .NET Standard [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia.</span><span class="sxs-lookup"><span data-stu-id="5d546-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d546-112">Jeśli używasz **uniwersalnych systemu Windows** lub **Xamarin**, Przenieś swój kod EF do .NET Standard biblioteki klas i [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia.</span><span class="sxs-lookup"><span data-stu-id="5d546-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="5d546-113">Określ biblioteki klas jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="5d546-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="5d546-114">Korzystając z narzędzi</span><span class="sxs-lookup"><span data-stu-id="5d546-114">Using the tools</span></span>
---------------
<span data-ttu-id="5d546-115">Przy każdym wywołaniu polecenia obejmuje dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="5d546-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="5d546-116">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="5d546-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="5d546-117">Domyślnie projektu docelowego **domyślny projekt** wybrany w konsoli Menedżera pakietów, ale można również określić za pomocą parametru - projektu.</span><span class="sxs-lookup"><span data-stu-id="5d546-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="5d546-118">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="5d546-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="5d546-119">Domyślnie jedną **Ustaw jako projekt startowy** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="5d546-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="5d546-120">Można również określić, za pomocą parametru - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="5d546-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="5d546-121">Wspólne parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="5d546-122">-Kontekst \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-122">-Context \<String></span></span>        | <span data-ttu-id="5d546-123">DbContext do użycia.</span><span class="sxs-lookup"><span data-stu-id="5d546-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="5d546-124">-Projektu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-124">-Project \<String></span></span>        | <span data-ttu-id="5d546-125">Projekt do użycia.</span><span class="sxs-lookup"><span data-stu-id="5d546-125">The project to use.</span></span>         |
| <span data-ttu-id="5d546-126">-StartupProject \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-126">-StartupProject \<String></span></span> | <span data-ttu-id="5d546-127">Projekt startowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="5d546-127">The startup project to use.</span></span> |
| <span data-ttu-id="5d546-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="5d546-128">-Verbose</span></span>                  | <span data-ttu-id="5d546-129">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="5d546-129">Show verbose output.</span></span>        |

<span data-ttu-id="5d546-130">Aby wyświetlić Pomoc informacje dotyczące polecenia, za pomocą programu PowerShell w `Get-Help` polecenia.</span><span class="sxs-lookup"><span data-stu-id="5d546-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="5d546-131">Parametry kontekstu, projektu i StartupProject obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5d546-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="5d546-132">Ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem do określenia środowiska ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d546-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="5d546-133">Polecenia</span><span class="sxs-lookup"><span data-stu-id="5d546-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="5d546-134">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="5d546-134">Add-Migration</span></span>

<span data-ttu-id="5d546-135">Dodaje nowe migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-135">Adds a new migration.</span></span>

<span data-ttu-id="5d546-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5d546-137">***-Nazwa*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-137">***-Name*** \<String></span></span>             | <span data-ttu-id="5d546-138">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="5d546-139"><nobr>-OutputDir \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="5d546-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="5d546-140">Katalog (i przestrzeni nazw sub) do użycia.</span><span class="sxs-lookup"><span data-stu-id="5d546-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="5d546-141">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="5d546-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="5d546-142">Wartość domyślna to "Migracji".</span><span class="sxs-lookup"><span data-stu-id="5d546-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="5d546-143">Parametry w **bold** są wymagane i w *italics* są pozycyjnych.</span><span class="sxs-lookup"><span data-stu-id="5d546-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="5d546-144">Baza danych listy</span><span class="sxs-lookup"><span data-stu-id="5d546-144">Drop-Database</span></span>

<span data-ttu-id="5d546-145">Odrzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5d546-145">Drops the database.</span></span>

<span data-ttu-id="5d546-146">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="5d546-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="5d546-147">-WhatIf</span></span> | <span data-ttu-id="5d546-148">Pokaż bazę danych, która będą pomijane, ale nie jej porzucić.</span><span class="sxs-lookup"><span data-stu-id="5d546-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="5d546-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="5d546-149">Get-DbContext</span></span>

<span data-ttu-id="5d546-150">Pobiera informacje o typie DbContext.</span><span class="sxs-lookup"><span data-stu-id="5d546-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="5d546-151">Usuń migracji</span><span class="sxs-lookup"><span data-stu-id="5d546-151">Remove-Migration</span></span>

<span data-ttu-id="5d546-152">Usuwa ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-152">Removes the last migration.</span></span>

<span data-ttu-id="5d546-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="5d546-154">-Force.</span><span class="sxs-lookup"><span data-stu-id="5d546-154">-Force</span></span> | <span data-ttu-id="5d546-155">Przywróć migracji, jeśli została zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5d546-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="5d546-156">Szkieletu DbContext</span><span class="sxs-lookup"><span data-stu-id="5d546-156">Scaffold-DbContext</span></span>

<span data-ttu-id="5d546-157">Rusztowania DbContext i jednostki typy dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5d546-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="5d546-158">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5d546-159"><nobr>***-Połączenia*** \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="5d546-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="5d546-160">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5d546-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="5d546-161">***-Dostawca*** \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="5d546-162">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="5d546-162">The provider to use.</span></span> <span data-ttu-id="5d546-163">(Np.</span><span class="sxs-lookup"><span data-stu-id="5d546-163">(E.g.</span></span> <span data-ttu-id="5d546-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="5d546-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="5d546-165">-OutputDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="5d546-166">Umieścić pliki katalogu.</span><span class="sxs-lookup"><span data-stu-id="5d546-166">The directory to put files in.</span></span> <span data-ttu-id="5d546-167">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="5d546-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="5d546-168">-ContextDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="5d546-169">Katalog mają zostać umieszczone w pliku DbContext.</span><span class="sxs-lookup"><span data-stu-id="5d546-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="5d546-170">Ścieżki są względem katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="5d546-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="5d546-171">-Kontekst \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-171">-Context \<String></span></span>                       | <span data-ttu-id="5d546-172">Nazwa typu DbContext w celu wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="5d546-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="5d546-173">-Schematy \<String [] ></span><span class="sxs-lookup"><span data-stu-id="5d546-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="5d546-174">Schematy tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="5d546-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="5d546-175">-Tabele \<String [] ></span><span class="sxs-lookup"><span data-stu-id="5d546-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="5d546-176">Tabele, aby wygenerować typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="5d546-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="5d546-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="5d546-177">-DataAnnotations</span></span>                         | <span data-ttu-id="5d546-178">Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="5d546-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="5d546-179">Przypadku jego pominięcia jest używana tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="5d546-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="5d546-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="5d546-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="5d546-181">Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5d546-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="5d546-182">-Force.</span><span class="sxs-lookup"><span data-stu-id="5d546-182">-Force</span></span>                                   | <span data-ttu-id="5d546-183">Zastąpienie istniejących plików.</span><span class="sxs-lookup"><span data-stu-id="5d546-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="5d546-184">Skrypt migracji</span><span class="sxs-lookup"><span data-stu-id="5d546-184">Script-Migration</span></span>

<span data-ttu-id="5d546-185">Generuje skrypt SQL z migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="5d546-186">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5d546-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="5d546-187">*-From* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-187">*-From* \<String></span></span> | <span data-ttu-id="5d546-188">Początkowy migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-188">The starting migration.</span></span> <span data-ttu-id="5d546-189">Wartość domyślna to 0 (początkowej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="5d546-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="5d546-190">*— Do* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-190">*-To* \<String></span></span>   | <span data-ttu-id="5d546-191">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-191">The ending migration.</span></span> <span data-ttu-id="5d546-192">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="5d546-193">-Idempotentności</span><span class="sxs-lookup"><span data-stu-id="5d546-193">-Idempotent</span></span>       | <span data-ttu-id="5d546-194">Generuj skrypt, który może być używany z bazy danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="5d546-195">-Output \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="5d546-195">-Output \<String></span></span> | <span data-ttu-id="5d546-196">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="5d546-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="5d546-197">Do z, i parametry wyjściowe obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5d546-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="5d546-198">Update-Database</span><span class="sxs-lookup"><span data-stu-id="5d546-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="5d546-199"><nobr>*-Migration* \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="5d546-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="5d546-200">Migracja docelowych.</span><span class="sxs-lookup"><span data-stu-id="5d546-200">The target migration.</span></span> <span data-ttu-id="5d546-201">W przypadku wartości "0" wszystkich migracji zostaną cofnięte.</span><span class="sxs-lookup"><span data-stu-id="5d546-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="5d546-202">Domyślnie ostatni migracji.</span><span class="sxs-lookup"><span data-stu-id="5d546-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="5d546-203">Parametr migracji obsługuje kartę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5d546-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
