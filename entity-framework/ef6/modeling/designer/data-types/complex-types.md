---
title: Typy złożone — Dr Designer — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418652"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="7d549-102">Typy złożone — Projektant EF</span><span class="sxs-lookup"><span data-stu-id="7d549-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="7d549-103">W tym temacie przedstawiono sposób mapowania typów złożonych o Entity Framework Designer (program Dr Designer) i sposób wykonywania zapytań dotyczących jednostek, które zawierają właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="7d549-104">Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="7d549-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektant EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="7d549-106">Podczas budowania modelu koncepcyjnego w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="7d549-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="7d549-107">Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.</span><span class="sxs-lookup"><span data-stu-id="7d549-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="7d549-108">Co to jest typ złożony</span><span class="sxs-lookup"><span data-stu-id="7d549-108">What is a Complex Type</span></span>

<span data-ttu-id="7d549-109">Typy złożone są nieskalarnymi właściwościami typów jednostek, które umożliwiają organizowanie właściwości skalarnych w obrębie jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d549-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="7d549-110">Podobnie jak jednostki, typy złożone składają się z właściwości skalarnych lub innych właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="7d549-111">Podczas pracy z obiektami, które reprezentują typy złożone, należy pamiętać o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="7d549-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="7d549-112">Typy złożone nie mają kluczy i dlatego nie mogą występować niezależnie.</span><span class="sxs-lookup"><span data-stu-id="7d549-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="7d549-113">Typy złożone mogą istnieć tylko jako właściwości typów jednostek lub innych typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="7d549-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="7d549-114">Typy złożone nie mogą uczestniczyć w skojarzeniach i nie mogą zawierać właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d549-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="7d549-115">Właściwości typu złożonego nie mogą mieć **wartości null**.</span><span class="sxs-lookup"><span data-stu-id="7d549-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="7d549-116">**InvalidOperationException **występuje, gdy zostanie wywołana **DbContext. metody SaveChanges** i zostanie napotkany obiekt złożony o wartości null.</span><span class="sxs-lookup"><span data-stu-id="7d549-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="7d549-117">Właściwości skalarne obiektów złożonych mogą mieć **wartość null**.</span><span class="sxs-lookup"><span data-stu-id="7d549-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="7d549-118">Typy złożone nie mogą dziedziczyć z innych typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="7d549-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="7d549-119">Typ złożony należy zdefiniować jako **klasę**.</span><span class="sxs-lookup"><span data-stu-id="7d549-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="7d549-120">EF wykrywa zmiany elementów członkowskich w obiekcie typu złożonego, gdy wywoływana jest **DbContext. DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="7d549-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="7d549-121">Entity Framework wywołania **DetectChanges** automatycznie po wywołaniu następujących elementów członkowskich: **nieogólnymi. Find**, **nieogólnymi. Local**, **nieogólnymi. Remove**, **Nieogólnymi. Add**, **nieogólnymi. Attach**, **DbContext. metody SaveChanges**, **DbContext. GetValidationErrors**, **DbContext. entry**, **DbChangeTracker. wpisy**.</span><span class="sxs-lookup"><span data-stu-id="7d549-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="7d549-122">Refaktoryzacja właściwości jednostki do nowego typu złożonego</span><span class="sxs-lookup"><span data-stu-id="7d549-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="7d549-123">Jeśli masz już jednostkę w modelu koncepcyjnym, możesz chcieć refaktoryzacji niektórych właściwości do właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="7d549-124">Na powierzchni projektanta zaznacz jedną lub więcej właściwości (z wyłączeniem właściwości nawigacji) jednostki, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję **Refaktoryzacja —&gt; Przenieś do nowego typu złożonego**.</span><span class="sxs-lookup"><span data-stu-id="7d549-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refaktoryzacja](~/ef6/media/refactor.png)

<span data-ttu-id="7d549-126">Nowy typ złożony z wybranymi właściwościami zostanie dodany do **przeglądarki modeli**.</span><span class="sxs-lookup"><span data-stu-id="7d549-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="7d549-127">Typ złożony ma przydaną nazwę domyślną.</span><span class="sxs-lookup"><span data-stu-id="7d549-127">The complex type is given a default name.</span></span>

