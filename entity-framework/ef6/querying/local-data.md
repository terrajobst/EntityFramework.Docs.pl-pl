---
title: Dane lokalne — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182432"
---
# <a name="local-data"></a><span data-ttu-id="be380-102">Dane lokalne</span><span class="sxs-lookup"><span data-stu-id="be380-102">Local Data</span></span>
<span data-ttu-id="be380-103">Uruchomienie zapytania LINQ bezpośrednio w odniesieniu do Nieogólnymi zawsze wyśle zapytanie do bazy danych, ale dostęp do danych znajdujących się obecnie w pamięci można uzyskać przy użyciu właściwości Nieogólnymi. local.</span><span class="sxs-lookup"><span data-stu-id="be380-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="be380-104">Możesz również uzyskać dostęp do dodatkowych informacji EF, które są śledzone na temat jednostek przy użyciu metod DbContext. entry i DbContext. ChangeTracker. Forms.</span><span class="sxs-lookup"><span data-stu-id="be380-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="be380-105">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="be380-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="be380-106">Używanie funkcji Local do przeglądania danych lokalnych</span><span class="sxs-lookup"><span data-stu-id="be380-106">Using Local to look at local data</span></span>  

<span data-ttu-id="be380-107">Właściwość Local elementu Nieogólnymi zapewnia prosty dostęp do jednostek zestawu, które są aktualnie śledzone przez kontekst i nie zostały oznaczone jako usunięte.</span><span class="sxs-lookup"><span data-stu-id="be380-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="be380-108">Uzyskanie dostępu do właściwości lokalnej nigdy nie powoduje wysłania zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be380-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="be380-109">Oznacza to, że jest on zwykle używany po wykonaniu zapytania.</span><span class="sxs-lookup"><span data-stu-id="be380-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="be380-110">Metoda Load Extension może służyć do wykonywania zapytania, tak aby kontekst śledzi wyniki.</span><span class="sxs-lookup"><span data-stu-id="be380-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="be380-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be380-111">For example:</span></span>  

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

<span data-ttu-id="be380-112">Jeśli mamy dwa blogi w bazie danych "ADO.NET blog" z BlogIdem 1 i "Blog programu Visual Studio" z BlogIdem 2 — możemy oczekiwać następujących danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="be380-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="be380-113">Ilustruje to trzy punkty:</span><span class="sxs-lookup"><span data-stu-id="be380-113">This illustrates three points:</span></span>  

- <span data-ttu-id="be380-114">Nowy blog "mój nowy blog" znajduje się w kolekcji lokalnej, mimo że nie został jeszcze zapisany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be380-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="be380-115">Ten blog ma klucz podstawowy równy zero, ponieważ baza danych nie wygenerowała jeszcze klucza rzeczywistego dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="be380-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="be380-116">Blog "ADO.NET" nie jest uwzględniony w kolekcji lokalnej, mimo że nadal jest śledzony przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="be380-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="be380-117">Wynika to z faktu, że został on usunięty z Nieogólnymi, co oznacza, że jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="be380-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="be380-118">Gdy Nieogólnymi jest używany do wykonywania zapytania, blog oznaczony jako do usunięcia (blog ADO.NET) jest zawarty w wynikach i nowym blogu (mój nowy blog), który nie został jeszcze zapisany w bazie danych, nie jest uwzględniony w wynikach.</span><span class="sxs-lookup"><span data-stu-id="be380-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="be380-119">Wynika to z faktu, że Nieogólnymi wykonuje zapytanie względem bazy danych, a zwrócone wyniki zawsze odzwierciedlają, co znajduje się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be380-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="be380-120">Używanie funkcji Local do dodawania i usuwania jednostek z kontekstu</span><span class="sxs-lookup"><span data-stu-id="be380-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="be380-121">Właściwość Local w Nieogólnymi zwraca [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) z zdarzeniami podłączonymi w taki sposób, aby pozostawały zsynchronizowane z zawartością kontekstu.</span><span class="sxs-lookup"><span data-stu-id="be380-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="be380-122">Oznacza to, że jednostki mogą być dodawane lub usuwane z lokalnej kolekcji lub Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="be380-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="be380-123">Oznacza to również, że zapytania, które przyjmują nowe jednostki w kontekście, spowodują aktualizację kolekcji lokalnej przy użyciu tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="be380-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="be380-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be380-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

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

<span data-ttu-id="be380-125">Przy założeniu, że mamy kilka wpisów otagowanych za pomocą elementów "Entity-Framework" i "asp.net", dane wyjściowe mogą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="be380-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```console
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

<span data-ttu-id="be380-126">Ilustruje to trzy punkty:</span><span class="sxs-lookup"><span data-stu-id="be380-126">This illustrates three points:</span></span>  

