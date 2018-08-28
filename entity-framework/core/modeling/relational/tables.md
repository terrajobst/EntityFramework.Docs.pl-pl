---
title: Mapowanie tabeli — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994140"
---
# <a name="table-mapping"></a><span data-ttu-id="e63ab-102">Mapowanie tabeli</span><span class="sxs-lookup"><span data-stu-id="e63ab-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="e63ab-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="e63ab-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e63ab-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="e63ab-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e63ab-105">Mapowanie tabeli identyfikuje dane, które tabeli powinien być odpytywane i zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e63ab-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e63ab-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="e63ab-106">Conventions</span></span>

<span data-ttu-id="e63ab-107">Zgodnie z Konwencją każdej jednostki będą konfigurowane tak, aby zamapować na tabelę z taką samą nazwę jak `DbSet<TEntity>` właściwość, która udostępnia jednostki w kontekście pochodnych.</span><span class="sxs-lookup"><span data-stu-id="e63ab-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="e63ab-108">Jeśli nie `DbSet<TEntity>` jest dołączony do danej jednostki, nazwa klasy jest używana.</span><span class="sxs-lookup"><span data-stu-id="e63ab-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e63ab-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="e63ab-109">Data Annotations</span></span>

<span data-ttu-id="e63ab-110">Korzystanie z adnotacji danych, aby skonfigurować tabelę, która mapuje typu.</span><span class="sxs-lookup"><span data-stu-id="e63ab-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="e63ab-111">Można również określić schemat, który należy do tabeli.</span><span class="sxs-lookup"><span data-stu-id="e63ab-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="e63ab-112">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="e63ab-112">Fluent API</span></span>

<span data-ttu-id="e63ab-113">Interfejs Fluent API umożliwiają skonfigurowanie tabeli, która mapuje typu.</span><span class="sxs-lookup"><span data-stu-id="e63ab-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="e63ab-114">Można również określić schemat, który należy do tabeli.</span><span class="sxs-lookup"><span data-stu-id="e63ab-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
