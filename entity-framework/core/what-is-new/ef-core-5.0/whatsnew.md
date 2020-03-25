---
title: Co nowego w EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136258"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="6ec7b-102">Co nowego w EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="6ec7b-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="6ec7b-103">EF Core 5,0 jest obecnie w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="6ec7b-104">Ta strona będzie zawierać przegląd interesujących zmian wprowadzonych w każdej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="6ec7b-105">Ta strona nie duplikuje [planu dla EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="6ec7b-106">Plan opisuje ogólne motywy dla EF Core 5,0, w tym wszystko, co planujemy uwzględnić przed wysyłką ostatecznej wersji.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="6ec7b-107">Będziemy dodawać linki z tego miejsca do oficjalnej dokumentacji w trakcie jej publikacji.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="6ec7b-108">Wersja zapoznawcza 1</span><span class="sxs-lookup"><span data-stu-id="6ec7b-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="6ec7b-109">Rejestrowanie proste</span><span class="sxs-lookup"><span data-stu-id="6ec7b-109">Simple logging</span></span>

<span data-ttu-id="6ec7b-110">Ta funkcja dodaje funkcje podobne do `Database.Log` w EF6.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="6ec7b-111">Oznacza to, że zapewnia prosty sposób pobierania dzienników z EF Core bez konieczności konfigurowania żadnego rodzaju zewnętrznej platformy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="6ec7b-112">Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="6ec7b-113">Dodatkowa dokumentacja jest śledzona przez [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="6ec7b-114">Prosty sposób uzyskiwania wygenerowanego kodu SQL</span><span class="sxs-lookup"><span data-stu-id="6ec7b-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="6ec7b-115">EF Core 5,0 wprowadza metodę rozszerzenia `ToQueryString`, która zwróci kod SQL, który zostanie wygenerowany przez EF Core podczas wykonywania zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="6ec7b-116">Wstępna dokumentacja jest uwzględniona w [statusie tygodniowym EF dla 9 stycznia 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="6ec7b-117">Dodatkowa dokumentacja jest śledzona przez [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="6ec7b-118">Użyj C# atrybutu, aby wskazać, że jednostka nie ma klucza</span><span class="sxs-lookup"><span data-stu-id="6ec7b-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="6ec7b-119">Typ jednostki można teraz skonfigurować jako bez klucza przy użyciu nowego `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="6ec7b-120">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="6ec7b-121">Dokumentacja jest śledzona przez [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="6ec7b-122">Parametry Connection lub Connection można zmienić w zainicjowanym kontekście DbContext</span><span class="sxs-lookup"><span data-stu-id="6ec7b-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="6ec7b-123">Teraz łatwiej jest utworzyć wystąpienie DbContext bez żadnego połączenia lub parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="6ec7b-124">Ponadto parametry połączenia lub połączenia można teraz przystąpić do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="6ec7b-125">Pozwala to temu samemu wystąpieniu kontekstu na dynamiczne łączenie się z różnymi bazami danych.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="6ec7b-126">Dokumentacja jest śledzona przez [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="6ec7b-127">Serwery proxy śledzenia zmian</span><span class="sxs-lookup"><span data-stu-id="6ec7b-127">Change-tracking proxies</span></span>

<span data-ttu-id="6ec7b-128">EF Core mogą teraz generować serwery proxy środowiska uruchomieniowego, które automatycznie implementują [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) i [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="6ec7b-129">Następnie te zmiany są raportowane w oparciu o właściwości jednostki bezpośrednio do EF Core, unikając konieczności skanowania pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="6ec7b-130">Jednak serwery proxy są dostarczane z własnym zestawem ograniczeń, więc nie są przeznaczone dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="6ec7b-131">Dokumentacja jest śledzona przez [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="6ec7b-132">Udoskonalone widoki debugowania</span><span class="sxs-lookup"><span data-stu-id="6ec7b-132">Enhanced debug views</span></span>

<span data-ttu-id="6ec7b-133">Widoki debugowania to prosty sposób na zajrzeć do wewnętrznych EF Core w przypadku problemów z debugowaniem.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="6ec7b-134">Widok debugowania dla modelu został zaimplementowany jakiś czas temu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="6ec7b-135">W przypadku EF Core 5,0 ten widok modelu jest łatwiejszy do odczytania i dodania nowego widoku debugowania dla śledzonych jednostek w Menedżerze stanu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="6ec7b-136">Wstępna dokumentacja jest uwzględniona w [Stanach tygodnia EF 12 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="6ec7b-137">Dodatkowa dokumentacja jest śledzona przez [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="6ec7b-138">Ulepszona obsługa semantyki o wartości null bazy danych</span><span class="sxs-lookup"><span data-stu-id="6ec7b-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="6ec7b-139">Relacyjne bazy danych zazwyczaj traktują wartości NULL jako nieznaną wartość i w związku z tym nie są równe żadnym innym NULL.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="6ec7b-140">C#z drugiej strony traktuje wartość null jako zdefiniowaną wartość, która porównuje ją z innymi wartościami null.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="6ec7b-141">EF Core domyślnie tłumaczy zapytania, tak aby korzystały C# z semantyki o wartości null.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="6ec7b-142">EF Core 5,0 znacznie poprawia wydajność tych tłumaczeń.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="6ec7b-143">Dokumentacja jest śledzona przez [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="6ec7b-144">Właściwości indeksatora</span><span class="sxs-lookup"><span data-stu-id="6ec7b-144">Indexer properties</span></span>

<span data-ttu-id="6ec7b-145">EF Core 5,0 obsługuje mapowanie właściwości C# indeksatora.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="6ec7b-146">Dzięki temu jednostki mogą działać jako zbiory właściwości, w których kolumny są mapowane na nazwane właściwości w zbiorze.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="6ec7b-147">Dokumentacja jest śledzona przez [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="6ec7b-148">Generowanie ograniczeń check dla mapowań wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6ec7b-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="6ec7b-149">Migracje EF Core 5,0 mogą teraz generować ograniczenia CHECK dla mapowań właściwości enum.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="6ec7b-150">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="6ec7b-151">Dokumentacja jest śledzona przez [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="6ec7b-152">Isrelacyjne</span><span class="sxs-lookup"><span data-stu-id="6ec7b-152">IsRelational</span></span>

<span data-ttu-id="6ec7b-153">Oprócz istniejących `IsSqlServer`, `IsSqlite`i `IsInMemory`dodano nową metodę `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="6ec7b-154">Można go użyć do sprawdzenia, czy DbContext używa dowolnego dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="6ec7b-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="6ec7b-156">Dokumentacja jest śledzona przez [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="6ec7b-157">Cosmos optymistyczne współbieżność za pomocą elementów ETag</span><span class="sxs-lookup"><span data-stu-id="6ec7b-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="6ec7b-158">Dostawca bazy danych Azure Cosmos DB obsługuje teraz optymistyczne współbieżność za pomocą elementów ETag.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="6ec7b-159">Użyj konstruktora modeli w OnModelCreating, aby skonfigurować element ETag:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="6ec7b-160">Metody SaveChanges następnie zgłosi `DbUpdateConcurrencyException` w konflikcie współbieżności, który [można obsłużyć](https://docs.microsoft.com/ef/core/saving/concurrency) w celu zaimplementowania ponownych prób itd.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="6ec7b-161">Dokumentacja jest śledzona przez [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="6ec7b-162">Tłumaczenie zapytań dla większej liczby konstrukcji DateTime</span><span class="sxs-lookup"><span data-stu-id="6ec7b-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="6ec7b-163">Zapytania zawierające nową konstrukcję DateTime są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="6ec7b-164">Ponadto następujące funkcje SQL Server są teraz mapowane:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="6ec7b-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="6ec7b-165">DateDiffWeek</span></span>
* <span data-ttu-id="6ec7b-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="6ec7b-166">DateFromParts</span></span>

<span data-ttu-id="6ec7b-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="6ec7b-168">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="6ec7b-169">Tłumaczenie zapytania dla większej liczby konstrukcji tablicy</span><span class="sxs-lookup"><span data-stu-id="6ec7b-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="6ec7b-170">Zapytania używające właściwości Contains, Length, SequenceEqual itp. on-Byte [] są teraz tłumaczone na SQL.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="6ec7b-171">Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="6ec7b-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="6ec7b-172">Dodatkowa dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="6ec7b-173">Tłumaczenie zapytania do tyłu</span><span class="sxs-lookup"><span data-stu-id="6ec7b-173">Query translation for Reverse</span></span>

<span data-ttu-id="6ec7b-174">Zapytania korzystające z `Reverse` są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="6ec7b-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="6ec7b-176">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="6ec7b-177">Tłumaczenie zapytania dla operatorów bitowych</span><span class="sxs-lookup"><span data-stu-id="6ec7b-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="6ec7b-178">Zapytania wykorzystujące operatory bitowe są teraz tłumaczone na przykład w większej liczbie przypadków:</span><span class="sxs-lookup"><span data-stu-id="6ec7b-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="6ec7b-179">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="6ec7b-180">Tłumaczenie zapytania dla ciągów w Cosmos</span><span class="sxs-lookup"><span data-stu-id="6ec7b-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="6ec7b-181">Zapytania korzystające z metod String zawierają, StartsWith i EndsWith są teraz tłumaczone przy użyciu dostawcy Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="6ec7b-182">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="6ec7b-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
