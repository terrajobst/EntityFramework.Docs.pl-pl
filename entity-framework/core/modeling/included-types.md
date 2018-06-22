---
title: W tym & wykluczanie typów - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054128"
---
# <a name="including--excluding-types"></a>W tym & wykluczanie typów

W tym typu w modelu oznacza, że EF ma metadane dotyczące typu, który będzie podejmować próby odczytu i zapisu wystąpień z/do bazy danych.

## <a name="conventions"></a>Konwencje

Konwencja typy, które są widoczne w `DbSet` właściwości kontekstu znajdują się w modelu. Ponadto typy, które są wymienione w `OnModelCreating` uwzględniane są również metody. Na koniec żadnych typów, które zostały znalezione przez cyklicznie badać właściwości nawigacji wykrytych typów znajdują się również w modelu.

**Na przykład w poniższym fragmencie kodu odnalezieniu wszystkich trzech typów:**

* `Blog`ponieważ jest widoczna w `DbSet` właściwości w tym kontekście

* `Post`ponieważ okaże się za pośrednictwem `Blog.Posts` właściwości nawigacji

* `AuditEntry`ponieważ jest wymieniony w`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>Adnotacji danych

Aby wyłączyć typ z modelu, można użyć adnotacji danych.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby wyłączyć typ z modelu, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
