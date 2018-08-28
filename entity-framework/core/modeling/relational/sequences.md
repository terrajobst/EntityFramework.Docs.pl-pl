---
title: Sekwencje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994519"
---
# <a name="sequences"></a><span data-ttu-id="94cdb-102">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="94cdb-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="94cdb-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="94cdb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="94cdb-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="94cdb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="94cdb-105">Sekwencja generuje kolejnych wartości liczbowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="94cdb-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="94cdb-106">Sekwencje nie są skojarzone z określonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="94cdb-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="94cdb-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="94cdb-107">Conventions</span></span>

<span data-ttu-id="94cdb-108">Zgodnie z Konwencją sekwencji nie są wprowadzane w modelu.</span><span class="sxs-lookup"><span data-stu-id="94cdb-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="94cdb-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="94cdb-109">Data Annotations</span></span>

<span data-ttu-id="94cdb-110">Nie można skonfigurować przy użyciu adnotacji danych sekwencji.</span><span class="sxs-lookup"><span data-stu-id="94cdb-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="94cdb-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="94cdb-111">Fluent API</span></span>

<span data-ttu-id="94cdb-112">Interfejs Fluent API umożliwia tworzenie sekwencji w modelu.</span><span class="sxs-lookup"><span data-stu-id="94cdb-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="94cdb-113">Można również skonfigurować dodatkowe aspekty sekwencji, takie jak jego schematu, wartość początkową i przyrostu.</span><span class="sxs-lookup"><span data-stu-id="94cdb-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="94cdb-114">Gdy wprowadzono sekwencji służy do generowania wartości właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="94cdb-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="94cdb-115">Na przykład, można użyć [wartości domyślne](default-values.md) do wstawienia następnej wartości z sekwencji.</span><span class="sxs-lookup"><span data-stu-id="94cdb-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
