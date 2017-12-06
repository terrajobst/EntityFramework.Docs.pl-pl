---
title: "Przełączanie pomiędzy wielu modeli tego samego typu DbContext - EF Core"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="a4c57-102">Przełączanie pomiędzy wielu modeli tego samego typu DbContext</span><span class="sxs-lookup"><span data-stu-id="a4c57-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="a4c57-103">Model tworzony `OnModelCreating` właściwości można użyć w kontekście Aby zmienić sposób modelu jest wbudowana.</span><span class="sxs-lookup"><span data-stu-id="a4c57-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="a4c57-104">Na przykład można użyć do wykluczenia niektórych właściwości:</span><span class="sxs-lookup"><span data-stu-id="a4c57-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="a4c57-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="a4c57-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="a4c57-106">Jednak jeśli wykonano czynności powyżej bez dodatkowych zmian jak ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="a4c57-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="a4c57-107">Jest to spowodowane modelu mechanizmu EF korzysta w celu poprawienia wydajności za pomocą tylko buforowania `OnModelCreating` raz i buforowanie modelu.</span><span class="sxs-lookup"><span data-stu-id="a4c57-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="a4c57-108">Domyślnie EF przyjęto założenie, że dla dowolnego kontekstu danego typu modelu będą takie same.</span><span class="sxs-lookup"><span data-stu-id="a4c57-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="a4c57-109">W tym celu Domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który zawiera tylko typ kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a4c57-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="a4c57-110">Aby zmienić to trzeba wymienić `IModelCacheKeyFactory` usługi.</span><span class="sxs-lookup"><span data-stu-id="a4c57-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="a4c57-111">Nowej implementacji musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które mają wpływ na modelu:</span><span class="sxs-lookup"><span data-stu-id="a4c57-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