<span data-ttu-id="7d549-128">Właściwość złożona nowo utworzonego typu zastępuje wybrane właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d549-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="7d549-129">Wszystkie mapowania właściwości są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="7d549-129">All property mappings are preserved.</span></span>

![Refaktoryzacja 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="7d549-131">Utwórz nowy typ złożony</span><span class="sxs-lookup"><span data-stu-id="7d549-131">Create a New Complex Type</span></span>

<span data-ttu-id="7d549-132">Można również utworzyć nowy typ złożony, który nie zawiera właściwości istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d549-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="7d549-133">Kliknij prawym przyciskiem myszy folder **typy złożone** w przeglądarce modelu, wskaż **typ złożony typu AddNew.** .</span><span class="sxs-lookup"><span data-stu-id="7d549-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="7d549-134">Alternatywnie możesz wybrać folder **typy złożone** i nacisnąć klawisz **INSERT** na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="7d549-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Dodaj nowy typ złożony](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="7d549-136">Nowy typ złożony zostanie dodany do folderu z nazwą domyślną.</span><span class="sxs-lookup"><span data-stu-id="7d549-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="7d549-137">Teraz możesz dodawać właściwości do typu.</span><span class="sxs-lookup"><span data-stu-id="7d549-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="7d549-138">Dodawanie właściwości do typu złożonego</span><span class="sxs-lookup"><span data-stu-id="7d549-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="7d549-139">Właściwości typu złożonego mogą być typami skalarnymi lub istniejącymi typami złożonymi.</span><span class="sxs-lookup"><span data-stu-id="7d549-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="7d549-140">Jednak właściwości typu złożonego nie mogą mieć odwołań cyklicznych.</span><span class="sxs-lookup"><span data-stu-id="7d549-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="7d549-141">Na przykład typ złożony **OnsiteCourseDetails** nie może mieć właściwości typu złożonego **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="7d549-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="7d549-142">Możesz dodać właściwość do typu złożonego na dowolnym z wymienionych poniżej sposobów.</span><span class="sxs-lookup"><span data-stu-id="7d549-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="7d549-143">W przeglądarce modelu kliknij prawym przyciskiem myszy typ złożony, wskaż polecenie **Dodaj**, a następnie wskaż **właściwość skalarną** lub **Właściwość złożoną**, a następnie wybierz żądany typ właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d549-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="7d549-144">Alternatywnie możesz wybrać typ złożony, a następnie nacisnąć klawisz **Insert** na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="7d549-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Dodawanie właściwości do typu złożonego](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="7d549-146">Nowa właściwość zostanie dodana do typu złożonego z nazwą domyślną.</span><span class="sxs-lookup"><span data-stu-id="7d549-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="7d549-147">Oraz</span><span class="sxs-lookup"><span data-stu-id="7d549-147">OR -</span></span>

-   <span data-ttu-id="7d549-148">Kliknij prawym przyciskiem myszy Właściwość jednostki na powierzchni **projektanta EF** i wybierz polecenie **Kopiuj**, a następnie kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Wklej**.</span><span class="sxs-lookup"><span data-stu-id="7d549-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="7d549-149">Zmień nazwę typu złożonego</span><span class="sxs-lookup"><span data-stu-id="7d549-149">Rename a Complex Type</span></span>

<span data-ttu-id="7d549-150">W przypadku zmiany nazwy typu złożonego wszystkie odwołania do tego typu są aktualizowane w całym projekcie.</span><span class="sxs-lookup"><span data-stu-id="7d549-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="7d549-151">Powoli kliknij dwukrotnie typ złożony w **przeglądarce modeli**.</span><span class="sxs-lookup"><span data-stu-id="7d549-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="7d549-152">Nazwa zostanie wybrana i w trybie edycji.</span><span class="sxs-lookup"><span data-stu-id="7d549-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="7d549-153">Oraz</span><span class="sxs-lookup"><span data-stu-id="7d549-153">OR -</span></span>

-   <span data-ttu-id="7d549-154">Kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Zmień nazwę**.</span><span class="sxs-lookup"><span data-stu-id="7d549-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="7d549-155">Oraz</span><span class="sxs-lookup"><span data-stu-id="7d549-155">OR -</span></span>

-   <span data-ttu-id="7d549-156">Wybierz typ złożony w przeglądarce modelu i naciśnij klawisz F2.</span><span class="sxs-lookup"><span data-stu-id="7d549-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="7d549-157">Oraz</span><span class="sxs-lookup"><span data-stu-id="7d549-157">OR -</span></span>

-   <span data-ttu-id="7d549-158">Kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="7d549-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="7d549-159">Edytuj nazwę w oknie **właściwości** .</span><span class="sxs-lookup"><span data-stu-id="7d549-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="7d549-160">Dodaj istniejący typ złożony do jednostki i zamapuj jego właściwości na kolumny tabeli</span><span class="sxs-lookup"><span data-stu-id="7d549-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="7d549-161">Kliknij prawym przyciskiem myszy jednostkę, wskaż polecenie **Dodaj nowy**i wybierz polecenie **złożona Właściwość**.</span><span class="sxs-lookup"><span data-stu-id="7d549-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="7d549-162">Do jednostki zostanie dodana właściwość typu złożonego o nazwie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="7d549-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="7d549-163">Domyślny typ (wybrany z istniejących typów złożonych) jest przypisany do właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d549-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="7d549-164">Przypisz żądany typ do właściwości w oknie **właściwości** .</span><span class="sxs-lookup"><span data-stu-id="7d549-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="7d549-165">Po dodaniu właściwości typu złożonego do jednostki należy zamapować jej właściwości na kolumny tabeli.</span><span class="sxs-lookup"><span data-stu-id="7d549-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="7d549-166">Kliknij prawym przyciskiem myszy typ jednostki na powierzchni projektowej lub w **przeglądarce modelu** a następnie wybierz pozycję **mapowania tabeli**.</span><span class="sxs-lookup"><span data-stu-id="7d549-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="7d549-167">Mapowania tabeli są wyświetlane w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="7d549-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="7d549-168">Rozwiń węzeł **mapy, aby &lt;nazwę tabeli&gt;** węźle .</span><span class="sxs-lookup"><span data-stu-id="7d549-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="7d549-169">Zostanie wyświetlony węzeł **mapowania kolumn** .</span><span class="sxs-lookup"><span data-stu-id="7d549-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="7d549-170">Rozwiń węzeł **mapowania kolumn** .</span><span class="sxs-lookup"><span data-stu-id="7d549-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="7d549-171">Zostanie wyświetlona lista wszystkich kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7d549-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="7d549-172">Domyślne właściwości (jeśli istnieją), do których mapy kolumn są wyświetlane w nagłówku **Value/Property** .</span><span class="sxs-lookup"><span data-stu-id="7d549-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="7d549-173">Wybierz kolumnę, którą chcesz zmapować, a następnie kliknij prawym przyciskiem myszy odpowiednią **wartość/właściwość** pole.</span><span class="sxs-lookup"><span data-stu-id="7d549-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="7d549-174">Zostanie wyświetlona lista rozwijana wszystkich właściwości skalarnych.</span><span class="sxs-lookup"><span data-stu-id="7d549-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="7d549-175">Wybierz odpowiednią właściwość.</span><span class="sxs-lookup"><span data-stu-id="7d549-175">Select the appropriate property.</span></span>

    ![Mapowanie typu złożonego](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="7d549-177">Powtórz kroki 6 i 7 dla każdej kolumny tabeli.</span><span class="sxs-lookup"><span data-stu-id="7d549-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="7d549-178">Aby usunąć mapowanie kolumny, wybierz kolumnę, którą chcesz zmapować, a następnie kliknij pole **wartość/właściwość** .</span><span class="sxs-lookup"><span data-stu-id="7d549-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="7d549-179">Następnie wybierz pozycję **Usuń** z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7d549-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="7d549-180">Mapowanie funkcji importowania do typu złożonego</span><span class="sxs-lookup"><span data-stu-id="7d549-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="7d549-181">Importy funkcji są oparte na procedurach składowanych.</span><span class="sxs-lookup"><span data-stu-id="7d549-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="7d549-182">Aby zmapować funkcję importu do typu złożonego, kolumny zwracane przez odpowiednią procedurę składowaną muszą być zgodne z właściwościami typu złożonego w liczbie i muszą mieć typy magazynów zgodne z typami właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d549-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="7d549-183">Kliknij dwukrotnie zaimportowaną funkcję, która ma zostać zmapowana do typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importy funkcji](~/ef6/media/functionimports.png)

-   <span data-ttu-id="7d549-185">Wypełnij ustawienia dla nowego importu funkcji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7d549-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="7d549-186">Określ procedurę składowaną, dla której tworzysz import funkcji w polu **nazwa procedury składowanej** .</span><span class="sxs-lookup"><span data-stu-id="7d549-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="7d549-187">To pole jest listą rozwijaną, która wyświetla wszystkie procedury składowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="7d549-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="7d549-188">Określ nazwę importu funkcji w polu  **Nazwa importu funkcji** .</span><span class="sxs-lookup"><span data-stu-id="7d549-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="7d549-189">Wybierz opcję **złożone** jako zwracany typ, a następnie określ konkretny typ zwracany złożone, wybierając odpowiedni typ z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7d549-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Edytuj import funkcji](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="7d549-191">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d549-191">Click **OK**.</span></span>
    <span data-ttu-id="7d549-192">Wpis importowania funkcji jest tworzony w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7d549-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="7d549-193">Dostosuj Mapowanie kolumn dla importu funkcji</span><span class="sxs-lookup"><span data-stu-id="7d549-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="7d549-194">Kliknij prawym przyciskiem myszy funkcję importowania w przeglądarce modelu i wybierz pozycję **Mapowanie importu funkcji**.</span><span class="sxs-lookup"><span data-stu-id="7d549-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="7d549-195">Zostanie wyświetlone okno **szczegóły mapowania** i wyświetlane jest mapowanie domyślne dla importu funkcji.</span><span class="sxs-lookup"><span data-stu-id="7d549-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="7d549-196">Strzałki oznaczają mapowania między wartościami kolumn i wartościami właściwości.</span><span class="sxs-lookup"><span data-stu-id="7d549-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="7d549-197">Domyślnie nazwy kolumn są takie same jak nazwy właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="7d549-198">Domyślne nazwy kolumn są wyświetlane w kolorze szarym.</span><span class="sxs-lookup"><span data-stu-id="7d549-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="7d549-199">W razie potrzeby zmień nazwy kolumn tak, aby były zgodne z nazwami kolumn zwracanymi przez procedurę składowaną, która odnosi się do importowania funkcji.</span><span class="sxs-lookup"><span data-stu-id="7d549-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="7d549-200">Usuń typ złożony</span><span class="sxs-lookup"><span data-stu-id="7d549-200">Delete a Complex Type</span></span>

<span data-ttu-id="7d549-201">Po usunięciu typu złożonego typ jest usuwany z modelu koncepcyjnego i mapowania dla wszystkich wystąpień tego typu są usuwane.</span><span class="sxs-lookup"><span data-stu-id="7d549-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="7d549-202">Odwołania do tego typu nie są jednak aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="7d549-202">However, references to the type are not updated.</span></span> <span data-ttu-id="7d549-203">Na przykład jeśli jednostka ma właściwość typu złożonego typu ComplexType1, a ComplexType1 jest usuwana w **przeglądarce modelu**, odpowiednia właściwość jednostki nie jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="7d549-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="7d549-204">Model nie zostanie sprawdzony, ponieważ zawiera jednostkę, która odwołuje się do usuniętego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="7d549-205">Można zaktualizować lub usunąć odwołania do usuniętych typów złożonych przy użyciu Entity Designer.</span><span class="sxs-lookup"><span data-stu-id="7d549-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="7d549-206">Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu i wybierz polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="7d549-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="7d549-207">Oraz</span><span class="sxs-lookup"><span data-stu-id="7d549-207">OR -</span></span>

-   <span data-ttu-id="7d549-208">W przeglądarce modeli wybierz typ złożony i naciśnij klawisz DELETE na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="7d549-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="7d549-209">Zapytanie dotyczące jednostek zawierających właściwości typu złożonego</span><span class="sxs-lookup"><span data-stu-id="7d549-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="7d549-210">Poniższy kod pokazuje, jak wykonać zapytanie, które zwraca kolekcję obiektów typu jednostki, które zawierają właściwość typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7d549-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
