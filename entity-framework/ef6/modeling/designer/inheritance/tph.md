---
title: Projektant TPH — dziedziczenie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418428"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="3f7c3-102">TPH dziedziczenia projektanta</span><span class="sxs-lookup"><span data-stu-id="3f7c3-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="3f7c3-103">W tym przewodniku krok po kroku pokazano, jak zaimplementować dziedziczenie dla hierarchii (TPH) w modelu koncepcyjnym przy użyciu Entity Framework Designer (program EF Designer).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="3f7c3-104">Dziedziczenie TPH używa jednej tabeli bazy danych do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="3f7c3-105">W tym instruktażu zamapujemy tabelę Person na trzy typy jednostek: Person (typ podstawowy), Student (pochodzi od osoby) i instruktora (pochodzi od osoby).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="3f7c3-106">Utworzymy model koncepcyjny na podstawie bazy danych (Database First), a następnie zmienimy model w celu zaimplementowania dziedziczenia TPH przy użyciu narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="3f7c3-107">Istnieje możliwość mapowania na dziedziczenie TPH przy użyciu Model First, ale należy napisać własny przepływ pracy generowania bazy danych, który jest skomplikowany.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="3f7c3-108">Następnie można przypisać ten przepływ pracy do właściwości **przepływu pracy generowania bazy danych** w programie Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="3f7c3-109">Łatwiejszym rozwiązaniem jest użycie Code First.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="3f7c3-110">Inne opcje dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="3f7c3-110">Other Inheritance Options</span></span>

