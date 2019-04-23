---
title: Właściwości wymagane/opcjonalne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929813"
---
# <a name="required-and-optional-properties"></a>Wymagane i opcjonalne właściwości

Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy, aby mogła zawierać `null`. Jeśli `null` nie jest prawidłową wartością ma być przypisane do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, właściwość, której typ CLR może zawierać wartości null zostanie skonfigurowany jako opcjonalny (`string`, `int?`, `byte[]`itp.). Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowany zgodnie z wymogami (`int`, `decimal`, `bool`itp.).

> [!NOTE]  
> Właściwość, której typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalną. Właściwość będzie zawsze być brana pod uwagę wymagane przez program Entity Framework.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby wskazać, że właściwość jest wymagana.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Interfejs Fluent API

Aby wskazać, że właściwość jest wymagana, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

