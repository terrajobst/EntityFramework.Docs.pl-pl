---
title: Projektanta CUD procedury składowane - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 7a3176e1057816dd11ced5fc545aa3baa672bd03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993892"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="d7839-102">Procedury składowane CUD projektanta</span><span class="sxs-lookup"><span data-stu-id="d7839-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="d7839-103">Ten przewodnik krok po kroku pokazano, jak mapować tworzenia\\Wstawianie, aktualizowanie i usuwanie operacji (CUD) typu jednostki do procedur składowanych, za pomocą funkcji Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="d7839-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="d7839-104">Domyślnie platforma Entity Framework automatycznie generuje instrukcje SQL dla operacje CUD, ale można również mapować procedur składowanych do tych operacji.</span><span class="sxs-lookup"><span data-stu-id="d7839-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="d7839-105">Należy pamiętać, że Code First platformy nie obsługuje mapowania do procedury składowanej lub funkcji.</span><span class="sxs-lookup"><span data-stu-id="d7839-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="d7839-106">Jednak można wywoływać procedury składowanej lub funkcji za pomocą metody System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="d7839-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="d7839-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d7839-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="d7839-108">Uwagi dotyczące mapowania operacje CUD do procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="d7839-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="d7839-109">Mapowanie operacje CUD do procedur składowanych, następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="d7839-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="d7839-110">Jeśli mapowanie jeden operacje CUD do procedury składowanej, mapować wszystkie z nich.</span><span class="sxs-lookup"><span data-stu-id="d7839-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="d7839-111">Jeśli nie należy mapować wszystkie trzy, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane i **UpdateException** zostanie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="d7839-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="d7839-112">Każdy parametr procedury składowanej musi być mapowane do właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="d7839-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="d7839-113">Jeśli serwer generuje wartość klucza podstawowego dla wstawionego wiersza, możesz zamapować tę wartość do właściwości klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="d7839-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="d7839-114">W poniższym przykładzie **InsertPerson** procedura składowana ma zwracać jako część zestawu wyników procedury składowanej nowo utworzony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="d7839-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="d7839-115">Klucz podstawowy jest mapowany na klucz jednostki (**PersonID**) przy użyciu **&lt;Dodaj powiązań wyników&gt;** funkcji projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d7839-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="d7839-116">Zapisane wywołania procedur są mapowane 1:1 jednostki w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d7839-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="d7839-117">Na przykład w przypadku zaimplementowania Hierarchia dziedziczenia w modelu koncepcyjnym i następnie mapy CUD procedur składowanych dla **nadrzędnego** (podstawowy) i **podrzędnych** jednostki (pochodnego), zapisywanie **Podrzędnych** zmian będzie wywoływać tylko **podrzędnych**przez procedury składowane, nie spowodują uruchomienia **nadrzędnego**użytkownika przechowywane wywołań procedur.</span><span class="sxs-lookup"><span data-stu-id="d7839-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7839-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d7839-118">Prerequisites</span></span>

<span data-ttu-id="d7839-119">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="d7839-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="d7839-120">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7839-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="d7839-121">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="d7839-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d7839-122">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="d7839-122">Set up the Project</span></span>

-   <span data-ttu-id="d7839-123">Otwórz program Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d7839-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="d7839-124">Wybierz **plikach&gt; New -&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="d7839-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="d7839-125">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d7839-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="d7839-126">Wprowadź **CUDSProcsSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="d7839-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="d7839-127">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="d7839-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="d7839-128">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="d7839-128">Create a Model</span></span>

