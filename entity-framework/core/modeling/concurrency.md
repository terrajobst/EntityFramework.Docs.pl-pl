---
title: Tokeny współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994229"
---
# <a name="concurrency-tokens"></a>Tokeny współbieżności

> [!NOTE]
> Ta strona dokumenty, jak skonfigurować tokeny współbieżności. Zobacz [Obsługa konfliktów współbieżności](../saving/concurrency.md) uzyskać szczegółowy opis sposobu działania kontroli współbieżności na programu EF Core i przykłady sposób obsługi konfliktów współbieżności w aplikacji.

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania kontroli optymistycznej współbieżności.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwości nigdy nie są skonfigurowane jako tokeny współbieżności.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Wersja sygnatury czasowej/wiersza

Sygnatura czasowa jest właściwością, gdzie nowa wartość jest generowany przez bazę danych, za każdym razem, gdy wstawieniu lub zaktualizowaniu wiersza. Właściwość jest również traktowane jako tokenem współbieżności. Dzięki temu uzyskasz wyjątek, jeśli osobę zmodyfikował wiersza, który próbujesz zaktualizować, ponieważ zapytania dla danych.

Jak odbywa się to zależy od używanego dostawcy bazy danych. Dla programu SQL Server sygnatura czasowa jest zazwyczaj używany na *byte []* właściwość, która będzie można skonfigurować jako *ROWVERSION* kolumny w bazie danych.

### <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwości nigdy nie są skonfigurowane jako sygnatur czasowych.

### <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować właściwości jako sygnaturę czasową.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie właściwości jako sygnaturę czasową.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
