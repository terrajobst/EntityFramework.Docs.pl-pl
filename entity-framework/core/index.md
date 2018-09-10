---
title: Przegląd — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: ee3fac9e9103749195886a632fbeac3163a46689
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250546"
---
# <a name="entity-framework-core"></a><span data-ttu-id="231f8-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="231f8-102">Entity Framework Core</span></span>

<span data-ttu-id="231f8-103">Entity Framework (EF) Core to lekkie, rozszerzalne, i technologii dostępu do popularnych danych Entity Framework w wersji dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="231f8-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="231f8-104">EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET do pracy z bazą danych, używając obiektów platformy .NET i eliminując potrzebę większość kodu dostępu do danych zwykle potrzebują do zapisania.</span><span class="sxs-lookup"><span data-stu-id="231f8-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="231f8-105">EF Core obsługuje wiele baz danych, zobacz [dostawcy baz danych](providers/index.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="231f8-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="231f8-106">Model</span><span class="sxs-lookup"><span data-stu-id="231f8-106">The Model</span></span>

<span data-ttu-id="231f8-107">Z programem EF Core dostęp do danych odbywa się przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="231f8-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="231f8-108">Model składa się z klas jednostek i pochodnej kontekstu, który reprezentuje sesję z bazą danych, dzięki czemu zapytania i zapisywać dane.</span><span class="sxs-lookup"><span data-stu-id="231f8-108">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="231f8-109">Zobacz [tworzenia modelu](modeling/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="231f8-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="231f8-110">Możesz wygenerować model z istniejącej bazy danych, przekazania kodu z modelu, aby dopasować bazy danych lub użyj migracji EF, aby utworzyć bazę danych z modelu (i rozwój go jak model zmienia się wraz z upływem czasu).</span><span class="sxs-lookup"><span data-stu-id="231f8-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="231f8-111">Wykonywanie zapytań</span><span class="sxs-lookup"><span data-stu-id="231f8-111">Querying</span></span>

<span data-ttu-id="231f8-112">Wystąpienia klas jednostek są pobierane z bazy danych, używając Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="231f8-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="231f8-113">Zobacz [zapytań danych](querying/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="231f8-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="231f8-114">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="231f8-114">Saving Data</span></span>

<span data-ttu-id="231f8-115">Dane są tworzone, usunięte i zmodyfikowane w bazie danych za pomocą wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="231f8-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="231f8-116">Zobacz [zapisywanie danych](saving/index.md) Aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="231f8-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="231f8-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="231f8-117">Next steps</span></span>

<span data-ttu-id="231f8-118">Aby wprowadzających samouczków, zobacz [rozpoczęcie korzystania z platformy Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="231f8-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

