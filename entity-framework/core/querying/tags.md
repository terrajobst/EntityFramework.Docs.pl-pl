---
title: Tagi zapytań — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654831"
---
# <a name="query-tags"></a>Tagi zapytań

> [!NOTE]
> Ta funkcja jest nowa w EF Core 2,2.

Ta funkcja pomaga skorelować zapytania LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwytywanymi w dziennikach.
Dodaj adnotację do zapytania LINQ przy użyciu nowej metody `TagWith()`:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

To zapytanie LINQ jest tłumaczone na następującą instrukcję SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Istnieje możliwość wywołania `TagWith()` wiele razy w tym samym zapytaniu.
Tagi zapytań kumulują się.
Na przykład uwzględniając następujące metody:

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

Tłumaczy na:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Istnieje również możliwość użycia ciągów wielowierszowych jako tagów zapytań.
Na przykład:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Tworzy następujące SQL:

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

**Tagi zapytań nie są można sparametryzować:** EF Core zawsze traktuje znaczniki zapytania w zapytaniu LINQ jako literały ciągów, które są zawarte w wygenerowanym języku SQL.
Skompilowane zapytania, które pobierają Tagi zapytań jako parametry, są niedozwolone.
