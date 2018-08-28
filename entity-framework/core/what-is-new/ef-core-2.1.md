---
title: What's new in EF Core 2.1 — EF Core
author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ac39d3b7bc001231c5d76f489931b86c108adde2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995872"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="1cf43-102">Nowe funkcje programu EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="1cf43-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="1cf43-103">Oprócz licznych poprawkach błędów i małych ulepszenia wydajności i funkcjonalności programu EF Core 2.1 zawiera niektóre istotne nowe funkcje:</span><span class="sxs-lookup"><span data-stu-id="1cf43-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1cf43-104">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="1cf43-104">Lazy loading</span></span>
<span data-ttu-id="1cf43-105">EF Core zawiera teraz konieczne bloków konstrukcyjnych dla każdego, kto Tworzenie klas jednostek, które mogą ładować swoje właściwości nawigacji na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1cf43-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="1cf43-106">Utworzyliśmy również nowy pakiet Microsoft.EntityFrameworkCore.Proxies, który wykorzystuje te bloki konstrukcyjne do produkcji proxy powolne ładowanie klas, w oparciu o co najmniej modyfikacji klas jednostek (na przykład klasy z właściwości nawigacji wirtualnego).</span><span class="sxs-lookup"><span data-stu-id="1cf43-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="1cf43-107">Odczyt [sekcji na ładowanie z opóźnieniem](xref:core/querying/related-data#lazy-loading) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="1cf43-108">Parametry w konstruktorach jednostki</span><span class="sxs-lookup"><span data-stu-id="1cf43-108">Parameters in entity constructors</span></span>
<span data-ttu-id="1cf43-109">Tworzenie jednostek, które przyjmują parametry w ich konstruktory mogę umożliwiliśmy jako jeden z bloków konstrukcyjnych wymagane do załadowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="1cf43-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="1cf43-110">Parametry można użyć do dodania wartości właściwości, delegatów powolne ładowanie i usług.</span><span class="sxs-lookup"><span data-stu-id="1cf43-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="1cf43-111">Odczyt [sekcji jednostki konstruktora z parametrami](xref:core/modeling/constructors) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="1cf43-112">Konwersje wartości</span><span class="sxs-lookup"><span data-stu-id="1cf43-112">Value conversions</span></span>
<span data-ttu-id="1cf43-113">Do tej pory programu EF Core może mapować tylko właściwości typów natywnie obsługiwane przez dostawcę podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1cf43-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="1cf43-114">Wartości zostały skopiowane i z powrotem między kolumnami i właściwości bez przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="1cf43-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="1cf43-115">Począwszy od programu EF Core 2.1 konwersji wartości można zastosować do przekształcania wartości z kolumn przed są stosowane do właściwości i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="1cf43-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="1cf43-116">Mamy wiele konwersje, które mogą być stosowane zgodnie z Konwencją, zgodnie z potrzebami, a także interfejsu API jawnego konfiguracji, który umożliwia rejestrowanie niestandardowe konwersje między kolumnami i właściwości.</span><span class="sxs-lookup"><span data-stu-id="1cf43-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="1cf43-117">Zastosowanie tej funkcji, należą:</span><span class="sxs-lookup"><span data-stu-id="1cf43-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="1cf43-118">Przechowywanie typów wyliczeniowych w postaci ciągów</span><span class="sxs-lookup"><span data-stu-id="1cf43-118">Storing enums as strings</span></span>
- <span data-ttu-id="1cf43-119">Mapowanie niepodpisane liczby całkowite z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="1cf43-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="1cf43-120">Automatyczne szyfrowanie i odszyfrowywanie wartości właściwości</span><span class="sxs-lookup"><span data-stu-id="1cf43-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="1cf43-121">Odczyt [sekcji konwersji wartości](xref:core/modeling/value-conversions) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="1cf43-122">Tłumaczenie LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="1cf43-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="1cf43-123">Przed wersją 2.1 w wersji EF Core operatorów GroupBy LINQ zawsze oceniono w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1cf43-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="1cf43-124">Obsługujemy teraz go tłumaczenia klauzuli SQL GROUP BY w najbardziej typowe przypadki.</span><span class="sxs-lookup"><span data-stu-id="1cf43-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="1cf43-125">W tym przykładzie przedstawiono zapytanie z GroupBy używany do obliczania różne funkcje agregujące:</span><span class="sxs-lookup"><span data-stu-id="1cf43-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="1cf43-126">Odpowiednie tłumaczenia SQL wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="1cf43-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="1cf43-127">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="1cf43-127">Data Seeding</span></span>
<span data-ttu-id="1cf43-128">Za pomocą nowej wersji będzie możliwe zapewnienie początkowe dane, aby wypełnić bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1cf43-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="1cf43-129">W odróżnieniu od w EF6, wstępne wypełnianie danych jest skojarzony typ jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="1cf43-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="1cf43-130">Następnie migracje EF Core można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="1cf43-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="1cf43-131">Na przykład można użyj go do skonfigurowania danych inicjatora dla wpisu w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1cf43-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="1cf43-132">Odczyt [sekcji na wstępne wypełnianie danych](xref:core/modeling/data-seeding) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="1cf43-133">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="1cf43-133">Query types</span></span>
<span data-ttu-id="1cf43-134">Modelu platformy EF Core mogą teraz zawierać typy zapytania.</span><span class="sxs-lookup"><span data-stu-id="1cf43-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="1cf43-135">Inaczej niż w przypadku typów jednostek, typy zapytań nie kluczy zdefiniowane i nie można wstawić, usunąć lub zaktualizować (oznacza to, że są one tylko do odczytu), ale mogą być zwrócone bezpośrednio przez zapytania.</span><span class="sxs-lookup"><span data-stu-id="1cf43-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="1cf43-136">Scenariusze użycia dla typów zapytań, należą:</span><span class="sxs-lookup"><span data-stu-id="1cf43-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="1cf43-137">Mapowanie do widoków bez kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="1cf43-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="1cf43-138">Mapowania tabel bez kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="1cf43-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="1cf43-139">Mapowanie do zapytań zdefiniowanych w modelu</span><span class="sxs-lookup"><span data-stu-id="1cf43-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="1cf43-140">Służy jako typ zwracany dla `FromSql()` zapytań</span><span class="sxs-lookup"><span data-stu-id="1cf43-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="1cf43-141">Odczyt [sekcji na typy zapytań](xref:core/modeling/query-types) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="1cf43-142">Zawierają typy pochodne</span><span class="sxs-lookup"><span data-stu-id="1cf43-142">Include for derived types</span></span>
<span data-ttu-id="1cf43-143">Zostanie on teraz jest to możliwe, aby określić właściwości nawigacji tylko wobec podczas pisania wyrażeń dla typów pochodnych `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="1cf43-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="1cf43-144">Silnie typizowaną wersję `Include`, obsługujemy za pomocą jawnego rzutowania lub `as` operatora.</span><span class="sxs-lookup"><span data-stu-id="1cf43-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="1cf43-145">Firma Microsoft obsługuje teraz również odwołuje się do nazwy właściwości nawigacji zdefiniowany dla typów pochodnych w wersję ciągu `Include`:</span><span class="sxs-lookup"><span data-stu-id="1cf43-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="1cf43-146">Odczyt [sekcji Dołącz z typami pochodnymi](xref:core/querying/related-data#include-on-derived-types) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="1cf43-147">Obsługa System.Transactions</span><span class="sxs-lookup"><span data-stu-id="1cf43-147">System.Transactions support</span></span>
<span data-ttu-id="1cf43-148">Dodaliśmy możliwość współpracy z System.Transactions funkcje, takie jak TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="1cf43-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="1cf43-149">Będzie to działać dla platformy .NET Core i .NET Framework podczas korzystania z dostawcy baz danych, które go obsługują.</span><span class="sxs-lookup"><span data-stu-id="1cf43-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="1cf43-150">Odczyt [sekcji System.Transactions](xref:core/saving/transactions#using-systemtransactions) Aby uzyskać więcej informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="1cf43-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="1cf43-151">Lepsze kolejność kolumn w początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="1cf43-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="1cf43-152">Na podstawie opinii klientów Zaktualizowaliśmy migracji można wstępnie wygenerować kolumn dla tabel w tej samej kolejności, ponieważ właściwości są deklarowane w klasach.</span><span class="sxs-lookup"><span data-stu-id="1cf43-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="1cf43-153">Należy pamiętać, programem EF Core nie można zmienić kolejności po dodaniu nowych elementów członkowskich po utworzeniu początkowego tabeli.</span><span class="sxs-lookup"><span data-stu-id="1cf43-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="1cf43-154">Optymalizacja skorelowany podzapytań</span><span class="sxs-lookup"><span data-stu-id="1cf43-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="1cf43-155">Ulepszyliśmy nasze translacji zapytania, aby uniknąć wykonywania "N + 1" zapytania SQL w wielu typowych scenariuszy, w których użycie właściwości nawigacji w projekcji prowadzi do łączenie danych z zapytania katalogu głównego przy użyciu danych z skorelowane podzapytanie.</span><span class="sxs-lookup"><span data-stu-id="1cf43-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="1cf43-156">Optymalizacja, wymagane jest buforowanie wyników z podzapytanie, a firma Microsoft wymaga, aby zmodyfikować zapytanie, aby zdecydować się na nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="1cf43-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="1cf43-157">Na przykład następujące zapytanie zwykle pobiera przetłumaczone na jednej kwerendzie dla klientów, a także N (gdzie "N" oznacza liczbę klientów zwrócił) oddzielnych zapytań dla zleceń:</span><span class="sxs-lookup"><span data-stu-id="1cf43-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="1cf43-158">Jeśli dołączysz `ToList()` w odpowiednim miejscu, wskazujesz, że buforowanie jest odpowiednia dla zamówienia, które umożliwiają optymalizację:</span><span class="sxs-lookup"><span data-stu-id="1cf43-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="1cf43-159">Należy pamiętać, że to zapytanie będzie tłumaczona tylko dwa kwerendy SQL: jeden dla klientów i kolejny zamówień.</span><span class="sxs-lookup"><span data-stu-id="1cf43-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="1cf43-160">Atrybut [należące do firmy]</span><span class="sxs-lookup"><span data-stu-id="1cf43-160">[Owned] attribute</span></span>

<span data-ttu-id="1cf43-161">Teraz jest możliwa do skonfigurowania [posiadane typy jednostek](xref:core/modeling/owned-entities) , po prostu Dodawanie adnotacji do typu z `[Owned]` a następnie sprawdzając, czy jednostka właściciel jest dodawany do modelu:</span><span class="sxs-lookup"><span data-stu-id="1cf43-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="1cf43-162">Narzędzie wiersza polecenia dotnet-ef zawarte w zestawie SDK programu .NET Core</span><span class="sxs-lookup"><span data-stu-id="1cf43-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="1cf43-163">_Dotnet ef_ polecenia są teraz częścią programu .NET Core SDK, w związku z tym nie będzie konieczne użycie DotNetCliToolReference w projekcie, aby można było użyć migracje lub tworzenia szkieletu DbContext z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1cf43-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="1cf43-164">Zobacz sekcję dotyczącą [instalowania narzędzi](xref:core/miscellaneous/cli/dotnet#installing-the-tools) Aby uzyskać więcej informacji o sposobie włączania narzędzia wiersza polecenia dla różnych wersji zestawu SDK programu .NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="1cf43-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="1cf43-165">Pakiet Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="1cf43-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="1cf43-166">Nowy pakiet zawiera atrybuty i interfejsy, które można użyć w swoich projektach, aby wzbogacić funkcji EF Core bez zależna od programu EF Core jako całości.</span><span class="sxs-lookup"><span data-stu-id="1cf43-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="1cf43-167">Na przykład atrybut [posiadane] i interfejs ILazyLoader znajdują się w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="1cf43-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="1cf43-168">Zdarzenia zmiany stanu</span><span class="sxs-lookup"><span data-stu-id="1cf43-168">State change events</span></span>

<span data-ttu-id="1cf43-169">Nowe `Tracked` i `StateChanged` zdarzenia `ChangeTracker` może być użyty do zapisu logikę, która reaguje na jednostki, wprowadzając kontekstu DbContext lub zmianę ich stanu.</span><span class="sxs-lookup"><span data-stu-id="1cf43-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="1cf43-170">Nieprzetworzone analizatora parametru SQL</span><span class="sxs-lookup"><span data-stu-id="1cf43-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="1cf43-171">Nowy analizator kodu jest dołączana do programu EF Core, który wykrywa potencjalnie niebezpieczną użycia interfejsów API raw SQL, takie jak `FromSql` lub `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="1cf43-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="1cf43-172">Na przykład dla następującego zapytania, zobaczysz ostrzeżenie ponieważ _minAge_ nie jest sparametryzowane:</span><span class="sxs-lookup"><span data-stu-id="1cf43-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="1cf43-173">Zgodności dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="1cf43-173">Database provider compatibility</span></span>

<span data-ttu-id="1cf43-174">Zaleca się używać programu EF Core 2.1 z dostawcami, które zostały zaktualizowane lub co najmniej przetestowany w celu pracy z programem EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1cf43-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="1cf43-175">Jeśli okaże się wszelkie nieoczekiwane niezgodności dowolne wysłać nowych funkcji lub jeśli chcesz przesłać opinię na nich, zgłoś go za pomocą [nasze narzędzia do śledzenia błędów](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="1cf43-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
