---
title: Trwa ładowanie powiązanych jednostek - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 1d59e6c079e306158ed918cde16e69c9cb084711
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489053"
---
# <a name="loading-related-entities"></a>Trwa ładowanie powiązanych jednostek
Entity Framework obsługuje ładowanie powiązanych danych - eager, ładowania i powolne ładowanie jawne ładowanie na trzy sposoby. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="eagerly-loading"></a>Trwa ładowanie eagerly  

Wczesne ładowanie polega, według których kwerendy dla jednego typu obiektu również ładuje powiązanych jednostek jako część zapytania. Wczesne ładowanie odbywa się przy użyciu metody Include. Na przykład poniższych zapytań załaduje naszych blogach i wszystkie wpisy związane z każdego bloga.  

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

Zwróć uwagę, że Include metody rozszerzenia w przestrzeni nazw System.Data.Entity dlatego upewnij się, że używasz tej przestrzeni nazw.  

### <a name="eagerly-loading-multiple-levels"></a>Trwa ładowanie eagerly wiele poziomów  

Istnieje również możliwość eagerly obciążenia na wielu poziomach powiązanych jednostek. Poniższych zapytań Pokaż przykładów jak to zrobić dla kolekcji i odwołanie do właściwości nawigacji.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Należy pamiętać, że nie jest obecnie można filtrować dane są ładowane które powiązanych jednostek. Obejmują będą zawsze Przenieś wszystkie powiązane jednostki.  

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem  

Powolne ładowanie polega na zgodnie z którą jednostki lub kolekcję jednostek jest automatycznie ładowany z bazy danych, uzyskiwania dostępu do właściwości, odnoszące się do jednostki/jednostki po raz pierwszy. Korzystając z typów jednostki POCO, powolne ładowanie odbywa się tworzenia wystąpień typów pochodnych serwera proxy, a następnie zastępowanie właściwości wirtualnego można dodać punktów zaczepienia ładowania. Na przykład korzystając z klasy jednostki blogu, które są zdefiniowane poniżej, pokrewnych wpisów zostanie załadowany po raz pierwszy uzyskano dostęp do właściwości nawigacji wpisy:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Włącz powolne ładowanie wyłączone do serializacji  

Powolne ładowanie i serializacji nie Mieszaj dobrze, a jeśli nie chcesz zachować ostrożność można zakończyć się wykonanie zapytania dotyczącego całą bazę danych, po prostu, ponieważ ładowania z opóźnieniem jest włączone. Serializatory większość pracy, uzyskując dostęp do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala ładowania z opóźnieniem, więc podlegają serializacji z kolejnych jednostek. Na tych jednostkach właściwości są dostępne, a nawet więcej jednostek są ładowane. Jest dobrą praktyką, aby włączyć powolne ładowanie wyłączone przed serializacji jednostki. Poniższe sekcje pokazują, jak to zrobić.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Wyłączenie z opóźnieniem, ładowania dla właściwości określonej nawigacji  

Powolne ładowanie kolekcji wpisy można wyłączyć, wprowadzając niewirtualną właściwość wpisy:  

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

Ładowanie wpisów w kolekcji można nadal można osiągnąć przy użyciu wczesne ładowanie (zobacz *Eagerly ładowania* powyżej) lub metody obciążenia (zobacz *jawnego ładowania* poniżej).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Wyłącz powolne ładowanie dla wszystkich obiektów  

Powolne ładowanie można wyłączyć dla wszystkich jednostek w kontekście przez ustawienie flagi na właściwość konfiguracji. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Ładowanie powiązanych jednostek nadal można osiągnąć, stosując wczesne ładowanie (zobacz *Eagerly ładowania* powyżej) lub metody obciążenia (zobacz *jawnego ładowania* poniżej).  

## <a name="explicitly-loading"></a>Jawne ładowanie  

Nawet w przypadku powolne ładowanie wyłączone jest nadal możliwe opóźnieniem ładowanie powiązanych jednostek, ale muszą być wykonane za pomocą jawnego wywołania. Aby to zrobić, użyj metodę ładowania przy uruchamianiu powiązanej jednostki. Na przykład:  

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

Należy pamiętać, że Reference — metoda powinien być używany, jeśli określona jednostka ma właściwość nawigacji, do innego pojedynczej jednostki. Z drugiej strony metoda kolekcji należy używać, jeśli określona jednostka ma właściwości nawigacji kolekcji innych podmiotów.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Stosowanie filtrów, gdy jawnie ładowanie powiązanych jednostek  

Metoda zapytania zapewnia dostęp do podstawowego zapytania, które będą używać programu Entity Framework, gdy ładowanie powiązanych jednostek. Następnie służy LINQ do zastosowania filtrów do zapytania przed wykonaniem jego wywołaniem metody rozszerzenia LINQ, takich jak tolist —, obciążenia itp. Metoda zapytania mogą być używane z właściwości nawigacji kolekcji i odwołania, ale jest najbardziej użyteczne dla kolekcji, której może służyć do załadowania tylko część kolekcji. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

Za pomocą metody zapytania jest zazwyczaj najlepiej jest wyłączyć powolne ładowanie dla właściwości nawigacji. Jest tak, ponieważ w przeciwnym razie całej kolekcji może być ładowane automatycznie przez mechanizm powolne ładowanie przed lub po wykonaniu zapytania filtrowanego.  

Należy pamiętać, że podczas relacji może być określony jako ciągu zamiast wyrażenia lambda, zwrócony element IQueryable nie ogólnego stosowania ciąg, a zatem metoda rzutowanie jest zwykle konieczna przed żadnych użytecznych może odbywać się z nim.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Liczba powiązanych jednostek bez ładowania je przy użyciu zapytań  

Czasami warto wiedzieć, ile jednostek są powiązane z inną jednostkę w bazie danych bez rzeczywiście ponoszenia kosztów ładowania tych jednostek. Metody zapytania przy użyciu metody liczba LINQ można to zrobić. Na przykład:  

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
