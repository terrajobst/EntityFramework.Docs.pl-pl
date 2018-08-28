---
title: Wprowadzenie do platformy ASP.NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996067"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="83de5-102">Wprowadzenie do programu EF Core programu ASP.NET Core przy użyciu nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="83de5-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="83de5-103">W tym samouczku utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane dostępu przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="83de5-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="83de5-104">Migracje umożliwia tworzenie bazy danych z modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="83de5-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="83de5-105">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="83de5-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83de5-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="83de5-106">Prerequisites</span></span>

<span data-ttu-id="83de5-107">Zainstaluj następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="83de5-107">Install the following software:</span></span>

* <span data-ttu-id="83de5-108">[Visual Studio 2017 w wersji 15.7](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:</span><span class="sxs-lookup"><span data-stu-id="83de5-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="83de5-109">**ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)</span><span class="sxs-lookup"><span data-stu-id="83de5-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="83de5-110">**Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="83de5-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="83de5-111">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="83de5-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="83de5-112">Utwórz nowy projekt w programie Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="83de5-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="83de5-113">Otwórz program Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="83de5-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="83de5-114">**Plik > Nowy > Projekt**</span><span class="sxs-lookup"><span data-stu-id="83de5-114">**File > New > Project**</span></span>
* <span data-ttu-id="83de5-115">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="83de5-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="83de5-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="83de5-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="83de5-117">Wprowadź **EFGetStarted.AspNetCore.NewDb** nazwę i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="83de5-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="83de5-118">W **Nowa aplikacja internetowa ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="83de5-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="83de5-119">Upewnij się, opcje **platformy .NET Core** i **platformy ASP.NET Core 2.1** wybranych z list rozwijanych</span><span class="sxs-lookup"><span data-stu-id="83de5-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="83de5-120">Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="83de5-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="83de5-121">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="83de5-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="83de5-122">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="83de5-122">Click **OK**</span></span>

<span data-ttu-id="83de5-123">Ostrzeżenie: Jeśli używasz **indywidualne konta użytkowników** zamiast **Brak** dla **uwierzytelniania** , a następnie na model Entity Framework Core zostanie dodany do projektu w `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="83de5-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="83de5-124">Przy użyciu technik, z którego dowiesz się, w tym samouczku, można dodać drugi model lub rozszerzyć ten istniejący model zawiera Twoje klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="83de5-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="83de5-125">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="83de5-125">Install Entity Framework Core</span></span>

<span data-ttu-id="83de5-126">Aby zainstalować programu EF Core, należy zainstalować pakiet dla dostawców bazy danych programu EF Core, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="83de5-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="83de5-127">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="83de5-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="83de5-128">Na potrzeby tego samouczka nie trzeba zainstalować pakiet dostawcy, ponieważ w tym samouczku użyto programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="83de5-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="83de5-129">Pakiet dostawcy programu SQL Server znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="83de5-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="83de5-130">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="83de5-130">Create the model</span></span>

<span data-ttu-id="83de5-131">Zdefiniuj klasę kontekstu i klas jednostek, które tworzą model:</span><span class="sxs-lookup"><span data-stu-id="83de5-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="83de5-132">Kliknij prawym przyciskiem myszy **modeli** i wybierz polecenie **Dodaj > klasa**.</span><span class="sxs-lookup"><span data-stu-id="83de5-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="83de5-133">Wprowadź **Model.cs** jako nazwę i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="83de5-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="83de5-134">Zastąp zawartość pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="83de5-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="83de5-135">W rzeczywistych aplikacjach przeważnie przełączyć każdej klasy z modelu w oddzielnym pliku.</span><span class="sxs-lookup"><span data-stu-id="83de5-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="83de5-136">Dla uproszczenia ten samouczek umieszcza wszystkie klasy w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="83de5-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="83de5-137">Zarejestrowanie kontekst wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="83de5-137">Register your context with dependency injection</span></span>

<span data-ttu-id="83de5-138">Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83de5-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="83de5-139">Składniki, które wymagają tych usług, (na przykład kontrolerach MVC) znajdują się następnie tych usług za pomocą właściwości lub parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="83de5-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="83de5-140">Zapewnienie `BloggingContext` dostępne dla kontrolerów MVC, zarejestruj go jako usługę.</span><span class="sxs-lookup"><span data-stu-id="83de5-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="83de5-141">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="83de5-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="83de5-142">Dodaj następujący kod `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="83de5-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="83de5-143">Wywołaj `AddDbContext` metodę, aby zarejestrować kontekst jako usługa.</span><span class="sxs-lookup"><span data-stu-id="83de5-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="83de5-144">Dodaj następujący wyróżniony kod do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="83de5-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="83de5-145">Uwaga: Rzeczywistej aplikacji ogólnie umieścić ciąg połączenia w zmiennej środowisku lub pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83de5-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="83de5-146">Dla uproszczenia w tym samouczku zdefiniowano go w kodzie.</span><span class="sxs-lookup"><span data-stu-id="83de5-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="83de5-147">Zobacz [parametry połączenia](../../miscellaneous/connection-strings.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="83de5-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="83de5-148">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="83de5-148">Create the database</span></span>

<span data-ttu-id="83de5-149">Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="83de5-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="83de5-150">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="83de5-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="83de5-151">Uruchom `Add-Migration InitialCreate` do tworzenia szkieletu migracji, aby utworzyć początkowy zestaw tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="83de5-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="83de5-152">Jeśli zostanie wyświetlony błąd wskazujący `The term 'add-migration' is not recognized as the name of a cmdlet`, zamknij i otwórz ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83de5-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="83de5-153">Uruchom `Update-Database` zastosować nową migrację do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="83de5-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="83de5-154">To polecenie tworzy bazę danych przed zastosowaniem migracji.</span><span class="sxs-lookup"><span data-stu-id="83de5-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="83de5-155">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="83de5-155">Create a controller</span></span>

<span data-ttu-id="83de5-156">Tworzenia szkieletu kontrolera i widoki dla `Blog` jednostki.</span><span class="sxs-lookup"><span data-stu-id="83de5-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="83de5-157">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="83de5-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="83de5-158">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="83de5-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="83de5-159">Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="83de5-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="83de5-160">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="83de5-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="83de5-161">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="83de5-161">Run the application</span></span>

<span data-ttu-id="83de5-162">Naciśnij klawisz F5, aby uruchomić i przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="83de5-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="83de5-163">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="83de5-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="83de5-164">Użyj linku, aby utworzyć wpisy na blogu.</span><span class="sxs-lookup"><span data-stu-id="83de5-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="83de5-165">Szczegóły testu, a następnie usuń linki.</span><span class="sxs-lookup"><span data-stu-id="83de5-165">Test the details and delete links.</span></span>

![obraz](_static/create.png)

![obraz](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="83de5-168">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="83de5-168">Additional Resources</span></span>

* <span data-ttu-id="83de5-169">[EF - nowej bazy danych za pomocą SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="83de5-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="83de5-170">Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux</span><span class="sxs-lookup"><span data-stu-id="83de5-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="83de5-171">Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83de5-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="83de5-172">Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83de5-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
