---
title: Ładowanie powiązanych jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005477"
---
# <a name="loading-related-entities"></a>Ładowanie powiązanych jednostek

Entity Framework obsługuje trzy sposoby ładowania powiązanych danych — eager ładowania, ładowania z opóźnieniem i bezpośredniego ładowania. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.

## <a name="eagerly-loading"></a>Ładowanie Eagerly

Ładowanie eager jest procesem, w którym zapytanie dla jednego typu jednostki również ładuje powiązane jednostki jako część zapytania. Ładowanie eager jest realizowane przy użyciu metody include. Na przykład poniższe zapytania spowodują załadowanie blogów i wszystkich wpisów związanych z każdym blogiem.

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> Include to metoda rozszerzająca w przestrzeni nazw System. Data. Entity, dlatego upewnij się, że używasz tej przestrzeni nazw.

### <a name="eagerly-loading-multiple-levels"></a>Eagerly ładowanie wielu poziomów

Możliwe jest również eagerly załadowanie wielu poziomów jednostek pokrewnych. W poniższych zapytaniach pokazano, jak to zrobić dla właściwości nawigacji kolekcji i odwołań.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> Nie jest obecnie możliwe filtrowanie powiązanych jednostek, które są ładowane. Funkcja include będzie zawsze obejmować wszystkie powiązane jednostki.  

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

Ładowanie z opóźnieniem to proces, w którym jednostka lub kolekcja jednostek są automatycznie ładowane z bazy danych przy pierwszym użyciu właściwości odwołującej się do jednostki/jednostek. W przypadku korzystania z typów jednostek POCO pobieranie z opóźnieniem jest realizowane przez utworzenie wystąpień pochodnych typów proxy, a następnie Zastępowanie właściwości wirtualnych w celu dodania punktu zaczepienia ładowania. Na przykład podczas korzystania z klasy jednostki blogu zdefiniowanej poniżej powiązane wpisy zostaną załadowane przy pierwszej próbie uzyskania dostępu do właściwości nawigacji:

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Włącz ładowanie z opóźnieniem dla serializacji

Pobieranie z opóźnieniem i Serializacja nie są dobrze mieszane i jeśli nie jest to konieczne, możesz zakończyć wykonywanie zapytań dotyczących całej bazy danych, tylko ponieważ jest włączone ładowanie z opóźnieniem. Większość serializatorów pracuje przez uzyskanie dostępu do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala ładowanie z opóźnieniem, więc więcej jednostek uzyskuje serializacji. Dostęp do tych właściwości jednostek jest możliwy, a jeszcze więcej jednostek jest załadowanych. Dobrym sposobem jest włączenie ładowania z opóźnieniem przed Serializacja jednostki. Poniższe sekcje pokazują, jak to zrobić.

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Wyłączanie ładowania z opóźnieniem dla określonych właściwości nawigacji

Pobieranie z opóźnieniem kolekcji ogłoszeń można wyłączyć, wprowadzając Właściwość Posts, która nie jest wirtualna:

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

Ładowanie kolekcji ogłoszeń można nadal osiągnąć przy użyciu ładowania eager (zobacz *Eagerly ładowanie* powyżej) lub metodę load (zobacz *jawne ładowanie* poniżej).

### <a name="turn-off-lazy-loading-for-all-entities"></a>Wyłącz ładowanie z opóźnieniem dla wszystkich jednostek

Ładowanie z opóźnieniem można wyłączyć dla wszystkich jednostek w kontekście przez ustawienie flagi właściwości konfiguracja. Na przykład:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

Ładowanie pokrewnych jednostek można nadal osiągnąć przy użyciu ładowania eager (zobacz *Eagerly ładowanie* powyżej) lub metodę load (zobacz *jawne ładowanie* poniżej).

## <a name="explicitly-loading"></a>Jawne ładowanie

Nawet po wyłączeniu ładowania z opóźnieniem nadal jest możliwe opóźnieniem pokrewnych jednostek, ale należy je wykonać z jawnym wywołaniem. W tym celu należy użyć metody Load dla wpisu jednostki powiązanej. Na przykład:

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> Metoda referencyjna powinna być używana, gdy jednostka ma właściwość nawigacji do innej pojedynczej jednostki. Z drugiej strony Metoda kolekcji powinna być używana, gdy jednostka ma właściwość nawigacji do kolekcji innych jednostek.

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Stosowanie filtrów podczas jawnego ładowania powiązanych jednostek

Metoda zapytania zapewnia dostęp do bazowego zapytania, którego Entity Framework będzie używać podczas ładowania powiązanych jednostek. Następnie można użyć LINQ do zastosowania filtrów do zapytania przed wykonaniem go za pomocą wywołania metody rozszerzenia LINQ, takiej jak ToList —, Load itp. Metoda zapytania może być używana z właściwościami odwołania i nawigacji kolekcji, ale jest najbardziej przydatna w przypadku kolekcji, których można użyć do załadowania tylko części kolekcji. Na przykład:

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

Korzystając z metody zapytania, zazwyczaj najlepiej jest wyłączyć ładowanie z opóźnieniem dla właściwości nawigacji. Dzieje się tak, ponieważ w przeciwnym razie cała kolekcja może zostać załadowana automatycznie przez mechanizm ładowania z opóźnieniem przed lub po wykonaniu odfiltrowanego zapytania.

> [!NOTE]
> Gdy relacja może być określona jako ciąg zamiast wyrażenia lambda, zwracany interfejs IQueryable nie jest ogólny, gdy jest używany ciąg i dlatego Metoda Cast jest zwykle wymagana, zanim wszystko będzie przydatne.

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Używanie zapytania do zliczania powiązanych jednostek bez ich ładowania

Czasami warto wiedzieć, ile jednostek jest związanych z inną jednostką w bazie danych, bez faktycznego ponoszenia kosztów ładowania wszystkich jednostek. W tym celu można użyć metody zapytania z metodą Count LINQ. Na przykład:

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
