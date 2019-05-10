---
title: Trwa ładowanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 590d16902329ffb3fff8026f8dfdcfc887f6dea3
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873193"
---
# <a name="loading-related-data"></a><span data-ttu-id="c3ef3-102">Ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="c3ef3-102">Loading Related Data</span></span>

<span data-ttu-id="c3ef3-103">Entity Framework Core umożliwia ładowanie powiązanych jednostek przy użyciu właściwości nawigacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="c3ef3-104">Istnieją trzy powszechnie używane wzorce Obiektowo używana do ładowania powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="c3ef3-105">**Wczesne ładowanie** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="c3ef3-106">**Jawne ładowanie** oznacza, że powiązanych danych jest jawnie załadowane z bazy danych w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="c3ef3-107">**Powolne ładowanie** oznacza, że powiązane przezroczyste załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="c3ef3-108">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="c3ef3-109">Wczesne ładowanie</span><span class="sxs-lookup"><span data-stu-id="c3ef3-109">Eager loading</span></span>

<span data-ttu-id="c3ef3-110">Możesz użyć `Include` metodę, aby określić powiązanych danych, które mają zostać uwzględnione w wynikach kwerendy.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="c3ef3-111">W poniższym przykładzie, blogów, które są zwracane w wynikach będzie mieć ich `Posts` właściwość wypełniony pokrewnych wpisów.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="c3ef3-112">Entity Framework Core będą automatycznie konfigurowania właściwości nawigacji do innych jednostek, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="c3ef3-113">Dlatego nawet wtedy, gdy jawnie nie zawiera danych dla właściwości nawigacji, nadal można wypełnić właściwość, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="c3ef3-114">Może zawierać dane związane z wieloma relacjami w ramach pojedynczego zapytania.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="c3ef3-115">W tym wiele poziomów</span><span class="sxs-lookup"><span data-stu-id="c3ef3-115">Including multiple levels</span></span>

<span data-ttu-id="c3ef3-116">Możesz przejść do szczegółów w relacji do uwzględnienia na wielu poziomach powiązanych danych przy użyciu `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="c3ef3-117">Poniższy przykład ładuje wszystkie blogi, ich pokrewnych wpisów i autor każdego wpisu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="c3ef3-118">Bieżące wersje programu Visual Studio oferuje opcje uzupełniania niepoprawny kod i może spowodować, że poprawne wyrażenia do oflagowania z błędami składni, korzystając z `ThenInclude` metody właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="c3ef3-119">Jest to objaw błędów funkcji IntelliSense w https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="c3ef3-120">Jest bezpiecznie zignorować te błędy składniowe fałszywe, tak długo, jak kod jest poprawny i mogą być pomyślnie skompilowane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="c3ef3-121">Można połączyć w łańcuch wielu wywołań `ThenInclude` aby kontynuować, włączając dalsze poziomy powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="c3ef3-122">Możesz połączyć wszystkie te które mają zostać objęte powiązane dane z wielu poziomów i wielu katalogów głównych tego samego zapytania.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="c3ef3-123">Możesz uwzględnić wiele powiązanych jednostek dla jednej jednostki, które jest uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="c3ef3-124">Na przykład podczas wykonywania zapytań dotyczących `Blogs`, możesz uwzględnić `Posts` , a następnie oba `Author` i `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="c3ef3-125">Aby to zrobić, należy określić każdy obejmować ścieżkę, począwszy od głównego.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="c3ef3-126">Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="c3ef3-127">Nie oznacza to, że otrzymasz sprzężeń nadmiarowy; w większości przypadków EF będzie konsolidować sprzężeń, jeśli generowanie kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="c3ef3-128">Obejmują na typy pochodne</span><span class="sxs-lookup"><span data-stu-id="c3ef3-128">Include on derived types</span></span>

<span data-ttu-id="c3ef3-129">Może zawierać dane związane z tego zdefiniowany tylko w typie pochodnym przy użyciu `Include` i `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="c3ef3-130">Biorąc pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-130">Given the following model:</span></span>

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

<span data-ttu-id="c3ef3-131">Zawartość `School` nawigacji wszystkie osoby, których uczniowie mogą być eagerly ładowane, przy użyciu wielu wzorców:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="c3ef3-132">Używanie cast</span><span class="sxs-lookup"><span data-stu-id="c3ef3-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="c3ef3-133">za pomocą `as` — operator</span><span class="sxs-lookup"><span data-stu-id="c3ef3-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="c3ef3-134">za pomocą przeciążenia `Include` przyjmującą parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="c3ef3-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="c3ef3-135">Ignorowane obejmuje</span><span class="sxs-lookup"><span data-stu-id="c3ef3-135">Ignored includes</span></span>

<span data-ttu-id="c3ef3-136">Jeśli zmienisz zapytanie tak, aby nie zwraca wystąpienia typu jednostki, który rozpoczął się zapytania, operatory dołączania są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="c3ef3-137">W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator jest używany do zmiany zwracanego typu anonimowego przez zapytanie.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="c3ef3-138">W tym przypadku operatory include nie mają wpływu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="c3ef3-139">Domyślnie EF Core zarejestruje ostrzeżenie obejmują gdy operatory są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="c3ef3-140">Zobacz [rejestrowania](../miscellaneous/logging.md) więcej informacji na temat wyświetlania danych wyjściowych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="c3ef3-141">Można zmienić zachowanie, gdy include operator jest ignorowana, throw lub nic nie rób.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="c3ef3-142">Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` korzystania z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="c3ef3-143">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="c3ef3-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="c3ef3-144">Ta funkcja została wprowadzona w programie EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="c3ef3-145">Można jawnie załadować właściwości nawigacji za pomocą `DbContext.Entry(...)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="c3ef3-146">Właściwość nawigacji można także jawnie załadować, wykonując oddzielne zapytania, które zwraca powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="c3ef3-147">Jeśli jest włączone śledzenie zmian, a następnie podczas ładowania jednostki, EF Core będą automatycznie ustawić właściwości nawigacji między jednostkami nowo załadowane do odwoływania się do żadnych jednostek już załadowany i ustawić właściwości nawigacji jednostki już załadowane do odwoływania się do Jednostka nowo załadowane.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="c3ef3-148">Tworzenie zapytań powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="c3ef3-148">Querying related entities</span></span>

