---
title: Azure Cosmos DB dostawcy — praca z danymi bez struktury — EF Core
description: Jak korzystać z Azure Cosmos DB danych bez struktury przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417193"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Praca z danymi bez struktury w dostawcy Azure Cosmos DB EF Core

EF Core został zaprojektowany w celu ułatwienia pracy z danymi, które są zgodne ze schematem zdefiniowanym w modelu. Jednak jedną z zalet Azure Cosmos DB jest elastyczność w kształcie przechowywanych danych.

## <a name="accessing-the-raw-json"></a>Uzyskiwanie dostępu do nieprzetworzonego pliku JSON

Istnieje możliwość uzyskania dostępu do właściwości, które nie są śledzone przez EF Core za pośrednictwem specjalnej właściwości w [stanie w tle](../../modeling/shadow-properties.md) o nazwie `"__jObject"` zawierającej `JObject` reprezentujący dane otrzymane ze sklepu i dane, które będą przechowywane:

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
> Właściwość `"__jObject"` jest częścią infrastruktury EF Core i powinna być używana tylko jako Ostatnia z nich, ponieważ może mieć inne zachowanie w przyszłych wydaniach.

> [!NOTE]
> Zmiany w jednostce przesłonią wartości przechowywane w `"__jObject"` podczas `SaveChanges`.

## <a name="using-cosmosclient"></a>Korzystanie z CosmosClient

Aby całkowicie oddzielić EF Core Pobierz obiekt [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , który jest [częścią zestawu Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) z `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Brak wartości właściwości

W poprzednim przykładzie usunęliśmy Właściwość `"TrackingNumber"` z kolejności. Ze względu na to, jak indeks działa w Cosmos DB, zapytania, które odwołują się do brakującej właściwości w innym miejscu niż projekcja, mogą zwrócić nieoczekiwane wyniki. Na przykład:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Sortowane zapytanie faktycznie nie zwraca żadnych wyników. Oznacza to, że należy zachować ostrożność, aby zawsze wypełniał właściwości mapowane przez EF Core podczas bezpośredniego pracy z magazynem.

> [!NOTE]
> Takie zachowanie może ulec zmianie w przyszłych wersjach programu Cosmos. Na przykład jeśli zasady indeksowania definiują indeks złożony {ID/? ASC, TrackingNumber/? ASC)}, a następnie zapytanie zawierające element "ORDER BY c.Id ASC, c. rozróżniacz __ASC" zwróci__ elementy, dla których brakuje właściwości `"TrackingNumber"`.