- <span data-ttu-id="be380-127">Nowy wpis "co nowego w programie EF", który został dodany do kolekcji lokalnej, zostanie śledzony przez kontekst w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="be380-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="be380-128">W związku z tym zostanie on wstawiony do bazy danych po wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="be380-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="be380-129">Wpis, który został usunięty z lokalnej kolekcji (Przewodnik EF początkującego), jest teraz oznaczony jako usunięty w kontekście.</span><span class="sxs-lookup"><span data-stu-id="be380-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="be380-130">W związku z tym zostanie on usunięty z bazy danych, gdy zostanie wywołana metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="be380-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="be380-131">Dodatkowa instrukcja post (ASP.NET początkujących) załadowana do kontekstu z drugim zapytaniem jest automatycznie dodawana do kolekcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="be380-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="be380-132">Warto zwrócić uwagę na to, że ponieważ jest to ObservableCollection wydajność, nie jest doskonałe dla dużej liczby jednostek.</span><span class="sxs-lookup"><span data-stu-id="be380-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="be380-133">W związku z tym w przypadku korzystania z tysięcy jednostek w Twoim kontekście może nie być zalecane użycie lokalnego.</span><span class="sxs-lookup"><span data-stu-id="be380-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="be380-134">Używanie lokalnego dla powiązania danych WPF</span><span class="sxs-lookup"><span data-stu-id="be380-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="be380-135">Właściwość Local w Nieogólnymi może być używana bezpośrednio do wiązania danych w aplikacji WPF, ponieważ jest to wystąpienie elementu ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="be380-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="be380-136">Zgodnie z opisem w poprzednich sekcjach oznacza to, że będzie on automatycznie zsynchronizowany z zawartością kontekstu, a zawartość kontekstu zostanie automatycznie zsynchronizowana z tym elementem.</span><span class="sxs-lookup"><span data-stu-id="be380-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="be380-137">Należy pamiętać, że w celu wstępnego wypełnienia kolekcji lokalnej z danymi, z których ma zostać utworzone powiązanie, od momentu, gdy lokalne nigdy nie powoduje, że zapytanie bazy danych jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="be380-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="be380-138">Nie jest to odpowiednie miejsce dla pełnego przykładu powiązania danych WPF, ale elementy klucza są następujące:</span><span class="sxs-lookup"><span data-stu-id="be380-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="be380-139">Skonfiguruj źródło powiązania</span><span class="sxs-lookup"><span data-stu-id="be380-139">Setup a binding source</span></span>  
- <span data-ttu-id="be380-140">Powiąż go z lokalną właściwością Twojego zestawu</span><span class="sxs-lookup"><span data-stu-id="be380-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="be380-141">Wypełnij lokalne przy użyciu zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="be380-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="be380-142">Powiązanie WPF z właściwościami nawigacji</span><span class="sxs-lookup"><span data-stu-id="be380-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="be380-143">Jeśli tworzysz powiązanie danych master/detail, możesz chcieć powiązać widok szczegółowy z właściwością nawigacji jednej z jednostek.</span><span class="sxs-lookup"><span data-stu-id="be380-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="be380-144">Prostym sposobem, aby to zrobić, jest użycie ObservableCollection dla właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="be380-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="be380-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be380-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="be380-146">Używanie funkcji Local do czyszczenia jednostek w metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="be380-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="be380-147">W większości przypadków jednostki usunięte z właściwości nawigacji nie zostaną automatycznie oznaczone jako usunięte w kontekście.</span><span class="sxs-lookup"><span data-stu-id="be380-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="be380-148">Na przykład, jeśli usuniesz obiekt post z kolekcji blog. Posts, ten wpis nie zostanie automatycznie usunięty po wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="be380-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="be380-149">Jeśli trzeba będzie usunąć te jednostki zawieszonego i oznaczyć je jako usunięte przed wywołaniem metody SaveChanges lub jako część zastąpionego metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="be380-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="be380-150">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be380-150">For example:</span></span>  

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

