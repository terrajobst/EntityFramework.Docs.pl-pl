---
title: Ładowanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4bf9598f9b7e74c2835d3926215de9a7ef4e6f96
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921791"
---
# <a name="loading-related-data"></a><span data-ttu-id="95bf3-102">Ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="95bf3-102">Loading Related Data</span></span>

<span data-ttu-id="95bf3-103">Entity Framework Core umożliwia używanie właściwości nawigacji w modelu do ładowania powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="95bf3-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="95bf3-104">Istnieją trzy typowe wzorce O/RM używane do ładowania powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="95bf3-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="95bf3-105">**Ładowanie eager** oznacza, że powiązane dane są ładowane z bazy danych w ramach wstępnego zapytania.</span><span class="sxs-lookup"><span data-stu-id="95bf3-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="95bf3-106">**Jawne ładowanie** oznacza, że powiązane dane są jawnie ładowane z bazy danych w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="95bf3-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="95bf3-107">**Ładowanie z opóźnieniem** oznacza, że powiązane dane są w sposób niewidoczny do załadowania z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="95bf3-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="95bf3-108">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="95bf3-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="95bf3-109">Ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="95bf3-109">Eager loading</span></span>

<span data-ttu-id="95bf3-110">Możesz użyć metody, `Include` aby określić powiązane dane, które mają być uwzględnione w wynikach zapytania.</span><span class="sxs-lookup"><span data-stu-id="95bf3-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="95bf3-111">W poniższym przykładzie w blogach, które są zwracane w wynikach, zostanie wypełniona `Posts` ich właściwość wraz z powiązanymi wpisami.</span><span class="sxs-lookup"><span data-stu-id="95bf3-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="95bf3-112">Entity Framework Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="95bf3-113">Dlatego nawet jeśli nie dołączysz jawnie danych dla właściwości nawigacji, właściwość może nadal zostać wypełniona, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="95bf3-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="95bf3-114">Można uwzględnić powiązane dane z wielu relacji w pojedynczym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="95bf3-115">Uwzględnienie wielu poziomów</span><span class="sxs-lookup"><span data-stu-id="95bf3-115">Including multiple levels</span></span>

