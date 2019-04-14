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
# <a name="table-splitting"></a><span data-ttu-id="e807f-102">Dzielenie tabeli</span><span class="sxs-lookup"><span data-stu-id="e807f-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="e807f-103">Ta funkcja jest nowa w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e807f-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="e807f-104">EF Core umożliwia mapowanie dwóch lub więcej podmiotów na jeden wiersz.</span><span class="sxs-lookup"><span data-stu-id="e807f-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="e807f-105">Jest to nazywane _tabeli podział_ lub _udostępnianie tabeli_.</span><span class="sxs-lookup"><span data-stu-id="e807f-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="e807f-106">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e807f-106">Configuration</span></span>

<span data-ttu-id="e807f-107">Aby korzystać z tabeli dzielenia, które typy jednostek, które muszą być mapowane do tej samej tabeli, mają klucze podstawowe mapowane na te same kolumny i co najmniej jednej relacji skonfigurowana między klucza podstawowego typu jednostki jednego i drugiego w tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e807f-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="e807f-108">Typowy scenariusz do dzielenia tabeli używa tylko podzbiór kolumn w tabeli dla większej wydajności lub hermetyzacji.</span><span class="sxs-lookup"><span data-stu-id="e807f-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="e807f-109">W tym przykładzie `Order` reprezentuje podzbiór `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="e807f-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="e807f-110">Oprócz wymaganej konfiguracji nazywamy `HasBaseType((string)null)` w celu uniknięcia mapowania `DetailedOrder` w tej samej hierarchii co `Order`.</span><span class="sxs-lookup"><span data-stu-id="e807f-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="e807f-111">Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) Aby uzyskać dodatkowy kontekst.</span><span class="sxs-lookup"><span data-stu-id="e807f-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="e807f-112">Użycie</span><span class="sxs-lookup"><span data-stu-id="e807f-112">Usage</span></span>

<span data-ttu-id="e807f-113">Zapisywanie i zapytania jednostki przy użyciu tabeli podział odbywa się w taki sam sposób jak inne podmioty, jedyną różnicą jest to, że wszystkie jednostki udostępnianie wiersza musi być śledzona dla insert.</span><span class="sxs-lookup"><span data-stu-id="e807f-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="e807f-114">Tokeny współbieżności</span><span class="sxs-lookup"><span data-stu-id="e807f-114">Concurrency tokens</span></span>

<span data-ttu-id="e807f-115">Jeśli żadnego z typów jednostek, udostępnianie tabeli jest tokenem współbieżności następnie go muszą być zawarte w wszystkich innych typów jednostek, aby uniknąć wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e807f-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="e807f-116">Aby uniknąć uwidaczniania go do kod konsumencki jest możliwe tworzenie, jeden w stanie w tle.</span><span class="sxs-lookup"><span data-stu-id="e807f-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]