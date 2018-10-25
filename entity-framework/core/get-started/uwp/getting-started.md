---
title: Wprowadzenie do platformy UWP — Nowa baza danych — EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022379"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="67e0a-102">Wprowadzenie do programu EF Core na platformie Universal Windows (UWP) przy użyciu nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="67e0a-103">W tym samouczku utworzysz aplikację platformy uniwersalnej Windows (UWP), która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="67e0a-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="67e0a-104">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="67e0a-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67e0a-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="67e0a-105">Prerequisites</span></span>

* <span data-ttu-id="67e0a-106">[Windows 10 Fall Creators Update (10.0; Kompilacja 16299) lub nowszym](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="67e0a-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="67e0a-107">[Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/) z **Universal Windows Platform Development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="67e0a-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="67e0a-108">[Zestaw SDK programu .NET core 2.1 lub nowszej](https://www.microsoft.com/net/core) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="67e0a-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67e0a-109">Ten samouczek używa platformy Entity Framework Core [migracje](xref:core/managing-schemas/migrations/index) poleceń do tworzenia i aktualizowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="67e0a-110">Te polecenia nie działają bezpośrednio z projektów platformy UWP.</span><span class="sxs-lookup"><span data-stu-id="67e0a-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="67e0a-111">Z tego powodu modelu danych aplikacji znajduje się w projekcie biblioteki udostępnionej, a oddzielną aplikację konsoli .NET Core jest używany do uruchamiania polecenia.</span><span class="sxs-lookup"><span data-stu-id="67e0a-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="67e0a-112">Utwórz projekt biblioteki do przechowywania modelu danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="67e0a-113">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67e0a-113">Open Visual Studio</span></span>

* <span data-ttu-id="67e0a-114">**Plik > Nowy > Projekt**</span><span class="sxs-lookup"><span data-stu-id="67e0a-114">**File > New > Project**</span></span>

* <span data-ttu-id="67e0a-115">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="67e0a-116">Wybierz **biblioteki klas (.NET Standard)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="67e0a-117">Nadaj projektowi nazwę *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="67e0a-118">Nazwij rozwiązanie *do obsługi blogów*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="67e0a-119">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="67e0a-120">Zainstaluj środowisko uruchomieniowe programu Entity Framework Core w projekcie modelu danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="67e0a-121">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="67e0a-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="67e0a-122">Ten samouczek używa bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="67e0a-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="67e0a-123">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="67e0a-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="67e0a-124">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="67e0a-125">Upewnij się, że projekt biblioteki *Blogging.Model* jest wybrany jako **domyślny projekt** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="67e0a-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="67e0a-126">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="67e0a-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="67e0a-127">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-127">Create the data model</span></span>

<span data-ttu-id="67e0a-128">Teraz nadszedł czas, aby zdefiniować *DbContext* i klas jednostek, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="67e0a-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="67e0a-129">Usuń *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="67e0a-130">Tworzenie *Model.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="67e0a-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="67e0a-131">Utwórz nowy projekt konsoli, aby uruchamiać polecenia migracji</span><span class="sxs-lookup"><span data-stu-id="67e0a-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="67e0a-132">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="67e0a-133">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="67e0a-134">Wybierz **Aplikacja konsoli (.NET Core)** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="67e0a-135">Nadaj projektowi nazwę *Blogging.Migrations.Startup*i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="67e0a-136">Dodaj odwołanie do projektu z *Blogging.Migrations.Startup* projekt *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="67e0a-137">Zainstaluj narzędzia Entity Framework Core w projekcie uruchamiania migracji</span><span class="sxs-lookup"><span data-stu-id="67e0a-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="67e0a-138">Aby włączyć polecenia migracji EF Core w konsoli Menedżera pakietów, należy zainstalować pakiet narzędzi programu EF Core w aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="67e0a-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="67e0a-139">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="67e0a-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="67e0a-140">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="67e0a-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="67e0a-141">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="67e0a-141">Create the initial migration</span></span>

 <span data-ttu-id="67e0a-142">Tworzenie początkowej migracji, określenie aplikacji konsoli jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="67e0a-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="67e0a-143">Uruchom `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="67e0a-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="67e0a-144">To polecenie scaffolds migracji, który tworzy początkowy zestaw tabel bazy danych dla modelu danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="67e0a-145">Tworzenie projektu platformy uniwersalnej systemu Windows</span><span class="sxs-lookup"><span data-stu-id="67e0a-145">Create the UWP project</span></span>

* <span data-ttu-id="67e0a-146">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="67e0a-147">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Universal**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="67e0a-148">Wybierz **pusta aplikacja (Windows Universal)** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="67e0a-149">Nadaj projektowi nazwę *Blogging.UWP*i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="67e0a-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67e0a-150">Co najmniej równa wersje docelową i minimalną **Windows 10 Fall Creators Update (10.0; kompilacja 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="67e0a-151">Poprzednie wersje systemu Windows 10 nie obsługują .NET Standard 2.0, która jest wymagana przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="67e0a-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="67e0a-152">Dodaj kod, aby utworzyć bazę danych podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="67e0a-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="67e0a-153">Ponieważ baza danych ma zostać utworzony na urządzeniu, która jest uruchamiana aplikacja, należy dodać kod Zastosuj wszelkie oczekujące migracji w lokalnej bazie danych podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="67e0a-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="67e0a-154">Przy pierwszym uruchomieniu aplikacji, to zajmie się tworzenie lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="67e0a-155">Dodaj odwołanie do projektu z *Blogging.UWP* projekt *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="67e0a-156">Otwórz *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="67e0a-157">Dodaj wyróżniony kod, aby zastosować wszelkie oczekujące migracji.</span><span class="sxs-lookup"><span data-stu-id="67e0a-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="67e0a-158">W przypadku zmiany modelu użycia `Add-Migration` polecenia do tworzenia szkieletu nową migrację do zastosowania odpowiednich zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="67e0a-159">Wszystkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="67e0a-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="67e0a-160">Używa programu EF Core `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="67e0a-161">Przy użyciu modelu danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-161">Use the data model</span></span>

<span data-ttu-id="67e0a-162">Można teraz używać programu EF Core przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="67e0a-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="67e0a-163">Otwórz *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="67e0a-164">Dodawanie obsługi ładowania strony i UI zawartości, które przedstawiono poniżej</span><span class="sxs-lookup"><span data-stu-id="67e0a-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="67e0a-165">Teraz Dodaj kod, aby Podłączanie do interfejsu użytkownika z bazą danych</span><span class="sxs-lookup"><span data-stu-id="67e0a-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="67e0a-166">Otwórz *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="67e0a-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="67e0a-167">Dodaj wyróżniony kod z poniższej listy:</span><span class="sxs-lookup"><span data-stu-id="67e0a-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="67e0a-168">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="67e0a-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="67e0a-169">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Blogging.UWP* projektu, a następnie wybierz pozycję **Wdróż**.</span><span class="sxs-lookup"><span data-stu-id="67e0a-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="67e0a-170">Ustaw *Blogging.UWP* jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="67e0a-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="67e0a-171">**Debuguj > Uruchom bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="67e0a-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="67e0a-172">Aplikacja tworzy i uruchamia.</span><span class="sxs-lookup"><span data-stu-id="67e0a-172">The app builds and runs.</span></span>

* <span data-ttu-id="67e0a-173">Wprowadź adres URL, a następnie kliknij przycisk **Dodaj** przycisku</span><span class="sxs-lookup"><span data-stu-id="67e0a-173">Enter a URL and click the **Add** button</span></span>

  ![obraz](_static/create.png)

  ![obraz](_static/list.png)

  <span data-ttu-id="67e0a-176">Tada!</span><span class="sxs-lookup"><span data-stu-id="67e0a-176">Tada!</span></span> <span data-ttu-id="67e0a-177">Masz teraz platformy Entity Framework Core uruchomioną prostą aplikacją platformy uniwersalnej systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="67e0a-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67e0a-178">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="67e0a-178">Next steps</span></span>

<span data-ttu-id="67e0a-179">Uzyskać zgodności i wydajności, którą należy wiedzieć podczas korzystania z programu EF Core przy użyciu platformy uniwersalnej systemu Windows, zobacz [implementacji platformy .NET obsługiwanych przez platformę EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="67e0a-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="67e0a-180">Zapoznaj się z innymi artykułami w tej dokumentacji, aby dowiedzieć się więcej na temat funkcji platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="67e0a-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
