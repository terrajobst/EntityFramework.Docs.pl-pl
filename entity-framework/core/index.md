---
title: Przegląd — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
---

# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core to lekkie, rozszerzalne, ["open source"](https://github.com/aspnet/EntityFrameworkCore) dla wielu platform wersję popularnych danych Entity Framework dostęp do technologii.

EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET mogą pracować z bazą danych, używając obiektów platformy .NET i eliminując potrzebę pisania większości kodu dostępu do danych.

EF Core obsługuje wiele aparatów baz danych, zobacz [Dostawcy baz danych](providers/index.md), aby uzyskać szczegółowe informacje.

## <a name="the-model"></a>Model

Z programem EF Core dostęp do danych odbywa się przy użyciu modelu. Model składa się z klas jednostek i obiekt kontekstu, który reprezentuje sesję z bazą danych, dzięki czemu zapytania i zapisywać dane. Zobacz [Tworzenie modelu](modeling/index.md), aby dowiedzieć się więcej.

Model można wygenerować z istniejącej bazy danych, kod ręcznie, model, aby dopasować bazy danych, lub użyć migracje EF utworzyć bazę danych z modelu i rozwój go jako modelu zmienia się wraz z upływem czasu.

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

## <a name="querying"></a>Wykonywanie zapytania

Wystąpienia klas jednostek są pobierane z bazy danych przy użyciu języka Language Integrated Query (LINQ). Zobacz [Wykonywanie zapytania o dane](querying/index.md), aby dowiedzieć się więcej.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Zapisywanie danych

Dane są tworzone, usuwane i modyfikowane w bazie danych za pomocą wystąpień klas jednostek. Zobacz [zapisywanie danych](saving/index.md), aby dowiedzieć się więcej.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Następne kroki

Aby skorzystać z samouczków wprowadzających, zobacz [Wprowadzenie do platformy Entity Framework Core](get-started/index.md).

