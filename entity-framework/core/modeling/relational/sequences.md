---
title: "Sekwencje — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="sequences"></a><span data-ttu-id="4aed3-102">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="4aed3-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="4aed3-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="4aed3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4aed3-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="4aed3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4aed3-105">Sekwencja generuje sekwencyjnych wartości liczbowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4aed3-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="4aed3-106">Sekwencji nie są skojarzone z określonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4aed3-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="4aed3-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="4aed3-107">Conventions</span></span>

<span data-ttu-id="4aed3-108">Według Konwencji sekwencji nie są wprowadzane w modelu.</span><span class="sxs-lookup"><span data-stu-id="4aed3-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4aed3-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="4aed3-109">Data Annotations</span></span>

<span data-ttu-id="4aed3-110">Nie można skonfigurować sekwencji za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="4aed3-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4aed3-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="4aed3-111">Fluent API</span></span>

<span data-ttu-id="4aed3-112">Interfejsu API Fluent służy do tworzenia sekwencji w modelu.</span><span class="sxs-lookup"><span data-stu-id="4aed3-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="4aed3-113">Można również skonfigurować dodatkowe aspekt sekwencji, takie jak schemat, wartość początkową i przyrostu.</span><span class="sxs-lookup"><span data-stu-id="4aed3-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="4aed3-114">Gdy wprowadzono sekwencji można użyć jej do generowania wartości właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="4aed3-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="4aed3-115">Na przykład można użyć [wartości domyślne](default-values.md) do wstawienia następnej wartości z sekwencji.</span><span class="sxs-lookup"><span data-stu-id="4aed3-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
