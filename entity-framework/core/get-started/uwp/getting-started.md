---
title: "Wprowadzenie do platformy uniwersalnej systemu Windows — nowej bazy danych — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="467ee-102">Wprowadzenie do podstawowych EF na platformę uniwersalną systemu Windows (UWP) z nową bazę danych</span><span class="sxs-lookup"><span data-stu-id="467ee-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="467ee-103">W tym samouczku używana Core EF 2.0.1 (wydane z platformy ASP.NET Core i .NET Core SDK 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="467ee-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="467ee-104">EF Core 2.0.0 brakuje niektórych ważnych poprawki wymagane do dobrej obsługi platformy uniwersalnej systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="467ee-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="467ee-105">W tym przewodniku będzie tworzenia aplikacji systemu Windows platformy Uniwersalnej, która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="467ee-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="467ee-106">**Należy rozważyć unikanie typy anonimowe w zapytaniach LINQ na platformy uniwersalnej systemu Windows**.</span><span class="sxs-lookup"><span data-stu-id="467ee-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="467ee-107">Wdrażanie aplikacji do sklepu z aplikacjami platformy uniwersalnej systemu Windows wymaga aplikacji do skompilowania z platformą .NET Native.</span><span class="sxs-lookup"><span data-stu-id="467ee-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="467ee-108">Zapytania z typy anonimowe ma pogorszenie wydajności na platformie .NET Native.</span><span class="sxs-lookup"><span data-stu-id="467ee-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="467ee-109">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="467ee-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="467ee-110">Wstępnie wymagane składniki</span><span class="sxs-lookup"><span data-stu-id="467ee-110">Prerequisites</span></span>

<span data-ttu-id="467ee-111">Poniższe elementy są wymagane w tym przewodniku:</span><span class="sxs-lookup"><span data-stu-id="467ee-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="467ee-112">[Windows 10 twórców spadku zaktualizować](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="467ee-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="467ee-113">[Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="467ee-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="467ee-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15,4 lub nowszym z **platformy uniwersalnej systemu Windows dla deweloperów** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="467ee-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="467ee-115">Utwórz nowy projekt modelu</span><span class="sxs-lookup"><span data-stu-id="467ee-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="467ee-116">Ze względu na ograniczenia w narzędziach .NET Core sposób interakcji z projektów uniwersalnych systemu Windows, które modelu musi być umieszczona w projekcie aplikacje platformy UWP, aby można było uruchamiać polecenia migracji w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="467ee-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="467ee-117">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="467ee-117">Open Visual Studio</span></span>

* <span data-ttu-id="467ee-118">Plik > Nowy > Projekt...</span><span class="sxs-lookup"><span data-stu-id="467ee-118">File > New > Project...</span></span>

* <span data-ttu-id="467ee-119">Z menu po lewej stronie wybierz Szablony > Visual C#</span><span class="sxs-lookup"><span data-stu-id="467ee-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="467ee-120">Wybierz **biblioteki klas (.NET Standard)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="467ee-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="467ee-121">Nadaj nazwę projektu, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="467ee-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="467ee-122">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="467ee-122">Install Entity Framework</span></span>

<span data-ttu-id="467ee-123">Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="467ee-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="467ee-124">W tym przewodniku zastosowano SQLite.</span><span class="sxs-lookup"><span data-stu-id="467ee-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="467ee-125">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="467ee-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="467ee-126">Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="467ee-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="467ee-127">Uruchom`Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="467ee-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="467ee-128">W dalszej części tego przewodnika również użyjemy narzędzi Framework niektóre jednostki do obsługi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="467ee-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="467ee-129">Dlatego zostanie zainstalowany pakiet narzędzi również.</span><span class="sxs-lookup"><span data-stu-id="467ee-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="467ee-130">Uruchom`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="467ee-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="467ee-131">Przeprowadź edycję pliku .csproj i Zastąp `<TargetFramework>netstandard2.0</TargetFramework>` z`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="467ee-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="467ee-132">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="467ee-132">Create your model</span></span>

<span data-ttu-id="467ee-133">Teraz nadszedł czas do definiowania klas kontekstu i jednostek, wchodzące w skład modelu.</span><span class="sxs-lookup"><span data-stu-id="467ee-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="467ee-134">Projekt > Dodaj klasę...</span><span class="sxs-lookup"><span data-stu-id="467ee-134">Project > Add Class...</span></span>

* <span data-ttu-id="467ee-135">Wprowadź *Model.cs* jako nazwy i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="467ee-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="467ee-136">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="467ee-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="467ee-137">Utwórz nowy projekt platformy uniwersalnej systemu Windows</span><span class="sxs-lookup"><span data-stu-id="467ee-137">Create a new UWP project</span></span>

* <span data-ttu-id="467ee-138">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="467ee-138">Open Visual Studio</span></span>

* <span data-ttu-id="467ee-139">Plik > Nowy > Projekt...</span><span class="sxs-lookup"><span data-stu-id="467ee-139">File > New > Project...</span></span>

* <span data-ttu-id="467ee-140">Z menu po lewej stronie wybierz Szablony > Visual C# > uniwersalnych systemu Windows</span><span class="sxs-lookup"><span data-stu-id="467ee-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="467ee-141">Wybierz **pusta aplikacja (uniwersalna systemu Windows)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="467ee-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="467ee-142">Nadaj nazwę projektu, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="467ee-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="467ee-143">Ustaw docelowy i co najmniej wersji do co najmniej`Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="467ee-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="467ee-144">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="467ee-144">Create your database</span></span>

<span data-ttu-id="467ee-145">Teraz, gdy masz modelu można użyć migracji tworzenia bazy danych dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="467ee-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="467ee-146">Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="467ee-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="467ee-147">Wybierz projekt z modelem jako domyślny projekt i ustaw go jako projekt startowy</span><span class="sxs-lookup"><span data-stu-id="467ee-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="467ee-148">Uruchom `Add-Migration MyFirstMigration` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="467ee-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="467ee-149">Ponieważ chcemy bazy danych ma zostać utworzony na urządzeniu, która aplikacja jest uruchamiana na dodamy kodu, zastosuj wszelkie oczekujące migracje do lokalnej bazy danych podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="467ee-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="467ee-150">Aplikacja jest uruchamiana, po raz pierwszy to zajmie się tworzenie firmie Microsoft w lokalnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="467ee-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="467ee-151">Kliknij prawym przyciskiem myszy **App.xaml** w **Eksploratora rozwiązań** i wybierz **widoku kodu**</span><span class="sxs-lookup"><span data-stu-id="467ee-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="467ee-152">Dodaj wyróżnione using na początku pliku</span><span class="sxs-lookup"><span data-stu-id="467ee-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="467ee-153">Dodaj wyróżniony kod, aby Zastosuj wszelkie oczekujące migracje</span><span class="sxs-lookup"><span data-stu-id="467ee-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="467ee-154">Jeśli wprowadzisz zmiany w przyszłości do modelu, możesz użyć `Add-Migration` polecenie, aby utworzyć szkielet nowe migracji, aby zastosować odpowiednie zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="467ee-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="467ee-155">Wszelkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="467ee-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="467ee-156">Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="467ee-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="467ee-157">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="467ee-157">Use your model</span></span>

<span data-ttu-id="467ee-158">Można teraz używać modelu do uzyskania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="467ee-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="467ee-159">Otwórz *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="467ee-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="467ee-160">Dodawanie obsługi obciążenia strony i interfejsu użytkownika zawartości wskazanych poniżej</span><span class="sxs-lookup"><span data-stu-id="467ee-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="467ee-161">Teraz dodamy kod, aby okablować interfejsu użytkownika z bazy danych</span><span class="sxs-lookup"><span data-stu-id="467ee-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="467ee-162">Kliknij prawym przyciskiem myszy **MainPage.xaml** w **Eksploratora rozwiązań** i wybierz **widoku kodu**</span><span class="sxs-lookup"><span data-stu-id="467ee-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="467ee-163">Dodaj wyróżniony kod z poniższej listy</span><span class="sxs-lookup"><span data-stu-id="467ee-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="467ee-164">Można teraz uruchomić aplikację, aby zobaczyć ją w akcji.</span><span class="sxs-lookup"><span data-stu-id="467ee-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="467ee-165">Debuguj > Uruchom bez debugowania</span><span class="sxs-lookup"><span data-stu-id="467ee-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="467ee-166">Aplikacja tworzenie i uruchamianie</span><span class="sxs-lookup"><span data-stu-id="467ee-166">The application will build and launch</span></span>

* <span data-ttu-id="467ee-167">Wprowadź adres URL, a następnie kliknij przycisk **Dodaj** przycisku</span><span class="sxs-lookup"><span data-stu-id="467ee-167">Enter a URL and click the **Add** button</span></span>

![obraz](_static/create.png)

![obraz](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="467ee-170">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="467ee-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="467ee-171">`SaveChanges()`można poprawić wydajność dzięki implementacji [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) w Twojej typów jednostek i przy użyciu `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="467ee-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="467ee-172">Tada!</span><span class="sxs-lookup"><span data-stu-id="467ee-172">Tada!</span></span> <span data-ttu-id="467ee-173">Masz teraz prostej aplikacji platformy uniwersalnej systemu Windows z programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="467ee-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="467ee-174">Zapoznaj się z innymi artykułami w tej dokumentacji, aby dowiedzieć się więcej na temat funkcji programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="467ee-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
