---
title: Dostawca Azure Cosmos DB — ograniczenia — EF Core
description: Ograniczenia dostawcy Azure Cosmos DB Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655987"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="c57b9-103">Ograniczenia dostawcy Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="c57b9-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="c57b9-104">Dostawca Cosmos ma pewne ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c57b9-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="c57b9-105">Wiele z tych ograniczeń jest wynikiem ograniczeń w źródłowym aparacie bazy danych Cosmos i nie są specyficzne dla EF.</span><span class="sxs-lookup"><span data-stu-id="c57b9-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="c57b9-106">Ale większość po prostu [nie została jeszcze zaimplementowana](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="c57b9-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="c57b9-107">Ograniczenia tymczasowe</span><span class="sxs-lookup"><span data-stu-id="c57b9-107">Temporary limitations</span></span>

- <span data-ttu-id="c57b9-108">Nawet jeśli istnieje tylko jeden typ jednostki bez dziedziczenia zamapowanego do kontenera, nadal ma właściwość rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="c57b9-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="c57b9-109">Typy jednostek z kluczami partycji nie działają prawidłowo w niektórych scenariuszach</span><span class="sxs-lookup"><span data-stu-id="c57b9-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="c57b9-110">wywołania `Include` nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="c57b9-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="c57b9-111">wywołania `Join` nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="c57b9-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="c57b9-112">Ograniczenia dotyczące Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="c57b9-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="c57b9-113">Podano tylko metody asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="c57b9-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="c57b9-114">Ponieważ nie istnieją żadne wersje synchronizacji metod niskiego poziomu EF Core opierają się na tym, że odpowiednie funkcje są obecnie implementowane przez wywoływanie `.Wait()` na zwracanym `Task`.</span><span class="sxs-lookup"><span data-stu-id="c57b9-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="c57b9-115">Oznacza to, że użycie metod, takich jak `SaveChanges`, lub `ToList` zamiast ich odpowiedników asynchronicznych może prowadzić do zakleszczenia w aplikacji</span><span class="sxs-lookup"><span data-stu-id="c57b9-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="c57b9-116">Ograniczenia Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c57b9-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="c57b9-117">Aby zapoznać się z pełnym omówieniem [Azure Cosmos DB obsługiwanych funkcji](/azure/cosmos-db/modeling-data), są to najbardziej znaczące różnice w porównaniu do relacyjnej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c57b9-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="c57b9-118">Transakcje inicjowane przez klienta nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="c57b9-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="c57b9-119">Niektóre zapytania między partycjami nie są obsługiwane lub są znacznie wolniejsze w zależności od operatorów</span><span class="sxs-lookup"><span data-stu-id="c57b9-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
