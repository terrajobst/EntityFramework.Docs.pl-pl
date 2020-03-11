---
title: Zapytania bez śledzenia — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417103"
---
# <a name="no-tracking-queries"></a>Zapytania bez śledzenia
Czasami warto pobrać jednostki z zapytania, ale nie mają one być śledzone przez kontekst. Może to skutkować lepszą wydajnością podczas wykonywania zapytań dotyczących dużej liczby jednostek w scenariuszach tylko do odczytu. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

Nowa metoda rozszerzająca AsNoTracking umożliwia uruchamianie dowolnego zapytania w ten sposób. Na przykład:  

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
