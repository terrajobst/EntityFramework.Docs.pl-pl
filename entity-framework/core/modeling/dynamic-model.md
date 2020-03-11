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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Przemienne między wieloma modelami o tym samym typie DbContext

Model zbudowany w `OnModelCreating` może użyć właściwości w kontekście, aby zmienić sposób kompilowania modelu. Załóżmy na przykład, że chcemy skonfigurować jednostkę w różny sposób w zależności od właściwości:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Niestety, ten kod nie działa tak, jak to jest, ponieważ EF kompiluje model i uruchamia `OnModelCreating` tylko raz, buforowanie wyniku ze względu na wydajność. Można jednak podpiąć do mechanizmu buforowania modelu, aby zapewnić, że EF wie o właściwości dającej różne modele.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Dr używa `IModelCacheKeyFactory` do generowania kluczy pamięci podręcznej dla modeli; Domyślnie EF zakłada, że dla dowolnego danego typu kontekstu model będzie taki sam, więc domyślna implementacja tej usługi zwraca klucz, który po prostu zawiera typ kontekstu. Aby generować różne modele z tego samego typu kontekstu, należy zastąpić usługę `IModelCacheKeyFactory` poprawną implementacją; wygenerowany klucz będzie porównywany z innymi kluczami modeli przy użyciu metody `Equals`, biorąc pod uwagę wszystkie zmienne, które wpływają na model:

Następująca implementacja przybiera `IgnoreIntProperty` do konta podczas tworzenia klucza pamięci podręcznej modelu:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Na koniec Zarejestruj nowe `IModelCacheKeyFactory` w `OnConfiguring`kontekstu:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Zobacz [pełny przykładowy projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) , aby uzyskać więcej kontekstu.
