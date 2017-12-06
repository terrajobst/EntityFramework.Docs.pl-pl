---
title: "Indeksy — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Indeksy

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Mapuje indeksu relacyjnej bazy danych do tego samego pojęcia jako indeks w podstawowe programu Entity Framework.

## <a name="conventions"></a>Konwencje

Konwencja, indeksy są nazywane `IX_<type name>_<property name>`. Złożone indeksów `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.

## <a name="data-annotations"></a>Adnotacji danych

Indeksy nie można skonfigurować za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
