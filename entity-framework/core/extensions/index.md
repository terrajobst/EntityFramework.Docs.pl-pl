---
title: Narzędzia & rozszerzenia - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634241"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="26860-102">EF Core Tools & rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="26860-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="26860-103">Te narzędzia i rozszerzenia zapewniają dodatkowe funkcje dla entity framework core 2.1 i nowsze.</span><span class="sxs-lookup"><span data-stu-id="26860-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="26860-104">Rozszerzenia są tworzone przez różne źródła i nie są obsługiwane w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="26860-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="26860-105">Rozważając rozszerzenie innej firmy, pamiętaj, aby ocenić jego jakość, licencjonowanie, kompatybilność, wsparcie itp., aby upewnić się, że spełnia Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="26860-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="26860-106">W szczególności rozszerzenie utworzone dla starszej wersji ef core może wymagać aktualizacji, zanim będzie działać z najnowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="26860-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="26860-107">Narzędzia</span><span class="sxs-lookup"><span data-stu-id="26860-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="26860-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="26860-108">LLBLGen Pro</span></span>

<span data-ttu-id="26860-109">LLBLGen Pro to rozwiązanie do modelowania jednostek z obsługą entity framework i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="26860-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="26860-110">Umożliwia łatwe definiowanie modelu jednostki i mapowanie go do bazy danych, najpierw przy użyciu bazy danych lub modelu, dzięki czemu można rozpocząć pisanie zapytań od razu.</span><span class="sxs-lookup"><span data-stu-id="26860-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="26860-111">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-111">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-112">witryna sieci web</span><span class="sxs-lookup"><span data-stu-id="26860-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="26860-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="26860-113">Devart Entity Developer</span></span>

<span data-ttu-id="26860-114">Entity Developer jest zaawansowanym projektantem ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ do SQL.</span><span class="sxs-lookup"><span data-stu-id="26860-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="26860-115">Obsługuje projektowanie modeli EF Core wizualnie, przy użyciu modelu pierwszy lub bazy danych pierwsze podejścia i C# lub Visual Basic generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="26860-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="26860-116">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-116">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-117">witryna sieci web</span><span class="sxs-lookup"><span data-stu-id="26860-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="26860-118">nHydrate ORM for Entity Framework</span><span class="sxs-lookup"><span data-stu-id="26860-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="26860-119">Orm, który tworzy silnie typizowane, rozszerzalne klasy dla entity framework.</span><span class="sxs-lookup"><span data-stu-id="26860-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="26860-120">Wygenerowany kod to Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="26860-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="26860-121">Nie ma żadnej różnicy.</span><span class="sxs-lookup"><span data-stu-id="26860-121">There is no difference.</span></span> <span data-ttu-id="26860-122">Nie jest to zamiennik ef lub niestandardowego ORM.</span><span class="sxs-lookup"><span data-stu-id="26860-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="26860-123">Jest to warstwa wizualna, modelowania, która umożliwia zespołowi zarządzanie złożonymi schematami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="26860-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="26860-124">Dobrze współpracuje z oprogramowaniem SCM, takim jak Git, umożliwiając dostęp wielu użytkowników do modelu przy minimalnych konfliktach.</span><span class="sxs-lookup"><span data-stu-id="26860-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="26860-125">Instalator śledzi zmiany modelu i tworzy skrypty uaktualnienia.</span><span class="sxs-lookup"><span data-stu-id="26860-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="26860-126">Dla EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="26860-126">For EF Core: 3.</span></span>

