---
title: "Podstawowe zapytania — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a><span data-ttu-id="ae291-102">Zapytania podstawowego</span><span class="sxs-lookup"><span data-stu-id="ae291-102">Basic Queries</span></span>

<span data-ttu-id="ae291-103">Dowiedz się, jak załadować jednostek z bazy danych przy użyciu języka integracji zapytania (LINQ).</span><span class="sxs-lookup"><span data-stu-id="ae291-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="ae291-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ae291-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="ae291-105">101 przykłady interfejsów LINQ</span><span class="sxs-lookup"><span data-stu-id="ae291-105">101 LINQ samples</span></span>

<span data-ttu-id="ae291-106">Ta strona zawiera kilka przykładów w celu osiągnięcia typowe zadania związane z programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ae291-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="ae291-107">Aby rozbudowany zestaw przykładów przedstawiający, co jest możliwe za pomocą LINQ, zobacz [101 przykłady interfejsów LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="ae291-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="ae291-108">Ładowania wszystkich danych</span><span class="sxs-lookup"><span data-stu-id="ae291-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="ae291-109">Ładowanie pojedynczej jednostki</span><span class="sxs-lookup"><span data-stu-id="ae291-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="ae291-110">Filtrowanie</span><span class="sxs-lookup"><span data-stu-id="ae291-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
