---
title: Pierwotne zapytania SQL - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 5bddddfbc2fe8d0ba99914f03b28bde4076fae42
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058717"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="baa7a-102">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="baa7a-102">Raw SQL Queries</span></span>

<span data-ttu-id="baa7a-103">Entity Framework Core umożliwia lista rozwijana umożliwiająca pierwotne zapytania SQL podczas pracy z usługą relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="baa7a-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="baa7a-104">Może to być przydatne, jeśli zapytanie, które należy wykonać, nie można wyrazić za pomocą LINQ lub przy użyciu zapytania LINQ wynikiem jest nieefektywne zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="baa7a-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="baa7a-105">Pierwotne zapytania SQL może zwrócić typów jednostek, lub, począwszy od programu EF Core 2.1, [typy zapytań](xref:core/modeling/query-types) będących częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="baa7a-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="baa7a-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="baa7a-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="baa7a-107">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="baa7a-107">Limitations</span></span>

<span data-ttu-id="baa7a-108">Istnieją pewne ograniczenia, które należy zwrócić uwagę podczas korzystania z pierwotne zapytania SQL:</span><span class="sxs-lookup"><span data-stu-id="baa7a-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="baa7a-109">Zapytanie SQL musi zwracać dane dotyczące wszystkich właściwości typu obiekt lub kwerendę.</span><span class="sxs-lookup"><span data-stu-id="baa7a-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="baa7a-110">Nazwy kolumn w zestawie wyników muszą być zgodne nazwy kolumn, które są mapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="baa7a-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="baa7a-111">Należy pamiętać, że to różni się od EF6 gdzie mapowania właściwości/kolumn została zignorowana dla pierwotne zapytania SQL i nazw musiały być zgodne z nazwami właściwości kolumny zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="baa7a-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="baa7a-112">Zapytania SQL nie może zawierać powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="baa7a-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="baa7a-113">Jednak w wielu przypadkach można utworzyć na podstawie zapytania za pomocą `Include` operator, aby zwrócić dane dotyczące (zobacz [powiązanych danych w tym](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="baa7a-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="baa7a-114">`SELECT` Konfigurowalna powinno być zazwyczaj instrukcji przekazany do tej metody: EF Core musi ocenić operatorów dodatkowych zapytań na serwerze (na przykład, aby przetłumaczyć operatorów LINQ zastosowane po `FromSql`), SQL podane są traktowane jak podzapytania.</span><span class="sxs-lookup"><span data-stu-id="baa7a-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="baa7a-115">Oznacza to, SQL przekazywane nie powinna zawierać żadnych znaków lub opcje, które nie są prawidłowe w podzapytaniu, takich jak:</span><span class="sxs-lookup"><span data-stu-id="baa7a-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="baa7a-116">końcowe średnikami</span><span class="sxs-lookup"><span data-stu-id="baa7a-116">a trailing semicolon</span></span>
  * <span data-ttu-id="baa7a-117">W programie SQL Server, końcowe wskazówka poziomie zapytania (na przykład `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="baa7a-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="baa7a-118">W programie SQL Server `ORDER BY` klauzula, która nie jest powiązany z `TOP 100 PERCENT` w `SELECT` — klauzula</span><span class="sxs-lookup"><span data-stu-id="baa7a-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="baa7a-119">Instrukcje SQL innych niż `SELECT` są rozpoznawane automatycznie jako innego niż konfigurowalna.</span><span class="sxs-lookup"><span data-stu-id="baa7a-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="baa7a-120">W konsekwencji pełnych wyników procedury składowane zawsze są zwracane do klienta i dowolnych operatorów LINQ zastosowane po `FromSql` jest oceniana w pamięci.</span><span class="sxs-lookup"><span data-stu-id="baa7a-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="baa7a-121">Podstawowe pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="baa7a-121">Basic raw SQL queries</span></span>

<span data-ttu-id="baa7a-122">Możesz użyć *FromSql* metodę rozszerzenia, aby rozpocząć zapytanie LINQ, na podstawie pierwotne zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="baa7a-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="baa7a-123">Pierwotne zapytania SQL może służyć do wykonywania procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="baa7a-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="baa7a-124">Przekazywanie parametrów</span><span class="sxs-lookup"><span data-stu-id="baa7a-124">Passing parameters</span></span>

<span data-ttu-id="baa7a-125">Podobnie jak w przypadku dowolnego interfejsu API, który akceptuje SQL, jest ważne, aby zdefiniować parametry wszystkie dane wejściowe użytkownika mają ochronę przed ataku polegającego na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="baa7a-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="baa7a-126">Można zawierają symbole zastępcze parametr ciągu zapytania SQL, a następnie podaj wartości parametrów jako dodatkowe argumenty.</span><span class="sxs-lookup"><span data-stu-id="baa7a-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="baa7a-127">Możesz podać wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="baa7a-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="baa7a-128">Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="baa7a-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="baa7a-129">Chociaż może to wyglądać jak `String.Format` składni, podana wartość jest opakowana w miejsce dodaje parametr i nazwę parametru wygenerowanego `{0}` określono symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="baa7a-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="baa7a-130">To jest taka sama zapytania, ale przy użyciu składni interpolacji ciągu, który jest obsługiwany w programie EF Core 2.0 i nowszych:</span><span class="sxs-lookup"><span data-stu-id="baa7a-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="baa7a-131">Można także skonstruować DbParameter i podać go jako wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="baa7a-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="baa7a-132">Dzięki temu można użyć nazwanych parametrów ciągu zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="baa7a-132">This allows you to use named parameters in the SQL query string.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="baa7a-133">Tworzenie za pomocą LINQ</span><span class="sxs-lookup"><span data-stu-id="baa7a-133">Composing with LINQ</span></span>

<span data-ttu-id="baa7a-134">Jeśli zapytanie SQL może być składana na w bazie danych, można utworzyć na podstawie początkowego pierwotne zapytania SQL przy użyciu operatorów LINQ.</span><span class="sxs-lookup"><span data-stu-id="baa7a-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="baa7a-135">Zapytania SQL, które mogą być składane na zaczynają się od `SELECT` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="baa7a-135">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="baa7a-136">W poniższym przykładzie użyto pierwotne zapytanie SQL, które wybiera z funkcji Table-Valued (TVF), a następnie komponuje się na nim za pomocą LINQ, aby wykonać filtrowanie i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="baa7a-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="baa7a-137">W tym powiązane dane</span><span class="sxs-lookup"><span data-stu-id="baa7a-137">Including related data</span></span>

<span data-ttu-id="baa7a-138">Tworzenie przy użyciu operatorów LINQ, może służyć do uwzględnienia powiązanych danych w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="baa7a-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="baa7a-139">**Zawsze używaj parametryzacji pierwotne zapytania SQL:** Interfejsy API, które akceptują pierwotne SQL string, takich jak `FromSql` i `ExecuteSqlCommand` Zezwalaj na wartości, które mają być łatwo przekazywane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="baa7a-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="baa7a-140">Oprócz sprawdzania poprawności danych wejściowych użytkownika, należy zawsze używać parametryzacji dla każdej wartości pierwotne zapytania SQL/polecenia.</span><span class="sxs-lookup"><span data-stu-id="baa7a-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="baa7a-141">Jeśli używasz ciągów dynamicznie tworzenie dowolnej części ciągu zapytania, a następnie ponosisz odpowiedzialność za sprawdzanie poprawności wszystkie dane wejściowe, aby zapewnić ochronę przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="baa7a-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
