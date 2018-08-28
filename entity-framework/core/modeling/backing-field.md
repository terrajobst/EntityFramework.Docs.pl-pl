---
title: Pola - zapasowe programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994096"
---
# <a name="backing-fields"></a>Pola zapasowe

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 1.1.

Pola zapasowego umożliwiają EF do odczytu lub zapisu do pola, a nie właściwości. Może to być przydatne, gdy hermetyzacji klasy jest używany do ograniczenia użytkowania i/lub zwiększ semantykę dostępu do danych przez kod aplikacji, ale wartość powinna być odczytywać i/lub zapisywanych w bazie danych bez korzystania z tych ograniczeń / ulepszenia.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją następujące pola zostaną odnalezione jak kopie pól dla danej właściwości (wymienione w kolejność pierwszeństwa). Pola odnajdywane będą tylko dla właściwości, które znajdują się w modelu. Aby uzyskać więcej informacji, na którym właściwości znajdują się w modelu, zobacz [uwzględnianie i wykluczanie właściwości](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Po skonfigurowaniu polem zapasowym EF zapisze bezpośrednio do tego pola, gdy materializowanie wystąpień jednostek w bazie danych (zamiast przy użyciu metoda ustawiająca właściwości). Jeśli EF musi odczytać lub zapisać wartości w pozostałych godzinach, właściwość zostanie użyty, jeśli jest to możliwe. Na przykład jeśli EF należy zaktualizować wartość właściwości go użyje metoda ustawiająca właściwości, jeśli jest zdefiniowana. Jeśli właściwość jest tylko do odczytu, następnie go zapisuje do pola.

## <a name="data-annotations"></a>Adnotacje danych

Pola zapasowe nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie z polem zapasowym dla właściwości.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Kontrolowanie, gdy pole jest używane

Można skonfigurować, kiedy EF używa pola lub właściwości. Zobacz [wyliczenia PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) obsługiwanych opcjach.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pola bez właściwości

Ogólne właściwości można też utworzyć w modelu nie ma odpowiadającą właściwość CLR w klasie jednostki, ale zamiast tego używa pola do przechowywania danych w jednostce. To różni się od [właściwości w tle](shadow-properties.md), której dane są przechowywane w śledzenie zmian. To wykorzystania, jeśli klasa jednostki używa metody do pobierania/ustawiania wartości.

EF można nadać nazwę pola w `Property(...)` interfejsu API. Jeśli nie ma właściwości o podanej nazwie, EF będzie wyglądać dla pola.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Możesz również podać właściwość nazwę, inna niż nazwa pola. Ta nazwa jest następnie używana podczas tworzenia modelu, głównie będzie można używać dla nazwy kolumny, który jest mapowany w bazie danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Jeśli nie ma właściwości w klasie jednostki, możesz użyć `EF.Property(...)` metody w zapytaniu programu LINQ do odwoływania się do właściwości, pod względem koncepcyjnym będącej częścią modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