<span data-ttu-id="be380-151">Powyższy kod używa kolekcji lokalnej do znajdowania wszystkich wpisów i oznacza, że nie ma odwołań do blogów jako usuniętych.</span><span class="sxs-lookup"><span data-stu-id="be380-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="be380-152">Wywołanie ToList — jest wymagane, ponieważ w przeciwnym razie kolekcja zostanie zmodyfikowana przez wywołanie usuwania podczas wyliczania.</span><span class="sxs-lookup"><span data-stu-id="be380-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="be380-153">W większości innych sytuacji można wykonywać zapytania bezpośrednio względem właściwości lokalnej bez uprzedniego używania ToList —.</span><span class="sxs-lookup"><span data-stu-id="be380-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="be380-154">Używanie lokalnych i ToBindingList dla powiązania danych Windows Forms</span><span class="sxs-lookup"><span data-stu-id="be380-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="be380-155">Windows Forms nie obsługuje bezpośredniego powiązania danych o pełnej wierności przy użyciu ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="be380-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="be380-156">Można jednak nadal używać lokalnej właściwości Nieogólnymi dla powiązania danych, aby uzyskać wszystkie korzyści opisane w poprzednich sekcjach.</span><span class="sxs-lookup"><span data-stu-id="be380-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="be380-157">Jest to realizowane za pomocą metody rozszerzenia ToBindingList, która tworzy implementację [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) utworzoną przez lokalny ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="be380-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="be380-158">Nie jest to odpowiednie miejsce dla przykładowego powiązania danych pełnej Windows Forms, ale elementy klucza są następujące:</span><span class="sxs-lookup"><span data-stu-id="be380-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="be380-159">Skonfiguruj źródło powiązania obiektu</span><span class="sxs-lookup"><span data-stu-id="be380-159">Setup an object binding source</span></span>  
- <span data-ttu-id="be380-160">Powiąż go z lokalną właściwością zestawu przy użyciu lokalnego. ToBindingList ()</span><span class="sxs-lookup"><span data-stu-id="be380-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="be380-161">Wypełnij lokalne przy użyciu zapytania do bazy danych</span><span class="sxs-lookup"><span data-stu-id="be380-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="be380-162">Pobieranie szczegółowych informacji o śledzonych jednostkach</span><span class="sxs-lookup"><span data-stu-id="be380-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="be380-163">Wiele przykładów w tej serii używa metody wejścia do zwrócenia wystąpienia DbEntityEntry dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="be380-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="be380-164">Ten obiekt wejścia pełni rolę punktu wyjścia do zbierania informacji o jednostce, takiej jak jej bieżący stan, a także do wykonywania operacji na jednostce, takich jak jawne ładowanie powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="be380-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="be380-165">Metody wpisów zwracają obiekty DbEntityEntry dla wielu lub wszystkich jednostek śledzonych przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="be380-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="be380-166">Dzięki temu można zbierać informacje lub wykonywać operacje na wielu jednostkach, a nie tylko pojedynczym wpisie.</span><span class="sxs-lookup"><span data-stu-id="be380-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="be380-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be380-167">For example:</span></span>  

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

<span data-ttu-id="be380-168">Zauważymy, że wprowadzamy klasy autor i czytelnik do przykładu — obie te klasy implementują interfejs IPerson.</span><span class="sxs-lookup"><span data-stu-id="be380-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="be380-169">Załóżmy, że w bazie danych znajdują się następujące dane:</span><span class="sxs-lookup"><span data-stu-id="be380-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="be380-170">Blog z BlogId = 1 i name = ' ADO.NET bloga '</span><span class="sxs-lookup"><span data-stu-id="be380-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="be380-171">Blog z BlogId = 2 i Name = "Blog programu Visual Studio"</span><span class="sxs-lookup"><span data-stu-id="be380-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="be380-172">Blog z BlogId = 3 i Name = ".NET Framework blog"</span><span class="sxs-lookup"><span data-stu-id="be380-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="be380-173">Autor z AuthorId = 1 i Name = "Jan Bloggs"</span><span class="sxs-lookup"><span data-stu-id="be380-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="be380-174">Czytnik z ReaderId = 1 i Name = "Jan Kowalski"</span><span class="sxs-lookup"><span data-stu-id="be380-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="be380-175">Wyjściem z uruchamiania kodu będzie:</span><span class="sxs-lookup"><span data-stu-id="be380-175">The output from running the code would be:</span></span>  

```console
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

<span data-ttu-id="be380-176">Przykłady te ilustrują kilka punktów:</span><span class="sxs-lookup"><span data-stu-id="be380-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="be380-177">Metody wpisów zwracają wpisy dla jednostek we wszystkich stanach, łącznie z usuniętymi.</span><span class="sxs-lookup"><span data-stu-id="be380-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="be380-178">Porównaj ten element z lokalnym, co spowoduje wyłączenie usuniętych jednostek.</span><span class="sxs-lookup"><span data-stu-id="be380-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="be380-179">Wpisy dla wszystkich typów jednostek są zwracane, gdy jest używana metoda nieogólna wpisów.</span><span class="sxs-lookup"><span data-stu-id="be380-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="be380-180">Gdy są używane metody wpisów ogólnych, zwracane są tylko dla jednostek, które są wystąpieniami typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="be380-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="be380-181">Ta wartość została wcześniej użyta w celu pobrania wpisów dla wszystkich blogów.</span><span class="sxs-lookup"><span data-stu-id="be380-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="be380-182">Był on również używany do pobierania wpisów dla wszystkich jednostek, które implementują IPerson.</span><span class="sxs-lookup"><span data-stu-id="be380-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="be380-183">Pokazuje to, że typ generyczny nie musi być rzeczywistym typem jednostki.</span><span class="sxs-lookup"><span data-stu-id="be380-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="be380-184">LINQ to Objects można użyć do filtrowania zwracanych wyników.</span><span class="sxs-lookup"><span data-stu-id="be380-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="be380-185">Ta wartość została użyta powyżej, aby znaleźć jednostki dowolnego typu, o ile są one modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="be380-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="be380-186">Należy zauważyć, że wystąpienia DbEntityEntry zawsze zawierają jednostkę o wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="be380-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="be380-187">Wpisy relacji i wpisy zastępcze nie są reprezentowane jako wystąpienia DbEntityEntry, więc nie ma potrzeby filtrowania.</span><span class="sxs-lookup"><span data-stu-id="be380-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
