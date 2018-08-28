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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="ef119-102">Migracje z wielu dostawców</span><span class="sxs-lookup"><span data-stu-id="ef119-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="ef119-103">[EF Core Tools] [ 1] tylko tworzenia szkieletu migracje active dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ef119-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="ef119-104">Czasami jednak warto użyć więcej niż jednego dostawcę (na przykład Microsoft SQL Server i bazy danych SQLite) z Twojego DbContext.</span><span class="sxs-lookup"><span data-stu-id="ef119-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="ef119-105">Istnieją dwa sposoby określają je za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="ef119-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="ef119-106">Dwa zestawy można zachować na potrzeby migracji — jeden dla każdego dostawcy — lub scalania ich do jednego zestawu, który działa na obu.</span><span class="sxs-lookup"><span data-stu-id="ef119-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="ef119-107">Dwa zestawy migracji</span><span class="sxs-lookup"><span data-stu-id="ef119-107">Two migration sets</span></span>
------------------
<span data-ttu-id="ef119-108">Pierwszym sposobem służy do generowania dwie migracje dla każdej zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="ef119-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="ef119-109">Jednym ze sposobów, aby zrobić to polega na umieszczeniu każdy zestaw migracji [w osobnym zestawie] [ 2] i ręcznie przełączać między dodawaniem dwie migracje active dostawcy (i zestawu migracji).</span><span class="sxs-lookup"><span data-stu-id="ef119-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="ef119-110">Jest innym podejściem, która ułatwia pracę z narzędziami do tworzenia nowego typu, który dziedziczy z typu DbContext i zastępuje active dostawcy.</span><span class="sxs-lookup"><span data-stu-id="ef119-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="ef119-111">Ten typ jest używany w projekt czas podczas dodawania lub zastosowanie migracji.</span><span class="sxs-lookup"><span data-stu-id="ef119-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="ef119-112">Ponieważ każdy zestaw migracji wykorzystuje swoje własne typy DbContext, to podejście nie wymaga użycia zestawu oddzielne migracje.</span><span class="sxs-lookup"><span data-stu-id="ef119-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="ef119-113">Podczas dodawania nowej migracji, określić typy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ef119-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="ef119-114">Nie potrzebujesz określić katalog wyjściowy dla następnej migracji, ponieważ są one tworzone jako elementy równorzędne do ostatni z nich.</span><span class="sxs-lookup"><span data-stu-id="ef119-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="ef119-115">Zestaw jedną migrację</span><span class="sxs-lookup"><span data-stu-id="ef119-115">One migration set</span></span>
-----------------
<span data-ttu-id="ef119-116">Jeśli nie masz dwa zestawy na potrzeby migracji, należy ręcznie połączyć je w jeden zestaw, który można zastosować do obu dostawców.</span><span class="sxs-lookup"><span data-stu-id="ef119-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="ef119-117">Adnotacje mogą współistnieć, ponieważ dostawca ignoruje wszelkie adnotacji, które go nie rozumie.</span><span class="sxs-lookup"><span data-stu-id="ef119-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="ef119-118">Na przykład kolumna klucza podstawowego, który działa zarówno z programu Microsoft SQL Server i bazy danych SQLite może wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="ef119-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="ef119-119">Jeśli operacje mogą być stosowane tylko dla jednego dostawcy (lub są one między dostawcami), użyj `ActiveProvider` właściwość, aby sprawdzić, który dostawca jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="ef119-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
