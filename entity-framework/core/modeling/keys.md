---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929839"
---
# <a name="keys-primary"></a>Klucze (podstawowe)

Klucz służy jako podstawowy identyfikator unikatowy dla każdego wystąpienia jednostki. Korzystając z relacyjnej bazy danych to mapuje do koncepcji *klucz podstawowy*. Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji). 

Jedną z następujących metod może służyć do instalacja/Tworzenie klucza podstawowego.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, właściwość o nazwie `Id` lub `<type name>Id` zostaną skonfigurowane jako klucza jednostki.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować jedną właściwość jako klucz jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie jednej właściwości klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

Można również skonfigurować wiele właściwości, aby być kluczem jednostki (znanych jako klucz złożony) za pomocą Fluent interfejsu API. Klucze złożone można skonfigurować tylko przy użyciu interfejsu API Fluent — konwencje nigdy nie skonfiguruje klucz złożony i nie umożliwia adnotacje danych konfiguracji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
