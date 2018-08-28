---
title: Dziedziczenie projektanta TPH - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 9a546f6450b5aa3b03c062d1ab2c6f9257ba8292
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995007"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="59e10-102">Dziedziczenie TPH projektanta</span><span class="sxs-lookup"><span data-stu-id="59e10-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="59e10-103">Ten przewodnik krok po kroku pokazano, jak zaimplementować Tabela wg hierarchii (TPH) dziedziczenia w model koncepcyjny za pomocą funkcji Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="59e10-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="59e10-104">Dziedziczenie TPH używa jednej tabeli bazy danych do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="59e10-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="59e10-105">W ramach tego przewodnika firma Microsoft będzie zmapowana tabeli osób do trzech typów jednostek: osoba (typ podstawowy), uczniów (pochodzi od osoby) i przez instruktorów (pochodzi od osoby).</span><span class="sxs-lookup"><span data-stu-id="59e10-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="59e10-106">Utworzymy model koncepcyjny z bazy danych (Database First) i następnie zmienić model, który ma implementują dziedziczenie TPH przy użyciu projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="59e10-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="59e10-107">Można zamapować na dziedziczenie TPH, za pomocą funkcji First modelu, ale trzeba napisać własne przepływu pracy generowanie bazy danych, którą jest złożony.</span><span class="sxs-lookup"><span data-stu-id="59e10-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="59e10-108">Następnie można przypisać ten przepływ pracy ma **generowanie bazy danych w przepływie pracy** właściwości w Projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="59e10-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="59e10-109">Alternatywą łatwiej jest używać Code First.</span><span class="sxs-lookup"><span data-stu-id="59e10-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="59e10-110">Inne opcje dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="59e10-110">Other Inheritance Options</span></span>

<span data-ttu-id="59e10-111">Tabela wg typu (TPT) jest inny typ dziedziczenia, w którym oddzielnych tabel w bazie danych są mapowane do jednostek, które uczestniczą w dziedziczeniu.</span><span class="sxs-lookup"><span data-stu-id="59e10-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="59e10-112">Aby uzyskać informacje o mapowaniu tabeli na typ dziedziczenia z projektancie platformy EF, zobacz [dziedziczenia TPT projektancie platformy EF](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="59e10-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="59e10-113">Dziedziczenie typu tabeli na konkretny (TPC) i wzory mieszane dziedziczenia są obsługiwane przez środowisko uruchomieniowe programu Entity Framework, ale nie są obsługiwane w Projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="59e10-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="59e10-114">Jeśli chcesz użyć TPC lub mieszane dziedziczenia, masz dwie opcje: Użyj Code First lub ręczna Edycja pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="59e10-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="59e10-115">Jeśli użytkownik chce pracować z pliku EDMX, okno Szczegóły mapowania, które zostaną wprowadzone w "trybie awaryjnym" i nie można zmienić mapowania za pomocą projektanta.</span><span class="sxs-lookup"><span data-stu-id="59e10-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59e10-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="59e10-116">Prerequisites</span></span>

<span data-ttu-id="59e10-117">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="59e10-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="59e10-118">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="59e10-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="59e10-119">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="59e10-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="59e10-120">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="59e10-120">Set up the Project</span></span>

-   <span data-ttu-id="59e10-121">Otwórz program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="59e10-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="59e10-122">Wybierz **plikach&gt; New -&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="59e10-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="59e10-123">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="59e10-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="59e10-124">Wprowadź **TPHDBFirstSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="59e10-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="59e10-125">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="59e10-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="59e10-126">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="59e10-126">Create a Model</span></span>

-   <span data-ttu-id="59e10-127">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="59e10-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="59e10-128">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="59e10-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="59e10-129">Wprowadź **TPHModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="59e10-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="59e10-130">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="59e10-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="59e10-131">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="59e10-131">Click **New Connection**.</span></span>
    <span data-ttu-id="59e10-132">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="59e10-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="59e10-133">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="59e10-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="59e10-134">W oknie dialogowym Wybierz obiekty bazy danych w węźle tabel, wybierz **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="59e10-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="59e10-135">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="59e10-135">Click **Finish**.</span></span>

