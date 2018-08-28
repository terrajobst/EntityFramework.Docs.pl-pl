---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995483"
---
# <a name="indexes"></a>Indeksy

Indeksy są typowe pojęcia w wielu magazynach danych. Podczas ich wdrażania w magazynie danych mogą się różnić, są one używane do lepiej wyszukiwań na podstawie kolumny (lub zestaw kolumn) wydajne.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją indeksu jest tworzony w każdej właściwości (lub zestaw właściwości), używany jako klucza obcego.

## <a name="data-annotations"></a>Adnotacje danych

Nie można utworzyć indeksy przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API służy do określenia indeksu w jednej właściwości. Indeksy są domyślnie nie jest unikatowa.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Można również określić czy indeksu powinna być unikatowa, co oznacza, że nie dwie jednostki może mieć tej samej wartości dla danej właściwości.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Można również określić indeks przez więcej niż jedną kolumnę.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Istnieje tylko jeden indeks na odrębnym zestawem właściwości. W przypadku konfigurowania indeksu na zbiór właściwości, które ma już zdefiniowane, indeks przy Konwencji lub poprzedniej konfiguracji, za pomocą interfejsu API Fluent następnie spowoduje zmianę definicji indeksu. Jest to przydatne, jeśli chcesz dodatkowo skonfigurować indeks, który został utworzony przez Konwencję.
