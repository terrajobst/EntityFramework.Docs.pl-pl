---
title: Wprowadzenie do platformy ASP.NET Core - istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151017"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="646dd-102">Wprowadzenie do podstawowych EF na platformy ASP.NET Core z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="646dd-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="646dd-103">W tym przykładzie utworzysz aplikacji platformy ASP.NET Core MVC, który wykonuje dostęp do podstawowych danych przy użyciu programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="646dd-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="646dd-104">Odtwarzanie użyje do utworzenia modelu programu Entity Framework, na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="646dd-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="646dd-105">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="646dd-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="646dd-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="646dd-106">Prerequisites</span></span>

<span data-ttu-id="646dd-107">Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="646dd-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="646dd-108">[15 ustęp 3 programu Visual Studio 2017](https://www.visualstudio.com/downloads/) z te obciążenia:</span><span class="sxs-lookup"><span data-stu-id="646dd-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="646dd-109">**ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)</span><span class="sxs-lookup"><span data-stu-id="646dd-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="646dd-110">**Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)</span><span class="sxs-lookup"><span data-stu-id="646dd-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="646dd-111">[.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="646dd-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="646dd-112">Blog bazy danych</span><span class="sxs-lookup"><span data-stu-id="646dd-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="646dd-113">Blog bazy danych</span><span class="sxs-lookup"><span data-stu-id="646dd-113">Blogging database</span></span>

<span data-ttu-id="646dd-114">W tym samouczku używana **obsługi blogów** bazy danych w wystąpieniu bazy danych LocalDb jako istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="646dd-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="646dd-115">Jeśli utworzono już **obsługi blogów** bazy danych jako część innego samouczek, można pominąć te kroki.</span><span class="sxs-lookup"><span data-stu-id="646dd-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="646dd-116">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="646dd-116">Open Visual Studio</span></span>
* <span data-ttu-id="646dd-117">**Narzędzia -> łączenia z bazą danych...**</span><span class="sxs-lookup"><span data-stu-id="646dd-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="646dd-118">Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**</span><span class="sxs-lookup"><span data-stu-id="646dd-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="646dd-119">Wprowadź **(localdb) \mssqllocaldb** jako **nazwa serwera**</span><span class="sxs-lookup"><span data-stu-id="646dd-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="646dd-120">Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="646dd-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="646dd-121">Baza danych master jest teraz wyświetlany w obszarze **połączenia danych** w **Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="646dd-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="646dd-122">Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowej kwerendy**</span><span class="sxs-lookup"><span data-stu-id="646dd-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="646dd-123">Skopiuj skrypt wymienione poniżej do edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="646dd-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="646dd-124">Kliknij prawym przyciskiem myszy w edytorze zapytań i wybierz **Execute**</span><span class="sxs-lookup"><span data-stu-id="646dd-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="646dd-125">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="646dd-125">Create a new project</span></span>

* <span data-ttu-id="646dd-126">Otwórz 2017 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="646dd-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="646dd-127">**Plik -> Nowy -> Projekt...**</span><span class="sxs-lookup"><span data-stu-id="646dd-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="646dd-128">Wybierz z menu po lewej stronie **zainstalowana -> Szablony -> Visual C# -> Sieć Web**</span><span class="sxs-lookup"><span data-stu-id="646dd-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="646dd-129">Wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="646dd-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="646dd-130">Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwy i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="646dd-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="646dd-131">Poczekaj, aż **nową aplikację sieci Web Core ASP.NET** wyświetlać okno dialogowe</span><span class="sxs-lookup"><span data-stu-id="646dd-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="646dd-132">W obszarze **ASP.NET Core szablony 2.0** wybierz **aplikacji sieci Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="646dd-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="646dd-133">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="646dd-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="646dd-134">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="646dd-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="646dd-135">Instalowanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="646dd-135">Install Entity Framework</span></span>

<span data-ttu-id="646dd-136">Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa.</span><span class="sxs-lookup"><span data-stu-id="646dd-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="646dd-137">W tym przewodniku zastosowano programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="646dd-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="646dd-138">Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="646dd-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="646dd-139">**Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="646dd-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="646dd-140">Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="646dd-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="646dd-141">Użyjemy niektóre Entity Framework narzędzia do tworzenia modeli z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="646dd-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="646dd-142">Dlatego zostanie zainstalowany pakiet narzędzi również:</span><span class="sxs-lookup"><span data-stu-id="646dd-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="646dd-143">Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="646dd-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="646dd-144">Użyjemy niektóre platformy ASP.NET Core szkieletów narzędzia do tworzenia widoków i kontrolerów później.</span><span class="sxs-lookup"><span data-stu-id="646dd-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="646dd-145">Dlatego zostanie zainstalowany ten pakiet projektu:</span><span class="sxs-lookup"><span data-stu-id="646dd-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="646dd-146">Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="646dd-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="646dd-147">Odtworzyć modelu</span><span class="sxs-lookup"><span data-stu-id="646dd-147">Reverse engineer your model</span></span>

<span data-ttu-id="646dd-148">Teraz nadszedł czas, aby utworzyć model EF na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="646dd-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="646dd-149">**Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="646dd-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="646dd-150">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="646dd-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="646dd-151">Jeśli zostanie wyświetlony błąd informujący o `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, a następnie zamknij i otwórz ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="646dd-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="646dd-152">Można określić tabel, które mają zostać wygenerowane przez dodanie jednostek dla `-Tables` argument polecenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="646dd-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="646dd-153">Np.</span><span class="sxs-lookup"><span data-stu-id="646dd-153">E.g.</span></span> <span data-ttu-id="646dd-154">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="646dd-154">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="646dd-155">Proces odtwarzania tworzony klas jednostek (`Blog.cs` & `Post.cs`) i kontekst pochodnych (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="646dd-155">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="646dd-156">Klas jednostek to proste C# obiekty reprezentujące dane można będzie kwerend i zapisywanie.</span><span class="sxs-lookup"><span data-stu-id="646dd-156">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="646dd-157">Kontekst reprezentuje sesji z bazą danych i służy do wykonywania zapytań i zapisać wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="646dd-157">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="646dd-158">Zarejestruj kontekst iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="646dd-158">Register your context with dependency injection</span></span>

<span data-ttu-id="646dd-159">Pojęcie iniekcji zależności jest podstawą do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="646dd-159">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="646dd-160">Usługi (takie jak `BloggingContext`) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="646dd-160">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="646dd-161">Składniki, które wymagają tych usług (takich jak kontrolerów MVC) są następnie udostępniane tych usług za pomocą konstruktora parametry lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="646dd-161">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="646dd-162">Aby uzyskać więcej informacji na temat iniekcji zależności zobacz [iniekcji zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) artykuł w witrynie platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="646dd-162">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="646dd-163">Usuń konfigurację kontekstu wbudowany</span><span class="sxs-lookup"><span data-stu-id="646dd-163">Remove inline context configuration</span></span>

<span data-ttu-id="646dd-164">W ASP.NET Core konfiguracji jest zazwyczaj wykonywane w **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="646dd-164">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="646dd-165">Z tego wzorca, możemy przenieść konfiguracji dostawcy bazy danych do **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="646dd-165">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="646dd-166">Otwórz `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="646dd-166">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="646dd-167">Usuń `OnConfiguring(...)` — metoda</span><span class="sxs-lookup"><span data-stu-id="646dd-167">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="646dd-168">Dodaj następujący Konstruktor, co pozwoli konfigurację do przekazania do kontekstu przez iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="646dd-168">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="646dd-169">Zarejestruj i skonfiguruj kontekst w pliku Startup.cs</span><span class="sxs-lookup"><span data-stu-id="646dd-169">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="646dd-170">Aby naszych kontrolerów MVC korzystać z `BloggingContext` zamierzamy, aby zarejestrować go jako usługa.</span><span class="sxs-lookup"><span data-stu-id="646dd-170">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="646dd-171">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="646dd-171">Open **Startup.cs**</span></span>
* <span data-ttu-id="646dd-172">Dodaj następujące `using` instrukcje na początku pliku</span><span class="sxs-lookup"><span data-stu-id="646dd-172">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="646dd-173">Teraz możemy użyć `AddDbContext(...)` metody, aby zarejestrować go jako usługa.</span><span class="sxs-lookup"><span data-stu-id="646dd-173">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="646dd-174">Zlokalizuj `ConfigureServices(...)` — metoda</span><span class="sxs-lookup"><span data-stu-id="646dd-174">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="646dd-175">Dodaj następujący kod, aby zarejestrować kontekstu w trybie usługi</span><span class="sxs-lookup"><span data-stu-id="646dd-175">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="646dd-176">W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="646dd-176">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="646dd-177">Dla uproszczenia możemy są definiowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="646dd-177">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="646dd-178">Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="646dd-178">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="646dd-179">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="646dd-179">Create a controller</span></span>

<span data-ttu-id="646dd-180">Następnie firma Microsoft będzie aktywują szkielet w naszym projektu.</span><span class="sxs-lookup"><span data-stu-id="646dd-180">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="646dd-181">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **kontrolera -> Dodaj...**</span><span class="sxs-lookup"><span data-stu-id="646dd-181">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="646dd-182">Wybierz **pełne zależności** i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="646dd-182">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="646dd-183">Możesz zignorować instrukcje `ScaffoldingReadMe.txt` pliku, który zostanie otwarty</span><span class="sxs-lookup"><span data-stu-id="646dd-183">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="646dd-184">Teraz, gdy jest włączona funkcja szkieletów, możemy utworzyć szkielet kontrolera dla `Blog` jednostki.</span><span class="sxs-lookup"><span data-stu-id="646dd-184">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="646dd-185">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **kontrolera -> Dodaj...**</span><span class="sxs-lookup"><span data-stu-id="646dd-185">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="646dd-186">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**</span><span class="sxs-lookup"><span data-stu-id="646dd-186">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="646dd-187">Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="646dd-187">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="646dd-188">Kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="646dd-188">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="646dd-189">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="646dd-189">Run the application</span></span>

<span data-ttu-id="646dd-190">Można teraz uruchomić aplikację, aby zobaczyć ją w akcji.</span><span class="sxs-lookup"><span data-stu-id="646dd-190">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="646dd-191">**Debugowanie -> Start bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="646dd-191">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="646dd-192">Aplikacja kompilacji i Otwórz w przeglądarce sieci web</span><span class="sxs-lookup"><span data-stu-id="646dd-192">The application will build and open in a web browser</span></span>
* <span data-ttu-id="646dd-193">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="646dd-193">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="646dd-194">Kliknij przycisk **Utwórz nową**</span><span class="sxs-lookup"><span data-stu-id="646dd-194">Click **Create New**</span></span>
* <span data-ttu-id="646dd-195">Wprowadź **adres Url** nowy blog i kliknij **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="646dd-195">Enter a **Url** for the new blog and click **Create**</span></span>

![obraz](_static/create.png)

![obraz](_static/index-existing-db.png)
