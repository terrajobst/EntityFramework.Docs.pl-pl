---
title: "W tym & Wykluczanie właściwości — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-properties"></a>W tym & Wykluczanie właściwości

W tym właściwości w modelu oznacza, że EF ma metadane dotyczące tej właściwości i podejmie próbę odczytu i zapisu wartości z/do bazy danych.

## <a name="conventions"></a>Konwencje

Według Konwencji właściwości publicznej metody pobierającej i ustawiającej będą uwzględniane w modelu.

## <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do wykluczania właściwość z modelu.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby wykluczyć właściwość z modelu, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
