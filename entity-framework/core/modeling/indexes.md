---
title: Indeksy — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054077"
---
# <a name="indexes"></a><span data-ttu-id="ae5b5-102">Indeksy</span><span class="sxs-lookup"><span data-stu-id="ae5b5-102">Indexes</span></span>

<span data-ttu-id="ae5b5-103">Indeksy są wspólnej definicji w wielu sklepach danych.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="ae5b5-104">Podczas wdrażania ich w magazynie danych mogą się różnić, są one używane do lepiej wyszukiwań na podstawie kolumny (lub zestaw kolumn) skuteczne.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="ae5b5-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ae5b5-105">Conventions</span></span>

<span data-ttu-id="ae5b5-106">Konwencja indeks jest tworzony w każdej właściwości (lub zbiór właściwości), używanych jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ae5b5-107">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="ae5b5-107">Data Annotations</span></span>

<span data-ttu-id="ae5b5-108">Nie można utworzyć indeksy za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ae5b5-109">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="ae5b5-109">Fluent API</span></span>

<span data-ttu-id="ae5b5-110">Interfejsu API Fluent służy do określenia indeksu dla jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="ae5b5-111">Domyślnie indeksy są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="ae5b5-112">Można również określić czy indeksu powinna być unikatowa, co oznacza, że nie dwie jednostki może mieć tej samej wartości dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="ae5b5-113">Można również określić indeksu przez więcej niż jedną kolumnę.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="ae5b5-114">Istnieje tylko jeden indeks na osobny zestaw właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="ae5b5-115">Jeśli Konfigurowanie indeksu na zbiór właściwości, które jest już zdefiniowana, indeksu przez Konwencję lub poprzednią konfigurację za pomocą interfejsu API Fluent następnie zmienia się definicji tego indeksu.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="ae5b5-116">Jest to przydatne, jeśli chcesz kontynuować konfigurowanie indeksu, który został utworzony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ae5b5-116">This is useful if you want to further configure an index that was created by convention.</span></span>
