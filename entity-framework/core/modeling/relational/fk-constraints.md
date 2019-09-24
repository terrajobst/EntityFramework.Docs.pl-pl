---
title: Ograniczenia klucza obcego — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197069"
---
# <a name="foreign-key-constraints"></a>Ograniczenia klucza obcego

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Ograniczenie klucza obcego jest wprowadzane dla każdej relacji w modelu.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, ograniczenia klucza obcego `FK_<dependent type name>_<principal type name>_<foreign key property name>`są nazywane. W przypadku złożonych kluczy `<foreign key property name>` obcych jest rozdzielana podkreśleniem lista nazw właściwości klucza obcego.

## <a name="data-annotations"></a>Adnotacje danych

Nazw ograniczeń klucza obcego nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można skonfigurować nazwę ograniczenia klucza obcego dla relacji.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
