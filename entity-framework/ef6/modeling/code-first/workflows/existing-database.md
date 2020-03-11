---
title: Code First do istniejącej bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418876"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="a66bf-102">Code First do istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="a66bf-102">Code First to an Existing Database</span></span>
<span data-ttu-id="a66bf-103">Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Code First projektowania dla istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="a66bf-104">Code First pozwala definiować model przy użyciu klas C\# lub VB.Net.</span><span class="sxs-lookup"><span data-stu-id="a66bf-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="a66bf-105">Opcjonalnie można wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości lub przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a66bf-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a66bf-106">Obejrzyj film</span><span class="sxs-lookup"><span data-stu-id="a66bf-106">Watch the video</span></span>
<span data-ttu-id="a66bf-107">Ten film wideo jest [teraz dostępny w witrynie Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="a66bf-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a66bf-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a66bf-108">Pre-Requisites</span></span>

<span data-ttu-id="a66bf-109">Aby ukończyć ten przewodnik, musisz mieć zainstalowany **program Visual Studio 2012** lub **Visual Studio 2013** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="a66bf-110">Wymagana jest również wersja **6,1** (lub nowsza) **Entity Framework Tools programu Visual Studio** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="a66bf-111">Aby uzyskać informacje na temat instalowania najnowszej wersji Entity Framework Tools, zobacz [pobierz Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="a66bf-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="a66bf-112">1. Tworzenie istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="a66bf-112">1. Create an Existing Database</span></span>

<span data-ttu-id="a66bf-113">Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="a66bf-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="a66bf-114">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a66bf-115">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a66bf-115">Open Visual Studio</span></span>
-   <span data-ttu-id="a66bf-116">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="a66bf-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a66bf-117">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="a66bf-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a66bf-118">Jeśli nie masz połączenia z bazą danych **Eksplorator serwera** przed wybraniem **Microsoft SQL Server** jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="a66bf-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="a66bf-120">Nawiąż połączenie z wystąpieniem programu LocalDB i wprowadź nazwę bazy danych na potrzeby prowadzenia **blogów**</span><span class="sxs-lookup"><span data-stu-id="a66bf-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Połączenie LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="a66bf-122">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Okno dialogowe Tworzenie bazy danych](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="a66bf-124">Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="a66bf-125">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="a66bf-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="a66bf-126">2. Utwórz aplikację</span><span class="sxs-lookup"><span data-stu-id="a66bf-126">2. Create the Application</span></span>

<span data-ttu-id="a66bf-127">Aby zachować prostotę, utworzysz podstawową aplikację konsolową, która używa Code First do uzyskiwania dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="a66bf-127">To keep things simple we will build a basic console application that uses Code First to do the data access:</span></span>

-   <span data-ttu-id="a66bf-128">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a66bf-128">Open Visual Studio</span></span>
-   <span data-ttu-id="a66bf-129">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="a66bf-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="a66bf-130">Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**</span><span class="sxs-lookup"><span data-stu-id="a66bf-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="a66bf-131">Wprowadź **CodeFirstExistingDatabaseSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="a66bf-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="a66bf-132">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="a66bf-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="a66bf-133">3. Odtwarzanie modelu</span><span class="sxs-lookup"><span data-stu-id="a66bf-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="a66bf-134">Użyjemy Entity Framework Tools dla programu Visual Studio, aby pomóc nam generować kod początkowy, który będzie mapowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-134">We will use the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="a66bf-135">Narzędzia te generują tylko kod, który można również wpisać ręcznie, jeśli wolisz.</span><span class="sxs-lookup"><span data-stu-id="a66bf-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="a66bf-136">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="a66bf-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a66bf-137">Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a66bf-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a66bf-138">Wprowadź **BloggingContext** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="a66bf-139">Spowoduje to uruchomienie **kreatora Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a66bf-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="a66bf-140">Wybierz **Code First z bazy danych** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-140">Select **Code First from Database** and click **Next**</span></span>

    ![Jeden CFE Kreatora](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="a66bf-142">Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Kreator dwóch CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="a66bf-144">Kliknij pole wyboru obok pozycji **tabele** , aby zaimportować wszystkie tabele, a następnie kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Kreator trzech CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="a66bf-146">Po zakończeniu procesu odtwarzania liczba elementów zostanie dodana do projektu, przyjrzyjmy się dodaniu.</span><span class="sxs-lookup"><span data-stu-id="a66bf-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="a66bf-147">Plik konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a66bf-147">Configuration file</span></span>

<span data-ttu-id="a66bf-148">Plik App. config został dodany do projektu, ten plik zawiera parametry połączenia do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="a66bf-149">*Zauważysz, że istnieją inne ustawienia w pliku konfiguracji, są to domyślne ustawienia EF, które informują Code First miejsca tworzenia baz danych. Ponieważ mapowanie do istniejącej bazy danych, to ustawienie zostanie zignorowane w naszej aplikacji.*</span><span class="sxs-lookup"><span data-stu-id="a66bf-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="a66bf-150">Kontekst pochodny</span><span class="sxs-lookup"><span data-stu-id="a66bf-150">Derived Context</span></span>

<span data-ttu-id="a66bf-151">Do projektu Dodano klasę **BloggingContext** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="a66bf-152">Kontekst reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="a66bf-153">Kontekst ujawnia **nieogólnymi&lt;&gt;** dla każdego typu w modelu.</span><span class="sxs-lookup"><span data-stu-id="a66bf-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="a66bf-154">Zauważ również, że Konstruktor domyślny wywołuje konstruktora podstawowego przy użyciu składni **name =** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="a66bf-155">Informuje to Code First, że parametry połączenia do użycia dla tego kontekstu powinny zostać załadowane z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a66bf-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="a66bf-156">*Jeśli używasz parametrów połączenia w pliku konfiguracji, należy zawsze używać składni **name =** . Gwarantuje to, że jeśli parametry połączenia nie są obecne, Entity Framework będzie zgłaszany zamiast tworzenia nowej bazy danych według Konwencji.*</span><span class="sxs-lookup"><span data-stu-id="a66bf-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="a66bf-157">Klasy modelu</span><span class="sxs-lookup"><span data-stu-id="a66bf-157">Model classes</span></span>

<span data-ttu-id="a66bf-158">Na koniec do projektu dodano także **blog** i Klasa **post** .</span><span class="sxs-lookup"><span data-stu-id="a66bf-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="a66bf-159">Są to klasy domeny, które składają się na nasz model.</span><span class="sxs-lookup"><span data-stu-id="a66bf-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="a66bf-160">Zobaczysz adnotacje danych zastosowane do klas, aby określić konfigurację, w której konwencje Code First nie były wyrównane z strukturą istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="a66bf-161">Na przykład, zobaczysz adnotację **StringLength** w **blog.Name** i **blogu. URL** , ponieważ ma ona maksymalną długość **200** w bazie danych (Code First domyślnym jest użycie długości Maximun obsługiwanej przez dostawcę bazy danych — **nvarchar (max)** w SQL Server).</span><span class="sxs-lookup"><span data-stu-id="a66bf-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="a66bf-162">4. odczytywanie & zapisywania danych</span><span class="sxs-lookup"><span data-stu-id="a66bf-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="a66bf-163">Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="a66bf-164">Zaimplementuj metodę **Main** w **program.cs** , jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a66bf-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="a66bf-165">Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego **bloga**.</span><span class="sxs-lookup"><span data-stu-id="a66bf-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="a66bf-166">Następnie używa zapytania LINQ do pobrania wszystkich **blogów** z bazy danych uporządkowane alfabetycznie według **tytułu**.</span><span class="sxs-lookup"><span data-stu-id="a66bf-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="a66bf-167">Teraz można uruchomić aplikację i przetestować ją.</span><span class="sxs-lookup"><span data-stu-id="a66bf-167">You can now run the application and test it.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="a66bf-168">Co zrobić, jeśli moja baza danych ulegnie zmianie?</span><span class="sxs-lookup"><span data-stu-id="a66bf-168">What if My Database Changes?</span></span>

<span data-ttu-id="a66bf-169">Kreator Code First do bazy danych służy do generowania zestawu punktów początkowych klas, które można następnie dostosować i zmodyfikować.</span><span class="sxs-lookup"><span data-stu-id="a66bf-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="a66bf-170">Jeśli schemat bazy danych ulegnie zmianie, można ręcznie edytować klasy lub wykonać inny inżynier odtwarzania w celu zastąpienia klas.</span><span class="sxs-lookup"><span data-stu-id="a66bf-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="a66bf-171">Używanie Migracje Code First do istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="a66bf-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="a66bf-172">Jeśli chcesz użyć Migracje Code First z istniejącą bazą danych, zobacz [migracje Code First do istniejącej bazy danych](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="a66bf-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="a66bf-173">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a66bf-173">Summary</span></span>

<span data-ttu-id="a66bf-174">W tym instruktażu oglądamy Code First opracowywanie przy użyciu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="a66bf-175">Użyto Entity Framework Tools dla programu Visual Studio w celu odtworzenia zestawu klas, które są zamapowane na bazę danych i może służyć do przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="a66bf-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