<span data-ttu-id="95bf3-116">Możesz przejść do szczegółów relacji, aby dołączyć wiele poziomów powiązanych danych przy użyciu `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="95bf3-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="95bf3-117">Poniższy przykład ładuje wszystkie blogi, ich powiązane wpisy i autora każdego wpisu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="95bf3-118">Bieżące wersje programu Visual Studio oferują nieprawidłowe opcje uzupełniania kodu i mogą spowodować, że poprawne wyrażenia mają być oflagowane z błędami `ThenInclude` składniowymi przy użyciu metody po właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="95bf3-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="95bf3-119">Jest to objaw błędu IntelliSense śledzonego w https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="95bf3-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="95bf3-120">Można bezpiecznie zignorować te błędy składni fałszywe o ile kod jest poprawny i może zostać skompilowany pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="95bf3-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="95bf3-121">Można `ThenInclude` połączyć wiele wywołań, aby kontynuować, włączając dalsze poziomy pokrewnych danych.</span><span class="sxs-lookup"><span data-stu-id="95bf3-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="95bf3-122">Możesz połączyć wszystkie te elementy, aby uwzględnić powiązane dane z wielu poziomów i wiele elementów głównych w tym samym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="95bf3-123">Możesz chcieć dołączyć wiele powiązanych jednostek dla jednej z dołączanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="95bf3-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="95bf3-124">Na przykład, podczas wykonywania zapytania `Blogs`, należy dołączyć `Posts` `Posts`zarówno `Author` , jak i `Tags` .</span><span class="sxs-lookup"><span data-stu-id="95bf3-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="95bf3-125">W tym celu należy określić wszystkie ścieżki dołączania, zaczynając od elementu głównego.</span><span class="sxs-lookup"><span data-stu-id="95bf3-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="95bf3-126">Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="95bf3-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="95bf3-127">Nie oznacza to, że nastąpi nadmiarowe sprzężenia; w większości przypadków program Dr konsoliduje sprzężenia podczas generowania bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="95bf3-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="95bf3-128">Uwzględnij w typach pochodnych</span><span class="sxs-lookup"><span data-stu-id="95bf3-128">Include on derived types</span></span>

<span data-ttu-id="95bf3-129">Można dołączyć powiązane dane z nawigacji zdefiniowanych tylko dla typu pochodnego przy użyciu `Include` i. `ThenInclude`</span><span class="sxs-lookup"><span data-stu-id="95bf3-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="95bf3-130">Uwzględniając następujący model:</span><span class="sxs-lookup"><span data-stu-id="95bf3-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="95bf3-131">`School` Zawartość nawigacji dla wszystkich osób, które są uczniami, może być eagerly załadowana przy użyciu wielu wzorców:</span><span class="sxs-lookup"><span data-stu-id="95bf3-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="95bf3-132">Używanie rzutowania</span><span class="sxs-lookup"><span data-stu-id="95bf3-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="95bf3-133">operator `as` using</span><span class="sxs-lookup"><span data-stu-id="95bf3-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="95bf3-134">Użycie przeciążenia `Include` , które pobiera parametr typu`string`</span><span class="sxs-lookup"><span data-stu-id="95bf3-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="95bf3-135">Zignorowane obejmuje</span><span class="sxs-lookup"><span data-stu-id="95bf3-135">Ignored includes</span></span>

<span data-ttu-id="95bf3-136">W przypadku zmiany zapytania, tak aby nie zwracało już wystąpień typu jednostki, z którym rozpoczyna się zapytanie, a następnie operatory include są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="95bf3-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="95bf3-137">W poniższym przykładzie operatory include są oparte na `Blog`, ale `Select` następnie operator służy do zmiany zapytania w celu zwrócenia typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="95bf3-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="95bf3-138">W takim przypadku operatory include nie mają żadnego wpływu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="95bf3-139">Domyślnie EF Core będzie rejestrować ostrzeżenie, gdy operatory include są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="95bf3-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="95bf3-140">Zobacz [Rejestrowanie](../miscellaneous/logging.md) , aby uzyskać więcej informacji na temat wyświetlania danych wyjściowych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="95bf3-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="95bf3-141">Można zmienić zachowanie, gdy operator include jest ignorowany do throw lub nic nie rób.</span><span class="sxs-lookup"><span data-stu-id="95bf3-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="95bf3-142">Jest to wykonywane podczas konfigurowania opcji dla kontekstu — zwykle w programie `DbContext.OnConfiguring`, lub w `Startup.cs` przypadku korzystania z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95bf3-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="95bf3-143">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="95bf3-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="95bf3-144">Ta funkcja została wprowadzona w EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="95bf3-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="95bf3-145">Można jawnie załadować właściwość nawigacji za pośrednictwem `DbContext.Entry(...)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="95bf3-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="95bf3-146">Możesz również jawnie załadować właściwość nawigacji przez wykonanie oddzielnego zapytania, które zwraca powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="95bf3-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="95bf3-147">Jeśli funkcja śledzenia zmian jest włączona, podczas ładowania jednostki EF Core automatycznie ustawił właściwości nawigacji nowo załadowanego jednostkę, aby odwołać się do wszystkich jednostek, które zostały już załadowane, i ustawić właściwości nawigacji już załadowanych jednostek, aby odwołać się do nowo załadowana jednostka.</span><span class="sxs-lookup"><span data-stu-id="95bf3-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="95bf3-148">Wykonywanie zapytania dotyczącego powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="95bf3-148">Querying related entities</span></span>

<span data-ttu-id="95bf3-149">Możesz również uzyskać zapytanie LINQ, które reprezentuje zawartość właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="95bf3-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="95bf3-150">Umożliwia to wykonywanie takich czynności, jak uruchamianie agregacji operatora na powiązanych jednostkach bez ładowania ich do pamięci.</span><span class="sxs-lookup"><span data-stu-id="95bf3-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="95bf3-151">Można również filtrować powiązane jednostki, które są ładowane do pamięci.</span><span class="sxs-lookup"><span data-stu-id="95bf3-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="95bf3-152">ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="95bf3-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="95bf3-153">Ta funkcja została wprowadzona w EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="95bf3-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="95bf3-154">Najprostszym sposobem użycia ładowania z opóźnieniem jest zainstalowanie pakietu [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) i włączenie go z wywołaniem do `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="95bf3-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="95bf3-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95bf3-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="95bf3-156">Lub w przypadku korzystania z AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="95bf3-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="95bf3-157">EF Core następnie włącza ładowanie z opóźnieniem dla każdej właściwości nawigacji, która może zostać przesłonięta — to oznacza, `virtual` że musi być i na klasie, z której można dziedziczyć.</span><span class="sxs-lookup"><span data-stu-id="95bf3-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="95bf3-158">Na przykład w następujących jednostkach `Post.Blog` właściwości i `Blog.Posts` nawigacji zostaną załadowane z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="95bf3-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="95bf3-159">Ładowanie z opóźnieniem bez serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="95bf3-159">Lazy loading without proxies</span></span>

