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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Tabele zoptymalizowane pod kątem pamięci Obsługa w dostawcy bazy danych programu SQL Server EF Core

> [!NOTE]  
>
> Ta funkcja została wprowadzona w EF Core 1.1.

[Pamięć — tabel zoptymalizowanych pod kątem](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją programu SQL Server, w którym cała tabela znajduje się w pamięci. Drugą kopię danych tabeli jest zachowywana na dysku, ale tylko na potrzeby trwałości. Dane w tabelach zoptymalizowanych pod kątem pamięci jest tylko do odczytu z dysku podczas odzyskiwania bazy danych. Na przykład po serwera należy ponownie uruchomić.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci

Można określić, że jednostka jest zamapowana do tabeli jest zoptymalizowany pod kątem pamięci. Gdy przy użyciu EF Core można tworzyć i obsługiwać bazy danych na podstawie modelu (za pomocą migracji lub `Database.EnsureCreated()`), zostanie utworzona tabeli zoptymalizowanej pod kątem pamięci dla tych obiektów.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
