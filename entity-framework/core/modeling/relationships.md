---
title: Relacje — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054326"
---
# <a name="relationships"></a>Relacje

Relacja definiuje sposób dwie jednostki odnoszą się do siebie. W relacyjnej bazie danych to jest reprezentowany przez ograniczenie klucza obcego.

> [!NOTE]  
> Większość przykładów w tym artykule umożliwiają pokazują pojęcia relacji jeden do wielu. Przykłady relacje jeden do jednego oraz wiele do wielu można znaleźć [inne wzorce relacji](#other-relationship-patterns) sekcji na końcu tego artykułu.

## <a name="definition-of-terms"></a>Definicje terminów

Liczba terminów używanych do opisywania relacji

* **Jednostki zależnej:** to jednostka, która zawiera właściwości klucza obcego. Czasami określane jako 'child' relacji.

* **Jednostka główna:** to jednostka, która zawiera właściwości klucza podstawowego/alternatywny. Czasami określane jako "parent" w relacji.

* **Klucz obcy:** właściwości w jednostki zależnej, który jest używany do przechowywania wartości właściwości klucza głównej powiązanej jednostki.

* **Klucz główny:** właściwości, który unikatowo identyfikuje jednostki podmiotu zabezpieczeń. Może to być kluczem podstawowym lub unikatowym.

* **Właściwość nawigacji:** właściwość zdefiniowaną w głównych i/lub zależnych jednostki, która zawiera odwołania do powiązanych entity(s).

  * **Właściwości nawigacji kolekcji:** właściwość nawigacji, który zawiera odwołania do wielu powiązanych jednostek.

  * **Właściwości nawigacji odwołania:** właściwość nawigacji, który zawiera odwołanie do jednego obiektu pokrewnego.

  * **Odwrócona właściwość nawigacji:** Omawiając właściwości nawigacji w szczególności, określenie to odnosi się do właściwości nawigacji na drugim końcu relacji.

Następujący przykładowy kod przedstawia relacji jeden do wielu między `Blog` i`Post`

* `Post`jest jednostki zależnej

* `Blog`jest główną jednostki

* `Post.BlogId`jest kluczem obcym

* `Blog.BlogId`jest to klucz główny (w tym przypadku jest kluczem podstawowym, a nie klucza alternatywnego)

* `Post.Blog`jest właściwością nawigacji odwołania

* `Blog.Posts`jest właściwością nawigacji kolekcji

* `Post.Blog`jest odwróconą właściwość nawigacji z `Blog.Posts` (i na odwrót)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konwencje

Według konwencji będzie można utworzyć relacji, po wykryte na typ właściwości nawigacji. Właściwość jest traktowany jako właściwość nawigacji, jeśli nie można zamapować typu, który wskazuje jako typem skalarnym przez bieżącego dostawcę bazy danych.

> [!NOTE]  
> Relacje, które zostały wykryte przez Konwencję zawsze będzie obowiązywać klucza podstawowego jednostki podmiotu zabezpieczeń. Pod kątem klucza alternatywnego, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu interfejsu API Fluent.

### <a name="fully-defined-relationships"></a>Relacje zdefiniowane w pełni

Najbardziej typowe wzorca dla relacji jest zdefiniowany po obu stronach relacji i właściwości klucza obcego zdefiniowana w klasie jednostki zależnej właściwości nawigacji.

* Jeśli między dwoma typami para właściwości nawigacji, następnie one zostanie skonfigurowany jako właściwości nawigacji odwrotny tej samej relacji.

* Jeśli jednostki zależnej zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`, lub `<principal entity name><primary key property name>` , a następnie zostaną skonfigurowane jako klucz obcy.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Jeśli istnieje wiele właściwości nawigacji zdefiniowane między dwoma typami (tj. więcej niż jedną parę różne nawigacji wskazujące ze sobą), następnie relacje nie zostaną utworzone przez Konwencję i konieczne będzie ręczne skonfigurowanie ich do identyfikacji sposób pary właściwości nawigacji.

### <a name="no-foreign-key-property"></a>Nie właściwości klucza obcego

Gdy jest zalecane, aby mieć zdefiniowany w klasie jednostki zależnej właściwości klucza obcego, nie jest wymagane. Przypadku nieznalezienia ma właściwości klucza obcego, zostaną wprowadzone tle właściwości klucza obcego o nazwie `<navigation property name><principal key property name>` (zobacz [właściwości tle](shadow-properties.md) Aby uzyskać więcej informacji).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Właściwości pojedynczej wartości

Łącznie z tylko jedną właściwość nawigacji (nie odwrotny nawigacji i ma właściwości klucza obcego) jest wystarczająca do zdefiniowano relacji w Konwencji. Można również mieć właściwości pojedynczej wartości i właściwości klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Usuwanie kaskadowe

Konwencja, usuwanie kaskadowe zostanie ustawiona do *Cascade* dla relacji wymagane i *ClientSetNull* dla relacji opcjonalne. *CASCADE* oznacza jednostki zależne, również zostaną usunięte. *ClientSetNull* oznacza, że pozostanie jednostek zależnych, które nie zostały załadowane do pamięci bez zmian i muszą być ręcznie usunięty lub zaktualizowany w celu wskaż prawidłową jednostkę podmiotu zabezpieczeń. Dla obiektów, które są ładowane do pamięci EF Core będzie podejmować próby ustaw wartość null z właściwości klucza obcego.

Zobacz [wymaganych i opcjonalnych relacje](#required-and-optional-relationships) sekcji różnicę między relacje wymaganych i opcjonalnych.

Zobacz [Cascade Delete](../saving/cascade-delete.md) więcej informacji na temat różnych Usuń zachowania i wartości domyślne używane przez Konwencję.

## <a name="data-annotations"></a>Adnotacji danych

Istnieją dwa adnotacji danych, które mogą służyć do konfigurowania relacji, `[ForeignKey]` i `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Adnotacje danych służy do konfigurowania właściwości, które należy używać jako właściwość klucza obcego w określonej relacji. Jest to zazwyczaj wykonywane, gdy właściwość klucza obcego nie zostanie wykryta przez Konwencję.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` Adnotacji można umieścić we właściwości nawigacji, albo w relacji. Nie trzeba go we właściwości nawigacji w klasie jednostki zależnej.

### <a name="inverseproperty"></a>[InverseProperty]

Adnotacje danych służy do konfigurowania, jak pary właściwości nawigacji jednostki zależnej i głównej. Jest to zazwyczaj wykonywane, gdy istnieje więcej niż jedna para właściwości nawigacji między dwoma typami jednostek.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować relację w interfejsie API Fluent, należy uruchomić, określając właściwości nawigacji, które tworzą relacji. `HasOne`lub `HasMany` identyfikuje właściwość nawigacji typu jednostki są od konfiguracji na. Następnie łańcucha wywołania `WithOne` lub `WithMany` do identyfikowania odwrotny nawigacji. `HasOne`/`WithOne`są używane dla właściwości nawigacji odwołania oraz `HasMany` / `WithMany` są używane w przypadku właściwości nawigacji kolekcji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Właściwości pojedynczej wartości

Jeśli masz tylko jedną właściwość nawigacji, istnieją przeciążenia bez parametrów metody `WithOne` i `WithMany`. Oznacza to, że jest koncepcyjnie odwołania lub kolekcji na końcu relacji, ale nie ma właściwości nawigacji zawarte w klasie jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Klucz obcy

Interfejsu API Fluent służy do konfigurowania właściwości, które należy używać jako właściwość klucza obcego w określonej relacji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Następujący przykładowy kod przedstawia sposób konfigurowania złożonych kluczy obcych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Można użyć ciągu przeciążenia `HasForeignKey(...)` do skonfigurowania właściwości w tle jako klucz obcy (zobacz [właściwości tle](shadow-properties.md) Aby uzyskać więcej informacji). Firma Microsoft zaleca jawnie dodawania właściwości cień do modelu, przed użyciem go jako klucz obcy (jak pokazano poniżej).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Klucza podmiotu

Jeśli chcesz, aby klucz obcy do odwołania właściwości innych niż klucz podstawowy, można użyć interfejsu API Fluent do skonfigurowania właściwości klucza podmiotu zabezpieczeń dla tej relacji. Właściwość, którą można skonfigurować jako główny klucza zostanie automatycznie, należy skonfigurować jako klucza alternatywnego (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Następujący przykładowy kod przedstawia sposób konfigurowania złożonych kluczy głównych.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> Kolejność, w którym należy określić główny właściwości klucza muszą być zgodne kolejność, w którym są określone dla klucza obcego.

### <a name="required-and-optional-relationships"></a>Relacje wymaganych i opcjonalnych

Aby określić, czy relacja jest wymagany lub opcjonalny, można użyć interfejsu API Fluent. Ostatecznie to określa, czy właściwość klucza obcego jest wymagany lub opcjonalny. Jest to użyteczna, gdy używasz klucz obcy Stan cienia. Jeśli masz właściwości klucza obcego w klasie jednostki, requiredness relacji jest określany na podstawie tego, czy właściwość klucza obcego jest wymagany lub opcjonalny (zobacz [właściwości wymaganych i opcjonalnych](required-optional.md) Aby uzyskać więcej informacji informacje).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>Usuwanie kaskadowe

Można użyć interfejsu API Fluent jawnie skonfigurować zachowanie usuwania kaskadowego określonej relacji.

Zobacz [Cascade Delete](../saving/cascade-delete.md) w sekcji zapisywania danych szczegółowe omówienie każdej z nich.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Innymi wzorami relacji

### <a name="one-to-one"></a>Jeden do jednego

Relacje jeden-do-jednego ma właściwości nawigacji odwołania po obu stronach. Wykonaj one tej samej Konwencji jako relacji jeden do wielu, ale wprowadzono unikatowego indeksu na właściwość klucza obcego w celu zapewnienia, że tylko jeden zależne od powiązany jest każdy podmiot zabezpieczeń.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF wybrać jeden jednostek jako zależnego oparte na możliwość wykrywania właściwości klucza obcego. Jeśli niewłaściwy jednostki jest wybrany jako zależnego, można użyć interfejsu API Fluent, aby rozwiązać ten problem.

Podczas konfigurowania relacji z interfejsu API Fluent, użyj `HasOne` i `WithOne` metody.

Podczas konfigurowania klucza obcego, należy określić typ jednostki zależnej — zauważyć przekazane do parametru ogólnego `HasForeignKey` na liście poniżej. W relacji jeden do wielu jest jasne, czy jednostka z nawigacji odwołania jest zależnego i z kolekcji jest podmiot zabezpieczeń. Ale nie jest tak w relacji jeden - dlatego trzeba jawnie definiować go.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Wiele do wielu

Relacje wiele do wielu bez klasę jednostki do reprezentowania tabeli sprzężenia nie są jeszcze obsługiwane. Jednak może wymagać relacji wiele do wielu, umieszczając klasę jednostki dla tabeli sprzężenia i relacje jeden do wielu oddzielnych dwóch mapowania.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
