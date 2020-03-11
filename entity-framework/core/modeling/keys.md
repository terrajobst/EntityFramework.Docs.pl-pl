---
title: Klucze — EF Core
description: Jak skonfigurować klucze dla typów jednostek podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416468"
---
# <a name="keys"></a>Klucze

Klucz służy jako unikatowy identyfikator dla każdego wystąpienia jednostki. Większość jednostek w EF ma jeden klucz, który mapuje na koncepcję *klucza podstawowego* w relacyjnych bazach danych (dla jednostek bez kluczy, zobacz [jednostki o niemniejszej](xref:core/modeling/keyless-entity-types)liczbie). Jednostki mogą mieć dodatkowe klucze poza kluczem podstawowym (zobacz [klucze alternatywne](#alternate-keys) , aby uzyskać więcej informacji).

Zgodnie z Konwencją właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz podstawowy jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [Typy jednostek należących](xref:core/modeling/owned-entities) do siebie używają różnych reguł do definiowania kluczy.

Jedną właściwość można skonfigurować jako klucz podstawowy jednostki w następujący sposób:

## <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

Można również skonfigurować wiele właściwości jako klucz jednostki — jest to nazywane kluczem złożonym. Klucze złożone można skonfigurować tylko za pomocą interfejsu API Fluent; konwencje nigdy nie będą konfigurować klucza złożonego i nie można użyć adnotacji danych, aby je skonfigurować.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>Nazwa klucza podstawowego

Według Konwencji w kluczach podstawowych relacyjnych baz danych tworzone są nazwy `PK_<type name>`. Nazwę ograniczenia PRIMARY KEY można skonfigurować w następujący sposób:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>Typy i wartości kluczy

Chociaż EF Core obsługuje używanie właściwości dowolnego typu pierwotnego jako klucza podstawowego, w tym `string`, `Guid`, `byte[]` i innych, nie wszystkie bazy danych obsługują wszystkie typy jako klucze. W niektórych przypadkach wartości klucza można przekonwertować na typ obsługiwany automatycznie. w przeciwnym razie konwersja powinna być [określona ręcznie](xref:core/modeling/value-conversions).

Właściwości klucza muszą zawsze mieć wartość inną niż domyślna podczas dodawania nowej jednostki do kontekstu, ale niektóre typy zostaną [wygenerowane przez bazę danych](xref:core/modeling/generated-properties). W takim przypadku EF podejmie próbę wygenerowania wartości tymczasowej, gdy jednostka zostanie dodana do celów śledzenia. Po wywołaniu [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) wartość tymczasowa zostanie zastąpiona wartością wygenerowaną przez bazę danych.

> [!Important]
> Jeśli właściwość klucza ma swoją wartość wygenerowaną przez bazę danych i określono wartość inną niż domyślna podczas dodawania jednostki, EF przyjmie, że jednostka już istnieje w bazie danych i spróbuje ją zaktualizować zamiast wstawiania nowej. Aby tego uniknąć, Wyłącz generowanie wartości lub zobacz [, jak określić wartości jawne dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).

## <a name="alternate-keys"></a>Klucze alternatywne

Alternatywny klucz służy jako alternatywny unikatowy identyfikator dla każdego wystąpienia jednostki oprócz klucza podstawowego; może być używany jako element docelowy relacji. W przypadku korzystania z relacyjnej bazy danych mapowanie do koncepcji unikatowego indeksu/ograniczenia w kolumnach klucza alternatywnego oraz co najmniej jedno ograniczenie klucza obcego odwołujące się do kolumn.

> [!TIP]
> Jeśli chcesz tylko wymusić unikatowość w kolumnie, zdefiniuj unikatowy indeks, a nie klucz alternatywny (zobacz [indeksy](indexes.md)). W EF klucze alternatywne są tylko do odczytu i zapewniają dodatkową semantykę za pośrednictwem unikatowych indeksów, ponieważ mogą one być używane jako obiekty docelowe klucza obcego.

Klucze alternatywne są zwykle wprowadzane w razie potrzeby i nie trzeba ich ręcznie konfigurować. Przy użyciu konwencji klucz alternatywny jest wprowadzany w przypadku identyfikowania właściwości, która nie jest kluczem podstawowym jako element docelowy relacji.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

Można również skonfigurować pojedynczą właściwość jako klucz alternatywny:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

Możesz również skonfigurować wiele właściwości jako klucz alternatywny (nazywany kluczem alternatywnym):

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

Na koniec, zgodnie z Konwencją, indeks i ograniczenie, które są wprowadzone dla klucza alternatywnego, będą nazwane `AK_<type name>_<property name>` (dla złożonych kluczy alternatywnych `<property name>` stać się podkreśleniem rozdzielanym podkreśleniami listy nazw właściwości). Można skonfigurować nazwę indeksu klucza alternatywnego i ograniczenia UNIQUE:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
