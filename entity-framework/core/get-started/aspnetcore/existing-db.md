---
title: Wprowadzenie do platformy ASP.NET Core — istniejącej bazy danych — EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 23cd53b0e162afc5db0243b7032bb9c5f18bfb35
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688696"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="72364-102">Wprowadzenie do programu EF Core programu ASP.NET Core z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="72364-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="72364-103">W tym samouczku utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane dostępu przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="72364-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="72364-104">Możesz odtwarzać istniejącą bazę danych do tworzenia modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72364-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="72364-105">[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="72364-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72364-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="72364-106">Prerequisites</span></span>

<span data-ttu-id="72364-107">Zainstaluj następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="72364-107">Install the following software:</span></span>

* <span data-ttu-id="72364-108">[Visual Studio 2017 w wersji 15.7](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:</span><span class="sxs-lookup"><span data-stu-id="72364-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="72364-109">**ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)</span><span class="sxs-lookup"><span data-stu-id="72364-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="72364-110">**Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)</span><span class="sxs-lookup"><span data-stu-id="72364-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="72364-111">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="72364-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="72364-112">Utwórz bazę danych do obsługi blogów</span><span class="sxs-lookup"><span data-stu-id="72364-112">Create Blogging database</span></span>

<span data-ttu-id="72364-113">W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72364-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="72364-114">Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innego samouczek, należy pominąć tę procedurę.</span><span class="sxs-lookup"><span data-stu-id="72364-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="72364-115">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72364-115">Open Visual Studio</span></span>
* <span data-ttu-id="72364-116">**Narzędzia -> nawiązać połączenie z bazą danych...**</span><span class="sxs-lookup"><span data-stu-id="72364-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="72364-117">Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**</span><span class="sxs-lookup"><span data-stu-id="72364-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="72364-118">Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**</span><span class="sxs-lookup"><span data-stu-id="72364-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="72364-119">Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="72364-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="72364-120">Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="72364-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="72364-121">Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="72364-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="72364-122">Skopiuj skrypt w edytorze zapytań</span><span class="sxs-lookup"><span data-stu-id="72364-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="72364-123">Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="72364-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="72364-124">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="72364-124">Create a new project</span></span>

* <span data-ttu-id="72364-125">Otwórz program Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="72364-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="72364-126">**Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="72364-126">**File > New > Project...**</span></span>
* <span data-ttu-id="72364-127">Z menu po lewej stronie wybierz **zainstalowane > Visual C# > sieci Web**</span><span class="sxs-lookup"><span data-stu-id="72364-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="72364-128">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu</span><span class="sxs-lookup"><span data-stu-id="72364-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="72364-129">Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="72364-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="72364-130">Poczekaj, aż **Nowa aplikacja internetowa ASP.NET Core** wyświetlać okno dialogowe</span><span class="sxs-lookup"><span data-stu-id="72364-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="72364-131">Upewnij się, że menu rozwijanego platform docelowych jest ustawiona na **platformy .NET Core**, a lista rozwijana wersji jest ustawiona na **platformy ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="72364-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="72364-132">Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu</span><span class="sxs-lookup"><span data-stu-id="72364-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="72364-133">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="72364-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="72364-134">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="72364-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="72364-135">Instalowanie platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="72364-135">Install Entity Framework Core</span></span>

<span data-ttu-id="72364-136">Aby zainstalować programu EF Core, należy zainstalować pakiet dla dostawców bazy danych programu EF Core, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="72364-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="72364-137">Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="72364-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="72364-138">Na potrzeby tego samouczka nie trzeba zainstalować pakiet dostawcy, ponieważ w tym samouczku użyto programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="72364-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="72364-139">Pakiet dostawcy programu SQL Server znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="72364-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="72364-140">Model odtworzyć.</span><span class="sxs-lookup"><span data-stu-id="72364-140">Reverse engineer your model</span></span>

