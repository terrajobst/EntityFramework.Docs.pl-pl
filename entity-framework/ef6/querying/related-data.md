---
title: Ładowanie powiązanych jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: f40034475ed6659b60ab4317605fd1d802218d69
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565313"
---
# <a name="loading-related-entities"></a><span data-ttu-id="c9c1b-102">Ładowanie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="c9c1b-102">Loading Related Entities</span></span>
<span data-ttu-id="c9c1b-103">Entity Framework obsługuje trzy sposoby ładowania powiązanych danych — eager ładowania, ładowania z opóźnieniem i bezpośredniego ładowania.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="c9c1b-104">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="c9c1b-105">Ładowanie Eagerly</span><span class="sxs-lookup"><span data-stu-id="c9c1b-105">Eagerly Loading</span></span>  

<span data-ttu-id="c9c1b-106">Ładowanie eager jest procesem, w którym zapytanie dla jednego typu jednostki również ładuje powiązane jednostki jako część zapytania.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="c9c1b-107">Ładowanie eager jest realizowane przy użyciu metody include.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="c9c1b-108">Na przykład poniższe zapytania spowodują załadowanie blogów i wszystkich wpisów związanych z każdym blogiem.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```  

<span data-ttu-id="c9c1b-109">Należy zauważyć, że include jest metodą rozszerzającą w przestrzeni nazw System. Data. Entity, dlatego upewnij się, że używasz tej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="c9c1b-110">Eagerly ładowanie wielu poziomów</span><span class="sxs-lookup"><span data-stu-id="c9c1b-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="c9c1b-111">Możliwe jest również eagerly załadowanie wielu poziomów jednostek pokrewnych.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="c9c1b-112">W poniższych zapytaniach pokazano, jak to zrobić dla właściwości nawigacji kolekcji i odwołań.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="c9c1b-113">Należy pamiętać, że nie jest obecnie możliwe filtrowanie powiązanych jednostek, które są ładowane.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="c9c1b-114">Funkcja include będzie zawsze obejmować wszystkie powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="c9c1b-115">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="c9c1b-115">Lazy Loading</span></span>  

<span data-ttu-id="c9c1b-116">Ładowanie z opóźnieniem to proces, w którym jednostka lub kolekcja jednostek są automatycznie ładowane z bazy danych przy pierwszym użyciu właściwości odwołującej się do jednostki/jednostek.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="c9c1b-117">W przypadku korzystania z typów jednostek POCO pobieranie z opóźnieniem jest realizowane przez utworzenie wystąpień pochodnych typów proxy, a następnie Zastępowanie właściwości wirtualnych w celu dodania punktu zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="c9c1b-118">Na przykład podczas korzystania z klasy jednostki blogu zdefiniowanej poniżej powiązane wpisy zostaną załadowane przy pierwszej próbie uzyskania dostępu do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="c9c1b-119">Włącz ładowanie z opóźnieniem dla serializacji</span><span class="sxs-lookup"><span data-stu-id="c9c1b-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="c9c1b-120">Pobieranie z opóźnieniem i Serializacja nie są dobrze mieszane i jeśli nie jest to konieczne, możesz zakończyć wykonywanie zapytań dotyczących całej bazy danych, tylko ponieważ jest włączone ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="c9c1b-121">Większość serializatorów pracuje przez uzyskanie dostępu do każdej właściwości w wystąpieniu typu.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="c9c1b-122">Dostęp do właściwości wyzwala ładowanie z opóźnieniem, więc więcej jednostek uzyskuje serializacji.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="c9c1b-123">Dostęp do tych właściwości jednostek jest możliwy, a jeszcze więcej jednostek jest załadowanych.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="c9c1b-124">Dobrym sposobem jest włączenie ładowania z opóźnieniem przed Serializacja jednostki.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="c9c1b-125">Poniższe sekcje pokazują, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="c9c1b-126">Wyłączanie ładowania z opóźnieniem dla określonych właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="c9c1b-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="c9c1b-127">Pobieranie z opóźnieniem kolekcji ogłoszeń można wyłączyć, wprowadzając Właściwość Posts, która nie jest wirtualna:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="c9c1b-128">Ładowanie kolekcji ogłoszeń można nadal osiągnąć przy użyciu ładowania eager (zobacz *Eagerly ładowanie* powyżej) lub metodę load (zobacz *jawne ładowanie* poniżej).</span><span class="sxs-lookup"><span data-stu-id="c9c1b-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="c9c1b-129">Wyłącz ładowanie z opóźnieniem dla wszystkich jednostek</span><span class="sxs-lookup"><span data-stu-id="c9c1b-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="c9c1b-130">Ładowanie z opóźnieniem można wyłączyć dla wszystkich jednostek w kontekście przez ustawienie flagi właściwości konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="c9c1b-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="c9c1b-132">Ładowanie pokrewnych jednostek można nadal osiągnąć przy użyciu ładowania eager (zobacz *Eagerly ładowanie* powyżej) lub metodę load (zobacz *jawne ładowanie* poniżej).</span><span class="sxs-lookup"><span data-stu-id="c9c1b-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="c9c1b-133">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="c9c1b-133">Explicitly Loading</span></span>  

<span data-ttu-id="c9c1b-134">Nawet po wyłączeniu ładowania z opóźnieniem nadal jest możliwe opóźnieniem pokrewnych jednostek, ale należy je wykonać z jawnym wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="c9c1b-135">W tym celu należy użyć metody Load dla wpisu jednostki powiązanej.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="c9c1b-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="c9c1b-137">Należy zauważyć, że metoda referencyjna powinna być używana, gdy jednostka ma właściwość nawigacji do innej pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="c9c1b-138">Z drugiej strony Metoda kolekcji powinna być używana, gdy jednostka ma właściwość nawigacji do kolekcji innych jednostek.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="c9c1b-139">Stosowanie filtrów podczas jawnego ładowania powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="c9c1b-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="c9c1b-140">Metoda zapytania zapewnia dostęp do bazowego zapytania, którego Entity Framework będzie używać podczas ładowania powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="c9c1b-141">Następnie można użyć LINQ do zastosowania filtrów do zapytania przed wykonaniem go za pomocą wywołania metody rozszerzenia LINQ, takiej jak ToList —, Load itp. Metoda zapytania może być używana z właściwościami odwołania i nawigacji kolekcji, ale jest najbardziej przydatna w przypadku kolekcji, których można użyć do załadowania tylko części kolekcji.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="c9c1b-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

<span data-ttu-id="c9c1b-143">Korzystając z metody zapytania, zazwyczaj najlepiej jest wyłączyć ładowanie z opóźnieniem dla właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="c9c1b-144">Dzieje się tak, ponieważ w przeciwnym razie cała kolekcja może zostać załadowana automatycznie przez mechanizm ładowania z opóźnieniem przed lub po wykonaniu odfiltrowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="c9c1b-145">Należy pamiętać, że chociaż relacja może być określona jako ciąg zamiast wyrażenia lambda, zwracany interfejs IQueryable nie jest ogólny, gdy jest używany ciąg i dlatego Metoda Cast jest zwykle wymagana, zanim wszystko będzie przydatne.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="c9c1b-146">Używanie zapytania do zliczania powiązanych jednostek bez ich ładowania</span><span class="sxs-lookup"><span data-stu-id="c9c1b-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="c9c1b-147">Czasami warto wiedzieć, ile jednostek jest związanych z inną jednostką w bazie danych, bez faktycznego ponoszenia kosztów ładowania wszystkich jednostek.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="c9c1b-148">W tym celu można użyć metody zapytania z metodą Count LINQ.</span><span class="sxs-lookup"><span data-stu-id="c9c1b-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="c9c1b-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c9c1b-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```  
