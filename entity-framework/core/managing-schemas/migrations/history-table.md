---
title: Tabela historii migracje niestandardowe — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488819"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="1679b-102">Tabela historii migracje niestandardowe</span><span class="sxs-lookup"><span data-stu-id="1679b-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="1679b-103">Domyślnie śledzi informacje o programu EF Core migracji, które zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="1679b-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="1679b-104">Z różnych powodów można dostosować do własnych potrzeb.</span><span class="sxs-lookup"><span data-stu-id="1679b-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1679b-105">W przypadku dostosowania tabeli historii migracje *po* z zastosowaniem migracje, jesteś odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1679b-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="1679b-106">Nazwa schematu i tabeli</span><span class="sxs-lookup"><span data-stu-id="1679b-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="1679b-107">Można zmienić schematu i tabeli przy użyciu nazwy `MigrationsHistoryTable()` method in Class metoda `OnConfiguring()` (lub `ConfigureServices()` programu ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="1679b-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="1679b-108">Oto przykład przy użyciu dostawcy programu SQL Server programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1679b-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="1679b-109">Inne zmiany</span><span class="sxs-lookup"><span data-stu-id="1679b-109">Other changes</span></span>
-------------
<span data-ttu-id="1679b-110">Aby skonfigurować dodatkowe aspekty tabeli, należy zastąpić i Zastąp właściwe dla dostawcy `IHistoryRepository` usługi.</span><span class="sxs-lookup"><span data-stu-id="1679b-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="1679b-111">Oto przykład zmiany MigrationId nazwa kolumny, która ma *identyfikator* w programie SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1679b-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="1679b-112">`SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznego obszaru nazw i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="1679b-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
