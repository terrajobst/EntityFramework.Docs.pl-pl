---
title: Dostawca Azure Cosmos DB — ograniczenia — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150810"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="d6f48-102">Ograniczenia dostawcy Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="d6f48-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="d6f48-103">Dostawca Cosmos ma pewne ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="d6f48-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="d6f48-104">Wiele z tych ograniczeń jest wynikiem ograniczeń w źródłowym aparacie bazy danych Cosmos i nie są specyficzne dla EF.</span><span class="sxs-lookup"><span data-stu-id="d6f48-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="d6f48-105">Ale większość po prostu [nie została jeszcze zaimplementowana](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="d6f48-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="d6f48-106">Ograniczenia tymczasowe</span><span class="sxs-lookup"><span data-stu-id="d6f48-106">Temporary limitations</span></span>

- <span data-ttu-id="d6f48-107">Nawet jeśli istnieje tylko jeden typ jednostki bez dziedziczenia zamapowanego do kontenera, nadal ma właściwość rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="d6f48-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="d6f48-108">Typy jednostek z kluczami partycji nie działają prawidłowo w niektórych scenariuszach</span><span class="sxs-lookup"><span data-stu-id="d6f48-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="d6f48-109">`Include`wywołania nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="d6f48-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="d6f48-110">`Join`wywołania nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="d6f48-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="d6f48-111">Ograniczenia dotyczące Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="d6f48-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="d6f48-112">Podano tylko metody asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="d6f48-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="d6f48-113">Ponieważ nie istnieją żadne wersje synchronizacji metod niskiego poziomu, EF Core opierają się na tym, że odpowiednie funkcje są obecnie implementowane przez `.Wait()` wywołanie `Task`zwróconych wartości.</span><span class="sxs-lookup"><span data-stu-id="d6f48-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="d6f48-114">Oznacza to, że użycie metod `SaveChanges`takich jak `ToList` , lub zamiast ich odpowiedników asynchronicznych może prowadzić do zakleszczenia w aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6f48-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="d6f48-115">Ograniczenia Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d6f48-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="d6f48-116">Aby zapoznać się z pełnym omówieniem [Azure Cosmos DB obsługiwanych funkcji](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), są to najbardziej znaczące różnice w porównaniu do relacyjnej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d6f48-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="d6f48-117">Transakcje inicjowane przez klienta nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="d6f48-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="d6f48-118">Niektóre zapytania między partycjami nie są obsługiwane lub są znacznie wolniejsze w zależności od operatorów</span><span class="sxs-lookup"><span data-stu-id="d6f48-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>