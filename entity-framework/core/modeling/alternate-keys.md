---
title: Klucze alternatywne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054068"
---
# <a name="alternate-keys"></a><span data-ttu-id="da1cb-102">Klucze alternatywne</span><span class="sxs-lookup"><span data-stu-id="da1cb-102">Alternate Keys</span></span>

<span data-ttu-id="da1cb-103">Klucz alternatywny służy jako alternatywny identyfikator unikatowy dla każdego wystąpienia jednostki, oprócz klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="da1cb-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="da1cb-104">Klucze alternatywne może służyć jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="da1cb-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="da1cb-105">Korzystanie z relacyjnej bazy danych to mapuje do koncepcji unikatowego indeksu/ograniczenia na alternatywny kolumn kluczy i jeden lub więcej ograniczeń klucza obcego odwołujące się do kolumn na liście.</span><span class="sxs-lookup"><span data-stu-id="da1cb-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="da1cb-106">Jeśli chcesz wymusić unikatowości kolumny, a następnie chcesz zamiast klucza alternatywnego unikatowego indeksu, zobacz [indeksów](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="da1cb-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="da1cb-107">EF klucze alternatywne zapewniają większą funkcjonalność niż unikatowe indeksy ponieważ mogą zostać użyte jako element docelowy klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="da1cb-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="da1cb-108">Klucze alternatywne zwykle są wprowadzane automatycznie w razie potrzeby i nie trzeba ręcznie skonfigurować je.</span><span class="sxs-lookup"><span data-stu-id="da1cb-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="da1cb-109">Zobacz [konwencje](#conventions) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="da1cb-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="da1cb-110">Konwencje</span><span class="sxs-lookup"><span data-stu-id="da1cb-110">Conventions</span></span>

<span data-ttu-id="da1cb-111">Według Konwencji klucza alternatywnego wprowadzono automatycznie podczas określania właściwości, która nie jest kluczem podstawowym jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="da1cb-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="da1cb-112">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="da1cb-112">Data Annotations</span></span>

<span data-ttu-id="da1cb-113">Nie można skonfigurować alternatywne klucze za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="da1cb-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="da1cb-114">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="da1cb-114">Fluent API</span></span>

<span data-ttu-id="da1cb-115">Aby skonfigurować jedną właściwość klucza alternatywnego za można Użyj interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="da1cb-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="da1cb-116">Aby skonfigurować wiele właściwości klucza alternatywnego (znany jako alternatywny klucz złożony) umożliwia także interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="da1cb-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
