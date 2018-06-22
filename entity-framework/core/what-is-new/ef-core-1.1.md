---
title: Co to jest nowa w programie EF 1.1 Core - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054287"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="7b3c1-102">Nowe funkcje w wersji 1.1 Core EF</span><span class="sxs-lookup"><span data-stu-id="7b3c1-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="7b3c1-103">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="7b3c1-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="7b3c1-104">Mapowanie pól</span><span class="sxs-lookup"><span data-stu-id="7b3c1-104">Field mapping</span></span>
<span data-ttu-id="7b3c1-105">Umożliwia skonfigurowanie polem zapasowym dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="7b3c1-106">Może to być przydatne w przypadku właściwości tylko do odczytu lub dane, które zamiast właściwości metod Get/Set.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="7b3c1-107">Mapowanie do tabel zoptymalizowanych pod kątem pamięci w programie SQL Server</span><span class="sxs-lookup"><span data-stu-id="7b3c1-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="7b3c1-108">Można określić, że jednostka jest zamapowana do tabeli jest zoptymalizowany pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="7b3c1-109">Gdy przy użyciu EF Core można tworzyć i obsługiwać bazy danych na podstawie modelu (za pomocą migracji lub `Database.EnsureCreated()`), zostanie utworzona tabeli zoptymalizowanej pod kątem pamięci dla tych obiektów.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="7b3c1-110">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="7b3c1-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="7b3c1-111">Śledzenie interfejsów API z EF6 dodatkowych zmian</span><span class="sxs-lookup"><span data-stu-id="7b3c1-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="7b3c1-112">Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itp.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="7b3c1-113">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="7b3c1-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="7b3c1-114">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="7b3c1-114">Explicit Loading</span></span>
<span data-ttu-id="7b3c1-115">Służy do wyzwalania wypełniania właściwości nawigacji jednostki, które zostało wcześniej załadowane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="7b3c1-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="7b3c1-116">DbSet.Find</span></span>
<span data-ttu-id="7b3c1-117">Zapewnia łatwy sposób pobrać oparte na jego wartości klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="7b3c1-118">Inne</span><span class="sxs-lookup"><span data-stu-id="7b3c1-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="7b3c1-119">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="7b3c1-119">Connection resiliency</span></span>
<span data-ttu-id="7b3c1-120">Automatycznie ponownych prób nie polecenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="7b3c1-121">Jest to szczególnie przydatne podczas połączenia SQL Azure, w których typowych błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="7b3c1-122">Uproszczony wymienna</span><span class="sxs-lookup"><span data-stu-id="7b3c1-122">Simplified service replacement</span></span>
<span data-ttu-id="7b3c1-123">Ułatwia Zastąp wewnętrznych usług, które używa EF.</span><span class="sxs-lookup"><span data-stu-id="7b3c1-123">Makes it easier to replace internal services that EF uses.</span></span>
