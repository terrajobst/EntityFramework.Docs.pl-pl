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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Przełączanie pomiędzy wiele modeli za pomocą tego samego typu DbContext

Wbudowany model `OnModelCreating` wystarczą właściwości dla kontekstu można zmienić, jak zbudowane jest model. Na przykład może zostać wykorzystana do wykluczenia niektórych właściwości:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Jednak jeśli chcesz wypróbować, wykonując powyższe bez wprowadzania dodatkowych zmian otrzymamy ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`. Jest to spowodowane przez model używa EF, aby zwiększyć wydajność, wywołując tylko mechanizmu buforowania `OnModelCreating` raz i buforowanie modelu.

Domyślnie EF przyjęto założenie, że we wszystkich kontekstach danego typu modelu będą takie same. Aby to osiągnąć, domyślna Implementacja klasy `IModelCacheKeyFactory` zwraca klucz, zawierający tylko typ kontekstu. Aby zmienić to musisz zastąpić `IModelCacheKeyFactory` usługi. Nowa implementacja musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które wpływają na modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
