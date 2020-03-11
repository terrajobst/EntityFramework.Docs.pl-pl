---
title: Migracja z wieloma dostawcami — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: efe95893f7dbfc8e5c4775e86d58abb32eee3c83
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416788"
---
# <a name="migrations-with-multiple-providers"></a>Migracja z wieloma dostawcami

[Narzędzia EF Core][1] dotyczą tylko migracji szkieletowej dla aktywnego dostawcy. Czasami jednak może być konieczne użycie więcej niż jednego dostawcy (na przykład Microsoft SQL Server i oprogramowania SQLite) w kontekście DbContext. Istnieją dwa sposoby obsługi tego w przypadku migracji. Można zachować dwa zestawy migracji — jeden dla każdego dostawcy — lub scalić je w jeden zestaw, który może współpracować z obydwoma.

## <a name="two-migration-sets"></a>Dwa zestawy migracji

W pierwszej kolejności są generowane dwie migracje dla każdej zmiany modelu.

Jednym ze sposobów jest umieszczenie każdego zestawu migracji [w osobnym zestawie][2] i ręczne przełączenie aktywnego dostawcy (i zestawu migracji) między dodawaniem dwóch migracji.

Innym podejściem, które ułatwia pracę z narzędziami, jest utworzenie nowego typu pochodzącego z kontekstu DbContext i zastąpienie aktywnego dostawcy. Ten typ jest używany w czasie projektowania podczas dodawania lub stosowania migracji.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Ponieważ każdy zestaw migracji używa własnych typów DbContext, to podejście nie wymaga użycia oddzielnego zestawu migracji.

Podczas dodawania nowej migracji należy określić typy kontekstowe.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> Nie musisz określać katalogu wyjściowego dla kolejnych migracji, ponieważ są one tworzone jako elementy równorzędne do ostatniego.

## <a name="one-migration-set"></a>Jeden zestaw migracji

Jeśli nie chcesz mieć dwóch zestawów migracji, możesz połączyć je ręcznie w jeden zestaw, który można zastosować do obu dostawców.

Adnotacje mogą współistnieć, ponieważ dostawca ignoruje wszelkie adnotacje, które nie są zrozumiałe. Na przykład kolumna klucza podstawowego, która działa z obydwoma Microsoft SQL Server i SQLite może wyglądać następująco.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Jeśli operacje mogą być stosowane tylko dla jednego dostawcy (lub różnią się między dostawcami), użyj właściwości `ActiveProvider`, aby określić, który dostawca jest aktywny.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
