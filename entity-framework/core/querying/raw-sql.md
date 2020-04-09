---
title: Nieprzetworzone zapytania SQL — EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417671"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="b6919-102">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="b6919-102">Raw SQL Queries</span></span>

<span data-ttu-id="b6919-103">Entity Framework Core umożliwia rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6919-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="b6919-104">Nieprzetworzone zapytania SQL są przydatne, jeśli kwerenda, której nie można wyrazić za pomocą LINQ.</span><span class="sxs-lookup"><span data-stu-id="b6919-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="b6919-105">Nieprzetworzone kwerendy SQL są również używane, jeśli przy użyciu kwerendy LINQ powoduje nieefektywne zapytanie SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="b6919-106">Nieprzetworzone kwerendy SQL mogą zwracać zwykłe typy jednostek lub [typy jednostek bezkluszeń,](xref:core/modeling/keyless-entity-types) które są częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="b6919-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="b6919-107">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="b6919-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="b6919-108">Podstawowe nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="b6919-108">Basic raw SQL queries</span></span>

<span data-ttu-id="b6919-109">Za pomocą `FromSqlRaw` metody rozszerzenia można rozpocząć kwerendę LINQ opartą na nieprzetworzonej kwerendzie SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="b6919-110">`FromSqlRaw`mogą być używane tylko na korzeniach zapytań, które są bezpośrednio na . `DbSet<>`</span><span class="sxs-lookup"><span data-stu-id="b6919-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="b6919-111">Nieprzetworzone zapytania SQL mogą być używane do wykonywania procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="b6919-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="b6919-112">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="b6919-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="b6919-113">**Zawsze używaj parametryzacji dla nieprzetworzonych zapytań SQL**</span><span class="sxs-lookup"><span data-stu-id="b6919-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="b6919-114">Podczas wprowadzania żadnych wartości dostarczonych przez użytkownika do nieprzetworzonej kwerendy SQL, należy zwrócić uwagę, aby uniknąć ataków iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="b6919-115">Oprócz sprawdzania poprawności, że takie wartości nie zawierają nieprawidłowych znaków, zawsze należy użyć parametryzacji, która wysyła wartości oddzielone od tekstu SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="b6919-116">W szczególności nigdy nie należy przekazywać ciągu`$""`łączonego lub interpolowanego `FromSqlRaw` ( `ExecuteSqlRaw`) z niezatwierdzonymi wartościami dostarczonymi przez użytkownika do lub .</span><span class="sxs-lookup"><span data-stu-id="b6919-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="b6919-117">Metody `FromSqlInterpolated` `ExecuteSqlInterpolated` i umożliwiają użycie składni interpolacji ciągów w sposób, który chroni przed atakami iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="b6919-118">Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i zapewniając dodatkowy argument.</span><span class="sxs-lookup"><span data-stu-id="b6919-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="b6919-119">Chociaż ta składnia może `String.Format` wyglądać jak składnia, podana `DbParameter` wartość jest zawijana `{0}` w a, a wygenerowana nazwa parametru wstawiona w miejscu, w którym określono symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="b6919-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="b6919-120">`FromSqlInterpolated`jest podobny `FromSqlRaw` do ale pozwala na użycie składni interpolacji ciągów.</span><span class="sxs-lookup"><span data-stu-id="b6919-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="b6919-121">Podobnie `FromSqlRaw`jak `FromSqlInterpolated` , może być używany tylko na korzenie kwerendy.</span><span class="sxs-lookup"><span data-stu-id="b6919-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="b6919-122">Podobnie jak w poprzednim przykładzie, wartość `DbParameter` jest konwertowana na i nie jest narażony na iniekcję SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="b6919-123">Przed wersją 3.0 i `FromSqlRaw` `FromSqlInterpolated` były dwa `FromSql`przeciążenia o nazwie .</span><span class="sxs-lookup"><span data-stu-id="b6919-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="b6919-124">Aby uzyskać więcej informacji, zobacz [sekcję Poprzednie wersje](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="b6919-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="b6919-125">Można również skonstruować DbParameter i podać go jako wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="b6919-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="b6919-126">Ponieważ używany jest zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy ciągu, `FromSqlRaw` może być bezpiecznie używany:</span><span class="sxs-lookup"><span data-stu-id="b6919-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="b6919-127">`FromSqlRaw`umożliwia użycie nazwanych parametrów w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="b6919-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="b6919-128">Komponowanie z LINQ</span><span class="sxs-lookup"><span data-stu-id="b6919-128">Composing with LINQ</span></span>

<span data-ttu-id="b6919-129">Można skomponować na początku początkowej nieprzetworzonej kwerendy SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="b6919-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="b6919-130">EF Core będzie traktować go jako podkwerendy i komponować nad nim w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b6919-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="b6919-131">W poniższym przykładzie użyto nieprzetworzonej kwerendy SQL, która wybiera z funkcji wycenianej w tabeli (TVF).</span><span class="sxs-lookup"><span data-stu-id="b6919-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="b6919-132">A następnie komponuje się na nim za pomocą LINQ do filtrowania i sortowania.</span><span class="sxs-lookup"><span data-stu-id="b6919-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="b6919-133">Powyższa kwerenda generuje następujące SQL:</span><span class="sxs-lookup"><span data-stu-id="b6919-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="b6919-134">W tym powiązane dane</span><span class="sxs-lookup"><span data-stu-id="b6919-134">Including related data</span></span>

<span data-ttu-id="b6919-135">Metoda `Include` może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:</span><span class="sxs-lookup"><span data-stu-id="b6919-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="b6919-136">Tworzenie z LINQ wymaga nieprzetworzonej kwerendy SQL do komponowania, ponieważ EF Core będzie traktować dostarczony SQL jako podkwerendę.</span><span class="sxs-lookup"><span data-stu-id="b6919-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="b6919-137">Kwerendy SQL, które mogą być `SELECT` składane na początku od słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="b6919-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="b6919-138">Ponadto sql przekazywane nie powinny zawierać żadnych znaków lub opcji, które nie są prawidłowe w podkwerendy, takich jak:</span><span class="sxs-lookup"><span data-stu-id="b6919-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="b6919-139">Średnik spływowy</span><span class="sxs-lookup"><span data-stu-id="b6919-139">A trailing semicolon</span></span>
- <span data-ttu-id="b6919-140">Na programie SQL Server końcowa wskazówka na `OPTION (HASH JOIN)`poziomie kwerendy (na przykład)</span><span class="sxs-lookup"><span data-stu-id="b6919-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="b6919-141">W programie SQL `ORDER BY` Server: klauzula, `OFFSET 0` `TOP 100 PERCENT` która `SELECT` nie jest używana z or w klauzuli</span><span class="sxs-lookup"><span data-stu-id="b6919-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="b6919-142">SQL Server nie zezwala na tworzenie za pośrednictwem wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowy SQL.</span><span class="sxs-lookup"><span data-stu-id="b6919-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="b6919-143">Użyj `AsEnumerable` `AsAsyncEnumerable` lub metody `FromSqlRaw` `FromSqlInterpolated` zaraz po lub metody, aby upewnić się, że EF Core nie próbuje komponować za pomocą procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="b6919-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="b6919-144">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="b6919-144">Change Tracking</span></span>

<span data-ttu-id="b6919-145">Kwerendy, które `FromSqlRaw` `FromSqlInterpolated` używają lub metody wykonaj dokładnie te same reguły śledzenia zmian, jak inne zapytania LINQ w EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6919-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="b6919-146">Na przykład jeśli kwerendy projekty typów jednostek, wyniki będą śledzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b6919-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="b6919-147">W poniższym przykładzie użyto nieprzetworzonej kwerendy SQL, która wybiera z funkcji `AsNoTracking`tvf (Table-Valued Function), a następnie wyłącza śledzenie zmian za pomocą połączenia do:</span><span class="sxs-lookup"><span data-stu-id="b6919-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="b6919-148">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="b6919-148">Limitations</span></span>

<span data-ttu-id="b6919-149">Istnieje kilka ograniczeń, o których należy pamiętać podczas korzystania z nieprzetworzonych zapytań SQL:</span><span class="sxs-lookup"><span data-stu-id="b6919-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="b6919-150">Kwerenda SQL musi zwracać dane dla wszystkich właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="b6919-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="b6919-151">Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="b6919-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="b6919-152">Należy zauważyć, że to zachowanie różni się od EF6.</span><span class="sxs-lookup"><span data-stu-id="b6919-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="b6919-153">EF6 zignorował właściwość do mapowania kolumn dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały być zgodne z nazwami właściwości.</span><span class="sxs-lookup"><span data-stu-id="b6919-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="b6919-154">Kwerenda SQL nie może zawierać powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="b6919-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="b6919-155">Jednak w wielu przypadkach można skomponować na `Include` górze kwerendy za pomocą operatora do zwracania powiązanych danych (zobacz [Uwzględnienie powiązanych danych](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="b6919-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="b6919-156">Poprzednie wersje</span><span class="sxs-lookup"><span data-stu-id="b6919-156">Previous versions</span></span>

<span data-ttu-id="b6919-157">EF Core w wersji 2.2 i wcześniej `FromSql`miał dwa przeciążenia metody o `FromSqlRaw` nazwie, który zachowywał się w taki sam sposób jak nowsze i `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="b6919-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="b6919-158">Łatwo było przypadkowo wywołać metodę ciągu nieprzetworzonego, gdy intencją było wywołanie interpolowanej metody ciągu, a na odwrót.</span><span class="sxs-lookup"><span data-stu-id="b6919-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="b6919-159">Wywołanie nieprawidłowe przeciążenie przypadkowo może spowodować kwerendy nie są parametryzowane, kiedy powinny być.</span><span class="sxs-lookup"><span data-stu-id="b6919-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
