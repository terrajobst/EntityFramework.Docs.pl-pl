---
title: Mapowanie tabeli — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196958"
---
# <a name="table-mapping"></a><span data-ttu-id="f7347-102">Mapowanie tabeli</span><span class="sxs-lookup"><span data-stu-id="f7347-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="f7347-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="f7347-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f7347-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="f7347-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f7347-105">Mapowanie tabeli określa, które dane tabeli mają być wysyłane z i zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7347-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f7347-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f7347-106">Conventions</span></span>

<span data-ttu-id="f7347-107">Zgodnie z Konwencją każda jednostka zostanie skonfigurowana do mapowania na tabelę o tej samej nazwie co `DbSet<TEntity>` właściwość, która uwidacznia jednostkę w kontekście pochodnym.</span><span class="sxs-lookup"><span data-stu-id="f7347-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="f7347-108">Jeśli nie `DbSet<TEntity>` jest uwzględniony dla danej jednostki, używana jest nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="f7347-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f7347-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f7347-109">Data Annotations</span></span>

<span data-ttu-id="f7347-110">Możesz użyć adnotacji danych, aby skonfigurować tabelę, do której jest mapowany typ.</span><span class="sxs-lookup"><span data-stu-id="f7347-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="f7347-111">Można również określić schemat, do którego należy tabela.</span><span class="sxs-lookup"><span data-stu-id="f7347-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="f7347-112">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="f7347-112">Fluent API</span></span>

<span data-ttu-id="f7347-113">Korzystając z interfejsu API Fluent, można skonfigurować tabelę, do której jest mapowany typ.</span><span class="sxs-lookup"><span data-stu-id="f7347-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="f7347-114">Można również określić schemat, do którego należy tabela.</span><span class="sxs-lookup"><span data-stu-id="f7347-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
