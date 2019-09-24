---
title: Pola zapasowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197481"
---
# <a name="backing-fields"></a>Pola zapasowe

> [!NOTE]  
> Ta funkcja jest nowa w EF Core 1,1.

Pola zapasowe umożliwiają EF odczyt i/lub zapis do pola, a nie właściwości. Może to być przydatne, gdy hermetyzacja w klasie jest używana do ograniczania użycia i/lub podniesienia semantyki dostępu do danych przez kod aplikacji, ale wartość powinna zostać odczytana i/lub zapisywana w bazie danych bez użycia tych ograniczeń/ usprawni.

## <a name="conventions"></a>Konwencje

Według Konwencji następujące pola zostaną odnalezione jako pola zapasowe dla danej właściwości (wymienione w kolejności pierwszeństwa). Pola są odnajdywane tylko dla właściwości, które są uwzględnione w modelu. Aby uzyskać więcej informacji na temat właściwości uwzględnionych w modelu, zobacz [m.in. & z wyłączeniem właściwości](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

W przypadku skonfigurowania pola zapasowego program Dr zapisuje bezpośrednio do tego pola podczas materializacji wystąpienia jednostek z bazy danych (zamiast używać metody ustawiającej właściwość). Jeśli EF musi odczytać lub zapisać wartość w innym czasie, będzie używać właściwości, jeśli jest to możliwe. Na przykład jeśli EF musi zaktualizować wartość właściwości, zostanie użyta Metoda ustawiająca właściwość, jeśli jest zdefiniowana. Jeśli właściwość jest tylko do odczytu, nastąpi zapis do pola.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować pól zapasowych przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można skonfigurować pole zapasowe dla właściwości.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Kontrolowanie, gdy pole jest używane

Można skonfigurować, kiedy dr używa pola lub właściwości. Zobacz [Wyliczenie PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) dla obsługiwanych opcji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pola bez właściwości

Można również utworzyć w modelu Właściwość koncepcyjną, która nie ma odpowiedniej właściwości CLR w klasie Entity, ale zamiast tego używa pola do przechowywania danych w jednostce. Różni się to od [Właściwości cienia](shadow-properties.md), gdzie dane są przechowywane w monitorze zmian. Zwykle jest to używane, jeśli Klasa jednostki używa metod do pobierania/ustawiania wartości.

Można nadać EF nazwę pola w `Property(...)` interfejsie API. Jeśli nie ma żadnej właściwości o podaną nazwę, EF będzie szukać pola.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Można również określić, aby nadać właściwości nazwę inną niż nazwa pola. Ta nazwa jest następnie używana podczas tworzenia modelu, w szczególności zostanie użyta jako nazwa kolumny, która jest mapowana na bazę danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Jeśli w klasie Entity nie ma właściwości, można użyć `EF.Property(...)` metody w zapytaniu LINQ, aby odwołać się do właściwości, która jest koncepcyjnie częścią modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
