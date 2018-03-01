---
title: "Typy jednostek z konstruktorów - EF Core"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 2632488569c538a11c7a31a9a866d2fadb29eeb5
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="entity-types-with-constructors"></a>Typy jednostek z konstruktorów

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 2.1.

W programie EF Core 2.1, obecnie istnieje możliwość zdefiniować konstruktora z parametrami, a Core EF wywołać konstruktora, podczas tworzenia wystąpienia jednostki. Parametry Konstruktor może być powiązana z mapowanej właściwości lub do różnych rodzajów usług w celu ułatwienia zachowań, takich jak opóźnionego ładowania.

> [!NOTE]  
> Począwszy od EF Core 2.1 wszystkie powiązania konstruktora jest przez Konwencję. Planowany konfiguracji określonych konstruktorów do użycia w przyszłości.

## <a name="binding-to-mapped-properties"></a>Powiązanie do mapowanej właściwości

Należy wziąć pod uwagę typowe modelu/wpis w blogu:

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

Gdy EF Core tworzy wystąpienia typu, takich jak dla wyników zapytania, utworzy pierwsze wywołanie konstruktora domyślnego i następnie ustaw wartość każdej właściwości z bazy danych. Jednak jeśli EF Core znajdzie sparametryzowanym konstruktorze z nazwy parametru i typów, które pasują do właściwości zamapować właściwości, a następnie zamiast tego zostanie wywołanie konstruktora sparametryzowane wartości tych właściwości, a nie jawnie ustawia każdej właściwości. Na przykład:

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
Niektóre rzeczy do uwzględnienia:
* Nie wszystkie właściwości musi być parametrami konstruktora. Na przykład właściwość Post.Content nie jest ustawiany przez żadnego parametru konstruktora, więc EF Core zostanie ustawiony po wywołaniu konstruktora w zwykły sposób.
* Nazwy i typy parametrów muszą być zgodne typy właściwości i nazwy, z wyjątkiem tego, że właściwości mogą być Pascal — z uwzględnieniem wielkości liter podczas parametry są pisane formatu.
* Podstawowe EF nie można ustawić właściwości nawigacji (na przykład blogu lub ogłoszeń powyżej) przy użyciu konstruktora.
* Konstruktor może być publiczne, prywatne, lub inne ułatwień dostępu.

### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Po właściwości są ustawiane za pośrednictwem konstruktora go warto utworzyć niektóre z nich tylko do odczytu. Obsługuje tę funkcję EF Core, ale jest kilka rzeczy, zaczekaj na:
* Właściwości bez metody pobierające nie są mapowane przez Konwencję. (To zwykle mapowania właściwości, które nie powinny być mapowane, takie jak właściwości obliczanej.)
* Za pomocą wartości kluczy automatycznie generowanych wymaga właściwości klucza, która jest odczytu i zapisu, ponieważ wartości klucza musi być ustawiona przez generator kluczy, podczas wstawiania nowych jednostek.

Prosty sposób, aby uniknąć tych czynności jest Użyj prywatnego setters. Na przykład:
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
Podstawowe EF widzi właściwości prywatnej setter jako odczytu i zapisu, co oznacza, że wszystkie właściwości są mapowane jak poprzednio i klucz może nadal być generowane przez Magazyn.

Jest to alternatywa dla użycia prywatnej metody ustawiające właściwości naprawdę tylko do odczytu i Dodaj mapowanie dokładniejsze w OnModelCreating. Podobnie niektóre właściwości można całkowicie usunięty i zastąpiony tylko pola. Na przykład wziąć pod uwagę następujące typy jednostek:

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
I tą konfiguracją w OnModelCreating:
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
Rzeczy do uwzględnienia:
* Klucz "property" jest polem. Nie jest `readonly` pola tak, aby mogły być używane klucze generowane przez Magazyn. 
* Inne właściwości są właściwości tylko do odczytu ustawiana tylko w konstruktorze.
* Jeśli wartość klucza podstawowego tylko określone przez EF lub odczytu z bazy danych, oznacza to, że nie trzeba wpisywać go w konstruktorze. Pozostawia jako proste pole klucza "property" i ułatwia wyczyścić go nie należy ustawiać jawnie podczas tworzenia nowego blogi lub wpisów.

> [!NOTE]  
> Ten kod spowoduje kompilatora ostrzeżenie "169" wskazujący, że pole jest nigdy używane. Ponieważ w rzeczywistości EF Core używa pola w sposób extralinguistic można zignorować to.

## <a name="injecting-services"></a>Wstrzyknięcie usług

EF Core można również wprowadzić "usługi" do konstruktora typu jednostki. Na przykład następujące czynności mogą zostać dodane:
* `DbContext` -bieżącego wystąpienia kontekstu, która może też wpisać jako typ pochodny typu DbContext
* `ILazyLoader` -Usługa opóźnionego ładowania — zobacz [dokumentacji opóźnionego ładowania](../querying/related-data.md) więcej szczegółów
* `Action<object, string>` -Delegowanie powolne ładowanie — zobacz [dokumentacji opóźnionego ładowania](../querying/related-data.md) więcej szczegółów
* `IEntityType` -EF Core metadane skojarzone z tym typem jednostki

> [!NOTE]  
> Począwszy od EF Core 2.1 mogą zostać dodane tylko usługi znane EF Core. Obsługa wstrzykiwania usługi aplikacji jest rozważane w przyszłości.

Na przykład można wprowadzony DbContext selektywne uzyskiwanie dostępu do bazy danych, aby uzyskać informacje o powiązanych jednostek bez ładowania je wszystkie. W poniższym przykładzie służy do uzyskania liczby wpisów w blogu bez ładowania wpisów:

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
Kilka istotnych Zwróć uwagę na ten temat:
* Konstruktor jest prywatny, ponieważ jest on tylko kiedykolwiek wywołania przez rdzeń EF, i innego konstruktora publicznego do użytku ogólnego.
* Kod przy użyciu usługi wprowadzony (tj. kontekst) jest obrony przed jego trwa `null` do obsługi przypadków gdzie EF Core nie jest tworzone wystąpienie.
* Ponieważ usługa jest przechowywana we właściwości odczytu/zapisu zostaną zresetowane, gdy obiekt jest dołączony do nowego wystąpienia kontekstu.

> [!WARNING]  
> Wstrzyknięcie DbContext, jak to często uważa się za wzorca oprogramowania od momentu jego couples Twojego typów jednostek bezpośrednio do EF Core. Należy rozważyć wszystkie opcje przed przy użyciu iniekcji usługi następująco.
