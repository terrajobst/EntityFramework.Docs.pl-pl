---
title: Ograniczenia klucza obcego - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054185"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="8234a-102">Ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="8234a-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="8234a-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="8234a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8234a-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="8234a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8234a-105">Wprowadzono ograniczenie klucza obcego dla każdej relacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="8234a-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="8234a-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="8234a-106">Conventions</span></span>

<span data-ttu-id="8234a-107">Według Konwencji o nazwie ograniczeń klucza obcego `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="8234a-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="8234a-108">Złożonych kluczy obcych `<foreign key property name>` staje się podkreślenia oddzielone listę nazw właściwości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="8234a-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8234a-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="8234a-109">Data Annotations</span></span>

<span data-ttu-id="8234a-110">Ograniczenie klucza obcego nazwy nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="8234a-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8234a-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="8234a-111">Fluent API</span></span>

<span data-ttu-id="8234a-112">Aby skonfigurować nazwę ograniczenia klucza obcego relacji, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8234a-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
