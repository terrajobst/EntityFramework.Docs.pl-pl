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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Obsługa tabel zoptymalizowanych pod kątem pamięci w SQL Server EF Core dostawcy bazy danych

[Tabele zoptymalizowane pod kątem pamięci](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją SQL Server, w której cała tabela znajduje się w pamięci. Druga kopia danych tabeli jest utrzymywana na dysku, ale tylko w celach trwałości. Dane w tabelach zoptymalizowanych pod kątem pamięci są odczytywane tylko z dysku podczas odzyskiwania bazy danych. Na przykład po ponownym uruchomieniu serwera.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci

Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci. Przy użyciu EF Core do tworzenia i konserwowania bazy danych opartej na modelu (z migracją lub `Database.EnsureCreated()`) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
