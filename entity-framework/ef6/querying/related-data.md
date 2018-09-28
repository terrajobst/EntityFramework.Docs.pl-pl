---
title: Trwa ładowanie powiązanych jednostek - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 2d33d9db8acc61f7d556e3eca46b1ea90198723e
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415760"
---
# <a name="loading-related-entities"></a><span data-ttu-id="67e72-102">Trwa ładowanie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="67e72-102">Loading Related Entities</span></span>
<span data-ttu-id="67e72-103">Entity Framework obsługuje ładowanie powiązanych danych - eager, ładowania i powolne ładowanie jawne ładowanie na trzy sposoby.</span><span class="sxs-lookup"><span data-stu-id="67e72-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="67e72-104">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="67e72-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="67e72-105">Trwa ładowanie eagerly</span><span class="sxs-lookup"><span data-stu-id="67e72-105">Eagerly Loading</span></span>  

<span data-ttu-id="67e72-106">Wczesne ładowanie polega, według których kwerendy dla jednego typu obiektu również ładuje powiązanych jednostek jako część zapytania.</span><span class="sxs-lookup"><span data-stu-id="67e72-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="67e72-107">Wczesne ładowanie odbywa się przy użyciu metody Include.</span><span class="sxs-lookup"><span data-stu-id="67e72-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="67e72-108">Na przykład poniższych zapytań załaduje naszych blogach i wszystkie wpisy związane z każdego bloga.</span><span class="sxs-lookup"><span data-stu-id="67e72-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blogs and its related posts
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

<span data-ttu-id="67e72-109">Zwróć uwagę, że Include metody rozszerzenia w przestrzeni nazw System.Data.Entity dlatego upewnij się, że używasz tej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="67e72-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="67e72-110">Trwa ładowanie eagerly wiele poziomów</span><span class="sxs-lookup"><span data-stu-id="67e72-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="67e72-111">Istnieje również możliwość eagerly obciążenia na wielu poziomach powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="67e72-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="67e72-112">Poniższych zapytań Pokaż przykładów jak to zrobić dla kolekcji i odwołanie do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="67e72-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

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

<span data-ttu-id="67e72-113">Należy pamiętać, że nie jest obecnie można filtrować dane są ładowane które powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="67e72-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="67e72-114">Obejmują będą zawsze Przenieś wszystkie powiązane jednostki.</span><span class="sxs-lookup"><span data-stu-id="67e72-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="67e72-115">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="67e72-115">Lazy Loading</span></span>  

<span data-ttu-id="67e72-116">Powolne ładowanie polega na zgodnie z którą jednostki lub kolekcję jednostek jest automatycznie ładowany z bazy danych, uzyskiwania dostępu do właściwości, odnoszące się do jednostki/jednostki po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="67e72-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="67e72-117">Korzystając z typów jednostki POCO, powolne ładowanie odbywa się tworzenia wystąpień typów pochodnych serwera proxy, a następnie zastępowanie właściwości wirtualnego można dodać punktów zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="67e72-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="67e72-118">Na przykład korzystając z klasy jednostki blogu, które są zdefiniowane poniżej, pokrewnych wpisów zostanie załadowany po raz pierwszy uzyskano dostęp do właściwości nawigacji wpisy:</span><span class="sxs-lookup"><span data-stu-id="67e72-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="67e72-119">Włącz powolne ładowanie wyłączone do serializacji</span><span class="sxs-lookup"><span data-stu-id="67e72-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="67e72-120">Powolne ładowanie i serializacji nie Mieszaj dobrze, a jeśli nie chcesz zachować ostrożność można zakończyć się wykonanie zapytania dotyczącego całą bazę danych, po prostu, ponieważ ładowania z opóźnieniem jest włączone.</span><span class="sxs-lookup"><span data-stu-id="67e72-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="67e72-121">Serializatory większość pracy, uzyskując dostęp do każdej właściwości w wystąpieniu typu.</span><span class="sxs-lookup"><span data-stu-id="67e72-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="67e72-122">Dostęp do właściwości wyzwala ładowania z opóźnieniem, więc podlegają serializacji z kolejnych jednostek.</span><span class="sxs-lookup"><span data-stu-id="67e72-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="67e72-123">Na tych jednostkach właściwości są dostępne, a nawet więcej jednostek są ładowane.</span><span class="sxs-lookup"><span data-stu-id="67e72-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="67e72-124">Jest dobrą praktyką, aby włączyć powolne ładowanie wyłączone przed serializacji jednostki.</span><span class="sxs-lookup"><span data-stu-id="67e72-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="67e72-125">Poniższe sekcje pokazują, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="67e72-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="67e72-126">Wyłączenie z opóźnieniem, ładowania dla właściwości określonej nawigacji</span><span class="sxs-lookup"><span data-stu-id="67e72-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="67e72-127">Powolne ładowanie kolekcji wpisy można wyłączyć, wprowadzając niewirtualną właściwość wpisy:</span><span class="sxs-lookup"><span data-stu-id="67e72-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

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

