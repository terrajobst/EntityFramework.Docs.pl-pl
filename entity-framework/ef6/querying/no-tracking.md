---
title: Zapytania śledzenia nie — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994243"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="13ed1-102">Bez śledzenia zapytań</span><span class="sxs-lookup"><span data-stu-id="13ed1-102">No-Tracking Queries</span></span>
<span data-ttu-id="13ed1-103">Czasami chcesz wrócić jednostek w wyniku zapytania, ale nie masz tych jednostek można śledzić w kontekście.</span><span class="sxs-lookup"><span data-stu-id="13ed1-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="13ed1-104">Może to spowodować lepszą wydajność podczas wykonywania zapytania dla dużej liczby obiektów w scenariuszach w trybie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="13ed1-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="13ed1-105">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="13ed1-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="13ed1-106">Nowej metody rozszerzenia AsNoTracking umożliwia dowolny typ kwerendy będą uruchamiane w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="13ed1-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="13ed1-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="13ed1-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
