---
title: Code First do nowej bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182574"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="d3c23-102">Code First do nowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-102">Code First to a New Database</span></span>
<span data-ttu-id="d3c23-103">Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Code First tworzenia elementów docelowych dla nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="d3c23-104">Ten scenariusz obejmuje bazę danych, która nie istnieje i Code First utworzy, lub pustą bazę danych, która Code First doda nowe tabele do programu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="d3c23-105">Code First pozwala definiować model przy użyciu klas C\# lub VB.Net.</span><span class="sxs-lookup"><span data-stu-id="d3c23-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="d3c23-106">Można opcjonalnie wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości albo za pomocą interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d3c23-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="d3c23-107">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="d3c23-107">Watch the video</span></span>
<span data-ttu-id="d3c23-108">Ten film wideo zawiera wprowadzenie do Code First tworzenia elementów docelowych dla nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="d3c23-109">Ten scenariusz obejmuje bazę danych, która nie istnieje i Code First utworzy, lub pustą bazę danych, która Code First doda nowe tabele do programu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="d3c23-110">Code First pozwala definiować model przy użyciu C# klas lub VB.NET.</span><span class="sxs-lookup"><span data-stu-id="d3c23-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="d3c23-111">Można opcjonalnie wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości albo za pomocą interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d3c23-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="d3c23-112">**Przedstawione przez**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="d3c23-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="d3c23-113">**Wideo**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (zip)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="d3c23-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d3c23-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d3c23-114">Pre-Requisites</span></span>

<span data-ttu-id="d3c23-115">Aby ukończyć ten przewodnik, musisz mieć zainstalowany co najmniej program Visual Studio 2010 lub Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d3c23-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d3c23-116">W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="d3c23-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="d3c23-117">1. Utwórz aplikację</span><span class="sxs-lookup"><span data-stu-id="d3c23-117">1. Create the Application</span></span>

<span data-ttu-id="d3c23-118">Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Code First do uzyskiwania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="d3c23-119">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3c23-119">Open Visual Studio</span></span>
-   <span data-ttu-id="d3c23-120">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d3c23-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="d3c23-121">Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**</span><span class="sxs-lookup"><span data-stu-id="d3c23-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="d3c23-122">Wprowadź **CodeFirstNewDatabaseSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="d3c23-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="d3c23-123">Wybierz **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="d3c23-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="d3c23-124">2. Utwórz model</span><span class="sxs-lookup"><span data-stu-id="d3c23-124">2. Create the Model</span></span>

<span data-ttu-id="d3c23-125">Zdefiniujmy bardzo prosty model przy użyciu klas.</span><span class="sxs-lookup"><span data-stu-id="d3c23-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="d3c23-126">Definiujemy je w pliku Program.cs, ale w prawdziwej aplikacji można podzielić klasy na osobne pliki i potencjalnie oddzielny projekt.</span><span class="sxs-lookup"><span data-stu-id="d3c23-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="d3c23-127">Poniżej definicji klasy programu w Program.cs Dodaj następujące dwie klasy.</span><span class="sxs-lookup"><span data-stu-id="d3c23-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="d3c23-128">Zauważymy, że są one wirtualne z zastosowaniem dwóch właściwości nawigacji (blog. post i post. blog).</span><span class="sxs-lookup"><span data-stu-id="d3c23-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="d3c23-129">Dzięki temu funkcja ładowania z opóźnieniem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d3c23-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="d3c23-130">Ładowanie z opóźnieniem oznacza, że zawartość tych właściwości zostanie automatycznie załadowana z bazy danych przy próbie uzyskania dostępu do nich.</span><span class="sxs-lookup"><span data-stu-id="d3c23-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="d3c23-131">3. Tworzenie kontekstu</span><span class="sxs-lookup"><span data-stu-id="d3c23-131">3. Create a Context</span></span>

<span data-ttu-id="d3c23-132">Teraz można zdefiniować kontekst pochodny, który reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="d3c23-133">Definiujemy kontekst, który pochodzi od elementu System. Data. Entity. DbContext i uwidacznia typ Nieogólnymi&lt;&gt; dla każdej klasy w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="d3c23-134">Teraz zaczynamy używać typów z Entity Framework, więc musimy dodać pakiet NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="d3c23-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="d3c23-135">**Projekt —&gt; zarządzać pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d3c23-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="d3c23-136">Uwaga: Jeśli nie masz **zarządzania pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d3c23-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="d3c23-137">Opcja należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="d3c23-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="d3c23-138">Wybierz kartę **online**</span><span class="sxs-lookup"><span data-stu-id="d3c23-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="d3c23-139">Wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="d3c23-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="d3c23-140">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="d3c23-140">Click **Install**</span></span>

