---
title: Zapytania śledzenia nie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490128"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="25463-102">Bez śledzenia zapytań</span><span class="sxs-lookup"><span data-stu-id="25463-102">No-Tracking Queries</span></span>
<span data-ttu-id="25463-103">Czasami chcesz wrócić jednostek w wyniku zapytania, ale nie masz tych jednostek można śledzić w kontekście.</span><span class="sxs-lookup"><span data-stu-id="25463-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="25463-104">Może to spowodować lepszą wydajność podczas wykonywania zapytania dla dużej liczby obiektów w scenariuszach w trybie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="25463-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="25463-105">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="25463-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="25463-106">Nowej metody rozszerzenia AsNoTracking umożliwia dowolny typ kwerendy będą uruchamiane w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="25463-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="25463-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="25463-107">For example:</span></span>  

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
