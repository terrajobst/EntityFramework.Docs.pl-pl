---
title: Ładowanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417678"
---
# <a name="loading-related-data"></a><span data-ttu-id="e40dc-102">Ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="e40dc-102">Loading Related Data</span></span>

<span data-ttu-id="e40dc-103">Entity Framework Core umożliwia używanie właściwości nawigacji w modelu do ładowania powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="e40dc-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="e40dc-104">Istnieją trzy typowe wzorce O/RM używane do ładowania powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="e40dc-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="e40dc-105">**Ładowanie eager** oznacza, że powiązane dane są ładowane z bazy danych w ramach wstępnego zapytania.</span><span class="sxs-lookup"><span data-stu-id="e40dc-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="e40dc-106">**Jawne ładowanie** oznacza, że powiązane dane są jawnie ładowane z bazy danych w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="e40dc-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="e40dc-107">**Ładowanie z opóźnieniem** oznacza, że powiązane dane są w sposób niewidoczny do załadowania z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e40dc-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="e40dc-108">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e40dc-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="e40dc-109">Ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="e40dc-109">Eager loading</span></span>

<span data-ttu-id="e40dc-110">Za pomocą metody `Include` można określić powiązane dane, które mają być uwzględnione w wynikach zapytania.</span><span class="sxs-lookup"><span data-stu-id="e40dc-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="e40dc-111">W poniższym przykładzie Blogi, które są zwracane w wynikach, będą miały Właściwość `Posts` wypełniane odpowiednimi wpisami.</span><span class="sxs-lookup"><span data-stu-id="e40dc-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="e40dc-112">Entity Framework Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e40dc-113">Dlatego nawet jeśli nie dołączysz jawnie danych dla właściwości nawigacji, właściwość może nadal zostać wypełniona, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="e40dc-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="e40dc-114">Można uwzględnić powiązane dane z wielu relacji w pojedynczym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="e40dc-115">Uwzględnienie wielu poziomów</span><span class="sxs-lookup"><span data-stu-id="e40dc-115">Including multiple levels</span></span>

<span data-ttu-id="e40dc-116">Możesz przejść do szczegółów relacji, aby uwzględnić wiele poziomów powiązanych danych przy użyciu metody `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="e40dc-117">Poniższy przykład ładuje wszystkie blogi, ich powiązane wpisy i autora każdego wpisu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="e40dc-118">Można połączyć wiele wywołań `ThenInclude`, aby kontynuować, w tym dalsze poziomy pokrewnych danych.</span><span class="sxs-lookup"><span data-stu-id="e40dc-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="e40dc-119">Możesz połączyć wszystkie te elementy, aby uwzględnić powiązane dane z wielu poziomów i wiele elementów głównych w tym samym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="e40dc-120">Możesz chcieć dołączyć wiele powiązanych jednostek dla jednej z dołączanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="e40dc-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="e40dc-121">Na przykład podczas wykonywania zapytania dotyczącego `Blogs`należy uwzględnić `Posts`, a następnie dodać zarówno `Author`, jak i `Tags` `Posts`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="e40dc-122">W tym celu należy określić wszystkie ścieżki dołączania, zaczynając od elementu głównego.</span><span class="sxs-lookup"><span data-stu-id="e40dc-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="e40dc-123">Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="e40dc-124">Nie oznacza to, że nastąpi nadmiarowe sprzężenia; w większości przypadków program Dr konsoliduje sprzężenia podczas generowania bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="e40dc-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="e40dc-125">Ponieważ wersja 3.0.0, każda `Include` spowoduje dodanie dodatkowego SPRZĘŻENIa do zapytań SQL generowanych przez dostawców relacyjnych, podczas gdy poprzednie wersje wygenerowały dodatkowe zapytania SQL.</span><span class="sxs-lookup"><span data-stu-id="e40dc-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="e40dc-126">Może to znacząco zmienić wydajność zapytań, aby lepiej lub gorszyć.</span><span class="sxs-lookup"><span data-stu-id="e40dc-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="e40dc-127">W szczególności zapytania LINQ o przekroczeniu dużej liczbie operatorów `Include` mogą wymagać podzielenia na wiele oddzielnych zapytań LINQ w celu uniknięcia problemu z wybuchem kartezjańskiego.</span><span class="sxs-lookup"><span data-stu-id="e40dc-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="e40dc-128">Uwzględnij w typach pochodnych</span><span class="sxs-lookup"><span data-stu-id="e40dc-128">Include on derived types</span></span>

<span data-ttu-id="e40dc-129">Można dołączyć powiązane dane z nawigacji zdefiniowanych tylko dla typu pochodnego przy użyciu `Include` i `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="e40dc-130">Uwzględniając następujący model:</span><span class="sxs-lookup"><span data-stu-id="e40dc-130">Given the following model:</span></span>

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

