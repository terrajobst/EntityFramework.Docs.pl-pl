---
title: Narzędzia & rozszerzenia — EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502085"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="d2e3c-102">Rozszerzenia narzędzi EF Core &</span><span class="sxs-lookup"><span data-stu-id="d2e3c-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="d2e3c-103">Te narzędzia i rozszerzenia zapewniają dodatkową funkcjonalność dla Entity Framework Core 2,1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d2e3c-104">Rozszerzenia są tworzone przez różne źródła i nie są obsługiwane w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="d2e3c-105">Biorąc pod uwagę rozszerzenie innej firmy, pamiętaj o ocenie jego jakości, licencjonowania, zgodności, wsparcia itp., aby upewnić się, że spełnia Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="d2e3c-106">W szczególności rozszerzenie skompilowane dla starszej wersji EF Core może wymagać aktualizacji, zanim będzie działały z najnowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="d2e3c-107">Narzędzia</span><span class="sxs-lookup"><span data-stu-id="d2e3c-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="d2e3c-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="d2e3c-108">LLBLGen Pro</span></span>

<span data-ttu-id="d2e3c-109">LLBLGen Pro to rozwiązanie do modelowania jednostek z obsługą Entity Framework i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="d2e3c-110">Umożliwia ona łatwe definiowanie modelu jednostki i mapowanie go do bazy danych przy użyciu najpierw pierwszej lub modelu bazy danych, dzięki czemu możesz od razu zacząć pisać zapytania.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="d2e3c-111">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-111">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-112">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d2e3c-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="d2e3c-113">Deweloper jednostki Devart</span><span class="sxs-lookup"><span data-stu-id="d2e3c-113">Devart Entity Developer</span></span>

<span data-ttu-id="d2e3c-114">Deweloper jednostki jest zaawansowanym projektantem ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="d2e3c-115">Obsługuje ona projektowanie EF Core modeli wizualnie, przy użyciu pierwszej metody modelu lub pierwszej podejścia do C# bazy danych i lub Visual Basic generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="d2e3c-116">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-116">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-117">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d2e3c-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="d2e3c-118">EF Core narzędzia do zarządzania</span><span class="sxs-lookup"><span data-stu-id="d2e3c-118">EF Core Power Tools</span></span>

<span data-ttu-id="d2e3c-119">EF Core PowerShell to rozszerzenie programu Visual Studio, które uwidacznia różne EF Core zadania czasu projektowania w prostym interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="d2e3c-120">Obejmuje ona odtwarzanie klas DbContext i Entity Classes z istniejących baz danych, a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), zarządzanie migracjami baz danych i wizualizacje modeli.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="d2e3c-121">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-122">Witryna typu wiki usługi GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="d2e3c-123">Entity Framework edytor wizualny</span><span class="sxs-lookup"><span data-stu-id="d2e3c-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="d2e3c-124">Entity Framework edytorem wizualnym jest rozszerzenie programu Visual Studio, które dodaje projektanta ORM do projektowania wizualizacji Dr 6 i klasy EF Core.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="d2e3c-125">Kod jest generowany przy użyciu szablonów T4, więc można go dostosować do własnych potrzeb.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="d2e3c-126">Obsługuje dziedziczenie, dwukierunkowe i dwukierunkowe skojarzenia, wyliczenia oraz możliwość kolorowania kodu klas i Dodawanie bloków tekstowych, aby wyjaśnić potencjalnie specjalne części projektu.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="d2e3c-127">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-127">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="d2e3c-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="d2e3c-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="d2e3c-129">CatFactory</span></span>

<span data-ttu-id="d2e3c-130">CatFactory to aparat tworzenia szkieletów dla platformy .NET Core, który umożliwia automatyzację generacji klas DbContext, jednostek, konfiguracji mapowania i klas repozytorium z bazy danych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="d2e3c-131">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-131">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-132">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="d2e3c-133">Generator Entity Framework Core LoreSoft</span><span class="sxs-lookup"><span data-stu-id="d2e3c-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="d2e3c-134">Generator Entity Framework Core (EFG) to narzędzie interfejs wiersza polecenia platformy .NET Core, które może generować modele EF Core z istniejącej bazy danych, podobnie jak `dotnet ef dbcontext scaffold`, ale również zapewnia bezpieczną [regenerację](https://efg.loresoft.com/en/latest/regeneration/) kodu przez zastąpienie regionu lub analizowanie plików mapowania.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="d2e3c-135">To narzędzie obsługuje generowanie modeli widoku, walidacji i kodu mapowania obiektów.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="d2e3c-136">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-136">For EF Core: 2.</span></span>

