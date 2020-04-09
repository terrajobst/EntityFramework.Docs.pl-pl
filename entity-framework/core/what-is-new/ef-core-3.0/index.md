---
title: Nowe funkcje w entity framework core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417943"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="5fb30-102">Nowe funkcje w entity framework core 3.0</span><span class="sxs-lookup"><span data-stu-id="5fb30-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="5fb30-103">Poniższa lista zawiera główne nowe funkcje w EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="5fb30-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="5fb30-104">Jako główne wydanie EF Core 3.0 zawiera również kilka [istotnych zmian,](xref:core/what-is-new/ef-core-3.0/breaking-changes)które są ulepszenia interfejsu API, które mogą mieć negatywny wpływ na istniejące aplikacje.</span><span class="sxs-lookup"><span data-stu-id="5fb30-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="5fb30-105">Remont LINQ</span><span class="sxs-lookup"><span data-stu-id="5fb30-105">LINQ overhaul</span></span>

<span data-ttu-id="5fb30-106">LINQ umożliwia pisanie zapytań bazy danych przy użyciu wybranego języka .NET, korzystając z informacji o typie rozszerzonym, aby oferować intellisense i sprawdzanie typu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5fb30-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="5fb30-107">Ale LINQ umożliwia również pisanie nieograniczonej liczby skomplikowanych zapytań zawierających dowolne wyrażenia (wywołania metody lub operacje).</span><span class="sxs-lookup"><span data-stu-id="5fb30-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="5fb30-108">Jak obsługiwać wszystkie te kombinacje jest głównym wyzwaniem dla dostawców LINQ.</span><span class="sxs-lookup"><span data-stu-id="5fb30-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="5fb30-109">W EF Core 3.0 firma Microsoft rearchitected naszego dostawcy LINQ, aby umożliwić tłumaczenie więcej wzorców zapytań do SQL, generowanie efektywnych zapytań w większej liczbie przypadków i zapobieganie nieefektywne zapytania nie zostaniewykryte.</span><span class="sxs-lookup"><span data-stu-id="5fb30-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="5fb30-110">Nowy dostawca LINQ jest podstawą, nad którą będziemy mogli zaoferować nowe możliwości zapytań i ulepszenia wydajności w przyszłych wersjach, bez przerywania istniejących aplikacji i dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="5fb30-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="5fb30-111">Ograniczona ocena klienta</span><span class="sxs-lookup"><span data-stu-id="5fb30-111">Restricted client evaluation</span></span>

<span data-ttu-id="5fb30-112">Najważniejsza zmiana projektu ma do czynienia z tym, jak obsługujemy wyrażenia LINQ, które nie mogą być konwertowane na parametry lub przetłumaczone na SQL.</span><span class="sxs-lookup"><span data-stu-id="5fb30-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="5fb30-113">W poprzednich wersjach EF Core zidentyfikowano, jakie części kwerendy można przetłumaczyć na język SQL, a resztę zapytania wykonał na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5fb30-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="5fb30-114">Ten typ wykonywania po stronie klienta jest pożądane w niektórych sytuacjach, ale w wielu innych przypadkach może spowodować nieefektywne zapytania.</span><span class="sxs-lookup"><span data-stu-id="5fb30-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="5fb30-115">Na przykład jeśli EF Core 2.2 nie może przetłumaczyć predykatu w `Where()` wywołaniu, wykonał instrukcję SQL bez filtru, przesłał wszystkie wiersze z bazy danych, a następnie odfiltrował je w pamięci:</span><span class="sxs-lookup"><span data-stu-id="5fb30-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="5fb30-116">Może to być dopuszczalne, jeśli baza danych zawiera niewielką liczbę wierszy, ale może spowodować znaczne problemy z wydajnością lub nawet niepowodzenie aplikacji, jeśli baza danych zawiera dużą liczbę wierszy.</span><span class="sxs-lookup"><span data-stu-id="5fb30-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number of rows.</span></span>

