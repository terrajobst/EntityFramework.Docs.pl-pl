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
# <a name="query-tags"></a><span data-ttu-id="3edc1-102">Tagi kwerendy</span><span class="sxs-lookup"><span data-stu-id="3edc1-102">Query tags</span></span>
> [!NOTE]
> <span data-ttu-id="3edc1-103">Ta funkcja jest nowa w programie EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="3edc1-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="3edc1-104">Funkcja ta ułatwia korelowanie zapytań LINQ w kodzie za pomocą wygenerowanego zapytań SQL przechwycone w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="3edc1-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="3edc1-105">Dodawanie adnotacji do zapytań LINQ, za pomocą nowego `TagWith()` metody:</span><span class="sxs-lookup"><span data-stu-id="3edc1-105">You annotate a LINQ query using the new `TagWith()` method:</span></span> 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="3edc1-106">To zapytanie LINQ jest tłumaczona na następującą instrukcję SQL:</span><span class="sxs-lookup"><span data-stu-id="3edc1-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="3edc1-107">Istnieje możliwość wywołania `TagWith()` wiele razy na tego samego zapytania.</span><span class="sxs-lookup"><span data-stu-id="3edc1-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="3edc1-108">Tagi kwerendy kumulują się.</span><span class="sxs-lookup"><span data-stu-id="3edc1-108">Query tags are cumulative.</span></span>
<span data-ttu-id="3edc1-109">Na przykład biorąc pod uwagę następujące metody:</span><span class="sxs-lookup"><span data-stu-id="3edc1-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="3edc1-110">Następujące zapytanie:</span><span class="sxs-lookup"><span data-stu-id="3edc1-110">The following query:</span></span>   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="3edc1-111">Przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="3edc1-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="3edc1-112">Użytkownik może również używać ciągów wielowierszowe jako tagi zapytania.</span><span class="sxs-lookup"><span data-stu-id="3edc1-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="3edc1-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3edc1-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="3edc1-114">Generuje następujące instrukcje SQL:</span><span class="sxs-lookup"><span data-stu-id="3edc1-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="3edc1-115">Znane ograniczenia</span><span class="sxs-lookup"><span data-stu-id="3edc1-115">Known limitations</span></span>
<span data-ttu-id="3edc1-116">**Tagi kwerendy nie można sparametryzować:** programu EF Core zawsze traktuje tagi kwerendy w zapytaniu LINQ jako literały ciągu, które znajdują się w wygenerowanej tabeli SQL.</span><span class="sxs-lookup"><span data-stu-id="3edc1-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="3edc1-117">Zapytania skompilowane, które przyjmują tagi kwerendy, ponieważ parametry nie są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="3edc1-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>