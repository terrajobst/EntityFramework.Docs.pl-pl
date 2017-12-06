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
<a name="custom-migrations-history-table"></a>Tabelę historii migracji niestandardowych
===============================
Domyślnie EF Core przechowuje informacje o z migracji, które zostały zastosowane do bazy danych rejestrowania ich w tabeli o nazwie `__EFMigrationsHistory`. Z różnych powodów można dostosować do własnych potrzeb.

> [!IMPORTANT]
> W przypadku dostosowania tabeli historii migracji *po* stosowania migracji, jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.

<a name="schema-and-table-name"></a>Nazwa schematu i tabeli
----------------------
Można zmienić schematu i tabeli nazwy przy użyciu `MigrationsHistoryTable()` metody w `OnConfiguring()` (lub `ConfigureServices()` na platformy ASP.NET Core). Oto przykład przy użyciu dostawcy programu SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Inne zmiany
-------------
Aby skonfigurować dodatkowe aspekty tabeli, zastąpienia i Zastąp specyficznego dla dostawcy `IHistoryRepository` usługi. Oto przykład zmiany nazwy kolumny MigrationId do *identyfikator* na serwerze SQL.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`znajduje się wewnątrz wewnętrznego obszaru nazw i mogą ulec zmianie w przyszłych wersjach.

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
