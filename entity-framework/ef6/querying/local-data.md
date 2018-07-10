---
title: Dane lokalne — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912740"
---
# <a name="local-data"></a>Dane lokalne
Uruchamianie zapytania LINQ bezpośrednio w odniesieniu do DbSet będą zawsze wysyłały zapytania do bazy danych, ale ma dostęp do danych, który jest aktualnie w pamięci przy użyciu właściwości DbSet.Local. Dostępne dodatkowe informacje, które służy do śledzenia EF dotyczących jednostek za pomocą metody DbContext.Entry i DbContext.ChangeTracker.Entries. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="using-local-to-look-at-local-data"></a>Spójrz na dane lokalne przy użyciu lokalnego  

Lokalną właściwość DbSet zapewnia prosty dostęp do zestawu jednostek, obecnie są śledzone od kontekstu, które nie zostały oznaczone jako usunięte. Uzyskiwanie dostępu do właściwości lokalnej nigdy nie powoduje, że zapytanie w celu wysłania do bazy danych. Oznacza to, że jest ona zwykle używana po zapytania została już wykonana. Metoda rozszerzenia obciążenia może służyć do wykonywania zapytania, tak, aby w kontekście śledzi wyniki. Na przykład:  

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

Gdyby dwóch blogów w bazie danych — "ADO.NET Blog" za pomocą BlogId 1 - i "Visual Studio Blog" za pomocą BlogId 2 oczekujemy można następujące dane wyjściowe:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Obrazuje to trzy punkty:  

- Nowego bloga "Moje nowego bloga" znajduje się w kolekcji lokalnej, nawet jeśli go nie został jeszcze zapisany w bazie danych. Ten blog zawiera klucza podstawowego równą zero, ponieważ baza danych nie ma jeszcze wygenerowany rzeczywistego klucza dla jednostki.  
- Nie ma w blogu ADO.NET w kolekcji lokalnej, nawet jeśli jest nadal śledzony przez kontekst. Jest to spowodowane usunęliśmy z DbSet, w tym samym oznaczając je jako usunięty.  
- Gdy DbSet służy do wykonywania zapytania blogu oznaczone do usunięcia (ADO.NET Blog) znajduje się w wynikach i nowego bloga (Mój Blog nowego) jeszcze nie zostały zapisane w bazie danych nie znajduje się w wynikach. Jest to spowodowane DbSet wykonuje zapytanie w bazie danych i wyników zwróconych zawsze odzwierciedla, co znajduje się w bazie danych.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Za pomocą lokalnego do dodawania i usuwania jednostek z kontekstu  

