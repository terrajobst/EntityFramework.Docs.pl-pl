---
title: Krótkie omówienie - programu EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 103e5e069687950a8411f2d92c7b5a191844e0ae
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948993"
---
# <a name="entity-framework-core-quick-overview"></a>Omówienie szybkiej programu Entity Framework Core

Entity Framework (EF) Core to lekkie, rozszerzalne, i technologii dostępu do popularnych danych Entity Framework w wersji dla wielu platform.

EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET do pracy z bazą danych, używając obiektów platformy .NET i eliminując potrzebę większość kodu dostępu do danych zwykle potrzebują do zapisania.

EF Core obsługuje wiele baz danych, zobacz [dostawcy baz danych](providers/index.md) Aby uzyskać szczegółowe informacje.

Jeśli chcesz dowiedzieć się, przez napisanie kodu, zalecamy jeden z naszych [wprowadzenie](get-started/index.md) przewodniki ułatwiające rozpoczęcie pracy z programem EF Core.

## <a name="what-is-new-in-ef-core"></a>Co nowego w programie EF Core

Jeśli jesteś zaznajomiony z programem EF Core i chcesz przejść bezpośrednio do szczegółowe informacje o najnowszych wersjach:

- **[What's new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Uaktualnianie istniejącej aplikacji do programu EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Pobierz platformy Entity Framework Core

[Zainstaluj pakiet NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) dostawcy bazy danych, którego chcesz użyć. Na przykład, aby zainstalować dostawcę programu SQL Server przy użyciu wieloplatformowego opracowywania aplikacji `dotnet` narzędzia w wierszu polecenia:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Zobacz [dostawcy baz danych](providers/index.md) uzyskać informacji na temat dostępnych dostawców i [Instalowanie programu EF Core](get-started/install/index.md) bardziej szczegółowe kroki instalacji.

## <a name="the-model"></a>Model

Z programem EF Core dostęp do danych odbywa się przy użyciu modelu. Model składa się z klas jednostek i pochodnej kontekstu, który reprezentuje sesję z bazą danych, dzięki czemu zapytania i zapisywać dane. Zobacz [tworzenia modelu](modeling/index.md) Aby dowiedzieć się więcej.

Możesz wygenerować model z istniejącej bazy danych, przekazania kodu z modelu, aby dopasować bazy danych lub użyj migracji EF, aby utworzyć bazę danych z modelu (i rozwój go jak model zmienia się wraz z upływem czasu).

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

## <a name="querying"></a>Wykonywanie zapytań

Wystąpienia klas jednostek są pobierane z bazy danych, używając Language Integrated Query (LINQ). Zobacz [zapytań danych](querying/index.md) Aby dowiedzieć się więcej.

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

Dane są tworzone, usunięte i zmodyfikowane w bazie danych za pomocą wystąpień klas jednostek. Zobacz [zapisywanie danych](saving/index.md) Aby dowiedzieć się więcej.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
