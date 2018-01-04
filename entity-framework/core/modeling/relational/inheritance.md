---
title: "Dziedziczenie (relacyjnej bazy danych) — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="554c3-102">Dziedziczenie (relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="554c3-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="554c3-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="554c3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="554c3-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="554c3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="554c3-105">Dziedziczenie w modelu EF służy do kontrolowania sposobu dziedziczenia klas jednostek jest reprezentowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="554c3-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="554c3-106">Obecnie tylko ze wzorcem (TPH) tabeli na hierarchii jest zaimplementowana w EF Core.</span><span class="sxs-lookup"><span data-stu-id="554c3-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="554c3-107">Inne typowe wzorce, takich jak tabeli na typ (TPT) i tabeli na konkretnych type (TPC) nie są jeszcze dostępne.</span><span class="sxs-lookup"><span data-stu-id="554c3-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="554c3-108">Konwencje</span><span class="sxs-lookup"><span data-stu-id="554c3-108">Conventions</span></span>

<span data-ttu-id="554c3-109">Według Konwencji dziedziczenia zostanie zamapowane przy użyciu wzorca tabeli na hierarchii (TPH).</span><span class="sxs-lookup"><span data-stu-id="554c3-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="554c3-110">TPH używa pojedynczej tabeli, aby przechowywać dane dla wszystkich typów w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="554c3-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="554c3-111">Kolumna dyskryminatora służy do identyfikowania typu reprezentuje każdego wiersza.</span><span class="sxs-lookup"><span data-stu-id="554c3-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="554c3-112">EF Core tylko ustawienia dziedziczenia, jeśli dwa lub więcej dziedziczonych typów jawnie znajdują się w modelu (zobacz [dziedziczenia](../inheritance.md) więcej szczegółów).</span><span class="sxs-lookup"><span data-stu-id="554c3-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="554c3-113">Poniżej przedstawiono przykładowy scenariusz dziedziczenia proste i dane przechowywane w tabeli relacyjnej bazy danych za pomocą wzorca TPH.</span><span class="sxs-lookup"><span data-stu-id="554c3-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="554c3-114">*Rozróżniacza* kolumny identyfikuje typu *blogu* są przechowywane w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="554c3-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![obraz](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="554c3-116">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="554c3-116">Data Annotations</span></span>

<span data-ttu-id="554c3-117">Za pomocą adnotacji danych nie można skonfigurować dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="554c3-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="554c3-118">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="554c3-118">Fluent API</span></span>

<span data-ttu-id="554c3-119">Aby skonfigurować nazwę i typ kolumny rozróżniacza i wartości, które są używane do identyfikowania poszczególnych typów w hierarchii, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="554c3-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
