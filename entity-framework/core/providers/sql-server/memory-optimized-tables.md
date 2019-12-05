---
title: Dostawca bazy danych Microsoft SQL Server — tabele zoptymalizowane pod kątem pamięci — EF Core
description: Jak używać tabel zoptymalizowanych pod kątem pamięci z dostawcą bazy danych Entity Framework Core SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824647"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="773e2-103">Obsługa tabel zoptymalizowanych pod kątem pamięci w SQL Server EF Core dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="773e2-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="773e2-104">[Tabele zoptymalizowane pod kątem pamięci](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją SQL Server, w której cała tabela znajduje się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="773e2-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="773e2-105">Druga kopia danych tabeli jest utrzymywana na dysku, ale tylko w celach trwałości.</span><span class="sxs-lookup"><span data-stu-id="773e2-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="773e2-106">Dane w tabelach zoptymalizowanych pod kątem pamięci są odczytywane tylko z dysku podczas odzyskiwania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="773e2-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="773e2-107">Na przykład po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="773e2-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="773e2-108">Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci</span><span class="sxs-lookup"><span data-stu-id="773e2-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="773e2-109">Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="773e2-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="773e2-110">W przypadku korzystania z EF Core do tworzenia i konserwowania bazy danych opartej na modelu (z [migracją](xref:core/managing-schemas/migrations/index) lub [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="773e2-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
