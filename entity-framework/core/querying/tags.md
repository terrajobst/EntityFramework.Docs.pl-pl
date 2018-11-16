---
title: Tagi zapytania — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688805"
---
# <a name="query-tags"></a>Tagi kwerendy
> [!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.2.

Funkcja ta ułatwia korelowanie zapytań LINQ w kodzie za pomocą wygenerowanego zapytań SQL przechwycone w dziennikach.
Dodawanie adnotacji do zapytań LINQ, za pomocą nowego `TagWith()` metody: 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

To zapytanie LINQ jest tłumaczona na następującą instrukcję SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Istnieje możliwość wywołania `TagWith()` wiele razy na tego samego zapytania.
Tagi kwerendy kumulują się.
Na przykład biorąc pod uwagę następujące metody:

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

Następujące zapytanie:   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

Przekłada się na:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Użytkownik może również używać ciągów wielowierszowe jako tagi zapytania.
Na przykład:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Generuje następujące instrukcje SQL:

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>Znane ograniczenia
**Tagi kwerendy nie można sparametryzować:** programu EF Core zawsze traktuje tagi kwerendy w zapytaniu LINQ jako literały ciągu, które znajdują się w wygenerowanej tabeli SQL.
Zapytania skompilowane, które przyjmują tagi kwerendy, ponieważ parametry nie są dozwolone.