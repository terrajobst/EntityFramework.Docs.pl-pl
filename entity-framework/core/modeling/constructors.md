---
title: Typy jednostek z konstruktorami — EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811896"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="61727-102">Typy jednostek z konstruktorami</span><span class="sxs-lookup"><span data-stu-id="61727-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="61727-103">Ta funkcja jest nowa w EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="61727-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="61727-104">Począwszy od EF Core 2,1, można teraz zdefiniować konstruktora z parametrami i mieć EF Core wywołać tego konstruktora podczas tworzenia wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="61727-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="61727-105">Parametry konstruktora można powiązać z zamapowanymi właściwościami lub różnymi rodzajami usług, aby ułatwić zachowanie takich zachowań jak ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="61727-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="61727-106">W EF Core 2,1 wszystkie powiązania konstruktora są według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="61727-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="61727-107">Konfiguracja określonych konstruktorów do użycia jest planowana w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="61727-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="61727-108">Powiązanie z zamapowanymi właściwościami</span><span class="sxs-lookup"><span data-stu-id="61727-108">Binding to mapped properties</span></span>

<span data-ttu-id="61727-109">Weź pod uwagę typowy model bloga/post:</span><span class="sxs-lookup"><span data-stu-id="61727-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="61727-110">Gdy EF Core tworzy wystąpienia tych typów, na przykład dla wyników zapytania, najpierw wywoła domyślnego konstruktora bez parametrów, a następnie ustawi każdą właściwość na wartość z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="61727-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="61727-111">Jeśli jednak EF Core odnajdzie sparametryzowany Konstruktor z nazwami i typami parametrów, które pasują do tych zamapowanych właściwości, wówczas będzie wywołać sparametryzowany Konstruktor z wartościami tych właściwości i nie ustawi jawnie żadnej właściwości.</span><span class="sxs-lookup"><span data-stu-id="61727-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="61727-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="61727-112">For example:</span></span>

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

<span data-ttu-id="61727-113">Niektóre rzeczy do zanotowania:</span><span class="sxs-lookup"><span data-stu-id="61727-113">Some things to note:</span></span>

* <span data-ttu-id="61727-114">Nie wszystkie właściwości muszą mieć parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="61727-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="61727-115">Na przykład właściwość post. Content nie jest ustawiana przez żaden parametr konstruktora, więc EF Core ustawi ją po wywołaniu konstruktora w normalny sposób.</span><span class="sxs-lookup"><span data-stu-id="61727-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="61727-116">Typy i nazwy parametrów muszą być zgodne z typami właściwości i nazwami, z tą różnicą, że właściwości mogą być w języku Pascal — wielkość liter w parametrach jest notacji CamelCase.</span><span class="sxs-lookup"><span data-stu-id="61727-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="61727-117">EF Core nie może ustawić właściwości nawigacji (takich jak blog lub wpisy powyżej) przy użyciu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="61727-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="61727-118">Konstruktor może być publiczny, prywatny lub mieć inne ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="61727-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="61727-119">Niemniej jednak serwery proxy ładowania opóźnionego wymagają, aby Konstruktor był dostępny z klasy dziedziczonego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61727-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="61727-120">Zazwyczaj oznacza to, że jest ona publiczna lub chroniona.</span><span class="sxs-lookup"><span data-stu-id="61727-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="61727-121">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="61727-121">Read-only properties</span></span>

<span data-ttu-id="61727-122">Gdy właściwości są ustawiane za pośrednictwem konstruktora, może się okazać, że niektóre z nich są tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="61727-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="61727-123">EF Core obsługuje to, ale istnieją pewne rzeczy, dla których można wyszukać:</span><span class="sxs-lookup"><span data-stu-id="61727-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="61727-124">Właściwości bez Setters nie są zamapowane według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="61727-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="61727-125">(W ten sposób można zamapować właściwości mapy, które nie powinny być mapowane, takie jak właściwości obliczane).</span><span class="sxs-lookup"><span data-stu-id="61727-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="61727-126">Użycie automatycznie generowanych wartości klucza wymaga właściwości klucza, która jest odczytywana do odczytu i zapisu, ponieważ wartość klucza musi być ustawiona przez generator kluczy podczas wstawiania nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="61727-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="61727-127">W łatwy sposób można uniknąć używania prywatnych metod ustawiających.</span><span class="sxs-lookup"><span data-stu-id="61727-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="61727-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="61727-128">For example:</span></span>

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

<span data-ttu-id="61727-129">EF Core widzi właściwość z prywatną metodą ustawiającą jako do odczytu i zapisu, co oznacza, że wszystkie właściwości są mapowane tak jak wcześniej, a klucz nadal może być generowany przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="61727-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="61727-130">Alternatywą dla korzystania z prywatnych metod ustawiających jest to, że właściwości naprawdę tylko do odczytu i dodają jawne mapowanie w OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="61727-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="61727-131">Podobnie niektóre właściwości można całkowicie usunąć i zastąpić tylko polami.</span><span class="sxs-lookup"><span data-stu-id="61727-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="61727-132">Rozważmy na przykład następujące typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="61727-132">For example, consider these entity types:</span></span>

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

