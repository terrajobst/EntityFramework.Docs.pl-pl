---
title: Klucze (podstawowe) — EF Core
description: Jak skonfigurować klucze dla typów jednostek podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824619"
---
# <a name="keys-primary"></a>Klucze (podstawowe)

Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki. W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*. Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).

Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.

## <a name="conventions"></a>Konwencje

Domyślnie właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> [Typy jednostek należących](xref:core/modeling/owned-entities) do siebie używają różnych reguł do definiowania kluczy.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs API Fluent

Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym). Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Typy i wartości kluczy

EF Core obsługuje używanie właściwości dowolnego typu pierwotnego jako klucza podstawowego, w tym `string`, `Guid`, `byte[]` i innych. Ale nie są one obsługiwane przez wszystkie bazy danych. W niektórych przypadkach wartości klucza można przekonwertować na typ obsługiwany automatycznie. w przeciwnym razie konwersja powinna być [określona ręcznie](xref:core/modeling/value-conversions).

Właściwości klucza muszą zawsze mieć wartość inną niż domyślna podczas dodawania nowej jednostki do kontekstu, ale niektóre typy zostaną [wygenerowane przez bazę danych](xref:core/modeling/generated-properties). W takim przypadku EF podejmie próbę wygenerowania wartości tymczasowej, gdy jednostka zostanie dodana do celów śledzenia. Po wywołaniu [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) wartość tymczasowa zostanie zastąpiona wartością wygenerowaną przez bazę danych.

> [!Important]
> Jeśli właściwość klucza ma wartość wygenerowaną przez bazę danych i określono wartość inną niż domyślna, gdy zostanie dodana jednostka, EF przyjmie, że jednostka już istnieje w bazie danych i spróbuje ją zaktualizować zamiast wstawiania nowej. Aby tego uniknąć, Wyłącz generowanie wartości lub zobacz [, jak określić wartości jawne dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).