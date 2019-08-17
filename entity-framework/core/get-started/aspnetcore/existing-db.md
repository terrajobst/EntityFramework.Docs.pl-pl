---
title: Wprowadzenie do ASP.NET Core — istniejąca baza danych — EF Core
author: rowanmiller
description: Wprowadzenie do EF Core na ASP.NET Core z istniejącą bazą danych
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565229"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="64cd0-103">Wprowadzenie do EF Core na ASP.NET Core z istniejącą bazą danych</span><span class="sxs-lookup"><span data-stu-id="64cd0-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="64cd0-104">W tym samouczku utworzysz aplikację ASP.NET Core MVC, która wykonuje podstawowy dostęp do danych przy użyciu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="64cd0-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="64cd0-105">Odtwarzanie istniejącej bazy danych w celu utworzenia modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="64cd0-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="64cd0-106">[Zapoznaj się z przykładem tego artykułu w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="64cd0-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64cd0-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="64cd0-107">Prerequisites</span></span>

<span data-ttu-id="64cd0-108">Zainstaluj następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="64cd0-108">Install the following software:</span></span>

* <span data-ttu-id="64cd0-109">[Program Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) z następującymi obciążeniami:</span><span class="sxs-lookup"><span data-stu-id="64cd0-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="64cd0-110">**ASP.NET i programowanie dla sieci Web** (w obszarze **chmura & sieci Web**)</span><span class="sxs-lookup"><span data-stu-id="64cd0-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="64cd0-111">**Programowanie dla wielu platform w środowisku .NET Core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="64cd0-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="64cd0-112">[Zestaw SDK platformy .NET Core 2,1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="64cd0-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="64cd0-113">Tworzenie bazy danych blogów</span><span class="sxs-lookup"><span data-stu-id="64cd0-113">Create Blogging database</span></span>

<span data-ttu-id="64cd0-114">W tym samouczku jest używany baza danych **blogów** w wystąpieniu LocalDB jako istniejąca baza danych.</span><span class="sxs-lookup"><span data-stu-id="64cd0-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="64cd0-115">Jeśli baza danych **blogów** została już utworzona w ramach innego samouczka, Pomiń te kroki.</span><span class="sxs-lookup"><span data-stu-id="64cd0-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="64cd0-116">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64cd0-116">Open Visual Studio</span></span>
* <span data-ttu-id="64cd0-117">**Narzędzia — > połączyć się z bazą danych...**</span><span class="sxs-lookup"><span data-stu-id="64cd0-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="64cd0-118">Wybierz **Microsoft SQL Server** i kliknij przycisk **Kontynuuj** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="64cd0-119">Wprowadź **(LocalDB) \mssqllocaldb** jako **nazwę serwera**</span><span class="sxs-lookup"><span data-stu-id="64cd0-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="64cd0-120">Wprowadź **wzorzec** jako **nazwę bazy danych** , a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="64cd0-121">Baza danych Master jest teraz wyświetlana w obszarze **połączenia danych** w **Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="64cd0-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="64cd0-122">Kliknij prawym przyciskiem myszy bazę danych w **Eksplorator serwera** a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="64cd0-123">Skopiuj skrypt wymieniony poniżej do edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="64cd0-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="64cd0-124">Kliknij prawym przyciskiem myszy Edytor zapytań i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="64cd0-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="64cd0-125">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="64cd0-125">Create a new project</span></span>

* <span data-ttu-id="64cd0-126">Open Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="64cd0-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="64cd0-127">**Plik > Nowy > projektu...**</span><span class="sxs-lookup"><span data-stu-id="64cd0-127">**File > New > Project...**</span></span>
* <span data-ttu-id="64cd0-128">Z menu po lewej stronie wybierz pozycję **zainstalowane C# > Visual > Web**</span><span class="sxs-lookup"><span data-stu-id="64cd0-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="64cd0-129">Wybierz szablon projektu **aplikacji sieci Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="64cd0-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="64cd0-130">Wprowadź **EFGetStarted. AspNetCore. ExistingDb** jako nazwę (musi dokładnie pasować do przestrzeni nazw, która została później użyta w kodzie), i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="64cd0-131">Zaczekaj na wyświetlenie okna dialogowego **Nowa aplikacja sieci Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="64cd0-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="64cd0-132">Upewnij się, że na liście rozwijanej platformy docelowej jest ustawiona wartość **.NET Core**, a na liście rozwijanej wersji jest ustawiona wartość **ASP.NET Core 2,1**</span><span class="sxs-lookup"><span data-stu-id="64cd0-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="64cd0-133">Wybierz szablon **aplikacja sieci Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="64cd0-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="64cd0-134">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="64cd0-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="64cd0-135">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="64cd0-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="64cd0-136">Zainstaluj Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="64cd0-136">Install Entity Framework Core</span></span>

<span data-ttu-id="64cd0-137">Aby zainstalować EF Core, należy zainstalować pakiet dla dostawców usługi EF Core Database, które mają być przeznaczone do celów.</span><span class="sxs-lookup"><span data-stu-id="64cd0-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="64cd0-138">Aby uzyskać listę dostępnych dostawców, zobacz [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="64cd0-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="64cd0-139">W tym samouczku nie trzeba instalować pakietu dostawcy, ponieważ samouczek używa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64cd0-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="64cd0-140">Pakiet dostawcy SQL Server jest zawarty w [pakiecie Microsoft. AspnetCore. app](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="64cd0-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="64cd0-141">Odtwarzanie modelu</span><span class="sxs-lookup"><span data-stu-id="64cd0-141">Reverse engineer your model</span></span>

<span data-ttu-id="64cd0-142">Teraz można utworzyć model EF na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="64cd0-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="64cd0-143">**Narzędzia — > Menedżer pakietów NuGet — > konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="64cd0-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="64cd0-144">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="64cd0-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="64cd0-145">Jeśli zostanie wyświetlony komunikat o błędzie z `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`informacją, Zamknij i ponownie otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64cd0-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="64cd0-146">Możesz określić, które tabele mają być generowane przez dodanie `-Tables` argumentu do powyższego polecenia.</span><span class="sxs-lookup"><span data-stu-id="64cd0-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="64cd0-147">Na przykład `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="64cd0-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="64cd0-148">Proces odtwarzania`Blog.cs`utworzył klasy jednostek ( & `Post.cs`) i kontekst pochodny (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="64cd0-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="64cd0-149">Klasy jednostek są prostymi C# obiektami, które reprezentują dane, które będą używane do wykonywania zapytań i zapisywania.</span><span class="sxs-lookup"><span data-stu-id="64cd0-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="64cd0-150">Oto klasy jednostek `Post`i: `Blog`</span><span class="sxs-lookup"><span data-stu-id="64cd0-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="64cd0-151">Aby włączyć ładowanie z opóźnieniem, możesz tworzyć właściwości `virtual` nawigacji (blog.post i post).</span><span class="sxs-lookup"><span data-stu-id="64cd0-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="64cd0-152">Kontekst reprezentuje sesję z bazą danych i umożliwia wykonywanie zapytań i zapisywanie wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="64cd0-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="64cd0-153">Rejestrowanie kontekstu z iniekcją zależności</span><span class="sxs-lookup"><span data-stu-id="64cd0-153">Register your context with dependency injection</span></span>

<span data-ttu-id="64cd0-154">Pojęcie iniekcji zależności jest środkowe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64cd0-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="64cd0-155">Usługi (takie jak `BloggingContext`) są rejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64cd0-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="64cd0-156">Składniki, które wymagają tych usług (takich jak kontrolery MVC), są następnie udostępniane przez parametry lub właściwości konstruktora.</span><span class="sxs-lookup"><span data-stu-id="64cd0-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="64cd0-157">Aby uzyskać więcej informacji na temat iniekcji zależności, zobacz artykuł [iniekcja zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) w witrynie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="64cd0-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="64cd0-158">Rejestrowanie i Konfigurowanie kontekstu w programie Startup.cs</span><span class="sxs-lookup"><span data-stu-id="64cd0-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="64cd0-159">Aby udostępnić `BloggingContext` kontrolery MVC, zarejestruj je jako usługę.</span><span class="sxs-lookup"><span data-stu-id="64cd0-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="64cd0-160">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="64cd0-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="64cd0-161">Dodaj następujące `using` instrukcje na początku pliku</span><span class="sxs-lookup"><span data-stu-id="64cd0-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="64cd0-162">Teraz możesz użyć `AddDbContext(...)` metody, aby zarejestrować ją jako usługę.</span><span class="sxs-lookup"><span data-stu-id="64cd0-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="64cd0-163">`ConfigureServices(...)` Znajdowanie metody</span><span class="sxs-lookup"><span data-stu-id="64cd0-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="64cd0-164">Dodaj następujący wyróżniony kod, aby zarejestrować kontekst jako usługę</span><span class="sxs-lookup"><span data-stu-id="64cd0-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="64cd0-165">W prawdziwej aplikacji zwykle umieszcza się parametry połączenia w pliku konfiguracyjnym lub zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="64cd0-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="64cd0-166">Dla uproszczenia ten samouczek został zdefiniowany w kodzie.</span><span class="sxs-lookup"><span data-stu-id="64cd0-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="64cd0-167">Aby uzyskać więcej informacji, zobacz [Parametry połączenia](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="64cd0-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="64cd0-168">Tworzenie kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="64cd0-168">Create a controller and views</span></span>

* <span data-ttu-id="64cd0-169">Kliknij prawym przyciskiem myszy folder controllers w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj > kontroler...**</span><span class="sxs-lookup"><span data-stu-id="64cd0-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="64cd0-170">Wybierz **kontroler MVC z widokami przy użyciu Entity Framework** i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="64cd0-171">Ustaw **klasę modelu** na **blog** i **klasę kontekstu danych** na **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="64cd0-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="64cd0-172">Kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="64cd0-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="64cd0-173">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="64cd0-173">Run the application</span></span>

<span data-ttu-id="64cd0-174">Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.</span><span class="sxs-lookup"><span data-stu-id="64cd0-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="64cd0-175">**Debugowanie — > Uruchom bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="64cd0-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="64cd0-176">Aplikacja zostanie skompilowana i otwarta w przeglądarce internetowej</span><span class="sxs-lookup"><span data-stu-id="64cd0-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="64cd0-177">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="64cd0-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="64cd0-178">Kliknij przycisk **Utwórz nowy**</span><span class="sxs-lookup"><span data-stu-id="64cd0-178">Click **Create New**</span></span>
* <span data-ttu-id="64cd0-179">Wprowadź **adres URL** nowego bloga i kliknij pozycję **Utwórz** .</span><span class="sxs-lookup"><span data-stu-id="64cd0-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Tworzenie strony](_static/create.png)

  ![Strona indeksu](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="64cd0-182">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="64cd0-182">Next steps</span></span>

<span data-ttu-id="64cd0-183">Aby uzyskać więcej informacji na temat tworzenia szkieletu klas kontekstu i jednostki, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="64cd0-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="64cd0-184">Odwrócenie inżynierii</span><span class="sxs-lookup"><span data-stu-id="64cd0-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="64cd0-185">Dokumentacja narzędzi Entity Framework Core Tools — interfejs wiersza polecenia platformy .NET</span><span class="sxs-lookup"><span data-stu-id="64cd0-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="64cd0-186">Dokumentacja narzędzi Entity Framework Core Tools — konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="64cd0-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
