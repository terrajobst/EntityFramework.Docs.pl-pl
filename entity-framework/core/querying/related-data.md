---
title: "Trwa ładowanie powiązanych danych - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: dadc6235c3879ae27ad5c99988a5e594872045df
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="loading-related-data"></a>Trwa ładowanie powiązanych danych

Program Entity Framework Core umożliwia przy użyciu właściwości nawigacji w modelu załadować powiązanych jednostek. Istnieją trzy typowe wzorce O/RM używana do załadowania powiązanych danych.
* **Ładowanie wczesny** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.
* **Ładowanie jawne** oznacza, że powiązane jest jawnie załadować danych z bazy danych w późniejszym czasie.
* **Powolne ładowanie** oznacza, że pokrewne niewidocznie załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="eager-loading"></a>Ładowanie wczesny

Można użyć `Include` metodę, aby określić powiązane dane mają zostać uwzględnione w wynikach zapytania. W poniższym przykładzie, blogów, z którego są zwracane w wynikach będzie miał ich `Posts` właściwości wypełniane przy użyciu powiązanych wpisów.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core będzie automatycznie konfigurowania właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu. Dlatego nawet wtedy, gdy wyraźnie nie zawierają danych dla właściwości nawigacji, właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.


Powiązane dane z wielu relacji można uwzględnić w jednym zapytaniu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>W tym wiele poziomów

Można przejść do relacji z obejmują wiele poziomów powiązanych danych przy użyciu `ThenInclude` metody. Poniższy przykład ładuje wszystkie blogi, ich powiązane ogłoszeń i autor każdego post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Bieżącej wersji programu Visual Studio oferują niepoprawny kod zakończenia opcje i może spowodować poprawne wyrażenia do błędy składniowe przy użyciu `ThenInclude` metody po właściwości nawigacji kolekcji. To jest symptomem błędów IntelliSense w https://github.com/dotnet/roslyn/issues/8237. Jest bezpiecznie zignorować te błędy składniowe fałszywe tak długo, jak kod jest poprawny i może zostać pomyślnie skompilowany. 

Tworzenia łańcucha wielu wywołań `ThenInclude` aby kontynuować, tym więcej poziomy powiązanych danych.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Możesz łączyć wszystko to zostać uwzględnione w jednym zapytaniu powiązane dane z różnych poziomach i wielu elementów głównych.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Można dołączyć wielu powiązanych jednostek dla jednego z jednostkami, które ma być. Na przykład podczas wykonywania zapytania `Blog`s, obejmują `Posts` , a następnie chcesz dołączyć zarówno `Author` i `Tags` z `Posts`. Aby to zrobić, należy określić każdy obejmować ścieżki od katalogu głównego. Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`. Oznacza to, że otrzymasz nadmiarowe sprzężeń, w większości przypadków będzie skonsolidować EF sprzężeń podczas generowania SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Obejmują na typy pochodne

Powiązane dane z tego zdefiniowany dla typu pochodnego przy użyciu mogą obejmować `Include` i `ThenInclude`. 

Podane następującego modelu:

```Csharp
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

Zawartość `School` nawigacji wszystkich osób, które są studentów dzielenia na załadowaniem przy użyciu wielu wzorców:

