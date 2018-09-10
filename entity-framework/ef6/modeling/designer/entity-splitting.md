---
title: Dzielenie projektanta jednostek — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250601"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="d892f-102">Projektant jednostki dzielenia</span><span class="sxs-lookup"><span data-stu-id="d892f-102">Designer Entity Splitting</span></span>
<span data-ttu-id="d892f-103">Ten poradnik pokazuje jak do mapowania typu encji dwie tabele, modyfikując model przy użyciu narzędzia Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="d892f-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="d892f-104">Jednostki można zamapować na wiele tabel, tabele udostępniania wspólny klucz.</span><span class="sxs-lookup"><span data-stu-id="d892f-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="d892f-105">Pojęcia, które są stosowane do mapowania typu jednostki z dwiema tabelami są łatwo rozszerzyć mapowania typu jednostki w więcej niż dwie tabele.</span><span class="sxs-lookup"><span data-stu-id="d892f-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="d892f-106">Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d892f-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektancie platformy EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="d892f-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d892f-108">Prerequisites</span></span>

<span data-ttu-id="d892f-109">Program Visual Studio 2012 lub Visual Studio 2010, Ultimate, Premium, Professional lub Web Express edition.</span><span class="sxs-lookup"><span data-stu-id="d892f-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d892f-110">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d892f-110">Create the Database</span></span>

<span data-ttu-id="d892f-111">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="d892f-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d892f-112">Jeśli używasz programu Visual Studio 2012, a następnie będzie utworzenie bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d892f-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="d892f-113">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d892f-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="d892f-114">Najpierw utworzymy bazę danych z dwoma tabelami, które będziemy połączyć do pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="d892f-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="d892f-115">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d892f-115">Open Visual Studio</span></span>
-   <span data-ttu-id="d892f-116">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="d892f-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d892f-117">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="d892f-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d892f-118">Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera, zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="d892f-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="d892f-119">Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany</span><span class="sxs-lookup"><span data-stu-id="d892f-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="d892f-120">Wprowadź **EntitySplitting** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="d892f-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="d892f-121">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="d892f-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="d892f-122">Nowa baza danych będzie teraz wyświetlany w Eksploratorze serwera</span><span class="sxs-lookup"><span data-stu-id="d892f-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="d892f-123">Jeśli używasz programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d892f-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="d892f-124">Kliknij prawym przyciskiem myszy w bazie danych w Eksploratorze serwera i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="d892f-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="d892f-125">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="d892f-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="d892f-126">Jeśli używasz programu Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d892f-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="d892f-127">Wybierz **dane —&gt; języka Transact SQL edytor —&gt; nowego połączenia zapytania...**</span><span class="sxs-lookup"><span data-stu-id="d892f-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="d892f-128">Wprowadź **.\\ SQLEXPRESS** jako nazwę serwera i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="d892f-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="d892f-129">Wybierz **EntitySplitting** bazy danych z listy rozwijanej w górnej części edytora zapytań</span><span class="sxs-lookup"><span data-stu-id="d892f-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="d892f-130">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonaj instrukcję SQL**</span><span class="sxs-lookup"><span data-stu-id="d892f-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="d892f-131">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="d892f-131">Create the Project</span></span>

-   <span data-ttu-id="d892f-132">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d892f-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="d892f-133">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **aplikację Konsolową** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d892f-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="d892f-134">Wprowadź **MapEntityToTablesSample** jako nazwę projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d892f-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="d892f-135">Kliknij przycisk **nie** Jeśli zostanie wyświetlony monit, aby zapisać zapytanie SQL utworzonego w pierwszej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d892f-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="d892f-136">Tworzenie modelu na podstawie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d892f-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="d892f-137">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="d892f-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="d892f-138">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="d892f-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d892f-139">Wprowadź **MapEntityToTablesModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d892f-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d892f-140">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej.**</span><span class="sxs-lookup"><span data-stu-id="d892f-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="d892f-141">Wybierz **EntitySplitting** połączenia z listy w dół i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d892f-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="d892f-142">W oknie dialogowym Wybierz obiekty bazy danych, zaznacz pole wyboru obok pozycji **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="d892f-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="d892f-143">Spowoduje to dodanie wszystkich tabel z **EntitySplitting** bazy danych do modelu.</span><span class="sxs-lookup"><span data-stu-id="d892f-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="d892f-144">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="d892f-144">Click **Finish**.</span></span>

