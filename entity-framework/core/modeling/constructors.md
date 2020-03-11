---
title: Typy jednostek z konstruktorami — EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417330"
---
# <a name="entity-types-with-constructors"></a>Typy jednostek z konstruktorami

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Począwszy od EF Core 2,1, można teraz zdefiniować konstruktora z parametrami i mieć EF Core wywołać tego konstruktora podczas tworzenia wystąpienia jednostki. Parametry konstruktora można powiązać z zamapowanymi właściwościami lub różnymi rodzajami usług, aby ułatwić zachowanie takich zachowań jak ładowanie z opóźnieniem.

> [!NOTE]  
> W EF Core 2,1 wszystkie powiązania konstruktora są według Konwencji. Konfiguracja określonych konstruktorów do użycia jest planowana w przyszłej wersji.

## <a name="binding-to-mapped-properties"></a>Powiązanie z zamapowanymi właściwościami

Weź pod uwagę typowy model bloga/post:

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

Gdy EF Core tworzy wystąpienia tych typów, na przykład dla wyników zapytania, najpierw wywoła domyślnego konstruktora bez parametrów, a następnie ustawi każdą właściwość na wartość z bazy danych. Jeśli jednak EF Core odnajdzie sparametryzowany Konstruktor z nazwami i typami parametrów, które pasują do tych zamapowanych właściwości, wówczas będzie wywołać sparametryzowany Konstruktor z wartościami tych właściwości i nie ustawi jawnie żadnej właściwości. Na przykład:

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

Niektóre rzeczy do zanotowania:

* Nie wszystkie właściwości muszą mieć parametry konstruktora. Na przykład właściwość post. Content nie jest ustawiana przez żaden parametr konstruktora, więc EF Core ustawi ją po wywołaniu konstruktora w normalny sposób.
* Typy i nazwy parametrów muszą być zgodne z typami właściwości i nazwami, z tą różnicą, że właściwości mogą być w języku Pascal — wielkość liter w parametrach jest notacji CamelCase.
* EF Core nie może ustawić właściwości nawigacji (takich jak blog lub wpisy powyżej) przy użyciu konstruktora.
* Konstruktor może być publiczny, prywatny lub mieć inne ułatwienia dostępu. Niemniej jednak serwery proxy ładowania opóźnionego wymagają, aby Konstruktor był dostępny z klasy dziedziczonego serwera proxy. Zazwyczaj oznacza to, że jest ona publiczna lub chroniona.

### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Gdy właściwości są ustawiane za pośrednictwem konstruktora, może się okazać, że niektóre z nich są tylko do odczytu. EF Core obsługuje to, ale istnieją pewne rzeczy, dla których można wyszukać:

* Właściwości bez Setters nie są zamapowane według Konwencji. (W ten sposób można zamapować właściwości mapy, które nie powinny być mapowane, takie jak właściwości obliczane).
* Użycie automatycznie generowanych wartości klucza wymaga właściwości klucza, która jest odczytywana do odczytu i zapisu, ponieważ wartość klucza musi być ustawiona przez generator kluczy podczas wstawiania nowych jednostek.

W łatwy sposób można uniknąć używania prywatnych metod ustawiających. Na przykład:

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

EF Core widzi właściwość z prywatną metodą ustawiającą jako do odczytu i zapisu, co oznacza, że wszystkie właściwości są mapowane tak jak wcześniej, a klucz nadal może być generowany przez magazyn.

Alternatywą dla korzystania z prywatnych metod ustawiających jest to, że właściwości naprawdę tylko do odczytu i dodają jawne mapowanie w OnModelCreating. Podobnie niektóre właściwości można całkowicie usunąć i zastąpić tylko polami. Rozważmy na przykład następujące typy jednostek:

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

I ta konfiguracja w OnModelCreating:

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

Kwestie do uwagi:

* Klucz "Property" jest teraz polem. Nie jest to pole `readonly`, aby można było użyć kluczy generowanych przez magazyn.
* Inne właściwości są właściwościami tylko do odczytu ustawionymi tylko w konstruktorze.
* Jeśli wartość klucza podstawowego jest kiedykolwiek ustawiona tylko przez EF lub odczytana z bazy danych, nie trzeba uwzględniać jej w konstruktorze. Spowoduje to pozostawienie klucza "Property" jako prostego pola i oznacza, że nie należy go ustawiać jawnie podczas tworzenia nowych blogów lub wpisów.

> [!NOTE]  
> Ten kod spowoduje ostrzeżenie kompilatora "169" wskazujący, że pole nigdy nie jest używane. Można to zignorować, ponieważ w rzeczywistości EF Core używa pola w sposób extralinguistic.

## <a name="injecting-services"></a>Wstrzyknięcie usług

EF Core może również wstrzyknąć "usługi" do konstruktora typu jednostki. Na przykład można wstrzyknąć następujące elementy:

* `DbContext` — bieżące wystąpienie kontekstu, które można również wpisać jako pochodny typ kontekstu
* `ILazyLoader` — usługa ładowania opóźnionego — zobacz [dokumentację ładowania z opóźnieniem](../querying/related-data.md) , aby uzyskać więcej szczegółów
* `Action<object, string>` — delegat ładowania z opóźnieniem — zobacz [dokumentację ładowania z opóźnieniem](../querying/related-data.md) , aby uzyskać więcej szczegółów
* `IEntityType` — metadane EF Core skojarzone z tym typem jednostki

> [!NOTE]  
> Począwszy od EF Core 2,1, można wstrzyknąć tylko usługi znane przez EF Core. Obsługa wstrzykiwania usług aplikacji jest brana pod uwagę w przyszłych wydaniach.

Na przykład można użyć wstrzykniętego DbContext, aby selektywnie uzyskiwać dostęp do bazy danych, aby uzyskać informacje o powiązanych jednostkach bez ładowania wszystkich elementów. W poniższym przykładzie można uzyskać liczbę ogłoszeń w blogu bez ładowania wpisów:

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

Oto kilka rzeczy, na które należy zwrócić uwagę:

* Konstruktor jest prywatny, ponieważ jest tylko kiedykolwiek wywoływany przez EF Core i istnieje inny Konstruktor publiczny do użytku ogólnego.
* Kod korzystający z wstrzykniętej usługi (czyli kontekstu) jest zgodny z `null` do obsługi przypadków, w których EF Core nie tworzy wystąpienia.
* Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu, zostanie zresetowana, gdy jednostka jest dołączona do nowego wystąpienia kontekstu.

> [!WARNING]  
> Wstrzykiwanie kontekstu DbContext jest często uznawane za Antywzorzec, ponieważ Couples typy jednostek bezpośrednio do EF Core. Uważnie Rozważ wszystkie opcje przed użyciem wstrzykiwania usługi.