<span data-ttu-id="59e10-136">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="59e10-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="59e10-137">Wszystkie obiekty, które zostały wybrane w oknie dialogowym Wybierz obiekty bazy danych są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="59e10-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="59e10-138">Oznacza to sposób, w jaki **osoby** tabela wygląda w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="59e10-138">That is how the **Person** table looks in the database.</span></span>

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="59e10-140">Implementowanie Tabela wg hierarchii dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="59e10-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="59e10-141">**Osoby** tabela ma **dyskryminatora** kolumny, która może mieć jedną z dwóch wartości: "Student" i "Instruktora".</span><span class="sxs-lookup"><span data-stu-id="59e10-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="59e10-142">W zależności od wartości **osoby** tabeli zostaną zmapowane do **uczniów** jednostki lub **przez instruktorów** jednostki.</span><span class="sxs-lookup"><span data-stu-id="59e10-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="59e10-143">**Osoby** tabela ma także dwie kolumny **DataZatrudnienia** i **EnrollmentDate**, które muszą być **dopuszczającego wartość null** ponieważ osoba nie może być dla uczniów i pod kierunkiem instruktora w tym samym czasie, (a przynajmniej nie w tym przewodniku).</span><span class="sxs-lookup"><span data-stu-id="59e10-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="59e10-144">Dodaj nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="59e10-144">Add new Entities</span></span>

-   <span data-ttu-id="59e10-145">Dodaj nową jednostkę.</span><span class="sxs-lookup"><span data-stu-id="59e10-145">Add a new entity.</span></span>
    <span data-ttu-id="59e10-146">Aby to zrobić, kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu programu Entity Framework Designer, a następnie wybierz **Add -&gt;jednostki**.</span><span class="sxs-lookup"><span data-stu-id="59e10-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="59e10-147">Typ **przez instruktorów** dla **nazwa jednostki** i wybierz **osoby** z listy rozwijanej dla **typ podstawowy**.</span><span class="sxs-lookup"><span data-stu-id="59e10-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="59e10-148">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="59e10-148">Click **OK**.</span></span>
