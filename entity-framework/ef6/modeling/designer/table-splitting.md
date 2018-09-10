---
title: Tabela projektanta dzielenia - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f07aeb0aa679f6fa8131c667ac808f17c3f03f20
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250988"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="249f1-102">Tabela projektanta dzielenia</span><span class="sxs-lookup"><span data-stu-id="249f1-102">Designer Table Splitting</span></span>
<span data-ttu-id="249f1-103">Ten poradnik pokazuje jak mapować wielu typów jednostek do pojedynczej tabeli, modyfikując model przy użyciu narzędzia Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="249f1-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="249f1-104">Jeden powodów, dla którego chcesz użyć tabeli podział opóźnia ładowanie niektórych właściwości podczas korzystania z opóźnieniem, ładowania do ładowania obiektów.</span><span class="sxs-lookup"><span data-stu-id="249f1-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="249f1-105">Można oddzielić właściwości, które mogą zawierać bardzo duże ilości danych na osobne jednostki i tylko załadować je, gdy jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="249f1-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="249f1-106">Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="249f1-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektancie platformy EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="249f1-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="249f1-108">Prerequisites</span></span>

<span data-ttu-id="249f1-109">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="249f1-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="249f1-110">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="249f1-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="249f1-111">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="249f1-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="249f1-112">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="249f1-112">Set up the Project</span></span>

<span data-ttu-id="249f1-113">Ten przewodnik korzysta z programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="249f1-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="249f1-114">Otwórz program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="249f1-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="249f1-115">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="249f1-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="249f1-116">W okienku po lewej stronie kliknij Visual C\#, a następnie wybierz szablon Aplikacja konsoli.</span><span class="sxs-lookup"><span data-stu-id="249f1-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="249f1-117">Wprowadź **TableSplittingSample** jako nazwę projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="249f1-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="249f1-118">Tworzenie modelu, w oparciu o bazę danych School</span><span class="sxs-lookup"><span data-stu-id="249f1-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="249f1-119">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="249f1-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="249f1-120">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="249f1-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="249f1-121">Wprowadź **TableSplittingModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="249f1-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="249f1-122">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej.**</span><span class="sxs-lookup"><span data-stu-id="249f1-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="249f1-123">Kliknij przycisk nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="249f1-123">Click New Connection.</span></span> <span data-ttu-id="249f1-124">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="249f1-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="249f1-125">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="249f1-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="249f1-126">W oknie dialogowym Wybierz obiekty bazy danych należy ujawniać **tabel** węzła i wyboru **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="249f1-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="249f1-127">Spowoduje to dodanie określonej tabeli, aby **School** modelu.</span><span class="sxs-lookup"><span data-stu-id="249f1-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="249f1-128">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="249f1-128">Click **Finish**.</span></span>

