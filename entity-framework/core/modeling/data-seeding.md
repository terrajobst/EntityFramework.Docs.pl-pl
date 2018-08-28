---
title: Wstępne wypełnianie danych — EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994482"
---
# <a name="data-seeding"></a><span data-ttu-id="06f81-102">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="06f81-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="06f81-103">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="06f81-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="06f81-104">Wstępne wypełnianie danych umożliwia zapewnienie początkowe dane, aby wypełnić bazę danych.</span><span class="sxs-lookup"><span data-stu-id="06f81-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="06f81-105">W odróżnieniu od w EF6 w programie EF Core wstępne wypełnianie danych jest skojarzony z typem jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="06f81-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="06f81-106">Następnie programu EF Core [migracje](xref:core/managing-schemas/migrations/index) można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="06f81-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="06f81-107">Na przykład można to skonfigurować dane `Blog` w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="06f81-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="06f81-108">Aby dodać jednostki, które mają relację wartości klucza obcego muszą być określone.</span><span class="sxs-lookup"><span data-stu-id="06f81-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="06f81-109">Często właściwości klucza obcego są w stanie w tle, tak aby można było ustawić wartości Klasa anonimowa powinny być używane:</span><span class="sxs-lookup"><span data-stu-id="06f81-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="06f81-110">Po dodaniu jednostki, zaleca się używanie [migracje](xref:core/managing-schemas/migrations/index) Aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="06f81-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="06f81-111">Alternatywnie, można użyć `context.Database.EnsureCreated()` do utworzenia nowej bazy danych zawierającej dane inicjatora, na przykład w przypadku bazy danych testów lub przy użyciu dostawcy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="06f81-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="06f81-112">Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` spowoduje zaktualizowanie żadnego schematu ani w bazie danych inicjatora.</span><span class="sxs-lookup"><span data-stu-id="06f81-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
