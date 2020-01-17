---
title: Tokeny współbieżności — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124369"
---
# <a name="concurrency-tokens"></a>Tokeny współbieżności

> [!NOTE]
> Ta strona przedstawia sposób konfigurowania tokenów współbieżności. Zobacz temat [obsługa konfliktów współbieżności](../saving/concurrency.md) , aby uzyskać szczegółowy opis sposobu działania kontroli współbieżności na EF Core i Przykłady sposobu obsługi konfliktów współbieżności w aplikacji.

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistycznej kontroli współbieżności.

## <a name="configuration"></a>Konfiguracja

### <a name="data-annotationstabdata-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Sygnatura czasowa/rowversion

Sygnatura czasowa/rowversion jest właściwością, dla której nowa wartość jest automatycznie generowana przez bazę danych przy każdym wstawieniu lub zaktualizowaniu wiersza. Właściwość jest również traktowana jako token współbieżności, co zapewnia, że występuje wyjątek, jeśli aktualizowany wiersz został zmieniony od czasu jego zapytania. Dokładne szczegóły są zależne od używanego dostawcy bazy danych; w przypadku SQL Server Właściwość *Byte []* jest zwykle używana, która zostanie skonfigurowana jako kolumna *rowversion* w bazie danych.

Właściwość można skonfigurować jako sygnaturę czasową/rowversion w następujący sposób:

### <a name="data-annotationstabdata-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
