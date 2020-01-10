---
title: Tokeny współbieżności — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781147"
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

[! code-CSharp [Main] (.. /.. /.. /samples/core/Modeling/FluentAPI/Timestamp.cs? Name = timestamp & Podświetl = 9, 17]

***
