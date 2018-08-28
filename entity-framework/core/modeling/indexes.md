---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995483"
---
# <a name="indexes"></a><span data-ttu-id="74929-102">Indeksy</span><span class="sxs-lookup"><span data-stu-id="74929-102">Indexes</span></span>

<span data-ttu-id="74929-103">Indeksy są typowe pojęcia w wielu magazynach danych.</span><span class="sxs-lookup"><span data-stu-id="74929-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="74929-104">Podczas ich wdrażania w magazynie danych mogą się różnić, są one używane do lepiej wyszukiwań na podstawie kolumny (lub zestaw kolumn) wydajne.</span><span class="sxs-lookup"><span data-stu-id="74929-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="74929-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="74929-105">Conventions</span></span>

<span data-ttu-id="74929-106">Zgodnie z Konwencją indeksu jest tworzony w każdej właściwości (lub zestaw właściwości), używany jako klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="74929-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="74929-107">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="74929-107">Data Annotations</span></span>

<span data-ttu-id="74929-108">Nie można utworzyć indeksy przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="74929-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="74929-109">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="74929-109">Fluent API</span></span>

<span data-ttu-id="74929-110">Interfejs Fluent API służy do określenia indeksu w jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="74929-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="74929-111">Indeksy są domyślnie nie jest unikatowa.</span><span class="sxs-lookup"><span data-stu-id="74929-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="74929-112">Można również określić czy indeksu powinna być unikatowa, co oznacza, że nie dwie jednostki może mieć tej samej wartości dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="74929-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="74929-113">Można również określić indeks przez więcej niż jedną kolumnę.</span><span class="sxs-lookup"><span data-stu-id="74929-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="74929-114">Istnieje tylko jeden indeks na odrębnym zestawem właściwości.</span><span class="sxs-lookup"><span data-stu-id="74929-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="74929-115">W przypadku konfigurowania indeksu na zbiór właściwości, które ma już zdefiniowane, indeks przy Konwencji lub poprzedniej konfiguracji, za pomocą interfejsu API Fluent następnie spowoduje zmianę definicji indeksu.</span><span class="sxs-lookup"><span data-stu-id="74929-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="74929-116">Jest to przydatne, jeśli chcesz dodatkowo skonfigurować indeks, który został utworzony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="74929-116">This is useful if you want to further configure an index that was created by convention.</span></span>
