---
title: Samodzielnie śledzenia jednostek przewodnik — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: d89c452410d34bea71e8220aae141c3bfca3e1ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490277"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="60f9f-102">Śledzenie własnym wskazówki jednostek</span><span class="sxs-lookup"><span data-stu-id="60f9f-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="60f9f-103">Nie zaleca się przy użyciu szablonu samoobsługowego tracking jednostek.</span><span class="sxs-lookup"><span data-stu-id="60f9f-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="60f9f-104">Tylko będą dostępne do obsługi istniejących aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60f9f-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="60f9f-105">Jeśli aplikacja wymaga pracy z wykresami odłączonych jednostek, należy wziąć pod uwagę inne alternatywy dla takich jak [słupkowych jednostek](http://trackableentities.github.io/), która jest podobna do samoobsługowego-Tracking-jednostek, które jest bardziej aktywnie rozwijany przez technologię Społeczność lub pisanie kodu niestandardowego za pomocą śledzenia interfejsów API zmian niskiego poziomu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="60f9f-106">W tym instruktażu przedstawiono scenariusz, w którym usługi Windows Communication Foundation (WCF) udostępnia operację, która zwraca grafu jednostki.</span><span class="sxs-lookup"><span data-stu-id="60f9f-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="60f9f-107">Następnie aplikacja kliencka manipuluje tego wykresu i przesyła zmiany do operacji usługi, która weryfikuje i zapisuje aktualizacji do bazy danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="60f9f-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="60f9f-108">Przed wykonaniem tego przewodnika, upewnij się, możesz przeczytać [jednostek Self-Tracking](index.md) strony.</span><span class="sxs-lookup"><span data-stu-id="60f9f-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="60f9f-109">W tym przewodniku wykona następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="60f9f-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="60f9f-110">Tworzy bazę danych w celu uzyskania dostępu do.</span><span class="sxs-lookup"><span data-stu-id="60f9f-110">Creates a database to access.</span></span>
-   <span data-ttu-id="60f9f-111">Tworzy bibliotekę klas, który zawiera model.</span><span class="sxs-lookup"><span data-stu-id="60f9f-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="60f9f-112">Zamiany w szablonie Self-Tracking jednostki Generator.</span><span class="sxs-lookup"><span data-stu-id="60f9f-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="60f9f-113">Przenosi klas jednostek do oddzielnego projektu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="60f9f-114">Tworzy usługę WCF, która udostępnia operacje do wykonywania zapytań i zapisać jednostki.</span><span class="sxs-lookup"><span data-stu-id="60f9f-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="60f9f-115">Tworzy klienta aplikacji (konsoli i WPF), które mogą korzystać z usługi.</span><span class="sxs-lookup"><span data-stu-id="60f9f-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="60f9f-116">Użyjemy pierwszej bazy danych w ramach tego przewodnika, ale te same techniki stosuje się jednakowo do pierwszego modelu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="60f9f-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="60f9f-117">Pre-Requisites</span></span>

<span data-ttu-id="60f9f-118">Do przeprowadzenia tego instruktażu potrzebujesz najnowszej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60f9f-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="60f9f-119">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="60f9f-119">Create a Database</span></span>

<span data-ttu-id="60f9f-120">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="60f9f-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="60f9f-121">Jeśli używasz programu Visual Studio 2012, a następnie będzie utworzenie bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="60f9f-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="60f9f-122">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="60f9f-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="60f9f-123">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="60f9f-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="60f9f-124">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60f9f-124">Open Visual Studio</span></span>
-   <span data-ttu-id="60f9f-125">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="60f9f-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="60f9f-126">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="60f9f-127">Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera, zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="60f9f-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="60f9f-128">Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany</span><span class="sxs-lookup"><span data-stu-id="60f9f-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="60f9f-129">Wprowadź **STESample** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="60f9f-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="60f9f-130">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="60f9f-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="60f9f-131">Nowa baza danych będzie teraz wyświetlany w Eksploratorze serwera</span><span class="sxs-lookup"><span data-stu-id="60f9f-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="60f9f-132">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="60f9f-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="60f9f-133">Kliknij prawym przyciskiem myszy w bazie danych w Eksploratorze serwera i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="60f9f-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="60f9f-134">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="60f9f-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="60f9f-135">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="60f9f-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="60f9f-136">Wybierz **dane —&gt; języka Transact SQL edytor —&gt; nowego połączenia zapytania...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="60f9f-137">Wprowadź **.\\ SQLEXPRESS** jako nazwę serwera i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="60f9f-138">Wybierz **STESample** bazy danych z listy rozwijanej w górnej części edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="60f9f-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="60f9f-139">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonaj instrukcję SQL**</span><span class="sxs-lookup"><span data-stu-id="60f9f-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="60f9f-140">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="60f9f-140">Create the Model</span></span>

