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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Tabele zoptymalizowane pod kątem pamięci, obsługa w dostawcy bazy danych programu SQL Server programu EF Core

> [!NOTE]  
>
> Ta funkcja została wprowadzona w programie EF Core 1.1.

[Zoptymalizowane pod kątem pamięci tabeli](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją programu SQL Server, w którym cała tabela znajduje się w pamięci. Drugą kopię danych tabeli jest zachowywana na dysku, ale tylko na potrzeby trwałości. Dane w tabelach zoptymalizowanych pod kątem pamięci jest tylko do odczytu z dysku podczas odzyskiwania bazy danych. Na przykład po serwera należy uruchomić ponownie.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci

Można określić, czy jednostka jest zamapowana do tabel jest zoptymalizowane pod kątem pamięci. Podczas tworzenia i bazę danych przy użyciu programu EF Core na podstawie modelu (przy użyciu migracji lub `Database.EnsureCreated()`), zoptymalizowana pod kątem pamięci tabeli zostanie utworzony dla tych jednostek.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
