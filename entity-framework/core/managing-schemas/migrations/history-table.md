---
title: Tabela historii niestandardowych migracji — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812072"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="ba9b9-102">Tabela historii niestandardowych migracji</span><span class="sxs-lookup"><span data-stu-id="ba9b9-102">Custom Migrations History Table</span></span>

<span data-ttu-id="ba9b9-103">Domyślnie program EF Core śledzi, które migracje zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="ba9b9-104">Z różnych powodów możesz chcieć dostosować tę tabelę, aby lepiej odpowiadała Twoim potrzebom.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba9b9-105">Jeśli dostosowano tabelę historii migracji *po* zastosowaniu migracji, użytkownik jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="ba9b9-106">Nazwa schematu i tabeli</span><span class="sxs-lookup"><span data-stu-id="ba9b9-106">Schema and table name</span></span>

<span data-ttu-id="ba9b9-107">Można zmienić nazwę schematu i tabeli przy użyciu metody `MigrationsHistoryTable()` w `OnConfiguring()` (lub `ConfigureServices()` na ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="ba9b9-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="ba9b9-108">Oto przykład użycia dostawcy EF Core SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="ba9b9-109">Inne zmiany</span><span class="sxs-lookup"><span data-stu-id="ba9b9-109">Other changes</span></span>

<span data-ttu-id="ba9b9-110">Aby skonfigurować dodatkowe aspekty tabeli, Przesłoń i Zastąp `IHistoryRepository` usługę specyficzną dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="ba9b9-111">Oto przykład zmiany nazwy kolumny MigrationId na *Identyfikator* na SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="ba9b9-112">`SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznej przestrzeni nazw i może ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="ba9b9-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
