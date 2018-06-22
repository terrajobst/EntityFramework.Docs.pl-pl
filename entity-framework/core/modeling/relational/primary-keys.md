---
title: Klucze podstawowe - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054179"
---
# <a name="primary-keys"></a>Klucze podstawowe

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Wprowadzono ograniczenia klucza podstawowego dla klucza poszczególnych typów jednostek.

## <a name="conventions"></a>Konwencje

Według Konwencji, będzie miała nazwę klucza podstawowego w bazie danych `PK_<type name>`.

## <a name="data-annotations"></a>Adnotacji danych

Za pomocą adnotacji danych można skonfigurować określone aspekty relacyjnej bazy danych klucza podstawowego.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować nazwę ograniczenia klucza podstawowego w bazie danych, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
