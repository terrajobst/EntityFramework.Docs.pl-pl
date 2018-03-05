---
title: "Trwa ładowanie powiązanych danych - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="d15cf-102">Trwa ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="d15cf-102">Loading Related Data</span></span>

<span data-ttu-id="d15cf-103">Program Entity Framework Core umożliwia przy użyciu właściwości nawigacji w modelu załadować powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d15cf-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="d15cf-104">Istnieją trzy typowe wzorce O/RM używana do załadowania powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="d15cf-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="d15cf-105">**Ładowanie wczesny** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.</span><span class="sxs-lookup"><span data-stu-id="d15cf-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="d15cf-106">**Ładowanie jawne** oznacza, że powiązane jest jawnie załadować danych z bazy danych w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="d15cf-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="d15cf-107">**Powolne ładowanie** oznacza, że pokrewne niewidocznie załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d15cf-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="d15cf-108">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="d15cf-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="d15cf-109">Ładowanie wczesny</span><span class="sxs-lookup"><span data-stu-id="d15cf-109">Eager loading</span></span>

<span data-ttu-id="d15cf-110">Można użyć `Include` metodę, aby określić powiązane dane mają zostać uwzględnione w wynikach zapytania.</span><span class="sxs-lookup"><span data-stu-id="d15cf-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="d15cf-111">W poniższym przykładzie, blogów, z którego są zwracane w wynikach będzie miał ich `Posts` właściwości wypełniane przy użyciu powiązanych wpisów.</span><span class="sxs-lookup"><span data-stu-id="d15cf-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="d15cf-112">Entity Framework Core będzie automatycznie konfigurowania właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="d15cf-113">Dlatego nawet wtedy, gdy wyraźnie nie zawierają danych dla właściwości nawigacji, właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="d15cf-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="d15cf-114">Powiązane dane z wielu relacji można uwzględnić w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="d15cf-115">W tym wiele poziomów</span><span class="sxs-lookup"><span data-stu-id="d15cf-115">Including multiple levels</span></span>

<span data-ttu-id="d15cf-116">Można przejść do relacji z obejmują wiele poziomów powiązanych danych przy użyciu `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="d15cf-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="d15cf-117">Poniższy przykład ładuje wszystkie blogi, ich powiązane ogłoszeń i autor każdego post.</span><span class="sxs-lookup"><span data-stu-id="d15cf-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="d15cf-118">Bieżącej wersji programu Visual Studio oferują niepoprawny kod zakończenia opcje i może spowodować poprawne wyrażenia do błędy składniowe przy użyciu `ThenInclude` metody po właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="d15cf-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="d15cf-119">To jest symptomem błędów IntelliSense w https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="d15cf-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="d15cf-120">Jest bezpiecznie zignorować te błędy składniowe fałszywe tak długo, jak kod jest poprawny i może zostać pomyślnie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="d15cf-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="d15cf-121">Tworzenia łańcucha wielu wywołań `ThenInclude` aby kontynuować, tym więcej poziomy powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="d15cf-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="d15cf-122">Możesz łączyć wszystko to zostać uwzględnione w jednym zapytaniu powiązane dane z różnych poziomach i wielu elementów głównych.</span><span class="sxs-lookup"><span data-stu-id="d15cf-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="d15cf-123">Można dołączyć wielu powiązanych jednostek dla jednego z jednostkami, które ma być.</span><span class="sxs-lookup"><span data-stu-id="d15cf-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="d15cf-124">Na przykład podczas wykonywania zapytania `Blog`s, obejmują `Posts` , a następnie chcesz dołączyć zarówno `Author` i `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="d15cf-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="d15cf-125">Aby to zrobić, należy określić każdy obejmować ścieżki od katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="d15cf-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="d15cf-126">Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="d15cf-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="d15cf-127">Oznacza to, że otrzymasz nadmiarowe sprzężeń, w większości przypadków będzie skonsolidować EF sprzężeń podczas generowania SQL.</span><span class="sxs-lookup"><span data-stu-id="d15cf-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="d15cf-128">Obejmują na typy pochodne</span><span class="sxs-lookup"><span data-stu-id="d15cf-128">Include on derived types</span></span>

<span data-ttu-id="d15cf-129">Powiązane dane z tego zdefiniowany dla typu pochodnego przy użyciu mogą obejmować `Include` i `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="d15cf-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="d15cf-130">Podane następującego modelu:</span><span class="sxs-lookup"><span data-stu-id="d15cf-130">Given the following model:</span></span>

```Csharp
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

<span data-ttu-id="d15cf-131">Zawartość `School` nawigacji wszystkich osób, które są studentów dzielenia na załadowaniem przy użyciu wielu wzorców:</span><span class="sxs-lookup"><span data-stu-id="d15cf-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="d15cf-132">Używanie cast</span><span class="sxs-lookup"><span data-stu-id="d15cf-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="d15cf-133">przy użyciu `as` — operator</span><span class="sxs-lookup"><span data-stu-id="d15cf-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="d15cf-134">za pomocą przeciążenia `Include` pobierającej parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="d15cf-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="d15cf-135">Ignorowane obejmuje</span><span class="sxs-lookup"><span data-stu-id="d15cf-135">Ignored includes</span></span>

