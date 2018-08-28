---
title: Właściwości wymagane/opcjonalne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995500"
---
# <a name="required-and-optional-properties"></a>Wymagane i opcjonalne właściwości

Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy, aby mogła zawierać `null`. Jeśli `null` nie jest prawidłową wartością ma być przypisane do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, właściwość, której typ CLR może zawierać wartości null zostanie skonfigurowany jako opcjonalny (`string`, `int?`, `byte[]`itp.). Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowany zgodnie z wymogami (`int`, `decimal`, `bool`itp.).

> [!NOTE]  
> Właściwość, której typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalną. Właściwość będzie zawsze być brana pod uwagę wymagane przez program Entity Framework.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby wskazać, że właściwość jest wymagana.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejs Fluent API

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
