---
title: Przemienne między wieloma modelami o tym samym typie DbContext — EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655725"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="a6007-102">Przemienne między wieloma modelami o tym samym typie DbContext</span><span class="sxs-lookup"><span data-stu-id="a6007-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="a6007-103">Model zbudowany w `OnModelCreating` może użyć właściwości w kontekście, aby zmienić sposób kompilowania modelu.</span><span class="sxs-lookup"><span data-stu-id="a6007-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="a6007-104">Na przykład można użyć, aby wykluczyć określoną właściwość:</span><span class="sxs-lookup"><span data-stu-id="a6007-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="a6007-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="a6007-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="a6007-106">Jeśli jednak podjęto próbę wykonania powyższych czynności bez dodatkowych zmian, ten sam model jest tworzony za każdym razem, gdy zostanie utworzony nowy kontekst dla dowolnej wartości `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="a6007-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="a6007-107">Jest to spowodowane przez mechanizm buforowania modelu, który służy do poprawy wydajności przez wywoływanie `OnModelCreating` raz i buforowanie modelu.</span><span class="sxs-lookup"><span data-stu-id="a6007-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="a6007-108">Domyślnie EF zakłada, że dla dowolnego danego typu kontekstu model będzie taki sam.</span><span class="sxs-lookup"><span data-stu-id="a6007-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="a6007-109">Aby to osiągnąć, domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który po prostu zawiera typ kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a6007-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="a6007-110">Aby zmienić tę wartość, musisz zastąpić usługę `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="a6007-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="a6007-111">Nowa implementacja musi zwrócić obiekt, który można porównać z innymi kluczami modelu przy użyciu metody `Equals`, która uwzględnia wszystkie zmienne, które wpływają na model:</span><span class="sxs-lookup"><span data-stu-id="a6007-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
