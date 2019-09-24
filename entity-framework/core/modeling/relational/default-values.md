---
title: Wartości domyślne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196986"
---
# <a name="default-values"></a><span data-ttu-id="2c1f4-102">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="2c1f4-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="2c1f4-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2c1f4-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="2c1f4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2c1f4-105">Wartość domyślna kolumny to wartość, która zostanie wstawiona, jeśli zostanie wstawiony nowy wiersz, ale nie określono wartości dla tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="2c1f4-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="2c1f4-106">Conventions</span></span>

<span data-ttu-id="2c1f4-107">Zgodnie z Konwencją wartość domyślna nie jest skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2c1f4-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="2c1f4-108">Data Annotations</span></span>

<span data-ttu-id="2c1f4-109">Nie można ustawić wartości domyślnej przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2c1f4-110">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="2c1f4-110">Fluent API</span></span>

<span data-ttu-id="2c1f4-111">Możesz użyć interfejsu API Fluent, aby określić wartość domyślną dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="2c1f4-112">Można również określić fragment SQL, który jest używany do obliczania wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="2c1f4-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
