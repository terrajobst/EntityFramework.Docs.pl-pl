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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Przełączanie pomiędzy wielu modeli tego samego typu DbContext

Model tworzony `OnModelCreating` właściwości można użyć w kontekście Aby zmienić sposób modelu jest wbudowana. Na przykład można użyć do wykluczenia niektórych właściwości:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Jednak jeśli wykonano czynności powyżej bez dodatkowych zmian jak ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`. Jest to spowodowane modelu mechanizmu EF korzysta w celu poprawienia wydajności za pomocą tylko buforowania `OnModelCreating` raz i buforowanie modelu.

Domyślnie EF przyjęto założenie, że dla dowolnego kontekstu danego typu modelu będą takie same. W tym celu Domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który zawiera tylko typ kontekstu. Aby zmienić to trzeba wymienić `IModelCacheKeyFactory` usługi. Nowej implementacji musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które mają wpływ na modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