<span data-ttu-id="e40dc-131">Zawartość `School` nawigacji dla wszystkich osób, które są uczniami, może być eagerly załadowana przy użyciu wielu wzorców:</span><span class="sxs-lookup"><span data-stu-id="e40dc-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="e40dc-132">Używanie rzutowania</span><span class="sxs-lookup"><span data-stu-id="e40dc-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="e40dc-133">Używanie operatora `as`</span><span class="sxs-lookup"><span data-stu-id="e40dc-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="e40dc-134">Używanie przeciążenia `Include`, które pobiera parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="e40dc-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="e40dc-135">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="e40dc-135">Explicit loading</span></span>

<span data-ttu-id="e40dc-136">Można jawnie załadować właściwość nawigacji za pośrednictwem interfejsu API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="e40dc-137">Możesz również jawnie załadować właściwość nawigacji przez wykonanie oddzielnego zapytania, które zwraca powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="e40dc-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="e40dc-138">Jeśli funkcja śledzenia zmian jest włączona, podczas ładowania jednostki EF Core automatycznie ustawił właściwości nawigacji nowo załadowanego jednostkę, aby odwołać się do wszystkich jednostek, które zostały już załadowane, i ustawić właściwości nawigacji już załadowanych jednostek, aby odwołać się do nowo załadowana jednostka.</span><span class="sxs-lookup"><span data-stu-id="e40dc-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="e40dc-139">Wykonywanie zapytania dotyczącego powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="e40dc-139">Querying related entities</span></span>

<span data-ttu-id="e40dc-140">Możesz również uzyskać zapytanie LINQ, które reprezentuje zawartość właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e40dc-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="e40dc-141">Umożliwia to wykonywanie takich czynności, jak uruchamianie agregacji operatora na powiązanych jednostkach bez ładowania ich do pamięci.</span><span class="sxs-lookup"><span data-stu-id="e40dc-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="e40dc-142">Można również filtrować powiązane jednostki, które są ładowane do pamięci.</span><span class="sxs-lookup"><span data-stu-id="e40dc-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="e40dc-143">ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="e40dc-143">Lazy loading</span></span>

