---
title: Najpierw kod istniejącej bazy danych — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912725"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="4a025-102">Najpierw kod istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="4a025-102">Code First to an Existing Database</span></span>
<span data-ttu-id="4a025-103">W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwiązania deweloperskiego Code First przeznaczonych dla istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="4a025-104">Kod umożliwia najpierw zdefiniować model za pomocą języka C\# lub klasy VB.Net.</span><span class="sxs-lookup"><span data-stu-id="4a025-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="4a025-105">Opcjonalnie dodatkowej konfiguracji można wykonać przy użyciu atrybutów klas i właściwości lub za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="4a025-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="4a025-106">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="4a025-106">Watch the video</span></span>
<span data-ttu-id="4a025-107">To wideo jest [teraz dostępna w witrynie Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="4a025-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="4a025-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4a025-108">Pre-Requisites</span></span>

<span data-ttu-id="4a025-109">Musisz mieć **programu Visual Studio 2012** lub **programu Visual Studio 2013** zainstalowany w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="4a025-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="4a025-110">Należy również wersja **6.1** (lub nowszym) z **Entity Framework Tools for Visual Studio** zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="4a025-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="4a025-111">Zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md) informacji na temat instalowania najnowszej wersji narzędzi Entity Framework Tools.</span><span class="sxs-lookup"><span data-stu-id="4a025-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="4a025-112">1. Utwórz istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="4a025-112">1. Create an Existing Database</span></span>

<span data-ttu-id="4a025-113">Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="4a025-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="4a025-114">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="4a025-115">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a025-115">Open Visual Studio</span></span>
-   <span data-ttu-id="4a025-116">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="4a025-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="4a025-117">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="4a025-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="4a025-118">Jeśli nie jest połączona z bazą danych z **Eksploratora serwera** zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="4a025-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="4a025-120">Połącz się z wystąpieniem usługi LocalDB, a następnie wprowadź **do obsługi blogów** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="4a025-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="4a025-122">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="4a025-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="4a025-124">Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="4a025-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="4a025-125">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="4a025-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="4a025-126">2. Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4a025-126">2. Create the Application</span></span>

<span data-ttu-id="4a025-127">Aby zachować ich prostotę zamierzamy utworzyć aplikację podstawowa konsola, która używa Code First do dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="4a025-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="4a025-128">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a025-128">Open Visual Studio</span></span>
-   <span data-ttu-id="4a025-129">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="4a025-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="4a025-130">Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**</span><span class="sxs-lookup"><span data-stu-id="4a025-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="4a025-131">Wprowadź **CodeFirstExistingDatabaseSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="4a025-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="4a025-132">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="4a025-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="4a025-133">3. Model odtwarzania</span><span class="sxs-lookup"><span data-stu-id="4a025-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="4a025-134">Będziemy korzystać z narzędzi Entity Framework Tools for Visual Studio ułatwia nam dotarcie kodu początkowego do mapowania na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="4a025-135">Te narzędzia po prostu generują kod, który można także wpisać ręcznie, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="4a025-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="4a025-136">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="4a025-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="4a025-137">Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="4a025-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="4a025-138">Wprowadź **BloggingContext** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="4a025-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="4a025-139">Spowoduje to uruchomienie **Kreator modelu Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="4a025-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="4a025-140">Wybierz **Code First z bazy danych** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="4a025-140">Select **Code First from Database** and click **Next**</span></span>

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="4a025-142">Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, a następnie kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="4a025-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="4a025-144">Kliknij pole wyboru obok pozycji **tabel** importowania wszystkich tabel, a następnie kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="4a025-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="4a025-146">Po procesu odtwarzania wykonaniu określonej liczby elementów będzie zostały dodane do projektu, możemy przyjrzeć się elementy dodane.</span><span class="sxs-lookup"><span data-stu-id="4a025-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="4a025-147">Plik konfiguracji</span><span class="sxs-lookup"><span data-stu-id="4a025-147">Configuration file</span></span>

<span data-ttu-id="4a025-148">Plik App.config został dodany do projektu, ten plik zawiera parametry połączenia z istniejącą bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="4a025-149">*Można zauważyć za niektóre inne ustawienia w pliku konfiguracji, są ustawienia domyślne programu EF, które informują Code First, gdzie można utworzyć bazy danych. Ponieważ firma Microsoft jest mapowany do istniejącej bazy danych, te ustawienia zostaną zignorowane w naszej aplikacji.*</span><span class="sxs-lookup"><span data-stu-id="4a025-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="4a025-150">Kontekst pochodne</span><span class="sxs-lookup"><span data-stu-id="4a025-150">Derived Context</span></span>

