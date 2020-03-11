---
title: Co nowego w EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417967"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="a7150-102">Co nowego w EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="a7150-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="a7150-103">EF Core 5,0 jest obecnie w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="a7150-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="a7150-104">Ta strona będzie zawierać przegląd interesujących zmian wprowadzonych w każdej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="a7150-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="a7150-105">Pierwsza wersja zapoznawcza EF Core 5,0 jest oczekiwana w ciągu pierwszego kwartału 2020.</span><span class="sxs-lookup"><span data-stu-id="a7150-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="a7150-106">Ta strona nie duplikuje [planu dla EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="a7150-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="a7150-107">Plan opisuje ogólne motywy dla EF Core 5,0, w tym wszystko, co planujemy uwzględnić przed wysyłką ostatecznej wersji.</span><span class="sxs-lookup"><span data-stu-id="a7150-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="a7150-108">Będziemy dodawać linki z tego miejsca do oficjalnej dokumentacji w trakcie jej publikacji.</span><span class="sxs-lookup"><span data-stu-id="a7150-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="a7150-109">Wersja zapoznawcza 1 (jeszcze nie wysłano)</span><span class="sxs-lookup"><span data-stu-id="a7150-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="a7150-110">Rejestrowanie proste</span><span class="sxs-lookup"><span data-stu-id="a7150-110">Simple logging</span></span>

