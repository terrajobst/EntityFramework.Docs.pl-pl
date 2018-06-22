---
title: Mapowania tabeli - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054182"
---
# <a name="table-mapping"></a><span data-ttu-id="c18b2-102">Mapowania tabeli</span><span class="sxs-lookup"><span data-stu-id="c18b2-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="c18b2-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="c18b2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c18b2-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="c18b2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c18b2-105">Mapowania tabeli identyfikuje danych tabeli należy z kwerendy i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c18b2-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c18b2-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c18b2-106">Conventions</span></span>

<span data-ttu-id="c18b2-107">Według Konwencji każdej jednostki będzie Instalatora, aby mapować do tabeli o takiej samej nazwie jak `DbSet<TEntity>` właściwość, która przedstawia jednostek w kontekście pochodnych.</span><span class="sxs-lookup"><span data-stu-id="c18b2-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="c18b2-108">Jeśli nie `DbSet<TEntity>` wchodzi dla danej jednostki, jest używana nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="c18b2-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c18b2-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="c18b2-109">Data Annotations</span></span>

<span data-ttu-id="c18b2-110">Adnotacje danych służy do konfigurowania tabeli, która mapuje typu.</span><span class="sxs-lookup"><span data-stu-id="c18b2-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="c18b2-111">Można również określić schemat, który należy do tabeli.</span><span class="sxs-lookup"><span data-stu-id="c18b2-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c18b2-112">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="c18b2-112">Fluent API</span></span>

<span data-ttu-id="c18b2-113">Aby skonfigurować tabelę, która mapuje typu, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="c18b2-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="c18b2-114">Można również określić schemat, który należy do tabeli.</span><span class="sxs-lookup"><span data-stu-id="c18b2-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
