---
title: Wprowadzenie do platformy .NET Framework — Nowa baza danych — EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997590"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="154d2-102">Wprowadzenie do programu EF Core w programie .NET Framework za pomocą nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="154d2-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="154d2-103">W tym samouczku utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="154d2-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="154d2-104">Migracje umożliwia tworzenie bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="154d2-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="154d2-105">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="154d2-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="154d2-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="154d2-106">Prerequisites</span></span>

* [<span data-ttu-id="154d2-107">Visual Studio 2017 w wersji 15.7 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="154d2-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="154d2-108">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="154d2-108">Create a new project</span></span>

* <span data-ttu-id="154d2-109">Otwórz program Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="154d2-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="154d2-110">**Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="154d2-110">**File > New > Project...**</span></span>

* <span data-ttu-id="154d2-111">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Desktop**</span><span class="sxs-lookup"><span data-stu-id="154d2-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="154d2-112">Wybierz **Aplikacja konsoli (.NET Framework)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="154d2-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="154d2-113">Upewnij się, że projekt jest ukierunkowany **platformy .NET Framework 4.6.1** lub nowszej</span><span class="sxs-lookup"><span data-stu-id="154d2-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="154d2-114">Nadaj projektowi nazwę *ConsoleApp.NewDb* i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="154d2-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="154d2-115">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="154d2-115">Install Entity Framework</span></span>

<span data-ttu-id="154d2-116">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="154d2-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="154d2-117">Ten samouczek używa programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="154d2-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="154d2-118">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="154d2-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="154d2-119">Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="154d2-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="154d2-120">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="154d2-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="154d2-121">W dalszej części tego samouczka użyjesz niektórych narzędzi Entity Framework Tools do obsługi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="154d2-122">Więc Zainstaluj również pakiet narzędzi.</span><span class="sxs-lookup"><span data-stu-id="154d2-122">So install the tools package as well.</span></span>

* <span data-ttu-id="154d2-123">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="154d2-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="154d2-124">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="154d2-124">Create the model</span></span>

<span data-ttu-id="154d2-125">Teraz nadszedł czas, do definiowania klas kontekstu i jednostek, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="154d2-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="154d2-126">**Projekt > Dodaj klasę...**</span><span class="sxs-lookup"><span data-stu-id="154d2-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="154d2-127">Wprowadź *Model.cs* jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="154d2-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="154d2-128">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="154d2-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="154d2-129">W rzeczywistej aplikacji można będzie umieścić każda klasa w oddzielnym pliku, a parametry połączenia w zmiennej środowisku lub pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="154d2-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="154d2-130">Dla uproszczenia wszystko jest w pliku pojedynczego kodu na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="154d2-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="154d2-131">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="154d2-131">Create the database</span></span>

<span data-ttu-id="154d2-132">Teraz, gdy model, można użyć migracje utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="154d2-133">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="154d2-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="154d2-134">Uruchom `Add-Migration InitialCreate` do tworzenia szkieletu migracji, aby utworzyć początkowy zestaw tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="154d2-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="154d2-135">Uruchom `Update-Database` zastosować nową migrację do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="154d2-136">Ponieważ baza danych nie istnieje, zostanie utworzony, przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="154d2-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="154d2-137">Jeśli wprowadzisz zmiany w modelu, można użyć `Add-Migration` polecenia do tworzenia szkieletu nowej migracji w celu sprawdzenia odpowiedni schemat zmienia się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="154d2-138">Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `Update-Database` polecenie, aby zastosować zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="154d2-139">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="154d2-140">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="154d2-140">Use the model</span></span>

<span data-ttu-id="154d2-141">Można teraz używać modelu przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="154d2-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="154d2-142">Otwórz *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="154d2-142">Open *Program.cs*</span></span>

* <span data-ttu-id="154d2-143">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="154d2-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="154d2-144">**Debuguj > Uruchom bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="154d2-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="154d2-145">Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="154d2-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![obraz](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="154d2-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="154d2-147">Additional Resources</span></span>

* [<span data-ttu-id="154d2-148">EF Core w programie .NET Framework przy użyciu istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="154d2-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="154d2-149">[EF Core na platformie .NET Core za pomocą nowej bazy danych — bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="154d2-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
