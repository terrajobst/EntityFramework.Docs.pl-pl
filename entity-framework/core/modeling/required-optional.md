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
# <a name="required-and-optional-properties"></a><span data-ttu-id="23757-102">Właściwości wymaganych i opcjonalnych</span><span class="sxs-lookup"><span data-stu-id="23757-102">Required and Optional Properties</span></span>

<span data-ttu-id="23757-103">Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy dla on zawierać `null`.</span><span class="sxs-lookup"><span data-stu-id="23757-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="23757-104">Jeśli `null` nie jest prawidłową wartość do przypisania do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.</span><span class="sxs-lookup"><span data-stu-id="23757-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="23757-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="23757-105">Conventions</span></span>

<span data-ttu-id="23757-106">Konwencja, właściwości, których typ CLR może zawierać wartości null zostanie skonfigurowana jako opcjonalny (`string`, `int?`, `byte[]`itp.).</span><span class="sxs-lookup"><span data-stu-id="23757-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="23757-107">Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowana jako wymagany (`int`, `decimal`, `bool`itp.).</span><span class="sxs-lookup"><span data-stu-id="23757-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="23757-108">Właściwości, których typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="23757-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="23757-109">Właściwość będą zawsze uznawane za wymagane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="23757-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="23757-110">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="23757-110">Data Annotations</span></span>

<span data-ttu-id="23757-111">Aby wskazać, że właściwość jest wymagana, można użyć adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="23757-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="23757-112">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="23757-112">Fluent API</span></span>

<span data-ttu-id="23757-113">Aby wskazać, że właściwość jest wymagana, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="23757-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