<span data-ttu-id="d3c23-141">Dodaj instrukcję using dla elementu System. Data. Entity w górnej części Program.cs.</span><span class="sxs-lookup"><span data-stu-id="d3c23-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="d3c23-142">Pod klasą post w Program.cs Dodaj następujący kontekst pochodny.</span><span class="sxs-lookup"><span data-stu-id="d3c23-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="d3c23-143">Poniżej znajduje się kompletna lista elementów, które powinny być zawarte w Program.cs.</span><span class="sxs-lookup"><span data-stu-id="d3c23-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="d3c23-144">To wszystko, co jest potrzebne do rozpoczęcia przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="d3c23-145">Oczywiście w tle jest dość dużo miejsca, a w tej chwili zajmiemy się tym, ale po raz pierwszy zobaczymy ją w działaniu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="d3c23-146">4. odczytywanie & zapisywania danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="d3c23-147">Zaimplementuj metodę Main w Program.cs, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d3c23-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="d3c23-148">Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego bloga.</span><span class="sxs-lookup"><span data-stu-id="d3c23-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="d3c23-149">Następnie używa zapytania LINQ do pobrania wszystkich blogów z bazy danych uporządkowane alfabetycznie według tytułu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="d3c23-150">Teraz możesz uruchomić aplikację i przetestować ją.</span><span class="sxs-lookup"><span data-stu-id="d3c23-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="d3c23-151">Gdzie są moje dane?</span><span class="sxs-lookup"><span data-stu-id="d3c23-151">Where’s My Data?</span></span>

<span data-ttu-id="d3c23-152">Według Konwencji DbContext utworzyła bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="d3c23-153">Jeśli lokalne wystąpienie programu SQL Express jest dostępne (domyślnie instalowane z programem Visual Studio 2010), Code First utworzyć bazę danych w tym wystąpieniu</span><span class="sxs-lookup"><span data-stu-id="d3c23-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="d3c23-154">Jeśli program SQL Express jest niedostępny, Code First będzie próbować korzystać z [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalowany domyślnie z programem Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="d3c23-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="d3c23-155">Baza danych jest nazywana po w pełni kwalifikowaną nazwą kontekstu pochodnego, w naszym przypadku jest to **CodeFirstNewDatabaseSample. BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="d3c23-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="d3c23-156">Są to tylko konwencje domyślne i istnieją różne sposoby zmiany bazy danych używanej przez Code First. więcej informacji można znaleźć w temacie **jak DbContext odnajduje model i połączenie z bazą danych** .</span><span class="sxs-lookup"><span data-stu-id="d3c23-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="d3c23-157">Możesz połączyć się z tą bazą danych przy użyciu Eksplorator serwera w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3c23-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="d3c23-158">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="d3c23-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d3c23-159">Kliknij prawym przyciskiem myszy pozycję **połączenia danych** i wybierz polecenie **Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="d3c23-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="d3c23-160">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="d3c23-162">Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany</span><span class="sxs-lookup"><span data-stu-id="d3c23-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="d3c23-163">Teraz można sprawdzić schemat, który Code First utworzony.</span><span class="sxs-lookup"><span data-stu-id="d3c23-163">We can now inspect the schema that Code First created.</span></span>

![Schemat początkowy](~/ef6/media/schemainitial.png)

