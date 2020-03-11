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
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="bc89f-103">Określanie opcji usługi Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="bc89f-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="bc89f-104">Ten interfejs API jest nowy w EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="bc89f-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="bc89f-105">Azure SQL Database oferuje [różne opcje cenowe](https://azure.microsoft.com/pricing/details/sql-database/single/) , które zwykle są konfigurowane za pomocą witryny Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="bc89f-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="bc89f-106">Jeśli jednak zarządzasz schemat przy użyciu [EF Core migracji](xref:core/managing-schemas/migrations/index) , możesz określić odpowiednie opcje w samym modelu.</span><span class="sxs-lookup"><span data-stu-id="bc89f-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="bc89f-107">Możesz określić warstwę usługi bazy danych (wydanie) za pomocą [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="bc89f-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="bc89f-108">Możesz określić maksymalny rozmiar bazy danych przy użyciu [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="bc89f-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="bc89f-109">Możesz określić poziom wydajności bazy danych (SERVICE_OBJECTIVE) za pomocą [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="bc89f-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="bc89f-110">Użyj [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) , aby skonfigurować pulę elastyczną, ponieważ wartość nie jest literałem ciągu:</span><span class="sxs-lookup"><span data-stu-id="bc89f-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="bc89f-111">Wszystkie obsługiwane wartości można znaleźć w [dokumentacji ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="bc89f-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>