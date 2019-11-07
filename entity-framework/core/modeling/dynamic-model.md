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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Przemienne między wieloma modelami o tym samym typie DbContext

Model zbudowany w `OnModelCreating` może użyć właściwości w kontekście, aby zmienić sposób kompilowania modelu. Na przykład można użyć, aby wykluczyć określoną właściwość:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Jeśli jednak podjęto próbę wykonania powyższych czynności bez dodatkowych zmian, ten sam model jest tworzony za każdym razem, gdy zostanie utworzony nowy kontekst dla dowolnej wartości `IgnoreIntProperty`. Jest to spowodowane przez mechanizm buforowania modelu, który służy do poprawy wydajności przez wywoływanie `OnModelCreating` raz i buforowanie modelu.

Domyślnie EF zakłada, że dla dowolnego danego typu kontekstu model będzie taki sam. Aby to osiągnąć, domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który po prostu zawiera typ kontekstu. Aby zmienić tę wartość, musisz zastąpić usługę `IModelCacheKeyFactory`. Nowa implementacja musi zwrócić obiekt, który można porównać z innymi kluczami modelu przy użyciu metody `Equals`, która uwzględnia wszystkie zmienne, które wpływają na model:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