-   <span data-ttu-id="d7839-129">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="d7839-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="d7839-130">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="d7839-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d7839-131">Wprowadź **CUDSProcs.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d7839-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d7839-132">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d7839-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="d7839-133">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="d7839-133">Click **New Connection**.</span></span> <span data-ttu-id="d7839-134">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d7839-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="d7839-135">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d7839-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="d7839-136">W polu Wybierz obiekty bazy danych okna dialogowego w polu **tabel** węzeł **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="d7839-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="d7839-137">Zaznacz również następujących procedur składowanych w obszarze **procedur przechowywanych i funkcji** węzła: **DeletePerson**, **InsertPerson**, i **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="d7839-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="d7839-138">Począwszy od programu Visual Studio 2012, projektancie platformy EF obsługuje importu zbiorczego procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="d7839-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="d7839-139">**Importowanie wybranych procedur przechowywanych i funkcji do modelu jednostki** jest zaznaczone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d7839-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="d7839-140">Ponieważ w tym przykładzie firma Microsoft ma procedur składowanych, które Wstawianie, aktualizowanie i usuwanie typów jednostek, firma Microsoft nie chcesz zaimportować je, a następnie będzie Usuń zaznaczenie tego pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="d7839-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="d7839-142">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="d7839-142">Click **Finish**.</span></span>
    <span data-ttu-id="d7839-143">Zostanie wyświetlona projektancie platformy EF, oferujący powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="d7839-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="d7839-144">Mapuj jednostki osoby do procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="d7839-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="d7839-145">Kliknij prawym przyciskiem myszy **osoby** typu jednostki, a następnie wybierz **mapowanie procedur przechowywanych**.</span><span class="sxs-lookup"><span data-stu-id="d7839-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="d7839-146">Procedura składowana mapowania są wyświetlane w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="d7839-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d7839-147">Kliknij przycisk  **&lt;wybierz opcję Wstaw funkcja&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d7839-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="d7839-148">Pole staje się listy rozwijanej, procedur składowanych w modelu magazynu, które mogą być mapowane na typy jednostek w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d7839-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="d7839-149">Wybierz **InsertPerson** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="d7839-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="d7839-150">Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="d7839-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="d7839-151">Należy pamiętać, że strzałki wskazują kierunek mapowania: wartości właściwości są dostarczane do parametrów procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="d7839-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="d7839-152">Kliknij przycisk  **&lt;Dodaj powiązanie wynik&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d7839-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="d7839-153">Typ **NewPersonID**, nazwa parametru zwrócony przez **InsertPerson** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="d7839-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="d7839-154">Upewnij się, że nie wpisz spacji wiodących i końcowych.</span><span class="sxs-lookup"><span data-stu-id="d7839-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="d7839-155">Naciśnij klawisz **wprowadź**.</span><span class="sxs-lookup"><span data-stu-id="d7839-155">Press **Enter**.</span></span>
-   <span data-ttu-id="d7839-156">Domyślnie **NewPersonID** jest mapowany na klucz jednostki **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="d7839-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="d7839-157">Należy pamiętać, że strzałka wskazuje kierunek mapowania: wartość kolumny wynik jest dostarczany do właściwości.</span><span class="sxs-lookup"><span data-stu-id="d7839-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="d7839-159">Kliknij przycisk **&lt;wybierz funkcję Update&gt;** i wybierz **UpdatePerson** z wyświetlonej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="d7839-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="d7839-160">Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="d7839-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="d7839-161">Kliknij przycisk **&lt;funkcji wybierz pozycję Usuń&gt;** i wybierz **DeletePerson** z wyświetlonej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="d7839-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="d7839-162">Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="d7839-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="d7839-163">Wstawianie, aktualizowanie i usuwanie operacji **osoby** typu jednostki są teraz mapowane na procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="d7839-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="d7839-164">Jeśli chcesz włączyć współbieżności sprawdzania podczas aktualizowania lub usuwania jednostki za pomocą procedur składowanych, użyj jednej z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="d7839-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="d7839-165">Użyj **dane wyjściowe** parametru, aby zwrócić liczbę wierszy z procedury składowanej i sprawdź, których to dotyczy **parametr na wiersze** pole wyboru obok nazwy parametru.</span><span class="sxs-lookup"><span data-stu-id="d7839-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="d7839-166">Jeśli wartość zwracana wynosi zero, gdy operacja jest wywoływana, [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) zostanie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="d7839-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="d7839-167">Sprawdź **Użyj oryginalnej wartości** pole wyboru obok właściwości, którą chcesz używać sprawdzania współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d7839-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="d7839-168">Próba aktualizacji wartości właściwości, która pierwotnie została odczytana z bazy danych będzie używany podczas zapisywania danych z powrotem do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d7839-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="d7839-169">Jeśli wartość jest niezgodna z wartością w bazie danych **OptimisticConcurrencyException** zostanie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="d7839-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="d7839-170">Użyj modelu</span><span class="sxs-lookup"><span data-stu-id="d7839-170">Use the Model</span></span>

<span data-ttu-id="d7839-171">Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="d7839-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="d7839-172">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="d7839-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="d7839-173">Ten kod tworzy nową **osoby** obiektu, następnie aktualizuje obiekt, a następnie usuwa obiekt.</span><span class="sxs-lookup"><span data-stu-id="d7839-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="d7839-174">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d7839-174">Compile and run the application.</span></span> <span data-ttu-id="d7839-175">Program generuje następujące dane wyjściowe \*</span><span class="sxs-lookup"><span data-stu-id="d7839-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="d7839-176">PersonID został wygenerowany automatycznie przez serwer, więc najprawdopodobniej zobaczysz liczbę różnych \*</span><span class="sxs-lookup"><span data-stu-id="d7839-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="d7839-177">Jeśli pracujesz z programu Visual Studio w wersji Ultimate, umożliwia Intellitrace za pomocą debugera zobacz instrukcje SQL, które są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="d7839-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
