---
title: Azure Cosmos DB dostawcy — praca z danymi bez struktury — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150817"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="f2aa1-102">Praca z danymi bez struktury w dostawcy Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="f2aa1-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="f2aa1-103">EF Core został zaprojektowany w celu ułatwienia pracy z danymi, które są zgodne ze schematem zdefiniowanym w modelu.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="f2aa1-104">Jednak jedną z zalet Azure Cosmos DB jest elastyczność w kształcie przechowywanych danych.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="f2aa1-105">Uzyskiwanie dostępu do nieprzetworzonego pliku JSON</span><span class="sxs-lookup"><span data-stu-id="f2aa1-105">Accessing the raw JSON</span></span>

<span data-ttu-id="f2aa1-106">Istnieje możliwość uzyskania dostępu do właściwości, które nie są śledzone przez EF Core za pomocą specjalnej właściwości w [stanie](../../modeling/shadow-properties.md) w tle `"__jObject"` o nazwie, `JObject` która zawiera dane otrzymane ze sklepu i dane, które będą przechowywane:</span><span class="sxs-lookup"><span data-stu-id="f2aa1-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="f2aa1-107">`"__jObject"` Właściwość jest częścią infrastruktury EF Core i powinna być używana tylko jako Ostatnia z nich, ponieważ może mieć inne zachowanie w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="f2aa1-108">Zmiany w jednostce przesłonią wartości przechowywane w `"__jObject"` trakcie. `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="f2aa1-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="f2aa1-109">Korzystanie z CosmosClient</span><span class="sxs-lookup"><span data-stu-id="f2aa1-109">Using CosmosClient</span></span>

<span data-ttu-id="f2aa1-110">Aby całkowicie oddzielić od EF Core pobrać `CosmosClient` obiekt, który jest [częścią zestawu Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) z `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="f2aa1-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="f2aa1-111">Brak wartości właściwości</span><span class="sxs-lookup"><span data-stu-id="f2aa1-111">Missing property values</span></span>

<span data-ttu-id="f2aa1-112">W poprzednim przykładzie usunięto `"TrackingNumber"` właściwość z kolejności.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="f2aa1-113">Ze względu na to, jak indeks działa w Cosmos DB, zapytania, które odwołują się do brakującej właściwości w innym miejscu niż projekcja, mogą zwrócić nieoczekiwane wyniki.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="f2aa1-114">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f2aa1-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="f2aa1-115">Sortowane zapytanie faktycznie nie zwraca żadnych wyników.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="f2aa1-116">Oznacza to, że należy zachować ostrożność, aby zawsze wypełniał właściwości mapowane przez EF Core podczas bezpośredniego pracy z magazynem.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="f2aa1-117">Takie zachowanie może ulec zmianie w przyszłych wersjach programu Cosmos.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="f2aa1-118">Na przykład jeśli zasady indeksowania definiują indeks złożony {ID/?</span><span class="sxs-lookup"><span data-stu-id="f2aa1-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="f2aa1-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="f2aa1-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="f2aa1-120">ASC)}, a następnie zapytanie zawiera element "Order by c.ID ASC, c. rozróżniacz __ASC" zwróci__ elementy, dla `"TrackingNumber"` których brakuje właściwości.</span><span class="sxs-lookup"><span data-stu-id="f2aa1-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>