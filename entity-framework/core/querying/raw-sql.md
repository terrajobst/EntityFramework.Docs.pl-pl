---
title: Zapytania RAW SQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 7ed9a8938f8b6dffa7149d64d7e869b0b0078169
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232173"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="62d13-102">Zapytania SQL pierwotnych</span><span class="sxs-lookup"><span data-stu-id="62d13-102">Raw SQL Queries</span></span>

<span data-ttu-id="62d13-103">Entity Framework Core pozwala na liście rozwijanej raw kwerendy SQL podczas pracy z relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="62d13-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="62d13-104">Może to być przydatne, jeśli kwerenda, którą chcesz wykonać, nie można wyrazić za pomocą LINQ lub za pomocą zapytań LINQ wynikiem jest nieefektywne SQL wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="62d13-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span> <span data-ttu-id="62d13-105">Nieprzetworzona zapytania SQL może zwracać typów jednostek lub, w programie EF Core 2.1, [typy zapytań](xref:core/modeling/query-types) które są częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="62d13-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="62d13-106">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="62d13-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="62d13-107">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="62d13-107">Limitations</span></span>

<span data-ttu-id="62d13-108">Istnieje kilka pod uwagę podczas korzystania z pierwotnych zapytania SQL następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="62d13-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="62d13-109">Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki lub zapytania.</span><span class="sxs-lookup"><span data-stu-id="62d13-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="62d13-110">Nazwy kolumn w zestawie wyników muszą być zgodne nazwy kolumn, które są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="62d13-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="62d13-111">Należy zauważyć, że ta różni się od EF6 gdzie mapowania właściwości/kolumn została zignorowana dla nieprzetworzonej zapytania SQL i nazw musiały być zgodne z nazwami właściwości kolumny zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="62d13-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="62d13-112">Zapytania SQL nie może zawierać powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="62d13-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="62d13-113">Jednak w wielu przypadkach można utworzyć na podstawie zapytania za pomocą `Include` operatora, aby zwrócić dane dotyczące (zobacz [powiązanych danych w tym](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="62d13-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="62d13-114">`SELECT` instrukcje przekazane do tej metody powinno być zazwyczaj zezwala na składanie: Jeśli Core EF należy ocenić operatorów zapytań dodatkowe na serwerze (np. tłumaczenie operatory LINQ zastosowane po `FromSql`), podane SQL będzie traktowany jako podzapytania.</span><span class="sxs-lookup"><span data-stu-id="62d13-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="62d13-115">Oznacza to, SQL przekazany nie powinna zawierać żadnych znaków ani opcje, które nie są prawidłowe w podzapytaniu, takich jak:</span><span class="sxs-lookup"><span data-stu-id="62d13-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="62d13-116">końcowe średnikami</span><span class="sxs-lookup"><span data-stu-id="62d13-116">a trailing semicolon</span></span>
  * <span data-ttu-id="62d13-117">W programie SQL Server poziom zapytań końcowe wskazówki, np. `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="62d13-117">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="62d13-118">W programie SQL Server `ORDER BY` klauzuli, która nie jest powiązany z `TOP 100 PERCENT` w `SELECT` — klauzula</span><span class="sxs-lookup"><span data-stu-id="62d13-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="62d13-119">Instrukcje SQL innych niż `SELECT` są rozpoznawane automatycznie jako zezwalają na składanie.</span><span class="sxs-lookup"><span data-stu-id="62d13-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="62d13-120">W konsekwencji pełnych wyników procedury składowane są zawsze zwracana do klienta i wszystkie operatory LINQ zastosowane po `FromSql` są obliczane w pamięci.</span><span class="sxs-lookup"><span data-stu-id="62d13-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="62d13-121">Podstawowe raw zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="62d13-121">Basic raw SQL queries</span></span>

<span data-ttu-id="62d13-122">Można użyć *FromSql* — metoda rozszerzenia, aby rozpocząć zapytania LINQ na podstawie pierwotnych zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="62d13-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="62d13-123">Nieprzetworzona zapytania SQL może służyć do wykonywania procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="62d13-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="62d13-124">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="62d13-124">Passing parameters</span></span>

<span data-ttu-id="62d13-125">Podobnie jak w przypadku jakiegokolwiek interfejsu API, który akceptuje SQL, koniecznie parametryzacja wszystkie dane wejściowe użytkownika mają ochrony przed atakiem iniekcji kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="62d13-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="62d13-126">Może zawierać parametru symbole zastępcze w ciągu zapytania SQL, a następnie podaj wartości parametrów jako dodatkowe argumenty.</span><span class="sxs-lookup"><span data-stu-id="62d13-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="62d13-127">Należy podać wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="62d13-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="62d13-128">Poniższy przykład przekazuje jeden parametr do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="62d13-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="62d13-129">Gdy wygląda jak `String.Format` składni, podana wartość jest ujęte w where dodaje parametr i nazwę parametru wygenerowanego `{0}` symbol zastępczy został określony.</span><span class="sxs-lookup"><span data-stu-id="62d13-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="62d13-130">Jest to ten sam zapytania, ale przy użyciu składni interpolacji ciąg, który jest obsługiwany w programie EF Core 2.0 i nowszych wersjach:</span><span class="sxs-lookup"><span data-stu-id="62d13-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="62d13-131">Można również utworzyć DbParameter i podać go jako wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="62d13-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="62d13-132">Dzięki temu można używać parametrów nazwanych w ciągu zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="62d13-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="62d13-133">Tworzenie za pomocą LINQ</span><span class="sxs-lookup"><span data-stu-id="62d13-133">Composing with LINQ</span></span>

<span data-ttu-id="62d13-134">Jeśli zapytanie SQL mogą być składane na w bazie danych, można utworzyć na podstawie początkowego raw zapytania SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="62d13-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="62d13-135">Zapytania SQL, które mogą być składane z tego z `SELECT` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="62d13-135">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="62d13-136">W poniższym przykładzie użyto raw zapytanie SQL, który wybiera z funkcji zwracającej tabelę (TVF), a następnie Redaguj na nim za pomocą LINQ, aby wykonać filtrowanie i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="62d13-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="62d13-137">W tym powiązane dane</span><span class="sxs-lookup"><span data-stu-id="62d13-137">Including related data</span></span>

<span data-ttu-id="62d13-138">Tworzenie przy użyciu operatorów LINQ może służyć do uwzględnienia powiązanych danych w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="62d13-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="62d13-139">**Zawsze używaj parametryzacja raw kwerendy SQL:** interfejsów API, które akceptuje raw SQL string, takich jak `FromSql` i `ExecuteSqlCommand` zezwalają na wartości, które mają być łatwo przekazane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="62d13-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="62d13-140">Oprócz sprawdzania poprawności danych wejściowych użytkownika, należy zawsze używać parametryzacja dla każdej wartości nieprzetworzonej zapytania SQL/polecenia.</span><span class="sxs-lookup"><span data-stu-id="62d13-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="62d13-141">Jeśli używasz ciągów dynamicznie tworzenie dowolną część ciągu zapytania, a następnie jest odpowiedzialny za sprawdzanie poprawności żadnych danych do ochrony przez atakami iniekcji kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="62d13-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
