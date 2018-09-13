---
title: Dziedziczenie projektanta TPT - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489456"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="3d9e3-102">Dziedziczenie TPT projektanta</span><span class="sxs-lookup"><span data-stu-id="3d9e3-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="3d9e3-103">Ten przewodnik krok po kroku pokazano, jak zaimplementować Tabela wg typu (TPT) dziedziczenia w modelu przy użyciu narzędzia Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="3d9e3-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="3d9e3-104">Tabela wg typu dziedziczenia używa osobnej tabeli w bazie danych do przechowywania danych dla właściwości dziedziczone i właściwości klucza dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="3d9e3-105">W tym przewodniku, firma Microsoft będzie zmapowana **kurs** (typ podstawowy) **OnlineCourse** (pochodzi z kursu) i **OnsiteCourse** (pochodzi od klasy **kurs**) jednostki do tabel z takich samych nazwach.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="3d9e3-106">Utworzymy model z bazy danych i następnie zmienić model, który ma implementują dziedziczenie TPT.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="3d9e3-107">Można również uruchomić z pierwszego modelu i następnie wygenerować bazę danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="3d9e3-108">Projektancie platformy EF używa strategii TPT domyślnie, a więc wszystkie dziedziczenia w modelu będą mapowane do oddzielnych tabel.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="3d9e3-109">Inne opcje dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="3d9e3-109">Other Inheritance Options</span></span>

<span data-ttu-id="3d9e3-110">Tabela wg hierarchii (TPH) jest inny typ dziedziczenia, w której jedna baza danych tabela jest używana do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="3d9e3-111">Aby uzyskać informacje o mapowaniu Tabela wg hierarchii dziedziczenia z Projektanta obiektów, zobacz [dziedziczenia TPH projektancie platformy EF](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="3d9e3-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="3d9e3-112">Należy pamiętać, że tabeli na na konkretny typ dziedziczenia (TPC) i dziedziczenie mieszane modeli są obsługiwane przez środowisko uruchomieniowe programu Entity Framework, ale nie są obsługiwane w Projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="3d9e3-113">Jeśli chcesz użyć TPC lub mieszane dziedziczenia, masz dwie opcje: Użyj Code First lub ręczna Edycja pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="3d9e3-114">Jeśli użytkownik chce pracować z pliku EDMX, okno Szczegóły mapowania, które zostaną wprowadzone w "trybie awaryjnym" i nie można zmienić mapowania za pomocą projektanta.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d9e3-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3d9e3-115">Prerequisites</span></span>

<span data-ttu-id="3d9e3-116">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="3d9e3-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3d9e3-117">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3d9e3-118">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3d9e3-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3d9e3-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="3d9e3-119">Set up the Project</span></span>

-   <span data-ttu-id="3d9e3-120">Otwórz program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3d9e3-121">Wybierz **plikach&gt; New -&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="3d9e3-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="3d9e3-122">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="3d9e3-123">Wprowadź **TPTDBFirstSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="3d9e3-124">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="3d9e3-125">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="3d9e3-125">Create a Model</span></span>

-   <span data-ttu-id="3d9e3-126">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="3d9e3-127">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3d9e3-128">Wprowadź **TPTModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3d9e3-129">W oknie dialogowym Wybierz zawartość modelu, wybierz pozycję \*\* Generuj z bazy danych, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-129">In the Choose Model Contents dialog box, select\*\* Generate from database\*\*, and then click **Next**.</span></span>
-   <span data-ttu-id="3d9e3-130">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-130">Click **New Connection**.</span></span>
    <span data-ttu-id="3d9e3-131">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3d9e3-132">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3d9e3-133">W oknie dialogowym Wybierz obiekty bazy danych w węźle tabel, wybierz **działu**, **kursu, OnlineCourse i OnsiteCourse** tabel.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="3d9e3-134">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-134">Click **Finish**.</span></span>

<span data-ttu-id="3d9e3-135">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="3d9e3-136">Wszystkie obiekty, które zostały wybrane w oknie dialogowym Wybierz obiekty bazy danych są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="3d9e3-137">Implementowanie dziedziczenia tabelę według typu</span><span class="sxs-lookup"><span data-stu-id="3d9e3-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="3d9e3-138">Na powierzchni projektowej kliknij prawym przyciskiem myszy **OnlineCourse** typu jednostki, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="3d9e3-139">W **właściwości** okna, ustaw właściwość typu Base **kurs**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="3d9e3-140">Kliknij prawym przyciskiem myszy **OnsiteCourse** typu jednostki, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="3d9e3-141">W **właściwości** okna, ustaw właściwość typu Base **kurs**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="3d9e3-142">Kliknij prawym przyciskiem myszy skojarzenia (wiersz) między **OnlineCourse** i **kurs** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="3d9e3-143">Wybierz **usunięte z modelu**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="3d9e3-144">Kliknij prawym przyciskiem myszy skojarzenie między **OnsiteCourse** i **kurs** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="3d9e3-145">Wybierz **usunięte z modelu**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="3d9e3-146">Firma Microsoft usunie teraz **CourseID** właściwość **OnlineCourse** i **OnsiteCourse** ponieważ te klasy dziedziczą **CourseID** z **kurs** typ podstawowy.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="3d9e3-147">Kliknij prawym przyciskiem myszy **CourseID** właściwość **OnlineCourse** typu jednostki, a następnie wybierz **usunięte z modelu**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="3d9e3-148">Kliknij prawym przyciskiem myszy **CourseID** właściwość **OnsiteCourse** typu jednostki, a następnie wybierz **usunięte z modelu**</span><span class="sxs-lookup"><span data-stu-id="3d9e3-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="3d9e3-149">Tabela wg typu dziedziczenia jest teraz implementowane.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="3d9e3-151">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="3d9e3-151">Use the Model</span></span>

<span data-ttu-id="3d9e3-152">Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="3d9e3-153">Wklej następujący kod do **Main** funkcji.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="3d9e3-154">Kod wykonuje trzy zapytania.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-154">The code executes three queries.</span></span> <span data-ttu-id="3d9e3-155">Pierwsze zapytanie powoduje zwrócenie wszystkich **kursów** związane z określonej dział.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="3d9e3-156">Drugie zapytanie używa **OfType** metodę, aby zwrócić **OnlineCourses** związane z określonej dział.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="3d9e3-157">Zwraca trzecie zapytanie **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="3d9e3-157">The third query returns **OnsiteCourses**.</span></span>

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
