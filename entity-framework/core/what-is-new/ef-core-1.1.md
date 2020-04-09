---
title: Co nowego w EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417518"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="15cf2-102">Nowe funkcje w EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="15cf2-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="15cf2-103">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="15cf2-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="15cf2-104">Mapowanie pola</span><span class="sxs-lookup"><span data-stu-id="15cf2-104">Field mapping</span></span>

<span data-ttu-id="15cf2-105">Umożliwia skonfigurowanie pola zapasowego dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="15cf2-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="15cf2-106">Może to być przydatne dla właściwości tylko do odczytu lub danych, które mają Get/Set metody, a nie właściwości.</span><span class="sxs-lookup"><span data-stu-id="15cf2-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="15cf2-107">Mapowanie do tabel zoptymalizowanych pod kątem pamięci w programie SQL Server</span><span class="sxs-lookup"><span data-stu-id="15cf2-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="15cf2-108">Można określić, że tabela, do której jest mapowana encja, jest zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="15cf2-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="15cf2-109">Podczas korzystania z EF Core do tworzenia i obsługi bazy danych `Database.EnsureCreated()`na podstawie modelu (z migracji lub), tabela zoptymalizowana pod kątem pamięci zostaną utworzone dla tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="15cf2-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="15cf2-110">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="15cf2-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="15cf2-111">Dodatkowe interfejsy API śledzenia zmian z ef6</span><span class="sxs-lookup"><span data-stu-id="15cf2-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="15cf2-112">Takie `Reload`jak `GetModifiedProperties` `GetDatabaseValues` , , itp.</span><span class="sxs-lookup"><span data-stu-id="15cf2-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="15cf2-113">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="15cf2-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="15cf2-114">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="15cf2-114">Explicit Loading</span></span>

<span data-ttu-id="15cf2-115">Umożliwia wyzwolenie zapełnienia właściwości nawigacji na encji, która została wcześniej załadowana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15cf2-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="15cf2-116">DbSet.Find (Znajdowanie zestawu DbSet.Find)</span><span class="sxs-lookup"><span data-stu-id="15cf2-116">DbSet.Find</span></span>

<span data-ttu-id="15cf2-117">Zapewnia łatwy sposób pobierania jednostki na podstawie jego wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="15cf2-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="15cf2-118">Inne</span><span class="sxs-lookup"><span data-stu-id="15cf2-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="15cf2-119">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="15cf2-119">Connection resiliency</span></span>

<span data-ttu-id="15cf2-120">Automatycznie ponawia ponawia nieudane polecenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15cf2-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="15cf2-121">Jest to szczególnie przydatne podczas połączenia z platformą SQL Azure, gdzie błędy przejściowe są typowe.</span><span class="sxs-lookup"><span data-stu-id="15cf2-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="15cf2-122">Uproszczona wymiana usług</span><span class="sxs-lookup"><span data-stu-id="15cf2-122">Simplified service replacement</span></span>

<span data-ttu-id="15cf2-123">Ułatwia zastąpienie usług wewnętrznych, które ef używa.</span><span class="sxs-lookup"><span data-stu-id="15cf2-123">Makes it easier to replace internal services that EF uses.</span></span>
