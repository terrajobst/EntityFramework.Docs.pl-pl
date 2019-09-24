---
title: Uwzględnienie & z wyłączeniem właściwości — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197412"
---
# <a name="including--excluding-properties"></a>Uwzględnianie i wykluczanie właściwości

Dołączenie właściwości w modelu oznacza, że Dr ma metadane dotyczące tej właściwości i podejmie próbę odczytu i zapisu wartości z/do bazy danych.

## <a name="conventions"></a>Konwencje

Według Konwencji właściwości publiczne z metodą pobierającą i setter zostaną uwzględnione w modelu.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby wykluczyć właściwość z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można wykluczyć właściwość z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
