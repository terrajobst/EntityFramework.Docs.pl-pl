---
title: Typy jednostek z konstruktorów — EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 0536393d074d82583f47faae13cc22498193cb7e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994896"
---
# <a name="entity-types-with-constructors"></a>Typy jednostek za pomocą konstruktorów

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Począwszy od programu EF Core 2.1 jest teraz można zdefiniować konstruktora z parametrami, a programem EF Core wywołania tego konstruktora, podczas tworzenia wystąpienia jednostki. Parametry Konstruktora może być powiązana z właściwości zamapowanych lub do różnych rodzajów usług w celu ułatwienia zachowania, takich jak ładowanych z opóźnieniem.

> [!NOTE]  
> Począwszy od programu EF Core 2.1 wszystkie powiązania Konstruktor jest przez Konwencję. Konfiguracja określonego konstruktora do użycia jest planowana w przyszłej wersji.

## <a name="binding-to-mapped-properties"></a>Powiązywanie właściwości zamapowanych

Należy rozważyć typowy modelu/wpis w blogu:

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

Jeśli programu EF Core tworzy wystąpienia tych typów, takich jak wyniki zapytania, jest będzie pierwsze wywołanie domyślnego konstruktora bez parametrów i następnie ustaw wartość każdej właściwości z bazy danych. Jednak jeśli programu EF Core znajdzie sparametryzowania konstruktora przy użyciu nazwy i typy, które pasują do właściwości parametrów mapowane właściwości, a następnie zamiast tego będzie wywoływać sparametryzowanego konstruktora o wartości dla tych właściwości, a nie ustawi każdej właściwości jawnie. Na przykład:

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
Niektóre kwestie, należy zwrócić uwagę:
* Nie wszystkie właściwości konieczne parametry konstruktora. Na przykład właściwość Post.Content nie jest ustawiany przez żadnych parametrów konstruktora, więc programu EF Core zostanie ustawiony po wywołaniu konstruktora w normalny sposób.
* Nazwy i typy parametrów muszą być zgodne typy właściwości i nazw, z tą różnicą, że właściwości mogą być Pascal — z uwzględnieniem wielkości liter podczas, gdy parametry są w formacie camelcase.
* EF Core nie można ustawić właściwości nawigacji (na przykład blogu lub wpisy powyżej) przy użyciu konstruktora.
* Konstruktor może być publiczne, prywatne, lub innych ułatwień dostępu.

### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Gdy właściwości są ustawiane za pośrednictwem konstruktora sensowne może się niektóre z nich w trybie tylko do odczytu. Obsługuje tę funkcję programu EF Core, ale jest kilka rzeczy, zwróć uwagę na:
* Właściwości bez metod ustawiających nie są zamapowane przez Konwencję. (To zwykle właściwości, które nie powinny być mapowane, takich jak obliczone właściwości mapy.)
* Przy użyciu automatycznie generowanego klucza wartości wymaga właściwości klucza, która jest odczytu i zapisu, ponieważ wartość klucza musi być ustawiona przez generator kluczy, podczas wstawiania nowych jednostek.

Prosty sposób, aby uniknąć tych rzeczy jest używać prywatnych metod ustawiających. Na przykład:
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
EF Core uznaje prywatnej setter właściwości odczytu / zapisu, co oznacza, że wszystkie właściwości są mapowane tak jak poprzednio, i klawisz mogą nadal być generowane przez Magazyn.

Alternatywa dla użycia prywatnych metod ustawiających jest właściwości tak naprawdę tylko do odczytu i Dodaj mapowanie dokładniejsze w OnModelCreating. Podobnie niektóre właściwości mogą całkowicie usunięty i zastąpiony tylko pola. Na przykład należy wziąć pod uwagę te typy jednostek:

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
I tej konfiguracji w OnModelCreating:
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
Kwestii, które należy zwrócić uwagę:
* Klucz "property" jest polem. Nie jest `readonly` tak, aby klucze generowane przez Magazyn mogą być używane.
* Inne właściwości są właściwości tylko do odczytu ustawiana tylko w konstruktorze.
* Jeśli wartość klucza podstawowego tylko nigdy nie jest ustawiony przez EF lub odczytu z bazy danych, następnie nie ma potrzeby umieszczenie go w konstruktorze. Pozostawia klucz "property" jako prostym polem i sprawia, że jasne, że jej nie powinna być ustawiona jawnie podczas tworzenia nowego blogów i wpisów.

> [!NOTE]  
> Ten kod będzie skutkować kompilatora ostrzeżenie "169" wskazujący, że pole nigdy nie jest używany. Może być został zignorowany, ponieważ w rzeczywistości programu EF Core jest za pomocą pola w sposób extralinguistic.

## <a name="injecting-services"></a>Wprowadzanie usługi

EF Core mogą umożliwić również iniekcję "usługi" do konstruktora typu jednostki. Na przykład że może wprowadzone:
* `DbContext` -wystąpienie kontekstu bieżącego można także wpisać jako pochodny typu DbContext
* `ILazyLoader` -Usługa ładowanych z opóźnieniem — zobacz [dokumentacji powolne ładowanie](../querying/related-data.md) Aby uzyskać więcej informacji
* `Action<object, string>` — obiekt delegowany ładowanych z opóźnieniem — zobacz [dokumentacji powolne ładowanie](../querying/related-data.md) Aby uzyskać więcej informacji
* `IEntityType` -metadanych programu EF Core skojarzonych z tym typem jednostki

> [!NOTE]  
> Począwszy od programu EF Core 2.1 może zostać wprowadzona tylko usługi znane przez platformę EF Core. Obsługa wprowadzanie usługi aplikacji jest rozważane w przyszłej wersji.

Na przykład można wprowadzonego typu DbContext na selektywny dostęp do bazy danych, aby uzyskać informacje o powiązanych jednostek bez ładowania ich wszystkich. W poniższym przykładzie służy do uzyskania liczby wpisów w blogu bez ładowania wpisy:

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
Kilka istotnych kwestii na ten temat:
* Konstruktor jest prywatny, ponieważ tylko nigdy nie jest wywoływana przez platformę EF Core i ma innego konstruktora publicznego do użytku ogólnego.
* Kod przy użyciu usługi wprowadzonego (kontekstu) jest obrony przed nim są `null` sposób obsługiwać przypadki, gdzie programu EF Core nie jest tworzone wystąpienie.
* Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu zostaną zresetowane, gdy jednostka jest dołączony do nowego wystąpienia kontekstu.

> [!WARNING]  
> Wprowadzanie DbContext następująco jest często uznawana za zapobieganie wzorca od czasu jej couples typów jednostek bezpośrednio do programu EF Core. Zastanów się uważnie wszystkie opcje przed przy użyciu iniekcji usługi w następujący sposób.
