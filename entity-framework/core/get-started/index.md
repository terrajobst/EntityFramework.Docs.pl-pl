---
title: Wprowadzenie — EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: d46c4bb9ac6c8f718b4da5ecd82d54710d41935f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824492"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="b07b3-102">Wprowadzenie z EF Core</span><span class="sxs-lookup"><span data-stu-id="b07b3-102">Getting Started with EF Core</span></span>

<span data-ttu-id="b07b3-103">W tym samouczku utworzysz aplikację konsolową .NET Core, która zapewnia dostęp do danych w bazie danych programu SQLite przy użyciu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b07b3-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="b07b3-104">Możesz wykonać czynności opisane w samouczku za pomocą programu Visual Studio w systemie Windows lub za pomocą interfejs wiersza polecenia platformy .NET Core w systemie Windows, macOS lub Linux.</span><span class="sxs-lookup"><span data-stu-id="b07b3-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="b07b3-105">[Zapoznaj się z przykładem tego artykułu w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="b07b3-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b07b3-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b07b3-106">Prerequisites</span></span>

<span data-ttu-id="b07b3-107">Zainstaluj następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="b07b3-107">Install the following software:</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b07b3-109">[Zestaw SDK platformy .NET Core 3,0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="b07b3-109">[.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b07b3-111">[Program Visual Studio 2019 w wersji 16,3 lub nowszej](https://www.visualstudio.com/downloads/) z tym obciążeniem:</span><span class="sxs-lookup"><span data-stu-id="b07b3-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="b07b3-112">**Programowanie dla wielu platform w środowisku .NET Core** (w innych zestawach **narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="b07b3-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="b07b3-113">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="b07b3-113">Create a new project</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b07b3-116">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b07b3-116">Open Visual Studio</span></span>
* <span data-ttu-id="b07b3-117">Kliknij pozycję **Utwórz nowy projekt**</span><span class="sxs-lookup"><span data-stu-id="b07b3-117">Click **Create a new project**</span></span>
* <span data-ttu-id="b07b3-118">Wybierz pozycję **aplikacja konsoli (.NET Core)** z **C#** tagiem i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="b07b3-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="b07b3-119">Wprowadź **EFGetStarted** dla nazwy i kliknij przycisk **Utwórz** .</span><span class="sxs-lookup"><span data-stu-id="b07b3-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="b07b3-120">Zainstaluj Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b07b3-120">Install Entity Framework Core</span></span>

<span data-ttu-id="b07b3-121">Aby zainstalować EF Core, należy zainstalować pakiet dla dostawców usługi EF Core Database, które mają być przeznaczone do celów.</span><span class="sxs-lookup"><span data-stu-id="b07b3-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="b07b3-122">W tym samouczku jest używane oprogramowanie SQLite, ponieważ jest ono wykonywane na wszystkich platformach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b07b3-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="b07b3-123">Aby uzyskać listę dostępnych dostawców, zobacz [dostawcy bazy danych](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b07b3-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b07b3-126">**Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="b07b3-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="b07b3-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b07b3-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="b07b3-128">Porada: Możesz także zainstalować pakiety, klikając prawym przyciskiem myszy projekt i wybierając pozycję **Zarządzaj pakietami NuGet** .</span><span class="sxs-lookup"><span data-stu-id="b07b3-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="b07b3-129">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="b07b3-129">Create the model</span></span>

<span data-ttu-id="b07b3-130">Zdefiniuj klasę kontekstu i klasy jednostek, które tworzą model.</span><span class="sxs-lookup"><span data-stu-id="b07b3-130">Define a context class and entity classes that make up the model.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b07b3-132">W katalogu projektu Utwórz **model.cs** z następującym kodem</span><span class="sxs-lookup"><span data-stu-id="b07b3-132">In the project directory, create **Model.cs** with the following code</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b07b3-134">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasy >**</span><span class="sxs-lookup"><span data-stu-id="b07b3-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="b07b3-135">Wprowadź **model.cs** jako nazwę, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="b07b3-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="b07b3-136">Zastąp zawartość pliku następującym kodem</span><span class="sxs-lookup"><span data-stu-id="b07b3-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="b07b3-137">EF Core [może również odtworzyć](../managing-schemas/scaffolding.md) model z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b07b3-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="b07b3-138">Porada: w rzeczywistej aplikacji należy umieścić każdą klasę w osobnym pliku i umieścić [Parametry połączenia](../miscellaneous/connection-strings.md) w pliku konfiguracyjnym lub zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="b07b3-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="b07b3-139">Aby zachować ten samouczek, wszystko jest zawarte w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="b07b3-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="b07b3-140">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="b07b3-140">Create the database</span></span>

<span data-ttu-id="b07b3-141">Poniższe kroki służą do tworzenia bazy [danych programu.](xref:core/managing-schemas/migrations/index)</span><span class="sxs-lookup"><span data-stu-id="b07b3-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b07b3-143">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b07b3-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="b07b3-144">Spowoduje to zainstalowanie programu [dotnet EF](../miscellaneous/cli/dotnet.md) i pakietu projektowego, który jest wymagany do uruchomienia polecenia w projekcie.</span><span class="sxs-lookup"><span data-stu-id="b07b3-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="b07b3-145">Polecenie `migrations` tworzy szkielet migracji w celu utworzenia początkowego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="b07b3-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="b07b3-146">`database update` polecenie tworzy bazę danych i stosuje do niej nową migrację.</span><span class="sxs-lookup"><span data-stu-id="b07b3-146">The `database update` command creates the database and applies the new migration to it.</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b07b3-148">Uruchom następujące polecenia w **konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="b07b3-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="b07b3-149">Spowoduje to zainstalowanie [narzędzi PMC dla EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b07b3-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="b07b3-150">Polecenie `Add-Migration` tworzy szkielet migracji w celu utworzenia początkowego zestawu tabel dla modelu.</span><span class="sxs-lookup"><span data-stu-id="b07b3-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="b07b3-151">`Update-Database` polecenie tworzy bazę danych i stosuje do niej nową migrację.</span><span class="sxs-lookup"><span data-stu-id="b07b3-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="b07b3-152">Tworzenie, odczytywanie, aktualizowanie & Usuwanie</span><span class="sxs-lookup"><span data-stu-id="b07b3-152">Create, read, update & delete</span></span>

* <span data-ttu-id="b07b3-153">Otwórz *program.cs* i Zastąp zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b07b3-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="b07b3-154">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b07b3-154">Run the app</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b07b3-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b07b3-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b07b3-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b07b3-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b07b3-157">Program Visual Studio używa niespójnego katalogu roboczego podczas uruchamiania aplikacji konsolowych platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b07b3-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="b07b3-158">(zobacz [dotnet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Powoduje to zgłaszanie wyjątku: *nie ma takiej tabeli: blogi*.</span><span class="sxs-lookup"><span data-stu-id="b07b3-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="b07b3-159">Aby zaktualizować katalog roboczy:</span><span class="sxs-lookup"><span data-stu-id="b07b3-159">To update the working directory:</span></span>

* <span data-ttu-id="b07b3-160">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**</span><span class="sxs-lookup"><span data-stu-id="b07b3-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="b07b3-161">Po prostu poniżej właściwości *TargetFramework* Dodaj następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="b07b3-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="b07b3-162">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="b07b3-162">Save the file</span></span>

<span data-ttu-id="b07b3-163">Teraz możesz uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="b07b3-163">Now you can run the app:</span></span>

* <span data-ttu-id="b07b3-164">**Debugowanie > uruchamiane bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="b07b3-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="b07b3-165">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b07b3-165">Next steps</span></span>

* <span data-ttu-id="b07b3-166">Postępuj zgodnie z [samouczkiem ASP.NET Core](/aspnet/core/data/ef-rp/intro) , aby użyć EF Core w aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="b07b3-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="b07b3-167">Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="b07b3-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="b07b3-168">[Skonfiguruj model](xref:core/modeling/index) , aby określić elementy, takie jak [wymagana](xref:core/modeling/required-optional) i [Maksymalna długość](xref:core/modeling/max-length)</span><span class="sxs-lookup"><span data-stu-id="b07b3-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/required-optional) and [maximum length](xref:core/modeling/max-length)</span></span>
* <span data-ttu-id="b07b3-169">Użyj [migracji](xref:core/managing-schemas/migrations/index) , aby zaktualizować schemat bazy danych po zmianie modelu</span><span class="sxs-lookup"><span data-stu-id="b07b3-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
