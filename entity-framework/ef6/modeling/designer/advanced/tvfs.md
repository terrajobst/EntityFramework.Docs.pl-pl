---
title: Funkcje zwracające tabelę funkcji (Tvf) - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 34aebd8f5f2c3b43c80e21c1a17a386597596c05
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283956"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="126d6-102">Funkcje z wartościami przechowywanymi w tabeli (funkcji Tvf)</span><span class="sxs-lookup"><span data-stu-id="126d6-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="126d6-103">**EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="126d6-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="126d6-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="126d6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="126d6-105">Przewodnik krok po kroku i wideo pokazuje, jak mapować zwracających tabelę (funkcji Tvf) za pomocą programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="126d6-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="126d6-106">Ilustruje też sposób wywoływania funkcji TVF w wyniku zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="126d6-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="126d6-107">Funkcji Tvf są obecnie obsługiwane tylko w bazie danych pierwszego przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="126d6-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="126d6-108">Obsługa funkcji TVF została wprowadzona w programie Entity Framework w wersji 5.</span><span class="sxs-lookup"><span data-stu-id="126d6-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="126d6-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak zwracających tabelę, wyliczeń i typów przestrzennych, należy wskazać .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="126d6-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="126d6-110">Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="126d6-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="126d6-111">Funkcji Tvf są bardzo podobne do procedur składowanych z jedną kluczową różnicą: wynik funkcji TVF jest konfigurowalna.</span><span class="sxs-lookup"><span data-stu-id="126d6-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="126d6-112">Oznacza to, że wyniki z funkcji TVF mogą być używane w zapytaniu LINQ, ale nie wyników procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="126d6-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="126d6-113">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="126d6-113">Watch the video</span></span>

<span data-ttu-id="126d6-114">**Osoba prezentująca**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="126d6-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="126d6-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="126d6-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="126d6-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="126d6-116">Pre-Requisites</span></span>

<span data-ttu-id="126d6-117">Aby ukończyć ten Instruktaż, musisz:</span><span class="sxs-lookup"><span data-stu-id="126d6-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="126d6-118">Zainstaluj [bazę danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="126d6-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="126d6-119">Posiadania najnowszej wersji programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="126d6-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="126d6-120">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="126d6-120">Set up the Project</span></span>

1.  <span data-ttu-id="126d6-121">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="126d6-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="126d6-122">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**</span><span class="sxs-lookup"><span data-stu-id="126d6-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="126d6-123">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu</span><span class="sxs-lookup"><span data-stu-id="126d6-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="126d6-124">Wprowadź **TVF** jako nazwę projektu i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="126d6-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="126d6-125">Dodawanie funkcji TVF w bazie danych</span><span class="sxs-lookup"><span data-stu-id="126d6-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="126d6-126">Wybierz **widok —&gt; Eksplorator obiektów SQL Server**</span><span class="sxs-lookup"><span data-stu-id="126d6-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="126d6-127">Jeśli LocalDB nie znajduje się lista serwerów: kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** do łączenia się z serwerem LocalDB</span><span class="sxs-lookup"><span data-stu-id="126d6-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="126d6-128">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="126d6-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="126d6-129">W węźle bazy danych, kliknij prawym przyciskiem myszy węzeł bazy danych School, a następnie wybierz pozycję **nowe zapytanie...**</span><span class="sxs-lookup"><span data-stu-id="126d6-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="126d6-130">W edytorze języka T-SQL, Wklej poniższą definicję funkcji TVF</span><span class="sxs-lookup"><span data-stu-id="126d6-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="126d6-131">Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="126d6-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="126d6-132">Funkcja GetStudentGradesForCourse zostanie dodany do bazy danych School</span><span class="sxs-lookup"><span data-stu-id="126d6-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="126d6-133">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="126d6-133">Create a Model</span></span>

1.  <span data-ttu-id="126d6-134">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**</span><span class="sxs-lookup"><span data-stu-id="126d6-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="126d6-135">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w **szablony** okienko</span><span class="sxs-lookup"><span data-stu-id="126d6-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="126d6-136">Wprowadź **TVFModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="126d6-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="126d6-137">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="126d6-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="126d6-138">Kliknij przycisk **nowe połączenie** Enter **(localdb)\\mssqllocaldb** w tekście nazwy serwera polu wprowadź **School** bazy danych programu nazwę kliknij **OK**</span><span class="sxs-lookup"><span data-stu-id="126d6-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="126d6-139">W polu Wybierz obiekty bazy danych okna dialogowego w polu **tabel** węzeł **osoby**, **StudentGrade**, i **kurs** tabel</span><span class="sxs-lookup"><span data-stu-id="126d6-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="126d6-140">Wybierz **GetStudentGradesForCourse** funkcja znajdujący się w folderze **procedur przechowywanych i funkcji** węzła należy pamiętać, że począwszy od programu Visual Studio 2012, Projektant obiektów umożliwia importowanie usługi batch Twoje procedur przechowywanych i funkcji</span><span class="sxs-lookup"><span data-stu-id="126d6-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="126d6-141">Kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="126d6-141">Click **Finish**</span></span>
9.  <span data-ttu-id="126d6-142">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="126d6-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="126d6-143">Wszystkie obiekty, które wybrano w **wybierz obiekty bazy danych** okno dialogowe, są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="126d6-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="126d6-144">Domyślnie kształt wynik każdego importowanych procedura składowana lub funkcja automatycznie stanie się nowy typ złożony w modelu entity.</span><span class="sxs-lookup"><span data-stu-id="126d6-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="126d6-145">Ale chcemy mapowania wyniki funkcji GetStudentGradesForCourse jednostki StudentGrade: kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **przeglądarka modeli** w przeglądarce modelu wybierz **Importy funkcja**, a następnie kliknij dwukrotnie **GetStudentGradesForCourse** funkcji w edytować funkcji Importuj okno dialogowe, zaznacz **jednostek** i wybierz polecenie **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="126d6-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="126d6-146">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="126d6-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="126d6-147">Otwórz plik, w których zdefiniowano metody Main.</span><span class="sxs-lookup"><span data-stu-id="126d6-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="126d6-148">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="126d6-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="126d6-149">Poniższy kod przedstawia sposób tworzenia zapytania, które korzysta z funkcji z wartościami przechowywanymi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="126d6-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="126d6-150">Zapytanie projektów wyniki na typ anonimowy, który zawiera powiązane tytuł kursu i powiązane uczniom i studentom klasy korporacyjnej, większa lub równa 3.5.</span><span class="sxs-lookup"><span data-stu-id="126d6-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="126d6-151">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="126d6-151">Compile and run the application.</span></span> <span data-ttu-id="126d6-152">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="126d6-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="126d6-153">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="126d6-153">Summary</span></span>

<span data-ttu-id="126d6-154">W tym przewodniku zobaczyliśmy, jak mapować zwracających tabelę (funkcji Tvf) za pomocą programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="126d6-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="126d6-155">On również przedstawiono sposób wywoływania funkcji TVF w wyniku zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="126d6-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
