---
title: Dziedziczenie (relacyjna baza danych) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2aaceb05bbc1b0eb5c116b3dc1fb33c90c115a70
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419682"
---
# <a name="inheritance-relational-database"></a>Dziedziczenie (relacyjna baza danych)

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Dziedziczenie w modelu platformy EF jest używane do kontrolowania, jak dziedziczenia klas jednostek jest reprezentowana w bazie danych.

> [!NOTE]  
> Obecnie tylko tabela wg hierarchii (TPH) ze wzorcem jest zaimplementowana w wersji EF Core. Innych typowych wzorców, takich jak tabela wg typu (TPT), a tabela konkretny-wg typu (TPC) nie są jeszcze dostępne.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją dziedziczenia zostanie zamapowane przy użyciu wzorca Tabela wg hierarchii (TPH). TPH używa jednej tabeli do przechowywania danych dla wszystkich typów w hierarchii. Kolumna dyskryminatora jest używany do identyfikowania typu każdy wiersz reprezentuje.

EF Core tylko skonfigurować dziedziczenie, jeśli co najmniej dwa dziedziczone typy są jawnie uwzględnione w modelu (zobacz [dziedziczenia](../inheritance.md) Aby uzyskać więcej informacji).

Poniżej przedstawiono przykład przedstawiający Scenariusz proste dziedziczenie i dane przechowywane w tabeli relacyjnej bazy danych za pomocą wzorca TPH. *Dyskryminatora* kolumna określa, jakiego typu *blogu* są przechowywane w każdym wierszu.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> Korzystając z mapowania TPH colmmns bazy danych zostaną zastosowane automatycznie dopuszczającego wartość null, zgodnie z potrzebami.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych, aby skonfigurować dziedziczenie.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie nazwy i typu kolumna dyskryminatora i wartości, które są używane do identyfikowania poszczególnych typów w hierarchii.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Konfigurowanie właściwość rozróżniacza

W powyższych przykładach dyskryminatora jest tworzona jako [w tle właściwość](xref:core/modeling/shadow-properties) na podstawowej jednostce w hierarchii. Ponieważ właściwości w modelu, można skonfigurować tak jak inne właściwości. Na przykład, aby ustawić maksymalną długość, gdy jest używany domyślnie przez Konwencję dyskryminatora:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Rozróżniacza również mogą być mapowane z właściwością rzeczywiste CLR w jednostce. Na przykład:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Łącząc te dwie rzeczy ze sobą istnieje możliwość zarówno mapy rozróżniacza z właściwością rzeczywistym i skonfiguruj go tak:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
