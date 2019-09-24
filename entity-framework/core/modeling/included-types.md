---
title: Uwzględnienie & z wyjątkiem typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197379"
---
# <a name="including--excluding-types"></a>Uwzględnianie i wykluczanie typów

Uwzględnienie typu w modelu oznacza, że Dr ma metadane dotyczące tego typu i spróbuje odczytywać i zapisywać wystąpienia z/do bazy danych.

## <a name="conventions"></a>Konwencje

Według Konwencji typy, które są dostępne we `DbSet` właściwościach kontekstu, są uwzględniane w modelu. Ponadto są również uwzględniane typy, które są wymienione `OnModelCreating` w metodzie. Na koniec wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów, są również uwzględnione w modelu.

**Na przykład w poniższym kodzie wykryto wszystkie trzy typy:**

* `Blog`ponieważ jest on uwidoczniony we `DbSet` właściwości kontekstu

* `Post`ponieważ jest ono odnajdywane za `Blog.Posts` pośrednictwem właściwości nawigacji

* `AuditEntry`ponieważ jest on wymieniony w`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby wykluczyć typ z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent do wykluczenia typu z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
