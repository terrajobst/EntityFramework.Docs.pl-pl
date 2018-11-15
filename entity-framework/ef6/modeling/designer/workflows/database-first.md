---
title: Najpierw — bazy danych platformy EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284086"
---
# <a name="database-first"></a><span data-ttu-id="e8636-102">Najpierw bazy danych</span><span class="sxs-lookup"><span data-stu-id="e8636-102">Database First</span></span>
<span data-ttu-id="e8636-103">W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do tworzenia pierwszej bazy danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="e8636-104">Baza danych najpierw można odtwarzać modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="e8636-105">Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="e8636-106">Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="e8636-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e8636-107">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="e8636-107">Watch the video</span></span>
<span data-ttu-id="e8636-108">To wideo zawiera wprowadzenie do tworzenia pierwszej bazy danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="e8636-109">Baza danych najpierw można odtwarzać modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="e8636-110">Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="e8636-111">Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="e8636-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="e8636-112">**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="e8636-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="e8636-113">**Film wideo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="e8636-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e8636-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e8636-114">Pre-Requisites</span></span>

<span data-ttu-id="e8636-115">Musisz mieć co najmniej programu Visual Studio 2010 lub Visual Studio 2012 są zainstalowane w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="e8636-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="e8636-116">Jeśli używasz programu Visual Studio 2010, należy również mieć [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="e8636-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="e8636-117">1. Utwórz istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e8636-117">1. Create an Existing Database</span></span>

<span data-ttu-id="e8636-118">Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="e8636-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="e8636-119">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="e8636-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="e8636-120">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="e8636-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="e8636-121">Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="e8636-122">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="e8636-123">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8636-123">Open Visual Studio</span></span>
-   <span data-ttu-id="e8636-124">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="e8636-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="e8636-125">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="e8636-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="e8636-126">Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="e8636-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="e8636-128">Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany, a następnie wprowadź **DatabaseFirst.Blogging** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="e8636-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Połączenia programu SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Połączenie LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="e8636-131">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="e8636-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Tworzenie okna dialogowego baza danych](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="e8636-133">Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="e8636-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="e8636-134">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="e8636-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="e8636-135">2. Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e8636-135">2. Create the Application</span></span>

