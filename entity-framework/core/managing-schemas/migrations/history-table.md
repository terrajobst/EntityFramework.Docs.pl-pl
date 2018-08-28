---
title: Tabela historii migracje niestandardowe — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995699"
---
<a name="custom-migrations-history-table"></a>Tabela historii migracje niestandardowe
===============================
Domyślnie śledzi informacje o programu EF Core migracji, które zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`. Z różnych powodów można dostosować do własnych potrzeb.

> [!IMPORTANT]
> W przypadku dostosowania tabeli historii migracje *po* z zastosowaniem migracje, jesteś odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.

<a name="schema-and-table-name"></a>Nazwa schematu i tabeli
----------------------
Można zmienić schematu i tabeli przy użyciu nazwy `MigrationsHistoryTable()` method in Class metoda `OnConfiguring()` (lub `ConfigureServices()` programu ASP.NET Core). Oto przykład przy użyciu dostawcy programu SQL Server programu EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Inne zmiany
-------------
Aby skonfigurować dodatkowe aspekty tabeli, należy zastąpić i Zastąp właściwe dla dostawcy `IHistoryRepository` usługi. Oto przykład zmiany MigrationId nazwa kolumny, która ma *identyfikator* w programie SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznego obszaru nazw i mogą ulec zmianie w przyszłych wersjach.

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
