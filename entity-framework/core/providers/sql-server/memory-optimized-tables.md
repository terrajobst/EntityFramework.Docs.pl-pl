---
title: "Microsoft SQL Server bazy danych dostawcy - tabel zoptymalizowanych pod kątem pamięci - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="d2719-102">Tabele zoptymalizowane pod kątem pamięci Obsługa w dostawcy bazy danych programu SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="d2719-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="d2719-103">Ta funkcja została wprowadzona w EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="d2719-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="d2719-104">[Pamięć — tabel zoptymalizowanych pod kątem](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją programu SQL Server, w którym cała tabela znajduje się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2719-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="d2719-105">Drugą kopię danych tabeli jest zachowywana na dysku, ale tylko na potrzeby trwałości.</span><span class="sxs-lookup"><span data-stu-id="d2719-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="d2719-106">Dane w tabelach zoptymalizowanych pod kątem pamięci jest tylko do odczytu z dysku podczas odzyskiwania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2719-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="d2719-107">Na przykład po serwera należy ponownie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="d2719-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="d2719-108">Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci</span><span class="sxs-lookup"><span data-stu-id="d2719-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="d2719-109">Można określić, że jednostka jest zamapowana do tabeli jest zoptymalizowany pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="d2719-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="d2719-110">Gdy przy użyciu EF Core można tworzyć i obsługiwać bazy danych na podstawie modelu (za pomocą migracji lub `Database.EnsureCreated()`), zostanie utworzona tabeli zoptymalizowanej pod kątem pamięci dla tych obiektów.</span><span class="sxs-lookup"><span data-stu-id="d2719-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
