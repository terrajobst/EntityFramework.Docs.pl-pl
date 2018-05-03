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
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="1f4c2-102">Wprowadzenie do podstawowych EF na .NET Framework za pomocą nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="1f4c2-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="1f4c2-103">W tym przewodniku zostanie utworzona aplikacja konsolowa, która wykonuje dostęp do podstawowych danych w bazie danych programu Microsoft SQL Server przy użyciu programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="1f4c2-104">Migracje użyje do utworzenia bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="1f4c2-105">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f4c2-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1f4c2-106">Prerequisites</span></span>

<span data-ttu-id="1f4c2-107">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="1f4c2-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="1f4c2-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1f4c2-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="1f4c2-109">Najnowszą wersję Menedżera pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="1f4c2-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="1f4c2-110">Najnowszą wersję programu Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f4c2-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="1f4c2-111">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="1f4c2-111">Create a new project</span></span>

* <span data-ttu-id="1f4c2-112">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f4c2-112">Open Visual Studio</span></span>

* <span data-ttu-id="1f4c2-113">Plik > Nowy > Projekt...</span><span class="sxs-lookup"><span data-stu-id="1f4c2-113">File > New > Project...</span></span>

* <span data-ttu-id="1f4c2-114">Z menu po lewej stronie wybierz Szablony > Visual C# > klasycznego pulpitu systemu Windows</span><span class="sxs-lookup"><span data-stu-id="1f4c2-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="1f4c2-115">Wybierz **aplikacji konsoli (.NET Framework)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="1f4c2-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="1f4c2-116">Upewnij się, ma być przeznaczona dla **.NET Framework 4.5.1** lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1f4c2-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="1f4c2-117">Nadaj nazwę projektu, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="1f4c2-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="1f4c2-118">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1f4c2-118">Install Entity Framework</span></span>

<span data-ttu-id="1f4c2-119">Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="1f4c2-120">W tym przewodniku zastosowano programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="1f4c2-121">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1f4c2-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="1f4c2-122">Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="1f4c2-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="1f4c2-123">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="1f4c2-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="1f4c2-124">W dalszej części tego przewodnika również użyjemy narzędzi Framework niektóre jednostki do obsługi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="1f4c2-125">Dlatego zostanie zainstalowany pakiet narzędzi również.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="1f4c2-126">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="1f4c2-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="1f4c2-127">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="1f4c2-127">Create your model</span></span>

<span data-ttu-id="1f4c2-128">Teraz nadszedł czas do definiowania klas kontekstu i jednostek, wchodzące w skład modelu.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="1f4c2-129">Projekt > Dodaj klasę...</span><span class="sxs-lookup"><span data-stu-id="1f4c2-129">Project > Add Class...</span></span>

* <span data-ttu-id="1f4c2-130">Wprowadź *Model.cs* jako nazwy i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="1f4c2-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="1f4c2-131">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="1f4c2-131">Replace the contents of the file with the following code</span></span>

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
> <span data-ttu-id="1f4c2-132">W rzeczywistej aplikacji można będzie umieścić każda klasa w osobnym pliku, a ciąg połączenia `App.Config` pliku i jego odczytu przy użyciu `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="1f4c2-133">Dla uproszczenia wprowadzamy wszystko w pliku jednego kodu dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="1f4c2-134">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="1f4c2-134">Create your database</span></span>

<span data-ttu-id="1f4c2-135">Teraz, gdy masz modelu można użyć migracji tworzenia bazy danych dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="1f4c2-136">Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="1f4c2-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="1f4c2-137">Uruchom `Add-Migration MyFirstMigration` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="1f4c2-138">Uruchom `Update-Database` na zastosowanie nowych migracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="1f4c2-139">Ponieważ baza danych nie istnieje, zostanie on utworzony dla Ciebie przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="1f4c2-140">Jeśli wprowadzisz zmiany w przyszłości do modelu, możesz użyć `Add-Migration` polecenie, aby utworzyć szkielet nowych migracji dokonanie odpowiedniej schematu zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="1f4c2-141">Po zaznaczeniu tej opcji szkieletu kodu (i wszelkie wymagane zmiany wprowadzone), można użyć `Update-Database` polecenie, aby zastosować zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="1f4c2-142">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="1f4c2-143">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="1f4c2-143">Use your model</span></span>

<span data-ttu-id="1f4c2-144">Można teraz używać modelu do uzyskania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="1f4c2-145">Otwórz *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="1f4c2-145">Open *Program.cs*</span></span>

* <span data-ttu-id="1f4c2-146">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="1f4c2-146">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="1f4c2-147">Debuguj > Uruchom bez debugowania</span><span class="sxs-lookup"><span data-stu-id="1f4c2-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="1f4c2-148">Zobaczysz jednego blogu jest zapisywana w bazie danych, a następnie szczegóły wszystkich blogi są podane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="1f4c2-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![obraz](_static/output-new-db.png)
