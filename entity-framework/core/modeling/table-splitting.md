---
title: Tabela dzielenia - programu EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562601"
---
# <a name="table-splitting"></a>Dzielenie tabeli

>[!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.0.

EF Core umożliwia mapowanie dwóch lub więcej podmiotów na jeden wiersz. Jest to nazywane _tabeli podział_ lub _udostępnianie tabeli_.

## <a name="configuration"></a>Konfiguracja

Aby korzystać z tabeli dzielenia, które typy jednostek, które muszą być mapowane do tej samej tabeli, mają klucze podstawowe mapowane na te same kolumny i co najmniej jednej relacji skonfigurowana między klucza podstawowego typu jednostki jednego i drugiego w tej samej tabeli.

Typowy scenariusz do dzielenia tabeli używa tylko podzbiór kolumn w tabeli dla większej wydajności lub hermetyzacji.

W tym przykładzie `Order` reprezentuje podzbiór `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Oprócz wymaganej konfiguracji nazywamy `HasBaseType((string)null)` w celu uniknięcia mapowania `DetailedOrder` w tej samej hierarchii co `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) Aby uzyskać dodatkowy kontekst.

## <a name="usage"></a>Użycie

Zapisywanie i zapytania jednostki przy użyciu tabeli podział odbywa się w taki sam sposób jak inne podmioty, jedyną różnicą jest to, że wszystkie jednostki udostępnianie wiersza musi być śledzona dla insert.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokeny współbieżności

Jeśli żadnego z typów jednostek, udostępnianie tabeli jest tokenem współbieżności następnie go muszą być zawarte w wszystkich innych typów jednostek, aby uniknąć wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.

Aby uniknąć uwidaczniania go do kod konsumencki jest możliwe tworzenie, jeden w stanie w tle.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]