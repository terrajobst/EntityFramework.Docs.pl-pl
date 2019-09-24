---
title: Klucze alternatywne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197472"
---
# <a name="alternate-keys"></a>Klucze alternatywne

Alternatywny klucz służy jako alternatywny unikatowy identyfikator dla każdego wystąpienia jednostki oprócz klucza podstawowego. Klucze alternatywne mogą służyć jako element docelowy relacji. W przypadku korzystania z relacyjnej bazy danych mapowanie do koncepcji unikatowego indeksu/ograniczenia w kolumnach klucza alternatywnego oraz co najmniej jedno ograniczenie klucza obcego odwołujące się do kolumn.

> [!TIP]  
> Jeśli chcesz, aby wymusić unikatowość kolumny, a następnie chcesz użyć indeksu unikatowego, a nie klucza alternatywnego, zobacz [indeksy](indexes.md). W EF klucze alternatywne zapewniają większą funkcjonalność niż indeksy unikatowe, ponieważ mogą być używane jako obiekty docelowe klucza obcego.

Klucze alternatywne są zwykle wprowadzane w razie potrzeby i nie trzeba ich ręcznie konfigurować. Aby uzyskać więcej informacji, zobacz [konwencje](#conventions) .

## <a name="conventions"></a>Konwencje

Przy użyciu konwencji klucz alternatywny jest wprowadzany w przypadku identyfikowania właściwości, która nie jest kluczem podstawowym jako elementu docelowego relacji.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

Nie można skonfigurować kluczy alternatywnych przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować pojedynczą właściwość jako klucz alternatywny.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz alternatywny (nazywany kluczem alternatywnym).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
