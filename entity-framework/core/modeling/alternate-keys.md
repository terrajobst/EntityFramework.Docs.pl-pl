---
title: Klucze alternatywne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996974"
---
# <a name="alternate-keys"></a><span data-ttu-id="5c3ff-102">Klucze alternatywne</span><span class="sxs-lookup"><span data-stu-id="5c3ff-102">Alternate Keys</span></span>

<span data-ttu-id="5c3ff-103">Klucza alternatywnego służy jako alternatywne Unikatowy identyfikator dla każdego wystąpienia jednostki, oprócz klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="5c3ff-104">Klucze alternatywne może służyć jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="5c3ff-105">Przy użyciu relacyjnej bazy danych to mapuje do koncepcji unikatowego indeksu/ograniczenia na alternatywny kolumn kluczy i jeden lub więcej ograniczeń klucza obcego odwołujące się do kolumn na liście.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="5c3ff-106">Jeśli chcesz wymusić unikatowość kolumny, a następnie chcesz, aby zamiast klucza alternatywnego unikatowego indeksu, zobacz [indeksów](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="5c3ff-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="5c3ff-107">W programie EF klucze alternatywne zapewnić większą funkcjonalność niż indeksy unikatowe, ponieważ może służyć jako cel klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="5c3ff-108">Klucze alternatywne są zwykle wprowadzane dla Ciebie w razie i nie trzeba ręcznie skonfigurować je.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="5c3ff-109">Zobacz [konwencje](#conventions) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="5c3ff-110">Konwencje</span><span class="sxs-lookup"><span data-stu-id="5c3ff-110">Conventions</span></span>

<span data-ttu-id="5c3ff-111">Zgodnie z Konwencją klucza alternatywnego został wprowadzony dla Ciebie podczas określania właściwości, który nie jest kluczem podstawowym jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="5c3ff-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="5c3ff-112">Data Annotations</span></span>

<span data-ttu-id="5c3ff-113">Klucze alternatywne nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5c3ff-114">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="5c3ff-114">Fluent API</span></span>

<span data-ttu-id="5c3ff-115">Interfejs Fluent API umożliwiają skonfigurowanie jednej właściwości klucza alternatywnego.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="5c3ff-116">Można również skonfigurować wiele właściwości jako klucza alternatywnego (znanych jako alternatywne klucz złożony) za pomocą Fluent interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5c3ff-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