<span data-ttu-id="67e72-128">Ładowanie wpisów w kolekcji można nadal można osiągnąć przy użyciu wczesne ładowanie (zobacz *Eagerly ładowania* powyżej) lub metody obciążenia (zobacz *jawnego ładowania* poniżej).</span><span class="sxs-lookup"><span data-stu-id="67e72-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="67e72-129">Wyłącz powolne ładowanie dla wszystkich obiektów</span><span class="sxs-lookup"><span data-stu-id="67e72-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="67e72-130">Powolne ładowanie można wyłączyć dla wszystkich jednostek w kontekście przez ustawienie flagi na właściwość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="67e72-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="67e72-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="67e72-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="67e72-132">Ładowanie powiązanych jednostek nadal można osiągnąć, stosując wczesne ładowanie (zobacz *Eagerly ładowania* powyżej) lub metody obciążenia (zobacz *jawnego ładowania* poniżej).</span><span class="sxs-lookup"><span data-stu-id="67e72-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="67e72-133">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="67e72-133">Explicitly Loading</span></span>  

<span data-ttu-id="67e72-134">Nawet w przypadku powolne ładowanie wyłączone jest nadal możliwe opóźnieniem ładowanie powiązanych jednostek, ale muszą być wykonane za pomocą jawnego wywołania.</span><span class="sxs-lookup"><span data-stu-id="67e72-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="67e72-135">Aby to zrobić, użyj metodę ładowania przy uruchamianiu powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="67e72-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="67e72-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="67e72-136">For example:</span></span>  

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

<span data-ttu-id="67e72-137">Należy pamiętać, że Reference — metoda powinien być używany, jeśli określona jednostka ma właściwość nawigacji, do innego pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="67e72-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="67e72-138">Z drugiej strony metoda kolekcji należy używać, jeśli określona jednostka ma właściwości nawigacji kolekcji innych podmiotów.</span><span class="sxs-lookup"><span data-stu-id="67e72-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="67e72-139">Stosowanie filtrów, gdy jawnie ładowanie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="67e72-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="67e72-140">Metoda zapytania zapewnia dostęp do podstawowego zapytania, które będą używać programu Entity Framework, gdy ładowanie powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="67e72-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="67e72-141">Następnie służy LINQ do zastosowania filtrów do zapytania przed wykonaniem jego wywołaniem metody rozszerzenia LINQ, takich jak tolist —, obciążenia itp. Metoda zapytania mogą być używane z właściwości nawigacji kolekcji i odwołania, ale jest najbardziej użyteczne dla kolekcji, której może służyć do załadowania tylko część kolekcji.</span><span class="sxs-lookup"><span data-stu-id="67e72-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="67e72-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="67e72-142">For example:</span></span>  

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

<span data-ttu-id="67e72-143">Za pomocą metody zapytania jest zazwyczaj najlepiej jest wyłączyć powolne ładowanie dla właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="67e72-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="67e72-144">Jest tak, ponieważ w przeciwnym razie całej kolekcji może być ładowane automatycznie przez mechanizm powolne ładowanie przed lub po wykonaniu zapytania filtrowanego.</span><span class="sxs-lookup"><span data-stu-id="67e72-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="67e72-145">Należy pamiętać, że podczas relacji może być określony jako ciągu zamiast wyrażenia lambda, zwrócony element IQueryable nie ogólnego stosowania ciąg, a zatem metoda rzutowanie jest zwykle konieczna przed żadnych użytecznych może odbywać się z nim.</span><span class="sxs-lookup"><span data-stu-id="67e72-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="67e72-146">Liczba powiązanych jednostek bez ładowania je przy użyciu zapytań</span><span class="sxs-lookup"><span data-stu-id="67e72-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="67e72-147">Czasami warto wiedzieć, ile jednostek są powiązane z inną jednostkę w bazie danych bez rzeczywiście ponoszenia kosztów ładowania tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="67e72-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="67e72-148">Metody zapytania przy użyciu metody liczba LINQ można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="67e72-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="67e72-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="67e72-149">For example:</span></span>  

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