-   <span data-ttu-id="59e10-149">Dodaj inną nową jednostkę.</span><span class="sxs-lookup"><span data-stu-id="59e10-149">Add another new entity.</span></span> <span data-ttu-id="59e10-150">Typ **uczniów** dla **nazwa jednostki** i wybierz **osoby** z listy rozwijanej dla **typ podstawowy**.</span><span class="sxs-lookup"><span data-stu-id="59e10-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="59e10-151">Dwóch nowych typów jednostki zostały dodane do powierzchni projektowej.</span><span class="sxs-lookup"><span data-stu-id="59e10-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="59e10-152">Strzałka wskazuje z nowych typów jednostek do **osoby** typu jednostki; oznacza to, że **osoby** jest typem podstawowym dla nowych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="59e10-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="59e10-153">Kliknij prawym przyciskiem myszy **DataZatrudnienia** właściwość **osoby** jednostki.</span><span class="sxs-lookup"><span data-stu-id="59e10-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="59e10-154">Wybierz **Wytnij** (lub użyj klawisza Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="59e10-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="59e10-155">Kliknij prawym przyciskiem myszy **przez instruktorów** jednostki, a następnie wybierz pozycję **Wklej** (lub użyj klawisza Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="59e10-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="59e10-156">Kliknij prawym przyciskiem myszy **DataZatrudnienia** właściwości i wybierz pozycję **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="59e10-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="59e10-157">W **właściwości** oknie **Nullable** właściwości **false**.</span><span class="sxs-lookup"><span data-stu-id="59e10-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="59e10-158">Kliknij prawym przyciskiem myszy **EnrollmentDate** właściwość **osoby** jednostki.</span><span class="sxs-lookup"><span data-stu-id="59e10-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="59e10-159">Wybierz **Wytnij** (lub użyj klawisza Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="59e10-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="59e10-160">Kliknij prawym przyciskiem myszy **uczniów** jednostki, a następnie wybierz pozycję **Wklej (lub użyj klawiszy Ctrl-V klucza).**</span><span class="sxs-lookup"><span data-stu-id="59e10-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="59e10-161">Wybierz **EnrollmentDate** właściwości i ustaw **Nullable** właściwości **false**.</span><span class="sxs-lookup"><span data-stu-id="59e10-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="59e10-162">Wybierz **osoby** typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="59e10-162">Select the **Person** entity type.</span></span> <span data-ttu-id="59e10-163">W **właściwości** oknie jego **abstrakcyjne** właściwości **true**.</span><span class="sxs-lookup"><span data-stu-id="59e10-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="59e10-164">Usuń **dyskryminatora** właściwość **osoby**.</span><span class="sxs-lookup"><span data-stu-id="59e10-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="59e10-165">Powodów, dla którego należy usunąć jest szczegółowo opisane w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="59e10-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="59e10-166">Mapuj jednostki</span><span class="sxs-lookup"><span data-stu-id="59e10-166">Map the entities</span></span>

-   <span data-ttu-id="59e10-167">Kliknij prawym przyciskiem myszy **przez instruktorów** i wybierz **mapowania tabeli.**</span><span class="sxs-lookup"><span data-stu-id="59e10-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="59e10-168">W oknie Szczegóły mapowania wybrano jednostki przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="59e10-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="59e10-169">Kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="59e10-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="59e10-170">**&lt;Dodaj tabelę lub widok&gt;** pole staje się listy rozwijanej tabel lub widoków, które można mapować wybranej jednostki.</span><span class="sxs-lookup"><span data-stu-id="59e10-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="59e10-171">Wybierz **osoby** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="59e10-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="59e10-172">**Szczegóły mapowania** okna jest aktualizowane przy użyciu domyślnego mapowania kolumn oraz opcję dodawania warunku.</span><span class="sxs-lookup"><span data-stu-id="59e10-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="59e10-173">Kliknij pozycję  **&lt;Dodaj warunek&gt;**.</span><span class="sxs-lookup"><span data-stu-id="59e10-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="59e10-174">**&lt;Dodaj warunek&gt;** pole staje się listy rozwijanej, kolumn, dla których można ustawić warunki.</span><span class="sxs-lookup"><span data-stu-id="59e10-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="59e10-175">Wybierz **dyskryminatora** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="59e10-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="59e10-176">W **Operator** kolumny **szczegóły mapowania** wybierz = z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="59e10-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="59e10-177">W **wartość/właściwości** wpisz **przez instruktorów**.</span><span class="sxs-lookup"><span data-stu-id="59e10-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="59e10-178">Wynik końcowy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="59e10-178">The end result should look like this:</span></span>

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="59e10-180">Powtórz te kroki dla **uczniów** typu jednostki, ale Utwórz warunek równa **uczniów** wartość.</span><span class="sxs-lookup"><span data-stu-id="59e10-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="59e10-181">*Dlatego Chcieliśmy, aby usunąć **dyskryminatora** właściwość, jest, ponieważ nie można zamapować kolumny tabeli więcej niż jeden raz. Ta kolumna będzie służyć warunkowego mapowania, więc nie można używać właściwości mapowania także. Jedynym sposobem może służyć w obu przypadkach, gdy warunek będzie używać **Is Null** lub **Is Not Null** porównania.*</span><span class="sxs-lookup"><span data-stu-id="59e10-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="59e10-182">Tabela wg hierarchii dziedziczenia jest teraz implementowane.</span><span class="sxs-lookup"><span data-stu-id="59e10-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="59e10-184">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="59e10-184">Use the Model</span></span>

<span data-ttu-id="59e10-185">Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="59e10-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="59e10-186">Wklej następujący kod do **Main** funkcji.</span><span class="sxs-lookup"><span data-stu-id="59e10-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="59e10-187">Kod wykonuje trzy zapytania.</span><span class="sxs-lookup"><span data-stu-id="59e10-187">The code executes three queries.</span></span> <span data-ttu-id="59e10-188">Pierwsze zapytanie powoduje zwrócenie wszystkich **osoby** obiektów.</span><span class="sxs-lookup"><span data-stu-id="59e10-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="59e10-189">Drugie zapytanie używa **OfType** metodę, aby zwrócić **przez instruktorów** obiektów.</span><span class="sxs-lookup"><span data-stu-id="59e10-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="59e10-190">Trzecie zapytanie używa **OfType** metodę, aby zwrócić **uczniów** obiektów.</span><span class="sxs-lookup"><span data-stu-id="59e10-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
