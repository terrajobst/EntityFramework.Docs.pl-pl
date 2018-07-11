---
title: Wprowadzenie do platformy ASP.NET Core — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949156"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="b6a1b-102">Wprowadzenie do programu EF Core programu ASP.NET Core z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="b6a1b-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="b6a1b-103">W tym instruktażu utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane access używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="b6a1b-104">Odtwarzanie użyje do tworzenia modelu Entity Framework, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="b6a1b-105">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6a1b-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6a1b-106">Prerequisites</span></span>

<span data-ttu-id="b6a1b-107">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="b6a1b-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="b6a1b-108">[Program Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:</span><span class="sxs-lookup"><span data-stu-id="b6a1b-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="b6a1b-109">**ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)</span><span class="sxs-lookup"><span data-stu-id="b6a1b-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="b6a1b-110">**Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="b6a1b-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="b6a1b-111">[.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="b6a1b-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="b6a1b-112">Do obsługi blogów bazy danych</span><span class="sxs-lookup"><span data-stu-id="b6a1b-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="b6a1b-113">Do obsługi blogów bazy danych</span><span class="sxs-lookup"><span data-stu-id="b6a1b-113">Blogging database</span></span>

<span data-ttu-id="b6a1b-114">W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="b6a1b-115">Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innym samouczku, można pominąć te kroki.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="b6a1b-116">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6a1b-116">Open Visual Studio</span></span>
* <span data-ttu-id="b6a1b-117">**Narzędzia -> nawiązać połączenie z bazą danych...**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="b6a1b-118">Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="b6a1b-119">Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="b6a1b-120">Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="b6a1b-121">Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="b6a1b-122">Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="b6a1b-123">Skopiuj skrypt wymienione poniżej pod warunkiem do edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="b6a1b-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="b6a1b-124">Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="b6a1b-125">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="b6a1b-125">Create a new project</span></span>

* <span data-ttu-id="b6a1b-126">Otwórz program Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b6a1b-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="b6a1b-127">**Plik -> Nowy -> Projekt...**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="b6a1b-128">Z menu po lewej stronie wybierz **zainstalowane -> Szablony -> Visual C# -> Sieć Web**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="b6a1b-129">Wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="b6a1b-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="b6a1b-130">Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="b6a1b-131">Poczekaj, aż **Nowa aplikacja internetowa ASP.NET Core** wyświetlać okno dialogowe</span><span class="sxs-lookup"><span data-stu-id="b6a1b-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="b6a1b-132">W obszarze **platformy ASP.NET Core szablonów w wersji 2.0** wybierz **aplikacji sieci Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="b6a1b-133">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="b6a1b-134">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="b6a1b-135">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b6a1b-135">Install Entity Framework</span></span>

<span data-ttu-id="b6a1b-136">Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="b6a1b-137">W tym przewodniku używa programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="b6a1b-138">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b6a1b-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="b6a1b-139">**Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="b6a1b-140">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="b6a1b-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="b6a1b-141">Firma Microsoft będzie używać pewnych narzędzi Entity Framework Tools do utworzenia modelu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="b6a1b-142">Dlatego firma Microsoft zainstaluje również za pomocą pakietu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="b6a1b-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="b6a1b-143">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="b6a1b-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="b6a1b-144">Firma Microsoft będzie używać niektórych narzędzi funkcja tworzenia szkieletu ASP.NET Core do tworzenia widoków i kontrolerów później.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="b6a1b-145">Dlatego firma Microsoft zainstaluje tego pakietu projektu:</span><span class="sxs-lookup"><span data-stu-id="b6a1b-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="b6a1b-146">Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="b6a1b-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="b6a1b-147">Model odtworzyć.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-147">Reverse engineer your model</span></span>

<span data-ttu-id="b6a1b-148">Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="b6a1b-149">**Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="b6a1b-150">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="b6a1b-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="b6a1b-151">Jeśli zostanie wyświetlony błąd wskazujący `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, a następnie zamknij i otwórz ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="b6a1b-152">Można określić tabel, które mają zostać wygenerowane jednostki dla, dodając `-Tables` argument polecenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="b6a1b-153">Na przykład `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="b6a1b-154">Proces odtwarzania utworzony klas jednostek (`Blog.cs` & `Post.cs`) i pochodnej kontekstu (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="b6a1b-155">Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="b6a1b-156">Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="b6a1b-157">Zarejestrowanie kontekst wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="b6a1b-157">Register your context with dependency injection</span></span>

<span data-ttu-id="b6a1b-158">Pojęcie wstrzykiwanie zależności stanowi podstawę do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="b6a1b-159">Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="b6a1b-160">Składniki, które wymagają tych usług, (na przykład kontrolerach MVC) znajdują się następnie tych usług za pomocą właściwości lub parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="b6a1b-161">Aby uzyskać więcej informacji na temat wstrzykiwanie zależności zobacz [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) artykuł w witrynie programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="b6a1b-162">Usuń konfigurację kontekstu wbudowane</span><span class="sxs-lookup"><span data-stu-id="b6a1b-162">Remove inline context configuration</span></span>

<span data-ttu-id="b6a1b-163">W programie ASP.NET Core konfiguracji jest zazwyczaj wykonywane w **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="b6a1b-164">Z tego wzorca, firma Microsoft przeniesie konfiguracji dostawcy bazy danych do **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="b6a1b-165">Otwórz `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="b6a1b-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="b6a1b-166">Usuń `OnConfiguring(...)` — metoda</span><span class="sxs-lookup"><span data-stu-id="b6a1b-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="b6a1b-167">Dodaj następujący Konstruktor, co umożliwi użytkownikom konfiguracji, które zostaną przekazane do kontekstu przez wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="b6a1b-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="b6a1b-168">Rejestrowanie i konfigurowanie kontekstu w pliku Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b6a1b-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="b6a1b-169">Aby nasze kontrolerów MVC użytkowania `BloggingContext` pobierzemy zarejestrował ją jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="b6a1b-170">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="b6a1b-171">Dodaj następujący kod `using` instrukcji na początku pliku</span><span class="sxs-lookup"><span data-stu-id="b6a1b-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="b6a1b-172">Teraz możemy użyć `AddDbContext(...)` metodę, aby zarejestrować go jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="b6a1b-173">Znajdź `ConfigureServices(...)` — metoda</span><span class="sxs-lookup"><span data-stu-id="b6a1b-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="b6a1b-174">Dodaj następujący kod, aby zarejestrować kontekst jako usługa</span><span class="sxs-lookup"><span data-stu-id="b6a1b-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="b6a1b-175">W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="b6a1b-176">Dla uproszczenia firma Microsoft jest definiowanie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="b6a1b-177">Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="b6a1b-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="b6a1b-178">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="b6a1b-178">Create a controller</span></span>

<span data-ttu-id="b6a1b-179">Firma Microsoft będzie następnie Włącz tworzenie szkieletów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="b6a1b-180">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="b6a1b-181">Wybierz **pełną zależności** i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="b6a1b-182">Można zignorować zgodnie z instrukcjami w `ScaffoldingReadMe.txt` pliku, który zostanie otwarty</span><span class="sxs-lookup"><span data-stu-id="b6a1b-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="b6a1b-183">Teraz, gdy jest włączona funkcja szkieletów, firma Microsoft tworzenia szkieletu kontrolera dla `Blog` jednostki.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="b6a1b-184">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="b6a1b-185">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="b6a1b-186">Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="b6a1b-187">Kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="b6a1b-188">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6a1b-188">Run the application</span></span>

<span data-ttu-id="b6a1b-189">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="b6a1b-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="b6a1b-190">**Debuguj -> Start bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="b6a1b-191">Utworzysz aplikację i Otwórz w przeglądarce sieci web</span><span class="sxs-lookup"><span data-stu-id="b6a1b-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="b6a1b-192">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="b6a1b-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="b6a1b-193">Kliknij przycisk **Utwórz nową**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-193">Click **Create New**</span></span>
* <span data-ttu-id="b6a1b-194">Wprowadź **adresu Url** nowego bloga, a następnie kliknij przycisk **Create**</span><span class="sxs-lookup"><span data-stu-id="b6a1b-194">Enter a **Url** for the new blog and click **Create**</span></span>

![obraz](_static/create.png)

![obraz](_static/index-existing-db.png)
