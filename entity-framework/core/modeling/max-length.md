---
title: Maksymalna długość — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197225"
---
# <a name="maximum-length"></a>Maksymalna długość

Skonfigurowanie maksymalnej długości zapewnia wskazówkę do magazynu danych o odpowiednim typie danych, który ma być używany dla danej właściwości. Maksymalna długość dotyczy tylko typów danych tablicowych, takich jak `string` i `byte[]`.

> [!NOTE]  
> Entity Framework nie sprawdza żadnej weryfikacji maksymalnej długości przed przekazaniem danych do dostawcy. Jest to dostawca lub magazyn danych do zweryfikowania, jeśli jest to konieczne. Na przykład podczas określania wartości docelowej SQL Server przekroczenie maksymalnej długości spowoduje wyjątek, ponieważ typ danych kolumny źródłowej nie zezwala na przechowywanie nadmiarowych danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, pozostało do dostawcy bazy danych, aby wybrać odpowiedni typ danych dla właściwości. Dla właściwości, które mają długość, dostawca bazy danych zwykle wybiera typ danych, który pozwala na najdłuższą długość danych. Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby skonfigurować maksymalną długość właściwości. W tym przykładzie element docelowy SQL Server spowoduje `nvarchar(500)` to użycie typu danych.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby skonfigurować maksymalną długość właściwości. W tym przykładzie element docelowy SQL Server spowoduje `nvarchar(500)` to użycie typu danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
