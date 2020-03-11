---
title: Co nowego w EF Core 1,1 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417518"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="67cb4-102">Nowe funkcje w EF Core 1,1</span><span class="sxs-lookup"><span data-stu-id="67cb4-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="67cb4-103">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="67cb4-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="67cb4-104">Mapowanie pól</span><span class="sxs-lookup"><span data-stu-id="67cb4-104">Field mapping</span></span>

<span data-ttu-id="67cb4-105">Umożliwia skonfigurowanie pola zapasowego dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="67cb4-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="67cb4-106">Może to być przydatne w przypadku właściwości tylko do odczytu lub danych, które mają metody get/set zamiast właściwości.</span><span class="sxs-lookup"><span data-stu-id="67cb4-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="67cb4-107">Mapowanie do tabel zoptymalizowanych pod kątem pamięci w SQL Server</span><span class="sxs-lookup"><span data-stu-id="67cb4-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="67cb4-108">Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="67cb4-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="67cb4-109">W przypadku korzystania z EF Core do tworzenia i konserwowania bazy danych na podstawie modelu (z migracją lub `Database.EnsureCreated()`) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="67cb4-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="67cb4-110">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="67cb4-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="67cb4-111">Dodatkowe interfejsy API śledzenia zmian z EF6</span><span class="sxs-lookup"><span data-stu-id="67cb4-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="67cb4-112">Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itd.</span><span class="sxs-lookup"><span data-stu-id="67cb4-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="67cb4-113">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="67cb4-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="67cb4-114">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="67cb4-114">Explicit Loading</span></span>

<span data-ttu-id="67cb4-115">Umożliwia wyzwalanie wypełniania właściwości nawigacji w jednostce, która została wcześniej załadowana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67cb4-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="67cb4-116">Nieogólnymi. Find</span><span class="sxs-lookup"><span data-stu-id="67cb4-116">DbSet.Find</span></span>

<span data-ttu-id="67cb4-117">Zapewnia łatwy sposób pobierania jednostki na podstawie jej wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="67cb4-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="67cb4-118">Inne</span><span class="sxs-lookup"><span data-stu-id="67cb4-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="67cb4-119">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="67cb4-119">Connection resiliency</span></span>

<span data-ttu-id="67cb4-120">Automatyczne ponawianie próby nieudanych prób wykonania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67cb4-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="67cb4-121">Jest to szczególnie przydatne w przypadku połączeń z usługą SQL Azure, w przypadku których błędy przejściowe są wspólne.</span><span class="sxs-lookup"><span data-stu-id="67cb4-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="67cb4-122">Uproszczone zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="67cb4-122">Simplified service replacement</span></span>

<span data-ttu-id="67cb4-123">Ułatwia zastępowanie usług wewnętrznych używanych przez program Dr.</span><span class="sxs-lookup"><span data-stu-id="67cb4-123">Makes it easier to replace internal services that EF uses.</span></span>
