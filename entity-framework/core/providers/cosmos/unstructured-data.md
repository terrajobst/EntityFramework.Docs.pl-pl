---
title: Azure Cosmos DB dostawcy — praca z danymi bez struktury — EF Core
description: Jak korzystać z Azure Cosmos DB danych bez struktury przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 0bfccbfd3af6e209967004752b5a3947d644544b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655517"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="97b8c-103">Praca z danymi bez struktury w dostawcy Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="97b8c-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="97b8c-104">EF Core został zaprojektowany w celu ułatwienia pracy z danymi, które są zgodne ze schematem zdefiniowanym w modelu.</span><span class="sxs-lookup"><span data-stu-id="97b8c-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="97b8c-105">Jednak jedną z zalet Azure Cosmos DB jest elastyczność w kształcie przechowywanych danych.</span><span class="sxs-lookup"><span data-stu-id="97b8c-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="97b8c-106">Uzyskiwanie dostępu do nieprzetworzonego pliku JSON</span><span class="sxs-lookup"><span data-stu-id="97b8c-106">Accessing the raw JSON</span></span>

<span data-ttu-id="97b8c-107">Istnieje możliwość uzyskania dostępu do właściwości, które nie są śledzone przez EF Core za pośrednictwem specjalnej właściwości w [stanie w tle](../../modeling/shadow-properties.md) o nazwie `"__jObject"` zawierającej `JObject` reprezentujący dane otrzymane ze sklepu i dane, które będą przechowywane:</span><span class="sxs-lookup"><span data-stu-id="97b8c-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
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
> <span data-ttu-id="97b8c-108">Właściwość `"__jObject"` jest częścią infrastruktury EF Core i powinna być używana tylko jako Ostatnia z nich, ponieważ może mieć inne zachowanie w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="97b8c-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="97b8c-109">Zmiany w jednostce przesłonią wartości przechowywane w `"__jObject"` podczas `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="97b8c-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="97b8c-110">Korzystanie z CosmosClient</span><span class="sxs-lookup"><span data-stu-id="97b8c-110">Using CosmosClient</span></span>

<span data-ttu-id="97b8c-111">Aby całkowicie oddzielić EF Core Pobierz obiekt [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , który jest [częścią zestawu Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) z `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="97b8c-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="97b8c-112">Brak wartości właściwości</span><span class="sxs-lookup"><span data-stu-id="97b8c-112">Missing property values</span></span>

<span data-ttu-id="97b8c-113">W poprzednim przykładzie usunęliśmy Właściwość `"TrackingNumber"` z kolejności.</span><span class="sxs-lookup"><span data-stu-id="97b8c-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="97b8c-114">Ze względu na to, jak indeks działa w Cosmos DB, zapytania, które odwołują się do brakującej właściwości w innym miejscu niż projekcja, mogą zwrócić nieoczekiwane wyniki.</span><span class="sxs-lookup"><span data-stu-id="97b8c-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="97b8c-115">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="97b8c-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="97b8c-116">Sortowane zapytanie faktycznie nie zwraca żadnych wyników.</span><span class="sxs-lookup"><span data-stu-id="97b8c-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="97b8c-117">Oznacza to, że należy zachować ostrożność, aby zawsze wypełniał właściwości mapowane przez EF Core podczas bezpośredniego pracy z magazynem.</span><span class="sxs-lookup"><span data-stu-id="97b8c-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="97b8c-118">Takie zachowanie może ulec zmianie w przyszłych wersjach programu Cosmos.</span><span class="sxs-lookup"><span data-stu-id="97b8c-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="97b8c-119">Na przykład jeśli zasady indeksowania definiują indeks złożony {ID/?</span><span class="sxs-lookup"><span data-stu-id="97b8c-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="97b8c-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="97b8c-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="97b8c-121">ASC)}, a następnie zapytanie zawiera element "ORDER BY c.Id ASC, c. rozróżniacz __ASC" zwróci__ elementy, dla których brakuje właściwości `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="97b8c-121">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
