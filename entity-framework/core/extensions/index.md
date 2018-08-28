---
title: Narzędzia i rozszerzenia — EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995516"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="49c40-102">EF Core Tools i rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="49c40-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="49c40-103">Narzędzia i rozszerzenia oferowanie dodatkowych funkcji dla platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="49c40-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="49c40-104">Rozszerzenia są skompilowanymi z różnych źródeł i nie są przechowywane jako część projektu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="49c40-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="49c40-105">Rozważając rozszerzenia innych firm, pamiętaj ocenić jakość, licencjonowanie, zgodności i pomocy technicznej, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="49c40-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="49c40-106">Narzędzia</span><span class="sxs-lookup"><span data-stu-id="49c40-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="49c40-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="49c40-107">LLBLGen Pro</span></span>

<span data-ttu-id="49c40-108">LLBLGen Pro jest jednostką modelowania rozwiązania z obsługą platformy Entity Framework i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="49c40-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="49c40-109">Dzięki temu można łatwo zdefiniować model jednostki i dokonaj mapowania go do bazy danych, najpierw przy użyciu bazy danych lub model, dzięki czemu możesz rozpocząć pracę już teraz Pisanie zapytań.</span><span class="sxs-lookup"><span data-stu-id="49c40-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="49c40-110">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="49c40-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="49c40-111">Devart jednostki dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="49c40-111">Devart Entity Developer</span></span>

<span data-ttu-id="49c40-112">Deweloper jednostki jest zaawansowane Projektant ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="49c40-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="49c40-113">Można użyć modelu i bazy danych zbliża się do zaprojektować ORM model i wygenerować kodu C# lub Visual Basic .NET dla niego.</span><span class="sxs-lookup"><span data-stu-id="49c40-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="49c40-114">Jego wprowadza nowe podejście do projektowania Modele ORM, zwiększa wydajność pracy i ułatwia tworzenie aplikacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="49c40-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="49c40-115">Witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="49c40-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="49c40-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="49c40-116">EF Core Power Tools</span></span>

<span data-ttu-id="49c40-117">Visual Studio 2017 + rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="49c40-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="49c40-118">Możesz odtwarzać klasy DbContext i POCO z istniejącej bazy danych lub projekt bazy danych SQL Server i wizualizować i sprawdzić swoje DbContext na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="49c40-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="49c40-119">Witryny typu wiki w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="49c40-120">Rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="49c40-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="49c40-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="49c40-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="49c40-122">Wtyczki dla Microsoft.EntityFrameworkCore do obsługi automatycznie historii zmian danych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="49c40-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="49c40-123">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="49c40-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="49c40-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="49c40-125">Dynamiczne rozszerzeń Linq dla Microsoft.EntityFrameworkCore, który dodaje asynchroniczna pomoc techniczna</span><span class="sxs-lookup"><span data-stu-id="49c40-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="49c40-126">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="49c40-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="49c40-127">EFCore.Practices</span></span>

<span data-ttu-id="49c40-128">Podjęto próbę do przechwytywania niektóre dobre lub najlepszych praktyk w interfejsie API obsługującego testów — w tym małe framework do skanowania w poszukiwaniu N + 1 zapytania.</span><span class="sxs-lookup"><span data-stu-id="49c40-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="49c40-129">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="49c40-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="49c40-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="49c40-131">Drugi poziom buforowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="49c40-131">Second Level Caching Library.</span></span> <span data-ttu-id="49c40-132">Drugi poziom buforowania jest pamięć podręczna zapytań.</span><span class="sxs-lookup"><span data-stu-id="49c40-132">Second level caching is a query cache.</span></span> <span data-ttu-id="49c40-133">Wyniki polecenia EF będą przechowywane w pamięci podręcznej, więc, że te same polecenia EF będzie pobrać dane z pamięci podręcznej, zamiast ponownie ich wykonania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="49c40-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="49c40-134">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="49c40-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="49c40-135">Detached.EntityFramework</span></span>

