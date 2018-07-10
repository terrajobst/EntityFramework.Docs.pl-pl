---
title: Projektant zapytań procedury składowane - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
caps.latest.revision: 3
ms.openlocfilehash: a08c1afc02266b35372a49fca1e829963e4785b2
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912749"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="65490-102">Procedury składowane projektanta zapytań</span><span class="sxs-lookup"><span data-stu-id="65490-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="65490-103">Ten przewodnik krok po kroku pokazano, jak używać Entity Framework Designer (Projektant EF) do zaimportowania procedur składowanych do modelu, a następnie wywołać importowanych procedur składowanych do pobierania wyników.</span><span class="sxs-lookup"><span data-stu-id="65490-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="65490-104">Należy pamiętać, że Code First platformy nie obsługuje mapowania do procedury składowanej lub funkcji.</span><span class="sxs-lookup"><span data-stu-id="65490-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="65490-105">Jednak można wywoływać procedury składowanej lub funkcji za pomocą metody System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="65490-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="65490-106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="65490-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="65490-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="65490-107">Prerequisites</span></span>

<span data-ttu-id="65490-108">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="65490-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="65490-109">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65490-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="65490-110">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="65490-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="65490-111">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="65490-111">Set up the Project</span></span>

-   <span data-ttu-id="65490-112">Otwórz program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="65490-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="65490-113">Wybierz **plikach&gt; New -&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="65490-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="65490-114">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="65490-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="65490-115">Wprowadź **EFwithSProcsSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="65490-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="65490-116">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="65490-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="65490-117">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="65490-117">Create a Model</span></span>

-   <span data-ttu-id="65490-118">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Add -&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="65490-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="65490-119">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="65490-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="65490-120">Wprowadź **EFwithSProcsModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="65490-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="65490-121">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="65490-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="65490-122">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="65490-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="65490-123">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="65490-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="65490-124">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="65490-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="65490-125">W oknie dialogowym Wybierz obiekty bazy danych sprawdź **tabel** pole wyboru, aby wszystkie tabele.</span><span class="sxs-lookup"><span data-stu-id="65490-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="65490-126">Zaznacz również następujących procedur składowanych w obszarze **procedur przechowywanych i funkcji** węzła: **GetStudentGrades** i **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="65490-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![{1&gt;Importuj&lt;1}](~/ef6/media/import.jpg)

    <span data-ttu-id="65490-128">*Począwszy od programu Visual Studio 2012, projektancie platformy EF obsługuje importu zbiorczego procedur składowanych. **Importowanie wybranych procedur przechowywanych i funkcji do modelu theentity** jest zaznaczone domyślnie.*</span><span class="sxs-lookup"><span data-stu-id="65490-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="65490-129">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="65490-129">Click **Finish**.</span></span>

<span data-ttu-id="65490-130">Domyślnie kształt wynik każdego importowanych procedura składowana lub funkcja, która zwraca więcej niż jedną kolumnę automatycznie stanie się nowy typ złożony.</span><span class="sxs-lookup"><span data-stu-id="65490-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="65490-131">W tym przykładzie chcemy zamapować wyniki **GetStudentGrades** funkcja **StudentGrade** jednostki oraz wyniki **GetDepartmentName** do **Brak** (**Brak** jest wartością domyślną).</span><span class="sxs-lookup"><span data-stu-id="65490-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="65490-132">Importowanie funkcji zwracać typ jednostki kolumny zwracane przez odpowiednie procedury składowanej muszą dokładnie odpowiadać właściwości skalarne typu jednostki zwracanego.</span><span class="sxs-lookup"><span data-stu-id="65490-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="65490-133">Importowanie funkcji może również zwracać kolekcji typów prostych, typów złożonych lub brak wartości.</span><span class="sxs-lookup"><span data-stu-id="65490-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="65490-134">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **przeglądarka modeli**.</span><span class="sxs-lookup"><span data-stu-id="65490-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="65490-135">W **przeglądarka modeli**, wybierz opcję **Importy funkcji**, a następnie kliknij dwukrotnie **GetStudentGrades** funkcji.</span><span class="sxs-lookup"><span data-stu-id="65490-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="65490-136">W oknie dialogowym Edytowanie importowanie funkcji, wybierz **jednostek** i wybierz polecenie **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="65490-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="65490-137">***Importowanie funkcji jest konfigurowalna** pole wyboru w górnej części **Importy funkcji** okno dialogowe umożliwia mapowanie konfigurowalna funkcji. Jeśli zaznaczysz to pole tylko konfigurowalna funkcje (zwracających tabelę), będą wyświetlane w **procedury składowanej / nazwa funkcji** listy rozwijanej. Jeśli to pole wyboru nie jest zaznaczone, tylko funkcje — konfigurowalna będą wyświetlane na liście.*</span><span class="sxs-lookup"><span data-stu-id="65490-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="65490-138">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="65490-138">Use the Model</span></span>

<span data-ttu-id="65490-139">Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="65490-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="65490-140">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="65490-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="65490-141">Kod wywołuje dwie procedury składowane: **GetStudentGrades** (zwraca **StudentGrades** dla określonego *StudentId*) i **GetDepartmentName** (zwraca nazwę działu parametr wyjściowy).</span><span class="sxs-lookup"><span data-stu-id="65490-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="65490-142">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="65490-142">Compile and run the application.</span></span> <span data-ttu-id="65490-143">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="65490-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="65490-144">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="65490-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="65490-145">Jeśli używane są parametry wyjściowe, ich wartości nie będzie dostępne, dopóki wyniki zostały całkowicie odczytane.</span><span class="sxs-lookup"><span data-stu-id="65490-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="65490-146">Jest to spowodowane bazowego zachowanie obiekt DbDataReader, zobacz [podczas pobierania danych przy użyciu elementu DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="65490-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
