---
title: Co to jest nowa w programie EF 2.1 Core - EF Core
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="48fe8-102">Nowe funkcje w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="48fe8-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="48fe8-103">Ta wersja jest dostępny w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="48fe8-103">This release is still in preview.</span></span>

<span data-ttu-id="48fe8-104">Oprócz wiele małych ulepszeń i poprawek usterek produktu więcej niż stu EF Core 2.1 zawiera kilka nowych funkcji:</span><span class="sxs-lookup"><span data-stu-id="48fe8-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="48fe8-105">powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="48fe8-105">Lazy loading</span></span>
<span data-ttu-id="48fe8-106">Podstawowe EF zawiera teraz konieczne bloków konstrukcyjnych dla każdego do tworzenia klas jednostek, które można załadować ich właściwości nawigacji na żądanie.</span><span class="sxs-lookup"><span data-stu-id="48fe8-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="48fe8-107">Również utworzono nowy pakiet, Microsoft.EntityFrameworkCore.Proxies, który wykorzystuje te bloki konstrukcyjne do utworzenia serwera proxy opóźnionego ładowania klas minimalny zestaw na podstawie zmodyfikowane klasy jednostki (np. klasy z właściwości nawigacji wirtualnego).</span><span class="sxs-lookup"><span data-stu-id="48fe8-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="48fe8-108">Odczyt [sekcję opóźnionego ładowania](xref:core/querying/related-data#lazy-loading) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="48fe8-109">Parametry w konstruktorach jednostki</span><span class="sxs-lookup"><span data-stu-id="48fe8-109">Parameters in entity constructors</span></span>
<span data-ttu-id="48fe8-110">Jako jeden z wymaganych bloków konstrukcyjnych dla ładowania opóźnionego możemy włączono tworzenie jednostek, które pobierają parametry w ich konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="48fe8-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="48fe8-111">Można wstawić wartości właściwości, delegatów opóźnionego ładowania i usługi mogą używać parametrów.</span><span class="sxs-lookup"><span data-stu-id="48fe8-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="48fe8-112">Odczyt [sekcję na temat jednostek konstruktora o parametrach](xref:core/modeling/constructors) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="48fe8-113">Konwersje wartości</span><span class="sxs-lookup"><span data-stu-id="48fe8-113">Value conversions</span></span>
<span data-ttu-id="48fe8-114">Do tej pory EF Core można mapują tylko właściwości typów obsługiwane przez dostawcę podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48fe8-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="48fe8-115">Wartości zostały skopiowane i z powrotem między kolumnami i właściwości bez przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="48fe8-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="48fe8-116">W programie EF Core 2.1, konwersji wartości można zastosować do przekształcenia wartości z kolumn przed ich zastosowaniem do właściwości i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="48fe8-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="48fe8-117">Mamy wiele konwersje, które mogą być stosowane przez Konwencję w razie potrzeby, jak również jawne konfiguracji interfejsu API, który umożliwia rejestrowanie niestandardowe konwersje między kolumnami i właściwości.</span><span class="sxs-lookup"><span data-stu-id="48fe8-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="48fe8-118">Niektóre stosowania tej funkcji to:</span><span class="sxs-lookup"><span data-stu-id="48fe8-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="48fe8-119">Przechowywanie wyliczenia w postaci ciągów</span><span class="sxs-lookup"><span data-stu-id="48fe8-119">Storing enums as strings</span></span>
- <span data-ttu-id="48fe8-120">Mapowanie podpisane liczby całkowite z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="48fe8-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="48fe8-121">Automatycznego szyfrowania i odszyfrowywania wartości właściwości</span><span class="sxs-lookup"><span data-stu-id="48fe8-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="48fe8-122">Odczyt [sekcję konwersji wartości](xref:core/modeling/value-conversions) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="48fe8-123">Tłumaczenie LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="48fe8-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="48fe8-124">Przed wersji 2.1, w podstawowej EF operator GroupBy LINQ jest zawsze oceniane w pamięci.</span><span class="sxs-lookup"><span data-stu-id="48fe8-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="48fe8-125">Obsługujemy teraz tłumaczenia go do klauzuli SQL GROUP BY w typowych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="48fe8-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="48fe8-126">Ten przykład zawiera zapytanie z GroupBy używanych do obliczenia różne funkcje agregujące:</span><span class="sxs-lookup"><span data-stu-id="48fe8-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="48fe8-127">Odpowiednie tłumaczenia SQL wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="48fe8-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="48fe8-128">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="48fe8-128">Data Seeding</span></span>
<span data-ttu-id="48fe8-129">Wraz z wydaniem nowej będzie możliwe zapewnienie początkowej danych do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="48fe8-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="48fe8-130">W odróżnieniu od do EF6, wstępne wypełnianie danych jest przypisany do typu jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="48fe8-131">Następnie EF Core migracji można automatycznie obliczyć co insert, update lub delete potrzebę operacje można zastosować podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="48fe8-132">Na przykład można użyj go do skonfigurowania danych inicjatora dla żądania Post w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="48fe8-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="48fe8-133">Odczyt [sekcję na temat wstępnego wypełniania danych](xref:core/modeling/data-seeding) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="48fe8-134">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="48fe8-134">Query types</span></span>
<span data-ttu-id="48fe8-135">Model EF Core teraz może zawierać typy zapytań.</span><span class="sxs-lookup"><span data-stu-id="48fe8-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="48fe8-136">Inaczej niż w przypadku typów jednostek, typy zapytań nie zostały zdefiniowane na nich klucze i nie można wstawić, usunięte lub zaktualizowane (tj. są tylko do odczytu), ale może być zwracany bezpośrednio przez zapytania.</span><span class="sxs-lookup"><span data-stu-id="48fe8-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="48fe8-137">Niektóre scenariusze użycia dotyczące typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="48fe8-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="48fe8-138">Mapowanie do widoków bez kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="48fe8-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="48fe8-139">Mapowanie do tabel bez kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="48fe8-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="48fe8-140">Mapowanie do zapytań, zdefiniowanego w modelu</span><span class="sxs-lookup"><span data-stu-id="48fe8-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="48fe8-141">Służy jako typ zwracany dla `FromSql()` zapytań</span><span class="sxs-lookup"><span data-stu-id="48fe8-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="48fe8-142">Odczyt [sekcję na temat typów zapytań](xref:core/modeling/query-types) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="48fe8-143">Uwzględnij typy pochodne</span><span class="sxs-lookup"><span data-stu-id="48fe8-143">Include for derived types</span></span>
<span data-ttu-id="48fe8-144">Będzie ona teraz można określić właściwości nawigacji zdefiniowana tylko na typów pochodnych, gdy tworzenie wyrażeń na potrzeby `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="48fe8-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="48fe8-145">Dla silnie typizowaną wersję `Include`, firma Microsoft obsługuje za pomocą jawnego rzutowania lub `as` operatora.</span><span class="sxs-lookup"><span data-stu-id="48fe8-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="48fe8-146">Firma Microsoft obsługuje teraz również odwołuje się do nazwy właściwości nawigacji zdefiniowanej w typach pochodnych w ciągu `Include`:</span><span class="sxs-lookup"><span data-stu-id="48fe8-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="48fe8-147">Odczyt [sekcję Include z typami pochodnymi](xref:core/querying/related-data#include-on-derived-types) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="48fe8-148">Obsługa System.Transactions</span><span class="sxs-lookup"><span data-stu-id="48fe8-148">System.Transactions support</span></span>
<span data-ttu-id="48fe8-149">Dodano możliwość korzystania z funkcji System.Transactions, takich jak element TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="48fe8-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="48fe8-150">To będzie działać na .NET Framework i .NET Core przy użyciu dostawcy bazy danych, które obsługują tę.</span><span class="sxs-lookup"><span data-stu-id="48fe8-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="48fe8-151">Odczyt [sekcję System.Transactions](xref:core/saving/transactions#using-systemtransactions) Aby uzyskać więcej informacji dotyczących tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48fe8-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="48fe8-152">Lepsze kolejność kolumn w początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="48fe8-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="48fe8-153">Na podstawie opinii klientów, zostały zaktualizowane migracje początkowo Generowanie kolumn w tabelach w tej samej kolejności jak właściwości są zadeklarowane w klasach.</span><span class="sxs-lookup"><span data-stu-id="48fe8-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="48fe8-154">Należy pamiętać, EF Core nie można zmienić kolejności podczas dodawania nowych elementów członkowskich po utworzeniu początkowej tabeli.</span><span class="sxs-lookup"><span data-stu-id="48fe8-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="48fe8-155">Optymalizacja podzapytania skorelowane</span><span class="sxs-lookup"><span data-stu-id="48fe8-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="48fe8-156">Firma Microsoft ulepszyła naszych Translacja zapytania, aby uniknąć wykonywania "N + 1" zapytania SQL w wielu typowych scenariuszy, w których użycie właściwości nawigacji w projekcji prowadzi do łączenie danych z zapytania głównego z danymi z podzapytania skorelowane.</span><span class="sxs-lookup"><span data-stu-id="48fe8-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="48fe8-157">Optymalizacja wymaga wyników buforowania tworzą podzapytanie i wymagamy, należy zmodyfikować zapytanie, aby zdecydować się na nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="48fe8-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="48fe8-158">Na przykład poniższe zapytanie zwykle pobiera przetłumaczyć jednej kwerendzie dla klientów, a także N (gdzie "N" oznacza liczbę klientów zwrócił) oddzielne zapytania dla zleceń:</span><span class="sxs-lookup"><span data-stu-id="48fe8-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="48fe8-159">W tym `ToList()` w odpowiednim miejscu, należy wskazać, że buforowanie jest odpowiednie dla zleceń, które umożliwiają optymalizację:</span><span class="sxs-lookup"><span data-stu-id="48fe8-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="48fe8-160">Należy pamiętać, że to zapytanie zostanie przekonwertowana na stronę tylko dwa zapytania SQL: jeden dla klientów i kolejnego dla zleceń.</span><span class="sxs-lookup"><span data-stu-id="48fe8-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="48fe8-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="48fe8-161">OwnedAttribute</span></span>

<span data-ttu-id="48fe8-162">Obecnie istnieje możliwość skonfigurowania [należące do typów jednostek](xref:core/modeling/owned-entities) przez po prostu Dodawanie adnotacji do typu z `[Owned]` , a następnie sprawdzając, czy jednostka właściciela jest dodawane do modelu:</span><span class="sxs-lookup"><span data-stu-id="48fe8-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="database-provider-compatibility"></a><span data-ttu-id="48fe8-163">Zgodność dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="48fe8-163">Database provider compatibility</span></span>

<span data-ttu-id="48fe8-164">Podstawowe EF 2.1 zaprojektowano tak, aby był zgodny z dostawcy bazy danych utworzone dla EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="48fe8-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="48fe8-165">Podczas gdy niektóre funkcje opisane powyżej (np. wartość konwersje) wymagają zaktualizowanej dostawcy, innych użytkowników (np. podczas ładowania opóźnionego) uaktywni się z istniejącymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="48fe8-165">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="48fe8-166">Jeśli okaże się żadnym nieoczekiwany niezgodności lub dowolnym wystawiania w nowych funkcji lub jeśli masz opinię na nich, zgłoś go za pomocą [naszych tracker problem](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="48fe8-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
