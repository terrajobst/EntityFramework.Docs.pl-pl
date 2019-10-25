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
# <a name="custom-migrations-history-table"></a>Tabela historii niestandardowych migracji

Domyślnie program EF Core śledzi, które migracje zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`. Z różnych powodów możesz chcieć dostosować tę tabelę, aby lepiej odpowiadała Twoim potrzebom.

> [!IMPORTANT]
> Jeśli dostosowano tabelę historii migracji *po* zastosowaniu migracji, użytkownik jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.

## <a name="schema-and-table-name"></a>Nazwa schematu i tabeli

Można zmienić nazwę schematu i tabeli przy użyciu metody `MigrationsHistoryTable()` w `OnConfiguring()` (lub `ConfigureServices()` na ASP.NET Core). Oto przykład użycia dostawcy EF Core SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Inne zmiany

Aby skonfigurować dodatkowe aspekty tabeli, Przesłoń i Zastąp `IHistoryRepository` usługę specyficzną dla dostawcy. Oto przykład zmiany nazwy kolumny MigrationId na *Identyfikator* na SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznej przestrzeni nazw i może ulec zmianie w przyszłych wersjach.

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
