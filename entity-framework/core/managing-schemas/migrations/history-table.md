---
title: "Tabelę historii migracji niestandardowej - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="a7c86-102">Tabelę historii migracji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="a7c86-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="a7c86-103">Domyślnie EF Core przechowuje informacje o z migracji, które zostały zastosowane do bazy danych rejestrowania ich w tabeli o nazwie `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="a7c86-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="a7c86-104">Z różnych powodów można dostosować do własnych potrzeb.</span><span class="sxs-lookup"><span data-stu-id="a7c86-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7c86-105">W przypadku dostosowania tabeli historii migracji *po* stosowania migracji, jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a7c86-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="a7c86-106">Nazwa schematu i tabeli</span><span class="sxs-lookup"><span data-stu-id="a7c86-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="a7c86-107">Można zmienić schematu i tabeli nazwy przy użyciu `MigrationsHistoryTable()` metody w `OnConfiguring()` (lub `ConfigureServices()` na platformy ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a7c86-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="a7c86-108">Oto przykład przy użyciu dostawcy programu SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7c86-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="a7c86-109">Inne zmiany</span><span class="sxs-lookup"><span data-stu-id="a7c86-109">Other changes</span></span>
-------------
<span data-ttu-id="a7c86-110">Aby skonfigurować dodatkowe aspekty tabeli, zastąpienia i Zastąp specyficznego dla dostawcy `IHistoryRepository` usługi.</span><span class="sxs-lookup"><span data-stu-id="a7c86-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="a7c86-111">Oto przykład zmiany nazwy kolumny MigrationId do *identyfikator* na serwerze SQL.</span><span class="sxs-lookup"><span data-stu-id="a7c86-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="a7c86-112">`SqlServerHistoryRepository`znajduje się wewnątrz wewnętrznego obszaru nazw i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="a7c86-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
