---
title: Wartości domyślne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054191"
---
# <a name="default-values"></a><span data-ttu-id="78808-102">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="78808-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="78808-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="78808-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="78808-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="78808-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="78808-105">Wartość domyślna kolumny to wartość, która zostanie wstawiony, jeśli dodaje się nowego wiersza, ale nie określono wartości dla kolumny.</span><span class="sxs-lookup"><span data-stu-id="78808-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="78808-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="78808-106">Conventions</span></span>

<span data-ttu-id="78808-107">Konwencja wartości domyślnej nie jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="78808-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="78808-108">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="78808-108">Data Annotations</span></span>

<span data-ttu-id="78808-109">Nie można ustawić wartość domyślną, za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="78808-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="78808-110">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="78808-110">Fluent API</span></span>

<span data-ttu-id="78808-111">Określenie wartości domyślnej dla właściwości, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="78808-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="78808-112">Można również określić fragmentu SQL, które jest używane do obliczania wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="78808-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
