---
title: Relacje-Dr Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418246"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="019e6-102">Relacje — Projektant EF</span><span class="sxs-lookup"><span data-stu-id="019e6-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="019e6-103">Ta strona zawiera informacje o konfigurowaniu relacji w modelu przy użyciu narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="019e6-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="019e6-104">Aby uzyskać ogólne informacje na temat relacji w EF oraz jak uzyskać dostęp do danych i manipulować nimi przy użyciu relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="019e6-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="019e6-105">Skojarzenia definiują relacje między typami jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="019e6-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="019e6-106">W tym temacie przedstawiono sposób mapowania skojarzeń z Entity Framework Designer (program EF Designer).</span><span class="sxs-lookup"><span data-stu-id="019e6-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="019e6-107">Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="019e6-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektant EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="019e6-109">Podczas budowania modelu koncepcyjnego w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="019e6-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="019e6-110">Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.</span><span class="sxs-lookup"><span data-stu-id="019e6-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="019e6-111">Omówienie skojarzeń</span><span class="sxs-lookup"><span data-stu-id="019e6-111">Associations Overview</span></span>

<span data-ttu-id="019e6-112">Podczas projektowania modelu przy użyciu narzędzia Dr Designer plik. edmx reprezentuje model.</span><span class="sxs-lookup"><span data-stu-id="019e6-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="019e6-113">W pliku. edmx element **Association** definiuje relację między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="019e6-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="019e6-114">Skojarzenie musi określać typy jednostek, które są uwzględnione w relacji, oraz liczbę typów jednostek na każdym końcu relacji, która jest znana jako liczebność.</span><span class="sxs-lookup"><span data-stu-id="019e6-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="019e6-115">Liczebność elementu end skojarzenia może mieć wartość jeden (1), zero lub jeden (0.. 1) lub wiele (\*).</span><span class="sxs-lookup"><span data-stu-id="019e6-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="019e6-116">Te informacje są określone w dwóch podrzędnych elementach **End** .</span><span class="sxs-lookup"><span data-stu-id="019e6-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="019e6-117">W czasie wykonywania wystąpienia typu jednostki na jednym końcu skojarzenia są dostępne za poorednictwem właściwości nawigacji lub kluczy obcych (Jeśli zdecydujesz się uwidocznić klucze obce w jednostkach).</span><span class="sxs-lookup"><span data-stu-id="019e6-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="019e6-118">Gdy klucze obce są uwidocznione, relacje między jednostkami są zarządzane za pomocą elementu **ReferentialConstraint** (element podrzędny elementu **Association** ).</span><span class="sxs-lookup"><span data-stu-id="019e6-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="019e6-119">Zaleca się, aby zawsze ujawniać klucze obce dla relacji w jednostkach.</span><span class="sxs-lookup"><span data-stu-id="019e6-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="019e6-120">W przypadku wielu do wielu (\*:\*) nie można dodać kluczy obcych do jednostek.</span><span class="sxs-lookup"><span data-stu-id="019e6-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="019e6-121">W \*:\*, informacje o skojarzeniu są zarządzane za pomocą niezależnego obiektu.</span><span class="sxs-lookup"><span data-stu-id="019e6-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="019e6-122">Aby uzyskać informacje na temat elementów CSDL (**ReferentialConstraint**, **Association**itp.), zobacz [specyfikację CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="019e6-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="019e6-123">Tworzenie i usuwanie skojarzeń</span><span class="sxs-lookup"><span data-stu-id="019e6-123">Create and Delete Associations</span></span>

<span data-ttu-id="019e6-124">Tworzenie skojarzenia przy użyciu narzędzia Dr Designer aktualizuje zawartość modelu pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="019e6-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="019e6-125">Po utworzeniu skojarzenia należy utworzyć mapowania dla skojarzenia (omówione w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="019e6-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="019e6-126">W tej sekcji założono, że dodano już jednostki, dla których chcesz utworzyć skojarzenie między modelem.</span><span class="sxs-lookup"><span data-stu-id="019e6-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="019e6-127">Aby utworzyć skojarzenie</span><span class="sxs-lookup"><span data-stu-id="019e6-127">To create an association</span></span>

1.  <span data-ttu-id="019e6-128">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowy**i wybierz pozycję **kojarzenie...** .</span><span class="sxs-lookup"><span data-stu-id="019e6-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="019e6-129">Wypełnij ustawienia skojarzenia w oknie dialogowym **Dodawanie skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="019e6-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Dodaj skojarzenie](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="019e6-131">Można wybrać, aby nie dodawać właściwości nawigacji lub właściwości klucza obcego do jednostek na końcu skojarzenia przez wyczyszczenie **właściwości nawigacji **i **Dodawanie właściwości klucza obcego do &lt;nazwy typu jednostki&gt; **pola wyboru jednostki.</span><span class="sxs-lookup"><span data-stu-id="019e6-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="019e6-132">Jeśli dodasz tylko jedną właściwość nawigacji, skojarzenie będzie przepływać tylko w jednym kierunku.</span><span class="sxs-lookup"><span data-stu-id="019e6-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="019e6-133">Jeśli dodasz brak właściwości nawigacji, musisz dodać właściwości klucza obcego w celu uzyskania dostępu do jednostek na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="019e6-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="019e6-134">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="019e6-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="019e6-135">Aby usunąć skojarzenie</span><span class="sxs-lookup"><span data-stu-id="019e6-135">To delete an association</span></span>

<span data-ttu-id="019e6-136">Aby usunąć skojarzenie, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="019e6-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="019e6-137">Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektanta EF i wybierz polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="019e6-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="019e6-138">Oraz</span><span class="sxs-lookup"><span data-stu-id="019e6-138">OR -</span></span>

-   <span data-ttu-id="019e6-139">Wybierz co najmniej jedno skojarzenie i naciśnij klawisz DELETE.</span><span class="sxs-lookup"><span data-stu-id="019e6-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="019e6-140">Uwzględnij właściwości klucza obcego w jednostkach (ograniczenia referencyjne)</span><span class="sxs-lookup"><span data-stu-id="019e6-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="019e6-141">Zaleca się, aby zawsze ujawniać klucze obce dla relacji w jednostkach.</span><span class="sxs-lookup"><span data-stu-id="019e6-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="019e6-142">Entity Framework używa ograniczenia referencyjnego, aby określić, że Właściwość działa jako klucz obcy dla relacji.</span><span class="sxs-lookup"><span data-stu-id="019e6-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="019e6-143">Jeśli zaznaczono pole wyboru ***Dodaj właściwości klucza obcego do &lt;nazwy typu jednostki&gt; jednostki*** podczas tworzenia relacji, to ograniczenie referencyjne zostało dodane dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="019e6-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="019e6-144">W przypadku dodawania lub edytowania ograniczenia referencyjnego za pomocą programu EF Projektant EF dodaje lub modyfikuje element **ReferentialConstraint** w zawartości CSDL pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="019e6-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="019e6-145">Kliknij dwukrotnie skojarzenie, które chcesz edytować.</span><span class="sxs-lookup"><span data-stu-id="019e6-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="019e6-146">Zostanie wyświetlone okno dialogowe **ograniczenie referencyjne** .</span><span class="sxs-lookup"><span data-stu-id="019e6-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="019e6-147">Z listy rozwijanej  **głównej** wybierz jednostkę główną w ograniczeniu referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="019e6-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="019e6-148">Właściwości klucza jednostki są dodawane do listy **klucz podmiotu zabezpieczeń** w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="019e6-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="019e6-149">Z listy rozwijanej  **zależne** wybierz jednostkę zależną w ograniczeniu referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="019e6-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="019e6-150">Dla każdego klucza podmiotu zabezpieczeń, który ma klucz zależny, wybierz odpowiedni klucz zależny z listy rozwijanej w kolumnie **klucz zależny** .</span><span class="sxs-lookup"><span data-stu-id="019e6-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Ref — ograniczenie](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="019e6-152">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="019e6-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="019e6-153">Tworzenie i edytowanie mapowań skojarzeń</span><span class="sxs-lookup"><span data-stu-id="019e6-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="019e6-154">Można określić sposób mapowania skojarzenia do bazy danych w oknie **szczegóły mapowania** w programie Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="019e6-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="019e6-155">Można mapować tylko szczegóły dla skojarzeń, które nie mają określonego ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="019e6-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="019e6-156">Jeśli określono ograniczenie referencyjne, właściwość klucza obcego jest uwzględniona w jednostce i można użyć szczegółów mapowania dla jednostki, aby określić kolumnę, do której jest mapowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="019e6-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="019e6-157">Tworzenie mapowania skojarzenia</span><span class="sxs-lookup"><span data-stu-id="019e6-157">Create an association mapping</span></span>

-   <span data-ttu-id="019e6-158">Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.</span><span class="sxs-lookup"><span data-stu-id="019e6-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="019e6-159">Spowoduje to wyświetlenie mapowania skojarzenia w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="019e6-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="019e6-160">Kliknij pozycję **Dodaj tabelę lub widok**.</span><span class="sxs-lookup"><span data-stu-id="019e6-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="019e6-161">Zostanie wyświetlona lista rozwijana zawierająca wszystkie tabele w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="019e6-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="019e6-162">Wybierz tabelę, do której zostanie zmapowane skojarzenie.</span><span class="sxs-lookup"><span data-stu-id="019e6-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="019e6-163">W oknie **szczegóły mapowania** są wyświetlane końce skojarzenia i właściwości klucza dla typu jednostki na każdym **końcu**.</span><span class="sxs-lookup"><span data-stu-id="019e6-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="019e6-164">Dla każdej właściwości klucza kliknij pole  **kolumny** , a następnie wybierz kolumnę, do której zostanie zamapowana właściwość.</span><span class="sxs-lookup"><span data-stu-id="019e6-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Szczegóły mapowania 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="019e6-166">Edytowanie mapowania skojarzenia</span><span class="sxs-lookup"><span data-stu-id="019e6-166">Edit an association mapping</span></span>

-   <span data-ttu-id="019e6-167">Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.</span><span class="sxs-lookup"><span data-stu-id="019e6-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="019e6-168">Spowoduje to wyświetlenie mapowania skojarzenia w oknie **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="019e6-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="019e6-169">Kliknij pozycję **mapy, aby &lt;nazwę tabeli&gt;** .</span><span class="sxs-lookup"><span data-stu-id="019e6-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="019e6-170">Zostanie wyświetlona lista rozwijana zawierająca wszystkie tabele w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="019e6-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="019e6-171">Wybierz tabelę, do której zostanie zmapowane skojarzenie.</span><span class="sxs-lookup"><span data-stu-id="019e6-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="019e6-172">W oknie **szczegóły mapowania** są wyświetlane końce skojarzenia i właściwości klucza dla typu jednostki na każdym końcu.</span><span class="sxs-lookup"><span data-stu-id="019e6-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="019e6-173">Dla każdej właściwości klucza kliknij pole  **kolumny** , a następnie wybierz kolumnę, do której zostanie zamapowana właściwość.</span><span class="sxs-lookup"><span data-stu-id="019e6-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="019e6-174">Edytuj i Usuń właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="019e6-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="019e6-175">Właściwości nawigacji są właściwościami skrótu, które są używane do lokalizowania jednostek na końcu skojarzenia w modelu.</span><span class="sxs-lookup"><span data-stu-id="019e6-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="019e6-176">Właściwości nawigacji można utworzyć podczas tworzenia skojarzenia między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="019e6-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="019e6-177">Aby edytować właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="019e6-177">To edit navigation properties</span></span>

-   <span data-ttu-id="019e6-178">Wybierz właściwość nawigacji na powierzchni projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="019e6-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="019e6-179">Informacje o właściwości nawigacji są wyświetlane w oknie **Właściwości** programu Visual Studio .</span><span class="sxs-lookup"><span data-stu-id="019e6-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="019e6-180">Zmień ustawienia właściwości w oknie **właściwości** .</span><span class="sxs-lookup"><span data-stu-id="019e6-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="019e6-181">Aby usunąć właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="019e6-181">To delete navigation properties</span></span>

-   <span data-ttu-id="019e6-182">Jeśli klucze obce nie są ujawnione w typach jednostek w modelu koncepcyjnym, usunięcie właściwości nawigacji może spowodować, że odpowiednie skojarzenie zostanie przesunięte tylko w jednym kierunku lub w ogóle nie przechodzą.</span><span class="sxs-lookup"><span data-stu-id="019e6-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="019e6-183">Kliknij prawym przyciskiem myszy właściwość nawigacji na powierzchni projektanta EF i wybierz polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="019e6-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
