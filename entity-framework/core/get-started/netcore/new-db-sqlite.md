---
title: Wprowadzenie do programu .NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
description: Wprowadzenie do platformy .NET Core przy użyciu platformy Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993695"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="37ac3-103">Wprowadzenie do nowej bazy danych z programem EF Core w aplikacji Konsolowej .NET Core</span><span class="sxs-lookup"><span data-stu-id="37ac3-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="37ac3-104">W tym samouczku utworzysz aplikację konsoli .NET Core, która wykonuje dostęp do danych w bazie danych SQLite przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="37ac3-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="37ac3-105">Migracje umożliwia tworzenie bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="37ac3-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="37ac3-106">Zobacz [ASP.NET Core — Nowa baza danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="37ac3-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="37ac3-107">Wyświetlanie przykładowych w tym artykule w witrynie GitHub] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="37ac3-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37ac3-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="37ac3-108">Prerequisites</span></span>

* [<span data-ttu-id="37ac3-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="37ac3-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="37ac3-110">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="37ac3-110">Create a new project</span></span>

* <span data-ttu-id="37ac3-111">Utwórz nowy projekt konsoli:</span><span class="sxs-lookup"><span data-stu-id="37ac3-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="37ac3-112">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="37ac3-112">Install Entity Framework Core</span></span>

<span data-ttu-id="37ac3-113">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="37ac3-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="37ac3-114">W tym instruktażu wykorzystano bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="37ac3-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="37ac3-115">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="37ac3-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="37ac3-116">Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="37ac3-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="37ac3-117">Uruchom `dotnet restore` zainstalować nowe pakiety.</span><span class="sxs-lookup"><span data-stu-id="37ac3-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="37ac3-118">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="37ac3-118">Create the model</span></span>

<span data-ttu-id="37ac3-119">Definiowanie klas jednostek i kontekstu, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="37ac3-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="37ac3-120">Utwórz nową *Model.cs* pliku o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="37ac3-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="37ac3-121">Porada: W rzeczywistej aplikacji, możesz umieścić każda klasa w oddzielnym pliku, a parametry połączenia w zmiennej środowisku lub pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="37ac3-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="37ac3-122">W celu uproszczenia tego samouczka wszystko, co znajduje się w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="37ac3-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="37ac3-123">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="37ac3-123">Create the database</span></span>

<span data-ttu-id="37ac3-124">Po utworzeniu modelu, użyj [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="37ac3-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="37ac3-125">Uruchom `dotnet ef migrations add InitialCreate` tworzenia szkieletu migracji i utworzyć początkowy zestaw tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="37ac3-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="37ac3-126">Uruchom `dotnet ef database update` zastosować nową migrację do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37ac3-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="37ac3-127">To polecenie tworzy bazę danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="37ac3-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="37ac3-128">*Blogging.db*\* bazy danych SQLite znajduje się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="37ac3-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="37ac3-129">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="37ac3-129">Use the model</span></span>

* <span data-ttu-id="37ac3-130">Otwórz *Program.cs* i zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="37ac3-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="37ac3-131">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="37ac3-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="37ac3-132">Blog jeden jest zapisywany w bazie danych i szczegółowe informacje o wszystkich blogów są wyświetlane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="37ac3-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="37ac3-133">Zmiana modelu:</span><span class="sxs-lookup"><span data-stu-id="37ac3-133">Changing the model:</span></span>

- <span data-ttu-id="37ac3-134">Jeśli wprowadzisz zmiany w modelu, można użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowego [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="37ac3-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="37ac3-135">Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `dotnet ef database update` polecenie, aby zastosować schemat zmienia się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="37ac3-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="37ac3-136">Używa programu EF Core `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37ac3-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="37ac3-137">Aparat bazy danych SQLite nie obsługuje niektórych zmiany schematu, które są obsługiwane w większości innych relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="37ac3-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="37ac3-138">Na przykład `DropColumn` operacja nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="37ac3-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="37ac3-139">EF Core migracji wygeneruje kod dla tych operacji.</span><span class="sxs-lookup"><span data-stu-id="37ac3-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="37ac3-140">Ale Jeśli spróbujesz zastosować je do bazy danych lub wygenerować skrypt programu EF Core zgłasza wyjątek wyjątków.</span><span class="sxs-lookup"><span data-stu-id="37ac3-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="37ac3-141">Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="37ac3-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="37ac3-142">W nowych wdrożeniach należy wziąć pod uwagę porzucenie bazy danych i tworzenia nowej, a nie przy użyciu migracji po zmianie modelu.</span><span class="sxs-lookup"><span data-stu-id="37ac3-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37ac3-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="37ac3-143">Additional Resources</span></span>

* [<span data-ttu-id="37ac3-144">Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="37ac3-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="37ac3-145">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ac3-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="37ac3-146">Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ac3-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
