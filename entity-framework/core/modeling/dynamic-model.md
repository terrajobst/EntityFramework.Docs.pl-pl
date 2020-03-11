---
title: Przemienne między wieloma modelami o tym samym typie DbContext — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: a160f0d382ee2a3ac7130ce1ac98eb24b3f79394
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416361"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="13953-102">Przemienne między wieloma modelami o tym samym typie DbContext</span><span class="sxs-lookup"><span data-stu-id="13953-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="13953-103">Model zbudowany w `OnModelCreating` może użyć właściwości w kontekście, aby zmienić sposób kompilowania modelu.</span><span class="sxs-lookup"><span data-stu-id="13953-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="13953-104">Załóżmy na przykład, że chcemy skonfigurować jednostkę w różny sposób w zależności od właściwości:</span><span class="sxs-lookup"><span data-stu-id="13953-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="13953-105">Niestety, ten kod nie działa tak, jak to jest, ponieważ EF kompiluje model i uruchamia `OnModelCreating` tylko raz, buforowanie wyniku ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="13953-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="13953-106">Można jednak podpiąć do mechanizmu buforowania modelu, aby zapewnić, że EF wie o właściwości dającej różne modele.</span><span class="sxs-lookup"><span data-stu-id="13953-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="13953-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="13953-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="13953-108">Dr używa `IModelCacheKeyFactory` do generowania kluczy pamięci podręcznej dla modeli; Domyślnie EF zakłada, że dla dowolnego danego typu kontekstu model będzie taki sam, więc domyślna implementacja tej usługi zwraca klucz, który po prostu zawiera typ kontekstu.</span><span class="sxs-lookup"><span data-stu-id="13953-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="13953-109">Aby generować różne modele z tego samego typu kontekstu, należy zastąpić usługę `IModelCacheKeyFactory` poprawną implementacją; wygenerowany klucz będzie porównywany z innymi kluczami modeli przy użyciu metody `Equals`, biorąc pod uwagę wszystkie zmienne, które wpływają na model:</span><span class="sxs-lookup"><span data-stu-id="13953-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="13953-110">Następująca implementacja przybiera `IgnoreIntProperty` do konta podczas tworzenia klucza pamięci podręcznej modelu:</span><span class="sxs-lookup"><span data-stu-id="13953-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="13953-111">Na koniec Zarejestruj nowe `IModelCacheKeyFactory` w `OnConfiguring`kontekstu:</span><span class="sxs-lookup"><span data-stu-id="13953-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="13953-112">Zobacz [pełny przykładowy projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) , aby uzyskać więcej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="13953-112">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
