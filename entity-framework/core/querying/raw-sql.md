---
title: Nieprzetworzone zapytania SQL — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813593"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="663f8-102">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="663f8-102">Raw SQL Queries</span></span>

<span data-ttu-id="663f8-103">Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="663f8-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="663f8-104">Może to być przydatne, jeśli zapytanie, które chcesz wykonać, nie może być wyrażone przy użyciu LINQ, lub jeśli użycie zapytania LINQ jest wynikiem niewydajnego zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="663f8-105">Surowe zapytania SQL mogą zwracać zwykłe typy jednostek lub [mniejsze typy jednostek](xref:core/modeling/keyless-entity-types) , które są częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="663f8-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="663f8-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="663f8-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="663f8-107">Podstawowe nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="663f8-107">Basic raw SQL queries</span></span>

<span data-ttu-id="663f8-108">Możesz użyć metody rozszerzenia `FromSqlRaw` , aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="663f8-109">W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="663f8-110">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="663f8-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="663f8-111">**Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL**</span><span class="sxs-lookup"><span data-stu-id="663f8-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="663f8-112">Wprowadzając wszelkie wartości podane przez użytkownika w nieprzetworzonej kwerendzie SQL, należy zachować ostrożność, aby uniknąć ataków iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="663f8-113">Oprócz sprawdzania, czy takie wartości nie zawierają nieprawidłowych znaków, należy zawsze używać parametryzacja, które wysyła wartości odrębnie od tekstu SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="663f8-114">W szczególności nigdy nie przekazuj połączonego lub interpolowanego ciągu (`$""`) z niezweryfikowanymi wartościami dostarczonymi przez użytkownika do `FromSqlRaw` lub. `ExecuteSqlRaw`</span><span class="sxs-lookup"><span data-stu-id="663f8-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="663f8-115">Metody `FromSqlInterpolated` i`ExecuteSqlInterpolated` umożliwiają używanie składni interpolacji ciągów w sposób chroniący przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="663f8-116">Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i dostarczając dodatkowy argument.</span><span class="sxs-lookup"><span data-stu-id="663f8-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="663f8-117">Chociaż może to wyglądać podobnie `String.Format` do składni, podana wartość jest opakowana `DbParameter` w i wygenerowana nazwa `{0}` parametru, gdzie symbol zastępczy został określony.</span><span class="sxs-lookup"><span data-stu-id="663f8-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="663f8-118">Jako alternatywę dla `FromSqlRaw`, można użyć `FromSqlInterpolated` , która umożliwia bezpieczne korzystanie z interpolacji ciągów.</span><span class="sxs-lookup"><span data-stu-id="663f8-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="663f8-119">Tak jak w poprzednim przykładzie, wartość jest konwertowana na `DbParameter` i dlatego nie jest narażona na wstrzyknięcie kodu SQL:</span><span class="sxs-lookup"><span data-stu-id="663f8-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="663f8-120">Przed wersjami 3,0 `FromSqlRaw` i `FromSqlInterpolated` były dwa przeciążenia o nazwie `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="663f8-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="663f8-121">Zobacz [sekcję poprzednie wersje](#previous-versions) , aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="663f8-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="663f8-122">Możesz również skonstruować DbParameter i podać go jako wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="663f8-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="663f8-123">Ponieważ jest używany zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy `FromSqlRaw` ciągu, można bezpiecznie użyć:</span><span class="sxs-lookup"><span data-stu-id="663f8-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="663f8-124">Pozwala to używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="663f8-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="663f8-125">Tworzenie za pomocą LINQ</span><span class="sxs-lookup"><span data-stu-id="663f8-125">Composing with LINQ</span></span>

<span data-ttu-id="663f8-126">Jeśli zapytanie SQL może składać się z bazy danych, możesz utworzyć na początku pierwotnego, nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="663f8-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="663f8-127">Zapytania SQL, które mogą być składane na początku `SELECT` ze słowem kluczowym.</span><span class="sxs-lookup"><span data-stu-id="663f8-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="663f8-128">W poniższym przykładzie użyto nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie tworzy je przy użyciu LINQ do wykonywania filtrowania i sortowania.</span><span class="sxs-lookup"><span data-stu-id="663f8-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="663f8-129">Spowoduje to wygenerowanie następującej kwerendy SQL:</span><span class="sxs-lookup"><span data-stu-id="663f8-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="663f8-130">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="663f8-130">Change Tracking</span></span>

<span data-ttu-id="663f8-131">Zapytania, które używają `FromSql` metod, są zgodne ze szczegółowymi regułami śledzenia zmian co inne zapytania LINQ w EF Core.</span><span class="sxs-lookup"><span data-stu-id="663f8-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="663f8-132">Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="663f8-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="663f8-133">Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="663f8-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="663f8-134">Uwzględnianie danych pokrewnych</span><span class="sxs-lookup"><span data-stu-id="663f8-134">Including related data</span></span>

<span data-ttu-id="663f8-135">`Include` Metoda może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:</span><span class="sxs-lookup"><span data-stu-id="663f8-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="663f8-136">Należy pamiętać, że wymaga to utworzenia pierwotnego zapytania SQL; nie będzie to szczególnie możliwe w przypadku wywołań procedur przechowywanych.</span><span class="sxs-lookup"><span data-stu-id="663f8-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="663f8-137">Zapoznaj się z uwagami dotyczącymi możliwości tworzenia w obszarze [ograniczenia](#limitations)).</span><span class="sxs-lookup"><span data-stu-id="663f8-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="663f8-138">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="663f8-138">Limitations</span></span>

<span data-ttu-id="663f8-139">Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:</span><span class="sxs-lookup"><span data-stu-id="663f8-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="663f8-140">Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="663f8-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="663f8-141">Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="663f8-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="663f8-142">Należy zauważyć, że różni się od EF6, gdzie mapowanie właściwości i kolumn zostało zignorowane dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="663f8-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="663f8-143">Zapytanie SQL nie może zawierać powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="663f8-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="663f8-144">Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora, `Include` aby zwrócić powiązane dane (zobacz m.in. [powiązane dane](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="663f8-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="663f8-145">`SELECT`instrukcje przesłane do tej metody powinny być zwykle możliwe do przesłania: Jeśli EF Core musi oszacować dodatkowe operatory zapytań na serwerze (na przykład w celu przetłumaczenia operatorów LINQ zastosowanych po `FromSql` metodach), podana wartość SQL będzie traktowana jako podzapytanie.</span><span class="sxs-lookup"><span data-stu-id="663f8-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="663f8-146">Oznacza to, że przesłany kod SQL nie powinien zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:</span><span class="sxs-lookup"><span data-stu-id="663f8-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="663f8-147">końcowy średnik</span><span class="sxs-lookup"><span data-stu-id="663f8-147">A trailing semicolon</span></span>
  * <span data-ttu-id="663f8-148">Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="663f8-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="663f8-149">Na SQL Server, `ORDER BY` klauzula, do której nie dołączono `OFFSET 0` ani `TOP 100 PERCENT` w `SELECT` klauzuli</span><span class="sxs-lookup"><span data-stu-id="663f8-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="663f8-150">Należy pamiętać, że SQL Server nie zezwala na tworzenie wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowe użycie języka SQL.</span><span class="sxs-lookup"><span data-stu-id="663f8-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="663f8-151">Operatory zapytań można wprowadzać po `AsEnumerable()` przeprowadzeniu oceny klienta.</span><span class="sxs-lookup"><span data-stu-id="663f8-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="663f8-152">Poprzednie wersje</span><span class="sxs-lookup"><span data-stu-id="663f8-152">Previous versions</span></span>

<span data-ttu-id="663f8-153">EF Core wersja 2,2 i wcześniejsze mają dwa przeciążenia o `FromSql` nazwach, które zachowują się w taki sam `FromSqlRaw` sposób `FromSqlInterpolated`jak nowsze i.</span><span class="sxs-lookup"><span data-stu-id="663f8-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="663f8-154">Dzięki temu bardzo łatwo można przypadkowo wywołać metodę nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="663f8-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="663f8-155">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="663f8-155">This could result in queries not being parameterized when they should have been.</span></span>
