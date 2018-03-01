---
title: "Wstępne wypełnianie danych - EF Core"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a>Wstępne wypełnianie danych

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 2.1.

Umożliwia wstępne wypełnianie danych początkowej danych do wypełniania bazy danych. W odróżnieniu od w EF6 w podstawowej EF wstępne wypełnianie danych jest skojarzona z typem jednostki jako część konfiguracji modelu. Następnie EF Core migracji można automatycznie obliczyć co insert, update lub delete potrzebę operacje można zastosować podczas uaktualniania bazy danych do nowej wersji modelu.

Na przykład można to skonfigurować dane `Blog` w `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Aby dodać jednostek, które mają relację z wartości klucza obcego muszą być określone. Często właściwości klucza obcego są w stanie w tle, tak aby można było ustawić wartości klasę anonimowego należy używać:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
