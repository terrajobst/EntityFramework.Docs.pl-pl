---
title: Mapowanie tabeli — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656104"
---
# <a name="table-mapping"></a><span data-ttu-id="86be9-102">Mapowanie tabeli</span><span class="sxs-lookup"><span data-stu-id="86be9-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="86be9-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="86be9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="86be9-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="86be9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="86be9-105">Mapowanie tabeli określa, które dane tabeli mają być wysyłane z i zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="86be9-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="86be9-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="86be9-106">Conventions</span></span>

<span data-ttu-id="86be9-107">Zgodnie z Konwencją każda jednostka zostanie skonfigurowana do mapowania na tabelę o tej samej nazwie co Właściwość `DbSet<TEntity>`, która uwidacznia jednostkę w kontekście pochodnym.</span><span class="sxs-lookup"><span data-stu-id="86be9-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="86be9-108">Jeśli żadna `DbSet<TEntity>` nie jest uwzględniona dla danej jednostki, zostanie użyta nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="86be9-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="86be9-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="86be9-109">Data Annotations</span></span>

<span data-ttu-id="86be9-110">Możesz użyć adnotacji danych, aby skonfigurować tabelę, do której jest mapowany typ.</span><span class="sxs-lookup"><span data-stu-id="86be9-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="86be9-111">Można również określić schemat, do którego należy tabela.</span><span class="sxs-lookup"><span data-stu-id="86be9-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="86be9-112">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="86be9-112">Fluent API</span></span>

<span data-ttu-id="86be9-113">Korzystając z interfejsu API Fluent, można skonfigurować tabelę, do której jest mapowany typ.</span><span class="sxs-lookup"><span data-stu-id="86be9-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;

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

<span data-ttu-id="86be9-114">Można również określić schemat, do którego należy tabela.</span><span class="sxs-lookup"><span data-stu-id="86be9-114">You can also specify a schema that the table belongs to.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
