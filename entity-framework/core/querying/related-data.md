---
title: Trwa ładowanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 65cfea07a40939c1c3615c97ec785a4082b21de5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994791"
---
# <a name="loading-related-data"></a>Ładowanie powiązanych danych

Entity Framework Core umożliwia ładowanie powiązanych jednostek przy użyciu właściwości nawigacji w modelu. Istnieją trzy powszechnie używane wzorce Obiektowo używana do ładowania powiązane dane.
* **Wczesne ładowanie** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.
* **Jawne ładowanie** oznacza, że powiązanych danych jest jawnie załadowane z bazy danych w późniejszym czasie.
* **Powolne ładowanie** oznacza, że powiązane przezroczyste załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="eager-loading"></a>Wczesne ładowanie

Możesz użyć `Include` metodę, aby określić powiązanych danych, które mają zostać uwzględnione w wynikach kwerendy. W poniższym przykładzie, blogów, które są zwracane w wynikach będzie mieć ich `Posts` właściwość wypełniony pokrewnych wpisów.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core będą automatycznie konfigurowania właściwości nawigacji do innych jednostek, które wcześniej zostały załadowane do wystąpienia kontekstu. Dlatego nawet wtedy, gdy jawnie nie zawiera danych dla właściwości nawigacji, nadal można wypełnić właściwość, jeśli niektóre lub wszystkie powiązane jednostki zostały wcześniej załadowane.


Może zawierać dane związane z wieloma relacjami w ramach pojedynczego zapytania.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>W tym wiele poziomów

Możesz przejść do szczegółów w relacji do uwzględnienia na wielu poziomach powiązanych danych przy użyciu `ThenInclude` metody. Poniższy przykład ładuje wszystkie blogi, ich pokrewnych wpisów i autor każdego wpisu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Bieżące wersje programu Visual Studio oferuje opcje uzupełniania niepoprawny kod i może spowodować, że poprawne wyrażenia do oflagowania z błędami składni, korzystając z `ThenInclude` metody właściwości nawigacji kolekcji. Jest to objaw błędów funkcji IntelliSense w https://github.com/dotnet/roslyn/issues/8237. Jest bezpiecznie zignorować te błędy składniowe fałszywe, tak długo, jak kod jest poprawny i mogą być pomyślnie skompilowane. 

Można połączyć w łańcuch wielu wywołań `ThenInclude` aby kontynuować, włączając dalsze poziomy powiązane dane.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Możesz połączyć wszystkie te które mają zostać objęte powiązane dane z wielu poziomów i wielu katalogów głównych tego samego zapytania.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Możesz uwzględnić wiele powiązanych jednostek dla jednej jednostki, które jest uwzględniane. Na przykład podczas wykonywania zapytań dotyczących `Blog`s, obejmują `Posts` , a następnie oba `Author` i `Tags` z `Posts`. Aby to zrobić, należy określić każdy obejmować ścieżkę, począwszy od głównego. Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`. Oznacza to, zostanie nadmiarowe sprzężenia, w większości przypadków będzie konsolidować EF sprzężeń podczas generowania SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Obejmują na typy pochodne

Może zawierać dane związane z tego zdefiniowany tylko w typie pochodnym przy użyciu `Include` i `ThenInclude`. 

Biorąc pod uwagę następujący wzór:

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

Zawartość `School` nawigacji wszystkie osoby, których uczniowie mogą być eagerly ładowane, przy użyciu wielu wzorców:

- Używanie cast
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- za pomocą `as` — operator
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- za pomocą przeciążenia `Include` przyjmującą parametr typu `string`
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Ignorowane obejmuje

Jeśli zmienisz zapytanie tak, aby nie zwraca wystąpienia typu jednostki, który rozpoczął się zapytania, operatory dołączania są ignorowane.

W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator jest używany do zmiany zwracanego typu anonimowego przez zapytanie. W tym przypadku operatory include nie mają wpływu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Domyślnie EF Core zarejestruje ostrzeżenie obejmują gdy operatory są ignorowane. Zobacz [rejestrowania](../miscellaneous/logging.md) więcej informacji na temat wyświetlania danych wyjściowych rejestrowania. Można zmienić zachowanie, gdy include operator jest ignorowana, throw lub nic nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` korzystania z platformy ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>jawne ładowanie

