---
title: Wstępne wypełnianie danych - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163203"
---
# <a name="data-seeding"></a><span data-ttu-id="3bb6b-102">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="3bb6b-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="3bb6b-103">Ta funkcja jest nowa w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="3bb6b-104">Umożliwia wstępne wypełnianie danych początkowej danych do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="3bb6b-105">W odróżnieniu od w EF6 w podstawowej EF wstępne wypełnianie danych jest skojarzona z typem jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="3bb6b-106">Następnie EF Core [migracje](xref:core/managing-schemas/migrations/index) można automatycznie obliczyć, co insert, update lub delete potrzebę operacje można zastosować podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="3bb6b-107">Na przykład można to skonfigurować dane `Blog` w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="3bb6b-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="3bb6b-108">Aby dodać jednostek, które mają relację z wartości klucza obcego muszą być określone.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="3bb6b-109">Często właściwości klucza obcego są w stanie w tle, tak aby można było ustawić wartości klasę anonimowego należy używać:</span><span class="sxs-lookup"><span data-stu-id="3bb6b-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="3bb6b-110">Po dodaniu jednostek zalecane jest użycie [migracje](xref:core/managing-schemas/migrations/index) Aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="3bb6b-111">Alternatywnie można użyć `context.Database.EnsureCreated()` do utworzenia nowej bazy danych zawierającej dane inicjatora, na przykład dla bazy danych testu lub przy użyciu dostawcy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="3bb6b-112">Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` ani zaktualizuje schematu ani w bazie danych inicjatora.</span><span class="sxs-lookup"><span data-stu-id="3bb6b-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
