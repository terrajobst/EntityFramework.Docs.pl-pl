---
title: Klucze alternatywne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054068"
---
# <a name="alternate-keys"></a>Klucze alternatywne

Klucz alternatywny służy jako alternatywny identyfikator unikatowy dla każdego wystąpienia jednostki, oprócz klucza podstawowego. Klucze alternatywne może służyć jako element docelowy relacji. Korzystanie z relacyjnej bazy danych to mapuje do koncepcji unikatowego indeksu/ograniczenia na alternatywny kolumn kluczy i jeden lub więcej ograniczeń klucza obcego odwołujące się do kolumn na liście.

> [!TIP]  
> Jeśli chcesz wymusić unikatowości kolumny, a następnie chcesz zamiast klucza alternatywnego unikatowego indeksu, zobacz [indeksów](indexes.md). EF klucze alternatywne zapewniają większą funkcjonalność niż unikatowe indeksy ponieważ mogą zostać użyte jako element docelowy klucz obcy.

Klucze alternatywne zwykle są wprowadzane automatycznie w razie potrzeby i nie trzeba ręcznie skonfigurować je. Zobacz [konwencje](#conventions) więcej szczegółów.

## <a name="conventions"></a>Konwencje

Według Konwencji klucza alternatywnego wprowadzono automatycznie podczas określania właściwości, która nie jest kluczem podstawowym jako element docelowy relacji.

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

## <a name="data-annotations"></a>Adnotacji danych

Nie można skonfigurować alternatywne klucze za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować jedną właściwość klucza alternatywnego za można Użyj interfejsu API Fluent.

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

Aby skonfigurować wiele właściwości klucza alternatywnego (znany jako alternatywny klucz złożony) umożliwia także interfejsu API Fluent.

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
