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
# <a name="required-and-optional-properties"></a><span data-ttu-id="5f882-102">Wymagane i opcjonalne właściwości</span><span class="sxs-lookup"><span data-stu-id="5f882-102">Required and Optional Properties</span></span>

<span data-ttu-id="5f882-103">Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy, aby mogła zawierać `null`.</span><span class="sxs-lookup"><span data-stu-id="5f882-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="5f882-104">Jeśli `null` nie jest prawidłową wartością ma być przypisane do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.</span><span class="sxs-lookup"><span data-stu-id="5f882-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="5f882-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="5f882-105">Conventions</span></span>

<span data-ttu-id="5f882-106">Zgodnie z Konwencją, właściwość, której typ CLR może zawierać wartości null zostanie skonfigurowany jako opcjonalny (`string`, `int?`, `byte[]`itp.).</span><span class="sxs-lookup"><span data-stu-id="5f882-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="5f882-107">Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowany zgodnie z wymogami (`int`, `decimal`, `bool`itp.).</span><span class="sxs-lookup"><span data-stu-id="5f882-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="5f882-108">Właściwość, której typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalną.</span><span class="sxs-lookup"><span data-stu-id="5f882-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="5f882-109">Właściwość będzie zawsze być brana pod uwagę wymagane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f882-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5f882-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="5f882-110">Data Annotations</span></span>

<span data-ttu-id="5f882-111">Korzystanie z adnotacji danych, aby wskazać, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="5f882-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="5f882-112">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="5f882-112">Fluent API</span></span>

<span data-ttu-id="5f882-113">Aby wskazać, że właściwość jest wymagana, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="5f882-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
