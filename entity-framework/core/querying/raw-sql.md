---
title: Nieprzetworzone zapytania SQL — EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417671"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="10ff8-102">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="10ff8-102">Raw SQL Queries</span></span>

<span data-ttu-id="10ff8-103">Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="10ff8-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="10ff8-104">Surowe zapytania SQL są przydatne, jeśli nie można wyrazić tego zapytania przy użyciu LINQ.</span><span class="sxs-lookup"><span data-stu-id="10ff8-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="10ff8-105">Nieprzetworzone zapytania SQL są używane również wtedy, gdy użycie zapytania LINQ skutkuje nieefektywnym zapytaniem SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="10ff8-106">Surowe zapytania SQL mogą zwracać zwykłe typy jednostek lub [mniejsze typy jednostek](xref:core/modeling/keyless-entity-types) , które są częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="10ff8-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="10ff8-107">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="10ff8-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="10ff8-108">Podstawowe nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="10ff8-108">Basic raw SQL queries</span></span>

<span data-ttu-id="10ff8-109">Możesz użyć metody rozszerzenia `FromSqlRaw`, aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="10ff8-110">`FromSqlRaw` można używać tylko w przypadku katalogów głównych zapytań, które są bezpośrednio w `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="10ff8-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="10ff8-111">W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="10ff8-112">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="10ff8-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="10ff8-113">**Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL**</span><span class="sxs-lookup"><span data-stu-id="10ff8-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="10ff8-114">Wprowadzając wszelkie wartości podane przez użytkownika w nieprzetworzonej kwerendzie SQL, należy zachować ostrożność, aby uniknąć ataków iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="10ff8-115">Oprócz sprawdzania, czy takie wartości nie zawierają nieprawidłowych znaków, należy zawsze używać parametryzacja, które wysyła wartości odrębnie od tekstu SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="10ff8-116">W szczególności nigdy nie przekazuj ciągu połączonego lub interpolowanego (`$""`) z niezweryfikowanymi wartościami dostarczonymi przez użytkownika do `FromSqlRaw` lub `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="10ff8-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="10ff8-117">Metody `FromSqlInterpolated` i `ExecuteSqlInterpolated` umożliwiają używanie składni interpolacji ciągów w taki sposób, aby chronić przed atakami polegającymi na iniekcji kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="10ff8-118">Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i dostarczając dodatkowy argument.</span><span class="sxs-lookup"><span data-stu-id="10ff8-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="10ff8-119">Ta składnia może wyglądać podobnie jak składnia `String.Format`, podana wartość jest opakowana w `DbParameter` i wygenerowaną nazwą parametru wstawioną w miejscu, w którym został określony `{0}` symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="10ff8-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="10ff8-120">`FromSqlInterpolated` jest podobna do `FromSqlRaw`, ale umożliwia użycie składni interpolacji ciągów.</span><span class="sxs-lookup"><span data-stu-id="10ff8-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="10ff8-121">Podobnie jak `FromSqlRaw`, `FromSqlInterpolated` można używać tylko w przypadku katalogów głównych zapytań.</span><span class="sxs-lookup"><span data-stu-id="10ff8-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="10ff8-122">Jak w poprzednim przykładzie, wartość jest konwertowana na `DbParameter` i nie jest narażona na wstrzyknięcie kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="10ff8-123">Przed wersjami 3,0 `FromSqlRaw` i `FromSqlInterpolated` były dwa przeciążenia o nazwie `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="10ff8-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="10ff8-124">Aby uzyskać więcej informacji, zobacz [sekcję poprzednie wersje](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="10ff8-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="10ff8-125">Możesz również skonstruować DbParameter i podać go jako wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="10ff8-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="10ff8-126">Ponieważ jest używany zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy ciągu, `FromSqlRaw` można bezpiecznie użyć:</span><span class="sxs-lookup"><span data-stu-id="10ff8-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="10ff8-127">`FromSqlRaw` pozwala używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="10ff8-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="10ff8-128">Tworzenie za pomocą LINQ</span><span class="sxs-lookup"><span data-stu-id="10ff8-128">Composing with LINQ</span></span>

<span data-ttu-id="10ff8-129">Możesz złożyć na początku wstępnego nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="10ff8-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="10ff8-130">EF Core traktuje ją jako podzapytanie i redaguje ją w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="10ff8-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="10ff8-131">Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF).</span><span class="sxs-lookup"><span data-stu-id="10ff8-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="10ff8-132">A następnie redaguje przy użyciu LINQ, aby przeprowadzić filtrowanie i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="10ff8-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="10ff8-133">Powyższe zapytanie generuje następujący kod SQL:</span><span class="sxs-lookup"><span data-stu-id="10ff8-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="10ff8-134">Uwzględnianie danych pokrewnych</span><span class="sxs-lookup"><span data-stu-id="10ff8-134">Including related data</span></span>

