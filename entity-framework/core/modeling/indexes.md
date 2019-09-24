---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197245"
---
# <a name="indexes"></a><span data-ttu-id="bf838-102">Zwiększa</span><span class="sxs-lookup"><span data-stu-id="bf838-102">Indexes</span></span>

<span data-ttu-id="bf838-103">Indeksy są typowymi koncepcjami w wielu magazynach danych.</span><span class="sxs-lookup"><span data-stu-id="bf838-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="bf838-104">Chociaż ich implementacja w magazynie danych może się różnić, są one używane do wykonywania wyszukiwania na podstawie kolumny (lub zestawu kolumn) bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="bf838-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="bf838-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="bf838-105">Conventions</span></span>

<span data-ttu-id="bf838-106">Zgodnie z Konwencją indeks jest tworzony we wszystkich właściwościach (lub zestawach właściwości), które są używane jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="bf838-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bf838-107">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="bf838-107">Data Annotations</span></span>

<span data-ttu-id="bf838-108">Nie można tworzyć indeksów przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="bf838-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="bf838-109">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="bf838-109">Fluent API</span></span>

<span data-ttu-id="bf838-110">Możesz użyć interfejsu API Fluent, aby określić indeks dla pojedynczej właściwości.</span><span class="sxs-lookup"><span data-stu-id="bf838-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="bf838-111">Domyślnie indeksy nie są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="bf838-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="bf838-112">Można również określić, że indeks powinien być unikatowy, co oznacza, że żadne dwie jednostki nie mogą mieć tych samych wartości dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="bf838-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="bf838-113">Można również określić indeks w więcej niż jednej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="bf838-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="bf838-114">Istnieje tylko jeden indeks dla każdego odrębnego zestawu właściwości.</span><span class="sxs-lookup"><span data-stu-id="bf838-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="bf838-115">Jeśli używasz interfejsu API Fluent do skonfigurowania indeksu dla zestawu właściwości, który ma już zdefiniowany indeks, w ramach Konwencji lub poprzedniej konfiguracji, zmienisz definicję tego indeksu.</span><span class="sxs-lookup"><span data-stu-id="bf838-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="bf838-116">Jest to przydatne, jeśli chcesz skonfigurować indeks, który został utworzony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="bf838-116">This is useful if you want to further configure an index that was created by convention.</span></span>
