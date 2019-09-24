---
title: Ograniczenia klucza obcego — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197069"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="2677f-102">Ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="2677f-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="2677f-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="2677f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2677f-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="2677f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2677f-105">Ograniczenie klucza obcego jest wprowadzane dla każdej relacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="2677f-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="2677f-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="2677f-106">Conventions</span></span>

<span data-ttu-id="2677f-107">Zgodnie z Konwencją, ograniczenia klucza obcego `FK_<dependent type name>_<principal type name>_<foreign key property name>`są nazywane.</span><span class="sxs-lookup"><span data-stu-id="2677f-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="2677f-108">W przypadku złożonych kluczy `<foreign key property name>` obcych jest rozdzielana podkreśleniem lista nazw właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="2677f-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2677f-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="2677f-109">Data Annotations</span></span>

<span data-ttu-id="2677f-110">Nazw ograniczeń klucza obcego nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="2677f-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2677f-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="2677f-111">Fluent API</span></span>

<span data-ttu-id="2677f-112">Za pomocą interfejsu API Fluent można skonfigurować nazwę ograniczenia klucza obcego dla relacji.</span><span class="sxs-lookup"><span data-stu-id="2677f-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
