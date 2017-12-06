---
title: "Szybki przegląd — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="d9a9a-102">Entity Framework Core szybki przegląd</span><span class="sxs-lookup"><span data-stu-id="d9a9a-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="d9a9a-103">Program Entity Framework (EF) Core to lekkie, rozszerzalny, i technologii dostępu do wersji i platform popularnych danych programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="d9a9a-104">Podstawowe EF jest mapowania obiektów relacyjnych (O/RM), który umożliwia deweloperom platformy .NET do pracy z bazą danych przy użyciu obiektów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="d9a9a-105">Eliminuje potrzebę większość deweloperów zazwyczaj trzeba napisać kod dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="d9a9a-106">Jądro EF obsługuje wiele baz danych, zobacz [dostawcy bazy danych](providers/index.md) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="d9a9a-107">Jeśli chcesz dowiedzieć się przez pisania kodu, zalecamy jednego z naszych [wprowadzenie](get-started/index.md) przewodniki ułatwiające rozpoczęcie pracy z EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="d9a9a-108">Najnowsza wersja: EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="d9a9a-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="d9a9a-109">Jeśli znasz podstawowe EF i chcesz przejść bezpośrednio do szczegółów nowej wersji:</span><span class="sxs-lookup"><span data-stu-id="d9a9a-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="d9a9a-110">**[Nowe funkcje w programie EF Core 2.0](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="d9a9a-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="d9a9a-111">**[Uaktualnianie istniejącej aplikacji EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="d9a9a-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="d9a9a-112">Pobierz program Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d9a9a-112">Get Entity Framework Core</span></span>

<span data-ttu-id="d9a9a-113">[Zainstaluj pakiet NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="d9a9a-114">Np.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-114">E.g.</span></span> <span data-ttu-id="d9a9a-115">Aby zainstalować dostawcy programu SQL Server w aplikacji dla wielu platform przy użyciu `dotnet` narzędzia w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="d9a9a-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="d9a9a-116">Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="d9a9a-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="d9a9a-117">Zobacz [dostawcy bazy danych](providers/index.md) uzyskać informacji o dostępnych dostawców i [instalowanie Core EF](get-started/install/index.md) bardziej szczegółowe kroki instalacji.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="d9a9a-118">Model</span><span class="sxs-lookup"><span data-stu-id="d9a9a-118">The Model</span></span>

<span data-ttu-id="d9a9a-119">Podstawowych EF dostępu do danych jest wykonywane przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="d9a9a-120">Model składa się z klas jednostek i pochodne kontekst reprezentujący sesji z bazy danych, umożliwiając zapytania i zapisać dane.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="d9a9a-121">Zobacz [tworzenia modelu](modeling/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="d9a9a-122">Można wygenerować model z istniejącej bazy danych, ręcznie kod modelu do dopasowania bazy danych lub użyj EF migracji do tworzenia bazy danych z modelu (i rozwijać go zgodnie z modelem zmienia się wraz z upływem czasu).</span><span class="sxs-lookup"><span data-stu-id="d9a9a-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a><span data-ttu-id="d9a9a-123">Wykonywanie zapytania</span><span class="sxs-lookup"><span data-stu-id="d9a9a-123">Querying</span></span>

<span data-ttu-id="d9a9a-124">Wystąpienia klas jednostki są pobierane z bazy danych przy użyciu języka zapytań zintegrowanym (LINQ).</span><span class="sxs-lookup"><span data-stu-id="d9a9a-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="d9a9a-125">Zobacz [danych zapytań](querying/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="d9a9a-126">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="d9a9a-126">Saving Data</span></span>

<span data-ttu-id="d9a9a-127">Dane są tworzone, usunięte i zmodyfikowane w bazie danych za pomocą wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="d9a9a-128">Zobacz [zapisywania danych](saving/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="d9a9a-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
