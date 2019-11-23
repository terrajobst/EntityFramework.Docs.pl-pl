---
title: Database First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182451"
---
# <a name="database-first"></a><span data-ttu-id="9ddea-102">Database First</span><span class="sxs-lookup"><span data-stu-id="9ddea-102">Database First</span></span>
<span data-ttu-id="9ddea-103">Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Database First opracowywania przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ddea-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="9ddea-104">Database First umożliwia odtwarzanie modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="9ddea-105">Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9ddea-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="9ddea-106">Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="9ddea-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="9ddea-107">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="9ddea-107">Watch the video</span></span>
<span data-ttu-id="9ddea-108">To wideo zawiera wprowadzenie do Database First tworzenia przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ddea-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="9ddea-109">Database First umożliwia odtwarzanie modelu z istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="9ddea-110">Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9ddea-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="9ddea-111">Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="9ddea-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="9ddea-112">**Przedstawione przez**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="9ddea-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="9ddea-113">**Wideo**: [wmv](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="9ddea-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9ddea-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9ddea-114">Pre-Requisites</span></span>

<span data-ttu-id="9ddea-115">Aby ukończyć ten przewodnik, musisz mieć zainstalowany co najmniej program Visual Studio 2010 lub Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9ddea-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="9ddea-116">W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="9ddea-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="9ddea-117">1. Tworzenie istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="9ddea-117">1. Create an Existing Database</span></span>

<span data-ttu-id="9ddea-118">Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="9ddea-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="9ddea-119">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9ddea-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="9ddea-120">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="9ddea-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="9ddea-121">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="9ddea-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="9ddea-122">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="9ddea-123">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ddea-123">Open Visual Studio</span></span>
-   <span data-ttu-id="9ddea-124">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="9ddea-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="9ddea-125">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="9ddea-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="9ddea-126">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="9ddea-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="9ddea-128">Połącz się z usługą LocalDB lub SQL Express, w zależności od tego, który został zainstalowany, a następnie wprowadź **DatabaseFirst. blog** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="9ddea-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![DF połączenia SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB połączenia DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="9ddea-131">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Okno dialogowe Tworzenie bazy danych](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="9ddea-133">Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="9ddea-134">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="9ddea-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="9ddea-135">2. Utwórz aplikację</span><span class="sxs-lookup"><span data-stu-id="9ddea-135">2. Create the Application</span></span>

<span data-ttu-id="9ddea-136">Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Database First do uzyskiwania dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="9ddea-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="9ddea-137">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ddea-137">Open Visual Studio</span></span>
-   <span data-ttu-id="9ddea-138">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="9ddea-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="9ddea-139">Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**</span><span class="sxs-lookup"><span data-stu-id="9ddea-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="9ddea-140">Wprowadź **DatabaseFirstSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="9ddea-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="9ddea-141">Wybierz **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="9ddea-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="9ddea-142">3. Odtwarzanie modelu</span><span class="sxs-lookup"><span data-stu-id="9ddea-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="9ddea-143">Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="9ddea-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="9ddea-144">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="9ddea-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="9ddea-145">Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="9ddea-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="9ddea-146">Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="9ddea-147">Spowoduje to uruchomienie **kreatora Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="9ddea-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="9ddea-148">Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-148">Select **Generate from Database** and click **Next**</span></span>

    ![Krok 1 Kreatora](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="9ddea-150">Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **BloggingContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Krok 2 Kreatora](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="9ddea-152">Kliknij pole wyboru obok pozycji "tabele", aby zaimportować wszystkie tabele, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="9ddea-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Krok 3 Kreatora](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="9ddea-154">Po zakończeniu procesu odtwarzania nowy model zostanie dodany do projektu i otwarty do wyświetlania w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9ddea-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="9ddea-155">Plik App. config został również dodany do projektu i zawiera szczegółowe informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Model początkowy](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="9ddea-157">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="9ddea-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="9ddea-158">Jeśli pracujesz w programie Visual Studio 2010, należy wykonać kilka dodatkowych kroków, aby przeprowadzić uaktualnienie do najnowszej wersji programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ddea-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="9ddea-159">Aktualizacja jest ważna, ponieważ zapewnia dostęp do ulepszonej powierzchni interfejsu API, która jest znacznie łatwiejsza w użyciu, a także najnowszych poprawek błędów.</span><span class="sxs-lookup"><span data-stu-id="9ddea-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="9ddea-160">Najpierw należy uzyskać najnowszą wersję Entity Framework z narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="9ddea-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="9ddea-161">**Projekt —&gt; zarządzać pakietami NuGet...** 
    , \*Jeśli nie masz opcji **Zarządzaj pakietami NuGet...** należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) \*</span><span class="sxs-lookup"><span data-stu-id="9ddea-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="9ddea-162">Wybierz kartę **online**</span><span class="sxs-lookup"><span data-stu-id="9ddea-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="9ddea-163">Wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="9ddea-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="9ddea-164">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="9ddea-164">Click **Install**</span></span>

<span data-ttu-id="9ddea-165">Następnie musimy wymienić nasz model, aby wygenerować kod, który korzysta z interfejsu API DbContext, który został wprowadzony w nowszych wersjach Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ddea-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="9ddea-166">Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="9ddea-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="9ddea-167">Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**</span><span class="sxs-lookup"><span data-stu-id="9ddea-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="9ddea-168">Wybierz pozycję Dr **5. x DbContext generator dla C\#** , wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Szablon DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="9ddea-170">4. odczytywanie & zapisywania danych</span><span class="sxs-lookup"><span data-stu-id="9ddea-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="9ddea-171">Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="9ddea-172">Klasy, które będą używane do uzyskiwania dostępu do danych są automatycznie generowane na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="9ddea-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="9ddea-173">*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 pliki BloggingModel.tt i BloggingModel.Context.tt będą znajdować się bezpośrednio w projekcie, a nie zagnieżdżone w pliku EDMX.*</span><span class="sxs-lookup"><span data-stu-id="9ddea-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Wygenerowane klasy DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="9ddea-175">Zaimplementuj metodę Main w Program.cs, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="9ddea-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="9ddea-176">Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego bloga.</span><span class="sxs-lookup"><span data-stu-id="9ddea-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="9ddea-177">Następnie używa zapytania LINQ do pobrania wszystkich blogów z bazy danych uporządkowane alfabetycznie według tytułu.</span><span class="sxs-lookup"><span data-stu-id="9ddea-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="9ddea-178">Teraz możesz uruchomić aplikację i przetestować ją.</span><span class="sxs-lookup"><span data-stu-id="9ddea-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="9ddea-179">5. postępowanie z zmianami bazy danych</span><span class="sxs-lookup"><span data-stu-id="9ddea-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="9ddea-180">Teraz można wprowadzić pewne zmiany w naszym schemacie bazy danych, po wprowadzeniu tych zmian należy zaktualizować nasz model, aby odzwierciedlić te zmiany.</span><span class="sxs-lookup"><span data-stu-id="9ddea-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="9ddea-181">Pierwszym krokiem jest wprowadzenie pewnych zmian w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="9ddea-182">Zamierzamy dodać tabelę użytkowników do schematu.</span><span class="sxs-lookup"><span data-stu-id="9ddea-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="9ddea-183">Kliknij prawym przyciskiem myszy bazę danych **DatabaseFirst. Blogi** w Eksplorator serwera i wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="9ddea-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="9ddea-184">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="9ddea-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="9ddea-185">Teraz, gdy schemat został zaktualizowany, należy zaktualizować model o te zmiany.</span><span class="sxs-lookup"><span data-stu-id="9ddea-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="9ddea-186">Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie "Aktualizuj model z bazy danych...", co spowoduje uruchomienie Kreatora aktualizacji</span><span class="sxs-lookup"><span data-stu-id="9ddea-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="9ddea-187">Na karcie dodawanie kreatora aktualizacji zaznacz pole wyboru obok pozycji tabele, co oznacza, że chcemy dodać nowe tabele ze schematu.</span><span class="sxs-lookup"><span data-stu-id="9ddea-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="9ddea-188">*Karta Odśwież przedstawia wszystkie istniejące tabele w modelu, które będą sprawdzane pod kątem zmian podczas aktualizacji. Karty Usuń pokazują wszystkie tabele, które zostały usunięte ze schematu i również zostaną usunięte z modelu w ramach aktualizacji. Informacje dotyczące tych dwóch kart są wykrywane automatycznie i udostępniane wyłącznie do celów informacyjnych. nie można zmienić żadnych ustawień.*</span><span class="sxs-lookup"><span data-stu-id="9ddea-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Kreator odświeżania](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="9ddea-190">Kliknij przycisk Zakończ w Kreatorze aktualizacji</span><span class="sxs-lookup"><span data-stu-id="9ddea-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="9ddea-191">Model zostanie zaktualizowany w celu uwzględnienia nowej jednostki użytkownika, która jest mapowana na tabelę użytkowników dodaną do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Zaktualizowano model](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="9ddea-193">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9ddea-193">Summary</span></span>

<span data-ttu-id="9ddea-194">W tym instruktażu oglądamy Database First opracowywanie, które umożliwiają nam tworzenie modelu w programie Dr Designer na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="9ddea-195">Następnie korzystamy z tego modelu, aby odczytywać i zapisywać dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="9ddea-196">Na koniec Zaktualizowaliśmy model w celu odzwierciedlenia zmian wprowadzonych w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9ddea-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
