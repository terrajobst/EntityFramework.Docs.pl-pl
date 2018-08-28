---
title: Wprowadzenie do platformy UWP — Nowa baza danych — EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996913"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="a6c7c-102">Wprowadzenie do programu EF Core na platformie Universal Windows (UWP) przy użyciu nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="a6c7c-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="a6c7c-103">W tym samouczku utworzysz aplikację platformy uniwersalnej Windows (UWP), która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="a6c7c-104">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="a6c7c-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6c7c-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a6c7c-105">Prerequisites</span></span>

* <span data-ttu-id="a6c7c-106">[Windows 10 Fall Creators Update (10.0; Kompilacja 16299) lub nowszym](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="a6c7c-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="a6c7c-107">[Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/) z **Universal Windows Platform Development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="a6c7c-108">[Zestaw SDK programu .NET core 2.1 lub nowszej](https://www.microsoft.com/net/core) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="a6c7c-109">Tworzenie projektu modelu</span><span class="sxs-lookup"><span data-stu-id="a6c7c-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6c7c-110">Ze względu na ograniczenia w taki sposób, narzędzia platformy .NET Core wchodzą w interakcję z projektów platformy UWP modelu musi być umieszczona w projekcie bez platformy UWP, aby można było uruchamiać polecenia migracji w **Konsola Menedżera pakietów** (PMC)</span><span class="sxs-lookup"><span data-stu-id="a6c7c-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="a6c7c-111">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6c7c-111">Open Visual Studio</span></span>

* <span data-ttu-id="a6c7c-112">**Plik > Nowy > Projekt**</span><span class="sxs-lookup"><span data-stu-id="a6c7c-112">**File > New > Project**</span></span>

* <span data-ttu-id="a6c7c-113">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="a6c7c-114">Wybierz **biblioteki klas (.NET Standard)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="a6c7c-115">Nadaj projektowi nazwę *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="a6c7c-116">Nazwij rozwiązanie *do obsługi blogów*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="a6c7c-117">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="a6c7c-118">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a6c7c-118">Install Entity Framework Core</span></span>

<span data-ttu-id="a6c7c-119">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="a6c7c-120">Ten samouczek używa bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="a6c7c-121">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a6c7c-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="a6c7c-122">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="a6c7c-123">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="a6c7c-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="a6c7c-124">W dalszej części tego samouczka używana będzie niektóre narzędzia Entity Framework Core do obsługi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="a6c7c-125">Więc Zainstaluj również pakiet narzędzi.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-125">So install the tools package as well.</span></span>

* <span data-ttu-id="a6c7c-126">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="a6c7c-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="a6c7c-127">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="a6c7c-127">Create the model</span></span>

<span data-ttu-id="a6c7c-128">Teraz nadszedł czas, do definiowania klas kontekstu i jednostek, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="a6c7c-129">Usuń *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="a6c7c-130">Tworzenie *Model.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a6c7c-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="a6c7c-131">Utwórz nowy projekt platformy uniwersalnej systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a6c7c-131">Create a new UWP project</span></span>

* <span data-ttu-id="a6c7c-132">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="a6c7c-133">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Universal**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="a6c7c-134">Wybierz **pusta aplikacja (Windows Universal)** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="a6c7c-135">Nadaj projektowi nazwę *Blogging.UWP*i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="a6c7c-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="a6c7c-136">Co najmniej równa wersje docelową i minimalną **Windows 10 Fall Creators Update (10.0; kompilacja 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="a6c7c-137">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="a6c7c-137">Create the initial migration</span></span>

<span data-ttu-id="a6c7c-138">Teraz, gdy model, należy skonfigurować aplikację do tworzenia bazy danych przy pierwszym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="a6c7c-139">W tej sekcji opisano tworzenie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="a6c7c-140">W poniższej sekcji dodasz kod, który ma zastosowanie tej migracji, po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="a6c7c-141">Narzędzia migracji wymagają projektu startowego bez platformy UWP, dzięki czemu można tworzyć najpierw.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="a6c7c-142">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="a6c7c-143">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="a6c7c-144">Wybierz **Aplikacja konsoli (.NET Core)** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="a6c7c-145">Nadaj projektowi nazwę *Blogging.Migrations.Startup*i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="a6c7c-146">Dodaj odwołanie do projektu z *Blogging.Migrations.Startup* projekt *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="a6c7c-147">Teraz możesz utworzyć początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="a6c7c-148">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="a6c7c-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="a6c7c-149">Wybierz *Blogging.Model* projektu jako **projekt domyślny**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="a6c7c-150">W **Eksploratora rozwiązań**ustaw *Blogging.Migrations.Startup* projekt jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="a6c7c-151">Uruchom `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="a6c7c-152">To polecenie scaffolds migracji, który tworzy początkowego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="a6c7c-153">Tworzenie bazy danych podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="a6c7c-153">Create the database on app startup</span></span>

<span data-ttu-id="a6c7c-154">Ponieważ baza danych ma zostać utworzony na urządzeniu, która jest uruchamiana aplikacja, należy dodać kod Zastosuj wszelkie oczekujące migracji w lokalnej bazie danych podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="a6c7c-155">Przy pierwszym uruchomieniu aplikacji, to zajmie się tworzenie lokalnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="a6c7c-156">Dodaj odwołanie do projektu z *Blogging.UWP* projekt *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="a6c7c-157">Otwórz *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="a6c7c-158">Dodaj wyróżniony kod, aby zastosować wszelkie oczekujące migracji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="a6c7c-159">W przypadku zmiany modelu użycia `Add-Migration` polecenia do tworzenia szkieletu nową migrację do zastosowania odpowiednich zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="a6c7c-160">Wszystkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="a6c7c-161">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="a6c7c-162">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="a6c7c-162">Use the model</span></span>

<span data-ttu-id="a6c7c-163">Można teraz używać modelu przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="a6c7c-164">Otwórz *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="a6c7c-165">Dodawanie obsługi ładowania strony i UI zawartości, które przedstawiono poniżej</span><span class="sxs-lookup"><span data-stu-id="a6c7c-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="a6c7c-166">Teraz Dodaj kod, aby Podłączanie do interfejsu użytkownika z bazą danych</span><span class="sxs-lookup"><span data-stu-id="a6c7c-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="a6c7c-167">Otwórz *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="a6c7c-168">Dodaj wyróżniony kod z poniższej listy:</span><span class="sxs-lookup"><span data-stu-id="a6c7c-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="a6c7c-169">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="a6c7c-170">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Blogging.UWP* projektu, a następnie wybierz pozycję **Wdróż**.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="a6c7c-171">Ustaw *Blogging.UWP* jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="a6c7c-172">**Debuguj > Uruchom bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="a6c7c-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="a6c7c-173">Aplikacja tworzy i uruchamia.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-173">The app builds and runs.</span></span>

* <span data-ttu-id="a6c7c-174">Wprowadź adres URL, a następnie kliknij przycisk **Dodaj** przycisku</span><span class="sxs-lookup"><span data-stu-id="a6c7c-174">Enter a URL and click the **Add** button</span></span>

  ![obraz](_static/create.png)

  ![obraz](_static/list.png)

  <span data-ttu-id="a6c7c-177">Tada!</span><span class="sxs-lookup"><span data-stu-id="a6c7c-177">Tada!</span></span> <span data-ttu-id="a6c7c-178">Masz teraz platformy Entity Framework Core uruchomioną prostą aplikacją platformy uniwersalnej systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6c7c-179">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a6c7c-179">Next steps</span></span>

<span data-ttu-id="a6c7c-180">Uzyskać zgodności i wydajności, którą należy wiedzieć podczas korzystania z programu EF Core przy użyciu platformy uniwersalnej systemu Windows, zobacz [implementacji platformy .NET obsługiwanych przez platformę EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="a6c7c-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="a6c7c-181">Zapoznaj się z innymi artykułami w tej dokumentacji, aby dowiedzieć się więcej na temat funkcji platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a6c7c-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
