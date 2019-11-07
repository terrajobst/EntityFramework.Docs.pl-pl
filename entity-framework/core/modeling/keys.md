---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655945"
---
# <a name="keys-primary"></a>Klucze (podstawowe)

Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki. W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*. Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).

Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs API Fluent

Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym). Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