<span data-ttu-id="e8636-136">Aby zachować ich prostotę zamierzamy utworzyć aplikację konsoli podstawowe, która używa pierwszej bazy danych do dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="e8636-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="e8636-137">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8636-137">Open Visual Studio</span></span>
-   <span data-ttu-id="e8636-138">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e8636-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="e8636-139">Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**</span><span class="sxs-lookup"><span data-stu-id="e8636-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="e8636-140">Wprowadź **DatabaseFirstSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="e8636-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="e8636-141">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="e8636-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="e8636-142">3. Model odtwarzania</span><span class="sxs-lookup"><span data-stu-id="e8636-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="e8636-143">Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="e8636-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="e8636-144">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="e8636-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="e8636-145">Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="e8636-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="e8636-146">Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="e8636-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="e8636-147">Spowoduje to uruchomienie **Kreator modelu Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="e8636-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="e8636-148">Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="e8636-148">Select **Generate from Database** and click **Next**</span></span>

    ![Kreator krok 1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="e8636-150">Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, wprowadź **BloggingContext** jako nazwa parametrów połączenia i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="e8636-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Kreator krok 2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="e8636-152">Kliknij pole wyboru obok "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"</span><span class="sxs-lookup"><span data-stu-id="e8636-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Kreator krok 3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="e8636-154">Po zakończeniu procesu odtwarzania nowy model jest dodawane do projektu i otworzona w celu wyświetlania w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="e8636-155">Dodano również pliku App.config do projektu przy użyciu szczegółów połączenia dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Początkowa modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="e8636-157">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="e8636-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="e8636-158">Jeśli pracujesz w programie Visual Studio 2010 istnieją pewne dodatkowe czynności, które należy wykonać uaktualnienie do najnowszej wersji programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="e8636-159">Uaktualnienie jest ważne, ponieważ daje ona dostęp do ulepszone powierzchni interfejsu API, który jest znacznie łatwiejsze do użycia, a także najnowsze poprawki.</span><span class="sxs-lookup"><span data-stu-id="e8636-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="e8636-160">Najpierw up, musimy pobrać najnowszą wersję programu Entity Framework z NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8636-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="e8636-161">\*\*Project —&gt; Zarządzaj pakietami NuGet... \*\* 
     \*Jeśli nie masz \**Zarządzaj pakietami NuGet... \*\* opcji, należy zainstalować [najnowszej wersji pakietu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="e8636-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="e8636-162">Wybierz **Online** kartę</span><span class="sxs-lookup"><span data-stu-id="e8636-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="e8636-163">Wybierz **EntityFramework** pakietu</span><span class="sxs-lookup"><span data-stu-id="e8636-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="e8636-164">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="e8636-164">Click **Install**</span></span>

<span data-ttu-id="e8636-165">Następnie należy zamienić nasz model, aby wygenerować kod, który korzysta z interfejsu API typu DbContext, który został wprowadzony w nowszych wersjach programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8636-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="e8636-166">Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="e8636-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="e8636-167">Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**</span><span class="sxs-lookup"><span data-stu-id="e8636-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="e8636-168">Wybierz EF **5.x Generator DbContext dla języka C\#**, wprowadź **BloggingModel** jako nazwę i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="e8636-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Szablon typu DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="e8636-170">4. Odczytywanie i zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="e8636-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="e8636-171">Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="e8636-172">Klasy użyjemy na potrzeby dostępu do danych są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="e8636-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="e8636-173">*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 BloggingModel.tt i BloggingModel.Context.tt plików będzie bezpośrednio w ramach projektu, a nie zagnieżdżony w pliku EDMX.*</span><span class="sxs-lookup"><span data-stu-id="e8636-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Wygenerowane klasy DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="e8636-175">Implementuje metody Main w pliku Program.cs, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="e8636-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="e8636-176">Ten kod tworzy nowe wystąpienie nasz kontekst i używa go do wstawiania nowego bloga.</span><span class="sxs-lookup"><span data-stu-id="e8636-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="e8636-177">Następnie używa zapytania LINQ, aby pobrać wszystkie blogi z bazy danych uporządkowana w kolejności alfabetycznej według tytułu.</span><span class="sxs-lookup"><span data-stu-id="e8636-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="e8636-178">Możesz teraz uruchomić aplikację i ją przetestować.</span><span class="sxs-lookup"><span data-stu-id="e8636-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="e8636-179">5. Zajmowanie się zmian w bazie danych</span><span class="sxs-lookup"><span data-stu-id="e8636-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="e8636-180">Teraz nadszedł czas, aby wprowadzić pewne zmiany na naszych schemat bazy danych, gdy firma Microsoft wprowadza te zmiany, należy również zaktualizować nasz model, aby odzwierciedlać wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="e8636-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="e8636-181">Pierwszym krokiem jest wprowadzić pewne zmiany schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="e8636-182">Zamierzamy dodać tabelę użytkowników ze schematem.</span><span class="sxs-lookup"><span data-stu-id="e8636-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="e8636-183">Kliknij prawym przyciskiem myszy **DatabaseFirst.Blogging** bazy danych w Eksploratorze serwera i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="e8636-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="e8636-184">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="e8636-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="e8636-185">Teraz, gdy schemat jest aktualizowany, nadszedł czas na aktualizowanie modelu z tymi zmianami.</span><span class="sxs-lookup"><span data-stu-id="e8636-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="e8636-186">Kliknij prawym przyciskiem myszy na puste miejsce w modelu w Projektancie platformy EF i wybierz opcję "Aktualizuj modelu z bazy danych...", co spowoduje uruchomienie Kreatora aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e8636-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="e8636-187">Na karcie Dodaj Kreatora aktualizacji zaznacz pole wyboru obok tabel oznacza to, że chcemy dodać nowe tabele ze schematu.</span><span class="sxs-lookup"><span data-stu-id="e8636-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="e8636-188">*Na karcie odświeżania pokazuje wszystkie istniejące tabele w modelu, który będzie sprawdzany zmian podczas aktualizacji. Usuń kartach wszystkie tabele zostały usunięte ze schematu, które zostaną także usunięte z modelu, w ramach aktualizacji. Informacje dotyczące tych dwóch kart jest wykrywany automatycznie i jest dostępne wyłącznie do celów informacyjnych, nie można zmienić dowolne ustawienia.*</span><span class="sxs-lookup"><span data-stu-id="e8636-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Odśwież Kreatora](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="e8636-190">Kliknij przycisk Zakończ w Kreatorze aktualizacji</span><span class="sxs-lookup"><span data-stu-id="e8636-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="e8636-191">Model został zaktualizowany do uwzględnienia nowej jednostki użytkownika, który jest mapowany do tabeli użytkowników, którą dodaliśmy do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Zaktualizowano modelu](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="e8636-193">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e8636-193">Summary</span></span>

<span data-ttu-id="e8636-194">W tym przewodniku, który przyjrzeliśmy się tworzenie pierwszej bazy danych, które umożliwiło nam Tworzenie modelu w Projektancie platformy EF, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="e8636-195">Następnie użyliśmy tego modelu do odczytu i zapisu pewne dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="e8636-196">Na koniec Zaktualizowaliśmy modelu aby odzwierciedlić zmiany wprowadzone do schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8636-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
