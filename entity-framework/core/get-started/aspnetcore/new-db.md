---
title: "Wprowadzenie do platformy ASP.NET Core - nowej bazy danych — EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: f6ed19d3c5d2ae8d1f5756558e50c1f0dddd2f07
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="c246a-102">Wprowadzenie do podstawowych EF na platformy ASP.NET Core nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="c246a-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="c246a-103">W tym przykładzie utworzysz aplikacji platformy ASP.NET Core MVC, który wykonuje dostęp do podstawowych danych przy użyciu programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c246a-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="c246a-104">Migracje użyje do utworzenia bazy danych z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="c246a-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="c246a-105">Zobacz [dodatkowe zasoby](#additional-resources) dla więcej samouczków Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c246a-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="c246a-106">Ten samouczek wymaga:</span><span class="sxs-lookup"><span data-stu-id="c246a-106">This tutorial requires:</span></span>
* <span data-ttu-id="c246a-107">[15 ustęp 3 programu Visual Studio 2017](https://www.visualstudio.com/downloads/) z te obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c246a-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="c246a-108">**ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)</span><span class="sxs-lookup"><span data-stu-id="c246a-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="c246a-109">**Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)</span><span class="sxs-lookup"><span data-stu-id="c246a-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="c246a-110">[.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c246a-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="c246a-111">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c246a-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="c246a-112">Utwórz nowy projekt w programie Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="c246a-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="c246a-113">**Plik > Nowy > Projekt**</span><span class="sxs-lookup"><span data-stu-id="c246a-113">**File > New > Project**</span></span>
* <span data-ttu-id="c246a-114">Wybierz z menu po lewej stronie **zainstalowana > Szablony > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c246a-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="c246a-115">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c246a-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c246a-116">Wprowadź **EFGetStarted.AspNetCore.NewDb** dla nazwy i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c246a-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="c246a-117">W **nową aplikację sieci Web Core ASP.NET** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="c246a-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="c246a-118">Upewnij się, opcje **.NET Core** i **ASP.NET Core 2.0** są wybrane na liście rozwijanej listy</span><span class="sxs-lookup"><span data-stu-id="c246a-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="c246a-119">Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="c246a-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="c246a-120">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="c246a-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="c246a-121">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="c246a-121">Click **OK**</span></span>

<span data-ttu-id="c246a-122">Ostrzeżenie: Jeśli używasz **indywidualnych kont użytkowników** zamiast **Brak** dla **uwierzytelniania** , a następnie modelu Entity Framework Core zostanie dodany do projektu w `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="c246a-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="c246a-123">Korzystanie z metod, które przedstawiono w ramach tego przewodnika, można dodać drugiego modelu lub rozszerzyć istniejącego modelu zawiera klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="c246a-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="c246a-124">Zainstaluj program Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c246a-124">Install Entity Framework Core</span></span>

<span data-ttu-id="c246a-125">Zainstaluj pakiet dla EF Core powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="c246a-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="c246a-126">W tym przewodniku zastosowano programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c246a-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="c246a-127">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c246a-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c246a-128">**Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="c246a-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c246a-129">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="c246a-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c246a-130">Użyjemy niektóre narzędzia Entity Framework Core utworzyć bazę danych z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="c246a-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="c246a-131">Dlatego zostanie zainstalowany pakiet narzędzi również:</span><span class="sxs-lookup"><span data-stu-id="c246a-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="c246a-132">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="c246a-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="c246a-133">Użyjemy niektóre platformy ASP.NET Core szkieletów narzędzia do tworzenia widoków i kontrolerów później.</span><span class="sxs-lookup"><span data-stu-id="c246a-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="c246a-134">Dlatego zostanie zainstalowany ten pakiet projektu:</span><span class="sxs-lookup"><span data-stu-id="c246a-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="c246a-135">Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="c246a-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c246a-136">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="c246a-136">Create the model</span></span>

<span data-ttu-id="c246a-137">Definiowanie kontekstu i jednostki klasy, które tworzą modelu:</span><span class="sxs-lookup"><span data-stu-id="c246a-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="c246a-138">Kliknij prawym przyciskiem myszy **modele** i wybierz polecenie **Dodaj > klasy**.</span><span class="sxs-lookup"><span data-stu-id="c246a-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="c246a-139">Wprowadź **Model.cs** jako nazwy i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c246a-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="c246a-140">Zastąp zawartość pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c246a-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="c246a-141">Uwaga: W rzeczywistej aplikacji zwykle przełączyć każdej klasy z modelu w oddzielnym pliku.</span><span class="sxs-lookup"><span data-stu-id="c246a-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="c246a-142">Dla uproszczenia wprowadzamy wszystkie klasy w jednym pliku w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="c246a-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="c246a-143">Zarejestruj kontekst iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="c246a-143">Register your context with dependency injection</span></span>

<span data-ttu-id="c246a-144">Usługi (takie jak `BloggingContext`) są zarejestrowane w usłudze [iniekcji zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c246a-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="c246a-145">Składniki, które wymagają tych usług (takich jak kontrolerów MVC) są następnie udostępniane tych usług za pomocą konstruktora parametry lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="c246a-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="c246a-146">Aby naszych kontrolerów MVC korzystać z `BloggingContext` firma Microsoft będzie rejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="c246a-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="c246a-147">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="c246a-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="c246a-148">Dodaj następujące `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="c246a-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="c246a-149">Dodaj `AddDbContext` metody, aby zarejestrować go w trybie usługi:</span><span class="sxs-lookup"><span data-stu-id="c246a-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="c246a-150">Dodaj następujący kod do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="c246a-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="c246a-151">Uwaga: Rzeczywiste aplikacji zwykle spowodowałaby parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c246a-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="c246a-152">Dla uproszczenia możemy są definiowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c246a-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="c246a-153">Zobacz [parametry połączenia](../../miscellaneous/connection-strings.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c246a-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="c246a-154">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="c246a-154">Create your database</span></span>

<span data-ttu-id="c246a-155">Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c246a-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="c246a-156">Otwórz PMC:</span><span class="sxs-lookup"><span data-stu-id="c246a-156">Open the PMC:</span></span>

  <span data-ttu-id="c246a-157">**Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="c246a-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="c246a-158">Uruchom `Add-Migration InitialCreate` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="c246a-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="c246a-159">Jeśli zostanie wyświetlony błąd informujący o `The term 'add-migration' is not recognized as the name of a cmdlet`, zamknij i otwórz ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c246a-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="c246a-160">Uruchom `Update-Database` na zastosowanie nowych migracji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c246a-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c246a-161">To polecenie tworzy bazy danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="c246a-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="c246a-162">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="c246a-162">Create a controller</span></span>

<span data-ttu-id="c246a-163">Aktywują szkielet do projektu:</span><span class="sxs-lookup"><span data-stu-id="c246a-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="c246a-164">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="c246a-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="c246a-165">Wybierz **minimalnego zależności** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c246a-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="c246a-166">Można zignorować lub usunąć *ScaffoldingReadMe.txt* pliku.</span><span class="sxs-lookup"><span data-stu-id="c246a-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="c246a-167">Teraz, gdy jest włączona funkcja szkieletów, możemy utworzyć szkielet kontrolera dla `Blog` jednostki.</span><span class="sxs-lookup"><span data-stu-id="c246a-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="c246a-168">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="c246a-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="c246a-169">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**.</span><span class="sxs-lookup"><span data-stu-id="c246a-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="c246a-170">Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="c246a-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="c246a-171">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c246a-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="c246a-172">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c246a-172">Run the application</span></span>

<span data-ttu-id="c246a-173">Naciśnij klawisz F5, aby uruchomić i przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="c246a-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="c246a-174">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="c246a-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="c246a-175">Utwórz łącze umożliwia utworzenie niektórych wpisów.</span><span class="sxs-lookup"><span data-stu-id="c246a-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="c246a-176">Testowanie szczegóły i usunąć łącza.</span><span class="sxs-lookup"><span data-stu-id="c246a-176">Test the details and delete links.</span></span>

![obraz](_static/create.png)

![obraz](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c246a-179">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c246a-179">Additional Resources</span></span>

* <span data-ttu-id="c246a-180">[EF - nowej bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek EF konsoli i platform.</span><span class="sxs-lookup"><span data-stu-id="c246a-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="c246a-181">Wprowadzenie do platformy ASP.NET Core MVC Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="c246a-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="c246a-182">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c246a-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="c246a-183">Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c246a-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