<span data-ttu-id="249f1-129">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="249f1-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="249f1-130">Wszystkie obiekty, które wybrano w **wybierz obiekty bazy danych** okno dialogowe, są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="249f1-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="249f1-131">Dwie jednostki mapy do pojedynczej tabeli</span><span class="sxs-lookup"><span data-stu-id="249f1-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="249f1-132">W tej sekcji zostanie podzielona **osoby** jednostki do dwóch jednostek i zamapować je do jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="249f1-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="249f1-133">**Osoby** jednostka nie zawiera żadnych właściwości, które mogą zawierać dużą ilość danych; jest używany tylko jako przykład.</span><span class="sxs-lookup"><span data-stu-id="249f1-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="249f1-134">Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i kliknij przycisk **jednostki**.</span><span class="sxs-lookup"><span data-stu-id="249f1-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="249f1-135">**Nowa jednostka** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="249f1-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="249f1-136">Typ **HireInfo** dla **nazwa jednostki** i **PersonID** dla **właściwości klucza** nazwy.</span><span class="sxs-lookup"><span data-stu-id="249f1-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="249f1-137">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="249f1-137">Click **OK**.</span></span>
-   <span data-ttu-id="249f1-138">Nowy typ jednostki jest tworzone i wyświetlane na powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="249f1-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="249f1-139">Wybierz **DataZatrudnienia** właściwość **osoby** typu jednostki, a następnie naciśnij klawisz **Ctrl + X** kluczy.</span><span class="sxs-lookup"><span data-stu-id="249f1-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="249f1-140">Wybierz **HireInfo** jednostki, a następnie naciśnij klawisz **klawisze Ctrl + V** kluczy.</span><span class="sxs-lookup"><span data-stu-id="249f1-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="249f1-141">Tworzenie skojarzenia między **osoby** i **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="249f1-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="249f1-142">Aby to zrobić, kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i kliknij przycisk **skojarzenie**.</span><span class="sxs-lookup"><span data-stu-id="249f1-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="249f1-143">**Dodawanie skojarzenia** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="249f1-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="249f1-144">**PersonHireInfo** domyślnie zostanie podana nazwa.</span><span class="sxs-lookup"><span data-stu-id="249f1-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="249f1-145">Określać liczebność **1(One)** po obu stronach relacji.</span><span class="sxs-lookup"><span data-stu-id="249f1-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="249f1-146">Naciśnij klawisz **OK**.</span><span class="sxs-lookup"><span data-stu-id="249f1-146">Press **OK**.</span></span>

<span data-ttu-id="249f1-147">Następny krok wymaga **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="249f1-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="249f1-148">Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **szczegóły mapowania**.</span><span class="sxs-lookup"><span data-stu-id="249f1-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="249f1-149">Wybierz **HireInfo** typu jednostki i kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="249f1-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="249f1-150">Wybierz **osoby** z **&lt;Dodaj tabelę lub widok&gt;** pole listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="249f1-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="249f1-151">Lista zawiera tabele lub widoki, które można mapować wybranej jednostki.</span><span class="sxs-lookup"><span data-stu-id="249f1-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="249f1-152">Odpowiednie właściwości powinny być mapowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="249f1-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapowanie](~/ef6/media/mapping.png)

-   <span data-ttu-id="249f1-154">Wybierz **PersonHireInfo** skojarzenia na powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="249f1-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="249f1-155">Kliknij prawym przyciskiem myszy asocjacji na projekt powierzchni i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="249f1-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="249f1-156">W **właściwości** wybierz **ograniczenia referencyjne** właściwości i kliknij przycisk z wielokropkiem.</span><span class="sxs-lookup"><span data-stu-id="249f1-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="249f1-157">Wybierz **osoby** z **jednostki** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="249f1-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="249f1-158">Naciśnij klawisz **OK**.</span><span class="sxs-lookup"><span data-stu-id="249f1-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="249f1-159">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="249f1-159">Use the Model</span></span>

-   <span data-ttu-id="249f1-160">Wklej następujący kod dla metody Main.</span><span class="sxs-lookup"><span data-stu-id="249f1-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="249f1-161">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="249f1-161">Compile and run the application.</span></span>

<span data-ttu-id="249f1-162">Poniższe instrukcje języka T-SQL zostały wykonane na **School** bazy danych w wyniku działania tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="249f1-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="249f1-163">Następujące **Wstaw** zostało wykonane w wyniku wykonania z kontekstu. SaveChanges() i łączy dane z **osoby** i **HireInfo** jednostek</span><span class="sxs-lookup"><span data-stu-id="249f1-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="249f1-165">Następujące **wybierz** zostało wykonane w wyniku wykonania z kontekstu. People.FirstOrDefault() i wybiera tylko potrzebne kolumny mapowane na **osoby**</span><span class="sxs-lookup"><span data-stu-id="249f1-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Wybierz 1](~/ef6/media/select1.png)

-   <span data-ttu-id="249f1-167">Następujące **wybierz** zostało wykonane w wyniku uzyskiwania dostępu do existingPerson.Instructor właściwość nawigacji z wybiera tylko potrzebne kolumny, które są mapowane na **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="249f1-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Wybierz opcję 2](~/ef6/media/select2.png)
