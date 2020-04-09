---
title: Co nowego w EF Core 5.0
description: Przegląd nowych funkcji w EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634281"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="b6717-103">Co nowego w EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="b6717-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="b6717-104">EF Core 5.0 jest obecnie w fazie rozwoju.</span><span class="sxs-lookup"><span data-stu-id="b6717-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="b6717-105">Ta strona będzie zawierać przegląd interesujących zmian wprowadzonych w każdej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b6717-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="b6717-106">Ta strona nie powiela [planu ef core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="b6717-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="b6717-107">Plan opisuje ogólne tematy ef core 5.0, w tym wszystko, co planujemy uwzględnić przed wysłaniem ostatecznej wersji.</span><span class="sxs-lookup"><span data-stu-id="b6717-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="b6717-108">Dodamy linki stąd do oficjalnej dokumentacji, jak to jest publikowane.</span><span class="sxs-lookup"><span data-stu-id="b6717-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="b6717-109">Preview 2</span><span class="sxs-lookup"><span data-stu-id="b6717-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="b6717-110">Użyj atrybutu C#, aby określić pole zapasowe właściwości</span><span class="sxs-lookup"><span data-stu-id="b6717-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="b6717-111">Atrybut C# może teraz służyć do określania pola zapasowego dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="b6717-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="b6717-112">Ten atrybut umożliwia EF Core nadal zapisywać i odczytywać z pola zapasowego, jak zwykle się zdarzyć, nawet wtedy, gdy pole zapasowe nie można znaleźć automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b6717-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="b6717-113">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="b6717-114">Dokumentacja jest śledzona przez [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="b6717-115">Kompletne mapowanie dyskryminatora</span><span class="sxs-lookup"><span data-stu-id="b6717-115">Complete discriminator mapping</span></span>

