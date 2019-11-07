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
# <a name="query-tags"></a><span data-ttu-id="2885a-102">Tagi zapytań</span><span class="sxs-lookup"><span data-stu-id="2885a-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="2885a-103">Ta funkcja jest nowa w EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="2885a-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="2885a-104">Ta funkcja pomaga skorelować zapytania LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwytywanymi w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="2885a-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="2885a-105">Dodaj adnotację do zapytania LINQ przy użyciu nowej metody `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="2885a-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="2885a-106">To zapytanie LINQ jest tłumaczone na następującą instrukcję SQL:</span><span class="sxs-lookup"><span data-stu-id="2885a-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="2885a-107">Istnieje możliwość wywołania `TagWith()` wiele razy w tym samym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="2885a-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="2885a-108">Tagi zapytań kumulują się.</span><span class="sxs-lookup"><span data-stu-id="2885a-108">Query tags are cumulative.</span></span>
<span data-ttu-id="2885a-109">Na przykład uwzględniając następujące metody:</span><span class="sxs-lookup"><span data-stu-id="2885a-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="2885a-110">Następujące zapytanie:</span><span class="sxs-lookup"><span data-stu-id="2885a-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="2885a-111">Tłumaczy na:</span><span class="sxs-lookup"><span data-stu-id="2885a-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="2885a-112">Istnieje również możliwość użycia ciągów wielowierszowych jako tagów zapytań.</span><span class="sxs-lookup"><span data-stu-id="2885a-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="2885a-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2885a-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="2885a-114">Tworzy następujące SQL:</span><span class="sxs-lookup"><span data-stu-id="2885a-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="2885a-115">Znane ograniczenia</span><span class="sxs-lookup"><span data-stu-id="2885a-115">Known limitations</span></span>

<span data-ttu-id="2885a-116">**Tagi zapytań nie są można sparametryzować:** EF Core zawsze traktuje znaczniki zapytania w zapytaniu LINQ jako literały ciągów, które są zawarte w wygenerowanym języku SQL.</span><span class="sxs-lookup"><span data-stu-id="2885a-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="2885a-117">Skompilowane zapytania, które pobierają Tagi zapytań jako parametry, są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="2885a-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
