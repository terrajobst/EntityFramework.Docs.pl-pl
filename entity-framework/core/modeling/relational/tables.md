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
# <a name="table-mapping"></a>Mapowania tabeli

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Mapowania tabeli identyfikuje danych tabeli należy z kwerendy i zapisane w bazie danych.

## <a name="conventions"></a>Konwencje

Według Konwencji każdej jednostki będzie Instalatora, aby mapować do tabeli o takiej samej nazwie jak `DbSet<TEntity>` właściwość, która przedstawia jednostek w kontekście pochodnych. Jeśli nie `DbSet<TEntity>` wchodzi dla danej jednostki, jest używana nazwa klasy.

## <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do konfigurowania tabeli, która mapuje typu.

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

Można również określić schemat, który należy do tabeli.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować tabelę, która mapuje typu, można użyć interfejsu API Fluent.

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

Można również określić schemat, który należy do tabeli.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