<span data-ttu-id="d892f-145">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="d892f-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="d892f-146">Mapuj jednostki z dwiema tabelami</span><span class="sxs-lookup"><span data-stu-id="d892f-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="d892f-147">W tym kroku zaktualizujemy **osoby** typu jednostki, łączyć dane z **osoby** i **PersonInfo** tabel.</span><span class="sxs-lookup"><span data-stu-id="d892f-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="d892f-148">Wybierz **E-mail** i **Phone** właściwości \*\* PersonInfo \*\* jednostki, a następnie naciśnij klawisz **Ctrl + X** kluczy.</span><span class="sxs-lookup"><span data-stu-id="d892f-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="d892f-149">Wybierz pozycję \*\* osoby \*\* jednostki, a następnie naciśnij klawisz **klawisze Ctrl + V** kluczy.</span><span class="sxs-lookup"><span data-stu-id="d892f-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="d892f-150">Na powierzchni projektowej wybierz **PersonInfo** jednostki, a następnie naciśnij klawisz **Usuń** przycisku na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="d892f-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="d892f-151">Kliknij przycisk **nie** po wyświetleniu monitu, jeśli chcesz usunąć **PersonInfo** tabelę z modelu, firma Microsoft zamiar zamapować na **osoby** jednostki.</span><span class="sxs-lookup"><span data-stu-id="d892f-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Usuwanie tabel](~/ef6/media/deletetables.png)

<span data-ttu-id="d892f-153">Następne kroki wymagają **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="d892f-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="d892f-154">Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **szczegóły mapowania**.</span><span class="sxs-lookup"><span data-stu-id="d892f-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="d892f-155">Wybierz **osoby** typu jednostki i kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="d892f-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d892f-156">Wybierz pozycję \*\* PersonInfo \*\* z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="d892f-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="d892f-157">**Szczegóły mapowania** okno zostanie zaktualizowana przy użyciu domyślnego mapowania kolumn, są w dobrym stanie w naszym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="d892f-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="d892f-158">**Osoby** typ jednostki jest obecnie zmapowane do **osoby** i **PersonInfo** tabel.</span><span class="sxs-lookup"><span data-stu-id="d892f-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapowanie 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="d892f-160">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="d892f-160">Use the Model</span></span>

-   <span data-ttu-id="d892f-161">Wklej następujący kod dla metody Main.</span><span class="sxs-lookup"><span data-stu-id="d892f-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="d892f-162">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d892f-162">Compile and run the application.</span></span>

<span data-ttu-id="d892f-163">Poniższe instrukcje języka T-SQL były wykonywane w bazie danych, w wyniku działania tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d892f-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="d892f-164">Następujące dwa **Wstaw** instrukcje zostały wykonane w wyniku wykonania z kontekstu. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="d892f-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="d892f-165">Przyjmują, dane z **osoby** jednostki i podziel go między **osoby** i **PersonInfo** tabel.</span><span class="sxs-lookup"><span data-stu-id="d892f-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Włóż dysk 1](~/ef6/media/insert1.png)

    ![Wstaw 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="d892f-168">Następujące **wybierz** zostało wykonane w wyniku wyliczanie osób w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d892f-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="d892f-169">Łączy ona dane z **osoby** i **PersonInfo** tabeli.</span><span class="sxs-lookup"><span data-stu-id="d892f-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Wybierz](~/ef6/media/select.png)