> [!NOTE]  
> Ta funkcja została wprowadzona w programie EF Core 1.1.

Można jawnie załadować właściwości nawigacji za pomocą `DbContext.Entry(...)` interfejsu API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Właściwość nawigacji można także jawnie załadować, wykonując oddzielne zapytania, które zwraca powiązanych jednostek. Jeśli jest włączone śledzenie zmian, a następnie podczas ładowania jednostki, EF Core będą automatycznie ustawić właściwości nawigacji między jednostkami nowo załadowane do odwoływania się do żadnych jednostek już załadowany i ustawić właściwości nawigacji jednostki już załadowane do odwoływania się do Jednostka nowo załadowane.

### <a name="querying-related-entities"></a>Tworzenie zapytań powiązanych jednostek

Można również uzyskać kwerenda LINQ, która reprezentuje zawartość właściwości nawigacji.

Dzięki temu można korzystać z możliwości, takie jak uruchomienie aggregate-operator za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Można również filtrować, które powiązanych jednostek są ładowane do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

> [!NOTE]  
> Ta funkcja została wprowadzona w programie EF Core 2.1.

Najprostszym sposobem użycia ładowanych z opóźnieniem jest po zainstalowaniu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu i włączanie go z wywołaniem `UseLazyLoadingProxies`. Na przykład:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Lub w przypadku używania AddDbContext:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core będzie z opóźnieniem, ładowania dla dowolnej właściwości nawigacji, która może zostać przesłonięta — oznacza to, Włącz musi być `virtual` i klasy, które mogą być dziedziczone z. Na przykład w następujących elementach `Post.Blog` i `Blog.Posts` właściwości nawigacji będą ładowane z opóźnieniem.
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
### <a name="lazy-loading-without-proxies"></a>Powolne ładowanie bez serwerów proxy

Serwery proxy ładowanych z opóźnieniem pracy przez iniekcję `ILazyLoader` usługi do jednostki, zgodnie z opisem w [Konstruktorzy typów jednostek](../modeling/constructors.md). Na przykład:
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
To nie wymaga typów jednostek, dziedziczenie lub właściwości nawigacji, aby być wirtualne oraz umożliwia wystąpień jednostek utworzonych za pomocą `new` z opóźnieniem obciążenia raz przyłączonych do kontekstu. Jednak wymaga to dodania odwołania do `ILazyLoader` usługa, która jest zdefiniowana w [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) pakietu. Ten pakiet zawiera minimalny zestaw typów, dzięki czemu istnieje niewielki wpływ w zależności od jego. Jednak aby całkowicie uniknąć zależności od tego, wszystkie pakiety programu EF Core w typów jednostek, istnieje możliwość wstawić `ILazyLoader.Load` metody jako pełnomocnik. Na przykład:
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
Kod powyżej używa `Load` metodę rozszerzenia, aby wprowadzić nieco oczyszczarkę plików przy użyciu delegata:
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
> Parametr konstruktora delegata ładowanych z opóźnieniem należy wywołać "lazyLoader". Konfigurację, aby użyć innej nazwy, niż jest to planowana w przyszłej wersji.

## <a name="related-data-and-serialization"></a>Powiązane dane i serializacja

Ponieważ właściwości nawigacji automatycznie poprawki będą programu EF Core, można na końcu cykli w grafie obiektu. Na przykład blog i jej powiązane wpisy ładowania spowoduje obiekt blogu, który odwołuje się zbiór wpisów. Każda z tych wpisów mają odwołaniem zwrotnym do blogu.

Niektóre środowiska serializacji nie zezwalają na tych cykli. Na przykład na składnik Json.NET zgłosi następujący wyjątek, jeśli okaże się cyklu.

> Newtonsoft.Json.JsonSerializationException: Self odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".

Jeśli używasz platformy ASP.NET Core, można skonfigurować program Json.NET, aby zignorować cykle, które znajdzie się na grafie obiektu. Jest to realizowane w `ConfigureServices(...)` method in Class metoda `Startup.cs`.

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
