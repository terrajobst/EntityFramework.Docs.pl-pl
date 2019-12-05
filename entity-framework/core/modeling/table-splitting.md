---
title: Podział tabeli — EF Core
description: Jak skonfigurować podział tabeli przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824557"
---
# <a name="table-splitting"></a><span data-ttu-id="58913-103">Dzielenie tabeli</span><span class="sxs-lookup"><span data-stu-id="58913-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="58913-104">Ta funkcja jest nowa w EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="58913-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="58913-105">EF Core umożliwia mapowanie dwóch lub więcej jednostek do jednego wiersza.</span><span class="sxs-lookup"><span data-stu-id="58913-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="58913-106">Jest to tzw. _dzielenie tabeli_ lub _udostępnianie tabel_.</span><span class="sxs-lookup"><span data-stu-id="58913-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="58913-107">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="58913-107">Configuration</span></span>

<span data-ttu-id="58913-108">Aby używać dzielenia tabel, należy zamapować typy jednostek na tę samą tabelę, mieć klucze podstawowe zamapowane na te same kolumny i co najmniej jedną relację skonfigurowaną między kluczem podstawowym jednego typu jednostki a inną w tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="58913-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="58913-109">Typowy scenariusz dzielenia tabeli polega na użyciu tylko podzbioru kolumn w tabeli w celu uzyskania większej wydajności lub hermetyzacji.</span><span class="sxs-lookup"><span data-stu-id="58913-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="58913-110">W tym przykładzie `Order` reprezentuje podzestaw `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="58913-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="58913-111">Poza wymaganą konfiguracją, która wywołuje `Property(o => o.Status).HasColumnName("Status")`, aby zmapować `DetailedOrder.Status` do tej samej kolumny co `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="58913-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="58913-112">Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) , aby uzyskać więcej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="58913-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="58913-113">Pomiar</span><span class="sxs-lookup"><span data-stu-id="58913-113">Usage</span></span>

<span data-ttu-id="58913-114">Zapisywanie i wykonywanie zapytań względem jednostek przy użyciu dzielenia tabeli odbywa się tak samo jak w przypadku innych jednostek.</span><span class="sxs-lookup"><span data-stu-id="58913-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="58913-115">I począwszy od EF Core 3,0 można `null`odwołanie do jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="58913-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="58913-116">Jeśli wszystkie kolumny używane przez jednostkę zależną są `NULL` są bazami danych, żadne wystąpienie dla niego nie zostanie utworzone podczas wykonywania zapytania.</span><span class="sxs-lookup"><span data-stu-id="58913-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="58913-117">Ponadto wszystkie właściwości są opcjonalne i mają ustawioną wartość `null`, co może być nieoczekiwane.</span><span class="sxs-lookup"><span data-stu-id="58913-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="58913-118">Tokeny współbieżności</span><span class="sxs-lookup"><span data-stu-id="58913-118">Concurrency tokens</span></span>

<span data-ttu-id="58913-119">Jeśli którykolwiek z typów jednostek współużytkujących tabelę ma token współbieżności, musi być uwzględniony we wszystkich innych typach jednostek, aby uniknąć nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek mapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="58913-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="58913-120">Aby nie ujawniać go w kodzie konsumowanym, można go utworzyć w tle.</span><span class="sxs-lookup"><span data-stu-id="58913-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