[<span data-ttu-id="26860-127">Strona Github</span><span class="sxs-lookup"><span data-stu-id="26860-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="26860-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="26860-128">EF Core Power Tools</span></span>

<span data-ttu-id="26860-129">EF Core Power Tools to rozszerzenie programu Visual Studio, które udostępnia różne zadania ef core projektowania w prostym interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="26860-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="26860-130">Obejmuje inżynierię odwrotną DbContext i klasy jednostek z istniejących baz danych i [DACPAc programu SQL Server,](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)zarządzanie migracjami baz danych i wizualizacje modelu.</span><span class="sxs-lookup"><span data-stu-id="26860-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="26860-131">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-132">Wiki GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="26860-133">Edytor wizualny programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="26860-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="26860-134">Edytor wizualny programu Entity Framework to rozszerzenie programu Visual Studio, które dodaje projektanta ORM do projektowania wizualnego klas EF 6 i EF Core.</span><span class="sxs-lookup"><span data-stu-id="26860-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="26860-135">Kod jest generowany przy użyciu szablonów T4, dzięki czemu można dostosować do każdego celu.</span><span class="sxs-lookup"><span data-stu-id="26860-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="26860-136">Obsługuje dziedziczenie, jednokierunkowe i dwukierunkowe skojarzenia, wyliczenia i możliwość kolor kodowania klas i dodawać bloki tekstu, aby wyjaśnić potencjalnie tajemne części projektu.</span><span class="sxs-lookup"><span data-stu-id="26860-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="26860-137">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-137">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-138">Rynek</span><span class="sxs-lookup"><span data-stu-id="26860-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="26860-139">Fabryka Kotów</span><span class="sxs-lookup"><span data-stu-id="26860-139">CatFactory</span></span>

<span data-ttu-id="26860-140">CatFactory to aparat szkieletów dla platformy .NET Core, który może zautomatyzować generowanie klas DbContext, jednostek, konfiguracji mapowania i klas repozytorium z bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26860-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="26860-141">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-141">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-142">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="26860-143">Generator rdzenia entity Framework firmy LoreSoft</span><span class="sxs-lookup"><span data-stu-id="26860-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="26860-144">Entity Framework Core Generator (efg) to narzędzie .NET Core CLI, które może `dotnet ef dbcontext scaffold`generować modele EF Core z istniejącej bazy danych, podobnie jak , ale obsługuje również [bezpieczną regenerację](https://efg.loresoft.com/en/latest/regeneration/) kodu poprzez zastępowanie regionu lub analizowanie plików mapowania.</span><span class="sxs-lookup"><span data-stu-id="26860-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="26860-145">To narzędzie obsługuje generowanie modeli widoku, sprawdzania poprawności i kodu mapera obiektów.</span><span class="sxs-lookup"><span data-stu-id="26860-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="26860-146">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-146">For EF Core: 2.</span></span>

<span data-ttu-id="26860-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentacja samouczka](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="26860-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="26860-148">Rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="26860-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="26860-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="26860-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="26860-150">Biblioteka wtyczek, która umożliwia automatyczne rejestrowanie zmian danych wykonywanych przez EF Core w tabeli historii.</span><span class="sxs-lookup"><span data-stu-id="26860-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="26860-151">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-151">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-152">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="26860-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="26860-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="26860-154">Rozszerzenie, które umożliwia przechowywanie wyników zapytań EF Core w pamięci podręcznej drugiego poziomu, dzięki czemu kolejne wykonanie tych samych kwerend można uniknąć dostępu do bazy danych i pobrać dane bezpośrednio z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="26860-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="26860-155">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-155">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-156">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="26860-157">Geco</span><span class="sxs-lookup"><span data-stu-id="26860-157">Geco</span></span>

<span data-ttu-id="26860-158">Geco (Generator Console) to prosty generator kodu oparty na projekcie konsoli, który działa na .NET Core i używa interpolowanych ciągów C# do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="26860-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="26860-159">Geco zawiera generator modelu wstecznego dla EF Core z obsługą pluralizacji, singularization i edytowalnych szablonów.</span><span class="sxs-lookup"><span data-stu-id="26860-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="26860-160">Zapewnia również generator skryptów danych źródłowych, moduł przesiewowy skryptów i czyszczenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="26860-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="26860-161">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-161">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-162">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="26860-163">EntityFrameworkCore.Scaffolding.Kierownica</span><span class="sxs-lookup"><span data-stu-id="26860-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="26860-164">Umożliwia dostosowanie klas inżynierii odwrotnej z istniejącej bazy danych przy użyciu programu Entity Framework Core toolchain z szablonami kierownicy.</span><span class="sxs-lookup"><span data-stu-id="26860-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="26860-165">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-166">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="26860-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="26860-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="26860-168">NeinLinq rozszerza dostawców LINQ, takich jak Entity Framework, aby umożliwić ponowne użycie funkcji, przepisywanie zapytań i tworzenie zapytań dynamicznych przy użyciu tłumaczonych predykatów i selektorów.</span><span class="sxs-lookup"><span data-stu-id="26860-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="26860-169">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-170">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="26860-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="26860-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="26860-172">Wtyczka dla Microsoft.EntityFrameworkCore do obsługi repozytorium, jednostki wzorców pracy i wielu baz danych z transakcjami rozproszonymi obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="26860-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="26860-173">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-173">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-174">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="26860-175">EFCore.BulkWybory</span><span class="sxs-lookup"><span data-stu-id="26860-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="26860-176">Rozszerzenia EF Core dla operacji zbiorczych (Wstaw, Aktualizacja, Usuń).</span><span class="sxs-lookup"><span data-stu-id="26860-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="26860-177">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-178">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="26860-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="26860-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="26860-180">Dodaje pluralizm w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="26860-180">Adds design-time pluralization.</span></span> <span data-ttu-id="26860-181">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-181">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-182">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="26860-183">Atrybut toolbelt.entityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="26860-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="26860-184">Odrodzenie atrybutu [Index] (z rozszerzeniem dla budynku modelu).</span><span class="sxs-lookup"><span data-stu-id="26860-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="26860-185">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-186">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="26860-187">EfCore.InMemoryPomowacze</span><span class="sxs-lookup"><span data-stu-id="26860-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="26860-188">Udostępnia otokę wokół dostawcy bazy danych EF Core w pamięci.</span><span class="sxs-lookup"><span data-stu-id="26860-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="26860-189">Sprawia, że działa bardziej jak dostawca relacyjne.</span><span class="sxs-lookup"><span data-stu-id="26860-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="26860-190">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-190">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-191">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="26860-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="26860-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="26860-193">Wdrożenie wsparcia czasowego.</span><span class="sxs-lookup"><span data-stu-id="26860-193">An implementation of temporal support.</span></span> <span data-ttu-id="26860-194">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-194">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-195">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="26860-196">Tabela EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="26860-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="26860-197">Łatwe wykonywanie zapytań czasowych w ulubionej `AsTemporalAll()`bazie `AsTemporalAsOf(date)` `AsTemporalFrom(startDate, endDate)`danych `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`za pomocą wprowadzonych metod rozszerzenia: , , , .</span><span class="sxs-lookup"><span data-stu-id="26860-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="26860-198">Dla EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="26860-198">For EF Core: 3.</span></span>

[<span data-ttu-id="26860-199">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="26860-200">EfCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="26860-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="26860-201">Zezwalaj na w pełni funkcjonalne zapytania Entity Framework Core względem [historii czasowej programu SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) przy użyciu kodu EF Core, jednostek i mapowań, które zostały już zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="26860-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="26860-202">Podróżuj w czasie, `using (TemporalQuery.AsOf(targetDateTime)) {...}`zawijając kod w pliku .</span><span class="sxs-lookup"><span data-stu-id="26860-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="26860-203">Dla EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="26860-203">For EF Core: 3.</span></span>

[<span data-ttu-id="26860-204">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="26860-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="26860-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="26860-206">Biblioteka rozszerzeń dla entity framework core, który umożliwia deweloperom, którzy używają programu SQL Server łatwo używać tabel czasowych.</span><span class="sxs-lookup"><span data-stu-id="26860-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="26860-207">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-207">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-208">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="26860-209">Element EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="26860-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="26860-210">Wysokowydajna pamięć podręczna zapytań drugiego poziomu.</span><span class="sxs-lookup"><span data-stu-id="26860-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="26860-211">Dla EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="26860-211">For EF Core: 2.</span></span>

[<span data-ttu-id="26860-212">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="26860-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="26860-213">Entity Framework Plus</span></span>

<span data-ttu-id="26860-214">Rozszerza dbContext o funkcje, takie jak: Filtr, Inspekcja, Buforowanie, Przyszłość kwerendy, Usuwanie partii, Aktualizacja wsadowa i inne.</span><span class="sxs-lookup"><span data-stu-id="26860-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="26860-215">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="26860-216">[Website](https://entityframework-plus.net/)
[Repozytorium GitHub witryny](https://github.com/zzzprojects/EntityFramework-Plus) sieci Web</span><span class="sxs-lookup"><span data-stu-id="26860-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="26860-217">Rozszerzenia struktury encji</span><span class="sxs-lookup"><span data-stu-id="26860-217">Entity Framework Extensions</span></span>

<span data-ttu-id="26860-218">Rozszerza DbContext o wysokiej wydajności operacji masowych: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge i więcej.</span><span class="sxs-lookup"><span data-stu-id="26860-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="26860-219">Dla EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="26860-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="26860-220">witryna sieci web</span><span class="sxs-lookup"><span data-stu-id="26860-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="26860-221">Wyekspresyfikacja</span><span class="sxs-lookup"><span data-stu-id="26860-221">Expressionify</span></span>

<span data-ttu-id="26860-222">Dodaj obsługę wywoływania metod rozszerzenia w linq lambdas.</span><span class="sxs-lookup"><span data-stu-id="26860-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="26860-223">Dla EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="26860-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="26860-224">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="26860-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="26860-225">XLinq (własno)</span><span class="sxs-lookup"><span data-stu-id="26860-225">XLinq</span></span>

<span data-ttu-id="26860-226">Technologia linq (Language Integrated Query) dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="26860-226">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="26860-227">Umożliwia użycie języka C# do pisania silnie wpisanych zapytań.</span><span class="sxs-lookup"><span data-stu-id="26860-227">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="26860-228">Dla EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="26860-228">For EF Core: 3.1</span></span>

- <span data-ttu-id="26860-229">Pełna obsługa języka C# dla tworzenia zapytań: wiele instrukcji wewnątrz lambda, zmienne, funkcje itp.</span><span class="sxs-lookup"><span data-stu-id="26860-229">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="26860-230">Brak luki semantycznej z SQL.</span><span class="sxs-lookup"><span data-stu-id="26860-230">No semantic gap with SQL.</span></span> <span data-ttu-id="26860-231">XLinq deklaruje instrukcje `SELECT`SQL `FROM` `WHERE`(jak , , ) jako metody pierwszej klasy C#, łącząc znaną składnię z intellisense, bezpieczeństwa typu i refaktoryzacji.</span><span class="sxs-lookup"><span data-stu-id="26860-231">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="26860-232">W rezultacie SQL staje się po prostu "inną" biblioteką klas eksponującą swój interfejs API lokalnie, dosłownie *"Language Integrated SQL"*.</span><span class="sxs-lookup"><span data-stu-id="26860-232">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="26860-233">witryna sieci web</span><span class="sxs-lookup"><span data-stu-id="26860-233">Website</span></span>](http://xlinq.live/)
