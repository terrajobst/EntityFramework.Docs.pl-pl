---
title: Relacje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929940"
---
# <a name="relationships"></a>Relacje

Relacja definiuje sposób dwie jednostki powiązane są ze sobą. W relacyjnej bazie danych jest reprezentowane przez ograniczenie klucza obcego.

> [!NOTE]  
> Większość przykładów w tym artykule umożliwiają zademonstrowania pojęć relacji jeden do wielu. Zobacz przykłady relacji jeden do jednego i wiele do wielu [inne wzorce relacji](#other-relationship-patterns) sekcji na końcu tego artykułu.

## <a name="definition-of-terms"></a>Definicje terminów

Istnieje wiele terminy używane do opisywania relacji

* **Jednostki zależne:** To jest jednostka, która zawiera właściwości kluczy obcych. Czasami określane jako "podrzędne" w relacji.

* **Jednostka główna:** To jest jednostka, która zawiera właściwości klucza podstawowego/alternatywny. Czasami określane jako "parent" w relacji.

* **Klucz obcy:** Właściwości w jednostce zależne, który jest używany do przechowywania wartości właściwości klucza jednostki powiązanej jednostki.

* **Klucz jednostki:** Właściwość, która jednoznacznie identyfikuje główną jednostki. Może to być kluczem podstawowym lub unikatowym.

* **Właściwość nawigacji:** Właściwości zdefiniowane w jednostce podmiotu zabezpieczeń i/lub zależne, który zawiera odwołania do powiązanych entity(s).

  * **Właściwości nawigacji kolekcji:** Właściwość nawigacji, który zawiera odwołania do wielu powiązanych jednostek.

  * **Odwołanie do właściwości nawigacji:** Właściwość nawigacji, która zawiera odwołanie do pojedynczego obiektu pokrewnego.

  * **Właściwość nawigacji odwrotność:** Omawiając właściwości określonej nawigacji, określenie to odnosi się do właściwości nawigacji na końcu relacji.

W poniższym fragmencie kodu przedstawiono relację jeden do wielu między `Blog` i `Post`

* `Post` to jednostka zależna

* `Blog` to jednostka główna

* `Post.BlogId` jest to klucz obcy

* `Blog.BlogId` to klucz jednostki (w tym przypadku jest kluczem podstawowym, a nie klucza alternatywnego)

* `Post.Blog` jest to właściwość nawigacji odwołania

* `Blog.Posts` jest to właściwość nawigacji kolekcji

* `Post.Blog` jest właściwość nawigacji odwrotność `Blog.Posts` (i na odwrót)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją będzie można utworzyć relacji, po wykryte na typ właściwości nawigacji. Właściwość jest traktowana jako właściwość nawigacji, jeśli nie można zamapować typu, który wskazuje jako typ skalarny przez bieżącego dostawcę bazy danych.

> [!NOTE]  
> Relacje, które zostały wykryte przez Konwencję zawsze będą ukierunkowane na klucz podstawowy jednostki głównej. Pod kątem klucza alternatywnego, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu interfejsu API Fluent.

### <a name="fully-defined-relationships"></a>W pełni zdefiniowanych relacji

Najczęstszym wzorcem dla relacji jest zdefiniowane na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależne właściwości nawigacji.

* Jeśli para właściwości nawigacji znajduje się między dwoma typami, następnie one zostaną skonfigurowane jako właściwości nawigacji odwrotność tej samej relacji.

* Jeśli jednostka zależna zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`, lub `<principal entity name><primary key property name>` , a następnie zostaną skonfigurowane jako klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Czy wiele właściwości nawigacji zdefiniowane między dwoma typami (czyli więcej niż jedną parę distinct źródłem, które wskazują na siebie nawzajem), następnie relacje nie zostanie utworzony zgodnie z Konwencją i trzeba będzie ręcznie skonfigurować je do identyfikowania sposób, w jaki pary właściwości nawigacji.

### <a name="no-foreign-key-property"></a>Nie właściwości klucza obcego

Mimo że zaleca się mieć właściwość klucza obcego zdefiniowanej w klasie jednostki zależne, nie jest wymagana. Jeśli zostanie znalezione nie właściwość klucza obcego, zostaną wprowadzone właściwości klucza obcego w tle o nazwie `<navigation property name><principal key property name>` (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Właściwości pojedynczej wartości

Łącznie z tylko jedną właściwość nawigacji (nie odwrotność nawigacji i nie właściwości klucza obcego) jest wystarczający, aby mieć relacji zdefiniowanych przez Konwencję. Można również mieć właściwości pojedynczej wartości i właściwości klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Usuwanie kaskadowe

Zgodnie z Konwencją, usuwanie kaskadowe zostanie ustawiony *Cascade* dla relacji wymagane i *ClientSetNull* dla relacji opcjonalne. *Kaskadowe* oznacza, że jednostki zależne również zostaną usunięte. *ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci pozostaną bez zmian i muszą być ręcznie usunięty lub poinformowani o prawidłową jednostkę główną. W przypadku jednostek, które są ładowane do pamięci programu EF Core będzie podejmować próby równa właściwości klucza obcego o wartości null.

Zobacz [relacje wymaganych i opcjonalnych](#required-and-optional-relationships) sekcji różnicę między relacje wymaganych i opcjonalnych.

Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) więcej szczegółów na temat różnych Usuń zachowania i wartości domyślne używane przez Konwencję.

## <a name="data-annotations"></a>Adnotacje danych

Istnieją dwa adnotacji danych, które mogą być używane do konfigurowania relacji `[ForeignKey]` i `[InverseProperty]`. Są one dostępne w `System.ComponentModel.DataAnnotations.Schema` przestrzeni nazw.

### <a name="foreignkey"></a>[Klucza obcego]

Korzystanie z adnotacji danych, aby skonfigurować, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji. Zazwyczaj jest to wykonywane, gdy właściwość klucza obcego nie został odnaleziony przez Konwencję.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` Adnotacji można umieścić we właściwości nawigacji, albo w relacji. Nie musi przejść na właściwość nawigacji w klasie jednostki zależne.

### <a name="inverseproperty"></a>[InverseProperty]

Korzystanie z adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji jednostki zależnej i głównej. Zazwyczaj jest to wykonywane, gdy istnieje więcej niż jedną parę właściwości nawigacji między dwoma typami encji.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Interfejs Fluent API

Aby skonfigurować relację w interfejsie API Fluent, możesz rozpocząć, określając właściwości nawigacji, które tworzą relacji. `HasOne` lub `HasMany` identyfikuje właściwość nawigacji typu jednostki, rozpoczynamy od konfiguracji na. Można następnie połączyć w łańcuch po wywołaniu `WithOne` lub `WithMany` do identyfikowania odwrotność nawigacji. `HasOne`/`WithOne` są używane dla właściwości nawigacji odwołania i `HasMany` / `WithMany` są używane dla właściwości nawigacji kolekcji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Właściwości pojedynczej wartości

Jeśli masz tylko jedną właściwość nawigacji, a następnie istnieją przeciążenia bez parametrów `WithOne` i `WithMany`. Oznacza to, że ma pod względem koncepcyjnym odwołanie lub kolekcji na końcu relacji, ale nie ma właściwości nawigacji zawarta w klasie jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Klucz obcy

Interfejs Fluent API umożliwiają skonfigurowanie, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożonego klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

Można użyć ciągu przeciążenia `HasForeignKey(...)` do skonfigurowania właściwości w tle jako klucz obcy (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji). Zalecane jest jawne dodanie właściwości w tle do modelu, zanim użyjesz go jako klucza obcego (jak pokazano poniżej).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Klucz jednostki

Jeśli chcesz, aby klucz obcy, aby odwoływać się do właściwości innego niż klucz podstawowy, można użyć interfejsu API Fluent do skonfigurowania właściwości klucza podmiotu zabezpieczeń dla relacji. Właściwość, którą można skonfigurować jako głównej będą kluczowe automatycznie należy skonfigurować jako klucza alternatywnego (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).

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

W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożony klucz jednostki.

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
> Kolejność, w której zostaną określone jednostki właściwości klucza musi być zgodna kolejność, w którym są określone w kluczu obcym.

### <a name="required-and-optional-relationships"></a>Relacje wymagane i opcjonalne

Aby określić, czy relacja jest wymagane lub opcjonalne, można użyć interfejsu API Fluent. Ostatecznie to określa, czy właściwość klucza obcego jest wymagane lub opcjonalne. Jest to najbardziej przydatne, korzystając z kluczem obcym stanu w tle. Jeśli masz właściwości klucza obcego w klasie jednostki, a następnie requiredness relacji jest określana na podstawie tego, czy właściwość klucza obcego jest wymagane lub opcjonalne (zobacz [wymagane i opcjonalne właściwości](required-optional.md) Aby uzyskać więcej informacji informacje o).

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

Interfejs Fluent API umożliwiają skonfigurowanie jawnie zachowanie usuwanie kaskadowe określonej relacji.

Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) sekcji Zapisywanie danych szczegółowe omówienie każdej z nich.

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

