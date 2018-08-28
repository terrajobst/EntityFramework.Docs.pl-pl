---
title: Wartości domyślne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996255"
---
# <a name="default-values"></a><span data-ttu-id="3b6eb-102">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="3b6eb-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="3b6eb-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3b6eb-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="3b6eb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3b6eb-105">Wartość domyślna w kolumnie jest wartość, która zostanie wstawiony, jeśli jest wstawiany nowego wiersza, ale nie określono wartości dla kolumny.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="3b6eb-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="3b6eb-106">Conventions</span></span>

<span data-ttu-id="3b6eb-107">Zgodnie z Konwencją wartość domyślna jest nieskonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3b6eb-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3b6eb-108">Data Annotations</span></span>

<span data-ttu-id="3b6eb-109">Nie można ustawić wartość domyślną, przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3b6eb-110">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="3b6eb-110">Fluent API</span></span>

<span data-ttu-id="3b6eb-111">Interfejs Fluent API umożliwia określenie wartości domyślnej dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="3b6eb-112">Można również określić fragment SQL, który jest używany do obliczania wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="3b6eb-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
