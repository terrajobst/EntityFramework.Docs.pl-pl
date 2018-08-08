---
title: Wprowadzenie do platformy .NET Framework — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614418"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="1223e-102">Wprowadzenie do programu EF Core w programie .NET Framework przy użyciu istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="1223e-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="1223e-103">W tym samouczku utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1223e-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="1223e-104">Aby utworzyć model Entity Framework odtwarzanie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="1223e-105">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="1223e-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1223e-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1223e-106">Prerequisites</span></span>

* [<span data-ttu-id="1223e-107">Visual Studio 2017 w wersji 15.7 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1223e-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="1223e-108">Utwórz bazę danych do obsługi blogów</span><span class="sxs-lookup"><span data-stu-id="1223e-108">Create Blogging database</span></span>

<span data-ttu-id="1223e-109">W tym samouczku **do obsługi blogów** bazy danych w wystąpieniu LocalDb jako istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="1223e-110">Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innego samouczek, należy pominąć tę procedurę.</span><span class="sxs-lookup"><span data-stu-id="1223e-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="1223e-111">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1223e-111">Open Visual Studio</span></span>

* <span data-ttu-id="1223e-112">**Narzędzia > nawiązać połączenie z bazą danych...**</span><span class="sxs-lookup"><span data-stu-id="1223e-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="1223e-113">Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**</span><span class="sxs-lookup"><span data-stu-id="1223e-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="1223e-114">Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**</span><span class="sxs-lookup"><span data-stu-id="1223e-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="1223e-115">Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="1223e-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="1223e-116">Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="1223e-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="1223e-117">Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="1223e-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="1223e-118">Skopiuj skrypt w edytorze zapytań</span><span class="sxs-lookup"><span data-stu-id="1223e-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="1223e-119">Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="1223e-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="1223e-120">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="1223e-120">Create a new project</span></span>

* <span data-ttu-id="1223e-121">Otwórz program Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1223e-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="1223e-122">**Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="1223e-122">**File > New > Project...**</span></span>

* <span data-ttu-id="1223e-123">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Desktop**</span><span class="sxs-lookup"><span data-stu-id="1223e-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="1223e-124">Wybierz **Aplikacja konsoli (.NET Framework)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="1223e-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="1223e-125">Upewnij się, że projekt jest ukierunkowany **platformy .NET Framework 4.6.1** lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1223e-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="1223e-126">Nadaj projektowi nazwę *ConsoleApp.ExistingDb* i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="1223e-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="1223e-127">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1223e-127">Install Entity Framework</span></span>

<span data-ttu-id="1223e-128">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="1223e-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="1223e-129">Ten samouczek używa programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1223e-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="1223e-130">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1223e-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="1223e-131">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="1223e-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="1223e-132">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="1223e-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="1223e-133">W następnym kroku użyjesz niektórych narzędzi Entity Framework Tools do odtworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="1223e-134">Więc Zainstaluj również pakiet narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1223e-134">So install the tools package as well.</span></span>

* <span data-ttu-id="1223e-135">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="1223e-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="1223e-136">Odtwarzanie modelu</span><span class="sxs-lookup"><span data-stu-id="1223e-136">Reverse engineer the model</span></span>

<span data-ttu-id="1223e-137">Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="1223e-138">**Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="1223e-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="1223e-139">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="1223e-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="1223e-140">Można określić tabel w celu wygenerowania jednostki dla, dodając `-Tables` argument polecenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="1223e-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="1223e-141">Na przykład `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="1223e-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="1223e-142">Proces odtwarzania utworzony klas jednostek (`Blog` i `Post`) i pochodnej kontekstu (`BloggingContext`) na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="1223e-143">Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="1223e-144">Poniżej przedstawiono `Blog` i `Post` klas jednostek:</span><span class="sxs-lookup"><span data-stu-id="1223e-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="1223e-145">Aby włączyć ładowanie z opóźnieniem, należy wybrać właściwości nawigacji `virtual` (Blog.Post i Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="1223e-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="1223e-146">Kontekst reprezentuje sesję z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-146">The context represents a session with the database.</span></span> <span data-ttu-id="1223e-147">Zawiera metody, które służy do wykonywania zapytań i Zapisz wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="1223e-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="1223e-148">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="1223e-148">Use the model</span></span>

<span data-ttu-id="1223e-149">Można teraz używać modelu przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="1223e-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="1223e-150">Otwórz *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="1223e-150">Open *Program.cs*</span></span>

* <span data-ttu-id="1223e-151">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="1223e-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="1223e-152">Debuguj > Uruchom bez debugowania</span><span class="sxs-lookup"><span data-stu-id="1223e-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="1223e-153">Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="1223e-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![obraz](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="1223e-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1223e-155">Additional Resources</span></span>

* [<span data-ttu-id="1223e-156">EF Core w programie .NET Framework za pomocą nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="1223e-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="1223e-157">[EF Core na platformie .NET Core za pomocą nowej bazy danych — bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="1223e-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
