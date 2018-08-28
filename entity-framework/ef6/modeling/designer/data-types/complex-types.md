---
title: Typy złożone - projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: d35504cbe60823249d54385962568802b3e41308
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994856"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="52811-102">Typy złożone - projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="52811-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="52811-103">W tym temacie pokazano, jak do mapowania typów złożonych z Entity Framework Designer (Projektant EF) oraz sposób wykonywania zapytań w przypadku jednostek, które zawierają właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="52811-104">Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="52811-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="52811-106">Podczas tworzenia modelu koncepcyjnego ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów.</span><span class="sxs-lookup"><span data-stu-id="52811-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="52811-107">Można zignorować te ostrzeżenia, ponieważ po dokonaniu wyboru wygenerować bazę danych z modelu, błędy znikną.</span><span class="sxs-lookup"><span data-stu-id="52811-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="52811-108">Co to jest typem złożonym</span><span class="sxs-lookup"><span data-stu-id="52811-108">What is a Complex Type</span></span>

<span data-ttu-id="52811-109">Typy złożone są właściwościami nieskalarnego typów jednostek, które umożliwiają właściwości skalarne, które mają być organizowane w ramach jednostek.</span><span class="sxs-lookup"><span data-stu-id="52811-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="52811-110">Podobnie jak jednostki typy złożone składają się z właściwości skalarne właściwości lub innego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="52811-111">Podczas pracy z obiektami, które reprezentują typy złożone, należy pamiętać o następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="52811-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="52811-112">Typy złożone nie ma kluczy i dlatego nie mogą istnieć niezależnie.</span><span class="sxs-lookup"><span data-stu-id="52811-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="52811-113">Typy złożone może istnieć tylko jako właściwości typów jednostek lub inne typy złożone.</span><span class="sxs-lookup"><span data-stu-id="52811-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="52811-114">Typy złożone nie mogą uczestniczyć w skojarzenia i nie może zawierać właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="52811-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="52811-115">Właściwości typu złożonego nie mogą być **null**.</span><span class="sxs-lookup"><span data-stu-id="52811-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="52811-116">** InvalidOperationException ** występuje, gdy **DbContext.SaveChanges** nosi nazwę i obiekt złożony null zostanie osiągnięty.</span><span class="sxs-lookup"><span data-stu-id="52811-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="52811-117">Właściwości skalarne obiektów złożonych może być **null**.</span><span class="sxs-lookup"><span data-stu-id="52811-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="52811-118">Typy złożone nie może dziedziczyć z innych typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="52811-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="52811-119">Należy zdefiniować typ złożony jako **klasy**.</span><span class="sxs-lookup"><span data-stu-id="52811-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="52811-120">EF wykrywa zmiany do elementów członkowskich w obiekcie typu złożonego przy **DbContext.DetectChanges** jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="52811-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="52811-121">Wywołuje platformy Entity Framework **metody DetectChanges** automatycznie, kiedy są wywoływane następujące składowe: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="52811-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="52811-122">Refaktoryzacji właściwości jednostki na nowy typ złożony</span><span class="sxs-lookup"><span data-stu-id="52811-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="52811-123">Jeśli masz już jednostki w modelu koncepcyjnego może zajść potrzeba refaktoryzacji niektórych właściwości na właściwość typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="52811-124">Na powierzchni projektowej, wybierz jeden lub więcej właściwości (z wyjątkiem właściwości nawigacji) jednostki, następnie kliknij prawym przyciskiem myszy i wybierz **Refaktoryzuj -&gt; przenieść do nowego typu złożonego**.</span><span class="sxs-lookup"><span data-stu-id="52811-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refaktoryzacja](~/ef6/media/refactor.png)

<span data-ttu-id="52811-126">Nowy typ złożony z wybranymi właściwościami zostanie dodany do **przeglądarka modeli**.</span><span class="sxs-lookup"><span data-stu-id="52811-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="52811-127">Typ złożony jest nadawana nazwa domyślna.</span><span class="sxs-lookup"><span data-stu-id="52811-127">The complex type is given a default name.</span></span>

