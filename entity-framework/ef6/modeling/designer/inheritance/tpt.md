---
title: Projektant TPT — dziedziczenie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418211"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="4df1e-102">TPT dziedziczenia projektanta</span><span class="sxs-lookup"><span data-stu-id="4df1e-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="4df1e-103">W tym przewodniku krok po kroku pokazano, jak zaimplementować dziedziczenie według typu (TPT) w modelu przy użyciu Entity Framework Designer (program EF Designer).</span><span class="sxs-lookup"><span data-stu-id="4df1e-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="4df1e-104">Dziedziczenie na poziomie tabeli używa oddzielnej tabeli w bazie danych, aby zachować dane dla właściwości niedziedziczonych i właściwości klucza dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="4df1e-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="4df1e-105">W tym instruktażu zamapujemy **kurs** (typ podstawowy), **OnlineCourse** (pochodzą z kursu) i **OnsiteCourse** (pochodzą od **kursów**) do tabel o tych samych nazwach.</span><span class="sxs-lookup"><span data-stu-id="4df1e-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="4df1e-106">Utworzymy model z bazy danych, a następnie zmienimy model w celu zaimplementowania dziedziczenia TPT.</span><span class="sxs-lookup"><span data-stu-id="4df1e-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="4df1e-107">Możesz również rozpocząć od Model First, a następnie wygenerować bazę danych na podstawie modelu.</span><span class="sxs-lookup"><span data-stu-id="4df1e-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="4df1e-108">Projektant EF domyślnie korzysta z strategii TPT, dlatego każde dziedziczenie w modelu zostanie zmapowane do oddzielnych tabel.</span><span class="sxs-lookup"><span data-stu-id="4df1e-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="4df1e-109">Inne opcje dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="4df1e-109">Other Inheritance Options</span></span>

<span data-ttu-id="4df1e-110">Tabela na hierarchię (TPH) jest innym typem dziedziczenia, w którym jedna tabela bazy danych jest używana do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="4df1e-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="4df1e-111">  Aby uzyskać informacje o sposobach mapowania dziedziczenia na poziomie tabeli przy użyciu Entity Designer, zobacz [dziedziczenie w programie EF Designer TPH](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="4df1e-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="4df1e-112">Należy zauważyć, że w środowisku uruchomieniowym Entity Framework są obsługiwane dziedziczenie typu tabeli dla konkretnych typów (TPC) i modele dziedziczenia mieszanego, ale nie są obsługiwane przez projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="4df1e-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="4df1e-113">Jeśli chcesz użyć programu TPC lub dziedziczenia mieszanego, masz dwie opcje: Użyj Code First lub ręcznie Edytuj plik EDMX.</span><span class="sxs-lookup"><span data-stu-id="4df1e-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="4df1e-114">Jeśli zdecydujesz się pracować z plikiem EDMX, okno Szczegóły mapowania zostanie umieszczone w trybie bezpiecznym i nie będzie można użyć projektanta, aby zmienić mapowania.</span><span class="sxs-lookup"><span data-stu-id="4df1e-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4df1e-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4df1e-115">Prerequisites</span></span>

<span data-ttu-id="4df1e-116">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="4df1e-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="4df1e-117">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4df1e-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="4df1e-118">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="4df1e-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="4df1e-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="4df1e-119">Set up the Project</span></span>

-   <span data-ttu-id="4df1e-120">Open Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="4df1e-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="4df1e-121">Wybierz pozycję **plik —&gt; nowy&gt; projekt**</span><span class="sxs-lookup"><span data-stu-id="4df1e-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="4df1e-122">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="4df1e-123">Wprowadź **TPTDBFirstSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="4df1e-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="4df1e-124">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="4df1e-125">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="4df1e-125">Create a Model</span></span>

-   <span data-ttu-id="4df1e-126">Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="4df1e-127">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="4df1e-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="4df1e-128">W polu Nazwa pliku wprowadź **TPTModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="4df1e-129">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję ** Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="4df1e-130">Kliknij pozycję **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-130">Click **New Connection**.</span></span>
    <span data-ttu-id="4df1e-131">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="4df1e-132">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4df1e-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="4df1e-133">W oknie dialogowym Wybierz obiekty bazy danych w węźle tabele wybierz tabele **dział**, **kurs, OnlineCourse i OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="4df1e-134">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-134">Click **Finish**.</span></span>

<span data-ttu-id="4df1e-135">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="4df1e-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="4df1e-136">Wszystkie obiekty wybrane w oknie dialogowym Wybierz obiekty bazy danych zostaną dodane do modelu.</span><span class="sxs-lookup"><span data-stu-id="4df1e-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="4df1e-137">Implementowanie dziedziczenia na poziomie tabeli</span><span class="sxs-lookup"><span data-stu-id="4df1e-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="4df1e-138">Na powierzchni projektowej kliknij prawym przyciskiem myszy typ jednostki **OnlineCourse** i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="4df1e-139">W oknie **Właściwości** ustaw właściwość typ podstawowy na **kurs**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="4df1e-140">Kliknij prawym przyciskiem myszy typ jednostki **OnsiteCourse** i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="4df1e-141">W oknie **Właściwości** ustaw właściwość typ podstawowy na **kurs**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="4df1e-142">Kliknij prawym przyciskiem myszy skojarzenie (wiersz) między typami jednostek **OnlineCourse** i **kurs** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="4df1e-143">Wybierz pozycję **Usuń z modelu**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="4df1e-144">Kliknij prawym przyciskiem myszy skojarzenie między typami jednostek **OnsiteCourse** i **kurs** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="4df1e-145">Wybierz pozycję **Usuń z modelu**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="4df1e-146">Usuniemy teraz Właściwość **CourseID** z **OnlineCourse** i **OnsiteCourse** , ponieważ te klasy dziedziczą **CourseID** z typu podstawowego **kursu** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="4df1e-147">Kliknij prawym przyciskiem myszy Właściwość **CourseID** typu jednostki **OnlineCourse** , a następnie wybierz pozycję **Usuń z modelu**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="4df1e-148">Kliknij prawym przyciskiem myszy Właściwość **CourseID** typu jednostki **OnsiteCourse** , a następnie wybierz pozycję **Usuń z modelu** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="4df1e-149">Dziedziczenie dla typu tabeli jest teraz zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="4df1e-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="4df1e-151">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="4df1e-151">Use the Model</span></span>

<span data-ttu-id="4df1e-152">Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="4df1e-153">Wklej następujący kod do funkcji **Main** .</span><span class="sxs-lookup"><span data-stu-id="4df1e-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="4df1e-154">Kod wykonuje trzy zapytania.</span><span class="sxs-lookup"><span data-stu-id="4df1e-154">The code executes three queries.</span></span> <span data-ttu-id="4df1e-155">Pierwsze zapytanie powoduje przywrócenie wszystkich **kursów** związanych z określonym działem.</span><span class="sxs-lookup"><span data-stu-id="4df1e-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="4df1e-156">Drugie zapytanie używa metody **OfType** do zwracania **OnlineCourses** związanych z określonym działem.</span><span class="sxs-lookup"><span data-stu-id="4df1e-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="4df1e-157">Trzecie zapytanie zwraca **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="4df1e-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
