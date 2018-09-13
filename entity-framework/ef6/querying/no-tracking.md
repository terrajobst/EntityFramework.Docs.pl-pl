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
# <a name="no-tracking-queries"></a>Bez śledzenia zapytań
Czasami chcesz wrócić jednostek w wyniku zapytania, ale nie masz tych jednostek można śledzić w kontekście. Może to spowodować lepszą wydajność podczas wykonywania zapytania dla dużej liczby obiektów w scenariuszach w trybie tylko do odczytu. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

Nowej metody rozszerzenia AsNoTracking umożliwia dowolny typ kwerendy będą uruchamiane w ten sposób. Na przykład:  

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
