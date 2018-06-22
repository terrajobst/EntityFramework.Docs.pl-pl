---
title: Tokeny współbieżności - EF Core
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745492"
---
# <a name="concurrency-tokens"></a>Tokeny współbieżności

> [!NOTE]
> Ta strona dokumenty, jak skonfigurować tokeny współbieżności. Zobacz [obsługi konfliktom współbieżności](../saving/concurrency.md) szczegółowy opis działania formant współbieżności na podstawowe EF i przykłady sposobu obsługi konfliktom współbieżności w aplikacji.

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistyczne sterowanie współbieżnością.

## <a name="conventions"></a>Konwencje

Według Konwencji właściwości nigdy nie są skonfigurowane jako tokeny współbieżności.

## <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do konfigurowania właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Interfejsu API Fluent

Interfejsu API Fluent umożliwia skonfigurowanie właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Wersja wiersza/znacznik czasu

Sygnatura czasowa jest właściwością, gdzie nowa wartość jest generowany przez bazę danych, za każdym razem, gdy wiersz jest wstawiane lub aktualizowane. Właściwości są także traktowane jako tokenem współbieżności. Dzięki temu otrzymasz wyjątek, jeśli osobom został zmodyfikowany na wiersz, który próbujesz zaktualizować, ponieważ zapytanie dotyczące danych.

Jak jest to osiągane jest używany dostawca bazy danych. Dla programu SQL Server sygnatury czasowej jest zwykle używany w *byte []* właściwość, która będzie można skonfigurować jako *ROWVERSION* kolumny w bazie danych.

### <a name="conventions"></a>Konwencje

Według Konwencji właściwości nigdy nie są skonfigurowane jako sygnatur czasowych.

### <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do konfigurowania właściwości jako sygnaturę czasową.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Interfejsu API Fluent

Do skonfigurowania właściwości jako sygnaturę czasową, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
