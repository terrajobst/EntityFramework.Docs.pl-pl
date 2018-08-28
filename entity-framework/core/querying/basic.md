---
title: Podstawowe zapytania — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 6a381f419cb0958ea0835070e22fe7a3212457d7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993708"
---
# <a name="basic-queries"></a><span data-ttu-id="e6a37-102">Podstawowe zapytania</span><span class="sxs-lookup"><span data-stu-id="e6a37-102">Basic Queries</span></span>

<span data-ttu-id="e6a37-103">Dowiedz się, jak można załadować jednostek z bazy danych przy użyciu Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="e6a37-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="e6a37-104">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e6a37-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="e6a37-105">101 przykładów LINQ</span><span class="sxs-lookup"><span data-stu-id="e6a37-105">101 LINQ samples</span></span>

<span data-ttu-id="e6a37-106">Ta strona zawiera kilka przykładów, aby osiągnąć typowych zadań przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e6a37-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="e6a37-107">Aby uzyskać obszerny zestaw przykładów pokazujący, co jest możliwe za pomocą LINQ, zobacz [101 przykładów LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="e6a37-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="e6a37-108">Podczas ładowania wszystkie dane</span><span class="sxs-lookup"><span data-stu-id="e6a37-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="e6a37-109">Trwa ładowanie pojedynczej jednostki</span><span class="sxs-lookup"><span data-stu-id="e6a37-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="e6a37-110">Filtrowanie</span><span class="sxs-lookup"><span data-stu-id="e6a37-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
