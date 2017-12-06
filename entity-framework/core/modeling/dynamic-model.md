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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Przełączanie pomiędzy wielu modeli tego samego typu DbContext

Model tworzony `OnModelCreating` właściwości można użyć w kontekście Aby zmienić sposób modelu jest wbudowana. Na przykład można użyć do wykluczenia niektórych właściwości:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Jednak jeśli wykonano czynności powyżej bez dodatkowych zmian jak ten sam model za każdym razem, gdy nowy kontekst jest tworzona dla dowolnej wartości `IgnoreIntProperty`. Jest to spowodowane modelu mechanizmu EF korzysta w celu poprawienia wydajności za pomocą tylko buforowania `OnModelCreating` raz i buforowanie modelu.

Domyślnie EF przyjęto założenie, że dla dowolnego kontekstu danego typu modelu będą takie same. W tym celu Domyślna implementacja `IModelCacheKeyFactory` zwraca klucz, który zawiera tylko typ kontekstu. Aby zmienić to trzeba wymienić `IModelCacheKeyFactory` usługi. Nowej implementacji musi zwracać obiekt, który można porównać do innych kluczy modelu przy użyciu `Equals` metodę, która uwzględnia wszystkie zmienne, które mają wpływ na modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