<span data-ttu-id="95bf3-160">Ładowanie serwerów proxy z opóźnieniem, które działają przez `ILazyLoader` wstrzyknięcie usługi do jednostki, zgodnie z opisem w [konstruktorach typu jednostki](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="95bf3-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="95bf3-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95bf3-161">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="95bf3-162">Nie wymaga to dziedziczenia typów jednostek ani właściwości nawigacji, które mają być wirtualne, i umożliwia wystąpienia jednostki utworzone za `new` pomocą do ładowania opóźnionego po dołączeniu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="95bf3-163">Jednak wymaga odwołania do `ILazyLoader` usługi, która jest zdefiniowana w pakiecie [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) .</span><span class="sxs-lookup"><span data-stu-id="95bf3-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="95bf3-164">Ten pakiet zawiera minimalny zestaw typów, dzięki czemu w zależności od tego ma być bardzo niewielki wpływ.</span><span class="sxs-lookup"><span data-stu-id="95bf3-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="95bf3-165">Jednak aby całkowicie uniknąć w zależności od dowolnego EF Core pakietów w typach jednostek, można wstrzyknąć `ILazyLoader.Load` metodę jako delegata.</span><span class="sxs-lookup"><span data-stu-id="95bf3-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="95bf3-166">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95bf3-166">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="95bf3-167">W powyższym kodzie `Load` użyto metody rozszerzenia, aby użyć delegata:</span><span class="sxs-lookup"><span data-stu-id="95bf3-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="95bf3-168">Parametr konstruktora dla delegata ładowania z opóźnieniem musi mieć nazwę "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="95bf3-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="95bf3-169">Konfiguracja służąca do używania innej nazwy niż jest planowana dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="95bf3-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="95bf3-170">Powiązane dane i Serializacja</span><span class="sxs-lookup"><span data-stu-id="95bf3-170">Related data and serialization</span></span>

<span data-ttu-id="95bf3-171">Ponieważ EF Core automatycznie naprawia właściwości nawigacji, można zakończyć z cyklami na grafie obiektów.</span><span class="sxs-lookup"><span data-stu-id="95bf3-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="95bf3-172">Na przykład załadowanie blogu i jego powiązanych wpisów spowoduje powstanie obiektu blogu, który odwołuje się do kolekcji wpisów.</span><span class="sxs-lookup"><span data-stu-id="95bf3-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="95bf3-173">Każdy z tych wpisów będzie miał odwołanie z powrotem do blogu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="95bf3-174">Niektóre platformy serializacji nie zezwalają na takie cykle.</span><span class="sxs-lookup"><span data-stu-id="95bf3-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="95bf3-175">Na przykład Json.NET zgłosi następujący wyjątek w przypadku napotkania cyklu.</span><span class="sxs-lookup"><span data-stu-id="95bf3-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="95bf3-176">Newtonsoft.Json.JsonSerializationException: Wykryto pętlę z odwołaniami do siebie dla właściwości "blog" z typem "WebApplication. models. bloga".</span><span class="sxs-lookup"><span data-stu-id="95bf3-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="95bf3-177">Jeśli używasz ASP.NET Core, możesz skonfigurować Json.NET do ignorowania cykli znalezionych w grafie obiektów.</span><span class="sxs-lookup"><span data-stu-id="95bf3-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="95bf3-178">Jest to realizowane za pomocą `ConfigureServices(...)` metody w `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="95bf3-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="95bf3-179">Kolejną alternatywą jest dekorować jednej z właściwości nawigacji z `[JsonIgnore]` atrybutem, który instruuje JSON.NET, aby nie przechodzą tej właściwości nawigacji podczas serializacji.</span><span class="sxs-lookup"><span data-stu-id="95bf3-179">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
