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
