---
title: Dane lokalne — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490457"
---
# <a name="local-data"></a><span data-ttu-id="14489-102">Dane lokalne</span><span class="sxs-lookup"><span data-stu-id="14489-102">Local Data</span></span>
<span data-ttu-id="14489-103">Uruchamianie zapytania LINQ bezpośrednio w odniesieniu do DbSet będą zawsze wysyłały zapytania do bazy danych, ale ma dostęp do danych, który jest aktualnie w pamięci przy użyciu właściwości DbSet.Local.</span><span class="sxs-lookup"><span data-stu-id="14489-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="14489-104">Dostępne dodatkowe informacje, które służy do śledzenia EF dotyczących jednostek za pomocą metody DbContext.Entry i DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="14489-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="14489-105">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="14489-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="14489-106">Spójrz na dane lokalne przy użyciu lokalnego</span><span class="sxs-lookup"><span data-stu-id="14489-106">Using Local to look at local data</span></span>  

<span data-ttu-id="14489-107">Lokalną właściwość DbSet zapewnia prosty dostęp do zestawu jednostek, obecnie są śledzone od kontekstu, które nie zostały oznaczone jako usunięte.</span><span class="sxs-lookup"><span data-stu-id="14489-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="14489-108">Uzyskiwanie dostępu do właściwości lokalnej nigdy nie powoduje, że zapytanie w celu wysłania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14489-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="14489-109">Oznacza to, że jest ona zwykle używana po zapytania została już wykonana.</span><span class="sxs-lookup"><span data-stu-id="14489-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="14489-110">Metoda rozszerzenia obciążenia może służyć do wykonywania zapytania, tak, aby w kontekście śledzi wyniki.</span><span class="sxs-lookup"><span data-stu-id="14489-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="14489-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="14489-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="14489-112">Gdyby dwóch blogów w bazie danych — "ADO.NET Blog" za pomocą BlogId 1 - i "Visual Studio Blog" za pomocą BlogId 2 oczekujemy można następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="14489-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="14489-113">Obrazuje to trzy punkty:</span><span class="sxs-lookup"><span data-stu-id="14489-113">This illustrates three points:</span></span>  