<span data-ttu-id="d15cf-136">Jeśli zmienisz zapytanie tak, aby nie będzie już zwracać wystąpień typów jednostek, które rozpoczął się zapytanie, operatorów include są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="d15cf-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="d15cf-137">W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator służy do zmiany zwracanego typu anonimowego przez zapytanie.</span><span class="sxs-lookup"><span data-stu-id="d15cf-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="d15cf-138">W takim przypadku operatory include nie obowiązują.</span><span class="sxs-lookup"><span data-stu-id="d15cf-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="d15cf-139">Domyślnie EF Core będzie rejestrować ostrzeżenie podczas obejmują operatory są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="d15cf-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="d15cf-140">Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="d15cf-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="d15cf-141">Można zmienić to zachowanie, gdy operator include jest ignorowana, aby zgłosić lub nie rób.</span><span class="sxs-lookup"><span data-stu-id="d15cf-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="d15cf-142">Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d15cf-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="d15cf-143">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="d15cf-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d15cf-144">Ta funkcja została wprowadzona w EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="d15cf-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="d15cf-145">Można w sposób jawny załadować właściwości nawigacji za pośrednictwem `DbContext.Entry(...)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d15cf-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="d15cf-146">Można również jawnie załadować właściwość nawigacji, wykonując oddzielne zapytania, które zwraca powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d15cf-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="d15cf-147">Jeśli jest włączone śledzenie zmian, podczas ładowania jednostki, EF Core zostanie automatycznie Ustaw właściwości nawigacji entitiy nowo załadowanych do odwoływania się do żadnych jednostek już załadowany, a następnie ustaw właściwości nawigacji jednostki już załadowany do odwoływania się do nowo załadowanych jednostki.</span><span class="sxs-lookup"><span data-stu-id="d15cf-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="d15cf-148">Wykonywanie zapytania powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="d15cf-148">Querying related entities</span></span>

<span data-ttu-id="d15cf-149">Można również uzyskać kwerendy LINQ, która reprezentuje zawartość właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d15cf-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="d15cf-150">Pozwala na wykonywanie czynności, takie jak uruchomienie operatora agregacji za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.</span><span class="sxs-lookup"><span data-stu-id="d15cf-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="d15cf-151">Można również filtrować, które powiązanych jednostek są ładowane do pamięci.</span><span class="sxs-lookup"><span data-stu-id="d15cf-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="d15cf-152">powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="d15cf-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d15cf-153">Ta funkcja została wprowadzona w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d15cf-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="d15cf-154">Najprostszym sposobem użycia opóźnionego ładowania jest zainstalowanie [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu i włączenie go w wyniku wywołania `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="d15cf-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="d15cf-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d15cf-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="d15cf-156">Lub w przypadku używania AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="d15cf-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="d15cf-157">Podstawowe EF następnie umożliwi opóźnionego ładowania dla dowolnego właściwość nawigacji, która może zostać zastąpiona — która jest, musi ona być `virtual` i klasy, które mogą być dziedziczone z.</span><span class="sxs-lookup"><span data-stu-id="d15cf-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="d15cf-158">Na przykład w następujących podmiotów `Post.Blog` i `Blog.Posts` właściwości nawigacji zostanie załadowany opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="d15cf-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="d15cf-159">Lazy — ładowania, bez serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="d15cf-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="d15cf-160">Ładowanie opóźnieniem proxy pracy przez wstrzykiwanie `ILazyLoader` usługi do jednostki, zgodnie z opisem w [konstruktorów typów jednostek](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="d15cf-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="d15cf-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d15cf-161">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="d15cf-162">To nie wymaga typy jednostek, które były dziedziczone z i właściwości nawigacji, aby być wirtualna i umożliwia utworzony z wystąpień jednostek `new` do opóźnionego ładowania raz dołączona do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="d15cf-163">Wymaga to jednak odwołanie do `ILazyLoader` usługi, składającą się do zestawu EF podstawowych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d15cf-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="d15cf-164">Aby uniknąć tego Core EF umożliwia `ILazyLoader.Load` metodę, aby zostać dodane jako pełnomocnik.</span><span class="sxs-lookup"><span data-stu-id="d15cf-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="d15cf-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d15cf-165">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="d15cf-166">Kod nad używa `Load` — metoda rozszerzenia, aby upewnić się, za pomocą obiektu delegowanego nieco czyszcząca:</span><span class="sxs-lookup"><span data-stu-id="d15cf-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
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
> <span data-ttu-id="d15cf-167">Parametr konstruktora delegata opóźnionego ładowania musi być wywoływana "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="d15cf-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="d15cf-168">Konfigurację, aby użyć innej nazwy, którego planowana jest w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d15cf-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="d15cf-169">Powiązane dane i serializacja</span><span class="sxs-lookup"><span data-stu-id="d15cf-169">Related data and serialization</span></span>

<span data-ttu-id="d15cf-170">Ponieważ właściwości nawigacji automatycznie konfigurowania będzie EF Core, można na końcu cykli na wykresie obiektu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="d15cf-171">Na przykład ładowanie blogu i powiązanych spowoduje wpisów w obiekcie blog, który odwołuje się do kolekcji wpisów.</span><span class="sxs-lookup"><span data-stu-id="d15cf-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="d15cf-172">Każdy z tych stanowisk ma odwołanie do blogu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="d15cf-173">Niektórych struktur serializacji nie zezwalają na takie cykle.</span><span class="sxs-lookup"><span data-stu-id="d15cf-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="d15cf-174">Na przykład struktury Json.NET zgłosi wyjątek po napotkaniu cyklu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="d15cf-175">Newtonsoft.Json.JsonSerializationException: Self odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".</span><span class="sxs-lookup"><span data-stu-id="d15cf-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="d15cf-176">Jeśli używasz platformy ASP.NET Core, można skonfigurować struktury Json.NET do ignorowania cykle znalezionych w grafie obiektu.</span><span class="sxs-lookup"><span data-stu-id="d15cf-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="d15cf-177">Jest to wykonywane w `ConfigureServices(...)` metoda `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="d15cf-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
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
