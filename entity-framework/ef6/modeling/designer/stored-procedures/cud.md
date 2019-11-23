---
title: CUD Designer — procedury składowane — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813560"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="7bd7f-102">Procedury składowane CUD projektanta</span><span class="sxs-lookup"><span data-stu-id="7bd7f-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="7bd7f-103">W tym przewodniku krok po kroku pokazano, jak zmapować operacje tworzenia\\INSERT, Update i Delete (CUD) typu jednostki na procedury składowane przy użyciu Entity Framework Designer (program EF Designer).</span><span class="sxs-lookup"><span data-stu-id="7bd7f-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="7bd7f-104"> Domyślnie Entity Framework automatycznie generuje instrukcje SQL dla operacji CUD, ale można również mapować procedury składowane na te operacje.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="7bd7f-105">Należy pamiętać, że Code First nie obsługuje mapowania na procedury składowane lub funkcje.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="7bd7f-106">Można jednak wywołać procedury składowane lub funkcje przy użyciu metody System. Data. Entity. Nieogólnymi. sqlQuery.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="7bd7f-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7bd7f-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="7bd7f-108">Zagadnienia dotyczące mapowania operacji CUD na procedury składowane</span><span class="sxs-lookup"><span data-stu-id="7bd7f-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="7bd7f-109">Podczas mapowania operacji CUD na procedury składowane są stosowane następujące zagadnienia:</span><span class="sxs-lookup"><span data-stu-id="7bd7f-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="7bd7f-110">W przypadku mapowania jednej z CUD operacji do procedury składowanej należy zamapować wszystkie z nich.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="7bd7f-111">Jeśli nie wszystkie trzy, niezamapowane operacje zakończą się niepowodzeniem, jeśli zostaną wykonane i zostanie zgłoszony wyjątek **updateexception** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="7bd7f-112">Należy zmapować każdy parametr procedury składowanej na właściwości obiektu.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="7bd7f-113">Jeśli serwer generuje wartość klucza podstawowego dla wstawionego wiersza, należy zmapować tę wartość z powrotem na właściwość klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="7bd7f-114">W poniższym przykładzie procedura składowana **InsertPerson** zwraca nowo utworzony klucz podstawowy w ramach zestawu wyników procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="7bd7f-115">Klucz podstawowy jest mapowany na klucz jednostki (**PersonID**) przy użyciu funkcji **&lt;dodaj powiązania wyniku&gt;**  funkcja programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="7bd7f-116">Wywołania procedury składowanej są mapowane 1:1 z jednostkami w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="7bd7f-117">Na przykład w przypadku zaimplementowania hierarchii dziedziczenia w modelu koncepcyjnym, a następnie zamapowania procedur składowanych CUD dla jednostek **nadrzędnych** (podstawowych) i **podrzędnych** (pochodnych), zapisanie zmian **podrzędnych** spowoduje wyzwolenie wywołań procedur przechowywanych **elementu nadrzędnego**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7bd7f-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7bd7f-118">Prerequisites</span></span>

<span data-ttu-id="7bd7f-119">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="7bd7f-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="7bd7f-120">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="7bd7f-121">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7bd7f-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7bd7f-122">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="7bd7f-122">Set up the Project</span></span>

- <span data-ttu-id="7bd7f-123">Open Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="7bd7f-124">Wybierz pozycję **plik —&gt; nowy&gt; projekt**</span><span class="sxs-lookup"><span data-stu-id="7bd7f-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="7bd7f-125">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="7bd7f-126">Wprowadź **CUDSProcsSample** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="7bd7f-127">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="7bd7f-128">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="7bd7f-128">Create a Model</span></span>