<span data-ttu-id="4a025-151">A **BloggingContext** klasy został dodany do projektu.</span><span class="sxs-lookup"><span data-stu-id="4a025-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="4a025-152">Kontekst reprezentuje sesję z bazą danych, dzięki czemu nam zapytania i Zapisz dane.</span><span class="sxs-lookup"><span data-stu-id="4a025-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="4a025-153">Udostępnia kontekst **DbSet&lt;TEntity&gt;**  dla każdego typu w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="4a025-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="4a025-154">Można także zauważyć, że domyślny konstruktor wywołuje konstruktora podstawowego przy użyciu **nazwa =** składni.</span><span class="sxs-lookup"><span data-stu-id="4a025-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="4a025-155">Oznacza to Code First, że parametry połączenia do użycia dla tego kontekstu powinny być załadowane z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4a025-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="4a025-156">*Należy zawsze używać **nazwa =** składni, w przypadku korzystania z parametrów połączenia w pliku konfiguracji. Daje to gwarancję, że jeśli nie ma parametrów połączenia następnie Entity Framework zgłosi zamiast tworzenia nowej bazy danych przez Konwencję.*</span><span class="sxs-lookup"><span data-stu-id="4a025-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="4a025-157">Klasy modelu</span><span class="sxs-lookup"><span data-stu-id="4a025-157">Model classes</span></span>

<span data-ttu-id="4a025-158">Na koniec **Blog** i **wpis** klasy również zostały dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="4a025-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="4a025-159">Są to klasy domeny, które tworzą nasz model.</span><span class="sxs-lookup"><span data-stu-id="4a025-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="4a025-160">Zobaczysz adnotacje danych stosowany do klas do określenia konfiguracji, której konwencje Code First nie będzie wyrównane z struktury istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="4a025-161">Na przykład, zobaczysz **StringLength** adnotacja **Blog.Name** i **Blog.Url** ponieważ mają maksymalną długość **200** w bazy danych (Code First domyślna ma używać długość maksymalna obsługiwana przez dostawcę bazy danych — **nvarchar(max)** w programie SQL Server).</span><span class="sxs-lookup"><span data-stu-id="4a025-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="4a025-162">4. Odczytywanie i zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="4a025-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="4a025-163">Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="4a025-164">Implementowanie **Main** method in Class metoda **Program.cs** jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4a025-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="4a025-165">Ten kod tworzy nowe wystąpienie klasy nasz kontekst, a następnie używa go, aby wstawić nowy **blogu**.</span><span class="sxs-lookup"><span data-stu-id="4a025-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="4a025-166">Następnie wykorzystuje zapytania LINQ można pobrać wszystkich **blogi** z bazy danych uporządkowana w kolejności alfabetycznej według **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="4a025-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="4a025-167">Możesz teraz uruchomić aplikację i ją przetestować.</span><span class="sxs-lookup"><span data-stu-id="4a025-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="4a025-168">Co zrobić, jeśli Moja baza danych zmiany?</span><span class="sxs-lookup"><span data-stu-id="4a025-168">What if My Database Changes?</span></span>

<span data-ttu-id="4a025-169">Code First Kreatora bazy danych jest przeznaczona do generowania początkowego zestaw punktu klas, które można dostosować i modyfikować.</span><span class="sxs-lookup"><span data-stu-id="4a025-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="4a025-170">Zmiany schematu bazy danych można ręcznie edytować klasy lub wykonania innego odtwarzania, aby zastąpić klasy.</span><span class="sxs-lookup"><span data-stu-id="4a025-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="4a025-171">Za pomocą migracje Code First istniejącą bazę danych</span><span class="sxs-lookup"><span data-stu-id="4a025-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="4a025-172">Jeśli chcesz użyć migracje Code First z istniejącej bazy danych, zobacz [migracje Code First istniejącą bazę danych](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="4a025-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="4a025-173">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4a025-173">Summary</span></span>

<span data-ttu-id="4a025-174">W tym przewodniku przyjrzeliśmy się rozwiązania deweloperskiego Code First przy użyciu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="4a025-175">Użyliśmy narzędzi Entity Framework Tools dla programu Visual Studio do odtworzenia zestaw klas, które są mapowane do bazy danych i może służyć do przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="4a025-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
