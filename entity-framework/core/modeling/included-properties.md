---
title: Uwzględnianie i wykluczanie właściwości — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929827"
---
# <a name="including--excluding-properties"></a>Uwzględnianie i wykluczanie właściwości

W tym właściwości w modelu oznacza, że EF ma metadane dotyczące tej właściwości i podejmie próbę odczytu i zapisu wartości z i do bazy danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwości publicznej metody pobierającej i ustawiającej zostaną uwzględnione w modelu.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, które mają zostać wykluczone z właściwością z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwia wykluczać właściwość z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
