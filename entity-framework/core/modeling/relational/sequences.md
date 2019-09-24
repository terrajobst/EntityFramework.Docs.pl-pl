---
title: Sekwencje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: ce02b9840e58102a60c1d8eacf6810365104d7d7
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196910"
---
# <a name="sequences"></a><span data-ttu-id="ef72a-102">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="ef72a-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="ef72a-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="ef72a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ef72a-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="ef72a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ef72a-105">Sekwencja generuje sekwencyjne wartości liczbowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef72a-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="ef72a-106">Sekwencje nie są skojarzone z określoną tabelą.</span><span class="sxs-lookup"><span data-stu-id="ef72a-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="ef72a-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ef72a-107">Conventions</span></span>

<span data-ttu-id="ef72a-108">Zgodnie z Konwencją sekwencje nie są wprowadzane w modelu.</span><span class="sxs-lookup"><span data-stu-id="ef72a-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ef72a-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="ef72a-109">Data Annotations</span></span>

<span data-ttu-id="ef72a-110">Nie można skonfigurować sekwencji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="ef72a-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ef72a-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="ef72a-111">Fluent API</span></span>

<span data-ttu-id="ef72a-112">Za pomocą interfejsu API Fluent można utworzyć sekwencję w modelu.</span><span class="sxs-lookup"><span data-stu-id="ef72a-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/Sequence.cs?highlight=7)] -->
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

<span data-ttu-id="ef72a-113">Możesz również skonfigurować dodatkowy aspekt sekwencji, taki jak jego schemat, wartość początkowa i przyrost.</span><span class="sxs-lookup"><span data-stu-id="ef72a-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
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

<span data-ttu-id="ef72a-114">Po wprowadzeniu sekwencji można użyć jej do generowania wartości właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="ef72a-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="ef72a-115">Na przykład można użyć [wartości domyślnych](default-values.md) , aby wstawić następną wartość z sekwencji.</span><span class="sxs-lookup"><span data-stu-id="ef72a-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
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
