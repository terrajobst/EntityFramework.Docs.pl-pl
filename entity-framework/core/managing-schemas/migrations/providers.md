---
title: "Migracje z wielu dostawców - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="1e584-102">Migracje z wielu dostawców</span><span class="sxs-lookup"><span data-stu-id="1e584-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="1e584-103">[EF podstawowe narzędzia] [ 1] tylko utworzyć szkielet migracji active dostawcy.</span><span class="sxs-lookup"><span data-stu-id="1e584-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="1e584-104">Czasami jednak warto użyć więcej niż jednego dostawcę (na przykład Microsoft SQL Server i bazy danych SQLite) z Twojego DbContext.</span><span class="sxs-lookup"><span data-stu-id="1e584-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="1e584-105">Istnieją dwa sposoby obsługi to z migracji.</span><span class="sxs-lookup"><span data-stu-id="1e584-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="1e584-106">Możesz zachować dwa zestawy migracji — po jednej dla każdego dostawcy — lub scalania je w pojedynczy zestaw działa na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1e584-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="1e584-107">Dwa zestawy migracji</span><span class="sxs-lookup"><span data-stu-id="1e584-107">Two migration sets</span></span>
------------------
<span data-ttu-id="1e584-108">Pierwszym sposobem służy do generowania dwóch migracje dla każdej zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="1e584-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="1e584-109">Aby wykonać to jest umieszczenie każdego zestawu migracji [w osobny zestaw] [ 2] i ręcznie przełączać między dwiema migracjami dodawaniem active dostawcy (i zestawu migracji).</span><span class="sxs-lookup"><span data-stu-id="1e584-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="1e584-110">Innym rozwiązaniem jest sprawia, że pracy przy użyciu narzędzi do utworzenia nowego typu, który pochodzi od użytkownika DbContext i zastępuje active dostawcy.</span><span class="sxs-lookup"><span data-stu-id="1e584-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="1e584-111">Ten typ jest używany na projekt czas podczas dodawania lub stosowania migracji.</span><span class="sxs-lookup"><span data-stu-id="1e584-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="1e584-112">Ponieważ każdy zestaw migracji używa własnego typy DbContext, to rozwiązanie nie wymaga użycia zestawu oddzielne migracji.</span><span class="sxs-lookup"><span data-stu-id="1e584-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="1e584-113">Podczas dodawania nowych migracji, należy określić typów kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1e584-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="1e584-114">Nie trzeba określić katalog wyjściowy dla następnej migracji, ponieważ są one tworzone jako elementy równorzędne do ostatnio.</span><span class="sxs-lookup"><span data-stu-id="1e584-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="1e584-115">Zestaw jedną migrację</span><span class="sxs-lookup"><span data-stu-id="1e584-115">One migration set</span></span>
-----------------
<span data-ttu-id="1e584-116">Jeśli nie lubisz, mając dwa zestawy migracji, należy ręcznie połączyć je w pojedynczy zestaw, który można zastosować do obu dostawców.</span><span class="sxs-lookup"><span data-stu-id="1e584-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="1e584-117">Adnotacje mogą współistnieć, ponieważ dostawca ignoruje wszystkie adnotacje, które go nie rozpoznaje.</span><span class="sxs-lookup"><span data-stu-id="1e584-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="1e584-118">Na przykład kolumna klucza podstawowego, który działa zarówno z programu Microsoft SQL Server i bazy danych SQLite może wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="1e584-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="1e584-119">Jeśli operacje mogą być stosowane tylko dla jednego dostawcy (lub są one od dostawcy), należy użyć `ActiveProvider` właściwość, aby sprawdzić, który dostawca jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="1e584-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
