---
title: Tworzenie kopii pola — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054533"
---
# <a name="backing-fields"></a>Pola zapasowego

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 1.1.

Pola zapasowy umożliwia EF do odczytu lub zapisu do pola, a nie właściwością. Może to być przydatne, gdy hermetyzacji klasy jest używany do ograniczenia stosowania i/lub zwiększenia semantyki wokół dostęp do danych przez kod aplikacji, ale wartość powinna być odczytywać i/lub zapisane w bazie danych bez korzystania z tych ograniczeń / ulepszenia.

## <a name="conventions"></a>Konwencje

Według Konwencji spowoduje odnalezienie następujących pól jak kopie pól dla danej właściwości (wymienione w kolejności priorytetu). Pola są odnajdywane tylko dla właściwości, które znajdują się w modelu. Aby uzyskać więcej informacji, na którym właściwości znajdują się w modelu, zobacz [tym & Wykluczanie właściwości](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Po skonfigurowaniu polem zapasowym EF zapisze bezpośrednio do tego pola, gdy materializowania wystąpień jednostek z bazy danych (a nie za pomocą metody ustawiającej właściwości). Jeśli EF musi odczytać lub zapisać wartość w innym czasie, właściwość zostanie użyty, jeśli to możliwe. Na przykład jeśli EF musi zaktualizować wartości dla właściwości, zostanie użyty metoda ustawiająca właściwości, jeśli jest zdefiniowana. Jeśli właściwość jest tylko do odczytu, następnie go zapisze do pola.

## <a name="data-annotations"></a>Adnotacji danych

Tworzenie kopii pól nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Interfejsu API Fluent służy do konfigurowania polem zapasowym dla właściwości.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Kontrolowanie, gdy to pole jest używane

Można skonfigurować podczas EF używa pola lub właściwości. Zobacz [PropertyAccessMode wyliczenia](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) dla obsługiwane opcje.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pola bez właściwości

Można też utworzyć właściwości koncepcyjnej w modelu, które nie mają odpowiednia właściwość CLR w klasie jednostki, ale zamiast tego używa pola do przechowywania danych w jednostce. Różni się to od [właściwości tle](shadow-properties.md), gdy dane są przechowywane w śledzenie zmian. To zazwyczaj będzie można użyć, jeśli klasa jednostki używa metody do pobierania/ustawiania wartości.

Można nadać EF nazwę pola w `Property(...)` interfejsu API. Jeśli nie ma właściwości o podanej nazwie, EF będzie wyglądać dla pola.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Można również podać właściwość nazwę inną niż nazwa pola. Ta nazwa jest następnie używana podczas tworzenia modelu, głównie będzie można użyć dla nazwy kolumny, który jest mapowany w bazie danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Jeśli nie ma właściwości w klasie jednostki, możesz użyć `EF.Property(...)` metody w zapytaniu składnika LINQ do odwoływania się do właściwości koncepcyjnie jest częścią modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