<span data-ttu-id="52811-128">Właściwości złożonej typu nowo utworzony zastępuje wybranych właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="52811-129">Wszystkie mapowania właściwości są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="52811-129">All property mappings are preserved.</span></span>

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="52811-131">Utwórz nowy typ złożony</span><span class="sxs-lookup"><span data-stu-id="52811-131">Create a New Complex Type</span></span>

<span data-ttu-id="52811-132">Można również utworzyć nowy typ złożony, który nie zawiera właściwości istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="52811-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="52811-133">Kliknij prawym przyciskiem myszy **typy złożone** folderu w przeglądarce modelu wskaż **typu złożonego działają funkcje AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="52811-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="52811-134">Alternatywnie, można wybrać **typy złożone** folder, a następnie naciśnij klawisz **Wstaw** kluczowe na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="52811-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="52811-136">Nowy typ złożony jest dodawany do folderu o domyślnej nazwie.</span><span class="sxs-lookup"><span data-stu-id="52811-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="52811-137">Teraz można dodać właściwości do typu.</span><span class="sxs-lookup"><span data-stu-id="52811-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="52811-138">Dodaj właściwości do typu złożonego</span><span class="sxs-lookup"><span data-stu-id="52811-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="52811-139">Typy skalarne lub istniejące typy złożone, może być właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="52811-140">Jednak właściwości typu złożonego nie może mieć odwołania cykliczne.</span><span class="sxs-lookup"><span data-stu-id="52811-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="52811-141">Na przykład typ złożony **OnsiteCourseDetails** nie może mieć właściwość typu złożonego **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="52811-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="52811-142">Można dodać właściwość, typ złożony sposoby wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="52811-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="52811-143">Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu, wskaż opcję **Dodaj**, następnie wskaż **właściwość skalarną** lub **właściwość typu złożonego**, następnie wybierz typ żądanej właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="52811-144">Alternatywnie można wybrać typ złożony, a następnie naciśnij klawisz **Wstaw** kluczowe na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="52811-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="52811-146">Nowa właściwość jest dodawany do typu złożonego o domyślnej nazwie.</span><span class="sxs-lookup"><span data-stu-id="52811-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="52811-147">LUB —</span><span class="sxs-lookup"><span data-stu-id="52811-147">OR -</span></span>

-   <span data-ttu-id="52811-148">Kliknij prawym przyciskiem myszy właściwość jednostek **projektancie platformy EF** powierzchni i wybierz **kopiowania**, kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz  **Wklej**.</span><span class="sxs-lookup"><span data-stu-id="52811-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="52811-149">Zmień nazwę typu złożonego</span><span class="sxs-lookup"><span data-stu-id="52811-149">Rename a Complex Type</span></span>

<span data-ttu-id="52811-150">Po zmianie nazwy typu złożonego, wszystkie odwołania do typu są aktualizowane w całym projekcie.</span><span class="sxs-lookup"><span data-stu-id="52811-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="52811-151">Wolno kliknij dwukrotnie typu złożonego w **przeglądarka modeli**.</span><span class="sxs-lookup"><span data-stu-id="52811-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="52811-152">Nazwa zostanie wybrana i w trybie edycji.</span><span class="sxs-lookup"><span data-stu-id="52811-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="52811-153">LUB —</span><span class="sxs-lookup"><span data-stu-id="52811-153">OR -</span></span>

-   <span data-ttu-id="52811-154">Kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz **Zmień nazwę**.</span><span class="sxs-lookup"><span data-stu-id="52811-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="52811-155">LUB —</span><span class="sxs-lookup"><span data-stu-id="52811-155">OR -</span></span>

