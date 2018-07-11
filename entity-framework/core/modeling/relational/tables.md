---
title: Mapowanie tabeli — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949012"
---
# <a name="table-mapping"></a>Mapowanie tabeli

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Mapowanie tabeli identyfikuje dane, które tabeli powinien być odpytywane i zapisywane w bazie danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją każdej jednostki będą konfigurowane tak, aby zamapować na tabelę z taką samą nazwę jak `DbSet<TEntity>` właściwość, która udostępnia jednostki w kontekście pochodnych. Jeśli nie `DbSet<TEntity>` jest dołączony do danej jednostki, nazwa klasy jest używana.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować tabelę, która mapuje typu.

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

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie tabeli, która mapuje typu.

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
