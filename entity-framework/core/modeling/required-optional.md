---
title: Właściwości wymaganych/opcjonalnych - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054251"
---
# <a name="required-and-optional-properties"></a>Właściwości wymaganych i opcjonalnych

Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy dla on zawierać `null`. Jeśli `null` nie jest prawidłową wartość do przypisania do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.

## <a name="conventions"></a>Konwencje

Konwencja, właściwości, których typ CLR może zawierać wartości null zostanie skonfigurowana jako opcjonalny (`string`, `int?`, `byte[]`itp.). Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowana jako wymagany (`int`, `decimal`, `bool`itp.).

> [!NOTE]  
> Właściwości, których typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalny. Właściwość będą zawsze uznawane za wymagane przez program Entity Framework.

## <a name="data-annotations"></a>Adnotacji danych

Aby wskazać, że właściwość jest wymagana, można użyć adnotacji danych.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby wskazać, że właściwość jest wymagana, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
