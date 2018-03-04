---
title: "Typy jednostek z konstruktorów - EF Core"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 38ab0c1c3cd8c490875abf30b8478c99bc58630f
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="8f5c7-102">Typy jednostek z konstruktorów</span><span class="sxs-lookup"><span data-stu-id="8f5c7-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="8f5c7-103">Ta funkcja jest nowa w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="8f5c7-104">W programie EF Core 2.1, obecnie istnieje możliwość zdefiniować konstruktora z parametrami, a Core EF wywołać konstruktora, podczas tworzenia wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="8f5c7-105">Parametry Konstruktor może być powiązana z mapowanej właściwości lub do różnych rodzajów usług w celu ułatwienia zachowań, takich jak opóźnionego ładowania.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="8f5c7-106">Począwszy od EF Core 2.1 wszystkie powiązania konstruktora jest przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="8f5c7-107">Planowany konfiguracji określonych konstruktorów do użycia w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="8f5c7-108">Powiązanie do mapowanej właściwości</span><span class="sxs-lookup"><span data-stu-id="8f5c7-108">Binding to mapped properties</span></span>

<span data-ttu-id="8f5c7-109">Należy wziąć pod uwagę typowe modelu/wpis w blogu:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-109">Consider a typical Blog/Post model:</span></span>

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="8f5c7-110">Gdy EF Core tworzy wystąpienia typu, takich jak dla wyników zapytania, utworzy pierwsze wywołanie konstruktora domyślnego i następnie ustaw wartość każdej właściwości z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="8f5c7-111">Jednak jeśli EF Core znajdzie sparametryzowanym konstruktorze z nazwy parametru i typów, które pasują do właściwości zamapować właściwości, a następnie zamiast tego zostanie wywołanie konstruktora sparametryzowane wartości tych właściwości, a nie jawnie ustawia każdej właściwości.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="8f5c7-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-112">For example:</span></span>

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="8f5c7-113">Niektóre rzeczy do uwzględnienia:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-113">Some things to note:</span></span>
* <span data-ttu-id="8f5c7-114">Nie wszystkie właściwości musi być parametrami konstruktora.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="8f5c7-115">Na przykład właściwość Post.Content nie jest ustawiany przez żadnego parametru konstruktora, więc EF Core zostanie ustawiony po wywołaniu konstruktora w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="8f5c7-116">Nazwy i typy parametrów muszą być zgodne typy właściwości i nazwy, z wyjątkiem tego, że właściwości mogą być Pascal — z uwzględnieniem wielkości liter podczas parametry są pisane formatu.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="8f5c7-117">Podstawowe EF nie można ustawić właściwości nawigacji (na przykład blogu lub ogłoszeń powyżej) przy użyciu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="8f5c7-118">Konstruktor może być publiczne, prywatne, lub inne ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="8f5c7-119">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="8f5c7-119">Read-only properties</span></span>

<span data-ttu-id="8f5c7-120">Po właściwości są ustawiane za pośrednictwem konstruktora go warto utworzyć niektóre z nich tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="8f5c7-121">Obsługuje tę funkcję EF Core, ale jest kilka rzeczy, zaczekaj na:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="8f5c7-122">Właściwości bez metody ustawiające nie są mapowane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="8f5c7-123">(To zwykle mapowania właściwości, które nie powinny być mapowane, takie jak właściwości obliczanej.)</span><span class="sxs-lookup"><span data-stu-id="8f5c7-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="8f5c7-124">Za pomocą wartości kluczy automatycznie generowanych wymaga właściwości klucza, która jest odczytu i zapisu, ponieważ wartości klucza musi być ustawiona przez generator kluczy, podczas wstawiania nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="8f5c7-125">Prosty sposób, aby uniknąć tych czynności jest Użyj prywatnego setters.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="8f5c7-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-126">For example:</span></span>
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}