<span data-ttu-id="60f9f-141">Up, potrzebujemy projektu, aby przełączyć modelu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="60f9f-142">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="60f9f-143">Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="60f9f-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="60f9f-144">Wprowadź **STESample** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="60f9f-145">Teraz utworzymy prosty model w Projektancie platformy EF dostępu do naszej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="60f9f-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="60f9f-146">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="60f9f-147">Wybierz **danych** z okienka po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="60f9f-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="60f9f-148">Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="60f9f-149">Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="60f9f-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="60f9f-150">Wprowadź informacje o połączeniu dla bazy danych, który został utworzony w poprzedniej sekcji</span><span class="sxs-lookup"><span data-stu-id="60f9f-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="60f9f-151">Wprowadź **BloggingContext** jako nazwy parametrów połączenia, a następnie kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="60f9f-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="60f9f-152">Zaznacz pole wyboru obok pozycji **tabel** i kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="60f9f-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="60f9f-153">Przechodź do generowania kodu WKLEJ</span><span class="sxs-lookup"><span data-stu-id="60f9f-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="60f9f-154">Teraz należy wyłączyć domyślne generowanie kodu i wymiany Self-Tracking jednostek.</span><span class="sxs-lookup"><span data-stu-id="60f9f-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="60f9f-155">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="60f9f-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="60f9f-156">Rozwiń **BloggingModel.edmx** w **Eksploratora rozwiązań** i Usuń **BloggingModel.tt** i **BloggingModel.Context.tt** 
     *Spowoduje to wyłączenie generowania kodu domyślnego*</span><span class="sxs-lookup"><span data-stu-id="60f9f-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="60f9f-157">Kliknij prawym przyciskiem myszy pusty obszar w Projektancie platformy EF powierzchni i wybierz **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="60f9f-158">Wybierz **Online** w okienku po lewej stronie i wyszukaj **Generator WKLEJ**</span><span class="sxs-lookup"><span data-stu-id="60f9f-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="60f9f-159">Wybierz **Generator Wklej kod c\#**  szablonu, wprowadź **STETemplate** jako nazwę i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="60f9f-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="60f9f-160">**STETemplate.tt** i **STETemplate.Context.tt** pliki zostaną dodane zagnieżdżony w ramach pliku BloggingModel.edmx</span><span class="sxs-lookup"><span data-stu-id="60f9f-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="60f9f-161">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="60f9f-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="60f9f-162">Kliknij prawym przyciskiem myszy pusty obszar w Projektancie platformy EF powierzchni i wybierz **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="60f9f-163">Wybierz **kodu** z okienka po lewej stronie i następnie **Generator jednostki Self-Tracking ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="60f9f-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="60f9f-164">Wprowadź **STETemplate** jako nazwę i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="60f9f-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="60f9f-165">**STETemplate.tt** i **STETemplate.Context.tt** pliki zostaną dodane bezpośrednio do projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="60f9f-166">Przenieś typów jednostek do oddzielnego projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="60f9f-167">Aby korzystanie z jednostek Self-Tracking naszej aplikacji klienta musi mieć dostęp do klas jednostek wygenerowana przez nasz model.</span><span class="sxs-lookup"><span data-stu-id="60f9f-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="60f9f-168">Ponieważ nie chcemy ujawniać cały model do aplikacji klienckiej będziemy przenosić klas jednostek do oddzielnego projektu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="60f9f-169">Pierwszym krokiem jest zatrzymać generowanie klas jednostek w istniejący projekt:</span><span class="sxs-lookup"><span data-stu-id="60f9f-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="60f9f-170">Kliknij prawym przyciskiem myszy **STETemplate.tt** w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="60f9f-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="60f9f-171">W **właściwości** Wyczyść okno **TextTemplatingFileGenerator** z **CustomTool** właściwości</span><span class="sxs-lookup"><span data-stu-id="60f9f-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="60f9f-172">Rozwiń **STETemplate.tt** w **Eksploratora rozwiązań** i usunąć wszystkie pliki zagnieżdżony w nim</span><span class="sxs-lookup"><span data-stu-id="60f9f-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="60f9f-173">Następnie użyjemy Dodaj nowy projekt i wygenerowania klas obiektów w nim</span><span class="sxs-lookup"><span data-stu-id="60f9f-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="60f9f-174">**Plik —&gt; Add -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="60f9f-175">Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="60f9f-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="60f9f-176">Wprowadź **STESample.Entities** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="60f9f-177">**Projekt —&gt; Dodaj istniejący element...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="60f9f-178">Przejdź do **STESample** folder projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="60f9f-179">Zaznacz, aby wyświetlić **wszystkie pliki (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="60f9f-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="60f9f-180">Wybierz **STETemplate.tt** pliku</span><span class="sxs-lookup"><span data-stu-id="60f9f-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="60f9f-181">Kliknij strzałkę listy rozwijanej obok **Dodaj** i wybrać **Dodaj jako łącze**</span><span class="sxs-lookup"><span data-stu-id="60f9f-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Dodaj połączony szablon](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="60f9f-183">Przedstawimy również upewnij się, że wygenerowanie klas obiektów w tej samej przestrzeni nazw jako kontekst.</span><span class="sxs-lookup"><span data-stu-id="60f9f-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="60f9f-184">Zmniejsza to po prostu liczbę za pomocą instrukcji, które musimy dodać w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60f9f-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="60f9f-185">Kliknij prawym przyciskiem myszy na połączonym **STETemplate.tt** w **Eksploratora rozwiązań** i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="60f9f-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="60f9f-186">W **właściwości** zestaw okna **niestandardowe narzędzie Namespace** do **STESample**</span><span class="sxs-lookup"><span data-stu-id="60f9f-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="60f9f-187">Kod wygenerowany przez szablon WKLEJ należy odwołanie do **System.Runtime.Serialization** w celu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="60f9f-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="60f9f-188">Ta biblioteka jest wymagany w przypadku WCF **DataContract** i **DataMember** atrybuty, które są używane dla typów możliwych do serializacji jednostek.</span><span class="sxs-lookup"><span data-stu-id="60f9f-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="60f9f-189">Kliknij prawym przyciskiem myszy **STESample.Entities** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="60f9f-190">W programie Visual Studio 2012 — zaznacz pole wyboru obok pozycji **System.Runtime.Serialization** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="60f9f-191">W programie Visual Studio 2010 — wybierz **System.Runtime.Serialization** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="60f9f-192">Na koniec projektu z nasz kontekst w nim należy odwołania do typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="60f9f-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="60f9f-193">Kliknij prawym przyciskiem myszy **STESample** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="60f9f-194">W programie Visual Studio 2012 — wybierz **rozwiązania** z okienka po lewej stronie, zaznacz pole wyboru obok pozycji **STESample.Entities** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="60f9f-195">W programie Visual Studio 2010 — wybierz **projektów** zaznacz **STESample.Entities** i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="60f9f-196">Inną opcją przechodzenia typów jednostek do oddzielnego projektu jest można przenieść pliku szablonu, a nie połączenie go z jego domyślnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="60f9f-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="60f9f-197">Jeśli to zrobisz, musisz zaktualizować **wejściowy** zmiennej szablonu, aby podać ścieżkę względną do pliku edmx (w tym przykładzie, która byłaby **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="60f9f-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="60f9f-198">Tworzenie usługi WCF</span><span class="sxs-lookup"><span data-stu-id="60f9f-198">Create a WCF Service</span></span>