-   <span data-ttu-id="52811-156">Wybierz typ złożony w przeglądarce modelu, a następnie naciśnij klawisz F2.</span><span class="sxs-lookup"><span data-stu-id="52811-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="52811-157">LUB —</span><span class="sxs-lookup"><span data-stu-id="52811-157">OR -</span></span>

-   <span data-ttu-id="52811-158">Kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="52811-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="52811-159">Edytuj nazwę w **właściwości** okna.</span><span class="sxs-lookup"><span data-stu-id="52811-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="52811-160">Dodawanie istniejącego typu złożonego do jednostki i zamapuj jego właściwości do kolumn tabeli</span><span class="sxs-lookup"><span data-stu-id="52811-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="52811-161">Kliknij prawym przyciskiem myszy jednostkę, wskaż opcję **Dodaj nowe**i wybierz **właściwość typu złożonego**.</span><span class="sxs-lookup"><span data-stu-id="52811-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="52811-162">Właściwość typu złożonego o domyślnej nazwie zostanie dodany do jednostki.</span><span class="sxs-lookup"><span data-stu-id="52811-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="52811-163">Domyślny typ (masz wybrane z istniejących typów złożonych) jest przypisywane do właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="52811-164">Przypisz żądany typ właściwości w **właściwości** okna.</span><span class="sxs-lookup"><span data-stu-id="52811-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="52811-165">Po dodaniu właściwość typu złożonego do jednostki, jego właściwości musi być mapowane na kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="52811-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="52811-166">Kliknij prawym przyciskiem myszy typ jednostki na powierzchni projektowej lub w **przeglądarka modeli** i wybierz **mapowania tabel**.</span><span class="sxs-lookup"><span data-stu-id="52811-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="52811-167">Mapowania tabel są wyświetlane w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="52811-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="52811-168">Rozwiń **mapuje &lt;nazwy tabeli&gt;**  węzła.</span><span class="sxs-lookup"><span data-stu-id="52811-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="52811-169">A **mapowania kolumn** węzeł jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="52811-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="52811-170">Rozwiń **mapowania kolumn** węzła.</span><span class="sxs-lookup"><span data-stu-id="52811-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="52811-171">Zostanie wyświetlona lista wszystkich kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="52811-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="52811-172">Domyślne właściwości (jeśli istnieje) który mapowania kolumn są wyświetlane w obszarze **wartość/właściwości** nagłówka.</span><span class="sxs-lookup"><span data-stu-id="52811-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="52811-173">Wybierz kolumnę, którą chcesz zamapować, a następnie kliknij prawym przyciskiem myszy odpowiedni **wartość/właściwości** pola.</span><span class="sxs-lookup"><span data-stu-id="52811-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="52811-174">Jest wyświetlana lista rozwijana lista właściwości skalarne.</span><span class="sxs-lookup"><span data-stu-id="52811-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="52811-175">Wybierz odpowiednie właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-175">Select the appropriate property.</span></span>

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="52811-177">Powtórz kroki 6 i 7 dla każdej kolumny.</span><span class="sxs-lookup"><span data-stu-id="52811-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="52811-178">Aby usunąć mapowanie kolumn, wybierz kolumnę, którą chcesz zamapować, a następnie kliknij przycisk **wartość/właściwości** pola.</span><span class="sxs-lookup"><span data-stu-id="52811-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="52811-179">Następnie wybierz **Usuń** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="52811-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="52811-180">Importowanie funkcji mapowania typu złożonego</span><span class="sxs-lookup"><span data-stu-id="52811-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="52811-181">Importy funkcji są oparte na procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="52811-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="52811-182">Aby mapować importowanie funkcji typu złożonego, kolumny zwracane przez odpowiednie procedury składowanej musi odpowiadać właściwości typu złożonego w liczbie i musi mieć typy magazynu, które są zgodne z typami właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="52811-183">Kliknij dwukrotnie importowanych funkcja, która ma być mapowany do typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![FunctionImports](~/ef6/media/functionimports.png)