## <a name="other-relationship-patterns"></a>Inne wzorce relacji

### <a name="one-to-one"></a>Jeden do jednego

Relacje jeden-do-jednego ma odwołanie do właściwości nawigacji po obu stronach. Używają tej samej Konwencji co relacji jeden do wielu, ale unikatowy indeks został wprowadzony na właściwość klucza obcego, aby upewnić się, że tylko jeden zależnych od powiązany jest każdy podmiot zabezpieczeń.

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
> EF wybrać jeden z jednostki, które jest zależne od oparte na możliwość wykrywania właściwości klucza obcego. Jeśli problem jednostki jest wybierany jako zależnych od ustawień lokalnych, można użyć Fluent API, aby rozwiązać ten problem.

Podczas konfigurowania relacja z interfejsem API Fluent, możesz użyć `HasOne` i `WithOne` metody.

Podczas konfigurowania klucza obcego, należy określić typ jednostki zależne - Zwróć uwagę, parametr generyczny udostępniane `HasForeignKey` na liście poniżej. W przypadku relacji jeden do wielu jest jasne, czy jednostka z nawigacją odwołanie jest zależnych od ustawień lokalnych, jak i z kolekcji jest podmiot zabezpieczeń. Ale to nie jest tak w przypadku relacji jeden do jednego — dlatego trzeba jawnie zdefiniować go.

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

Relacje wiele do wielu, bez klasę jednostki, do reprezentowania tabelę sprzężenia nie są jeszcze obsługiwane. Jednak może reprezentować relacji wiele do wielu, umieszczając klasę jednostki dla tabeli sprzężenia i dwie oddzielne relacje jeden do wielu mapowania.

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
