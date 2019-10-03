---
title: Dostawca bazy danych Microsoft SQL Server — tabele zoptymalizowane pod kątem pamięci — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813496"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="842f3-102">Obsługa tabel zoptymalizowanych pod kątem pamięci w SQL Server EF Core dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="842f3-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="842f3-103">[Tabele zoptymalizowane pod kątem pamięci](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją SQL Server, w której cała tabela znajduje się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="842f3-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="842f3-104">Druga kopia danych tabeli jest utrzymywana na dysku, ale tylko w celach trwałości.</span><span class="sxs-lookup"><span data-stu-id="842f3-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="842f3-105">Dane w tabelach zoptymalizowanych pod kątem pamięci są odczytywane tylko z dysku podczas odzyskiwania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="842f3-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="842f3-106">Na przykład po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="842f3-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="842f3-107">Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci</span><span class="sxs-lookup"><span data-stu-id="842f3-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="842f3-108">Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="842f3-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="842f3-109">Przy użyciu EF Core do tworzenia i konserwowania bazy danych opartej na modelu (z migracją lub `Database.EnsureCreated()`) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="842f3-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
