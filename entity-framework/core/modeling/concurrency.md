---
title: Tokeny współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197456"
---
# <a name="concurrency-tokens"></a>Tokeny współbieżności

> [!NOTE]
> Ta strona przedstawia sposób konfigurowania tokenów współbieżności. Zobacz temat [obsługa konfliktów współbieżności](../saving/concurrency.md) , aby uzyskać szczegółowy opis sposobu działania kontroli współbieżności na EF Core i Przykłady sposobu obsługi konfliktów współbieżności w aplikacji.

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistycznej kontroli współbieżności.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwości nigdy nie są konfigurowane jako tokeny współbieżności.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby skonfigurować właściwość jako token współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Interfejs API Fluent

Aby skonfigurować właściwość jako token współbieżności, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Sygnatura czasowa/wersja wiersza

Sygnatura czasowa jest właściwością, w której nowa wartość jest generowana przez bazę danych przy każdym wstawieniu lub zaktualizowaniu wiersza. Właściwość jest również traktowana jako token współbieżności. Dzięki temu zostanie wyświetlony wyjątek, jeśli ktoś inny zmodyfikował wiersz, który próbujesz zaktualizować od momentu wysłania zapytania o dane.

W jaki sposób jest używany dostawca bazy danych. W przypadku SQL Server sygnatura czasowa jest zwykle używana we właściwości *Byte []* , która zostanie skonfigurowana jako kolumna *rowversion* w bazie danych.

### <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwości nigdy nie są konfigurowane jako sygnatury czasowe.

### <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby skonfigurować właściwość jako sygnaturę czasową.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Interfejs API Fluent

Aby skonfigurować właściwość jako sygnaturę czasową, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
