---
title: Typy jednostek z konstruktorów — EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 5bf49718f02c1860871b1f4c255ec4d98fce2fc7
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405248"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="b3a1b-102">Typy jednostek za pomocą konstruktorów</span><span class="sxs-lookup"><span data-stu-id="b3a1b-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="b3a1b-103">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="b3a1b-104">Począwszy od programu EF Core 2.1 jest teraz można zdefiniować konstruktora z parametrami, a programem EF Core wywołania tego konstruktora, podczas tworzenia wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="b3a1b-105">Parametry Konstruktora może być powiązana z właściwości zamapowanych lub do różnych rodzajów usług w celu ułatwienia zachowania, takich jak ładowanych z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="b3a1b-106">Począwszy od programu EF Core 2.1 wszystkie powiązania Konstruktor jest przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="b3a1b-107">Konfiguracja określonego konstruktora do użycia jest planowana w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="b3a1b-108">Powiązywanie właściwości zamapowanych</span><span class="sxs-lookup"><span data-stu-id="b3a1b-108">Binding to mapped properties</span></span>

<span data-ttu-id="b3a1b-109">Należy rozważyć typowy modelu/wpis w blogu:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-109">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="b3a1b-110">Jeśli programu EF Core tworzy wystąpienia tych typów, takich jak wyniki zapytania, jest będzie pierwsze wywołanie domyślnego konstruktora bez parametrów i następnie ustaw wartość każdej właściwości z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="b3a1b-111">Jednak jeśli programu EF Core znajdzie sparametryzowania konstruktora przy użyciu nazwy i typy, które pasują do właściwości parametrów mapowane właściwości, a następnie zamiast tego będzie wywoływać sparametryzowanego konstruktora o wartości dla tych właściwości, a nie ustawi każdej właściwości jawnie.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="b3a1b-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-112">For example:</span></span>

``` csharp
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
<span data-ttu-id="b3a1b-113">Niektóre kwestie, należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-113">Some things to note:</span></span>
* <span data-ttu-id="b3a1b-114">Nie wszystkie właściwości konieczne parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="b3a1b-115">Na przykład właściwość Post.Content nie jest ustawiany przez żadnych parametrów konstruktora, więc programu EF Core zostanie ustawiony po wywołaniu konstruktora w normalny sposób.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="b3a1b-116">Nazwy i typy parametrów muszą być zgodne typy właściwości i nazw, z tą różnicą, że właściwości mogą być Pascal — z uwzględnieniem wielkości liter podczas, gdy parametry są w formacie camelcase.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="b3a1b-117">EF Core nie można ustawić właściwości nawigacji (na przykład blogu lub wpisy powyżej) przy użyciu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="b3a1b-118">Konstruktor może być publiczne, prywatne, lub innych ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="b3a1b-119">Powolne ładowanie serwery proxy wymagają jednak, czy Konstruktor jest dostępna w klasie dziedziczącej serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="b3a1b-120">Zwykle oznacza to, dzięki czemu publiczne lub chronione.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="b3a1b-121">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="b3a1b-121">Read-only properties</span></span>

<span data-ttu-id="b3a1b-122">Gdy właściwości są ustawiane za pośrednictwem konstruktora sensowne może się niektóre z nich w trybie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="b3a1b-123">Obsługuje tę funkcję programu EF Core, ale jest kilka rzeczy, zwróć uwagę na:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-123">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="b3a1b-124">Właściwości bez metod ustawiających nie są zamapowane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="b3a1b-125">(To zwykle właściwości, które nie powinny być mapowane, takich jak obliczone właściwości mapy.)</span><span class="sxs-lookup"><span data-stu-id="b3a1b-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="b3a1b-126">Przy użyciu automatycznie generowanego klucza wartości wymaga właściwości klucza, która jest odczytu i zapisu, ponieważ wartość klucza musi być ustawiona przez generator kluczy, podczas wstawiania nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="b3a1b-127">Prosty sposób, aby uniknąć tych rzeczy jest używać prywatnych metod ustawiających.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="b3a1b-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-128">For example:</span></span>
``` csharp
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
<span data-ttu-id="b3a1b-129">EF Core uznaje prywatnej setter właściwości odczytu / zapisu, co oznacza, że wszystkie właściwości są mapowane tak jak poprzednio, i klawisz mogą nadal być generowane przez Magazyn.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="b3a1b-130">Alternatywa dla użycia prywatnych metod ustawiających jest właściwości tak naprawdę tylko do odczytu i Dodaj mapowanie dokładniejsze w OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="b3a1b-131">Podobnie niektóre właściwości mogą całkowicie usunięty i zastąpiony tylko pola.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="b3a1b-132">Na przykład należy wziąć pod uwagę te typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-132">For example, consider these entity types:</span></span>