<span data-ttu-id="49c40-136">Załadowanie i zapisanie wykresy całego odłączonych jednostek (jednostka wraz z ich obiektów podrzędnych i listy).</span><span class="sxs-lookup"><span data-stu-id="49c40-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="49c40-137">Czerp inspirację [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="49c40-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="49c40-138">Chodzi o to również dodać niektóre wtyczki do simplificate powtarzających się zadań, takich jak przeprowadzanie inspekcji i dzielenia na strony.</span><span class="sxs-lookup"><span data-stu-id="49c40-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="49c40-139">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="49c40-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="49c40-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="49c40-141">Pobieranie klucza podstawowego (w tym kluczy złożonych) z dowolnej jednostki jako słownik.</span><span class="sxs-lookup"><span data-stu-id="49c40-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="49c40-142">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="49c40-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="49c40-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="49c40-144">Reaktywne rozszerzenia otoki dla gorących dostrzegalne elementy jednostek platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="49c40-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="49c40-145">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="49c40-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="49c40-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="49c40-147">Dodaj wyzwalacze do jednostek przy użyciu Wstawianie, aktualizowanie i usuwanie zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="49c40-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="49c40-148">Istnieją trzy zdarzenia dla każdego: przed, po i w przypadku awarii.</span><span class="sxs-lookup"><span data-stu-id="49c40-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="49c40-149">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="49c40-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="49c40-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="49c40-151">Wpisane korzystać OriginalValue swoje właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="49c40-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="49c40-152">Proste i złożone są obsługiwane, są nawigacji/kolekcji.</span><span class="sxs-lookup"><span data-stu-id="49c40-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="49c40-153">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="49c40-154">Geco</span><span class="sxs-lookup"><span data-stu-id="49c40-154">Geco</span></span>

<span data-ttu-id="49c40-155">Geco udostępnia generator odwrotnego modelu z obsługą Pluralizacja/Singularization oraz szablonów do edycji na podstawie ciągów języka C# 6.0 interpolowane i uruchamiania na.Net Core.</span><span class="sxs-lookup"><span data-stu-id="49c40-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="49c40-156">Umożliwia także generator skryptów inicjatora ze skryptami scalania SQL i moduł uruchamiający skrypt.</span><span class="sxs-lookup"><span data-stu-id="49c40-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="49c40-157">Repozytorium Github</span><span class="sxs-lookup"><span data-stu-id="49c40-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="49c40-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="49c40-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="49c40-159">LinqKit.Microsoft.EntityFrameworkCore to bezpłatny zestaw rozszerzeń dla programu LINQ do SQL i EntityFrameworkCore użytkownicy zaawansowani.</span><span class="sxs-lookup"><span data-stu-id="49c40-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="49c40-160">Dzięki obsłudze Include(...) i IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="49c40-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="49c40-161">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="49c40-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="49c40-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="49c40-163">NeinLinq.EntityFrameworkCore zawiera przydatne rozszerzenia dotyczące korzystania z LINQ dostawców, takich jak Entity Framework, które obsługują tylko drobne podzbiór funkcji platformy .NET, ponowne użycie funkcji, ponowne napisanie zapytania, nawet nadawania wartość null i tworzenie zapytań dynamicznych za pomocą można przetłumaczyć predykatach i selektory.</span><span class="sxs-lookup"><span data-stu-id="49c40-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="49c40-164">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="49c40-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="49c40-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="49c40-166">Wtyczki dla Microsoft.EntityFrameworkCore do obsługi repozytorium, jednostkę pracy wzorców i wielu bazy danych z transakcji rozproszonych obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="49c40-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="49c40-167">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="49c40-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="49c40-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="49c40-169">Powolne ładowanie dla platformy EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="49c40-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="49c40-170">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="49c40-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="49c40-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="49c40-172">Rozszerzenia EntityFrameworkCore potrzeby zbiorczych operacji (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="49c40-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="49c40-173">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="49c40-174">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="49c40-174">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="49c40-175">Dodaje pluralizacja czasu projektowania do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="49c40-175">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="49c40-176">Repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="49c40-176">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