<span data-ttu-id="a7150-111">Ta funkcja dodaje funkcje podobne do `Database.Log` w EF6.</span><span class="sxs-lookup"><span data-stu-id="a7150-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="a7150-112">Oznacza to, że zapewnia prosty sposób pobierania dzienników z EF Core bez konieczności konfigurowania żadnego rodzaju zewnętrznej platformy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a7150-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="a7150-113">Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="a7150-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="a7150-114">Dodatkowa dokumentacja jest śledzona przez [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-114">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="a7150-115">Prosty sposób uzyskiwania wygenerowanego kodu SQL</span><span class="sxs-lookup"><span data-stu-id="a7150-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="a7150-116">EF Core 5,0 wprowadza metodę rozszerzenia `ToQueryString`, która zwróci kod SQL, który zostanie wygenerowany przez EF Core podczas wykonywania zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="a7150-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="a7150-117">Wstępna dokumentacja jest uwzględniona w [statusie tygodniowym EF dla 9 stycznia 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="a7150-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="a7150-118">Dodatkowa dokumentacja jest śledzona przez [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-118">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="a7150-119">Udoskonalone widoki debugowania</span><span class="sxs-lookup"><span data-stu-id="a7150-119">Enhanced debug views</span></span>

<span data-ttu-id="a7150-120">Widoki debugowania to prosty sposób na zajrzeć do wewnętrznych EF Core w przypadku problemów z debugowaniem.</span><span class="sxs-lookup"><span data-stu-id="a7150-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="a7150-121">Widok debugowania dla modelu został zaimplementowany jakiś czas temu.</span><span class="sxs-lookup"><span data-stu-id="a7150-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="a7150-122">W przypadku EF Core 5,0 ten widok modelu jest łatwiejszy do odczytania i dodania nowego widoku debugowania dla śledzonych jednostek w Menedżerze stanu.</span><span class="sxs-lookup"><span data-stu-id="a7150-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="a7150-123">Wstępna dokumentacja jest uwzględniona w [Stanach tygodnia EF 12 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="a7150-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="a7150-124">Dodatkowa dokumentacja jest śledzona przez [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-124">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="a7150-125">Parametry Connection lub Connection można zmienić w zainicjowanym kontekście DbContext</span><span class="sxs-lookup"><span data-stu-id="a7150-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="a7150-126">Teraz łatwiej jest utworzyć wystąpienie DbContext bez żadnego połączenia lub parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="a7150-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="a7150-127">Ponadto parametry połączenia lub połączenia można teraz przystąpić do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a7150-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="a7150-128">Pozwala to temu samemu wystąpieniu kontekstu na dynamiczne łączenie się z różnymi bazami danych.</span><span class="sxs-lookup"><span data-stu-id="a7150-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="a7150-129">Dokumentacja jest śledzona przez [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-129">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="a7150-130">Serwery proxy śledzenia zmian</span><span class="sxs-lookup"><span data-stu-id="a7150-130">Change-tracking proxies</span></span>

<span data-ttu-id="a7150-131">EF Core mogą teraz generować serwery proxy środowiska uruchomieniowego, które automatycznie implementują [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) i [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="a7150-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="a7150-132">Następnie te zmiany są raportowane w oparciu o właściwości jednostki bezpośrednio do EF Core, unikając konieczności skanowania pod kątem zmian.</span><span class="sxs-lookup"><span data-stu-id="a7150-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="a7150-133">Jednak serwery proxy są dostarczane z własnym zestawem ograniczeń, więc nie są przeznaczone dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a7150-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="a7150-134">Dokumentacja jest śledzona przez [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-134">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="a7150-135">Ulepszona obsługa semantyki o wartości null bazy danych</span><span class="sxs-lookup"><span data-stu-id="a7150-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="a7150-136">Relacyjne bazy danych zazwyczaj traktują wartości NULL jako nieznaną wartość i w związku z tym nie są równe żadnym innym NULL.</span><span class="sxs-lookup"><span data-stu-id="a7150-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="a7150-137">C#z drugiej strony traktuje wartość null jako zdefiniowaną wartość, która porównuje ją z innymi wartościami null.</span><span class="sxs-lookup"><span data-stu-id="a7150-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="a7150-138">EF Core domyślnie tłumaczy zapytania, tak aby korzystały C# z semantyki o wartości null.</span><span class="sxs-lookup"><span data-stu-id="a7150-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="a7150-139">EF Core 5,0 znacznie poprawia wydajność tych tłumaczeń.</span><span class="sxs-lookup"><span data-stu-id="a7150-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="a7150-140">Dokumentacja jest śledzona przez [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-140">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="a7150-141">Właściwości indeksatora</span><span class="sxs-lookup"><span data-stu-id="a7150-141">Indexer properties</span></span>

<span data-ttu-id="a7150-142">EF Core 5,0 obsługuje mapowanie właściwości C# indeksatora.</span><span class="sxs-lookup"><span data-stu-id="a7150-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="a7150-143">Dzięki temu jednostki mogą działać jako zbiory właściwości, w których kolumny są mapowane na nazwane właściwości w zbiorze.</span><span class="sxs-lookup"><span data-stu-id="a7150-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="a7150-144">Dokumentacja jest śledzona przez [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-144">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="a7150-145">Generowanie ograniczeń check dla mapowań wyliczenia</span><span class="sxs-lookup"><span data-stu-id="a7150-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="a7150-146">Migracje EF Core 5,0 mogą teraz generować ograniczenia CHECK dla mapowań właściwości enum.</span><span class="sxs-lookup"><span data-stu-id="a7150-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="a7150-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a7150-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="a7150-148">Dokumentacja jest śledzona przez [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-148">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="a7150-149">Tłumaczenie zapytań dla większej liczby konstrukcji DateTime</span><span class="sxs-lookup"><span data-stu-id="a7150-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="a7150-150">Zapytania zawierające nową konstrukcję DataTime są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="a7150-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="a7150-151">Ponadto funkcja SQL Server DateDiffWeek jest teraz zamapowana.</span><span class="sxs-lookup"><span data-stu-id="a7150-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="a7150-152">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-152">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="a7150-153">Tłumaczenie zapytania dla większej liczby konstrukcji tablicy</span><span class="sxs-lookup"><span data-stu-id="a7150-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="a7150-154">Zapytania używające właściwości Contains, Length, SequenceEqual itp. on-Byte [] są teraz tłumaczone na SQL.</span><span class="sxs-lookup"><span data-stu-id="a7150-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="a7150-155">Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="a7150-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="a7150-156">Dodatkowa dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-156">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="a7150-157">Tłumaczenie zapytania do tyłu</span><span class="sxs-lookup"><span data-stu-id="a7150-157">Query translation for Reverse</span></span>

<span data-ttu-id="a7150-158">Zapytania korzystające z `Reverse` są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="a7150-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="a7150-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a7150-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="a7150-160">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-160">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="a7150-161">Tłumaczenie zapytania dla operatorów bitowych</span><span class="sxs-lookup"><span data-stu-id="a7150-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="a7150-162">Zapytania wykorzystujące operatory bitowe są teraz tłumaczone na przykład w większej liczbie przypadków:</span><span class="sxs-lookup"><span data-stu-id="a7150-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="a7150-163">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-163">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="a7150-164">Tłumaczenie zapytania dla ciągów w Cosmos</span><span class="sxs-lookup"><span data-stu-id="a7150-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="a7150-165">Zapytania korzystające z metod String zawierają, StartsWith i EndsWith są teraz tłumaczone przy użyciu dostawcy Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a7150-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="a7150-166">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.</span><span class="sxs-lookup"><span data-stu-id="a7150-166">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