<span data-ttu-id="d3c23-165">Program DbContext pracował, jakie klasy należy uwzględnić w modelu, przeglądając zdefiniowane przez nas właściwości Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="d3c23-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="d3c23-166">Następnie używa domyślnego zestawu Konwencji Code First, aby określić nazwy tabel i kolumn, określić typy danych, znaleźć klucze podstawowe itd.</span><span class="sxs-lookup"><span data-stu-id="d3c23-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="d3c23-167">W dalszej części tego instruktażu zawarto informacje na temat sposobu przesłania tych konwencji.</span><span class="sxs-lookup"><span data-stu-id="d3c23-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="d3c23-168">5. postępowanie z zmianami modelu</span><span class="sxs-lookup"><span data-stu-id="d3c23-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="d3c23-169">Teraz można wprowadzić pewne zmiany w modelu, gdy wprowadzimy te zmiany, należy również zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="d3c23-170">W tym celu użyjemy funkcji o nazwie Migracje Code First lub migracji na krótko.</span><span class="sxs-lookup"><span data-stu-id="d3c23-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="d3c23-171">Migracja pozwala nam na posiadanie uporządkowanego zestawu kroków opisujących sposób uaktualniania (i obniżania poziomu) naszego schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="d3c23-172">Każda z tych kroków, znana jako migracja, zawiera kod opisujący zmiany, które mają zostać zastosowane.</span><span class="sxs-lookup"><span data-stu-id="d3c23-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="d3c23-173">Pierwszym krokiem jest włączenie Migracje Code First dla naszych BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="d3c23-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="d3c23-174">**Narzędzia — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="d3c23-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="d3c23-175">Uruchamianie polecenia **enable-migrations** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="d3c23-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="d3c23-176">Do naszego projektu dodano nowy folder migracji zawierający dwa elementy:</span><span class="sxs-lookup"><span data-stu-id="d3c23-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="d3c23-177">**Configuration.cs** — ten plik zawiera ustawienia, które będą używane przez migracje na potrzeby migrowania BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="d3c23-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="d3c23-178">Nie musimy niczego zmienić w tym instruktażu, ale w tym miejscu możesz określić dane dotyczące inicjatora, zarejestrować dostawców dla innych baz danych, zmienić przestrzeń nazw, w której są generowane migracje itp.</span><span class="sxs-lookup"><span data-stu-id="d3c23-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="d3c23-179">**&lt;sygnatura czasowa&gt;\_InitialCreate.cs** — jest to Twoja pierwsza migracja. reprezentuje ona zmiany, które zostały już zastosowane do bazy danych w celu przejęcia ich przez pustą bazę danych, która zawiera tabele blogów i wpisów.</span><span class="sxs-lookup"><span data-stu-id="d3c23-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="d3c23-180">Mimo że Code First automatycznie utworzyć te tabele dla nas, teraz, gdy przejdziemy do migracji, zostały one przekonwertowane do migracji.</span><span class="sxs-lookup"><span data-stu-id="d3c23-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="d3c23-181">Code First został również zarejestrowany w lokalnej bazie danych, że ta migracja została już zastosowana.</span><span class="sxs-lookup"><span data-stu-id="d3c23-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="d3c23-182">Sygnatura czasowa w nazwie pliku jest używana do określania kolejności.</span><span class="sxs-lookup"><span data-stu-id="d3c23-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="d3c23-183">Teraz Wprowadźmy zmiany w naszym modelu, dodając Właściwość adresu URL do klasy bloga:</span><span class="sxs-lookup"><span data-stu-id="d3c23-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="d3c23-184">Uruchom polecenie **Add-Migration AddUrl** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d3c23-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="d3c23-185">Polecenie Add-Migration sprawdza zmiany od momentu ostatniej migracji i tworzy szkielet nowej migracji wraz ze wszystkimi znalezionymi zmianami.</span><span class="sxs-lookup"><span data-stu-id="d3c23-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="d3c23-186">Możemy nadać nazwę migracji; w tym przypadku wywołujemy migrację "AddUrl".</span><span class="sxs-lookup"><span data-stu-id="d3c23-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="d3c23-187">Kod szkieletu oznacza, że musimy dodać kolumnę adresu URL, która może przechowywać dane ciągu do właściciela bazy danych. Tabela blogów.</span><span class="sxs-lookup"><span data-stu-id="d3c23-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="d3c23-188">W razie potrzeby możemy edytować kod szkieletowy, ale nie jest to wymagane w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="d3c23-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="d3c23-189">Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d3c23-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="d3c23-190">To polecenie spowoduje zastosowanie wszelkich oczekujących migracji do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="d3c23-191">Nasza migracja InitialCreate została już zastosowana, dlatego migracje będą dotyczyły właśnie nowej migracji AddUrl.</span><span class="sxs-lookup"><span data-stu-id="d3c23-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="d3c23-192">Porada: w przypadku wywołania metody Update-Database można użyć przełącznika **– verbose** , aby zobaczyć, które SQL jest wykonywane względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="d3c23-193">Nowa kolumna adresu URL zostanie dodana do tabeli blogów w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="d3c23-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schemat z adresem URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="d3c23-195">6. adnotacje dotyczące danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-195">6. Data Annotations</span></span>

