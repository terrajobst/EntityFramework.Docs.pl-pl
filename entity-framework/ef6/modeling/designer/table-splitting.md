---
title: Dzielenie tabeli projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418169"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="5daf9-102">Dzielenie tabeli projektanta</span><span class="sxs-lookup"><span data-stu-id="5daf9-102">Designer Table Splitting</span></span>
<span data-ttu-id="5daf9-103">W tym instruktażu pokazano, jak zmapować wiele typów jednostek do pojedynczej tabeli, modyfikując model przy użyciu Entity Framework Designer (Dr Designer).</span><span class="sxs-lookup"><span data-stu-id="5daf9-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="5daf9-104">Jednym z powodów użycia dzielenia tabeli jest opóźnienie ładowania niektórych właściwości w przypadku ładowania obiektów przy użyciu opóźnionych obciążeń.</span><span class="sxs-lookup"><span data-stu-id="5daf9-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="5daf9-105"> Możesz oddzielić właściwości, które mogą zawierać bardzo dużą ilość danych w osobnej jednostce i ładować ją tylko wtedy, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="5daf9-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="5daf9-106">Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="5daf9-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektant EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="5daf9-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5daf9-108">Prerequisites</span></span>

<span data-ttu-id="5daf9-109">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="5daf9-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="5daf9-110">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5daf9-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="5daf9-111">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5daf9-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5daf9-112">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="5daf9-112">Set up the Project</span></span>

<span data-ttu-id="5daf9-113">W tym instruktażu jest używany program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5daf9-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="5daf9-114">Open Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5daf9-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="5daf9-115">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="5daf9-116">W lewym okienku kliknij pozycję Visual C\#, a następnie wybierz szablon Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="5daf9-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="5daf9-117">Wprowadź **TableSplittingSample** jako nazwę projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="5daf9-118">Tworzenie modelu opartego na bazie danych szkoły</span><span class="sxs-lookup"><span data-stu-id="5daf9-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="5daf9-119">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="5daf9-120">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="5daf9-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5daf9-121">W polu Nazwa pliku wprowadź **TableSplittingModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5daf9-122">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **Dalej.**</span><span class="sxs-lookup"><span data-stu-id="5daf9-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="5daf9-123">Kliknij pozycję nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="5daf9-123">Click New Connection.</span></span> <span data-ttu-id="5daf9-124">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="5daf9-125">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5daf9-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="5daf9-126">W oknie dialogowym Wybierz obiekty bazy danych unfold **tabele** węzła i sprawdź tabelę **Person** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="5daf9-127">Spowoduje to dodanie określonej tabeli do modelu **szkoły** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="5daf9-128">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-128">Click **Finish**.</span></span>

<span data-ttu-id="5daf9-129">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="5daf9-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="5daf9-130">Wszystkie obiekty wybrane w oknie dialogowym **Wybierz obiekty bazy danych** są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="5daf9-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="5daf9-131">Mapuj dwie jednostki na jedną tabelę</span><span class="sxs-lookup"><span data-stu-id="5daf9-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="5daf9-132">W tej sekcji podzielę jednostkę **osoby** na dwie jednostki, a następnie mapując je do pojedynczej tabeli.</span><span class="sxs-lookup"><span data-stu-id="5daf9-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="5daf9-133">Jednostka **osoby** nie zawiera żadnych właściwości, które mogą zawierać dużą ilość danych; jest on właśnie używany jako przykład.</span><span class="sxs-lookup"><span data-stu-id="5daf9-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="5daf9-134">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowe**, a następnie kliknij pozycję **Jednostka**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="5daf9-135">Zostanie wyświetlone okno dialogowe **Nowa jednostka** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="5daf9-136">Wpisz **HireInfo** dla **nazwy obiektu** i **PersonID** dla nazwy **właściwości klucza** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="5daf9-137">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-137">Click **OK**.</span></span>
-   <span data-ttu-id="5daf9-138">Nowy typ jednostki jest tworzony i wyświetlany na powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="5daf9-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="5daf9-139">Wybierz właściwość **HireDate**  **osoby** typ jednostki i naciśnij **kombinację klawiszy Ctrl + X** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="5daf9-140">Wybierz jednostkę **HireInfo** i naciśnij klawisze **Ctrl + V** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="5daf9-141">Utwórz skojarzenie między **osobą** i **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="5daf9-142">W tym celu kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowe**, a następnie kliknij pozycję **skojarzenie**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="5daf9-143">Zostanie wyświetlone okno dialogowe **Dodawanie skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="5daf9-144">Nazwa **PersonHireInfo** jest domyślnie określona.</span><span class="sxs-lookup"><span data-stu-id="5daf9-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="5daf9-145">Określ liczebność **1 (jeden)** na obu końcach relacji.</span><span class="sxs-lookup"><span data-stu-id="5daf9-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="5daf9-146">Naciśnij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-146">Press **OK**.</span></span>

<span data-ttu-id="5daf9-147">Następny krok wymaga okna **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="5daf9-148">Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **szczegóły mapowania**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="5daf9-149">Wybierz typ jednostki  **HireInfo** , a następnie kliknij pozycję **&lt;Dodaj tabelę lub widok&gt;**  w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="5daf9-150">Wybierz **z** listy rozwijanej pole **&lt;Dodaj tabelę lub widok&gt;**  .</span><span class="sxs-lookup"><span data-stu-id="5daf9-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="5daf9-151">Lista zawiera tabele lub widoki, do których można zamapować wybraną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="5daf9-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="5daf9-152">Odpowiednie właściwości powinny być domyślnie mapowane.</span><span class="sxs-lookup"><span data-stu-id="5daf9-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapowanie](~/ef6/media/mapping.png)

-   <span data-ttu-id="5daf9-154">Wybierz skojarzenie **PersonHireInfo** na powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="5daf9-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="5daf9-155">Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="5daf9-156">W oknie **Właściwości** wybierz właściwość **ograniczenia referencyjne** i kliknij przycisk wielokropka.</span><span class="sxs-lookup"><span data-stu-id="5daf9-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="5daf9-157">Wybierz **osobę** z listy rozwijanej **główna** .</span><span class="sxs-lookup"><span data-stu-id="5daf9-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="5daf9-158">Naciśnij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daf9-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="5daf9-159">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="5daf9-159">Use the Model</span></span>

-   <span data-ttu-id="5daf9-160">Wklej następujący kod w metodzie Main.</span><span class="sxs-lookup"><span data-stu-id="5daf9-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="5daf9-161">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5daf9-161">Compile and run the application.</span></span>

<span data-ttu-id="5daf9-162">Następujące instrukcje języka T-SQL zostały wykonane względem bazy danych **szkoły** w wyniku uruchomienia tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5daf9-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="5daf9-163">Następujący element **INSERT** został wykonany w wyniku wykonywania kontekstu. Metody SaveChanges () i łączy dane od **osoby** i jednostek **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="5daf9-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="5daf9-165">Następujący **wybór** został wykonany w wyniku wykonywania kontekstu. **Persons** . FirstOrDefault () i wybiera tylko kolumny mapowane do osoby</span><span class="sxs-lookup"><span data-stu-id="5daf9-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Wybierz 1](~/ef6/media/select1.png)

-   <span data-ttu-id="5daf9-167">Następujący **wybór** został wykonany w wyniku uzyskania dostępu do właściwości nawigacji ExistingPerson. instruktor i wybiera tylko kolumny mapowane na **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="5daf9-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Wybierz 2](~/ef6/media/select2.png)
