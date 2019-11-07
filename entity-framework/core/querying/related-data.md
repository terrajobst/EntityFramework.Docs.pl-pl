---
title: Ładowanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: bfabe8fd5b0a64edd5d97baff3beab9d712f1c20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654632"
---
# <a name="loading-related-data"></a>Ładowanie powiązanych danych

Entity Framework Core umożliwia używanie właściwości nawigacji w modelu do ładowania powiązanych jednostek. Istnieją trzy typowe wzorce O/RM używane do ładowania powiązanych danych.

* **Ładowanie eager** oznacza, że powiązane dane są ładowane z bazy danych w ramach wstępnego zapytania.
* **Jawne ładowanie** oznacza, że powiązane dane są jawnie ładowane z bazy danych w późniejszym czasie.
* **Ładowanie z opóźnieniem** oznacza, że powiązane dane są w sposób niewidoczny do załadowania z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="eager-loading"></a>Ładowanie eager

Za pomocą metody `Include` można określić powiązane dane, które mają być uwzględnione w wynikach zapytania. W poniższym przykładzie Blogi, które są zwracane w wynikach, będą miały Właściwość `Posts` wypełniane odpowiednimi wpisami.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core automatycznie naprawia właściwości nawigacji do wszystkich innych jednostek, które zostały wcześniej załadowane do wystąpienia kontekstu. Dlatego nawet jeśli nie dołączysz jawnie danych dla właściwości nawigacji, właściwość może nadal zostać wypełniona, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.

Można uwzględnić powiązane dane z wielu relacji w pojedynczym zapytaniu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Uwzględnienie wielu poziomów

