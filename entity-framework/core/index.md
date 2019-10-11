---
title: Przegląd Entity Framework Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e736251753134b716e64f24f6c517ed9f66a7db4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181327"
---
# <a name="entity-framework-core"></a><span data-ttu-id="3a4c7-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3a4c7-102">Entity Framework Core</span></span>

<span data-ttu-id="3a4c7-103">Entity Framework (EF) Core to [lekkie, rozszerzalne i](https://github.com/aspnet/EntityFrameworkCore) wieloplatformowe wersje popularnej Entity Framework technologii dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="3a4c7-104">EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET mogą pracować z bazą danych, używając obiektów platformy .NET i eliminując potrzebę pisania większości kodu dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="3a4c7-105">EF Core obsługuje wiele aparatów baz danych, zobacz [Dostawcy baz danych](providers/index.md), aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="3a4c7-106">Model</span><span class="sxs-lookup"><span data-stu-id="3a4c7-106">The Model</span></span>

<span data-ttu-id="3a4c7-107">W przypadku EF Core dostęp do danych odbywa się przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="3a4c7-108">Model składa się z klas jednostek i obiektu kontekstu, który reprezentuje sesję z bazą danych, co pozwala na wykonywanie zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="3a4c7-109">Zobacz [Tworzenie modelu](modeling/index.md), aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="3a4c7-110">Można wygenerować model z istniejącej bazy danych, ręcznie nakodować model do bazy danych lub użyć [migracji EF](managing-schemas/migrations/index.md) do utworzenia bazy danych z modelu, a następnie przydzielenia go wraz ze zmianą modelu w miarę upływu czasu.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="querying"></a><span data-ttu-id="3a4c7-111">Wykonywanie zapytania</span><span class="sxs-lookup"><span data-stu-id="3a4c7-111">Querying</span></span>

<span data-ttu-id="3a4c7-112">Wystąpienia klas jednostek są pobierane z bazy danych przy użyciu języka Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="3a4c7-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="3a4c7-113">Zobacz [Wykonywanie zapytania o dane](querying/index.md), aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="3a4c7-114">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="3a4c7-114">Saving Data</span></span>

<span data-ttu-id="3a4c7-115">Dane są tworzone, usuwane i modyfikowane w bazie danych za pomocą wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="3a4c7-116">Zobacz [zapisywanie danych](saving/index.md), aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="3a4c7-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="3a4c7-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3a4c7-117">Next steps</span></span>

<span data-ttu-id="3a4c7-118">Aby skorzystać z samouczków wprowadzających, zobacz [Wprowadzenie do platformy Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="3a4c7-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