<span data-ttu-id="b6717-116">EF Core używa kolumny rozróżniacza do [mapowania TPH hierarchii dziedziczenia](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="b6717-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="b6717-117">Niektóre ulepszenia wydajności są możliwe, tak długo, jak EF Core zna wszystkie możliwe wartości dla dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="b6717-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="b6717-118">EF Core 5.0 implementuje teraz te ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="b6717-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="b6717-119">Na przykład poprzednie wersje EF Core zawsze generować ten SQL dla kwerendy zwracając wszystkie typy w hierarchii:</span><span class="sxs-lookup"><span data-stu-id="b6717-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="b6717-120">EF Core 5.0 wygeneruje teraz następujące elementy po skonfigurowaniu pełnego mapowania dyskryminatora:</span><span class="sxs-lookup"><span data-stu-id="b6717-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="b6717-121">Będzie to domyślne zachowanie, począwszy od wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b6717-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="b6717-122">Ulepszenia wydajności w witrynie Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="b6717-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="b6717-123">Dokonaliśmy dwóch ulepszeń wydajności dla sqlite:</span><span class="sxs-lookup"><span data-stu-id="b6717-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="b6717-124">Pobieranie danych binarnych i ciągów za pomocą GetBytes, GetChars i GetTextReader jest teraz bardziej wydajne, korzystając z SqliteBlob i strumieni.</span><span class="sxs-lookup"><span data-stu-id="b6717-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="b6717-125">Inicjowanie SqliteConnection jest teraz leniwy.</span><span class="sxs-lookup"><span data-stu-id="b6717-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="b6717-126">Te ulepszenia są w ADO.NET microsoft.data.sqlite dostawcy, a tym samym również zwiększyć wydajność poza EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6717-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="b6717-127">Wersja zapoznawcza 1</span><span class="sxs-lookup"><span data-stu-id="b6717-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="b6717-128">Proste rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="b6717-128">Simple logging</span></span>

<span data-ttu-id="b6717-129">Ta funkcja dodaje funkcje podobne do `Database.Log` w EF6.</span><span class="sxs-lookup"><span data-stu-id="b6717-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="b6717-130">Oznacza to, że zapewnia prosty sposób, aby uzyskać dzienniki z EF Core bez konieczności konfigurowania wszelkiego rodzaju zewnętrznej struktury rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="b6717-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="b6717-131">Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 5 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)</span><span class="sxs-lookup"><span data-stu-id="b6717-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="b6717-132">Dodatkowa dokumentacja jest śledzona przez [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="b6717-133">Prosty sposób na wygenerowanie SQL</span><span class="sxs-lookup"><span data-stu-id="b6717-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="b6717-134">EF Core 5.0 `ToQueryString` wprowadza metodę rozszerzenia, która zwróci SQL, który EF Core wygeneruje podczas wykonywania kwerendy LINQ.</span><span class="sxs-lookup"><span data-stu-id="b6717-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="b6717-135">Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 9 stycznia 2020 r.](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)</span><span class="sxs-lookup"><span data-stu-id="b6717-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="b6717-136">Dodatkowa dokumentacja jest śledzona przez [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="b6717-137">Użyj atrybutu C#, aby wskazać, że encja nie ma klucza</span><span class="sxs-lookup"><span data-stu-id="b6717-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="b6717-138">Typ jednostki można teraz skonfigurować jako bez `KeylessAttribute`klucza przy użyciu nowego .</span><span class="sxs-lookup"><span data-stu-id="b6717-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="b6717-139">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="b6717-140">Dokumentacja jest śledzona przez [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="b6717-141">Połączenie lub parametry połączenia można zmienić przy zainicjowaniu dbContext</span><span class="sxs-lookup"><span data-stu-id="b6717-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="b6717-142">Teraz łatwiej jest utworzyć wystąpienie DbContext bez połączenia lub ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="b6717-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="b6717-143">Ponadto parametry połączenia lub połączenia można teraz zmutować w wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="b6717-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="b6717-144">Ta funkcja umożliwia tego samego wystąpienia kontekstu dynamicznie połączyć się z różnymi bazami danych.</span><span class="sxs-lookup"><span data-stu-id="b6717-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="b6717-145">Dokumentacja jest śledzona przez [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="b6717-146">Serwery proxy śledzenia zmian</span><span class="sxs-lookup"><span data-stu-id="b6717-146">Change-tracking proxies</span></span>

<span data-ttu-id="b6717-147">EF Core może teraz generować serwery proxy środowiska uruchomieniowego, które automatycznie implementują [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) i [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="b6717-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="b6717-148">Następnie zgłaszają zmiany wartości właściwości jednostki bezpośrednio do EF Core, unikając konieczności skanowania w poszukiwaniu zmian.</span><span class="sxs-lookup"><span data-stu-id="b6717-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="b6717-149">Jednak serwery proxy mają własny zestaw ograniczeń, więc nie są dla wszystkich.</span><span class="sxs-lookup"><span data-stu-id="b6717-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="b6717-150">Dokumentacja jest śledzona przez [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="b6717-151">Ulepszone widoki debugowania</span><span class="sxs-lookup"><span data-stu-id="b6717-151">Enhanced debug views</span></span>

<span data-ttu-id="b6717-152">Widoki debugowania są łatwym sposobem, aby spojrzeć na wewnętrzne EF Core podczas debugowania problemów.</span><span class="sxs-lookup"><span data-stu-id="b6717-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="b6717-153">Widok debugowania dla modelu został zaimplementowany jakiś czas temu.</span><span class="sxs-lookup"><span data-stu-id="b6717-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="b6717-154">Dla EF Core 5.0 ułatwiliśmy odczytanie widoku modelu i dodaliśmy nowy widok debugowania dla śledzonych jednostek w menedżerze stanu.</span><span class="sxs-lookup"><span data-stu-id="b6717-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="b6717-155">Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 12 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)</span><span class="sxs-lookup"><span data-stu-id="b6717-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="b6717-156">Dodatkowa dokumentacja jest śledzona przez [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="b6717-157">Ulepszona obsługa semantyki zerowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="b6717-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="b6717-158">Relacyjne bazy danych zazwyczaj traktują NULL jako nieznaną wartość i dlatego nie jest równa żadnej innej wartości NULL.</span><span class="sxs-lookup"><span data-stu-id="b6717-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="b6717-159">Podczas C# traktuje null jako zdefiniowaną wartość, która porównuje równe innym null.</span><span class="sxs-lookup"><span data-stu-id="b6717-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="b6717-160">EF Core domyślnie tłumaczy zapytania, tak aby używać semantyki null języka C#.</span><span class="sxs-lookup"><span data-stu-id="b6717-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="b6717-161">EF Core 5.0 znacznie poprawia wydajność tych tłumaczeń.</span><span class="sxs-lookup"><span data-stu-id="b6717-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="b6717-162">Dokumentacja jest śledzona przez [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="b6717-163">Właściwości indeksatora</span><span class="sxs-lookup"><span data-stu-id="b6717-163">Indexer properties</span></span>

<span data-ttu-id="b6717-164">EF Core 5.0 obsługuje mapowanie właściwości indeksatora Języka C#.</span><span class="sxs-lookup"><span data-stu-id="b6717-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="b6717-165">Właściwości te umożliwiają jednostkom działanie jako worki właściwości, w których kolumny są mapowane na nazwane właściwości w torbie.</span><span class="sxs-lookup"><span data-stu-id="b6717-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="b6717-166">Dokumentacja jest śledzona przez [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="b6717-167">Generowanie ograniczeń kontrolnych dla mapowań wyliczenia</span><span class="sxs-lookup"><span data-stu-id="b6717-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="b6717-168">Ef Core 5.0 Migracje można teraz generować ograniczenia CHECK dla mapowań właściwości wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b6717-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="b6717-169">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="b6717-170">Dokumentacja jest śledzona przez [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="b6717-171">IsRelational (IsRelational)</span><span class="sxs-lookup"><span data-stu-id="b6717-171">IsRelational</span></span>

<span data-ttu-id="b6717-172">Oprócz `IsRelational` istniejącej `IsSqlServer`metody `IsSqlite`dodano nową metodę oraz `IsInMemory`.</span><span class="sxs-lookup"><span data-stu-id="b6717-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="b6717-173">Ta metoda może służyć do testowania, jeśli DbContext używa dowolnego dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6717-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="b6717-174">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="b6717-175">Dokumentacja jest śledzona przez [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="b6717-176">Optymistyczna współbieżność kosmosu z ETagami</span><span class="sxs-lookup"><span data-stu-id="b6717-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="b6717-177">Dostawca bazy danych usługi Azure Cosmos DB obsługuje teraz optymistyczną współbieżność przy użyciu tagów ETags.</span><span class="sxs-lookup"><span data-stu-id="b6717-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="b6717-178">Użyj konstruktora modeli w OnModelCreating, aby skonfigurować eTag:</span><span class="sxs-lookup"><span data-stu-id="b6717-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="b6717-179">SaveChanges następnie zrzuci `DbUpdateConcurrencyException` konflikt współbieżności, który może być [obsługiwany](https://docs.microsoft.com/ef/core/saving/concurrency) w celu zaimplementowania ponownych prób itp.</span><span class="sxs-lookup"><span data-stu-id="b6717-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="b6717-180">Dokumentacja jest śledzona przez [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="b6717-181">Tłumaczenia zapytań, aby uzyskać więcej konstrukcji DateTime</span><span class="sxs-lookup"><span data-stu-id="b6717-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="b6717-182">Zapytania zawierające nową konstrukcję DateTime są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="b6717-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="b6717-183">Ponadto są teraz mapowane następujące funkcje programu SQL Server:</span><span class="sxs-lookup"><span data-stu-id="b6717-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="b6717-184">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="b6717-184">DateDiffWeek</span></span>
* <span data-ttu-id="b6717-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="b6717-185">DateFromParts</span></span>

<span data-ttu-id="b6717-186">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="b6717-187">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="b6717-188">Wykonywanie zapytań o więcej konstrukcji tablicy bajtów</span><span class="sxs-lookup"><span data-stu-id="b6717-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="b6717-189">Kwerendy używające zawiera, długość, sequenceEqual, itp.</span><span class="sxs-lookup"><span data-stu-id="b6717-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="b6717-190">Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 5 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)</span><span class="sxs-lookup"><span data-stu-id="b6717-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="b6717-191">Dodatkowa dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="b6717-192">Tłumaczenie kwerendy dla reverse</span><span class="sxs-lookup"><span data-stu-id="b6717-192">Query translation for Reverse</span></span>

<span data-ttu-id="b6717-193">Kwerendy `Reverse` za pomocą są teraz tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="b6717-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="b6717-194">Przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="b6717-195">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="b6717-196">Translacja kwerend dla operatorów bitowych</span><span class="sxs-lookup"><span data-stu-id="b6717-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="b6717-197">Zapytania używające operatorów bitowych są teraz tłumaczone w większej liczbie przypadków Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b6717-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="b6717-198">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="b6717-199">Tłumaczenie kwerend dla ciągów w usłudze Cosmos</span><span class="sxs-lookup"><span data-stu-id="b6717-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="b6717-200">Kwerendy, które używają metod ciągu Zawiera, StartsWith i EndsWith są teraz tłumaczone podczas korzystania z dostawcy usługi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b6717-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="b6717-201">Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .</span><span class="sxs-lookup"><span data-stu-id="b6717-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