<span data-ttu-id="d2e3c-137">[Samouczek](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentacja](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="d2e3c-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="d2e3c-138">Rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="d2e3c-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="d2e3c-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="d2e3c-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="d2e3c-140">Biblioteka wtyczek, która umożliwia automatyczne rejestrowanie zmian danych wykonywanych przez EF Core w tabeli historii.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="d2e3c-141">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-141">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-142">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="d2e3c-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="d2e3c-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="d2e3c-144">Rozszerzenie, które umożliwia przechowywanie wyników zapytań EF Core w pamięci podręcznej drugiego poziomu, tak aby kolejne wykonania tych samych zapytań mogły uniknąć dostępu do bazy danych i pobierać dane bezpośrednio z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="d2e3c-145">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-145">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-146">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="d2e3c-147">Geco</span><span class="sxs-lookup"><span data-stu-id="d2e3c-147">Geco</span></span>

<span data-ttu-id="d2e3c-148">Geco (konsola generatora) to prosty generator kodu oparty na projekcie konsoli, który działa na platformie .NET Core i używa C# interpolowanych ciągów do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="d2e3c-149">Geco obejmuje generator modelu Odwróć dla EF Core z obsługą szablonów pluralizacja, singularization i edytowalnych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="d2e3c-150">Udostępnia również Generator skryptów danych inicjatora, moduł uruchamiający skrypty i oczyszczarkę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="d2e3c-151">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-151">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-152">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="d2e3c-153">EntityFrameworkCore. Tworzenie szkieletów. kierownicy</span><span class="sxs-lookup"><span data-stu-id="d2e3c-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="d2e3c-154">Umożliwia dostosowanie klas odtworzonych z istniejącej bazy danych przy użyciu Entity Framework Core łańcucha narzędzi z szablonami kierownicy.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="d2e3c-155">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-156">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="d2e3c-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="d2e3c-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="d2e3c-158">NeinLinq rozszerza dostawców LINQ, takich jak Entity Framework, aby włączyć ponowne używanie funkcji, zapisywania zapytań i kompilowania zapytań dynamicznych przy użyciu predykatów z możliwością tłumaczenia i selektorów.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="d2e3c-159">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-160">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="d2e3c-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="d2e3c-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="d2e3c-162">Wtyczka dla elementu Microsoft. EntityFrameworkCore do obsługi repozytorium, wzorców jednostek roboczych i wielu baz danych z obsługiwaną transakcją rozproszoną.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="d2e3c-163">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-163">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-164">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="d2e3c-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="d2e3c-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="d2e3c-166">Rozszerzenia EF Core dla operacji zbiorczych (INSERT, Update i Delete).</span><span class="sxs-lookup"><span data-stu-id="d2e3c-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="d2e3c-167">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-168">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="d2e3c-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="d2e3c-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="d2e3c-170">Dodaje pluralizacja czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-170">Adds design-time pluralization.</span></span> <span data-ttu-id="d2e3c-171">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-171">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-172">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="d2e3c-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="d2e3c-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="d2e3c-174">Revival [index] atrybut (z rozszerzeniem dla kompilowania modelu).</span><span class="sxs-lookup"><span data-stu-id="d2e3c-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="d2e3c-175">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-176">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="d2e3c-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="d2e3c-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="d2e3c-178">Zawiera otokę wokół EF Core dostawcy bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="d2e3c-179">Sprawia, że działa tak samo jak dostawca relacyjny.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="d2e3c-180">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-180">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-181">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="d2e3c-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="d2e3c-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="d2e3c-183">Implementacja obsługi danych czasowych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-183">An implementation of temporal support.</span></span> <span data-ttu-id="d2e3c-184">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-184">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-185">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="d2e3c-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="d2e3c-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="d2e3c-187">Łatwe wykonywanie zapytań czasowych w ulubionej bazie danych przy użyciu wprowadzonych metod rozszerzających: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="d2e3c-188">Dla EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-188">For EF Core: 3.</span></span>

[<span data-ttu-id="d2e3c-189">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="d2e3c-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="d2e3c-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="d2e3c-191">Zezwalaj na w pełni funkcjonalne zapytania Entity Framework Core w odniesieniu do [SQL Server historii](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) danych czasowych przy użyciu zdefiniowanego w EF Core kodu, jednostek i mapowań.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="d2e3c-192">Poruszaj się po czasie, zawijając kod w `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="d2e3c-193">Dla EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-193">For EF Core: 3.</span></span>

[<span data-ttu-id="d2e3c-194">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="d2e3c-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="d2e3c-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="d2e3c-196">Biblioteka rozszerzeń dla Entity Framework Core, która umożliwia deweloperom, którzy używają SQL Server do łatwego korzystania z tabel danych czasowych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="d2e3c-197">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-197">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-198">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="d2e3c-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="d2e3c-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="d2e3c-200">Pamięć podręczna zapytań o wysokiej wydajności.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="d2e3c-201">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-201">For EF Core: 2.</span></span>

[<span data-ttu-id="d2e3c-202">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="d2e3c-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="d2e3c-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="d2e3c-203">Entity Framework Plus</span></span>

<span data-ttu-id="d2e3c-204">Rozszerza kontekst DbContext z funkcjami takimi jak: Filter include, Audit, buforowanie, Future Query, Batch Delete, Batch Update i innych.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="d2e3c-205">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="d2e3c-206">[Witryna internetowa](https://entityframework-plus.net/)
[repozytorium GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="d2e3c-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="d2e3c-207">Rozszerzenia Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d2e3c-207">Entity Framework Extensions</span></span>

<span data-ttu-id="d2e3c-208">Rozszerza swój kontekst dbwith operacji zbiorczych o wysokiej wydajności: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge i inne.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="d2e3c-209">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d2e3c-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d2e3c-210">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d2e3c-210">Website</span></span>](https://entityframework-extensions.net/)
