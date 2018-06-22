---
title: Przełączanie pomiędzy wielu modeli tego samego typu DbContext - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678725"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="e4481-102">Przełączanie pomiędzy wielu modeli tego samego typu DbContext</span><span class="sxs-lookup"><span data-stu-id="e4481-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="e4481-103">Model tworzony `OnModelCreating` właściwości można użyć w kontekście Aby zmienić sposób modelu jest wbudowana.</span><span class="sxs-lookup"><span data-stu-id="e4481-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="e4481-104">Na przykład można użyć do wykluczenia niektórych właściwości:</span><span class="sxs-lookup"><span data-stu-id="e4481-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="e4481-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="e4481-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="e4481-106">Jednak jeśli wykonano czynności powyżej bez dodatkowych zmian jak ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="e4481-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="e4481-107">Jest to spowodowane modelu mechanizmu EF korzysta w celu poprawienia wydajności za pomocą tylko buforowania `OnModelCreating` raz i buforowanie modelu.</span><span class="sxs-lookup"><span data-stu-id="e4481-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="e4481-108">Domyślnie EF przyjęto założenie, że dla dowolnego kontekstu danego typu modelu będą takie same.</span><span class="sxs-lookup"><span data-stu-id="e4481-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="e4481-109">W tym celu Domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który zawiera tylko typ kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e4481-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="e4481-110">Aby zmienić to trzeba wymienić `IModelCacheKeyFactory` usługi.</span><span class="sxs-lookup"><span data-stu-id="e4481-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="e4481-111">Nowej implementacji musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które mają wpływ na modelu:</span><span class="sxs-lookup"><span data-stu-id="e4481-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