-   <span data-ttu-id="52811-185">Wypełnij ustawienia dla nowego importowanie funkcji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="52811-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="52811-186">Określ procedury składowanej, dla którego tworzysz importowanie funkcji, w **Nazwa procedury składowanej** pola.</span><span class="sxs-lookup"><span data-stu-id="52811-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="52811-187">To pole jest listy rozwijanej, który wyświetla wszystkie procedury przechowywane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="52811-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="52811-188">Określ nazwę funkcji importu w **nazwy importowanie funkcji** pola.</span><span class="sxs-lookup"><span data-stu-id="52811-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="52811-189">Wybierz **złożonych** jako zwracany typ, a następnie określ określonych zwracany typ złożony, wybierając odpowiedni typ z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="52811-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="52811-191">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="52811-191">Click **OK**.</span></span>
    <span data-ttu-id="52811-192">W modelu koncepcyjnym jest tworzony wpis importu funkcji.</span><span class="sxs-lookup"><span data-stu-id="52811-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="52811-193">Dostosuj kolumny mapowania dla importu — funkcja</span><span class="sxs-lookup"><span data-stu-id="52811-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="52811-194">Kliknij prawym przyciskiem myszy importowanie funkcji w przeglądarce modelu, a następnie wybierz pozycję **Importowanie mapowania funkcji**.</span><span class="sxs-lookup"><span data-stu-id="52811-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="52811-195">**Szczegóły mapowania** oknie wyświetlane wraz z domyślnego mapowania do importu funkcji.</span><span class="sxs-lookup"><span data-stu-id="52811-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="52811-196">Strzałki wskazują mapowania między wartości w kolumnie i wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="52811-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="52811-197">Domyślnie nazwy kolumn są zakłada się, że taka sama jak nazwy właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="52811-198">Domyślne nazwy kolumn są wyświetlane w kolorze szarym.</span><span class="sxs-lookup"><span data-stu-id="52811-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="52811-199">W razie potrzeby zmień nazwy kolumn do dopasowania nazwy kolumn, które są zwracane przez procedurę składowaną, która odnosi się do importu funkcji.</span><span class="sxs-lookup"><span data-stu-id="52811-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="52811-200">Usuń typ złożony</span><span class="sxs-lookup"><span data-stu-id="52811-200">Delete a Complex Type</span></span>

<span data-ttu-id="52811-201">Jeśli usuniesz to typ złożony, typ zostanie usunięta z modelu koncepcyjnego i mapowania dla wszystkich wystąpień tego typu są usuwane.</span><span class="sxs-lookup"><span data-stu-id="52811-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="52811-202">Odwołania do typu nie są aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="52811-202">However, references to the type are not updated.</span></span> <span data-ttu-id="52811-203">Na przykład, jeśli określona jednostka ma właściwość typu złożonego typu ComplexType1 i ComplexType1 zostanie usunięty z **przeglądarka modeli**, odpowiednie właściwości jednostki nie jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="52811-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="52811-204">Model nie zostanie przeprowadzona Weryfikacja, ponieważ zawiera jednostki, do której odwołuje się do usuniętego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="52811-205">Można zaktualizować lub usunąć odwołania do usuniętych typów złożonych przy użyciu narzędzia Projektant jednostki.</span><span class="sxs-lookup"><span data-stu-id="52811-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="52811-206">Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu, a następnie wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="52811-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="52811-207">LUB —</span><span class="sxs-lookup"><span data-stu-id="52811-207">OR -</span></span>

-   <span data-ttu-id="52811-208">Wybierz typ złożony w przeglądarce modelu i na klawiaturze naciśnij klawisz Delete.</span><span class="sxs-lookup"><span data-stu-id="52811-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="52811-209">Zapytań dotyczących jednostek zawierający właściwości typu złożonego</span><span class="sxs-lookup"><span data-stu-id="52811-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="52811-210">Poniższy kod przedstawia sposób wykonywania zapytania, które zwraca kolekcję jednostek typu obiektów, które zawierają właściwość typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="52811-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
