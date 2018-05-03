---
title: Wprowadzenie .NET Core - nowej bazy danych — EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Rozpoczynanie pracy z platformą .NET Core przy użyciu programu Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="e5631-104">Wprowadzenie do podstawowych EF w aplikacji konsoli .NET Core nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e5631-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="e5631-105">W tym przewodniku spowoduje utworzenie aplikacji konsoli .NET Core, który wykonuje dostęp do podstawowych danych względem bazy danych SQLite przy użyciu programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e5631-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="e5631-106">Migracje użyje do utworzenia bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="e5631-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="e5631-107">Zobacz [platformy ASP.NET Core - nową bazę danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e5631-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="e5631-108">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e5631-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5631-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e5631-109">Prerequisites</span></span>

<span data-ttu-id="e5631-110">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="e5631-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="e5631-111">System operacyjny obsługuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5631-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="e5631-112">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (mimo że instrukcje może służyć do tworzenia aplikacji z poprzedniej wersji z bardzo kilka zmian).</span><span class="sxs-lookup"><span data-stu-id="e5631-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="e5631-113">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="e5631-113">Create a new project</span></span>

* <span data-ttu-id="e5631-114">Utwórz nową `ConsoleApp.SQLite` folderu projektu i użyj `dotnet` polecenie, aby umieścić w nim aplikacji .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5631-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="e5631-115">Zainstaluj program Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e5631-115">Install Entity Framework Core</span></span>

<span data-ttu-id="e5631-116">Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="e5631-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e5631-117">W tym przewodniku zastosowano SQLite.</span><span class="sxs-lookup"><span data-stu-id="e5631-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="e5631-118">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e5631-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e5631-119">Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="e5631-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="e5631-120">Ręcznie Edytuj `ConsoleApp.SQLite.csproj` dodać DotNetCliToolReference do Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="e5631-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="e5631-121">`ConsoleApp.SQLite.csproj` teraz powinny zawierać następujące:</span><span class="sxs-lookup"><span data-stu-id="e5631-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="e5631-122">Uwaga: Numery wersji użyta powyżej były prawidłowe w czasie publikowania.</span><span class="sxs-lookup"><span data-stu-id="e5631-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="e5631-123">Uruchom `dotnet restore` do zainstalowania nowych pakietów.</span><span class="sxs-lookup"><span data-stu-id="e5631-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e5631-124">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="e5631-124">Create the model</span></span>

<span data-ttu-id="e5631-125">Definiowanie kontekstu i jednostki klasy, które tworzą modelu.</span><span class="sxs-lookup"><span data-stu-id="e5631-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="e5631-126">Utwórz nową *Model.cs* pliku z następującą zawartość.</span><span class="sxs-lookup"><span data-stu-id="e5631-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="e5631-127">Porada: W rzeczywistej aplikacji można będzie umieścić każda klasa w osobnym pliku, a ciąg połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e5631-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="e5631-128">Aby zachować samouczka proste, wprowadzamy wszystko w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="e5631-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e5631-129">Utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="e5631-129">Create the database</span></span>

<span data-ttu-id="e5631-130">Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5631-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="e5631-131">Uruchom `dotnet ef migrations add InitialCreate` utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="e5631-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="e5631-132">Uruchom `dotnet ef database update` na zastosowanie nowych migracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e5631-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="e5631-133">To polecenie tworzy bazy danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="e5631-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="e5631-134">Używając ścieżek względnych z SQLite ścieżka będzie względem zestawu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5631-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="e5631-135">W tym przykładzie jest głównym pliku binarnego `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, więc będzie bazy danych SQLite w `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="e5631-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="e5631-136">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="e5631-136">Use your model</span></span>

* <span data-ttu-id="e5631-137">Otwórz *Program.cs* i Zastąp zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e5631-137">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="e5631-138">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e5631-138">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="e5631-139">Jeden blogu jest zapisywana w bazie danych i szczegółowe informacje o wszystkich blogi są wyświetlane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="e5631-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="e5631-140">Zmiana modelu:</span><span class="sxs-lookup"><span data-stu-id="e5631-140">Changing the model:</span></span>

- <span data-ttu-id="e5631-141">Jeśli wprowadzisz zmiany w modelu, możesz użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowy [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) dokonanie odpowiedniej schematu zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e5631-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="e5631-142">Po zaznaczeniu tej opcji szkieletu kodu (i wszelkie wymagane zmiany wprowadzone), można użyć `dotnet ef database update` polecenie, aby zastosować zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5631-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="e5631-143">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5631-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="e5631-144">SQLite nie obsługuje wszystkich migracji (zmiany schematu) ze względu na ograniczenia w SQLite.</span><span class="sxs-lookup"><span data-stu-id="e5631-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="e5631-145">Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="e5631-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="e5631-146">W przypadku nowych aplikacji rozważ porzucenie bazy danych i tworzenia nowego zamiast migracje po zmianie modelu.</span><span class="sxs-lookup"><span data-stu-id="e5631-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5631-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e5631-147">Additional Resources</span></span>

* <span data-ttu-id="e5631-148">[Oprogramowanie .NET core - nowej bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek EF konsoli i platform.</span><span class="sxs-lookup"><span data-stu-id="e5631-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="e5631-149">Wprowadzenie do platformy ASP.NET Core MVC Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="e5631-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="e5631-150">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5631-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="e5631-151">Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5631-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