<span data-ttu-id="c3ef3-149">Można również uzyskać kwerenda LINQ, która reprezentuje zawartość właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="c3ef3-150">Dzięki temu można korzystać z możliwości, takie jak uruchomienie aggregate-operator za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="c3ef3-151">Można również filtrować, które powiązanych jednostek są ładowane do pamięci.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="c3ef3-152">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="c3ef3-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="c3ef3-153">Ta funkcja została wprowadzona w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="c3ef3-154">Najprostszym sposobem użycia ładowanych z opóźnieniem jest po zainstalowaniu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu i włączanie go z wywołaniem `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="c3ef3-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="c3ef3-156">Lub w przypadku używania AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="c3ef3-157">EF Core będzie z opóźnieniem, ładowania dla dowolnej właściwości nawigacji, która może zostać przesłonięta — oznacza to, Włącz musi być `virtual` i klasy, które mogą być dziedziczone z.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="c3ef3-158">Na przykład w następujących elementach `Post.Blog` i `Blog.Posts` właściwości nawigacji będą ładowane z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="c3ef3-159">Powolne ładowanie bez serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="c3ef3-159">Lazy loading without proxies</span></span>

<span data-ttu-id="c3ef3-160">Serwery proxy ładowanych z opóźnieniem pracy przez iniekcję `ILazyLoader` usługi do jednostki, zgodnie z opisem w [Konstruktorzy typów jednostek](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="c3ef3-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="c3ef3-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-161">For example:</span></span>
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
<span data-ttu-id="c3ef3-162">To nie wymaga typów jednostek, dziedziczenie lub właściwości nawigacji, aby być wirtualne oraz umożliwia wystąpień jednostek utworzonych za pomocą `new` z opóźnieniem obciążenia raz przyłączonych do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="c3ef3-163">Jednak wymaga to dodania odwołania do `ILazyLoader` usługa, która jest zdefiniowana w [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="c3ef3-164">Ten pakiet zawiera minimalny zestaw typów, dzięki czemu istnieje niewielki wpływ w zależności od jego.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="c3ef3-165">Jednak aby całkowicie uniknąć zależności od tego, wszystkie pakiety programu EF Core w typów jednostek, istnieje możliwość wstawić `ILazyLoader.Load` metody jako pełnomocnik.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="c3ef3-166">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-166">For example:</span></span>
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
<span data-ttu-id="c3ef3-167">Kod powyżej używa `Load` metodę rozszerzenia, aby wprowadzić nieco oczyszczarkę plików przy użyciu delegata:</span><span class="sxs-lookup"><span data-stu-id="c3ef3-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="c3ef3-168">Parametr konstruktora delegata ładowanych z opóźnieniem należy wywołać "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="c3ef3-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="c3ef3-169">Konfigurację, aby użyć innej nazwy, niż jest to planowana w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="c3ef3-170">Powiązane dane i serializacja</span><span class="sxs-lookup"><span data-stu-id="c3ef3-170">Related data and serialization</span></span>

<span data-ttu-id="c3ef3-171">Ponieważ właściwości nawigacji automatycznie poprawki będą programu EF Core, można na końcu cykli w grafie obiektu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="c3ef3-172">Na przykład blog i jej powiązane wpisy ładowania spowoduje obiekt blogu, który odwołuje się zbiór wpisów.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="c3ef3-173">Każda z tych wpisów mają odwołaniem zwrotnym do blogu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="c3ef3-174">Niektóre środowiska serializacji nie zezwalają na tych cykli.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="c3ef3-175">Na przykład na składnik Json.NET zgłosi następujący wyjątek, jeśli okaże się cyklu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="c3ef3-176">Newtonsoft.Json.JsonSerializationException: Samodzielna odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".</span><span class="sxs-lookup"><span data-stu-id="c3ef3-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="c3ef3-177">Jeśli używasz platformy ASP.NET Core, można skonfigurować program Json.NET, aby zignorować cykle, które znajdzie się na grafie obiektu.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="c3ef3-178">Jest to realizowane w `ConfigureServices(...)` method in Class metoda `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="c3ef3-179">Innym sposobem jest do jednej z właściwości nawigacji o dekoracji `[JsonIgnore]` atrybut, który powoduje, że program Json.NET, aby nie przechodzą przez tę właściwość nawigacji podczas serializowania.</span><span class="sxs-lookup"><span data-stu-id="c3ef3-179">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
