---
title: Indeksy — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054077"
---
# <a name="indexes"></a>Indeksy

Indeksy są wspólnej definicji w wielu sklepach danych. Podczas wdrażania ich w magazynie danych mogą się różnić, są one używane do lepiej wyszukiwań na podstawie kolumny (lub zestaw kolumn) skuteczne.

## <a name="conventions"></a>Konwencje

Konwencja indeks jest tworzony w każdej właściwości (lub zbiór właściwości), używanych jako klucz obcy.

## <a name="data-annotations"></a>Adnotacji danych

Nie można utworzyć indeksy za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Interfejsu API Fluent służy do określenia indeksu dla jednej właściwości. Domyślnie indeksy są unikatowe.

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

Można również określić indeksu przez więcej niż jedną kolumnę.

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
> Istnieje tylko jeden indeks na osobny zestaw właściwości. Jeśli Konfigurowanie indeksu na zbiór właściwości, które jest już zdefiniowana, indeksu przez Konwencję lub poprzednią konfigurację za pomocą interfejsu API Fluent następnie zmienia się definicji tego indeksu. Jest to przydatne, jeśli chcesz kontynuować konfigurowanie indeksu, który został utworzony przez Konwencję.