- <span data-ttu-id="7bd7f-129">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="7bd7f-130">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="7bd7f-131">W polu Nazwa pliku wprowadź **CUDSProcs. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="7bd7f-132">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="7bd7f-133">Kliknij pozycję **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-133">Click **New Connection**.</span></span> <span data-ttu-id="7bd7f-134">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="7bd7f-135">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="7bd7f-136">W oknie dialogowym Wybierz obiekty bazy danych w węźle **tabele** wybierz tabelę **osoba** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="7bd7f-137">Należy również wybrać następujące procedury składowane w węźle **procedury składowane i funkcje** : **DeletePerson**, **InsertPerson**i **UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="7bd7f-138">Począwszy od programu Visual Studio 2012, program Dr Designer obsługuje zbiorcze Importowanie procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="7bd7f-139">**Importowane procedury składowane i funkcje do modelu jednostki** są domyślnie zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="7bd7f-140">Ponieważ w tym przykładzie mamy procedury składowane, które wstawiają, aktualizują i usuwają typy jednostek, nie chcemy ich zaimportować i usuń zaznaczenie tego pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Importuj elementy S](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="7bd7f-142">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-142">Click **Finish**.</span></span>
    <span data-ttu-id="7bd7f-143">Zostanie wyświetlony Projektant EF, który zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="7bd7f-144">Mapuj jednostkę osoby na procedury składowane</span><span class="sxs-lookup"><span data-stu-id="7bd7f-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="7bd7f-145">Kliknij prawym przyciskiem myszy **osobę** typ jednostki i wybierz pozycję **mapowanie procedury składowanej**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="7bd7f-146">Mapowania procedury składowanej pojawiają się w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="7bd7f-147">Kliknij \*\*&lt;wybierz pozycję wstaw&gt;funkcji \*\*.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="7bd7f-148">Pole stanie się listą rozwijaną procedur składowanych w modelu magazynu, które mogą być mapowane na typy jednostek w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="7bd7f-149">Z listy rozwijanej wybierz pozycję **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="7bd7f-150">Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="7bd7f-151">Należy zauważyć, że strzałki wskazują kierunek mapowania: wartości właściwości są dostarczane do parametrów procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="7bd7f-152">Kliknij **&lt;dodać powiązanie wyniku&gt;** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="7bd7f-153">Wpisz **NewPersonID**, nazwę parametru zwróconego przez procedurę składowaną **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="7bd7f-154">Pamiętaj, aby nie wpisywać spacji wiodących lub końcowych.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="7bd7f-155">Naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-155">Press **Enter**.</span></span>
- <span data-ttu-id="7bd7f-156">Domyślnie **NewPersonID** jest mapowany na klucz jednostki **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="7bd7f-157">Należy zauważyć, że strzałka wskazuje kierunek mapowania: wartość kolumny wynik jest dostarczana do właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Szczegóły mapowania](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="7bd7f-159">Kliknij pozycję **&lt;wybierz pozycję Aktualizuj funkcję&gt;**  a następnie wybierz pozycję **UpdatePerson** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="7bd7f-160">Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="7bd7f-161">Kliknij **&lt;wybierz pozycję Usuń funkcję&gt;**  i wybierz pozycję **DeletePerson** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="7bd7f-162">Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="7bd7f-163">Operacje INSERT, Update i DELETE elementu typ jednostki są teraz **mapowane na procedury** składowane.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="7bd7f-164">Jeśli chcesz włączyć sprawdzanie współbieżności podczas aktualizowania lub usuwania jednostki za pomocą procedur składowanych, użyj jednej z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="7bd7f-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="7bd7f-165">Użyj parametru **wyjściowego** , aby zwrócić liczbę odnośnych wierszy z procedury składowanej, a następnie zaznacz pole wyboru **parametr, którego dotyczy wiersz** obok nazwy parametru.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="7bd7f-166">Jeśli zwracana wartość jest równa zero, gdy operacja jest wywoływana, zostanie zgłoszony  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="7bd7f-167">Zaznacz pole wyboru **Użyj oryginalnej wartości** obok właściwości, która ma być używana do sprawdzania współbieżności.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="7bd7f-168">Po próbie aktualizacji wartość właściwości, która pierwotnie odczytana z bazy danych, zostanie użyta podczas zapisywania danych z powrotem do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="7bd7f-169">Jeśli wartość nie jest zgodna z wartością w bazie danych, zostanie zgłoszony **OptimisticConcurrencyException** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7bd7f-170">Korzystanie z modelu</span><span class="sxs-lookup"><span data-stu-id="7bd7f-170">Use the Model</span></span>

<span data-ttu-id="7bd7f-171">Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="7bd7f-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="7bd7f-172">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="7bd7f-173">Kod tworzy nowy obiekt **osoby** , a następnie aktualizuje obiekt, a wreszcie usuwa obiekt.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

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

- <span data-ttu-id="7bd7f-174">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-174">Compile and run the application.</span></span> <span data-ttu-id="7bd7f-175">Program tworzy następujące dane wyjściowe \*</span><span class="sxs-lookup"><span data-stu-id="7bd7f-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="7bd7f-176">PersonID jest generowana automatycznie przez serwer, więc najprawdopodobniej zobaczysz inną liczbę \*</span><span class="sxs-lookup"><span data-stu-id="7bd7f-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="7bd7f-177">Jeśli pracujesz z wersją Ultimate programu Visual Studio, możesz użyć IntelliTrace z debugerem, aby wyświetlić instrukcje SQL, które są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="7bd7f-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
