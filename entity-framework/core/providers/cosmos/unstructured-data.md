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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Praca z danymi bez struktury w dostawcy Azure Cosmos DB EF Core

EF Core został zaprojektowany w celu ułatwienia pracy z danymi, które są zgodne ze schematem zdefiniowanym w modelu. Jednak jedną z zalet Azure Cosmos DB jest elastyczność w kształcie przechowywanych danych.

## <a name="accessing-the-raw-json"></a>Uzyskiwanie dostępu do nieprzetworzonego pliku JSON

Istnieje możliwość uzyskania dostępu do właściwości, które nie są śledzone przez EF Core za pomocą specjalnej właściwości w [stanie](../../modeling/shadow-properties.md) w tle `"__jObject"` o nazwie, `JObject` która zawiera dane otrzymane ze sklepu i dane, które będą przechowywane:

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
> `"__jObject"` Właściwość jest częścią infrastruktury EF Core i powinna być używana tylko jako Ostatnia z nich, ponieważ może mieć inne zachowanie w przyszłych wydaniach.

> [!NOTE]
> Zmiany w jednostce przesłonią wartości przechowywane w `"__jObject"` trakcie. `SaveChanges`

## <a name="using-cosmosclient"></a>Korzystanie z CosmosClient

Aby całkowicie oddzielić od EF Core pobrać `CosmosClient` obiekt, który jest [częścią zestawu Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) z `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Brak wartości właściwości

W poprzednim przykładzie usunięto `"TrackingNumber"` właściwość z kolejności. Ze względu na to, jak indeks działa w Cosmos DB, zapytania, które odwołują się do brakującej właściwości w innym miejscu niż projekcja, mogą zwrócić nieoczekiwane wyniki. Na przykład:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Sortowane zapytanie faktycznie nie zwraca żadnych wyników. Oznacza to, że należy zachować ostrożność, aby zawsze wypełniał właściwości mapowane przez EF Core podczas bezpośredniego pracy z magazynem.

> [!NOTE]
> Takie zachowanie może ulec zmianie w przyszłych wersjach programu Cosmos. Na przykład jeśli zasady indeksowania definiują indeks złożony {ID/? ASC, TrackingNumber/? ASC)}, a następnie zapytanie zawiera element "Order by c.ID ASC, c. rozróżniacz __ASC" zwróci__ elementy, dla `"TrackingNumber"` których brakuje właściwości.