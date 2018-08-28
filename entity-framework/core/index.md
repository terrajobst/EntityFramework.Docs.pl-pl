---
title: Przegląd — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: d9fcafb35248b1af54e1ac707e2ff7d4e80e4aa2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995654"
---
# <a name="entity-framework-core"></a><span data-ttu-id="ffe52-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ffe52-102">Entity Framework Core</span></span>

<span data-ttu-id="ffe52-103">Entity Framework (EF) Core to lekkie, rozszerzalne, i technologii dostępu do popularnych danych Entity Framework w wersji dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ffe52-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="ffe52-104">EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET do pracy z bazą danych, używając obiektów platformy .NET i eliminując potrzebę większość kodu dostępu do danych zwykle potrzebują do zapisania.</span><span class="sxs-lookup"><span data-stu-id="ffe52-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="ffe52-105">EF Core obsługuje wiele baz danych, zobacz [dostawcy baz danych](providers/index.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="ffe52-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="ffe52-106">Jeśli chcesz dowiedzieć się, przez napisanie kodu, zalecamy jeden z naszych [wprowadzenie](get-started/index.md) przewodniki ułatwiające rozpoczęcie pracy z programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="ffe52-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="ffe52-107">Co nowego w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="ffe52-107">What is new in EF Core</span></span>

<span data-ttu-id="ffe52-108">Jeśli jesteś zaznajomiony z programem EF Core i chcesz przejść bezpośrednio do szczegółowe informacje o najnowszych wersjach:</span><span class="sxs-lookup"><span data-stu-id="ffe52-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="ffe52-109">**[What's new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="ffe52-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="ffe52-110">**[Uaktualnianie istniejącej aplikacji do programu EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="ffe52-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="ffe52-111">Pobierz platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ffe52-111">Get Entity Framework Core</span></span>

<span data-ttu-id="ffe52-112">[Zainstaluj pakiet NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="ffe52-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="ffe52-113">Na przykład, aby zainstalować dostawcę programu SQL Server przy użyciu wieloplatformowego opracowywania aplikacji `dotnet` narzędzia w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="ffe52-113">For example, to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="ffe52-114">Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="ffe52-114">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="ffe52-115">Zobacz [dostawcy baz danych](providers/index.md) uzyskać informacji na temat dostępnych dostawców i [Instalowanie programu EF Core](get-started/install/index.md) bardziej szczegółowe kroki instalacji.</span><span class="sxs-lookup"><span data-stu-id="ffe52-115">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="ffe52-116">Model</span><span class="sxs-lookup"><span data-stu-id="ffe52-116">The Model</span></span>

<span data-ttu-id="ffe52-117">Z programem EF Core dostęp do danych odbywa się przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="ffe52-117">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="ffe52-118">Model składa się z klas jednostek i pochodnej kontekstu, który reprezentuje sesję z bazą danych, dzięki czemu zapytania i zapisywać dane.</span><span class="sxs-lookup"><span data-stu-id="ffe52-118">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="ffe52-119">Zobacz [tworzenia modelu](modeling/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="ffe52-119">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="ffe52-120">Możesz wygenerować model z istniejącej bazy danych, przekazania kodu z modelu, aby dopasować bazy danych lub użyj migracji EF, aby utworzyć bazę danych z modelu (i rozwój go jak model zmienia się wraz z upływem czasu).</span><span class="sxs-lookup"><span data-stu-id="ffe52-120">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="ffe52-121">Wykonywanie zapytań</span><span class="sxs-lookup"><span data-stu-id="ffe52-121">Querying</span></span>

<span data-ttu-id="ffe52-122">Wystąpienia klas jednostek są pobierane z bazy danych, używając Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="ffe52-122">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="ffe52-123">Zobacz [zapytań danych](querying/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="ffe52-123">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="ffe52-124">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="ffe52-124">Saving Data</span></span>

<span data-ttu-id="ffe52-125">Dane są tworzone, usunięte i zmodyfikowane w bazie danych za pomocą wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="ffe52-125">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="ffe52-126">Zobacz [zapisywanie danych](saving/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="ffe52-126">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
