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
# <a name="local-data"></a>Dane lokalne
Uruchomienie zapytania LINQ bezpośrednio w odniesieniu do Nieogólnymi zawsze wyśle zapytanie do bazy danych, ale dostęp do danych znajdujących się obecnie w pamięci można uzyskać przy użyciu właściwości Nieogólnymi. local. Możesz również uzyskać dostęp do dodatkowych informacji EF, które są śledzone na temat jednostek przy użyciu metod DbContext. entry i DbContext. ChangeTracker. Forms. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

## <a name="using-local-to-look-at-local-data"></a>Używanie funkcji Local do przeglądania danych lokalnych  

Właściwość Local elementu Nieogólnymi zapewnia prosty dostęp do jednostek zestawu, które są aktualnie śledzone przez kontekst i nie zostały oznaczone jako usunięte. Uzyskanie dostępu do właściwości lokalnej nigdy nie powoduje wysłania zapytania do bazy danych. Oznacza to, że jest on zwykle używany po wykonaniu zapytania. Metoda Load Extension może służyć do wykonywania zapytania, tak aby kontekst śledzi wyniki. Na przykład:  

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

Jeśli mamy dwa blogi w bazie danych "ADO.NET blog" z BlogIdem 1 i "Blog programu Visual Studio" z BlogIdem 2 — możemy oczekiwać następujących danych wyjściowych:  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Ilustruje to trzy punkty:  

- Nowy blog "mój nowy blog" znajduje się w kolekcji lokalnej, mimo że nie został jeszcze zapisany w bazie danych. Ten blog ma klucz podstawowy równy zero, ponieważ baza danych nie wygenerowała jeszcze klucza rzeczywistego dla jednostki.  
- Blog "ADO.NET" nie jest uwzględniony w kolekcji lokalnej, mimo że nadal jest śledzony przez kontekst. Wynika to z faktu, że został on usunięty z Nieogólnymi, co oznacza, że jest usuwany.  
- Gdy Nieogólnymi jest używany do wykonywania zapytania, blog oznaczony jako do usunięcia (blog ADO.NET) jest zawarty w wynikach i nowym blogu (mój nowy blog), który nie został jeszcze zapisany w bazie danych, nie jest uwzględniony w wynikach. Wynika to z faktu, że Nieogólnymi wykonuje zapytanie względem bazy danych, a zwrócone wyniki zawsze odzwierciedlają, co znajduje się w bazie danych.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Używanie funkcji Local do dodawania i usuwania jednostek z kontekstu  

Właściwość Local w Nieogólnymi zwraca [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) z zdarzeniami podłączonymi w taki sposób, aby pozostawały zsynchronizowane z zawartością kontekstu. Oznacza to, że jednostki mogą być dodawane lub usuwane z lokalnej kolekcji lub Nieogólnymi. Oznacza to również, że zapytania, które przyjmują nowe jednostki w kontekście, spowodują aktualizację kolekcji lokalnej przy użyciu tych jednostek. Na przykład:  

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

Przy założeniu, że mamy kilka wpisów otagowanych za pomocą elementów "Entity-Framework" i "asp.net", dane wyjściowe mogą wyglądać następująco:  

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

Ilustruje to trzy punkty:  

- Nowy wpis "co nowego w programie EF", który został dodany do kolekcji lokalnej, zostanie śledzony przez kontekst w stanie dodany. W związku z tym zostanie on wstawiony do bazy danych po wywołaniu metody SaveChanges.  
- Wpis, który został usunięty z lokalnej kolekcji (Przewodnik EF początkującego), jest teraz oznaczony jako usunięty w kontekście. W związku z tym zostanie on usunięty z bazy danych, gdy zostanie wywołana metody SaveChanges.  
- Dodatkowa instrukcja post (ASP.NET początkujących) załadowana do kontekstu z drugim zapytaniem jest automatycznie dodawana do kolekcji lokalnej.  

Warto zwrócić uwagę na to, że ponieważ jest to ObservableCollection wydajność, nie jest doskonałe dla dużej liczby jednostek. W związku z tym w przypadku korzystania z tysięcy jednostek w Twoim kontekście może nie być zalecane użycie lokalnego.  

## <a name="using-local-for-wpf-data-binding"></a>Używanie lokalnego dla powiązania danych WPF  

Właściwość Local w Nieogólnymi może być używana bezpośrednio do wiązania danych w aplikacji WPF, ponieważ jest to wystąpienie elementu ObservableCollection. Zgodnie z opisem w poprzednich sekcjach oznacza to, że będzie on automatycznie zsynchronizowany z zawartością kontekstu, a zawartość kontekstu zostanie automatycznie zsynchronizowana z tym elementem. Należy pamiętać, że w celu wstępnego wypełnienia kolekcji lokalnej z danymi, z których ma zostać utworzone powiązanie, od momentu, gdy lokalne nigdy nie powoduje, że zapytanie bazy danych jest wymagane.  

Nie jest to odpowiednie miejsce dla pełnego przykładu powiązania danych WPF, ale elementy klucza są następujące:  

