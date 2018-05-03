---
title: Wprowadzenie do platformy .NET Framework — nowej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Wprowadzenie do podstawowych EF na .NET Framework za pomocą nowej bazy danych

W tym przewodniku zostanie utworzona aplikacja konsolowa, która wykonuje dostęp do podstawowych danych w bazie danych programu Microsoft SQL Server przy użyciu programu Entity Framework. Migracje użyje do utworzenia bazy danych z modelu.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Najnowszą wersję Menedżera pakietów NuGet](https://dist.nuget.org/index.html)

* [Najnowszą wersję programu Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio

* Plik > Nowy > Projekt...

* Z menu po lewej stronie wybierz Szablony > Visual C# > klasycznego pulpitu systemu Windows

* Wybierz **aplikacji konsoli (.NET Framework)** szablonu projektu

* Upewnij się, ma być przeznaczona dla **.NET Framework 4.5.1** lub nowszy

* Nadaj nazwę projektu, a następnie kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano programu SQL Server. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

W dalszej części tego przewodnika również użyjemy narzędzi Framework niektóre jednostki do obsługi bazy danych. Dlatego zostanie zainstalowany pakiet narzędzi również.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Tworzenie modelu

Teraz nadszedł czas do definiowania klas kontekstu i jednostek, wchodzące w skład modelu.

* Projekt > Dodaj klasę...

* Wprowadź *Model.cs* jako nazwy i kliknij przycisk **OK**

* Zastąp zawartość pliku następującym kodem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

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

> [!TIP]  
> W rzeczywistej aplikacji można będzie umieścić każda klasa w osobnym pliku, a ciąg połączenia `App.Config` pliku i jego odczytu przy użyciu `ConfigurationManager`. Dla uproszczenia wprowadzamy wszystko w pliku jednego kodu dla tego samouczka.

## <a name="create-your-database"></a>Tworzenie bazy danych

Teraz, gdy masz modelu można użyć migracji tworzenia bazy danych dla Ciebie.

* Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów

* Uruchom `Add-Migration MyFirstMigration` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.

* Uruchom `Update-Database` na zastosowanie nowych migracji w bazie danych. Ponieważ baza danych nie istnieje, zostanie on utworzony dla Ciebie przed zastosowaniem migracji.

> [!TIP]  
> Jeśli wprowadzisz zmiany w przyszłości do modelu, możesz użyć `Add-Migration` polecenie, aby utworzyć szkielet nowych migracji dokonanie odpowiedniej schematu zmiany w bazie danych. Po zaznaczeniu tej opcji szkieletu kodu (i wszelkie wymagane zmiany wprowadzone), można użyć `Update-Database` polecenie, aby zastosować zmiany do bazy danych.
>
>Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.

## <a name="use-your-model"></a>Użyj modelu

Można teraz używać modelu do uzyskania dostępu do danych.

* Otwórz *Program.cs*

* Zastąp zawartość pliku następującym kodem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
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

![obraz](_static/output-new-db.png)
