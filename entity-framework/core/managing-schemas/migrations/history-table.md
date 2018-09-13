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
