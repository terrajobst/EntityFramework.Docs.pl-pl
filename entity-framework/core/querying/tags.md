---
title: Tagi zapytań — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417891"
---
# <a name="query-tags"></a>Znaczniki zapytań

> [!NOTE]
> Ta funkcja jest nowa w EF Core 2.2.

Ta funkcja pomaga skorelować zapytania LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwyconymi w dziennikach.
Adnotacje kwerendy LINQ przy `TagWith()` użyciu nowej metody:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Ta kwerenda LINQ jest tłumaczona na następującą instrukcję SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Istnieje możliwość wywołania `TagWith()` wiele razy na tej samej kwerendzie.
Tagi zapytań są kumulowane.
Na przykład, biorąc pod uwagę następujące metody:

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

Istnieje również możliwość używania ciągów wielowierszowych jako tagów zapytań.
Przykład:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Tworzy następujący sql:

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

**Znaczniki zapytań nie są parametryzowane:** EF Core zawsze traktuje tagi zapytań w kwerendzie LINQ jako literały ciągów, które są zawarte w wygenerowanym języku SQL.
Skompilowane kwerendy, które przyjmują tagi zapytań, ponieważ parametry nie są dozwolone.
