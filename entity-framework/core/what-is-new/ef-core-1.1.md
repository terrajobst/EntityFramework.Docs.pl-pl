---
title: Co nowego w EF Core 1,1 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656009"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="a6fa0-102">Nowe funkcje w EF Core 1,1</span><span class="sxs-lookup"><span data-stu-id="a6fa0-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="a6fa0-103">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="a6fa0-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="a6fa0-104">Mapowanie pól</span><span class="sxs-lookup"><span data-stu-id="a6fa0-104">Field mapping</span></span>

<span data-ttu-id="a6fa0-105">Umożliwia skonfigurowanie pola zapasowego dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="a6fa0-106">Może to być przydatne w przypadku właściwości tylko do odczytu lub danych, które mają metody get/set zamiast właściwości.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="a6fa0-107">Mapowanie do tabel zoptymalizowanych pod kątem pamięci w SQL Server</span><span class="sxs-lookup"><span data-stu-id="a6fa0-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="a6fa0-108">Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="a6fa0-109">W przypadku korzystania z EF Core do tworzenia i konserwowania bazy danych na podstawie modelu (z migracją lub `Database.EnsureCreated()`) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="a6fa0-110">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="a6fa0-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="a6fa0-111">Dodatkowe interfejsy API śledzenia zmian z EF6</span><span class="sxs-lookup"><span data-stu-id="a6fa0-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="a6fa0-112">Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itd.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="a6fa0-113">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="a6fa0-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="a6fa0-114">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="a6fa0-114">Explicit Loading</span></span>

<span data-ttu-id="a6fa0-115">Umożliwia wyzwalanie wypełniania właściwości nawigacji w jednostce, która została wcześniej załadowana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="a6fa0-116">Nieogólnymi. Find</span><span class="sxs-lookup"><span data-stu-id="a6fa0-116">DbSet.Find</span></span>

<span data-ttu-id="a6fa0-117">Zapewnia łatwy sposób pobierania jednostki na podstawie jej wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="a6fa0-118">Inne</span><span class="sxs-lookup"><span data-stu-id="a6fa0-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="a6fa0-119">Odporność połączenia</span><span class="sxs-lookup"><span data-stu-id="a6fa0-119">Connection resiliency</span></span>

<span data-ttu-id="a6fa0-120">Automatyczne ponawianie próby nieudanych prób wykonania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="a6fa0-121">Jest to szczególnie przydatne w przypadku połączeń z usługą SQL Azure, w przypadku których błędy przejściowe są wspólne.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="a6fa0-122">Uproszczone zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="a6fa0-122">Simplified service replacement</span></span>

<span data-ttu-id="a6fa0-123">Ułatwia zastępowanie usług wewnętrznych używanych przez program Dr.</span><span class="sxs-lookup"><span data-stu-id="a6fa0-123">Makes it easier to replace internal services that EF uses.</span></span>