<span data-ttu-id="10ff8-135">Metoda `Include` może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:</span><span class="sxs-lookup"><span data-stu-id="10ff8-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="10ff8-136">Redagowanie przy użyciu LINQ wymaga, aby pierwotne zapytanie SQL było możliwe do utworzenia, ponieważ EF Core będzie traktować dane SQL jako podzapytanie.</span><span class="sxs-lookup"><span data-stu-id="10ff8-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="10ff8-137">Zapytania SQL, które mogą być składane na początku za pomocą słowa kluczowego `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="10ff8-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="10ff8-138">Dodatkowo przekazanie danych SQL nie powinno zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:</span><span class="sxs-lookup"><span data-stu-id="10ff8-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="10ff8-139">Końcowy średnik</span><span class="sxs-lookup"><span data-stu-id="10ff8-139">A trailing semicolon</span></span>
- <span data-ttu-id="10ff8-140">Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="10ff8-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="10ff8-141">Na SQL Server, klauzula `ORDER BY`, która nie jest używana z `OFFSET 0` lub `TOP 100 PERCENT` w klauzuli `SELECT`</span><span class="sxs-lookup"><span data-stu-id="10ff8-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="10ff8-142">SQL Server nie zezwala na tworzenie wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowe użycie języka SQL.</span><span class="sxs-lookup"><span data-stu-id="10ff8-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="10ff8-143">Użyj metody `AsEnumerable` lub `AsAsyncEnumerable` bezpośrednio po `FromSqlRaw` lub `FromSqlInterpolated` metod, aby upewnić się, że EF Core nie próbuje utworzyć przez procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="10ff8-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="10ff8-144">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="10ff8-144">Change Tracking</span></span>

<span data-ttu-id="10ff8-145">Zapytania, które używają metod `FromSqlRaw` lub `FromSqlInterpolated`, są zgodne ze szczegółowymi regułami śledzenia zmian, co inne zapytania LINQ w EF Core.</span><span class="sxs-lookup"><span data-stu-id="10ff8-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="10ff8-146">Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="10ff8-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="10ff8-147">Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="10ff8-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="10ff8-148">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="10ff8-148">Limitations</span></span>

<span data-ttu-id="10ff8-149">Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:</span><span class="sxs-lookup"><span data-stu-id="10ff8-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="10ff8-150">Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="10ff8-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="10ff8-151">Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="10ff8-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="10ff8-152">Należy zauważyć, że to zachowanie różni się od EF6.</span><span class="sxs-lookup"><span data-stu-id="10ff8-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="10ff8-153">EF6 zignorował właściwość do mapowania kolumn dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="10ff8-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="10ff8-154">Zapytanie SQL nie może zawierać powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="10ff8-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="10ff8-155">Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora `Include`, aby zwrócić powiązane dane (zobacz [m.in. powiązane dane](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="10ff8-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="10ff8-156">Poprzednie wersje</span><span class="sxs-lookup"><span data-stu-id="10ff8-156">Previous versions</span></span>

<span data-ttu-id="10ff8-157">EF Core w wersji 2,2 i starszych miała dwa przeciążenia metody o nazwie `FromSql`, które zadziałały w taki sam sposób jak nowsze `FromSqlRaw` i `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="10ff8-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="10ff8-158">Można łatwo przypadkowo wywołać metodę nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="10ff8-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="10ff8-159">Przypadkowe wywołanie nieprawidłowego przeciążenia może spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="10ff8-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