<span data-ttu-id="5fb30-117">W EF Core 3.0 ograniczyliśmy ocenę klienta tylko do projekcji najwyższego poziomu `Select()`(zasadniczo ostatnie wezwanie).</span><span class="sxs-lookup"><span data-stu-id="5fb30-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="5fb30-118">Gdy EF Core 3.0 wykrywa wyrażenia, które nie mogą być tłumaczone w innym miejscu w kwerendzie, zgłasza wyjątek środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="5fb30-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="5fb30-119">Aby ocenić warunek predykatu na kliencie, jak w poprzednim przykładzie, deweloperzy muszą teraz jawnie przełączyć ocenę kwerendy do LINQ do obiektów:</span><span class="sxs-lookup"><span data-stu-id="5fb30-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="5fb30-120">Zobacz [dokumentację zmian podziału,](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) aby uzyskać więcej informacji na temat tego, jak może to wpłynąć na istniejące aplikacje.</span><span class="sxs-lookup"><span data-stu-id="5fb30-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="5fb30-121">Pojedyncza instrukcja SQL na kwerendę LINQ</span><span class="sxs-lookup"><span data-stu-id="5fb30-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="5fb30-122">Innym aspektem projektu, który zmienił się znacząco w 3.0 jest to, że teraz zawsze generuje jedną instrukcję SQL dla zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="5fb30-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="5fb30-123">W poprzednich wersjach użyliśmy do generowania wielu `Include()` instrukcji SQL w niektórych przypadkach, przetłumaczone wywołania właściwości nawigacji kolekcji i przetłumaczone zapytania, które następnie niektóre wzorce z podksy.</span><span class="sxs-lookup"><span data-stu-id="5fb30-123">In previous versions, we used to generate multiple SQL statements in certain cases, translated `Include()` calls on collection navigation properties and translated queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="5fb30-124">Chociaż w niektórych przypadkach było `Include()` to wygodne, a nawet pomogło uniknąć wysyłania nadmiarowych danych przez sieć, implementacja była złożona i spowodowało pewne skrajnie nieefektywne zachowania (zapytania N +1).</span><span class="sxs-lookup"><span data-stu-id="5fb30-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, and it resulted in some extremely inefficient behaviors (N+1 queries).</span></span> <span data-ttu-id="5fb30-125">Były sytuacje, w których dane zwracane w wielu kwerendach był potencjalnie niespójne.</span><span class="sxs-lookup"><span data-stu-id="5fb30-125">There were situations in which the data returned across multiple queries was potentially inconsistent.</span></span>

<span data-ttu-id="5fb30-126">Podobnie jak w przypadku oceny klienta, jeśli EF Core 3.0 nie można przetłumaczyć zapytania LINQ na jedną instrukcję SQL, zgłasza wyjątek środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="5fb30-126">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="5fb30-127">Ale zrobiliśmy EF Core stanie tłumaczyć wiele typowych wzorców, które były używane do generowania wielu zapytań do pojedynczej kwerendy z jonów.</span><span class="sxs-lookup"><span data-stu-id="5fb30-127">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="5fb30-128">Pomoc techniczna aplikacji Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5fb30-128">Cosmos DB support</span></span>

<span data-ttu-id="5fb30-129">Dostawca usługi Cosmos DB dla ef core umożliwia deweloperom zaznajomionym z modelem programowania EF łatwo kierować usługi Azure Cosmos DB jako bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5fb30-129">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="5fb30-130">Celem jest, aby niektóre z zalet usługi Cosmos DB, takich jak dystrybucja globalna, "zawsze włączony" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5fb30-130">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="5fb30-131">Dostawca włącza większość funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w usłudze Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5fb30-131">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="5fb30-132">Aby uzyskać więcej informacji, zobacz [dokumentację dostawcy usługi Cosmos DB.](xref:core/providers/cosmos/index)</span><span class="sxs-lookup"><span data-stu-id="5fb30-132">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="5fb30-133">Pomoc techniczna języka C# 8.0</span><span class="sxs-lookup"><span data-stu-id="5fb30-133">C# 8.0 support</span></span>

