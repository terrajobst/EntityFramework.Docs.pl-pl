---
title: W tym & wykluczanie typów - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054128"
---
# <a name="including--excluding-types"></a><span data-ttu-id="c00f3-102">W tym & wykluczanie typów</span><span class="sxs-lookup"><span data-stu-id="c00f3-102">Including & Excluding Types</span></span>

<span data-ttu-id="c00f3-103">W tym typu w modelu oznacza, że EF ma metadane dotyczące typu, który będzie podejmować próby odczytu i zapisu wystąpień z/do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c00f3-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c00f3-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c00f3-104">Conventions</span></span>

<span data-ttu-id="c00f3-105">Konwencja typy, które są widoczne w `DbSet` właściwości kontekstu znajdują się w modelu.</span><span class="sxs-lookup"><span data-stu-id="c00f3-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="c00f3-106">Ponadto typy, które są wymienione w `OnModelCreating` uwzględniane są również metody.</span><span class="sxs-lookup"><span data-stu-id="c00f3-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="c00f3-107">Na koniec żadnych typów, które zostały znalezione przez cyklicznie badać właściwości nawigacji wykrytych typów znajdują się również w modelu.</span><span class="sxs-lookup"><span data-stu-id="c00f3-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="c00f3-108">**Na przykład w poniższym fragmencie kodu odnalezieniu wszystkich trzech typów:**</span><span class="sxs-lookup"><span data-stu-id="c00f3-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="c00f3-109">`Blog`ponieważ jest widoczna w `DbSet` właściwości w tym kontekście</span><span class="sxs-lookup"><span data-stu-id="c00f3-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="c00f3-110">`Post`ponieważ okaże się za pośrednictwem `Blog.Posts` właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="c00f3-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="c00f3-111">`AuditEntry`ponieważ jest wymieniony w`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="c00f3-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="c00f3-112">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="c00f3-112">Data Annotations</span></span>

<span data-ttu-id="c00f3-113">Aby wyłączyć typ z modelu, można użyć adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="c00f3-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c00f3-114">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="c00f3-114">Fluent API</span></span>

<span data-ttu-id="c00f3-115">Aby wyłączyć typ z modelu, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="c00f3-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
