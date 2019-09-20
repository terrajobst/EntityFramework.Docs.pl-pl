---
title: Właściwości wymagane/opcjonalne-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149150"
---
# <a name="required-and-optional-properties"></a>Właściwości wymagane i opcjonalne

Właściwość jest uważana za opcjonalną, jeśli jest poprawna `null`, aby mogła ją zawierać. Jeśli `null` nie jest prawidłową wartością do przypisania do właściwości, zostanie ona uznana za właściwość wymaganą.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwość, której typ .NET może zawierać wartość null, zostanie skonfigurowana jako`string`opcjonalna `byte[]`(, `int?`, itp.). Właściwości, których typ CLR nie może zawierać wartości null, zostaną skonfigurowane`int`jako `decimal`wymagane `bool`(,, itp.).

> [!NOTE]  
> Nie można skonfigurować właściwości, której typ .NET nie może zawierać wartości null. Właściwość będzie zawsze uznawana za wymaganą przez Entity Framework.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby wskazać, że właściwość jest wymagana.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby wskazać, że właściwość jest wymagana.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