<span data-ttu-id="5fb30-134">EF Core 3.0 korzysta z kilku [nowych funkcji w języku C# 8.0:](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)</span><span class="sxs-lookup"><span data-stu-id="5fb30-134">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="5fb30-135">Strumienie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="5fb30-135">Asynchronous streams</span></span>

<span data-ttu-id="5fb30-136">Asynchroniczne wyniki kwerendy są teraz `IAsyncEnumerable<T>` udostępniane przy użyciu `await foreach`nowego standardowego interfejsu i mogą być używane przy użyciu programu .</span><span class="sxs-lookup"><span data-stu-id="5fb30-136">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

<span data-ttu-id="5fb30-137">Zobacz [strumieni asynchronicznych w dokumentacji języka C#,](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5fb30-137">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="5fb30-138">Typy referencyjne dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="5fb30-138">Nullable reference types</span></span>

<span data-ttu-id="5fb30-139">Gdy ta nowa funkcja jest włączona w kodzie, EF Core sprawdza nullability właściwości typu odwołania i stosuje go do odpowiednich kolumn i relacji w bazie danych: `[Required]` właściwości typów odwołań niepodważalnych są traktowane tak, jakby miały atrybut adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5fb30-139">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="5fb30-140">Na przykład w następującej klasie właściwości oznaczone `string?` jako typu zostaną skonfigurowane `string` jako opcjonalne, podczas gdy będą skonfigurowane zgodnie z wymaganiami:</span><span class="sxs-lookup"><span data-stu-id="5fb30-140">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

<span data-ttu-id="5fb30-141">Zobacz [Praca z typami odwołań nullable](xref:core/miscellaneous/nullable-reference-types) w dokumentacji EF Core, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5fb30-141">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="5fb30-142">Przechwytywanie operacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="5fb30-142">Interception of database operations</span></span>

<span data-ttu-id="5fb30-143">Nowy interfejs API przechwytywania w EF Core 3.0 umożliwia zapewnienie niestandardowej logiki do wywoływania automatycznie, gdy operacje niskiego poziomu bazy danych występują w ramach normalnego działania EF Core.</span><span class="sxs-lookup"><span data-stu-id="5fb30-143">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="5fb30-144">Na przykład podczas otwierania połączeń, zatwierdzania transakcji lub wykonywania poleceń.</span><span class="sxs-lookup"><span data-stu-id="5fb30-144">For example, when opening connections, committing transactions, or executing commands.</span></span>

<span data-ttu-id="5fb30-145">Podobnie jak funkcje przechwytywania, które istniały w EF 6, interceptory umożliwiają przechwytywanie operacji przed lub po ich wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="5fb30-145">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="5fb30-146">Po przechwyceniu ich, zanim się zdarzyć, można by-pass wykonanie i dostarczyć alternatywne wyniki z logiki przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="5fb30-146">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span>

<span data-ttu-id="5fb30-147">Na przykład, aby manipulować tekstem `IDbCommandInterceptor`polecenia, można utworzyć :</span><span class="sxs-lookup"><span data-stu-id="5fb30-147">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

<span data-ttu-id="5fb30-148">I zarejestruj go `DbContext`w swoim:</span><span class="sxs-lookup"><span data-stu-id="5fb30-148">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="5fb30-149">Inżynieria odwrotna widoków bazy danych</span><span class="sxs-lookup"><span data-stu-id="5fb30-149">Reverse engineering of database views</span></span>

<span data-ttu-id="5fb30-150">Nazwy typów kwerend, które reprezentują dane, które mogą być odczytywane z bazy danych, ale nie są aktualizowane, zostały zmienione na [typy jednostek bezkluczeń](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="5fb30-150">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span>
<span data-ttu-id="5fb30-151">Ponieważ są one doskonałe dopasowanie do mapowania widoków bazy danych w większości scenariuszy, EF Core teraz automatycznie tworzy typy jednostek bezkluzyfowych, gdy widoki bazy danych inżynierii odwrotnej.</span><span class="sxs-lookup"><span data-stu-id="5fb30-151">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="5fb30-152">Na przykład za pomocą [narzędzia wiersza polecenia dotnet ef](xref:core/miscellaneous/cli/dotnet) można wpisać:</span><span class="sxs-lookup"><span data-stu-id="5fb30-152">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="5fb30-153">A narzędzie będzie teraz automatycznie szkielety typów widoków i tabel bez klawiszy:</span><span class="sxs-lookup"><span data-stu-id="5fb30-153">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5fb30-154">Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="5fb30-154">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="5fb30-155">Począwszy od EF Core 3.0, `OrderDetails` jeśli jest własnością `Order` lub jawnie mapowane do `Order` tej `OrderDetails` samej `OrderDetails` tabeli, będzie można dodać bez i wszystkie właściwości, z wyjątkiem klucza podstawowego będą mapowane do kolumn nullable.</span><span class="sxs-lookup"><span data-stu-id="5fb30-155">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="5fb30-156">Podczas wykonywania zapytań EF `OrderDetails` `null` Core ustawi się, jeśli którakolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie `null`ma wymaganych właściwości oprócz klucza podstawowego i wszystkie właściwości są .</span><span class="sxs-lookup"><span data-stu-id="5fb30-156">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="5fb30-157">EF 6.3 na .NET Core</span><span class="sxs-lookup"><span data-stu-id="5fb30-157">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="5fb30-158">Nie jest to funkcja EF Core 3.0, ale uważamy, że jest to ważne dla wielu naszych obecnych klientów.</span><span class="sxs-lookup"><span data-stu-id="5fb30-158">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span>

<span data-ttu-id="5fb30-159">Rozumiemy, że wiele istniejących aplikacji używa poprzednich wersji ef i że przenoszenie ich do EF Core tylko w celu skorzystania z .NET Core może wymagać znacznego wysiłku.</span><span class="sxs-lookup"><span data-stu-id="5fb30-159">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span>
<span data-ttu-id="5fb30-160">Z tego powodu zdecydowaliśmy się przenieść najnowszą wersję EF 6 do uruchomienia na .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="5fb30-160">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span>

<span data-ttu-id="5fb30-161">Aby uzyskać więcej informacji, [zobacz, co nowego w EF 6](xref:ef6/what-is-new/index).</span><span class="sxs-lookup"><span data-stu-id="5fb30-161">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="5fb30-162">Przełożone funkcje</span><span class="sxs-lookup"><span data-stu-id="5fb30-162">Postponed features</span></span>

<span data-ttu-id="5fb30-163">Niektóre funkcje pierwotnie planowane dla EF Core 3.0 zostały przełożone na przyszłe wersje:</span><span class="sxs-lookup"><span data-stu-id="5fb30-163">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="5fb30-164">Możliwość ignorowania części modelu w migracjach, śledzonych jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="5fb30-164">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="5fb30-165">Jednostki worka właściwości, śledzone jako dwa oddzielne problemy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) dotyczące jednostek typu udostępnionego i [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) na temat obsługi mapowania właściwości indeksowanych.</span><span class="sxs-lookup"><span data-stu-id="5fb30-165">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
