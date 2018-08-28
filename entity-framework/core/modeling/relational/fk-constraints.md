---
title: Ograniczenia klucza obcego — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993991"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="02b04-102">Ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="02b04-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="02b04-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="02b04-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="02b04-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="02b04-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="02b04-105">Wprowadzono ograniczenie klucza obcego dla każdej relacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="02b04-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="02b04-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="02b04-106">Conventions</span></span>

<span data-ttu-id="02b04-107">Zgodnie z Konwencją ograniczenia klucza obcego są nazywane `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="02b04-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="02b04-108">Złożone kluczy obcych `<foreign key property name>` staje się oddzielone znakiem podkreślenia listę nazw właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="02b04-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="02b04-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="02b04-109">Data Annotations</span></span>

<span data-ttu-id="02b04-110">Nazwy ograniczenia klucza obcego nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="02b04-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="02b04-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="02b04-111">Fluent API</span></span>

<span data-ttu-id="02b04-112">Interfejs Fluent API umożliwiają skonfigurowanie nazwę ograniczenia klucza obcego relacji.</span><span class="sxs-lookup"><span data-stu-id="02b04-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

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