Możesz przejść do szczegółów relacji, aby uwzględnić wiele poziomów powiązanych danych przy użyciu metody `ThenInclude`. Poniższy przykład ładuje wszystkie blogi, ich powiązane wpisy i autora każdego wpisu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Można połączyć wiele wywołań `ThenInclude`, aby kontynuować, w tym dalsze poziomy pokrewnych danych.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Możesz połączyć wszystkie te elementy, aby uwzględnić powiązane dane z wielu poziomów i wiele elementów głównych w tym samym zapytaniu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Możesz chcieć dołączyć wiele powiązanych jednostek dla jednej z dołączanych jednostek. Na przykład podczas wykonywania zapytania dotyczącego `Blogs`należy uwzględnić `Posts`, a następnie dodać zarówno `Author`, jak i `Tags` `Posts`. W tym celu należy określić wszystkie ścieżki dołączania, zaczynając od elementu głównego. Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`. Nie oznacza to, że nastąpi nadmiarowe sprzężenia; w większości przypadków program Dr konsoliduje sprzężenia podczas generowania bazy danych SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Ponieważ wersja 3.0.0, każda `Include` spowoduje dodanie dodatkowego SPRZĘŻENIa do zapytań SQL generowanych przez dostawców relacyjnych, podczas gdy poprzednie wersje wygenerowały dodatkowe zapytania SQL. Może to znacząco zmienić wydajność zapytań, aby lepiej lub gorszyć. W szczególności zapytania LINQ o przekroczeniu dużej liczbie operatorów `Include` mogą wymagać podzielenia na wiele oddzielnych zapytań LINQ w celu uniknięcia problemu z wybuchem kartezjańskiego.

### <a name="include-on-derived-types"></a>Uwzględnij w typach pochodnych

Można dołączyć powiązane dane z nawigacji zdefiniowanych tylko dla typu pochodnego przy użyciu `Include` i `ThenInclude`.

Uwzględniając następujący model:

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

Zawartość `School` nawigacji dla wszystkich osób, które są uczniami, może być eagerly załadowana przy użyciu wielu wzorców:

* Używanie rzutowania

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* Używanie operatora `as`

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* Używanie przeciążenia `Include`, które pobiera parametr typu `string`

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Jawne ładowanie

Można jawnie załadować właściwość nawigacji za pośrednictwem interfejsu API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Możesz również jawnie załadować właściwość nawigacji przez wykonanie oddzielnego zapytania, które zwraca powiązane jednostki. Jeśli funkcja śledzenia zmian jest włączona, podczas ładowania jednostki EF Core automatycznie ustawił właściwości nawigacji nowo załadowanego jednostkę, aby odwołać się do wszystkich jednostek, które zostały już załadowane, i ustawić właściwości nawigacji już załadowanych jednostek, aby odwołać się do nowo załadowana jednostka.

### <a name="querying-related-entities"></a>Wykonywanie zapytania dotyczącego powiązanych jednostek

Możesz również uzyskać zapytanie LINQ, które reprezentuje zawartość właściwości nawigacji.

Umożliwia to wykonywanie takich czynności, jak uruchamianie agregacji operatora na powiązanych jednostkach bez ładowania ich do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Można również filtrować powiązane jednostki, które są ładowane do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

Najprostszym sposobem użycia ładowania z opóźnieniem jest zainstalowanie pakietu [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) i włączenie go przy użyciu wywołania do `UseLazyLoadingProxies`. Na przykład:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

Lub w przypadku korzystania z AddDbContext:

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

EF Core następnie Włącz ładowanie z opóźnieniem dla każdej właściwości nawigacji, która może zostać przesłonięta — to oznacza, że musi być `virtual` i na klasie, z której można dziedziczyć. Na przykład w poniższych jednostkach właściwości nawigacji `Post.Blog` i `Blog.Posts` zostaną załadowane z opóźnieniem.

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

### <a name="lazy-loading-without-proxies"></a>Ładowanie z opóźnieniem bez serwerów proxy

Ładowanie serwerów proxy z opóźnieniem, które działają przez wstrzyknięcie usługi `ILazyLoader` do jednostki, zgodnie z opisem w [konstruktorach typu jednostki](../modeling/constructors.md). Na przykład:

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

Nie wymaga to dziedziczenia typów jednostek ani właściwości nawigacji, które mają być wirtualne, i umożliwia wystąpienia jednostek utworzone przy użyciu `new` do ładowania z opóźnieniem po dołączeniu do kontekstu. Wymaga to jednak odwołania do usługi `ILazyLoader`, która jest zdefiniowana w pakiecie [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) . Ten pakiet zawiera minimalny zestaw typów, dzięki czemu w zależności od tego ma być bardzo niewielki wpływ. Jednak aby całkowicie uniknąć w zależności od dowolnego EF Core pakietów w typach jednostek, można wstrzyknąć metodę `ILazyLoader.Load` jako delegata. Na przykład:

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

W powyższym kodzie użyto metody rozszerzenia `Load`, aby użyć delegata:

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
> Parametr konstruktora dla delegata ładowania z opóźnieniem musi mieć nazwę "lazyLoader". Konfiguracja służąca do używania innej nazwy niż jest planowana dla przyszłej wersji.

## <a name="related-data-and-serialization"></a>Powiązane dane i Serializacja

Ponieważ EF Core automatycznie naprawia właściwości nawigacji, można zakończyć z cyklami na grafie obiektów. Na przykład załadowanie blogu i jego powiązanych wpisów spowoduje powstanie obiektu blogu, który odwołuje się do kolekcji wpisów. Każdy z tych wpisów będzie miał odwołanie z powrotem do blogu.

Niektóre platformy serializacji nie zezwalają na takie cykle. Na przykład Json.NET zgłosi następujący wyjątek w przypadku napotkania cyklu.

> Newtonsoft. JSON. JsonSerializationException: wykryto pętlę odwołującą dla właściwości "blog" z typem "WebApplication. MODELES. blog".

Jeśli używasz ASP.NET Core, możesz skonfigurować Json.NET do ignorowania cykli znalezionych w grafie obiektów. Jest to realizowane za pomocą metody `ConfigureServices(...)` w `Startup.cs`.

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

Kolejną alternatywą jest dekorować jednej z właściwości nawigacji z atrybutem `[JsonIgnore]`, który instruuje Json.NET, aby nie przechodzą tej właściwości nawigacji podczas serializacji.
