---
title: Wprowadzenie do platformy .NET Framework — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388471"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Wprowadzenie do programu EF Core w programie .NET Framework przy użyciu istniejącej bazy danych

W tym instruktażu utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework. Odtwarzanie użyje do tworzenia modelu Entity Framework, w oparciu o istniejącą bazę danych.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:

* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) — co najmniej w wersji 15.3

* [Najnowszą wersję Menedżera pakietów NuGet](https://dist.nuget.org/index.html)

* [Najnowszą wersję programu Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Do obsługi blogów bazy danych](#blogging-database)

### <a name="blogging-database"></a>Do obsługi blogów bazy danych

W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych.

> [!TIP]  
> Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innym samouczku, można pominąć te kroki.

* Otwórz program Visual Studio

* Narzędzia > nawiązać połączenie z bazą danych...

* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**

* Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**

* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**

* Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**

* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**

* Skopiuj skrypt wymienione poniżej pod warunkiem do edytora zapytań

* Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio

* Plik > Nowy > Projekt...

* Z menu po lewej stronie wybierz Szablony > Visual C# > Windows

* Wybierz **aplikację Konsolową** szablonu projektu

* Upewnij się, przeznaczonych do pracy **platformy .NET Framework 4.6.1** lub nowszej

* Nazwij projekt, a następnie kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. W tym przewodniku używa programu SQL Server. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Aby włączyć odtwarzania z istniejącej bazy danych, musimy zainstalować kilka innych pakietów, zbyt.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Model odtworzyć.

Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.

* Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów

* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Procesu odtwarzania utworzone klasy jednostek i pochodnej kontekst na podstawie schematu istniejącej bazy danych. Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Użyj modelu

Można teraz używać modelu przeprowadzić dostępu do danych.

* Otwórz *Program.cs*

* Zastąp zawartość pliku następującym kodem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Debuguj > Uruchom bez debugowania

Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.

![obraz](_static/output-existing-db.png)