- <span data-ttu-id="14489-114">Nowego bloga "Moje nowego bloga" znajduje się w kolekcji lokalnej, nawet jeśli go nie został jeszcze zapisany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="14489-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="14489-115">Ten blog zawiera klucza podstawowego równą zero, ponieważ baza danych nie ma jeszcze wygenerowany rzeczywistego klucza dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="14489-116">Nie ma w blogu ADO.NET w kolekcji lokalnej, nawet jeśli jest nadal śledzony przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="14489-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="14489-117">Jest to spowodowane usunęliśmy z DbSet, w tym samym oznaczając je jako usunięty.</span><span class="sxs-lookup"><span data-stu-id="14489-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="14489-118">Gdy DbSet służy do wykonywania zapytania blogu oznaczone do usunięcia (ADO.NET Blog) znajduje się w wynikach i nowego bloga (Mój Blog nowego) jeszcze nie zostały zapisane w bazie danych nie znajduje się w wynikach.</span><span class="sxs-lookup"><span data-stu-id="14489-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="14489-119">Jest to spowodowane DbSet wykonuje zapytanie w bazie danych i wyników zwróconych zawsze odzwierciedla, co znajduje się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="14489-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="14489-120">Za pomocą lokalnego do dodawania i usuwania jednostek z kontekstu</span><span class="sxs-lookup"><span data-stu-id="14489-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="14489-121">Zwraca właściwości lokalnej na DbSet [observablecollection —](https://msdn.microsoft.com/library/ms668604.aspx) ze zdarzeniami podłączyłeś w taki sposób, że pozostaje on synchronizowany z zawartością kontekstu.</span><span class="sxs-lookup"><span data-stu-id="14489-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="14489-122">Oznacza to, że można dodać lub usuwane z kolekcji lokalnej lub DbSet jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="14489-123">Oznacza to również, że zapytania, które łączą nowe jednostki w kontekście spowoduje kolekcji lokalnej aktualizowana przy użyciu tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="14489-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="14489-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="14489-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="14489-125">Przy założeniu, że mieliśmy kilka wpisów oznakowane za pomocą programu "entity framework" i "asp.net" danych wyjściowych może wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="14489-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="14489-126">Obrazuje to trzy punkty:</span><span class="sxs-lookup"><span data-stu-id="14489-126">This illustrates three points:</span></span>  

- <span data-ttu-id="14489-127">Nowy wpis "What's New in EF" który został dodany do lokalnej kolekcji staje się śledzone przez kontekst w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="14489-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="14489-128">W związku z tym będzie można wstawić do bazy danych po wywołaniu funkcji SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="14489-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="14489-129">Wpis, który został usunięty z kolekcji lokalnej (Przewodnik dla początkujących EF) jest teraz oznaczony jako usunięty w kontekście.</span><span class="sxs-lookup"><span data-stu-id="14489-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="14489-130">W związku z tym będzie można usunąć z bazy danych po wywołaniu funkcji SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="14489-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="14489-131">Dodatkowe wpis (Przewodnik dla początkujących ASP.NET) ładowane do kontekstu z drugiego zapytania jest automatycznie dodawany do kolekcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="14489-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="14489-132">Jedno końcowe należy zwrócić uwagę na lokalny jest to, że ponieważ jej wydajności observablecollection — nie nadaje się doskonale dla dużej liczby obiektów.</span><span class="sxs-lookup"><span data-stu-id="14489-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="14489-133">Dlatego jeśli masz do czynienia z tysiącami jednostek w kontekście usługi nie może być wskazane zastosowanie lokalnego.</span><span class="sxs-lookup"><span data-stu-id="14489-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="14489-134">Przy użyciu lokalnego powiązanie danych WPF</span><span class="sxs-lookup"><span data-stu-id="14489-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="14489-135">Właściwości lokalnej na DbSet może służyć bezpośrednio do wiązania danych w aplikacji WPF, ponieważ jest on wystąpieniem observablecollection —.</span><span class="sxs-lookup"><span data-stu-id="14489-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="14489-136">Zgodnie z opisem w poprzedniej sekcji, oznacza to, że zostanie automatycznie zsynchronizowany z zawartością kontekstu i zawartość kontekst będzie automatycznie zsynchronizowany z nim.</span><span class="sxs-lookup"><span data-stu-id="14489-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="14489-137">Uwaga: należy do wstępnego wypełniania kolekcji lokalnej przy użyciu danych w celu zapewnienia żadnych czynności, aby powiązać od czasu lokalnego, nigdy nie powoduje zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14489-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="14489-138">To nie jest w odpowiednim miejscu, aby uzyskać pełną próbkę powiązanie danych WPF, ale są kluczowe elementy:</span><span class="sxs-lookup"><span data-stu-id="14489-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="14489-139">Instalator źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="14489-139">Setup a binding source</span></span>  
- <span data-ttu-id="14489-140">Powiązywanie lokalną właściwość do zestawu</span><span class="sxs-lookup"><span data-stu-id="14489-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="14489-141">Wypełnienie lokalnego przy użyciu zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14489-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="14489-142">Powiązania WPF do właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="14489-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="14489-143">Jeśli przeprowadzasz danych wzorzec/szczegół powiązania, możesz chcieć powiązać widok szczegółów z właściwością nawigacji jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="14489-144">Łatwy sposób na udostępnienie tej pracy jest używać ObservableCollection dla właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="14489-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="14489-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="14489-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="14489-146">Czyszczenie jednostek w SaveChanges przy użyciu lokalnego</span><span class="sxs-lookup"><span data-stu-id="14489-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="14489-147">W większości przypadków usunięte z właściwości nawigacji jednostki zostaną automatycznie oznaczone jako usunięte w kontekście.</span><span class="sxs-lookup"><span data-stu-id="14489-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="14489-148">Na przykład usunięcie obiektu wpis z kolekcji Blog.Posts, a następnie opublikuj, nie zostaną automatycznie usunięte po wywołaniu funkcji SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="14489-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="14489-149">Jeśli jest potrzebna do usunięcia może potrzebne do znalezienia tych jednostek delegujące i oznacz je jako usunięte przed wywołaniem funkcji SaveChanges lub jako część zgodnym z przesłoniętą SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="14489-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="14489-150">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="14489-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="14489-151">Powyższy kod używa kolekcji lokalnej, aby znaleźć wszystkie wpisy i znaczniki, Wszyscy, którzy nie mają odwołania blogu jako usunięty.</span><span class="sxs-lookup"><span data-stu-id="14489-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="14489-152">Konieczne jest wywołanie tolist —, ponieważ w przeciwnym razie kolekcji zostanie zmodyfikowany przez Usuń wywołanie, gdy jest wyliczany.</span><span class="sxs-lookup"><span data-stu-id="14489-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="14489-153">W innych sytuacjach można tworzyć zapytania bezpośrednio w odniesieniu do lokalnych właściwości bez użycia tolist — najpierw.</span><span class="sxs-lookup"><span data-stu-id="14489-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="14489-154">Używanie powiązania danych lokalnych i ToBindingList dla Windows Forms</span><span class="sxs-lookup"><span data-stu-id="14489-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="14489-155">Formularze Windows nie obsługuje powiązanie danych o pełnej wierności bezpośrednio za pomocą observablecollection —.</span><span class="sxs-lookup"><span data-stu-id="14489-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="14489-156">Jednak nadal umożliwia właściwości lokalne DbSet dla powiązania danych korzystać ze wszystkich zalet opisanego w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="14489-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="14489-157">Jest to osiągane przy użyciu metody rozszerzenia ToBindingList, co powoduje utworzenie [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementacji wspierana przez lokalny observablecollection —.</span><span class="sxs-lookup"><span data-stu-id="14489-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="14489-158">To nie jest w odpowiednim miejscu, aby uzyskać pełną próbkę powiązanie danych formularzy Windows, ale są kluczowe elementy:</span><span class="sxs-lookup"><span data-stu-id="14489-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="14489-159">Konfigurowanie obiektu źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="14489-159">Setup an object binding source</span></span>  
- <span data-ttu-id="14489-160">Powiązywanie właściwości lokalnej przy użyciu Local.ToBindingList() zestawu</span><span class="sxs-lookup"><span data-stu-id="14489-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="14489-161">Wypełnij lokalnego przy użyciu zapytania do bazy danych</span><span class="sxs-lookup"><span data-stu-id="14489-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="14489-162">Pobieranie szczegółowych informacji na temat śledzonych jednostek</span><span class="sxs-lookup"><span data-stu-id="14489-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="14489-163">Metoda wpis Większość przykładów w tej serii zwrócić DbEntityEntry wystąpienie jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="14489-164">Ten obiekt wpisu, następnie działa jako punktu wyjścia do zbierania informacji o jednostce, takie jak jego bieżący stan, a także do wykonywania operacji na jednostce, takich jak jawne ładowanie powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="14489-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="14489-165">Metody wpisy zwracają obiekty DbEntityEntry dla wielu lub wszystkich jednostek śledzony przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="14489-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="14489-166">Dzięki temu można zebrać informacje, lub wykonywać operacje na wiele jednostek, a nie tylko pojedynczy wpis.</span><span class="sxs-lookup"><span data-stu-id="14489-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="14489-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="14489-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="14489-168">Można zauważyć wprowadzamy klasy autora i czytnika, na przykład — zarówno w ramach tych zajęć implementować interfejs IPerson.</span><span class="sxs-lookup"><span data-stu-id="14489-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="14489-169">Załóżmy, że mamy następujące dane w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="14489-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="14489-170">Blog o BlogId = 1 i nazwa = "ADO.NET Blog"</span><span class="sxs-lookup"><span data-stu-id="14489-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="14489-171">Blog o BlogId = 2, a nazwa = "Blog Visual Studio"</span><span class="sxs-lookup"><span data-stu-id="14489-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="14489-172">Blog o BlogId = 3, a nazwa = "Blog programu .NET Framework"</span><span class="sxs-lookup"><span data-stu-id="14489-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="14489-173">Tworzenie za pomocą wartość IDAutora = 1 i Name = "Jan Bloggs"</span><span class="sxs-lookup"><span data-stu-id="14489-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="14489-174">Czytnik z ReaderId = 1 i Name = "Jan Kowalski"</span><span class="sxs-lookup"><span data-stu-id="14489-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="14489-175">Dane wyjściowe na uruchamianie kodu będą:</span><span class="sxs-lookup"><span data-stu-id="14489-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="14489-176">Te przykłady ilustrują kilka punktów:</span><span class="sxs-lookup"><span data-stu-id="14489-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="14489-177">Metody wpisy zwrócić wpisy dla jednostki w wszystkie stany, w tym usunięte.</span><span class="sxs-lookup"><span data-stu-id="14489-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="14489-178">Ten element, aby porównać lokalnego, co wyklucza usunąć jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="14489-179">Wpisy dla wszystkich typów jednostek są zwracane, gdy używana jest metoda wpisy nieogólnego.</span><span class="sxs-lookup"><span data-stu-id="14489-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="14489-180">Gdy używana jest metoda ogólnego wpisy wpisy zwracane są tylko dla jednostek, które są wystąpieniami typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="14489-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="14489-181">To zostało użyte powyżej można pobrać Wpisy dla wszystkich naszych blogach.</span><span class="sxs-lookup"><span data-stu-id="14489-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="14489-182">Pobierz wpisy dla wszystkich jednostek, które implementują IPerson została również.</span><span class="sxs-lookup"><span data-stu-id="14489-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="14489-183">W tym przykładzie pokazano, typ ogólny nie musi być typu rzeczywistego jednostki.</span><span class="sxs-lookup"><span data-stu-id="14489-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="14489-184">LINQ do obiektów może służyć do filtrowania wyników zwróconych.</span><span class="sxs-lookup"><span data-stu-id="14489-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="14489-185">To zostało użyte powyżej można znaleźć jednostki dowolnego typu, tak długo, jak zostaną one zmienione.</span><span class="sxs-lookup"><span data-stu-id="14489-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="14489-186">Należy pamiętać, wystąpień DbEntityEntry będzie zawsze zawierać Jednostka inna niż null.</span><span class="sxs-lookup"><span data-stu-id="14489-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="14489-187">Wpisów relacji i wpisy zastępcze nie są reprezentowane jako DbEntityEntry wystąpień, dzięki czemu nie ma potrzeby do filtrowania tych.</span><span class="sxs-lookup"><span data-stu-id="14489-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