Zwraca właściwości lokalnej na DbSet [observablecollection —](https://msdn.microsoft.com/library/ms668604.aspx) ze zdarzeniami podłączyłeś w taki sposób, że pozostaje on synchronizowany z zawartością kontekstu. Oznacza to, że można dodać lub usuwane z kolekcji lokalnej lub DbSet jednostki. Oznacza to również, że zapytania, które łączą nowe jednostki w kontekście spowoduje kolekcji lokalnej aktualizowana przy użyciu tych jednostek. Na przykład:  

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

Przy założeniu, że mieliśmy kilka wpisów oznakowane za pomocą programu "entity framework" i "asp.net" danych wyjściowych może wyglądać mniej więcej tak:  

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

Obrazuje to trzy punkty:  

- Nowy wpis "What's New in EF" który został dodany do lokalnej kolekcji staje się śledzone przez kontekst w stanie dodany. W związku z tym będzie można wstawić do bazy danych po wywołaniu funkcji SaveChanges.  
- Wpis, który został usunięty z kolekcji lokalnej (Przewodnik dla początkujących EF) jest teraz oznaczony jako usunięty w kontekście. W związku z tym będzie można usunąć z bazy danych po wywołaniu funkcji SaveChanges.  
- Dodatkowe wpis (Przewodnik dla początkujących ASP.NET) ładowane do kontekstu z drugiego zapytania jest automatycznie dodawany do kolekcji lokalnej.  

Jedno końcowe należy zwrócić uwagę na lokalny jest to, że ponieważ jej wydajności observablecollection — nie nadaje się doskonale dla dużej liczby obiektów. Dlatego jeśli masz do czynienia z tysiącami jednostek w kontekście usługi nie może być wskazane zastosowanie lokalnego.  

## <a name="using-local-for-wpf-data-binding"></a>Przy użyciu lokalnego powiązanie danych WPF  

Właściwości lokalnej na DbSet może służyć bezpośrednio do wiązania danych w aplikacji WPF, ponieważ jest on wystąpieniem observablecollection —. Zgodnie z opisem w poprzedniej sekcji, oznacza to, że zostanie automatycznie zsynchronizowany z zawartością kontekstu i zawartość kontekst będzie automatycznie zsynchronizowany z nim. Uwaga: należy do wstępnego wypełniania kolekcji lokalnej przy użyciu danych w celu zapewnienia żadnych czynności, aby powiązać od czasu lokalnego, nigdy nie powoduje zapytania do bazy danych.  

To nie jest w odpowiednim miejscu, aby uzyskać pełną próbkę powiązanie danych WPF, ale są kluczowe elementy:  

- Instalator źródło wiążące  
- Powiązywanie lokalną właściwość do zestawu  
- Wypełnienie lokalnego przy użyciu zapytania do bazy danych.  

## <a name="wpf-binding-to-navigation-properties"></a>Powiązania WPF do właściwości nawigacji  

Jeśli przeprowadzasz danych wzorzec/szczegół powiązania, możesz chcieć powiązać widok szczegółów z właściwością nawigacji jednej jednostki. Łatwy sposób na udostępnienie tej pracy jest używać ObservableCollection dla właściwości nawigacji. Na przykład:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Czyszczenie jednostek w SaveChanges przy użyciu lokalnego  

W większości przypadków usunięte z właściwości nawigacji jednostki zostaną automatycznie oznaczone jako usunięte w kontekście. Na przykład usunięcie obiektu wpis z kolekcji Blog.Posts, a następnie opublikuj, nie zostaną automatycznie usunięte po wywołaniu funkcji SaveChanges. Jeśli jest potrzebna do usunięcia może potrzebne do znalezienia tych jednostek delegujące i oznacz je jako usunięte przed wywołaniem funkcji SaveChanges lub jako część zgodnym z przesłoniętą SaveChanges. Na przykład:  

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

Powyższy kod używa kolekcji lokalnej, aby znaleźć wszystkie wpisy i znaczniki, Wszyscy, którzy nie mają odwołania blogu jako usunięty. Konieczne jest wywołanie tolist —, ponieważ w przeciwnym razie kolekcji zostanie zmodyfikowany przez Usuń wywołanie, gdy jest wyliczany. W innych sytuacjach można tworzyć zapytania bezpośrednio w odniesieniu do lokalnych właściwości bez użycia tolist — najpierw.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Używanie powiązania danych lokalnych i ToBindingList dla Windows Forms  

Formularze Windows nie obsługuje powiązanie danych o pełnej wierności bezpośrednio za pomocą observablecollection —. Jednak nadal umożliwia właściwości lokalne DbSet dla powiązania danych korzystać ze wszystkich zalet opisanego w poprzedniej sekcji. Jest to osiągane przy użyciu metody rozszerzenia ToBindingList, co powoduje utworzenie [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementacji wspierana przez lokalny observablecollection —.  

To nie jest w odpowiednim miejscu, aby uzyskać pełną próbkę powiązanie danych formularzy Windows, ale są kluczowe elementy:  

- Konfigurowanie obiektu źródło wiążące  
- Powiązywanie właściwości lokalnej przy użyciu Local.ToBindingList() zestawu  
- Wypełnij lokalnego przy użyciu zapytania do bazy danych  

## <a name="getting-detailed-information-about-tracked-entities"></a>Pobieranie szczegółowych informacji na temat śledzonych jednostek  

Metoda wpis Większość przykładów w tej serii zwrócić DbEntityEntry wystąpienie jednostki. Ten obiekt wpisu, następnie działa jako punktu wyjścia do zbierania informacji o jednostce, takie jak jego bieżący stan, a także do wykonywania operacji na jednostce, takich jak jawne ładowanie powiązanych jednostek.  

Metody wpisy zwracają obiekty DbEntityEntry dla wielu lub wszystkich jednostek śledzony przez kontekst. Dzięki temu można zebrać informacje, lub wykonywać operacje na wiele jednostek, a nie tylko pojedynczy wpis. Na przykład:  

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

Można zauważyć wprowadzamy klasy autora i czytnika, na przykład — zarówno w ramach tych zajęć implementować interfejs IPerson.  

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

Załóżmy, że mamy następujące dane w bazie danych:

Blog o BlogId = 1 i nazwa = "ADO.NET Blog"  
Blog o BlogId = 2, a nazwa = "Blog Visual Studio"  
Blog o BlogId = 3, a nazwa = "Blog programu .NET Framework"  
Tworzenie za pomocą wartość IDAutora = 1 i Name = "Jan Bloggs"  
Czytnik z ReaderId = 1 i Name = "Jan Kowalski"  

Dane wyjściowe na uruchamianie kodu będą:  

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

Te przykłady ilustrują kilka punktów:  

- Metody wpisy zwrócić wpisy dla jednostki w wszystkie stany, w tym usunięte. Ten element, aby porównać lokalnego, co wyklucza usunąć jednostki.  
- Wpisy dla wszystkich typów jednostek są zwracane, gdy używana jest metoda wpisy nieogólnego. Gdy używana jest metoda ogólnego wpisy wpisy zwracane są tylko dla jednostek, które są wystąpieniami typu ogólnego. To zostało użyte powyżej można pobrać Wpisy dla wszystkich naszych blogach. Pobierz wpisy dla wszystkich jednostek, które implementują IPerson została również. W tym przykładzie pokazano, typ ogólny nie musi być typu rzeczywistego jednostki.  
- LINQ do obiektów może służyć do filtrowania wyników zwróconych. To zostało użyte powyżej można znaleźć jednostki dowolnego typu, tak długo, jak zostaną one zmienione.  

Należy pamiętać, wystąpień DbEntityEntry będzie zawsze zawierać Jednostka inna niż null. Wpisów relacji i wpisy zastępcze nie są reprezentowane jako DbEntityEntry wystąpień, dzięki czemu nie ma potrzeby do filtrowania tych.
