---
title: Uwzględnienie & z wyjątkiem typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197379"
---
# <a name="including--excluding-types"></a><span data-ttu-id="d73eb-102">Uwzględnianie i wykluczanie typów</span><span class="sxs-lookup"><span data-stu-id="d73eb-102">Including & Excluding Types</span></span>

<span data-ttu-id="d73eb-103">Uwzględnienie typu w modelu oznacza, że Dr ma metadane dotyczące tego typu i spróbuje odczytywać i zapisywać wystąpienia z/do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d73eb-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d73eb-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="d73eb-104">Conventions</span></span>

<span data-ttu-id="d73eb-105">Według Konwencji typy, które są dostępne we `DbSet` właściwościach kontekstu, są uwzględniane w modelu.</span><span class="sxs-lookup"><span data-stu-id="d73eb-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="d73eb-106">Ponadto są również uwzględniane typy, które są wymienione `OnModelCreating` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="d73eb-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="d73eb-107">Na koniec wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów, są również uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="d73eb-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="d73eb-108">**Na przykład w poniższym kodzie wykryto wszystkie trzy typy:**</span><span class="sxs-lookup"><span data-stu-id="d73eb-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="d73eb-109">`Blog`ponieważ jest on uwidoczniony we `DbSet` właściwości kontekstu</span><span class="sxs-lookup"><span data-stu-id="d73eb-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="d73eb-110">`Post`ponieważ jest ono odnajdywane za `Blog.Posts` pośrednictwem właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="d73eb-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="d73eb-111">`AuditEntry`ponieważ jest on wymieniony w`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="d73eb-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="d73eb-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="d73eb-112">Data Annotations</span></span>

<span data-ttu-id="d73eb-113">Możesz użyć adnotacji danych, aby wykluczyć typ z modelu.</span><span class="sxs-lookup"><span data-stu-id="d73eb-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="d73eb-114">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="d73eb-114">Fluent API</span></span>

<span data-ttu-id="d73eb-115">Możesz użyć interfejsu API Fluent do wykluczenia typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="d73eb-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
