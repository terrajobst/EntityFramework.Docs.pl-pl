---
title: Podział tabeli — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656037"
---
# <a name="table-splitting"></a>Dzielenie tabeli

>[!NOTE]
> Ta funkcja jest nowa w EF Core 2,0.

EF Core umożliwia mapowanie dwóch lub więcej jednostek do jednego wiersza. Jest to tzw. _dzielenie tabeli_ lub _udostępnianie tabel_.

## <a name="configuration"></a>Konfiguracja

Aby używać dzielenia tabel, należy zamapować typy jednostek na tę samą tabelę, mieć klucze podstawowe zamapowane na te same kolumny i co najmniej jedną relację skonfigurowaną między kluczem podstawowym jednego typu jednostki a inną w tej samej tabeli.

Typowy scenariusz dzielenia tabeli polega na użyciu tylko podzbioru kolumn w tabeli w celu uzyskania większej wydajności lub hermetyzacji.

W tym przykładzie `Order` reprezentuje podzestaw `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Poza wymaganą konfiguracją, która wywołuje `Property(o => o.Status).HasColumnName("Status")`, aby zmapować `DetailedOrder.Status` do tej samej kolumny co `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) , aby uzyskać więcej kontekstu.

## <a name="usage"></a>Użycie

Zapisywanie i wykonywanie zapytań względem jednostek przy użyciu dzielenia tabeli odbywa się tak samo jak w przypadku innych jednostek. I począwszy od EF Core 3,0 można `null`odwołanie do jednostki zależnej. Jeśli wszystkie kolumny używane przez jednostkę zależną są `NULL` są bazami danych, żadne wystąpienie dla niego nie zostanie utworzone podczas wykonywania zapytania. Ponadto wszystkie właściwości są opcjonalne i mają ustawioną wartość `null`, co może być nieoczekiwane.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokeny współbieżności

Jeśli którykolwiek z typów jednostek współużytkujących tabelę ma token współbieżności, musi być uwzględniony we wszystkich innych typach jednostek, aby uniknąć nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek mapowanych na tę samą tabelę.

Aby nie ujawniać go w kodzie konsumowanym, można go utworzyć w tle.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
