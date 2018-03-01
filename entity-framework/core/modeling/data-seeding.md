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
# <a name="data-seeding"></a><span data-ttu-id="5216b-102">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="5216b-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="5216b-103">Ta funkcja jest nowa w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5216b-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="5216b-104">Umożliwia wstępne wypełnianie danych początkowej danych do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5216b-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="5216b-105">W odróżnieniu od w EF6 w podstawowej EF wstępne wypełnianie danych jest skojarzona z typem jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="5216b-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="5216b-106">Następnie EF Core migracji można automatycznie obliczyć co insert, update lub delete potrzebę operacje można zastosować podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="5216b-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="5216b-107">Na przykład można to skonfigurować dane `Blog` w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="5216b-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="5216b-108">Aby dodać jednostek, które mają relację z wartości klucza obcego muszą być określone.</span><span class="sxs-lookup"><span data-stu-id="5216b-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="5216b-109">Często właściwości klucza obcego są w stanie w tle, tak aby można było ustawić wartości klasę anonimowego należy używać:</span><span class="sxs-lookup"><span data-stu-id="5216b-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
