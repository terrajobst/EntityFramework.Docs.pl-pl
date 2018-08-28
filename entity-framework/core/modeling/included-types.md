---
title: Uwzględnianie i wykluczanie typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996153"
---
# <a name="including--excluding-types"></a><span data-ttu-id="cdf1f-102">Uwzględnianie i wykluczanie typów</span><span class="sxs-lookup"><span data-stu-id="cdf1f-102">Including & Excluding Types</span></span>

<span data-ttu-id="cdf1f-103">W tym typu w modelu oznacza, że EF ma metadane o typ, który podejmie próbę odczytu i zapisu wystąpienia z i do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="cdf1f-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="cdf1f-104">Conventions</span></span>

<span data-ttu-id="cdf1f-105">Zgodnie z Konwencją, typy, które są widoczne w `DbSet` właściwości kontekstu znajdują się w modelu.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="cdf1f-106">Ponadto, typy, które są wymienione w `OnModelCreating` metody dostępne są również.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="cdf1f-107">Ponadto wszystkie typy, które znajdują się przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów znajdują się również w modelu.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="cdf1f-108">**Na przykład w poniższym fragmencie kodu wszystkich trzech typów zostaną wykryte:**</span><span class="sxs-lookup"><span data-stu-id="cdf1f-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="cdf1f-109">`Blog` ponieważ jest ona uwidoczniona w `DbSet` właściwości w kontekście</span><span class="sxs-lookup"><span data-stu-id="cdf1f-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="cdf1f-110">`Post` ponieważ jest odnalezione `Blog.Posts` właściwość nawigacji</span><span class="sxs-lookup"><span data-stu-id="cdf1f-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="cdf1f-111">`AuditEntry` ponieważ są one wymienione w `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="cdf1f-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="cdf1f-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="cdf1f-112">Data Annotations</span></span>

<span data-ttu-id="cdf1f-113">Korzystanie z adnotacji danych, aby wyłączyć typ z modelu.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="cdf1f-114">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="cdf1f-114">Fluent API</span></span>

<span data-ttu-id="cdf1f-115">Aby wyłączyć typ z modelu, można użyć Fluent API.</span><span class="sxs-lookup"><span data-stu-id="cdf1f-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
