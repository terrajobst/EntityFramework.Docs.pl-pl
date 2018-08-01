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
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="2d812-102">Wprowadzenie do programu EF Core w programie .NET Framework przy użyciu istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d812-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="2d812-103">W tym instruktażu utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2d812-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="2d812-104">Odtwarzanie użyje do tworzenia modelu Entity Framework, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="2d812-105">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2d812-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d812-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2d812-106">Prerequisites</span></span>

<span data-ttu-id="2d812-107">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="2d812-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="2d812-108">[Program Visual Studio 2017](https://www.visualstudio.com/downloads/) — co najmniej w wersji 15.3</span><span class="sxs-lookup"><span data-stu-id="2d812-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) - at least version 15.3</span></span>

* [<span data-ttu-id="2d812-109">Najnowszą wersję Menedżera pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="2d812-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="2d812-110">Najnowszą wersję programu Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="2d812-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="2d812-111">Do obsługi blogów bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d812-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="2d812-112">Do obsługi blogów bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d812-112">Blogging database</span></span>

<span data-ttu-id="2d812-113">W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="2d812-114">Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innym samouczku, można pominąć te kroki.</span><span class="sxs-lookup"><span data-stu-id="2d812-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="2d812-115">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d812-115">Open Visual Studio</span></span>

* <span data-ttu-id="2d812-116">Narzędzia > nawiązać połączenie z bazą danych...</span><span class="sxs-lookup"><span data-stu-id="2d812-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="2d812-117">Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**</span><span class="sxs-lookup"><span data-stu-id="2d812-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="2d812-118">Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**</span><span class="sxs-lookup"><span data-stu-id="2d812-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="2d812-119">Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="2d812-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="2d812-120">Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="2d812-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="2d812-121">Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="2d812-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="2d812-122">Skopiuj skrypt wymienione poniżej pod warunkiem do edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="2d812-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="2d812-123">Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="2d812-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="2d812-124">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="2d812-124">Create a new project</span></span>

* <span data-ttu-id="2d812-125">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d812-125">Open Visual Studio</span></span>

* <span data-ttu-id="2d812-126">Plik > Nowy > Projekt...</span><span class="sxs-lookup"><span data-stu-id="2d812-126">File > New > Project...</span></span>

* <span data-ttu-id="2d812-127">Z menu po lewej stronie wybierz Szablony > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="2d812-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="2d812-128">Wybierz **aplikację Konsolową** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="2d812-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="2d812-129">Upewnij się, przeznaczonych do pracy **platformy .NET Framework 4.6.1** lub nowszej</span><span class="sxs-lookup"><span data-stu-id="2d812-129">Ensure you are targeting **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="2d812-130">Nazwij projekt, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="2d812-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="2d812-131">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2d812-131">Install Entity Framework</span></span>

<span data-ttu-id="2d812-132">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="2d812-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="2d812-133">W tym przewodniku używa programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2d812-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="2d812-134">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2d812-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="2d812-135">Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="2d812-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="2d812-136">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="2d812-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="2d812-137">Aby włączyć odtwarzania z istniejącej bazy danych, musimy zainstalować kilka innych pakietów, zbyt.</span><span class="sxs-lookup"><span data-stu-id="2d812-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="2d812-138">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="2d812-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="2d812-139">Model odtworzyć.</span><span class="sxs-lookup"><span data-stu-id="2d812-139">Reverse engineer your model</span></span>

<span data-ttu-id="2d812-140">Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="2d812-141">Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="2d812-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="2d812-142">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d812-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="2d812-143">Procesu odtwarzania utworzone klasy jednostek i pochodnej kontekst na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="2d812-144">Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

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

<span data-ttu-id="2d812-145">Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d812-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="use-your-model"></a><span data-ttu-id="2d812-146">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="2d812-146">Use your model</span></span>

<span data-ttu-id="2d812-147">Można teraz używać modelu przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="2d812-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="2d812-148">Otwórz *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="2d812-148">Open *Program.cs*</span></span>

* <span data-ttu-id="2d812-149">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="2d812-149">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="2d812-150">Debuguj > Uruchom bez debugowania</span><span class="sxs-lookup"><span data-stu-id="2d812-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="2d812-151">Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="2d812-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![obraz](_static/output-existing-db.png)
