---
title: Nieprzetworzone zapytania SQL — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b0c9ba1bb452e47e8348d000e3f7b88cc2730d8e
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149300"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ad13e-102">Nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="ad13e-102">Raw SQL Queries</span></span>

<span data-ttu-id="ad13e-103">Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="ad13e-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="ad13e-104">Może to być przydatne, jeśli zapytanie, które chcesz wykonać, nie może być wyrażone przy użyciu LINQ, lub jeśli użycie zapytania LINQ jest wynikiem nieefektywnych zapytań SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="ad13e-105">Surowe zapytania SQL mogą zwracać typy jednostek lub, rozpoczynając od EF Core 2,1, [typy jednostek](xref:core/modeling/keyless-entity-types) , które są częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="ad13e-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="ad13e-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ad13e-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="ad13e-107">Podstawowe nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="ad13e-107">Basic raw SQL queries</span></span>

<span data-ttu-id="ad13e-108">Możesz użyć metody rozszerzenia *z tabel* , aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="ad13e-109">W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="ad13e-110">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="ad13e-110">Passing parameters</span></span>

<span data-ttu-id="ad13e-111">Podobnie jak w przypadku dowolnego interfejsu API, który akceptuje SQL, ważne jest, aby Sparametryzuj wszelkie dane wprowadzane przez użytkownika w celu ochrony przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="ad13e-112">Można dołączać symbole zastępcze parametrów w ciągu zapytania SQL, a następnie podawać wartości parametrów jako dodatkowe argumenty.</span><span class="sxs-lookup"><span data-stu-id="ad13e-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="ad13e-113">Wszelkie podawane wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="ad13e-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="ad13e-114">Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="ad13e-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="ad13e-115">Chociaż może to wyglądać podobnie `String.Format` do składni, podana wartość jest opakowana w parametr i wygenerowaną nazwą parametru wstawioną, `{0}` gdzie symbol zastępczy został określony.</span><span class="sxs-lookup"><span data-stu-id="ad13e-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="ad13e-116">To jest to samo zapytanie, ale przy użyciu składni interpolacji ciągów, która jest obsługiwana w EF Core 2,0 i powyżej:</span><span class="sxs-lookup"><span data-stu-id="ad13e-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="ad13e-117">Możesz również skonstruować DbParameter i podać go jako wartość parametru:</span><span class="sxs-lookup"><span data-stu-id="ad13e-117">You can also construct a DbParameter and supply it as a parameter value:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="ad13e-118">Pozwala to używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="ad13e-118">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="ad13e-119">Tworzenie za pomocą LINQ</span><span class="sxs-lookup"><span data-stu-id="ad13e-119">Composing with LINQ</span></span>

<span data-ttu-id="ad13e-120">Jeśli zapytanie SQL może składać się z bazy danych, możesz utworzyć na początku pierwotnego, nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="ad13e-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="ad13e-121">Zapytania SQL, które mogą być składane na początku `SELECT` ze słowem kluczowym.</span><span class="sxs-lookup"><span data-stu-id="ad13e-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="ad13e-122">W poniższym przykładzie użyto nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie tworzy je przy użyciu LINQ do wykonywania filtrowania i sortowania.</span><span class="sxs-lookup"><span data-stu-id="ad13e-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="ad13e-123">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="ad13e-123">Change Tracking</span></span>

<span data-ttu-id="ad13e-124">Zapytania, które używają `FromSql()` dokładnie tych samych reguł śledzenia zmian, jak inne zapytania LINQ w EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad13e-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="ad13e-125">Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ad13e-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="ad13e-126">Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem. AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="ad13e-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="ad13e-127">Uwzględnianie danych pokrewnych</span><span class="sxs-lookup"><span data-stu-id="ad13e-127">Including related data</span></span>

<span data-ttu-id="ad13e-128">`Include()` Metoda może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:</span><span class="sxs-lookup"><span data-stu-id="ad13e-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="ad13e-129">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="ad13e-129">Limitations</span></span>

<span data-ttu-id="ad13e-130">Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:</span><span class="sxs-lookup"><span data-stu-id="ad13e-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="ad13e-131">Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki lub zapytania.</span><span class="sxs-lookup"><span data-stu-id="ad13e-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="ad13e-132">Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="ad13e-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="ad13e-133">Należy zauważyć, że różni się od EF6, gdzie mapowanie właściwości i kolumn zostało zignorowane dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="ad13e-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="ad13e-134">Zapytanie SQL nie może zawierać powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="ad13e-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="ad13e-135">Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora, `Include` aby zwrócić powiązane dane (zobacz m.in. [powiązane dane](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="ad13e-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="ad13e-136">`SELECT`instrukcje przesłane do tej metody powinny być zwykle możliwe do przesłania: Jeśli EF Core musi oszacować dodatkowe operatory zapytań na serwerze (na przykład w celu przetłumaczenia operatorów LINQ zastosowanych po `FromSql`), podana wartość SQL będzie traktowana jako podzapytanie.</span><span class="sxs-lookup"><span data-stu-id="ad13e-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="ad13e-137">Oznacza to, że przesłany kod SQL nie powinien zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:</span><span class="sxs-lookup"><span data-stu-id="ad13e-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="ad13e-138">końcowy średnik</span><span class="sxs-lookup"><span data-stu-id="ad13e-138">a trailing semicolon</span></span>
  * <span data-ttu-id="ad13e-139">Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="ad13e-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="ad13e-140">Na SQL Server, `ORDER BY` klauzula, do której nie dołączono `OFFSET 0` ani `TOP 100 PERCENT` w `SELECT` klauzuli</span><span class="sxs-lookup"><span data-stu-id="ad13e-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="ad13e-141">Instrukcje SQL inne niż `SELECT` są rozpoznawane automatycznie jako nieumożliwiające tworzenie.</span><span class="sxs-lookup"><span data-stu-id="ad13e-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="ad13e-142">W związku z tym pełne wyniki procedur składowanych są zawsze zwracane do klienta i wszystkie operatory LINQ zastosowane po `FromSql` zakończeniu są oceniane w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ad13e-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="ad13e-143">**Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL:** Oprócz sprawdzania poprawności danych wejściowych użytkownika należy zawsze używać parametryzacja dla wszystkich wartości używanych w nieprzetworzonym zapytaniu SQL/poleceniu.</span><span class="sxs-lookup"><span data-stu-id="ad13e-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="ad13e-144">Interfejsy API, które akceptują nieprzetworzony ciąg `FromSql` SQL `ExecuteSqlCommand` , takie jak i zezwalają na łatwe przekazywanie wartości jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ad13e-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="ad13e-145">`FromSql` Przeciążenia i `ExecuteSqlCommand` akceptujące FormattableString umożliwiają również korzystanie z składni interpolacji ciągów w taki sposób, aby pomóc w ochronie przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntax in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="ad13e-146">Jeśli używasz łączenia ciągów lub interpolacji do dynamicznego kompilowania dowolnej części ciągu zapytania lub przekazywania danych wejściowych użytkownika do instrukcji lub procedur składowanych, które mogą wykonywać te dane wejściowe jako dynamiczny SQL, wówczas użytkownik jest odpowiedzialny za sprawdzanie poprawności wszelkich danych wejściowych Ochrona przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="ad13e-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
