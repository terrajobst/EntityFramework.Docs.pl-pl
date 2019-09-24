---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197270"
---
# <a name="keys-primary"></a>Klucze (podstawowe)

Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki. W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*. Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji). 

Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwość o `Id` nazwie `<type name>Id` lub zostanie skonfigurowana jako klucz jednostki.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs API Fluent

Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym). Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