``` csharp
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
<span data-ttu-id="b3a1b-133">I tej konfiguracji w OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-133">And this configuration in OnModelCreating:</span></span>
``` csharp
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
<span data-ttu-id="b3a1b-134">Kwestii, które należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-134">Things to note:</span></span>
* <span data-ttu-id="b3a1b-135">Klucz "property" jest polem.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-135">The key "property" is now a field.</span></span> <span data-ttu-id="b3a1b-136">Nie jest `readonly` tak, aby klucze generowane przez Magazyn mogą być używane.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="b3a1b-137">Inne właściwości są właściwości tylko do odczytu ustawiana tylko w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="b3a1b-138">Jeśli wartość klucza podstawowego tylko nigdy nie jest ustawiony przez EF lub odczytu z bazy danych, następnie nie ma potrzeby umieszczenie go w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="b3a1b-139">Pozostawia klucz "property" jako prostym polem i sprawia, że jasne, że jej nie powinna być ustawiona jawnie podczas tworzenia nowego blogów i wpisów.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="b3a1b-140">Ten kod będzie skutkować kompilatora ostrzeżenie "169" wskazujący, że pole nigdy nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="b3a1b-141">Może być został zignorowany, ponieważ w rzeczywistości programu EF Core jest za pomocą pola w sposób extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="b3a1b-142">Wprowadzanie usługi</span><span class="sxs-lookup"><span data-stu-id="b3a1b-142">Injecting services</span></span>

<span data-ttu-id="b3a1b-143">EF Core mogą umożliwić również iniekcję "usługi" do konstruktora typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="b3a1b-144">Na przykład że może wprowadzone:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-144">For example, the following can be injected:</span></span>
* <span data-ttu-id="b3a1b-145">`DbContext` -wystąpienie kontekstu bieżącego można także wpisać jako pochodny typu DbContext</span><span class="sxs-lookup"><span data-stu-id="b3a1b-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="b3a1b-146">`ILazyLoader` -Usługa ładowanych z opóźnieniem — zobacz [dokumentacji powolne ładowanie](../querying/related-data.md) Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="b3a1b-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="b3a1b-147">`Action<object, string>` — obiekt delegowany ładowanych z opóźnieniem — zobacz [dokumentacji powolne ładowanie](../querying/related-data.md) Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="b3a1b-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="b3a1b-148">`IEntityType` -metadanych programu EF Core skojarzonych z tym typem jednostki</span><span class="sxs-lookup"><span data-stu-id="b3a1b-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="b3a1b-149">Począwszy od programu EF Core 2.1 może zostać wprowadzona tylko usługi znane przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="b3a1b-150">Obsługa wprowadzanie usługi aplikacji jest rozważane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="b3a1b-151">Na przykład można wprowadzonego typu DbContext na selektywny dostęp do bazy danych, aby uzyskać informacje o powiązanych jednostek bez ładowania ich wszystkich.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="b3a1b-152">W poniższym przykładzie służy do uzyskania liczby wpisów w blogu bez ładowania wpisy:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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
<span data-ttu-id="b3a1b-153">Kilka istotnych kwestii na ten temat:</span><span class="sxs-lookup"><span data-stu-id="b3a1b-153">A few things to notice about this:</span></span>
* <span data-ttu-id="b3a1b-154">Konstruktor jest prywatny, ponieważ tylko nigdy nie jest wywoływana przez platformę EF Core i ma innego konstruktora publicznego do użytku ogólnego.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="b3a1b-155">Kod przy użyciu usługi wprowadzonego (kontekstu) jest obrony przed nim są `null` sposób obsługiwać przypadki, gdzie programu EF Core nie jest tworzone wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="b3a1b-156">Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu zostaną zresetowane, gdy jednostka jest dołączony do nowego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="b3a1b-157">Wprowadzanie DbContext następująco jest często uznawana za zapobieganie wzorca od czasu jej couples typów jednostek bezpośrednio do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="b3a1b-158">Zastanów się uważnie wszystkie opcje przed przy użyciu iniekcji usługi w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="b3a1b-158">Carefully consider all options before using service injection like this.</span></span>