<span data-ttu-id="d3c23-196">Z tego powodu tylko firma Dr odnajduje model przy użyciu jego Konwencji domyślnych, ale istnieje dużo czasu, gdy nasze klasy nie przestrzegają Konwencji i będziemy mogli przeprowadzić dalszą konfigurację.</span><span class="sxs-lookup"><span data-stu-id="d3c23-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="d3c23-197">Dostępne są dwie opcje: przejrzyjemy adnotacje dotyczące danych w tej sekcji, a następnie interfejs API Fluent w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d3c23-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="d3c23-198">Dodajmy klasę użytkownika do naszego modelu</span><span class="sxs-lookup"><span data-stu-id="d3c23-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="d3c23-199">Musimy również dodać zestaw do naszego kontekstu pochodnego</span><span class="sxs-lookup"><span data-stu-id="d3c23-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="d3c23-200">Jeśli podjęto próbę dodania migracji, wystąpił błąd informujący,*że nie zdefiniowano klucza "EntityType" User ". Zdefiniuj klucz dla tego elementu EntityType. "*</span><span class="sxs-lookup"><span data-stu-id="d3c23-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="d3c23-201">ponieważ EF nie ma możliwości znajomości, że nazwa użytkownika powinna być kluczem podstawowym dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d3c23-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="d3c23-202">Teraz będziemy używać adnotacji danych, więc musimy dodać instrukcję using w górnej części Program.cs</span><span class="sxs-lookup"><span data-stu-id="d3c23-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="d3c23-203">Teraz Dodaj adnotację do właściwości username, aby określić, że jest kluczem podstawowym</span><span class="sxs-lookup"><span data-stu-id="d3c23-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="d3c23-204">Użycie polecenia **Add-Migration adduser** do tworzenia szkieletu migracji w celu zastosowania tych zmian do bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="d3c23-205">Uruchom polecenie **Update-Database** , aby zastosować nową migrację do bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3c23-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="d3c23-206">Nowa tabela jest teraz dodawana do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d3c23-206">The new table is now added to the database:</span></span>

![Schemat z użytkownikami](~/ef6/media/schemawithusers.png)

<span data-ttu-id="d3c23-208">Pełna lista adnotacji obsługiwanych przez EF to:</span><span class="sxs-lookup"><span data-stu-id="d3c23-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="d3c23-209">Atrybut atrybutu</span><span class="sxs-lookup"><span data-stu-id="d3c23-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="d3c23-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="d3c23-211">MaxLengthattribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="d3c23-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="d3c23-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="d3c23-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="d3c23-215">ComplexTypeattribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="d3c23-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="d3c23-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="d3c23-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="d3c23-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="d3c23-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="d3c23-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="d3c23-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="d3c23-222">7. interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="d3c23-222">7. Fluent API</span></span>

<span data-ttu-id="d3c23-223">W poprzedniej sekcji oglądamy używanie adnotacji danych do uzupełniania lub zastępowania elementów wykrytych w ramach Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d3c23-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="d3c23-224">Innym sposobem skonfigurowania modelu jest za pośrednictwem interfejsu API usługi Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="d3c23-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="d3c23-225">Większość konfiguracji modelu można wykonać przy użyciu prostych adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="d3c23-226">Interfejs API Fluent to bardziej zaawansowany sposób określania konfiguracji modelu, która obejmuje wszystkie elementy, które mogą wykonywać adnotacje danych, a także bardziej zaawansowaną konfigurację, która nie jest możliwa w przypadku adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="d3c23-227">Adnotacje danych i interfejs API Fluent mogą być używane razem.</span><span class="sxs-lookup"><span data-stu-id="d3c23-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="d3c23-228">Aby uzyskać dostęp do interfejsu API Fluent, Zastąp metodę OnModelCreating w kontekście DbContext.</span><span class="sxs-lookup"><span data-stu-id="d3c23-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="d3c23-229">Załóżmy, że chcemy zmienić nazwę kolumny, która jest przechowywana przez User. DisplayName w celu wyświetlenia nazwy\_.</span><span class="sxs-lookup"><span data-stu-id="d3c23-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="d3c23-230">Zastąp metodę OnModelCreating na BloggingContext następującym kodem</span><span class="sxs-lookup"><span data-stu-id="d3c23-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="d3c23-231">Użyj polecenia **Add-Migration ChangeDisplayName** , aby przeprowadzić szkielet migracji w celu zastosowania tych zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="d3c23-232">Uruchom polecenie **Update-Database** , aby zastosować nową migrację do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="d3c23-233">Nazwa kolumny DisplayName została zmieniona na nazwę wyświetlaną\_:</span><span class="sxs-lookup"><span data-stu-id="d3c23-233">The DisplayName column is now renamed to display\_name:</span></span>

![Zmieniono nazwę schematu z nazwą wyświetlaną](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="d3c23-235">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d3c23-235">Summary</span></span>

<span data-ttu-id="d3c23-236">W tym instruktażu oglądamy Code First opracowywanie przy użyciu nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="d3c23-237">Definiujemy model przy użyciu klas, a następnie używa tego modelu do tworzenia bazy danych i przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="d3c23-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="d3c23-238">Po utworzeniu bazy danych używamy Migracje Code First, aby zmienić schemat w miarę rozwoju modelu.</span><span class="sxs-lookup"><span data-stu-id="d3c23-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="d3c23-239">Przedstawiono również sposób konfigurowania modelu przy użyciu adnotacji danych i interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d3c23-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