- Skonfiguruj źródło powiązania  
- Powiąż go z lokalną właściwością Twojego zestawu  
- Wypełnij lokalne przy użyciu zapytania do bazy danych.  

## <a name="wpf-binding-to-navigation-properties"></a>Powiązanie WPF z właściwościami nawigacji  

Jeśli tworzysz powiązanie danych master/detail, możesz chcieć powiązać widok szczegółowy z właściwością nawigacji jednej z jednostek. Prostym sposobem, aby to zrobić, jest użycie ObservableCollection dla właściwości nawigacji. Na przykład:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Używanie funkcji Local do czyszczenia jednostek w metody SaveChanges  

W większości przypadków jednostki usunięte z właściwości nawigacji nie zostaną automatycznie oznaczone jako usunięte w kontekście. Na przykład, jeśli usuniesz obiekt post z kolekcji blog. Posts, ten wpis nie zostanie automatycznie usunięty po wywołaniu metody SaveChanges. Jeśli trzeba będzie usunąć te jednostki zawieszonego i oznaczyć je jako usunięte przed wywołaniem metody SaveChanges lub jako część zastąpionego metody SaveChanges. Na przykład:  

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

Powyższy kod używa kolekcji lokalnej do znajdowania wszystkich wpisów i oznacza, że nie ma odwołań do blogów jako usuniętych. Wywołanie ToList — jest wymagane, ponieważ w przeciwnym razie kolekcja zostanie zmodyfikowana przez wywołanie usuwania podczas wyliczania. W większości innych sytuacji można wykonywać zapytania bezpośrednio względem właściwości lokalnej bez uprzedniego używania ToList —.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Używanie lokalnych i ToBindingList dla powiązania danych Windows Forms  

Windows Forms nie obsługuje bezpośredniego powiązania danych o pełnej wierności przy użyciu ObservableCollection. Można jednak nadal używać lokalnej właściwości Nieogólnymi dla powiązania danych, aby uzyskać wszystkie korzyści opisane w poprzednich sekcjach. Jest to realizowane za pomocą metody rozszerzenia ToBindingList, która tworzy implementację [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) utworzoną przez lokalny ObservableCollection.  

Nie jest to odpowiednie miejsce dla przykładowego powiązania danych pełnej Windows Forms, ale elementy klucza są następujące:  

- Skonfiguruj źródło powiązania obiektu  
- Powiąż go z lokalną właściwością zestawu przy użyciu lokalnego. ToBindingList ()  
- Wypełnij lokalne przy użyciu zapytania do bazy danych  

## <a name="getting-detailed-information-about-tracked-entities"></a>Pobieranie szczegółowych informacji o śledzonych jednostkach  

Wiele przykładów w tej serii używa metody wejścia do zwrócenia wystąpienia DbEntityEntry dla jednostki. Ten obiekt wejścia pełni rolę punktu wyjścia do zbierania informacji o jednostce, takiej jak jej bieżący stan, a także do wykonywania operacji na jednostce, takich jak jawne ładowanie powiązanej jednostki.  

Metody wpisów zwracają obiekty DbEntityEntry dla wielu lub wszystkich jednostek śledzonych przez kontekst. Dzięki temu można zbierać informacje lub wykonywać operacje na wielu jednostkach, a nie tylko pojedynczym wpisie. Na przykład:  

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

Zauważymy, że wprowadzamy klasy autor i czytelnik do przykładu — obie te klasy implementują interfejs IPerson.  

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

Załóżmy, że w bazie danych znajdują się następujące dane:

Blog z BlogId = 1 i name = ' ADO.NET bloga '  
Blog z BlogId = 2 i Name = "Blog programu Visual Studio"  
Blog z BlogId = 3 i Name = ".NET Framework blog"  
Autor z AuthorId = 1 i Name = "Jan Bloggs"  
Czytnik z ReaderId = 1 i Name = "Jan Kowalski"  

Wyjściem z uruchamiania kodu będzie:  

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

Przykłady te ilustrują kilka punktów:  

- Metody wpisów zwracają wpisy dla jednostek we wszystkich stanach, łącznie z usuniętymi. Porównaj ten element z lokalnym, co spowoduje wyłączenie usuniętych jednostek.  
- Wpisy dla wszystkich typów jednostek są zwracane, gdy jest używana metoda nieogólna wpisów. Gdy są używane metody wpisów ogólnych, zwracane są tylko dla jednostek, które są wystąpieniami typu ogólnego. Ta wartość została wcześniej użyta w celu pobrania wpisów dla wszystkich blogów. Był on również używany do pobierania wpisów dla wszystkich jednostek, które implementują IPerson. Pokazuje to, że typ generyczny nie musi być rzeczywistym typem jednostki.  
- LINQ to Objects można użyć do filtrowania zwracanych wyników. Ta wartość została użyta powyżej, aby znaleźć jednostki dowolnego typu, o ile są one modyfikowane.  

Należy zauważyć, że wystąpienia DbEntityEntry zawsze zawierają jednostkę o wartości innej niż null. Wpisy relacji i wpisy zastępcze nie są reprezentowane jako wystąpienia DbEntityEntry, więc nie ma potrzeby filtrowania.
