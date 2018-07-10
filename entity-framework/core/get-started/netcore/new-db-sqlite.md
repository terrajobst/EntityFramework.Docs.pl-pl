---
title: Wprowadzenie do programu .NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Wprowadzenie do platformy .NET Core przy użyciu platformy Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911505"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="9b8f9-104">Wprowadzenie do nowej bazy danych z programem EF Core w aplikacji Konsolowej .NET Core</span><span class="sxs-lookup"><span data-stu-id="9b8f9-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="9b8f9-105">W tym instruktażu utworzysz aplikację konsoli .NET Core, która wykonuje dostęp do danych w bazie danych SQLite przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="9b8f9-106">Migracje umożliwia tworzenie bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="9b8f9-107">Zobacz [ASP.NET Core — Nowa baza danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="9b8f9-108">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b8f9-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9b8f9-109">Prerequisites</span></span>

<span data-ttu-id="9b8f9-110">[.NET Core SDK](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="9b8f9-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="9b8f9-111">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="9b8f9-111">Create a new project</span></span>

* <span data-ttu-id="9b8f9-112">Utwórz nowy projekt konsoli:</span><span class="sxs-lookup"><span data-stu-id="9b8f9-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="9b8f9-113">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9b8f9-113">Install Entity Framework Core</span></span>

<span data-ttu-id="9b8f9-114">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9b8f9-115">W tym instruktażu wykorzystano bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="9b8f9-116">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9b8f9-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9b8f9-117">Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="9b8f9-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="9b8f9-118">Uruchom `dotnet restore` zainstalować nowe pakiety.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9b8f9-119">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="9b8f9-119">Create the model</span></span>

<span data-ttu-id="9b8f9-120">Definiowanie klas jednostek i kontekstu, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="9b8f9-121">Utwórz nową *Model.cs* pliku o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="9b8f9-122">Porada: W rzeczywistej aplikacji, możesz umieścić każda klasa w oddzielnym pliku, a parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="9b8f9-123">W celu uproszczenia tego samouczka wszystko, co znajduje się w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9b8f9-124">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b8f9-124">Create the database</span></span>

<span data-ttu-id="9b8f9-125">Po utworzeniu modelu, użyj [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="9b8f9-126">Uruchom `dotnet ef migrations add InitialCreate` tworzenia szkieletu migracji i utworzyć początkowy zestaw tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="9b8f9-127">Uruchom `dotnet ef database update` zastosować nową migrację do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="9b8f9-128">To polecenie tworzy bazę danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="9b8f9-129">*Blogging.db*\* bazy danych SQLite znajduje się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="9b8f9-130">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="9b8f9-130">Use your model</span></span>

* <span data-ttu-id="9b8f9-131">Otwórz *Program.cs* i zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9b8f9-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="9b8f9-132">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9b8f9-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="9b8f9-133">Blog jeden jest zapisywany w bazie danych i szczegółowe informacje o wszystkich blogów są wyświetlane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="9b8f9-134">Zmiana modelu:</span><span class="sxs-lookup"><span data-stu-id="9b8f9-134">Changing the model:</span></span>

- <span data-ttu-id="9b8f9-135">W przypadku wprowadzenia zmian do modelu, można użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowego [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) się odpowiedni schemat zmienia się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="9b8f9-136">Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `dotnet ef database update` polecenie, aby zastosować zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="9b8f9-137">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="9b8f9-138">Bazy danych SQLite nie obsługuje wszystkich migracji (zmiany schematu) ze względu na ograniczenia w bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="9b8f9-139">Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="9b8f9-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="9b8f9-140">W nowych wdrożeniach należy wziąć pod uwagę porzucenie bazy danych i tworzenia nowej, a nie przy użyciu migracji po zmianie modelu.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b8f9-141">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9b8f9-141">Additional Resources</span></span>

* <span data-ttu-id="9b8f9-142">[.NET core — Nowa baza danych za pomocą SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="9b8f9-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="9b8f9-143">Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="9b8f9-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="9b8f9-144">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b8f9-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="9b8f9-145">Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b8f9-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
