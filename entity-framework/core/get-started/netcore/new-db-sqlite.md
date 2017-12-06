---
title: "Wprowadzenie .NET Core - nowej bazy danych — EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "Rozpoczynanie pracy z platformą .NET Core przy użyciu programu Entity Framework Core"
keywords: .NET core, Entity Framework Core, programu VS, kod programu Visual Studio, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="13a41-104">Wprowadzenie do podstawowych EF w aplikacji konsoli .NET Core nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="13a41-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="13a41-105">W tym przewodniku spowoduje utworzenie aplikacji konsoli .NET Core, który wykonuje dostęp do podstawowych danych względem bazy danych SQLite przy użyciu programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="13a41-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="13a41-106">Migracje użyje do utworzenia bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="13a41-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="13a41-107">Zobacz [platformy ASP.NET Core - nową bazę danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="13a41-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="13a41-108">[.NET Core SDK](https://www.microsoft.com/net/download/core) nie obsługuje już `project.json` lub programu Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="13a41-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="13a41-109">Firma Microsoft zaleca [migracji z project.json do csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="13a41-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="13a41-110">Jeśli używasz programu Visual Studio, zaleca się migrowanie do [programu Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="13a41-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="13a41-111">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="13a41-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13a41-112">Wstępnie wymagane składniki</span><span class="sxs-lookup"><span data-stu-id="13a41-112">Prerequisites</span></span>

<span data-ttu-id="13a41-113">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="13a41-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="13a41-114">System operacyjny obsługuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="13a41-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="13a41-115">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (mimo że instrukcje może służyć do tworzenia aplikacji z poprzedniej wersji z bardzo kilka zmian).</span><span class="sxs-lookup"><span data-stu-id="13a41-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="13a41-116">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="13a41-116">Create a new project</span></span>

* <span data-ttu-id="13a41-117">Utwórz nową `ConsoleApp.SQLite` folderu projektu i użyj `dotnet` polecenie, aby umieścić w nim aplikacji .NET Core.</span><span class="sxs-lookup"><span data-stu-id="13a41-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="13a41-118">Zainstaluj program Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="13a41-118">Install Entity Framework Core</span></span>

<span data-ttu-id="13a41-119">Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="13a41-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="13a41-120">W tym przewodniku zastosowano SQLite.</span><span class="sxs-lookup"><span data-stu-id="13a41-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="13a41-121">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="13a41-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="13a41-122">Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="13a41-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="13a41-123">Ręcznie Edytuj `ConsoleApp.SQLite.csproj` dodać DotNetCliToolReference do Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="13a41-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 <span data-ttu-id="13a41-124">Uwaga: Przyszłej wersji `dotnet` będzie obsługiwać DotNetCliToolReferences za pośrednictwem`dotnet add tool`</span><span class="sxs-lookup"><span data-stu-id="13a41-124">Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`</span></span>

<span data-ttu-id="13a41-125">`ConsoleApp.SQLite.csproj`teraz powinny zawierać następujące:</span><span class="sxs-lookup"><span data-stu-id="13a41-125">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="13a41-126">Uwaga: Numery wersji użyta powyżej były prawidłowe w czasie publikowania.</span><span class="sxs-lookup"><span data-stu-id="13a41-126">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="13a41-127">Uruchom `dotnet restore` do zainstalowania nowych pakietów.</span><span class="sxs-lookup"><span data-stu-id="13a41-127">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="13a41-128">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="13a41-128">Create the model</span></span>

<span data-ttu-id="13a41-129">Definiowanie kontekstu i jednostki klasy, które tworzą modelu.</span><span class="sxs-lookup"><span data-stu-id="13a41-129">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="13a41-130">Utwórz nową *Model.cs* pliku z następującą zawartość.</span><span class="sxs-lookup"><span data-stu-id="13a41-130">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="13a41-131">Porada: W rzeczywistej aplikacji można będzie umieścić każda klasa w osobnym pliku, a ciąg połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="13a41-131">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="13a41-132">Aby zachować samouczka proste, wprowadzamy wszystko w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="13a41-132">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="13a41-133">Utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="13a41-133">Create the database</span></span>

<span data-ttu-id="13a41-134">Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="13a41-134">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="13a41-135">Uruchom `dotnet ef migrations add InitialCreate` utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="13a41-135">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="13a41-136">Uruchom `dotnet ef database update` na zastosowanie nowych migracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13a41-136">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="13a41-137">To polecenie tworzy bazy danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="13a41-137">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="13a41-138">Używając ścieżek względnych z SQLite ścieżka będzie względem zestawu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13a41-138">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="13a41-139">W tym przykładzie jest głównym pliku binarnego `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, więc będzie bazy danych SQLite w `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="13a41-139">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="13a41-140">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="13a41-140">Use your model</span></span>

* <span data-ttu-id="13a41-141">Otwórz *Program.cs* i Zastąp zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="13a41-141">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="13a41-142">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="13a41-142">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="13a41-143">Jeden blogu jest zapisywana w bazie danych i szczegółowe informacje o wszystkich blogi są wyświetlane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="13a41-143">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="13a41-144">Zmiana modelu:</span><span class="sxs-lookup"><span data-stu-id="13a41-144">Changing the model:</span></span>

- <span data-ttu-id="13a41-145">Jeśli wprowadzisz zmiany w modelu, możesz użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowy [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) dokonanie odpowiedniej schematu zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13a41-145">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="13a41-146">Po zaznaczeniu tej opcji szkieletu kodu (i wszelkie wymagane zmiany wprowadzone), można użyć `dotnet ef database update` polecenie, aby zastosować zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="13a41-146">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="13a41-147">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="13a41-147">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="13a41-148">SQLite nie obsługuje wszystkich migracji (zmiany schematu) ze względu na ograniczenia w SQLite.</span><span class="sxs-lookup"><span data-stu-id="13a41-148">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="13a41-149">Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="13a41-149">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="13a41-150">W przypadku nowych aplikacji rozważ porzucenie bazy danych i tworzenia nowego zamiast migracje po zmianie modelu.</span><span class="sxs-lookup"><span data-stu-id="13a41-150">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13a41-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="13a41-151">Additional Resources</span></span>

* <span data-ttu-id="13a41-152">[Oprogramowanie .NET core - nowej bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek EF konsoli i platform.</span><span class="sxs-lookup"><span data-stu-id="13a41-152">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="13a41-153">Wprowadzenie do platformy ASP.NET Core MVC Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="13a41-153">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="13a41-154">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13a41-154">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="13a41-155">Wprowadzenie do platformy ASP.NET Core oraz Entity Framework Core za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13a41-155">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
