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
# <a name="table-mapping"></a>Mapowanie tabeli

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Mapowanie tabeli określa, które dane tabeli mają być wysyłane z i zapisywane w bazie danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją każda jednostka zostanie skonfigurowana do mapowania na tabelę o tej samej nazwie co Właściwość `DbSet<TEntity>`, która uwidacznia jednostkę w kontekście pochodnym. Jeśli żadna `DbSet<TEntity>` nie jest uwzględniona dla danej jednostki, zostanie użyta nazwa klasy.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby skonfigurować tabelę, do której jest mapowany typ.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Można również określić schemat, do którego należy tabela.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować tabelę, do której jest mapowany typ.

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

Można również określić schemat, do którego należy tabela.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