- Używanie cast
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- przy użyciu `as` — operator
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- za pomocą przeciążenia `Include` pobierającej parametr typu `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Ignorowane obejmuje

Jeśli zmienisz zapytanie tak, aby nie będzie już zwracać wystąpień typów jednostek, które rozpoczął się zapytanie, operatorów include są ignorowane.

W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator służy do zmiany zwracanego typu anonimowego przez zapytanie. W takim przypadku operatory include nie obowiązują.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Domyślnie EF Core będzie rejestrować ostrzeżenie podczas obejmują operatory są ignorowane. Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania. Można zmienić to zachowanie, gdy operator include jest ignorowana, aby zgłosić lub nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Ładowanie jawne

> [!NOTE]  
> Ta funkcja została wprowadzona w EF Core 1.1.

Można w sposób jawny załadować właściwości nawigacji za pośrednictwem `DbContext.Entry(...)` interfejsu API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Można również jawnie załadować właściwość nawigacji, wykonując oddzielne zapytania, które zwraca powiązanych jednostek. Jeśli jest włączone śledzenie zmian, podczas ładowania jednostki, EF Core zostanie automatycznie Ustaw właściwości nawigacji entitiy nowo załadowanych do odwoływania się do żadnych jednostek już załadowany, a następnie ustaw właściwości nawigacji jednostki już załadowany do odwoływania się do nowo załadowanych jednostki.

### <a name="querying-related-entities"></a>Wykonywanie zapytania powiązanych jednostek

Można również uzyskać kwerendy LINQ, która reprezentuje zawartość właściwości nawigacji.

Pozwala na wykonywanie czynności, takie jak uruchomienie operatora agregacji za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Można również filtrować, które powiązanych jednostek są ładowane do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>powolne ładowanie

> [!NOTE]  
> Ta funkcja została wprowadzona w programie EF Core 2.1.

Najprostszym sposobem użycia opóźnionego ładowania jest zainstalowanie [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu i włączenie go w wyniku wywołania `UseLazyLoadingProxies`. Na przykład:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Lub w przypadku używania AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
Podstawowe EF następnie umożliwi opóźnionego ładowania dla dowolnego właściwość nawigacji, która może zostać zastąpiona — która jest, musi ona być `virtual` i klasy, które mogą być dziedziczone z. Na przykład w następujących podmiotów `Post.Blog` i `Blog.Posts` właściwości nawigacji zostanie załadowany opóźnieniem.
```Csharp
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
### <a name="lazy-loading-without-proxies"></a>Lazy — ładowania, bez serwerów proxy

Ładowanie opóźnieniem proxy pracy przez wstrzykiwanie `ILazyLoader` usługi do jednostki, zgodnie z opisem w [konstruktorów typów jednostek](../modeling/constructors.md). Na przykład:
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
To nie wymaga typy jednostek, które były dziedziczone z i właściwości nawigacji, aby być wirtualna i umożliwia utworzony z wystąpień jednostek `new` do opóźnionego ładowania raz dołączona do kontekstu. Wymaga to jednak odwołanie do `ILazyLoader` usługi, składającą się do zestawu EF podstawowych typów jednostek. Aby uniknąć tego Core EF umożliwia `ILazyLoader.Load` metodę, aby zostać dodane jako pełnomocnik. Na przykład:
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Kod nad używa `Load` — metoda rozszerzenia, aby upewnić się, za pomocą obiektu delegowanego nieco czyszcząca:
```Csharp
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
> Parametr konstruktora delegata opóźnionego ładowania musi być wywoływana "lazyLoader". Konfigurację, aby użyć innej nazwy, którego planowana jest w przyszłości.

## <a name="related-data-and-serialization"></a>Powiązane dane i serializacja

Ponieważ właściwości nawigacji automatycznie konfigurowania będzie EF Core, można na końcu cykli na wykresie obiektu. Na przykład ładowanie blogu i powiązanych spowoduje wpisów w obiekcie blog, który odwołuje się do kolekcji wpisów. Każdy z tych stanowisk ma odwołanie do blogu.

Niektórych struktur serializacji nie zezwalają na takie cykle. Na przykład struktury Json.NET zgłosi wyjątek po napotkaniu cyklu.

> Newtonsoft.Json.JsonSerializationException: Self odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".

Jeśli używasz platformy ASP.NET Core, można skonfigurować struktury Json.NET do ignorowania cykle znalezionych w grafie obiektu. Jest to wykonywane w `ConfigureServices(...)` metoda `Startup.cs`.

``` csharp
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
