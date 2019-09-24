---
title: Klucze alternatywne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197472"
---
# <a name="alternate-keys"></a><span data-ttu-id="caa62-102">Klucze alternatywne</span><span class="sxs-lookup"><span data-stu-id="caa62-102">Alternate Keys</span></span>

<span data-ttu-id="caa62-103">Alternatywny klucz służy jako alternatywny unikatowy identyfikator dla każdego wystąpienia jednostki oprócz klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="caa62-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="caa62-104">Klucze alternatywne mogą służyć jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="caa62-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="caa62-105">W przypadku korzystania z relacyjnej bazy danych mapowanie do koncepcji unikatowego indeksu/ograniczenia w kolumnach klucza alternatywnego oraz co najmniej jedno ograniczenie klucza obcego odwołujące się do kolumn.</span><span class="sxs-lookup"><span data-stu-id="caa62-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="caa62-106">Jeśli chcesz, aby wymusić unikatowość kolumny, a następnie chcesz użyć indeksu unikatowego, a nie klucza alternatywnego, zobacz [indeksy](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="caa62-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="caa62-107">W EF klucze alternatywne zapewniają większą funkcjonalność niż indeksy unikatowe, ponieważ mogą być używane jako obiekty docelowe klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="caa62-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="caa62-108">Klucze alternatywne są zwykle wprowadzane w razie potrzeby i nie trzeba ich ręcznie konfigurować.</span><span class="sxs-lookup"><span data-stu-id="caa62-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="caa62-109">Aby uzyskać więcej informacji, zobacz [konwencje](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="caa62-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="caa62-110">Konwencje</span><span class="sxs-lookup"><span data-stu-id="caa62-110">Conventions</span></span>

<span data-ttu-id="caa62-111">Przy użyciu konwencji klucz alternatywny jest wprowadzany w przypadku identyfikowania właściwości, która nie jest kluczem podstawowym jako elementu docelowego relacji.</span><span class="sxs-lookup"><span data-stu-id="caa62-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="caa62-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="caa62-112">Data Annotations</span></span>

<span data-ttu-id="caa62-113">Nie można skonfigurować kluczy alternatywnych przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="caa62-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="caa62-114">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="caa62-114">Fluent API</span></span>

<span data-ttu-id="caa62-115">Korzystając z interfejsu API Fluent, można skonfigurować pojedynczą właściwość jako klucz alternatywny.</span><span class="sxs-lookup"><span data-stu-id="caa62-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

<span data-ttu-id="caa62-116">Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz alternatywny (nazywany kluczem alternatywnym).</span><span class="sxs-lookup"><span data-stu-id="caa62-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