<span data-ttu-id="61727-133">I ta konfiguracja w OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="61727-133">And this configuration in OnModelCreating:</span></span>

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

<span data-ttu-id="61727-134">Kwestie do uwagi:</span><span class="sxs-lookup"><span data-stu-id="61727-134">Things to note:</span></span>

* <span data-ttu-id="61727-135">Klucz "Property" jest teraz polem.</span><span class="sxs-lookup"><span data-stu-id="61727-135">The key "property" is now a field.</span></span> <span data-ttu-id="61727-136">Nie jest to pole `readonly`, aby można było użyć kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="61727-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="61727-137">Inne właściwości są właściwościami tylko do odczytu ustawionymi tylko w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="61727-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="61727-138">Jeśli wartość klucza podstawowego jest kiedykolwiek ustawiona tylko przez EF lub odczytana z bazy danych, nie trzeba uwzględniać jej w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="61727-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="61727-139">Spowoduje to pozostawienie klucza "Property" jako prostego pola i oznacza, że nie należy go ustawiać jawnie podczas tworzenia nowych blogów lub wpisów.</span><span class="sxs-lookup"><span data-stu-id="61727-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="61727-140">Ten kod spowoduje ostrzeżenie kompilatora "169" wskazujący, że pole nigdy nie jest używane.</span><span class="sxs-lookup"><span data-stu-id="61727-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="61727-141">Można to zignorować, ponieważ w rzeczywistości EF Core używa pola w sposób extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="61727-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="61727-142">Wstrzyknięcie usług</span><span class="sxs-lookup"><span data-stu-id="61727-142">Injecting services</span></span>

<span data-ttu-id="61727-143">EF Core może również wstrzyknąć "usługi" do konstruktora typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="61727-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="61727-144">Na przykład można wstrzyknąć następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="61727-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="61727-145">`DbContext` — bieżące wystąpienie kontekstu, które można również wpisać jako pochodny typ kontekstu</span><span class="sxs-lookup"><span data-stu-id="61727-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="61727-146">`ILazyLoader` — usługa ładowania opóźnionego — zobacz [dokumentację ładowania z opóźnieniem](../querying/related-data.md) , aby uzyskać więcej szczegółów</span><span class="sxs-lookup"><span data-stu-id="61727-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="61727-147">`Action<object, string>` — delegat ładowania z opóźnieniem — zobacz [dokumentację ładowania z opóźnieniem](../querying/related-data.md) , aby uzyskać więcej szczegółów</span><span class="sxs-lookup"><span data-stu-id="61727-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="61727-148">`IEntityType` — metadane EF Core skojarzone z tym typem jednostki</span><span class="sxs-lookup"><span data-stu-id="61727-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="61727-149">Począwszy od EF Core 2,1, można wstrzyknąć tylko usługi znane przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="61727-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="61727-150">Obsługa wstrzykiwania usług aplikacji jest brana pod uwagę w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="61727-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="61727-151">Na przykład można użyć wstrzykniętego DbContext, aby selektywnie uzyskiwać dostęp do bazy danych, aby uzyskać informacje o powiązanych jednostkach bez ładowania wszystkich elementów.</span><span class="sxs-lookup"><span data-stu-id="61727-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="61727-152">W poniższym przykładzie można uzyskać liczbę ogłoszeń w blogu bez ładowania wpisów:</span><span class="sxs-lookup"><span data-stu-id="61727-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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

<span data-ttu-id="61727-153">Oto kilka rzeczy, na które należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="61727-153">A few things to notice about this:</span></span>

* <span data-ttu-id="61727-154">Konstruktor jest prywatny, ponieważ jest tylko kiedykolwiek wywoływany przez EF Core i istnieje inny Konstruktor publiczny do użytku ogólnego.</span><span class="sxs-lookup"><span data-stu-id="61727-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="61727-155">Kod korzystający z wstrzykniętej usługi (czyli kontekstu) jest zgodny z `null` do obsługi przypadków, w których EF Core nie tworzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="61727-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="61727-156">Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu, zostanie zresetowana, gdy jednostka jest dołączona do nowego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="61727-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="61727-157">Wstrzykiwanie kontekstu DbContext jest często uznawane za Antywzorzec, ponieważ Couples typy jednostek bezpośrednio do EF Core.</span><span class="sxs-lookup"><span data-stu-id="61727-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="61727-158">Uważnie Rozważ wszystkie opcje przed użyciem wstrzykiwania usługi.</span><span class="sxs-lookup"><span data-stu-id="61727-158">Carefully consider all options before using service injection like this.</span></span>
