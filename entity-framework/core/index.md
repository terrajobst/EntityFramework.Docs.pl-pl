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
# <a name="entity-framework-core-quick-overview"></a>Entity Framework Core szybki przegląd

Program Entity Framework (EF) Core to lekkie, rozszerzalny, i technologii dostępu do wersji i platform popularnych danych programu Entity Framework.

Podstawowe EF jest mapowania obiektów relacyjnych (O/RM), który umożliwia deweloperom platformy .NET do pracy z bazą danych przy użyciu obiektów platformy .NET. Eliminuje potrzebę większość deweloperów zazwyczaj trzeba napisać kod dostępu do danych. Jądro EF obsługuje wiele baz danych, zobacz [dostawcy bazy danych](providers/index.md) szczegółowe informacje.

Jeśli chcesz dowiedzieć się przez pisania kodu, zalecamy jednego z naszych [wprowadzenie](get-started/index.md) przewodniki ułatwiające rozpoczęcie pracy z EF Core.

## <a name="latest-version-ef-core-20"></a>Najnowsza wersja: EF Core 2.0

Jeśli znasz podstawowe EF i chcesz przejść bezpośrednio do szczegółów nowej wersji:

- **[Nowe funkcje w programie EF Core 2.0](what-is-new/index.md)**
- **[Uaktualnianie istniejącej aplikacji EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**

## <a name="get-entity-framework-core"></a>Pobierz program Entity Framework Core

[Zainstaluj pakiet NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) dostawcy bazy danych, którego chcesz użyć. Np. Aby zainstalować dostawcy programu SQL Server w aplikacji dla wielu platform przy użyciu `dotnet` narzędzia w wierszu polecenia:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Zobacz [dostawcy bazy danych](providers/index.md) uzyskać informacji o dostępnych dostawców i [instalowanie Core EF](get-started/install/index.md) bardziej szczegółowe kroki instalacji.

## <a name="the-model"></a>Model

Podstawowych EF dostępu do danych jest wykonywane przy użyciu modelu. Model składa się z klas jednostek i pochodne kontekst reprezentujący sesji z bazy danych, umożliwiając zapytania i zapisać dane. Zobacz [tworzenia modelu](modeling/index.md) Aby dowiedzieć się więcej.

Można wygenerować model z istniejącej bazy danych, ręcznie kod modelu do dopasowania bazy danych lub użyj EF migracji do tworzenia bazy danych z modelu (i rozwijać go zgodnie z modelem zmienia się wraz z upływem czasu).

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

## <a name="querying"></a>Wykonywanie zapytania

Wystąpienia klas jednostki są pobierane z bazy danych przy użyciu języka zapytań zintegrowanym (LINQ). Zobacz [danych zapytań](querying/index.md) Aby dowiedzieć się więcej.

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

Dane są tworzone, usunięte i zmodyfikowane w bazie danych za pomocą wystąpień klas jednostek. Zobacz [zapisywania danych](saving/index.md) Aby dowiedzieć się więcej.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
