---
title: Program Microsoft SQL Server bazy danych dostawcy — tabele zoptymalizowane pod kątem pamięci — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995805"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="88512-102">Tabele zoptymalizowane pod kątem pamięci, obsługa w dostawcy bazy danych programu SQL Server programu EF Core</span><span class="sxs-lookup"><span data-stu-id="88512-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="88512-103">Ta funkcja została wprowadzona w programie EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="88512-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="88512-104">[Zoptymalizowane pod kątem pamięci tabeli](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją programu SQL Server, w którym cała tabela znajduje się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="88512-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="88512-105">Drugą kopię danych tabeli jest zachowywana na dysku, ale tylko na potrzeby trwałości.</span><span class="sxs-lookup"><span data-stu-id="88512-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="88512-106">Dane w tabelach zoptymalizowanych pod kątem pamięci jest tylko do odczytu z dysku podczas odzyskiwania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88512-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="88512-107">Na przykład po serwera należy uruchomić ponownie.</span><span class="sxs-lookup"><span data-stu-id="88512-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="88512-108">Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci</span><span class="sxs-lookup"><span data-stu-id="88512-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="88512-109">Można określić, czy jednostka jest zamapowana do tabel jest zoptymalizowane pod kątem pamięci.</span><span class="sxs-lookup"><span data-stu-id="88512-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="88512-110">Podczas tworzenia i bazę danych przy użyciu programu EF Core na podstawie modelu (przy użyciu migracji lub `Database.EnsureCreated()`), zoptymalizowana pod kątem pamięci tabeli zostanie utworzony dla tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="88512-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
