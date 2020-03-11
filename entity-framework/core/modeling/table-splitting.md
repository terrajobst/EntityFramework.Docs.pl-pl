---
title: Podział tabeli — EF Core
description: Jak skonfigurować podział tabeli przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: de24f8903af79ebd7f68e6b74288257883c1fa8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417399"
---
# <a name="table-splitting"></a>Dzielenie tabeli

EF Core umożliwia mapowanie dwóch lub więcej jednostek do jednego wiersza. Jest to tzw. _dzielenie tabeli_ lub _udostępnianie tabel_.

## <a name="configuration"></a>Konfiguracja

Aby używać dzielenia tabel, należy zamapować typy jednostek na tę samą tabelę, mieć klucze podstawowe zamapowane na te same kolumny i co najmniej jedną relację skonfigurowaną między kluczem podstawowym jednego typu jednostki a inną w tej samej tabeli.

Typowy scenariusz dzielenia tabeli polega na użyciu tylko podzbioru kolumn w tabeli w celu uzyskania większej wydajności lub hermetyzacji.

W tym przykładzie `Order` reprezentuje podzestaw `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Poza wymaganą konfiguracją, która wywołuje `Property(o => o.Status).HasColumnName("Status")`, aby zmapować `DetailedOrder.Status` do tej samej kolumny co `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Zobacz [pełny przykładowy projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) , aby uzyskać więcej kontekstu.

## <a name="usage"></a>Sposób użycia

Zapisywanie i wykonywanie zapytań względem jednostek przy użyciu dzielenia tabeli odbywa się w taki sam sposób jak inne jednostki:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Opcjonalna jednostka zależna

> [!NOTE]
> Ta funkcja została wprowadzona w EF Core 3,0.

Jeśli wszystkie kolumny używane przez jednostkę zależną są `NULL` w bazie danych, żadne wystąpienie dla niego nie zostanie utworzone podczas wykonywania zapytania. Umożliwia to modelowanie opcjonalnej jednostki zależnej, gdzie Właściwość relacji na podmiotu zabezpieczeń będzie równa null. Należy zauważyć, że to również wszystkie właściwości zależne są opcjonalne i ustawione na `null`, co może nie być oczekiwane.

## <a name="concurrency-tokens"></a>Tokeny współbieżności

Jeśli którykolwiek z typów jednostek współużytkujących tabelę ma token współbieżności, musi być również uwzględniony we wszystkich innych typach jednostek. Jest to konieczne w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.

Aby uniknąć ujawniania tokenu współbieżności w kodzie konsumowanym, możliwe jest utworzenie jednej jako [Właściwości cienia](xref:core/modeling/shadow-properties):

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
