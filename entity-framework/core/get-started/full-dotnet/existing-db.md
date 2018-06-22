---
title: Wprowadzenie do platformy .NET Framework — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812628"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Wprowadzenie do podstawowych EF w programie .NET Framework z istniejącej bazy danych

W tym przewodniku zostanie utworzona aplikacja konsolowa, która wykonuje dostęp do podstawowych danych w bazie danych programu Microsoft SQL Server przy użyciu programu Entity Framework. Odtwarzanie użyje do utworzenia modelu programu Entity Framework, na podstawie istniejącej bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Najnowszą wersję Menedżera pakietów NuGet](https://dist.nuget.org/index.html)

* [Najnowszą wersję programu Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Blog bazy danych](#blogging-database)

### <a name="blogging-database"></a>Blog bazy danych

W tym samouczku używana **obsługi blogów** bazy danych w wystąpieniu bazy danych LocalDb jako istniejącej bazy danych.

> [!TIP]  
> Jeśli utworzono już **obsługi blogów** bazy danych jako część innego samouczek, można pominąć te kroki.

* Otwórz program Visual Studio

* Narzędzia > łączenia z bazą danych...

* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**

* Wprowadź **(localdb) \mssqllocaldb** jako **nazwa serwera**

* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**

* Baza danych master jest teraz wyświetlany w obszarze **połączenia danych** w **Eksploratora serwera**

* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowej kwerendy**

* Skopiuj skrypt wymienione poniżej do edytora zapytań

* Kliknij prawym przyciskiem myszy w edytorze zapytań i wybierz **Execute**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio

* Plik > Nowy > Projekt...

* Z menu po lewej stronie wybierz Szablony > Visual C# > systemu Windows

* Wybierz **aplikacji konsoli** szablonu projektu

* Upewnij się, ma być przeznaczona dla **.NET Framework 4.5.1** lub nowszy

* Nadaj nazwę projektu, a następnie kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano programu SQL Server. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Aby włączyć odtwarzania z istniejącej bazy danych należy zbyt zainstalować kilka innych pakietów.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Odtworzyć modelu

Teraz nadszedł czas, aby utworzyć model EF na podstawie istniejącej bazy danych.

* Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów

* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Proces odtwarzania utworzony klas jednostek i pochodne kontekst na podstawie schematu istniejącej bazy danych. Klas jednostek to proste C# obiekty reprezentujące dane można będzie kwerend i zapisywanie.

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

Kontekst reprezentuje sesji z bazą danych i służy do wykonywania zapytań i zapisać wystąpień klas jednostek.

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

Można teraz używać modelu do uzyskania dostępu do danych.

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

Zobaczysz jednego blogu jest zapisywana w bazie danych, a następnie szczegóły wszystkich blogi są podane w konsoli.

![obraz](_static/output-existing-db.png)
