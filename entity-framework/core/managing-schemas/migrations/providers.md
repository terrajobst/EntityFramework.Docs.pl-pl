---
title: Migracje z wielu dostawców — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997976"
---
<a name="migrations-with-multiple-providers"></a>Migracje z wielu dostawców
==================================
[EF Core Tools] [ 1] tylko tworzenia szkieletu migracje active dostawcy. Czasami jednak warto użyć więcej niż jednego dostawcę (na przykład Microsoft SQL Server i bazy danych SQLite) z Twojego DbContext. Istnieją dwa sposoby określają je za pomocą migracji. Dwa zestawy można zachować na potrzeby migracji — jeden dla każdego dostawcy — lub scalania ich do jednego zestawu, który działa na obu.

<a name="two-migration-sets"></a>Dwa zestawy migracji
------------------
Pierwszym sposobem służy do generowania dwie migracje dla każdej zmiany modelu.

Jednym ze sposobów, aby zrobić to polega na umieszczeniu każdy zestaw migracji [w osobnym zestawie] [ 2] i ręcznie przełączać między dodawaniem dwie migracje active dostawcy (i zestawu migracji).

Jest innym podejściem, która ułatwia pracę z narzędziami do tworzenia nowego typu, który dziedziczy z typu DbContext i zastępuje active dostawcy. Ten typ jest używany w projekt czas podczas dodawania lub zastosowanie migracji.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Ponieważ każdy zestaw migracji wykorzystuje swoje własne typy DbContext, to podejście nie wymaga użycia zestawu oddzielne migracje.

Podczas dodawania nowej migracji, określić typy kontekstu.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Nie potrzebujesz określić katalog wyjściowy dla następnej migracji, ponieważ są one tworzone jako elementy równorzędne do ostatni z nich.

<a name="one-migration-set"></a>Zestaw jedną migrację
-----------------
Jeśli nie masz dwa zestawy na potrzeby migracji, należy ręcznie połączyć je w jeden zestaw, który można zastosować do obu dostawców.

Adnotacje mogą współistnieć, ponieważ dostawca ignoruje wszelkie adnotacji, które go nie rozumie. Na przykład kolumna klucza podstawowego, który działa zarówno z programu Microsoft SQL Server i bazy danych SQLite może wyglądać następująco.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Jeśli operacje mogą być stosowane tylko dla jednego dostawcy (lub są one między dostawcami), użyj `ActiveProvider` właściwość, aby sprawdzić, który dostawca jest aktywny.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