<span data-ttu-id="60f9f-199">Teraz nadszedł czas, aby dodać usługi WCF w celu udostępnienia danych, Zaczniemy od utworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="60f9f-200">**Plik —&gt; Add -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="60f9f-201">Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacja usługi WCF**</span><span class="sxs-lookup"><span data-stu-id="60f9f-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="60f9f-202">Wprowadź **STESample.Service** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="60f9f-203">Dodaj odwołanie do **System.Data.Entity** zestawu</span><span class="sxs-lookup"><span data-stu-id="60f9f-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="60f9f-204">Dodaj odwołanie do **STESample** i **STESample.Entities** projektów</span><span class="sxs-lookup"><span data-stu-id="60f9f-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="60f9f-205">Potrzebujemy skopiować parametry połączenia platformy EF do tego projektu, dzięki czemu znajduje się w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="60f9f-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="60f9f-206">Otwórz **App.Config** plik \*\* STESample \*\* projektu i skopiuj **connectionStrings** — element</span><span class="sxs-lookup"><span data-stu-id="60f9f-206">Open the **App.Config** file for the \*\*STESample \*\*project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="60f9f-207">Wklej **connectionStrings** element jako element podrzędny **konfiguracji** elementu **Web.Config** w pliku **STESample.Service** projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="60f9f-208">Teraz nadszedł czas, aby zaimplementować rzeczywistej usługi.</span><span class="sxs-lookup"><span data-stu-id="60f9f-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="60f9f-209">Otwórz **IService1.cs** i zastąp jego zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="60f9f-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="60f9f-210">Otwórz **plik Service1.svc** i zastąp jego zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="60f9f-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="60f9f-211">Korzystanie z usługi z aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="60f9f-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="60f9f-212">Utwórz aplikację konsolową, która korzysta z naszych usług.</span><span class="sxs-lookup"><span data-stu-id="60f9f-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="60f9f-213">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="60f9f-214">Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacji konsoli**</span><span class="sxs-lookup"><span data-stu-id="60f9f-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="60f9f-215">Wprowadź **STESample.ConsoleTest** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="60f9f-216">Dodaj odwołanie do **STESample.Entities** projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="60f9f-217">Potrzebujemy odwołanie do usługi dla usługi WCF</span><span class="sxs-lookup"><span data-stu-id="60f9f-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="60f9f-218">Kliknij prawym przyciskiem myszy **STESample.ConsoleTest** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie do usługi...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="60f9f-219">Kliknij przycisk **odnajdywania**</span><span class="sxs-lookup"><span data-stu-id="60f9f-219">Click **Discover**</span></span>
-   <span data-ttu-id="60f9f-220">Wprowadź **BloggingService** jako przestrzeń nazw i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="60f9f-221">Obecnie firma Microsoft może pisanie kodu w celu korzystania z usługi.</span><span class="sxs-lookup"><span data-stu-id="60f9f-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="60f9f-222">Otwórz **Program.cs** i zastąp jego zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="60f9f-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="60f9f-223">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="60f9f-224">Kliknij prawym przyciskiem myszy **STESample.ConsoleTest** projektu w **Eksploratora rozwiązań** i wybierz **debugowanie -&gt; Uruchom nowe wystąpienie**</span><span class="sxs-lookup"><span data-stu-id="60f9f-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="60f9f-225">Zostaną wyświetlone następujące dane wyjściowe, gdy aplikacja wykonuje.</span><span class="sxs-lookup"><span data-stu-id="60f9f-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="60f9f-226">Korzystanie z usługi z aplikacji WPF</span><span class="sxs-lookup"><span data-stu-id="60f9f-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="60f9f-227">Utworzymy aplikację WPF, która korzysta z naszych usług.</span><span class="sxs-lookup"><span data-stu-id="60f9f-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="60f9f-228">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="60f9f-229">Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacji WPF**</span><span class="sxs-lookup"><span data-stu-id="60f9f-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="60f9f-230">Wprowadź **STESample.WPFTest** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="60f9f-231">Dodaj odwołanie do **STESample.Entities** projektu</span><span class="sxs-lookup"><span data-stu-id="60f9f-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="60f9f-232">Potrzebujemy odwołanie do usługi dla usługi WCF</span><span class="sxs-lookup"><span data-stu-id="60f9f-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="60f9f-233">Kliknij prawym przyciskiem myszy **STESample.WPFTest** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie do usługi...**</span><span class="sxs-lookup"><span data-stu-id="60f9f-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="60f9f-234">Kliknij przycisk **odnajdywania**</span><span class="sxs-lookup"><span data-stu-id="60f9f-234">Click **Discover**</span></span>
-   <span data-ttu-id="60f9f-235">Wprowadź **BloggingService** jako przestrzeń nazw i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="60f9f-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="60f9f-236">Obecnie firma Microsoft może pisanie kodu w celu korzystania z usługi.</span><span class="sxs-lookup"><span data-stu-id="60f9f-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="60f9f-237">Otwórz **MainWindow.xaml** i zastąp jego zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="60f9f-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="60f9f-238">Otwórz kod związany z dla głównego (**MainWindow.xaml.cs**) i zastąp jego zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="60f9f-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="60f9f-239">Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.</span><span class="sxs-lookup"><span data-stu-id="60f9f-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="60f9f-240">Kliknij prawym przyciskiem myszy **STESample.WPFTest** projektu w **Eksploratora rozwiązań** i wybierz **debugowanie -&gt; Uruchom nowe wystąpienie**</span><span class="sxs-lookup"><span data-stu-id="60f9f-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="60f9f-241">Można manipulować danymi na ekranie i zapisać go za pośrednictwem usługi przy użyciu **Zapisz** przycisku</span><span class="sxs-lookup"><span data-stu-id="60f9f-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Okno główne programu WPF](~/ef6/media/wpf.png)
