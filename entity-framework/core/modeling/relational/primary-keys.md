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
---
# <a name="primary-keys"></a><span data-ttu-id="ae128-102">Klucze podstawowe</span><span class="sxs-lookup"><span data-stu-id="ae128-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="ae128-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="ae128-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ae128-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="ae128-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ae128-105">Wprowadzono ograniczenia klucza podstawowego dla klucza poszczególnych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="ae128-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="ae128-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ae128-106">Conventions</span></span>

<span data-ttu-id="ae128-107">Według Konwencji, będzie miała nazwę klucza podstawowego w bazie danych `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="ae128-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ae128-108">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="ae128-108">Data Annotations</span></span>

<span data-ttu-id="ae128-109">Za pomocą adnotacji danych można skonfigurować określone aspekty relacyjnej bazy danych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ae128-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ae128-110">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="ae128-110">Fluent API</span></span>

<span data-ttu-id="ae128-111">Aby skonfigurować nazwę ograniczenia klucza podstawowego w bazie danych, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ae128-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
