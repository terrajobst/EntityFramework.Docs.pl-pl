---
title: Dziedziczenie (relacyjna baza danych) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655630"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="f8862-102">Dziedziczenie (relacyjna baza danych)</span><span class="sxs-lookup"><span data-stu-id="f8862-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="f8862-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="f8862-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f8862-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="f8862-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f8862-105">Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8862-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="f8862-106">Obecnie tylko wzorzec tabeli dla hierarchii (TPH) jest implementowany w EF Core.</span><span class="sxs-lookup"><span data-stu-id="f8862-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="f8862-107">Inne typowe wzorce, takie jak Table-per-Type (TPT) i typu tabeli (TPC), nie są jeszcze dostępne.</span><span class="sxs-lookup"><span data-stu-id="f8862-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="f8862-108">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f8862-108">Conventions</span></span>

<span data-ttu-id="f8862-109">Zgodnie z Konwencją dziedziczenie zostanie zmapowane przy użyciu wzorca tabela-na-hierarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="f8862-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="f8862-110">TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f8862-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="f8862-111">Kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.</span><span class="sxs-lookup"><span data-stu-id="f8862-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="f8862-112">EF Core zostanie skonfigurowana tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy zostaną jawnie dołączone do modelu (zobacz [dziedziczenie](../inheritance.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="f8862-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="f8862-113">Poniżej znajduje się przykład przedstawiający prosty scenariusz dziedziczenia oraz dane przechowywane w tabeli relacyjnej bazy danych przy użyciu wzorca TPH.</span><span class="sxs-lookup"><span data-stu-id="f8862-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="f8862-114">Kolumna *rozróżniacza* określa, który typ *blogu* jest przechowywany w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="f8862-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="f8862-116">W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null.</span><span class="sxs-lookup"><span data-stu-id="f8862-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f8862-117">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f8862-117">Data Annotations</span></span>

<span data-ttu-id="f8862-118">Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="f8862-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f8862-119">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="f8862-119">Fluent API</span></span>

<span data-ttu-id="f8862-120">Korzystając z interfejsu API Fluent, można skonfigurować nazwę i typ kolumny dyskryminatora oraz wartości, które są używane do identyfikowania poszczególnych typów w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f8862-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="f8862-121">Konfigurowanie właściwości rozróżniacza</span><span class="sxs-lookup"><span data-stu-id="f8862-121">Configuring the discriminator property</span></span>

<span data-ttu-id="f8862-122">W powyższych przykładach rozróżniacz jest tworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f8862-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="f8862-123">Ponieważ jest to właściwość w modelu, można ją skonfigurować tak samo jak inne właściwości.</span><span class="sxs-lookup"><span data-stu-id="f8862-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="f8862-124">Na przykład, aby ustawić maksymalną długość w przypadku użycia domyślnego rozróżniacza Konwencji przez Konwencję:</span><span class="sxs-lookup"><span data-stu-id="f8862-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="f8862-125">Rozróżniacz może być również mapowany do rzeczywistej właściwości CLR w jednostce.</span><span class="sxs-lookup"><span data-stu-id="f8862-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="f8862-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f8862-126">For example:</span></span>

```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="f8862-127">Łącząc te dwa elementy jednocześnie, możliwe jest zamapowanie rozróżniacza na rzeczywistą Właściwość i skonfigurowanie go:</span><span class="sxs-lookup"><span data-stu-id="f8862-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