<span data-ttu-id="3f7c3-111">Tabela na typ (TPT) jest innym typem dziedziczenia, w którym oddzielne tabele w bazie danych są mapowane na jednostki, które uczestniczą w dziedziczeniu.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="3f7c3-112"> Aby uzyskać informacje o sposobach mapowania dziedziczenia na typ tabeli przy użyciu narzędzia Dr Designer, zobacz [dziedziczenie w programie EF Designer TPT](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="3f7c3-113">Dziedziczenie typu "tabela-konkretna" (TPC) i modele dziedziczenia mieszanego są obsługiwane przez środowisko uruchomieniowe Entity Framework, ale nie są obsługiwane przez projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="3f7c3-114">Jeśli chcesz użyć programu TPC lub dziedziczenia mieszanego, masz dwie opcje: Użyj Code First lub ręcznie Edytuj plik EDMX.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="3f7c3-115">Jeśli zdecydujesz się pracować z plikiem EDMX, okno Szczegóły mapowania zostanie umieszczone w trybie bezpiecznym i nie będzie można użyć projektanta, aby zmienić mapowania.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f7c3-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3f7c3-116">Prerequisites</span></span>

<span data-ttu-id="3f7c3-117">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="3f7c3-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3f7c3-118">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3f7c3-119">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3f7c3-120">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="3f7c3-120">Set up the Project</span></span>

-   <span data-ttu-id="3f7c3-121">Open Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3f7c3-122">Wybierz pozycję **plik —&gt; nowy&gt; projekt**</span><span class="sxs-lookup"><span data-stu-id="3f7c3-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="3f7c3-123">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="3f7c3-124">Wprowadź **TPHDBFirstSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="3f7c3-125">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="3f7c3-126">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="3f7c3-126">Create a Model</span></span>

-   <span data-ttu-id="3f7c3-127">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="3f7c3-128">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3f7c3-129">W polu Nazwa pliku wprowadź **TPHModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3f7c3-130">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="3f7c3-131">Kliknij pozycję **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-131">Click **New Connection**.</span></span>
    <span data-ttu-id="3f7c3-132">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3f7c3-133">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3f7c3-134">W oknie dialogowym Wybierz obiekty bazy danych w węźle tabele wybierz tabelę **osoba** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="3f7c3-135">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-135">Click **Finish**.</span></span>

<span data-ttu-id="3f7c3-136">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="3f7c3-137">Wszystkie obiekty wybrane w oknie dialogowym Wybierz obiekty bazy danych zostaną dodane do modelu.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="3f7c3-138">Jest to sposób, w jaki tabela **osoba** przeszukuje bazę danych.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-138">That is how the **Person** table looks in the database.</span></span>

![Tabela osób](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="3f7c3-140">Implementowanie dziedziczenia w hierarchii na poziomie tabeli</span><span class="sxs-lookup"><span data-stu-id="3f7c3-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="3f7c3-141">Tabela **osób** ma kolumnę **rozróżniacza** , która może mieć jedną z dwóch wartości: "Student" i "instruktor".</span><span class="sxs-lookup"><span data-stu-id="3f7c3-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="3f7c3-142">W zależności od wartości tabeli **Person** zostanie zamapowana na jednostkę **ucznia** lub jednostkę **instruktora** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="3f7c3-143">Tabela **osób** ma także dwie kolumny: **HireDate** i **EnrollmentDate**, które muszą mieć **wartość null** , ponieważ osoba nie może być studentem i instruktorem w tym samym czasie (co najmniej w tym instruktażu).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="3f7c3-144">Dodaj nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="3f7c3-144">Add new Entities</span></span>

-   <span data-ttu-id="3f7c3-145">Dodaj nową jednostkę.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-145">Add a new entity.</span></span>
    <span data-ttu-id="3f7c3-146">W tym celu kliknij prawym przyciskiem myszy puste miejsce na powierzchni projektowej Entity Framework Designer i wybierz polecenie **dodaj&gt;jednostkę**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="3f7c3-147">Wpisz **instruktora**  **nazwy jednostki** a następnie wybierz pozycję **osoba** z listy rozwijanej dla **typu podstawowego**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="3f7c3-148">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-148">Click **OK**.</span></span>
-   <span data-ttu-id="3f7c3-149">Dodaj kolejną nową jednostkę.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-149">Add another new entity.</span></span> <span data-ttu-id="3f7c3-150">Wpisz  **uczniów** dla **nazwy jednostki** a następnie wybierz pozycję **osoba** z listy rozwijanej **Typ podstawowy**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="3f7c3-151">Dodano dwa nowe typy jednostek do powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="3f7c3-152">Strzałka wskazuje od nowych typów jednostek do **osoby** typ jednostki; oznacza to, że **osoba** jest typem podstawowym dla nowych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="3f7c3-153">Kliknij prawym przyciskiem myszy Właściwość **HireDate** jednostki  **osoby** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="3f7c3-154">Wybierz pozycję **Wytnij** (lub użyj klawisza Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="3f7c3-155">Kliknij prawym przyciskiem myszy jednostkę  **instruktora** i wybierz polecenie **Wklej** (lub użyj klawisza Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="3f7c3-156">Kliknij prawym przyciskiem myszy Właściwość **HireDate** i wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="3f7c3-157">W oknie **właściwości** ustaw właściwość **null** na **wartość false**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="3f7c3-158">Kliknij prawym przyciskiem myszy Właściwość **EnrollmentDate** jednostki  **osoby** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="3f7c3-159">Wybierz pozycję **Wytnij** (lub użyj klawisza Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="3f7c3-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="3f7c3-160">Kliknij prawym przyciskiem myszy jednostkę **studenta** i wybierz polecenie **Wklej (lub użyj klawisza Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="3f7c3-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="3f7c3-161">Wybierz właściwość **EnrollmentDate** i ustaw właściwość  **nullable** na **wartość false**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="3f7c3-162">Wybierz typ jednostki  **osoby** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-162">Select the **Person** entity type.</span></span> <span data-ttu-id="3f7c3-163">W oknie **właściwości** Ustaw dla jego właściwości **abstract**  **wartość true**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="3f7c3-164">Usuń właściwość **rozróżniacza** z **osoby**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="3f7c3-165">Powód, który powinien zostać usunięty, został wyjaśniony w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="3f7c3-166">Mapowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="3f7c3-166">Map the entities</span></span>

-   <span data-ttu-id="3f7c3-167">Kliknij prawym przyciskiem myszy **instruktora** i wybierz pozycję **Mapowanie tabeli.**</span><span class="sxs-lookup"><span data-stu-id="3f7c3-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="3f7c3-168">W oknie Szczegóły mapowania została wybrana jednostka instruktora.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="3f7c3-169">Kliknij **&lt;dodać tabelę lub widok&gt;**  w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="3f7c3-170"> **&lt;dodać tabelę lub widok&gt;**  stanie się listy rozwijanej tabel lub widoków, do których można zamapować wybraną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="3f7c3-171">Z listy rozwijanej wybierz pozycję **osoba** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="3f7c3-172">Okno **szczegóły mapowania** zostało zaktualizowane z domyślnymi mapowaniami kolumn i opcją dodawania warunku.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="3f7c3-173">Kliknij **&lt;dodać warunek&gt;** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="3f7c3-174"> **&lt;dodać warunek&gt;**  stanie się listy rozwijanej kolumn, dla których można ustawić warunki.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="3f7c3-175">Z listy rozwijanej wybierz pozycję **rozróżniacz** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="3f7c3-176">W kolumnie  **operatora** okna **szczegóły mapowania** wybierz pozycję = z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="3f7c3-177">W kolumnie **wartość/właściwość** wpisz **instruktor**.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="3f7c3-178">Wynik końcowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3f7c3-178">The end result should look like this:</span></span>

    ![Szczegóły mapowania](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="3f7c3-180">Powtórz te kroki dla **ucznia** typ jednostki, ale Oznacz warunek jako wartość **ucznia** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="3f7c3-181">*Powód usunięcia właściwości **rozróżniacza** jest spowodowany tym, że nie można zmapować kolumny tabeli więcej niż raz. Ta kolumna będzie używana na potrzeby mapowania warunkowego, dlatego nie można jej używać również do mapowania właściwości. Jedynym sposobem użycia dla obu, jeśli warunek używa elementu **is o wartości null** lub **nie jest wartością null** porównaniem.*</span><span class="sxs-lookup"><span data-stu-id="3f7c3-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="3f7c3-182">Dziedziczenie na poziomie tabeli jest teraz zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH końcowy](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="3f7c3-184">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="3f7c3-184">Use the Model</span></span>

<span data-ttu-id="3f7c3-185">Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="3f7c3-186">Wklej następujący kod do funkcji **Main** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="3f7c3-187">Kod wykonuje trzy zapytania.</span><span class="sxs-lookup"><span data-stu-id="3f7c3-187">The code executes three queries.</span></span> <span data-ttu-id="3f7c3-188">Pierwsze zapytanie powoduje przywrócenie wszystkich obiektów **osób** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="3f7c3-189">Drugie zapytanie używa metody **OfType** do zwracania obiektów **instruktora** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="3f7c3-190">Trzecie zapytanie używa metody **OfType** do zwracania obiektów **ucznia** .</span><span class="sxs-lookup"><span data-stu-id="3f7c3-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
