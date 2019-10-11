---
title: Funkcje o wartościach tabelowych (TVFs) — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182537"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="9092b-102">Funkcje z wartościami przechowywanymi w tabeli (TVFs)</span><span class="sxs-lookup"><span data-stu-id="9092b-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="9092b-103">**EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="9092b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="9092b-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="9092b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9092b-105">Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania funkcji z wartościami przechowywanymi w tabeli (TVFs) przy użyciu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9092b-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="9092b-106">Przedstawiono w nim również sposób wywoływania TVF z zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="9092b-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="9092b-107">TVFs są obecnie obsługiwane tylko w przepływie pracy Database First.</span><span class="sxs-lookup"><span data-stu-id="9092b-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="9092b-108">Obsługa TVF została wprowadzona w Entity Framework w wersji 5.</span><span class="sxs-lookup"><span data-stu-id="9092b-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="9092b-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak funkcje z wartościami przechowywanymi w tabeli, wyliczenia i typy przestrzenne, należy określić wartość docelową .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="9092b-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="9092b-110">Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="9092b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="9092b-111">TVFs są bardzo podobne do procedur składowanych z jedną różnicą kluczową: wynik TVF można utworzyć.</span><span class="sxs-lookup"><span data-stu-id="9092b-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="9092b-112">Oznacza to, że wyniki z TVF można użyć w zapytaniu LINQ, podczas gdy nie można wykonać wyników procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9092b-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="9092b-113">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="9092b-113">Watch the video</span></span>

<span data-ttu-id="9092b-114">**Przedstawione przez**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="9092b-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="9092b-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="9092b-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9092b-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9092b-116">Pre-Requisites</span></span>

<span data-ttu-id="9092b-117">Aby ukończyć ten przewodnik, musisz wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9092b-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="9092b-118">Zainstaluj [szkołę bazy danych](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9092b-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="9092b-119">Mieć najnowszą wersję programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9092b-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9092b-120">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="9092b-120">Set up the Project</span></span>

1.  <span data-ttu-id="9092b-121">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9092b-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="9092b-122">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .</span><span class="sxs-lookup"><span data-stu-id="9092b-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="9092b-123">W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli**</span><span class="sxs-lookup"><span data-stu-id="9092b-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="9092b-124">Wprowadź **TVF** jako nazwę projektu, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="9092b-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="9092b-125">Dodawanie TVF do bazy danych</span><span class="sxs-lookup"><span data-stu-id="9092b-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="9092b-126">Wybierz pozycję **Widok-&gt; Eksplorator obiektów SQL Server**</span><span class="sxs-lookup"><span data-stu-id="9092b-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="9092b-127">Jeśli LocalDB nie znajduje się na liście serwerów: Kliknij prawym przyciskiem myszy **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** do nawiązania połączenia z serwerem LocalDB</span><span class="sxs-lookup"><span data-stu-id="9092b-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="9092b-128">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="9092b-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="9092b-129">W węźle bazy danych kliknij prawym przyciskiem myszy węzeł baza danych szkoły i wybierz polecenie **nowe zapytanie...**</span><span class="sxs-lookup"><span data-stu-id="9092b-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="9092b-130">W edytorze T-SQL wklej następującą definicję TVF</span><span class="sxs-lookup"><span data-stu-id="9092b-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="9092b-131">Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="9092b-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="9092b-132">Funkcja GetStudentGradesForCourse jest dodawana do bazy danych szkoły</span><span class="sxs-lookup"><span data-stu-id="9092b-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="9092b-133">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="9092b-133">Create a Model</span></span>

1.  <span data-ttu-id="9092b-134">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="9092b-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="9092b-135">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku **Szablony** .</span><span class="sxs-lookup"><span data-stu-id="9092b-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="9092b-136">W polu Nazwa pliku wprowadź **TVFModel. edmx** , a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="9092b-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="9092b-137">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="9092b-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="9092b-138">Kliknij pozycję **nowe połączenie** wprowadź **(LocalDB) \\mssqllocaldb** w polu tekstowym nazwa serwera wprowadź **Szkoła** For Nazwa bazy danych kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="9092b-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="9092b-139">W oknie dialogowym Wybierz obiekty bazy danych w obszarze **tabele** Node wybierz **osobę**, **StudentGrade**i **kurs**@no__t — 5tables</span><span class="sxs-lookup"><span data-stu-id="9092b-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="9092b-140">Wybierz funkcję **GetStudentGradesForCourse** znajdującą się w obszarze **procedury składowane i funkcje** node Uwaga, która rozpoczyna się od programu Visual Studio 2012, Entity Designer umożliwia zbiorcze Importowanie procedur i funkcji przechowywanych</span><span class="sxs-lookup"><span data-stu-id="9092b-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="9092b-141">Kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="9092b-141">Click **Finish**</span></span>
9.  <span data-ttu-id="9092b-142">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="9092b-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="9092b-143">Wszystkie obiekty wybrane w polu **Wybierz obiekty bazy danych** dialog są dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="9092b-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="9092b-144">Domyślnie kształt wynik każdej zaimportowanej procedury lub funkcji składowanej automatycznie stanie się nowym typem złożonym w modelu jednostki.</span><span class="sxs-lookup"><span data-stu-id="9092b-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="9092b-145">Jednak chcemy zmapować wyniki funkcji GetStudentGradesForCourse na jednostkę StudentGrade: Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz pozycję **przeglądarka modelu** w przeglądarce modelu, wybierz opcję **Importy funkcji**, a następnie kliknij dwukrotnie funkcję **GetStudentGradesForCourse** w oknie dialogowym Edytowanie importu funkcji, wybierz pozycję **jednostki** .  and wybierz **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="9092b-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="9092b-146">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="9092b-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="9092b-147">Otwórz plik, w którym jest zdefiniowana Metoda Main.</span><span class="sxs-lookup"><span data-stu-id="9092b-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="9092b-148">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="9092b-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="9092b-149">Poniższy kod ilustruje sposób tworzenia zapytania korzystającego z funkcji zwracającej tabelę.</span><span class="sxs-lookup"><span data-stu-id="9092b-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="9092b-150">Zapytanie projektuje wyniki do typu anonimowego, który zawiera powiązane z nim tytuł kursu i pokrewnych uczniów o klasie wyższej lub równej 3,5.</span><span class="sxs-lookup"><span data-stu-id="9092b-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="9092b-151">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="9092b-151">Compile and run the application.</span></span> <span data-ttu-id="9092b-152">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="9092b-152">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="9092b-153">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9092b-153">Summary</span></span>

<span data-ttu-id="9092b-154">W tym instruktażu przedstawiono sposób mapowania funkcji zwracającej tabelę (TVFs) przy użyciu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9092b-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="9092b-155">Przedstawiono w nim również sposób wywoływania TVF z zapytania LINQ.</span><span class="sxs-lookup"><span data-stu-id="9092b-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