```
<span data-ttu-id="8f5c7-127">Podstawowe EF widzi właściwości prywatnej setter jako odczytu i zapisu, co oznacza, że wszystkie właściwości są mapowane jak poprzednio i klucz może nadal być generowane przez Magazyn.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="8f5c7-128">Jest to alternatywa dla użycia prywatnej metody ustawiające właściwości naprawdę tylko do odczytu i Dodaj mapowanie dokładniejsze w OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="8f5c7-129">Podobnie niektóre właściwości można całkowicie usunięty i zastąpiony tylko pola.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="8f5c7-130">Na przykład wziąć pod uwagę następujące typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-130">For example, consider these entity types:</span></span>

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="8f5c7-131">I tą konfiguracją w OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-131">And this configuration in OnModelCreating:</span></span>
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
<span data-ttu-id="8f5c7-132">Rzeczy do uwzględnienia:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-132">Things to note:</span></span>
* <span data-ttu-id="8f5c7-133">Klucz "property" jest polem.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-133">The key "property" is now a field.</span></span> <span data-ttu-id="8f5c7-134">Nie jest `readonly` pola tak, aby mogły być używane klucze generowane przez Magazyn.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="8f5c7-135">Inne właściwości są właściwości tylko do odczytu ustawiana tylko w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="8f5c7-136">Jeśli wartość klucza podstawowego tylko określone przez EF lub odczytu z bazy danych, oznacza to, że nie trzeba wpisywać go w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="8f5c7-137">Pozostawia jako proste pole klucza "property" i ułatwia wyczyścić go nie należy ustawiać jawnie podczas tworzenia nowego blogi lub wpisów.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="8f5c7-138">Ten kod spowoduje kompilatora ostrzeżenie "169" wskazujący, że pole jest nigdy używane.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="8f5c7-139">Ponieważ w rzeczywistości EF Core używa pola w sposób extralinguistic można zignorować to.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="8f5c7-140">Wstrzyknięcie usług</span><span class="sxs-lookup"><span data-stu-id="8f5c7-140">Injecting services</span></span>

<span data-ttu-id="8f5c7-141">EF Core można również wprowadzić "usługi" do konstruktora typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="8f5c7-142">Na przykład następujące czynności mogą zostać dodane:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="8f5c7-143">`DbContext` -bieżącego wystąpienia kontekstu, która może też wpisać jako typ pochodny typu DbContext</span><span class="sxs-lookup"><span data-stu-id="8f5c7-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="8f5c7-144">`ILazyLoader` -Usługa opóźnionego ładowania — zobacz [dokumentacji opóźnionego ładowania](../querying/related-data.md) więcej szczegółów</span><span class="sxs-lookup"><span data-stu-id="8f5c7-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="8f5c7-145">`Action<object, string>` -Delegowanie powolne ładowanie — zobacz [dokumentacji opóźnionego ładowania](../querying/related-data.md) więcej szczegółów</span><span class="sxs-lookup"><span data-stu-id="8f5c7-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="8f5c7-146">`IEntityType` -EF Core metadane skojarzone z tym typem jednostki</span><span class="sxs-lookup"><span data-stu-id="8f5c7-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="8f5c7-147">Począwszy od EF Core 2.1 mogą zostać dodane tylko usługi znane EF Core.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="8f5c7-148">Obsługa wstrzykiwania usługi aplikacji jest rozważane w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="8f5c7-149">Na przykład można wprowadzony DbContext selektywne uzyskiwanie dostępu do bazy danych, aby uzyskać informacje o powiązanych jednostek bez ładowania je wszystkie.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="8f5c7-150">W poniższym przykładzie służy do uzyskania liczby wpisów w blogu bez ładowania wpisów:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="8f5c7-151">Kilka istotnych Zwróć uwagę na ten temat:</span><span class="sxs-lookup"><span data-stu-id="8f5c7-151">A few things to notice about this:</span></span>
* <span data-ttu-id="8f5c7-152">Konstruktor jest prywatny, ponieważ jest on tylko kiedykolwiek wywołania przez rdzeń EF, i innego konstruktora publicznego do użytku ogólnego.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="8f5c7-153">Kod przy użyciu usługi wprowadzony (tj. kontekst) jest obrony przed jego trwa `null` do obsługi przypadków gdzie EF Core nie jest tworzone wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="8f5c7-154">Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu zostaną zresetowane, gdy obiekt jest dołączony do nowego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="8f5c7-155">Wstrzyknięcie DbContext, jak to często uważa się za wzorca oprogramowania od momentu jego couples Twojego typów jednostek bezpośrednio do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="8f5c7-156">Należy rozważyć wszystkie opcje przed przy użyciu iniekcji usługi następująco.</span><span class="sxs-lookup"><span data-stu-id="8f5c7-156">Carefully consider all options before using service injection like this.</span></span>
