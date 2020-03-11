---
title: Microsoft SQL Server dostawcę bazy danych — opcje Azure SQL Database-EF Core
description: Jak określić warstwę usługi i poziom wydajności dla Azure SQL Database za pomocą dostawcy SQL Server Entity Framework Core Database
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417898"
---
# <a name="specifying-azure-sql-database-options"></a>Określanie opcji usługi Azure SQL Database

>[!NOTE]
> Ten interfejs API jest nowy w EF Core 3,1.

Azure SQL Database oferuje [różne opcje cenowe](https://azure.microsoft.com/pricing/details/sql-database/single/) , które zwykle są konfigurowane za pomocą witryny Azure Portal. Jeśli jednak zarządzasz schemat przy użyciu [EF Core migracji](xref:core/managing-schemas/migrations/index) , możesz określić odpowiednie opcje w samym modelu.

Możesz określić warstwę usługi bazy danych (wydanie) za pomocą [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

Możesz określić maksymalny rozmiar bazy danych przy użyciu [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

Możesz określić poziom wydajności bazy danych (SERVICE_OBJECTIVE) za pomocą [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

Użyj [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) , aby skonfigurować pulę elastyczną, ponieważ wartość nie jest literałem ciągu:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> Wszystkie obsługiwane wartości można znaleźć w [dokumentacji ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).