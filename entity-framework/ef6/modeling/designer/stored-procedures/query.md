---
title: Procedury składowane zapytania projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182482"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="de921-102">Procedury składowane zapytania projektanta</span><span class="sxs-lookup"><span data-stu-id="de921-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="de921-103">W tym przewodniku krok po kroku pokazano, jak używać Entity Framework Designer (programu EF Designer) do importowania procedur składowanych do modelu, a następnie wywoływać zaimportowane procedury składowane w celu pobrania wyników.</span><span class="sxs-lookup"><span data-stu-id="de921-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="de921-104">Należy pamiętać, że Code First nie obsługuje mapowania na procedury składowane lub funkcje.</span><span class="sxs-lookup"><span data-stu-id="de921-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="de921-105">Można jednak wywołać procedury składowane lub funkcje przy użyciu metody System. Data. Entity. Nieogólnymi. sqlQuery.</span><span class="sxs-lookup"><span data-stu-id="de921-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="de921-106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="de921-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="de921-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="de921-107">Prerequisites</span></span>

<span data-ttu-id="de921-108">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="de921-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="de921-109">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de921-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="de921-110">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="de921-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="de921-111">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="de921-111">Set up the Project</span></span>

-   <span data-ttu-id="de921-112">Open Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="de921-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="de921-113">Wybierz **plik-&gt; nowy-&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="de921-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="de921-114">W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="de921-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="de921-115">Wprowadź **EFwithSProcsSample** AS nazwę.</span><span class="sxs-lookup"><span data-stu-id="de921-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="de921-116">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="de921-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="de921-117">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="de921-117">Create a Model</span></span>

-   <span data-ttu-id="de921-118">Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **Dodaj-&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="de921-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="de921-119">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="de921-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="de921-120">W polu Nazwa pliku wprowadź **EFwithSProcsModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="de921-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="de921-121">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="de921-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="de921-122">Kliknij pozycję **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="de921-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="de921-123">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB) \\mssqllocaldb**), wybierz metodę uwierzytelniania, typ **szkoły** for nazwę bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="de921-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="de921-124">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="de921-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="de921-125">W oknie dialogowym Wybierz obiekty bazy danych sprawdź **tabele** checkbox, aby zaznaczyć wszystkie tabele.</span><span class="sxs-lookup"><span data-stu-id="de921-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="de921-126">Należy również wybrać następujące procedury składowane w węźle **procedury składowane i funkcje** : **GetStudentGrades** i **getdepartmentname**.</span><span class="sxs-lookup"><span data-stu-id="de921-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Import](~/ef6/media/import.jpg)

    <span data-ttu-id="de921-128">@no__t 0Starting z programem Visual Studio 2012 program Dr Designer obsługuje zbiorcze Importowanie procedur składowanych. **Importowane wybrane procedury składowane i funkcje do modelu theentity** są domyślnie zaznaczone. \*</span><span class="sxs-lookup"><span data-stu-id="de921-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="de921-129">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="de921-129">Click **Finish**.</span></span>

<span data-ttu-id="de921-130">Domyślnie kształt wynik każdej zaimportowanej procedury składowanej, która zwraca więcej niż jedną kolumnę, automatycznie stanie się nowym typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="de921-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="de921-131">W tym przykładzie chcemy zmapować wyniki funkcji **GetStudentGrades** na jednostkę **StudentGrade** i wyniki elementu **getdepartmentname** na **none** (**Brak** jest wartością domyślną).</span><span class="sxs-lookup"><span data-stu-id="de921-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="de921-132">W celu zaimportowania funkcji w celu zwrócenia typu jednostki kolumny zwracane przez odpowiednią procedurę składowaną muszą dokładnie pasować do właściwości skalarnych zwracanego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="de921-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="de921-133">Import funkcji może również zwracać kolekcje typów prostych, typów złożonych lub bez wartości.</span><span class="sxs-lookup"><span data-stu-id="de921-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="de921-134">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz pozycję **przeglądarka modeli**.</span><span class="sxs-lookup"><span data-stu-id="de921-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="de921-135">W oknie **przeglądarka modelu**wybierz pozycję **Importy funkcji**, a następnie kliknij dwukrotnie funkcję **GetStudentGrades** .</span><span class="sxs-lookup"><span data-stu-id="de921-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="de921-136">W oknie dialogowym Edytowanie importu funkcji wybierz pozycję **jednostki** and wybierz pozycję **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="de921-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="de921-137">@no__t — pole wyboru 0The **funkcji jest** możliwe do utworzenia w górnej części okna dialogowego **Importy funkcji** umożliwia zamapowanie na funkcje do redagowania. Jeśli to pole wyboru jest zaznaczone, na liście rozwijanej **procedura składowana/nazwa funkcji** będą wyświetlane tylko funkcje, które można tworzyć (funkcje z wartościami przechowywanymi w tabeli). Jeśli to pole nie zostanie zaznaczone, na liście zostaną wyświetlone tylko funkcje, których nie można utworzyć. \*</span><span class="sxs-lookup"><span data-stu-id="de921-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="de921-138">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="de921-138">Use the Model</span></span>

<span data-ttu-id="de921-139">Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="de921-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="de921-140">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="de921-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="de921-141">Kod wywołuje dwie procedury składowane: **GetStudentGrades** (zwraca **StudentGrades** dla określonych *StudentID*) i **getdepartmentname** (zwraca nazwę działu w parametrze danych wyjściowych).</span><span class="sxs-lookup"><span data-stu-id="de921-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="de921-142">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="de921-142">Compile and run the application.</span></span> <span data-ttu-id="de921-143">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="de921-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="de921-144">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="de921-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="de921-145">Jeśli są używane parametry wyjściowe, ich wartości nie będą dostępne, dopóki wyniki nie zostaną całkowicie odczytane.</span><span class="sxs-lookup"><span data-stu-id="de921-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="de921-146">Jest to spowodowane zachowaniem podstawowego elementu DbDataReader. zobacz [pobieranie danych za pomocą elementu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) , aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="de921-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
