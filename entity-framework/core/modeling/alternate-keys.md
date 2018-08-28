---
title: Klucze alternatywne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996974"
---
# <a name="alternate-keys"></a>Klucze alternatywne

Klucza alternatywnego służy jako alternatywne Unikatowy identyfikator dla każdego wystąpienia jednostki, oprócz klucz podstawowy. Klucze alternatywne może służyć jako element docelowy relacji. Przy użyciu relacyjnej bazy danych to mapuje do koncepcji unikatowego indeksu/ograniczenia na alternatywny kolumn kluczy i jeden lub więcej ograniczeń klucza obcego odwołujące się do kolumn na liście.

> [!TIP]  
> Jeśli chcesz wymusić unikatowość kolumny, a następnie chcesz, aby zamiast klucza alternatywnego unikatowego indeksu, zobacz [indeksów](indexes.md). W programie EF klucze alternatywne zapewnić większą funkcjonalność niż indeksy unikatowe, ponieważ może służyć jako cel klucza obcego.

Klucze alternatywne są zwykle wprowadzane dla Ciebie w razie i nie trzeba ręcznie skonfigurować je. Zobacz [konwencje](#conventions) Aby uzyskać więcej informacji.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją klucza alternatywnego został wprowadzony dla Ciebie podczas określania właściwości, który nie jest kluczem podstawowym jako element docelowy relacji.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Adnotacje danych

Klucze alternatywne nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie jednej właściwości klucza alternatywnego.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Można również skonfigurować wiele właściwości jako klucza alternatywnego (znanych jako alternatywne klucz złożony) za pomocą Fluent interfejsu API.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