<span data-ttu-id="72364-141">Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="72364-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="72364-142">**Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="72364-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="72364-143">Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="72364-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="72364-144">Jeśli zostanie wyświetlony błąd wskazujący `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, a następnie zamknij i otwórz ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72364-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="72364-145">Można określić tabel, które mają zostać wygenerowane jednostki dla, dodając `-Tables` argument polecenia powyżej.</span><span class="sxs-lookup"><span data-stu-id="72364-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="72364-146">Na przykład `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="72364-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="72364-147">Proces odtwarzania utworzony klas jednostek (`Blog.cs` & `Post.cs`) i pochodnej kontekstu (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72364-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="72364-148">Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="72364-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="72364-149">Poniżej przedstawiono `Blog` i `Post` klas jednostek:</span><span class="sxs-lookup"><span data-stu-id="72364-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="72364-150">Aby włączyć ładowanie z opóźnieniem, należy wybrać właściwości nawigacji `virtual` (Blog.Post i Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="72364-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="72364-151">Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="72364-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="72364-152">Zarejestrowanie kontekst wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="72364-152">Register your context with dependency injection</span></span>

<span data-ttu-id="72364-153">Pojęcie wstrzykiwanie zależności stanowi podstawę do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72364-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="72364-154">Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="72364-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="72364-155">Składniki, które wymagają tych usług, (na przykład kontrolerach MVC) znajdują się następnie tych usług za pomocą właściwości lub parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="72364-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="72364-156">Aby uzyskać więcej informacji na temat wstrzykiwanie zależności zobacz [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) artykuł w witrynie programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72364-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="72364-157">Rejestrowanie i konfigurowanie kontekstu w pliku Startup.cs</span><span class="sxs-lookup"><span data-stu-id="72364-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="72364-158">Zapewnienie `BloggingContext` dostępne dla kontrolerów MVC, zarejestruj go jako usługę.</span><span class="sxs-lookup"><span data-stu-id="72364-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="72364-159">Otwórz **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="72364-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="72364-160">Dodaj następujący kod `using` instrukcji na początku pliku</span><span class="sxs-lookup"><span data-stu-id="72364-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="72364-161">Teraz możesz używać `AddDbContext(...)` metodę, aby zarejestrować go jako usługę.</span><span class="sxs-lookup"><span data-stu-id="72364-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="72364-162">Znajdź `ConfigureServices(...)` — metoda</span><span class="sxs-lookup"><span data-stu-id="72364-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="72364-163">Dodaj następujący wyróżniony kod, aby zarejestrować kontekst jako usługa</span><span class="sxs-lookup"><span data-stu-id="72364-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="72364-164">W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w zmiennej środowisku lub pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="72364-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="72364-165">W tym samouczku dla uproszczenia, ma możesz zdefiniować go w kodzie.</span><span class="sxs-lookup"><span data-stu-id="72364-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="72364-166">Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="72364-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="72364-167">Tworzenie kontrolera i widoki</span><span class="sxs-lookup"><span data-stu-id="72364-167">Create a controller and views</span></span>

* <span data-ttu-id="72364-168">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**</span><span class="sxs-lookup"><span data-stu-id="72364-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="72364-169">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**</span><span class="sxs-lookup"><span data-stu-id="72364-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="72364-170">Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="72364-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="72364-171">Kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="72364-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="72364-172">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="72364-172">Run the application</span></span>

<span data-ttu-id="72364-173">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="72364-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="72364-174">**Debuguj -> Start bez debugowania**</span><span class="sxs-lookup"><span data-stu-id="72364-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="72364-175">Aplikacja zostanie skompilowana i zostanie otwarta w przeglądarce sieci web</span><span class="sxs-lookup"><span data-stu-id="72364-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="72364-176">Przejdź do `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="72364-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="72364-177">Kliknij przycisk **Utwórz nową**</span><span class="sxs-lookup"><span data-stu-id="72364-177">Click **Create New**</span></span>
* <span data-ttu-id="72364-178">Wprowadź **adresu Url** nowego bloga, a następnie kliknij przycisk **Create**</span><span class="sxs-lookup"><span data-stu-id="72364-178">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Tworzenie strony](_static/create.png)

  ![Strona indeksu](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="72364-181">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="72364-181">Next steps</span></span>

<span data-ttu-id="72364-182">Aby uzyskać więcej informacji na temat sposobu tworzenia szkieletu klasy kontekstu i jednostek, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="72364-182">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="72364-183">Odtwarzanie</span><span class="sxs-lookup"><span data-stu-id="72364-183">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="72364-184">Entity Framework Core odnoszą się narzędzia — interfejs wiersza polecenia platformy .NET</span><span class="sxs-lookup"><span data-stu-id="72364-184">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="72364-185">Entity Framework Core odnoszą się narzędzia — Konsola Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="72364-185">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)