<span data-ttu-id="e40dc-144">Najprostszym sposobem użycia ładowania z opóźnieniem jest zainstalowanie pakietu [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) i włączenie go przy użyciu wywołania do `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="e40dc-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e40dc-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="e40dc-146">Lub w przypadku korzystania z AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="e40dc-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="e40dc-147">EF Core następnie Włącz ładowanie z opóźnieniem dla każdej właściwości nawigacji, która może zostać przesłonięta — to oznacza, że musi być `virtual` i na klasie, z której można dziedziczyć.</span><span class="sxs-lookup"><span data-stu-id="e40dc-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="e40dc-148">Na przykład w poniższych jednostkach właściwości nawigacji `Post.Blog` i `Blog.Posts` zostaną załadowane z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="e40dc-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

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

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="e40dc-149">Ładowanie z opóźnieniem bez serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="e40dc-149">Lazy loading without proxies</span></span>

<span data-ttu-id="e40dc-150">Ładowanie serwerów proxy z opóźnieniem, które działają przez wstrzyknięcie usługi `ILazyLoader` do jednostki, zgodnie z opisem w [konstruktorach typu jednostki](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="e40dc-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="e40dc-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e40dc-151">For example:</span></span>

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

<span data-ttu-id="e40dc-152">Nie wymaga to dziedziczenia typów jednostek ani właściwości nawigacji, które mają być wirtualne, i umożliwia wystąpienia jednostek utworzone przy użyciu `new` do ładowania z opóźnieniem po dołączeniu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="e40dc-153">Wymaga to jednak odwołania do usługi `ILazyLoader`, która jest zdefiniowana w pakiecie [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) .</span><span class="sxs-lookup"><span data-stu-id="e40dc-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="e40dc-154">Ten pakiet zawiera minimalny zestaw typów, dzięki czemu w zależności od tego ma być bardzo niewielki wpływ.</span><span class="sxs-lookup"><span data-stu-id="e40dc-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="e40dc-155">Jednak aby całkowicie uniknąć w zależności od dowolnego EF Core pakietów w typach jednostek, można wstrzyknąć metodę `ILazyLoader.Load` jako delegata.</span><span class="sxs-lookup"><span data-stu-id="e40dc-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="e40dc-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e40dc-156">For example:</span></span>

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

<span data-ttu-id="e40dc-157">W powyższym kodzie użyto metody rozszerzenia `Load`, aby użyć delegata:</span><span class="sxs-lookup"><span data-stu-id="e40dc-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

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
> <span data-ttu-id="e40dc-158">Parametr konstruktora dla delegata ładowania z opóźnieniem musi mieć nazwę "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="e40dc-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="e40dc-159">Konfiguracja służąca do używania innej nazwy niż jest planowana dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="e40dc-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="e40dc-160">Powiązane dane i Serializacja</span><span class="sxs-lookup"><span data-stu-id="e40dc-160">Related data and serialization</span></span>

<span data-ttu-id="e40dc-161">Ponieważ EF Core automatycznie naprawia właściwości nawigacji, można zakończyć z cyklami na grafie obiektów.</span><span class="sxs-lookup"><span data-stu-id="e40dc-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="e40dc-162">Na przykład załadowanie blogu i jego powiązanych wpisów spowoduje powstanie obiektu blogu, który odwołuje się do kolekcji wpisów.</span><span class="sxs-lookup"><span data-stu-id="e40dc-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="e40dc-163">Każdy z tych wpisów będzie miał odwołanie z powrotem do blogu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="e40dc-164">Niektóre platformy serializacji nie zezwalają na takie cykle.</span><span class="sxs-lookup"><span data-stu-id="e40dc-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="e40dc-165">Na przykład Json.NET zgłosi następujący wyjątek w przypadku napotkania cyklu.</span><span class="sxs-lookup"><span data-stu-id="e40dc-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="e40dc-166">Newtonsoft. JSON. JsonSerializationException: wykryto pętlę odwołującą dla właściwości "blog" z typem "WebApplication. MODELES. blog".</span><span class="sxs-lookup"><span data-stu-id="e40dc-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="e40dc-167">Jeśli używasz ASP.NET Core, możesz skonfigurować Json.NET do ignorowania cykli znalezionych w grafie obiektów.</span><span class="sxs-lookup"><span data-stu-id="e40dc-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="e40dc-168">Jest to realizowane za pomocą metody `ConfigureServices(...)` w `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="e40dc-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="e40dc-169">Kolejną alternatywą jest dekorować jednej z właściwości nawigacji z atrybutem `[JsonIgnore]`, który instruuje Json.NET, aby nie przechodzą tej właściwości nawigacji podczas serializacji.</span><span class="sxs-lookup"><span data-stu-id="e40dc-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
