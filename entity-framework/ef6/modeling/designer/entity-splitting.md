---
title: Dzielenie jednostek projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418463"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="75495-102">Dzielenie jednostek projektanta</span><span class="sxs-lookup"><span data-stu-id="75495-102">Designer Entity Splitting</span></span>
<span data-ttu-id="75495-103">W tym instruktażu pokazano, jak zmapować typ jednostki na dwie tabele, modyfikując model przy użyciu Entity Framework Designer (Dr Designer).</span><span class="sxs-lookup"><span data-stu-id="75495-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="75495-104">Możesz zmapować jednostkę na wiele tabel, gdy tabele korzystają ze wspólnego klucza.</span><span class="sxs-lookup"><span data-stu-id="75495-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="75495-105">Koncepcje, które mają zastosowanie do mapowania typu jednostki na dwie tabele, można łatwo rozszerzyć w celu mapowania typu jednostki na więcej niż dwie tabele.</span><span class="sxs-lookup"><span data-stu-id="75495-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="75495-106">Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="75495-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektant EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="75495-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="75495-108">Prerequisites</span></span>

<span data-ttu-id="75495-109">Visual Studio 2012 lub Visual Studio 2010, Ultimate, Premium, Professional lub Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="75495-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="75495-110">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="75495-110">Create the Database</span></span>

<span data-ttu-id="75495-111">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="75495-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="75495-112">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="75495-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="75495-113">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="75495-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="75495-114">Najpierw utworzymy bazę danych z dwiema tabelami, które zostaną połączone w jedną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="75495-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="75495-115">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75495-115">Open Visual Studio</span></span>
-   <span data-ttu-id="75495-116">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="75495-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="75495-117">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="75495-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="75495-118">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem **Microsoft SQL Server** jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="75495-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="75495-119">Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany</span><span class="sxs-lookup"><span data-stu-id="75495-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="75495-120">Wprowadź **EntitySplitting** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="75495-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="75495-121">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="75495-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="75495-122">Nowa baza danych zostanie wyświetlona w Eksplorator serwera</span><span class="sxs-lookup"><span data-stu-id="75495-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="75495-123">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="75495-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="75495-124">Kliknij prawym przyciskiem myszy bazę danych w Eksplorator serwera a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="75495-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="75495-125">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="75495-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="75495-126">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="75495-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="75495-127">Wybierz pozycję **dane —&gt; edytor języka Transact SQL —&gt; nowe połączenie zapytania...**</span><span class="sxs-lookup"><span data-stu-id="75495-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="75495-128">Wprowadź **.\\SQLExpress** jako nazwę serwera, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="75495-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="75495-129">Wybierz bazę danych **EntitySplitting** z listy rozwijanej w górnej części edytora zapytań.</span><span class="sxs-lookup"><span data-stu-id="75495-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="75495-130">Skopiuj poniższy kod SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Wykonaj SQL**</span><span class="sxs-lookup"><span data-stu-id="75495-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="75495-131">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="75495-131">Create the Project</span></span>

-   <span data-ttu-id="75495-132">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="75495-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="75495-133">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **Aplikacja konsolowa** .</span><span class="sxs-lookup"><span data-stu-id="75495-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="75495-134">Wprowadź **MapEntityToTablesSample** jako nazwę projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="75495-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="75495-135">Kliknij przycisk **nie** , jeśli zostanie wyświetlony monit o zapisanie zapytania SQL utworzonego w pierwszej sekcji.</span><span class="sxs-lookup"><span data-stu-id="75495-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="75495-136">Tworzenie modelu opartego na bazie danych</span><span class="sxs-lookup"><span data-stu-id="75495-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="75495-137">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="75495-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="75495-138">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="75495-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="75495-139">W polu Nazwa pliku wprowadź **MapEntityToTablesModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="75495-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="75495-140">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **Dalej.**</span><span class="sxs-lookup"><span data-stu-id="75495-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="75495-141">Wybierz połączenie **EntitySplitting** z listy rozwijanej i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="75495-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="75495-142">W oknie dialogowym Wybierz obiekty bazy danych zaznacz pole wyboru obok pozycji **tabele** węźle.</span><span class="sxs-lookup"><span data-stu-id="75495-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="75495-143">Spowoduje to dodanie wszystkich tabel z bazy danych **EntitySplitting** do modelu.</span><span class="sxs-lookup"><span data-stu-id="75495-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="75495-144">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="75495-144">Click **Finish**.</span></span>

<span data-ttu-id="75495-145">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="75495-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="75495-146">Mapowanie jednostki na dwie tabele</span><span class="sxs-lookup"><span data-stu-id="75495-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="75495-147">W tym kroku będziemy aktualizować typ podmiotu **osoby** , aby łączyć dane z **osób** i tabel **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="75495-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="75495-148">Wybierz  **e-mail** i właściwości **telefonu** jednostki **PersonInfo **i naciśnij klawisze **Ctrl + X** .</span><span class="sxs-lookup"><span data-stu-id="75495-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="75495-149">Wybierz jednostkę **osoby **i naciśnij klawisze **Ctrl + V** .</span><span class="sxs-lookup"><span data-stu-id="75495-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="75495-150">Na powierzchni projektowej wybierz jednostkę **PersonInfo** i naciśnij przycisk **Usuń** na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="75495-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="75495-151">Kliknij przycisk **nie** , gdy zostanie wyświetlony monit o usunięcie tabeli **PersonInfo** z modelu, zamierzamy zamapować ją na jednostkę **osoby** .</span><span class="sxs-lookup"><span data-stu-id="75495-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Usuń tabele](~/ef6/media/deletetables.png)

<span data-ttu-id="75495-153">Następne kroki wymagają okna **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="75495-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="75495-154">Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **szczegóły mapowania**.</span><span class="sxs-lookup"><span data-stu-id="75495-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="75495-155">Wybierz typ jednostki  **osoba** , a następnie kliknij pozycję **&lt;Dodaj tabelę lub widok&gt;**  w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="75495-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="75495-156">Z listy rozwijanej wybierz pozycję **PersonInfo ** .</span><span class="sxs-lookup"><span data-stu-id="75495-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="75495-157">Okno **szczegóły mapowania** zostało zaktualizowane z domyślnymi mapowaniami kolumn, ale są one bardzo szczegółowe w naszym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="75495-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="75495-158">Typ jednostki  **osoby** jest teraz mapowany do tabel **osoby** i **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="75495-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapowanie 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="75495-160">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="75495-160">Use the Model</span></span>

-   <span data-ttu-id="75495-161">Wklej następujący kod w metodzie Main.</span><span class="sxs-lookup"><span data-stu-id="75495-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="75495-162">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="75495-162">Compile and run the application.</span></span>

<span data-ttu-id="75495-163">Następujące instrukcje języka T-SQL zostały wykonane względem bazy danych w wyniku uruchomienia tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75495-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="75495-164">Dwie poniższe instrukcje **INSERT** zostały wykonane w wyniku wykonywania kontekstu. Metody SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="75495-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="75495-165">Pobierają one dane z podmiotu **osoby** i dzielą je między **osoby** i tabele **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="75495-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Wstaw 1](~/ef6/media/insert1.png)

    ![Wstaw 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="75495-168">Następujący **wybór** został wykonany w wyniku wyliczenia osób w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="75495-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="75495-169">Łączy dane od **osoby** i tabeli **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="75495-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Wybierz pozycję](~/ef6/media/select.png)
