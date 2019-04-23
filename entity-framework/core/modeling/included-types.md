---
title: Uwzględnianie i wykluczanie typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929800"
---
# <a name="including--excluding-types"></a>Uwzględnianie i wykluczanie typów

W tym typu w modelu oznacza, że EF ma metadane o typ, który podejmie próbę odczytu i zapisu wystąpienia z i do bazy danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, typy, które są widoczne w `DbSet` właściwości kontekstu znajdują się w modelu. Ponadto, typy, które są wymienione w `OnModelCreating` metody dostępne są również. Ponadto wszystkie typy, które znajdują się przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów znajdują się również w modelu.

**Na przykład w poniższym fragmencie kodu wszystkich trzech typów zostaną wykryte:**

* `Blog` ponieważ jest ona uwidoczniona w `DbSet` właściwości w kontekście

* `Post` ponieważ jest odnalezione `Blog.Posts` właściwość nawigacji

* `AuditEntry` ponieważ są one wymienione w `OnModelCreating`

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

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby wyłączyć typ z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Interfejs Fluent API

Aby wyłączyć typ z modelu, można użyć Fluent API.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
