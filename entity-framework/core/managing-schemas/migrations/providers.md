---
title: Migracje z wielu dostawców - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002808"
---
<a name="migrations-with-multiple-providers"></a>Migracje z wielu dostawców
==================================
[EF podstawowe narzędzia] [ 1] tylko utworzyć szkielet migracji active dostawcy. Czasami jednak warto użyć więcej niż jednego dostawcę (na przykład Microsoft SQL Server i bazy danych SQLite) z Twojego DbContext. Istnieją dwa sposoby obsługi to z migracji. Możesz zachować dwa zestawy migracji — po jednej dla każdego dostawcy — lub scalania je w pojedynczy zestaw działa na serwerze.

<a name="two-migration-sets"></a>Dwa zestawy migracji
------------------
Pierwszym sposobem służy do generowania dwóch migracje dla każdej zmiany modelu.

Aby wykonać to jest umieszczenie każdego zestawu migracji [w osobny zestaw] [ 2] i ręcznie przełączać między dwiema migracjami dodawaniem active dostawcy (i zestawu migracji).

Innym rozwiązaniem jest sprawia, że pracy przy użyciu narzędzi do utworzenia nowego typu, który pochodzi od użytkownika DbContext i zastępuje active dostawcy. Ten typ jest używany na projekt czas podczas dodawania lub stosowania migracji.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Ponieważ każdy zestaw migracji używa własnego typy DbContext, to rozwiązanie nie wymaga użycia zestawu oddzielne migracji.

Podczas dodawania nowych migracji, należy określić typów kontekstu.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Nie trzeba określić katalog wyjściowy dla następnej migracji, ponieważ są one tworzone jako elementy równorzędne do ostatnio.

<a name="one-migration-set"></a>Zestaw jedną migrację
-----------------
Jeśli nie lubisz, mając dwa zestawy migracji, należy ręcznie połączyć je w pojedynczy zestaw, który można zastosować do obu dostawców.

Adnotacje mogą współistnieć, ponieważ dostawca ignoruje wszystkie adnotacje, które go nie rozpoznaje. Na przykład kolumna klucza podstawowego, który działa zarówno z programu Microsoft SQL Server i bazy danych SQLite może wyglądać następująco.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Jeśli operacje mogą być stosowane tylko dla jednego dostawcy (lub są one od dostawcy), należy użyć `ActiveProvider` właściwość, aby sprawdzić, który dostawca jest aktywny.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
