---
title: Wskazówki dotyczące samoobsługowego śledzenia jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181715"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="d12ba-102">Wskazówki dotyczące samośledzenia jednostek</span><span class="sxs-lookup"><span data-stu-id="d12ba-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d12ba-103">Nie zalecamy już korzystania z szablonu samośledzenia jednostek.</span><span class="sxs-lookup"><span data-stu-id="d12ba-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="d12ba-104">Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d12ba-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="d12ba-105">Jeśli aplikacja wymaga pracy z odłączonymi wykresami jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [śledzone jednostki](https://trackableentities.github.io/), które są technologią podobną do samodzielnego śledzenia jednostek, które są opracowywane przez społeczność lub piszą kod niestandardowy przy użyciu interfejsów API śledzenia zmian niskiego poziomu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](https://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="d12ba-106">W tym instruktażu przedstawiono scenariusz, w którym usługa Windows Communication Foundation (WCF) uwidacznia operację zwracającą wykres jednostki.</span><span class="sxs-lookup"><span data-stu-id="d12ba-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="d12ba-107">Następnie aplikacja kliencka operuje na tym grafie i przesyła modyfikacje do operacji usługi, która weryfikuje i zapisuje aktualizacje bazy danych przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d12ba-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="d12ba-108">Przed ukończeniem tego instruktażu upewnij się, że odczytywano stronę [jednostki samośledzenia](index.md) .</span><span class="sxs-lookup"><span data-stu-id="d12ba-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="d12ba-109">W tym instruktażu są wykonywane następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="d12ba-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="d12ba-110">Tworzy bazę danych do uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-110">Creates a database to access.</span></span>
-   <span data-ttu-id="d12ba-111">Tworzy bibliotekę klas, która zawiera model.</span><span class="sxs-lookup"><span data-stu-id="d12ba-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="d12ba-112">Zamienia na szablon generatora jednostek samośledzenia.</span><span class="sxs-lookup"><span data-stu-id="d12ba-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="d12ba-113">Przenosi klasy jednostek do oddzielnego projektu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="d12ba-114">Tworzy usługę WCF, która uwidacznia operacje do wykonywania zapytań i zapisywania jednostek.</span><span class="sxs-lookup"><span data-stu-id="d12ba-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="d12ba-115">Tworzy aplikacje klienckie (konsole i WPF) korzystające z usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="d12ba-116">Będziemy używać Database First w tym instruktażu, ale te same techniki mają jednakową zastosowanie do Model First.</span><span class="sxs-lookup"><span data-stu-id="d12ba-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d12ba-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d12ba-117">Pre-Requisites</span></span>

<span data-ttu-id="d12ba-118">Aby ukończyć ten Instruktaż, potrzebna jest Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d12ba-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="d12ba-119">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d12ba-119">Create a Database</span></span>

<span data-ttu-id="d12ba-120">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d12ba-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d12ba-121">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d12ba-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="d12ba-122">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d12ba-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="d12ba-123">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d12ba-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d12ba-124">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d12ba-124">Open Visual Studio</span></span>
-   <span data-ttu-id="d12ba-125">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="d12ba-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d12ba-126">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d12ba-127">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem **Microsoft SQL Server** jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="d12ba-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="d12ba-128">Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany</span><span class="sxs-lookup"><span data-stu-id="d12ba-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="d12ba-129">Wprowadź **STESample** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="d12ba-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="d12ba-130">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="d12ba-131">Nowa baza danych zostanie wyświetlona w Eksplorator serwera</span><span class="sxs-lookup"><span data-stu-id="d12ba-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="d12ba-132">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d12ba-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="d12ba-133">Kliknij prawym przyciskiem myszy bazę danych w Eksplorator serwera a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="d12ba-134">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="d12ba-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="d12ba-135">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d12ba-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="d12ba-136">Wybierz pozycję **dane —&gt; edytor języka Transact SQL —&gt; nowe połączenie zapytania...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="d12ba-137">Wprowadź **.\\SQLExpress** jako nazwę serwera, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="d12ba-138">Wybierz bazę danych **STESample** z listy rozwijanej w górnej części edytora zapytań.</span><span class="sxs-lookup"><span data-stu-id="d12ba-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="d12ba-139">Skopiuj poniższy kod SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Wykonaj SQL**</span><span class="sxs-lookup"><span data-stu-id="d12ba-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-model"></a><span data-ttu-id="d12ba-140">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="d12ba-140">Create the Model</span></span>

<span data-ttu-id="d12ba-141">Najpierw potrzebny jest projekt, w którym należy umieścić model.</span><span class="sxs-lookup"><span data-stu-id="d12ba-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="d12ba-142">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="d12ba-143">Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **bibliotekę klas**</span><span class="sxs-lookup"><span data-stu-id="d12ba-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="d12ba-144">Wprowadź **STESample** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="d12ba-145">Teraz utworzysz prosty model w programie Dr Designer, aby uzyskać dostęp do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d12ba-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="d12ba-146">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="d12ba-147">Wybierz pozycję **dane** w okienku po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="d12ba-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d12ba-148">Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d12ba-149">Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="d12ba-150">Wprowadź informacje o połączeniu dla bazy danych utworzonej w poprzedniej sekcji</span><span class="sxs-lookup"><span data-stu-id="d12ba-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="d12ba-151">Wprowadź **BloggingContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="d12ba-152">Zaznacz pole wyboru obok pozycji **tabele** i kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="d12ba-153">Zamień na generowanie kodu wklej</span><span class="sxs-lookup"><span data-stu-id="d12ba-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="d12ba-154">Teraz musimy wyłączyć domyślne generowanie kodu i zamianę na jednostki samośledzenia.</span><span class="sxs-lookup"><span data-stu-id="d12ba-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="d12ba-155">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d12ba-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="d12ba-156">Rozwiń węzeł **BloggingModel. edmx** w **Eksplorator rozwiązań** i Usuń **BloggingModel.tt** i **BloggingModel.Context.tt** ,
    *spowoduje to wyłączenie domyślnego generowania kodu* .</span><span class="sxs-lookup"><span data-stu-id="d12ba-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="d12ba-157">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektanta EF i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="d12ba-158">Wybierz pozycję **online** w okienku po lewej stronie i Wyszukaj **Generator wklej**</span><span class="sxs-lookup"><span data-stu-id="d12ba-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="d12ba-159">Wybierz **Generator wklej dla szablonu C\#** , wprowadź **STETemplate** jako nazwę, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="d12ba-160">Pliki **STETemplate.tt** i **STETemplate.Context.tt** są dodawane zagnieżdżone w pliku BloggingModel. edmx</span><span class="sxs-lookup"><span data-stu-id="d12ba-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="d12ba-161">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d12ba-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="d12ba-162">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektanta EF i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="d12ba-163">Zaznacz **kod** w lewym okienku, a następnie **ADO.NET samośledzenia jednostek**</span><span class="sxs-lookup"><span data-stu-id="d12ba-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="d12ba-164">Wprowadź **STETemplate** jako nazwę, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="d12ba-165">Pliki **STETemplate.tt** i **STETemplate.Context.tt** są dodawane bezpośrednio do projektu</span><span class="sxs-lookup"><span data-stu-id="d12ba-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="d12ba-166">Przenoszenie typów jednostek do oddzielnego projektu</span><span class="sxs-lookup"><span data-stu-id="d12ba-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="d12ba-167">Aby korzystać z jednostek samośledzenia, nasza aplikacja kliencka musi mieć dostęp do klas jednostek generowanych przez nasz model.</span><span class="sxs-lookup"><span data-stu-id="d12ba-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="d12ba-168">Ponieważ nie chcemy ujawniać całego modelu aplikacji klienckiej, zamierzamy przenieść klasy jednostek do osobnego projektu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="d12ba-169">Pierwszym krokiem jest zatrzymanie generowania klas jednostki w istniejącym projekcie:</span><span class="sxs-lookup"><span data-stu-id="d12ba-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="d12ba-170">Kliknij prawym przyciskiem myszy pozycję **STETemplate.tt** w **Eksplorator rozwiązań** i wybierz pozycję **Właściwości**</span><span class="sxs-lookup"><span data-stu-id="d12ba-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="d12ba-171">W oknie **Właściwości** Wyczyść **TextTemplatingFileGenerator** z właściwości **CustomTool**</span><span class="sxs-lookup"><span data-stu-id="d12ba-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="d12ba-172">Rozwiń **STETemplate.tt** w **Eksplorator rozwiązań** i Usuń wszystkie pliki zagnieżdżone w tym obszarze</span><span class="sxs-lookup"><span data-stu-id="d12ba-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="d12ba-173">Następnie będziemy dodawać nowy projekt i generować klasy jednostek</span><span class="sxs-lookup"><span data-stu-id="d12ba-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="d12ba-174">**&gt; dodawania&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="d12ba-175">Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **bibliotekę klas**</span><span class="sxs-lookup"><span data-stu-id="d12ba-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="d12ba-176">Wprowadź **STESample. Entities** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="d12ba-177">**Projekt —&gt; dodać istniejącego elementu...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="d12ba-178">Przejdź do folderu projektu **STESample**</span><span class="sxs-lookup"><span data-stu-id="d12ba-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="d12ba-179">Wybierz, aby wyświetlić **wszystkie pliki (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="d12ba-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="d12ba-180">Wybierz plik **STETemplate.tt**</span><span class="sxs-lookup"><span data-stu-id="d12ba-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="d12ba-181">Kliknij strzałkę listy rozwijanej obok przycisku **Dodaj** i wybierz pozycję **Dodaj jako łącze**</span><span class="sxs-lookup"><span data-stu-id="d12ba-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Dodaj połączony szablon](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="d12ba-183">Chcemy również upewnić się, że klasy jednostek są generowane w tej samej przestrzeni nazw co kontekst.</span><span class="sxs-lookup"><span data-stu-id="d12ba-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="d12ba-184">Pozwala to zmniejszyć liczbę instrukcji using, które należy dodać w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d12ba-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="d12ba-185">Kliknij prawym przyciskiem myszy **STETemplate.tt** połączone w **Eksplorator rozwiązań** i wybierz polecenie **Właściwości**</span><span class="sxs-lookup"><span data-stu-id="d12ba-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="d12ba-186">W oknie **Właściwości** Ustaw **przestrzeń nazw niestandardowego narzędzia** na **STESample**</span><span class="sxs-lookup"><span data-stu-id="d12ba-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="d12ba-187">Kod wygenerowany przez szablon wklej musi być odwołaniem do elementu **System. Runtime. Serialization** , aby można było go skompilować.</span><span class="sxs-lookup"><span data-stu-id="d12ba-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="d12ba-188">Ta biblioteka jest wymagana dla atrybutów **DataContract** i **DataMember** programu WCF, które są używane dla serializowanych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d12ba-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="d12ba-189">Kliknij prawym przyciskiem myszy projekt **STESample. Entities** w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj odwołanie...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="d12ba-190">W programie Visual Studio 2012 — zaznacz pole wyboru obok pozycji **System. Runtime. Serialization** i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="d12ba-191">W programie Visual Studio 2010 — wybierz pozycję **System. Runtime. Serialization** i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="d12ba-192">Na koniec projekt z naszym kontekstem musi być odwołaniem do typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d12ba-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="d12ba-193">Kliknij prawym przyciskiem myszy projekt **STESample** w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj odwołanie...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="d12ba-194">W programie Visual Studio 2012 — wybierz **rozwiązanie** z okienka po lewej stronie, zaznacz pole wyboru obok pozycji **STESample. Entities** i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="d12ba-195">W programie Visual Studio 2010 — wybierz kartę **projekty** , wybierz pozycję **STESample. Entities** , a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="d12ba-196">Kolejną opcją przenoszenia typów jednostek do oddzielnego projektu jest przeniesienie pliku szablonu zamiast łączenia go z lokalizacji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="d12ba-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="d12ba-197">W takim przypadku należy zaktualizować zmienną **plik_wejściowy** w szablonie, aby zapewnić ścieżkę względną do pliku edmx (w tym przykładzie będzie to **..\\BloggingModel. edmx**).</span><span class="sxs-lookup"><span data-stu-id="d12ba-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="d12ba-198">Tworzenie usługi WCF</span><span class="sxs-lookup"><span data-stu-id="d12ba-198">Create a WCF Service</span></span>

<span data-ttu-id="d12ba-199">Teraz czas na dodanie usługi WCF w celu udostępnienia naszych danych zostanie rozpoczęty przez utworzenie projektu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="d12ba-200">**&gt; dodawania&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="d12ba-201">Wybierz pozycję **Visual C\#** w lewym okienku, a następnie **aplikację usługi WCF**</span><span class="sxs-lookup"><span data-stu-id="d12ba-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="d12ba-202">Wprowadź **STESample. Service** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="d12ba-203">Dodawanie odwołania do zestawu **System. Data. Entity**</span><span class="sxs-lookup"><span data-stu-id="d12ba-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="d12ba-204">Dodawanie odwołania do projektów **STESample** i **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="d12ba-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="d12ba-205">Musimy skopiować parametry połączenia EF do tego projektu, aby znalazł się on w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d12ba-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="d12ba-206">Otwórz plik **App. config** projektu **STESample **i skopiuj element **connectionStrings**</span><span class="sxs-lookup"><span data-stu-id="d12ba-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="d12ba-207">Wklej element **connectionStrings** jako element podrzędny elementu **konfiguracji** w pliku **Web. config** w projekcie **STESample. Service**</span><span class="sxs-lookup"><span data-stu-id="d12ba-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="d12ba-208">Teraz czas na wdrożenie rzeczywistej usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="d12ba-209">Otwórz **IService1.cs** i Zastąp zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="d12ba-209">Open **IService1.cs** and replace the contents with the following code</span></span>

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

-   <span data-ttu-id="d12ba-210">Otwórz **Service1. svc** i Zastąp zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="d12ba-210">Open **Service1.svc** and replace the contents with the following code</span></span>

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

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="d12ba-211">Korzystanie z usługi z poziomu aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="d12ba-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="d12ba-212">Utwórzmy aplikację konsolową, która używa naszej usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="d12ba-213">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="d12ba-214">Wybierz pozycję **Visual C\#** w okienku po lewej stronie, a następnie **aplikację konsolową**</span><span class="sxs-lookup"><span data-stu-id="d12ba-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="d12ba-215">Wprowadź **STESample. ConsoleTest** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="d12ba-216">Dodaj odwołanie do projektu **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="d12ba-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="d12ba-217">Potrzebujemy odwołania do usługi dla naszej usługi WCF</span><span class="sxs-lookup"><span data-stu-id="d12ba-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="d12ba-218">Kliknij prawym przyciskiem myszy projekt **STESample. ConsoleTest** w **Eksplorator rozwiązań** i wybierz pozycję **Dodaj odwołanie do usługi...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="d12ba-219">Kliknij pozycję **odkryj**</span><span class="sxs-lookup"><span data-stu-id="d12ba-219">Click **Discover**</span></span>
-   <span data-ttu-id="d12ba-220">Wprowadź **BloggingService** jako przestrzeń nazw, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="d12ba-221">Teraz możemy napisać kod, aby korzystać z usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="d12ba-222">Otwórz **program.cs** i Zastąp zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d12ba-222">Open **Program.cs** and replace the contents with the following code.</span></span>

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

<span data-ttu-id="d12ba-223">Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="d12ba-224">Kliknij prawym przyciskiem myszy projekt **STESample. ConsoleTest** w **Eksplorator rozwiązań** i wybierz polecenie **Debuguj-&gt; Uruchom nowe wystąpienie**</span><span class="sxs-lookup"><span data-stu-id="d12ba-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="d12ba-225">Po uruchomieniu aplikacji zobaczysz następujące dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="d12ba-225">You'll see the following output when the application executes.</span></span>

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="d12ba-226">Korzystanie z usługi z aplikacji WPF</span><span class="sxs-lookup"><span data-stu-id="d12ba-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="d12ba-227">Utwórzmy aplikację WPF, która używa naszej usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="d12ba-228">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="d12ba-229">Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **aplikację WPF**</span><span class="sxs-lookup"><span data-stu-id="d12ba-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="d12ba-230">Wprowadź **STESample. WPFTest** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="d12ba-231">Dodaj odwołanie do projektu **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="d12ba-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="d12ba-232">Potrzebujemy odwołania do usługi dla naszej usługi WCF</span><span class="sxs-lookup"><span data-stu-id="d12ba-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="d12ba-233">Kliknij prawym przyciskiem myszy projekt **STESample. WPFTest** w **Eksplorator rozwiązań** i wybierz pozycję **Dodaj odwołanie do usługi...**</span><span class="sxs-lookup"><span data-stu-id="d12ba-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="d12ba-234">Kliknij pozycję **odkryj**</span><span class="sxs-lookup"><span data-stu-id="d12ba-234">Click **Discover**</span></span>
-   <span data-ttu-id="d12ba-235">Wprowadź **BloggingService** jako przestrzeń nazw, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d12ba-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="d12ba-236">Teraz możemy napisać kod, aby korzystać z usługi.</span><span class="sxs-lookup"><span data-stu-id="d12ba-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="d12ba-237">Otwórz **MainWindow. XAML** i Zastąp zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d12ba-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

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

-   <span data-ttu-id="d12ba-238">Otwórz kod związany z MainWindow (**MainWindow.XAML.cs**) i Zastąp zawartość następującym kodem</span><span class="sxs-lookup"><span data-stu-id="d12ba-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

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

<span data-ttu-id="d12ba-239">Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.</span><span class="sxs-lookup"><span data-stu-id="d12ba-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="d12ba-240">Kliknij prawym przyciskiem myszy projekt **STESample. WPFTest** w **Eksplorator rozwiązań** i wybierz polecenie **Debuguj-&gt; Uruchom nowe wystąpienie**</span><span class="sxs-lookup"><span data-stu-id="d12ba-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="d12ba-241">Można manipulować danymi przy użyciu ekranu i zapisywać je za pośrednictwem usługi przy użyciu przycisku **Zapisz**</span><span class="sxs-lookup"><span data-stu-id="d12ba-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Główne okno WPF](~/ef6/media/wpf.png)
