---
title: "Dziedziczenie (relacyjnej bazy danych) — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a>Dziedziczenie (relacyjnej bazy danych)

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Dziedziczenie w modelu EF służy do kontrolowania sposobu dziedziczenia klas jednostek jest reprezentowana w bazie danych.

## <a name="conventions"></a>Konwencje

Według Konwencji dziedziczenia zostanie zamapowane przy użyciu wzorca tabeli na hierarchii (TPH). TPH używa pojedynczej tabeli, aby przechowywać dane dla wszystkich typów w hierarchii. Kolumna dyskryminatora służy do identyfikowania typu reprezentuje każdego wiersza.

EF tylko ustawienia dziedziczenia, jeśli dwa lub więcej dziedziczonych typów jawnie znajdują się w modelu (zobacz [dziedziczenia](../inheritance.md) więcej szczegółów).

Poniżej przedstawiono przykładowy scenariusz dziedziczenia proste i dane przechowywane w tabeli relacyjnej bazy danych za pomocą wzorca TPH. *Rozróżniacza* kolumny identyfikuje typu *blogu* są przechowywane w każdym wierszu.

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

## <a name="data-annotations"></a>Adnotacji danych

Za pomocą adnotacji danych nie można skonfigurować dziedziczenia.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować nazwę i typ kolumny rozróżniacza i wartości, które są używane do identyfikowania poszczególnych typów w hierarchii, można użyć interfejsu API Fluent.

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
