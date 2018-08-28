---
title: Przełączanie pomiędzy wiele modeli z tym samym typem DbContext — EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994989"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="e5da0-102">Przełączanie pomiędzy wiele modeli za pomocą tego samego typu DbContext</span><span class="sxs-lookup"><span data-stu-id="e5da0-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="e5da0-103">Wbudowany model `OnModelCreating` wystarczą właściwości dla kontekstu można zmienić, jak zbudowane jest model.</span><span class="sxs-lookup"><span data-stu-id="e5da0-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="e5da0-104">Na przykład może zostać wykorzystana do wykluczenia niektórych właściwości:</span><span class="sxs-lookup"><span data-stu-id="e5da0-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="e5da0-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="e5da0-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="e5da0-106">Jednak jeśli chcesz wypróbować, wykonując powyższe bez wprowadzania dodatkowych zmian otrzymamy ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="e5da0-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="e5da0-107">Jest to spowodowane przez model używa EF, aby zwiększyć wydajność, wywołując tylko mechanizmu buforowania `OnModelCreating` raz i buforowanie modelu.</span><span class="sxs-lookup"><span data-stu-id="e5da0-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="e5da0-108">Domyślnie EF przyjęto założenie, że we wszystkich kontekstach danego typu modelu będą takie same.</span><span class="sxs-lookup"><span data-stu-id="e5da0-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="e5da0-109">Aby to osiągnąć, domyślna Implementacja klasy `IModelCacheKeyFactory` zwraca klucz, zawierający tylko typ kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e5da0-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="e5da0-110">Aby zmienić to musisz zastąpić `IModelCacheKeyFactory` usługi.</span><span class="sxs-lookup"><span data-stu-id="e5da0-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="e5da0-111">Nowa implementacja musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które wpływają na modelu:</span><span class="sxs-lookup"><span data-stu-id="e5da0-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
