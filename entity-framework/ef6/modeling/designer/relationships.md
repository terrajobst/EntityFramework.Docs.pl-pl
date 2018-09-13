---
title: Relacje — projektancie platformy EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490686"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="35b77-102">Relacje — projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="35b77-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="35b77-103">Ta strona zawiera informacje o konfigurowaniu relacje w modelu przy użyciu projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="35b77-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="35b77-104">Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="35b77-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="35b77-105">Skojarzenia definiowania relacji między typami encji w modelu.</span><span class="sxs-lookup"><span data-stu-id="35b77-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="35b77-106">W tym temacie pokazano, jak mapować skojarzenia z Entity Framework Designer (Projektant EF).</span><span class="sxs-lookup"><span data-stu-id="35b77-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="35b77-107">Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="35b77-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Projektancie platformy EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="35b77-109">Podczas tworzenia modelu koncepcyjnego ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów.</span><span class="sxs-lookup"><span data-stu-id="35b77-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="35b77-110">Można zignorować te ostrzeżenia, ponieważ po dokonaniu wyboru wygenerować bazę danych z modelu, błędy znikną.</span><span class="sxs-lookup"><span data-stu-id="35b77-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="35b77-111">Omówienie skojarzenia</span><span class="sxs-lookup"><span data-stu-id="35b77-111">Associations Overview</span></span>

<span data-ttu-id="35b77-112">Podczas projektowania modelu przy użyciu projektanta EF plik edmx reprezentuje model.</span><span class="sxs-lookup"><span data-stu-id="35b77-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="35b77-113">W pliku edmx **skojarzenia** element definiuje relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="35b77-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="35b77-114">Skojarzenia należy określić typy jednostek, które są zaangażowane w relacji i możliwa liczba typów jednostek na każdym końcu relacji, który jest znany jako liczebności.</span><span class="sxs-lookup"><span data-stu-id="35b77-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="35b77-115">Liczebność elementu end skojarzenia mogą mieć wartość równą jeden (1), zero lub jeden (od 0 do 1) lub wielu (\*).</span><span class="sxs-lookup"><span data-stu-id="35b77-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="35b77-116">Informacja ta jest określona w dwóch podrzędny **zakończenia** elementów.</span><span class="sxs-lookup"><span data-stu-id="35b77-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="35b77-117">W czasie wykonywania wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub klucze obce (Jeśli użytkownik chce udostępnić klucze obce w jednostkach).</span><span class="sxs-lookup"><span data-stu-id="35b77-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="35b77-118">Za pomocą kluczy obcych widoczne, relacji między jednostkami odbywa się za pomocą **ReferentialConstraint** elementu (element podrzędny **skojarzenia** elementu).</span><span class="sxs-lookup"><span data-stu-id="35b77-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="35b77-119">Zalecane jest, należy zawsze udostępnić klucze obce w przypadku relacji w jednostkach.</span><span class="sxs-lookup"><span data-stu-id="35b77-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="35b77-120">W wielu do wielu (\*:\*) klucze obce nie można dodać do jednostki.</span><span class="sxs-lookup"><span data-stu-id="35b77-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="35b77-121">W \*:\* relacji, informacji o skojarzeniu odbywa się za pomocą tworzenie niezależnych obiektów.</span><span class="sxs-lookup"><span data-stu-id="35b77-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="35b77-122">Aby uzyskać informacje o elementach CSDL (**ReferentialConstraint**, **skojarzenia**, itp.) zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="35b77-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="35b77-123">Tworzenie i usuwanie skojarzenia</span><span class="sxs-lookup"><span data-stu-id="35b77-123">Create and Delete Associations</span></span>

<span data-ttu-id="35b77-124">Tworzenie skojarzenia z aktualizacjami projektancie platformy EF modelu zawartości pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="35b77-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="35b77-125">Po utworzeniu skojarzenia, należy utworzyć mapowania dla skojarzenia (zostało to omówione w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="35b77-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="35b77-126">W tej sekcji założono, dodano już jednostkami, z którymi chcesz utworzyć skojarzenie między do modelu.</span><span class="sxs-lookup"><span data-stu-id="35b77-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="35b77-127">Aby utworzyć skojarzenie</span><span class="sxs-lookup"><span data-stu-id="35b77-127">To create an association</span></span>

1.  <span data-ttu-id="35b77-128">Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i wybierz **skojarzenie...** .</span><span class="sxs-lookup"><span data-stu-id="35b77-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="35b77-129">Wypełnij ustawienia dla skojarzenia w **Dodawanie skojarzenia** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="35b77-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Dodaj skojarzenie](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="35b77-131">Użytkownik może nie dodać właściwości nawigacji lub właściwości klucza obcego z jednostkami: końcach asocjacji, czyszcząc \*\* właściwość nawigacji \*\* i \*\* dodać właściwości klucza obcego do &lt;Nazwa typu jednostki&gt; jednostki \*\* pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="35b77-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the \*\*Navigation Property \*\*and \*\*Add foreign key properties to the &lt;entity type name&gt; Entity \*\*checkboxes.</span></span> <span data-ttu-id="35b77-132">Jeśli dodasz tylko jedną właściwość nawigacji, stowarzyszenia będą traversable tylko w jednym kierunku.</span><span class="sxs-lookup"><span data-stu-id="35b77-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="35b77-133">Jeśli dodasz żadnych właściwości nawigacji, użytkownik musi dodać właściwości klucza obcego w celu uzyskania dostępu do jednostek na końcach asocjacji.</span><span class="sxs-lookup"><span data-stu-id="35b77-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="35b77-134">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="35b77-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="35b77-135">Aby usunąć skojarzenie</span><span class="sxs-lookup"><span data-stu-id="35b77-135">To delete an association</span></span>

<span data-ttu-id="35b77-136">Aby usunąć czy skojarzenie, jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="35b77-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="35b77-137">Kliknij prawym przyciskiem myszy skojarzenia w Projektancie platformy EF powierzchni i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="35b77-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="35b77-138">LUB —</span><span class="sxs-lookup"><span data-stu-id="35b77-138">OR -</span></span>

-   <span data-ttu-id="35b77-139">Wybierz jeden lub więcej skojarzeń, a następnie naciśnij klawisz DELETE.</span><span class="sxs-lookup"><span data-stu-id="35b77-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="35b77-140">Zawierają właściwości klucza obcego w jednostkach (ograniczenia referencyjne)</span><span class="sxs-lookup"><span data-stu-id="35b77-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="35b77-141">Zalecane jest, należy zawsze udostępnić klucze obce w przypadku relacji w jednostkach.</span><span class="sxs-lookup"><span data-stu-id="35b77-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="35b77-142">Platformy Entity Framework używa ograniczenia referencyjnego do identyfikowania, że właściwość działa jako klucz obcy relacji.</span><span class="sxs-lookup"><span data-stu-id="35b77-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="35b77-143">Gdy zaznaczono ***właściwości klucza obcego, aby dodać &lt;Nazwa typu jednostki&gt; jednostki*** pola wyboru przy tworzeniu relacji, to ograniczenie referencyjne została dodana dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="35b77-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="35b77-144">Gdy używasz projektancie platformy EF umożliwiają dodawanie lub edytowanie ograniczenia referencyjnego projektancie platformy EF dodaje lub modyfikuje **ReferentialConstraint** element CSDL zawartość pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="35b77-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="35b77-145">Kliknij dwukrotnie skojarzenia, które chcesz edytować.</span><span class="sxs-lookup"><span data-stu-id="35b77-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="35b77-146">**Ograniczenia referencyjnego** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="35b77-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="35b77-147">Z **jednostki** listy rozwijanej wybierz jednostkę główną w ograniczeniu referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="35b77-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="35b77-148">Właściwości klucza jednostki są dodawane do **klucz jednostki** listy w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="35b77-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="35b77-149">Z **zależne** listy rozwijanej wybierz jednostki zależne w ograniczeniu referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="35b77-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="35b77-150">Dla każdego klucza podmiotu zabezpieczeń, kluczu zależnych, wybierz odpowiedni klucz zależne z listy rozwijanej w **zależne klucz** kolumny.</span><span class="sxs-lookup"><span data-stu-id="35b77-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Ograniczenie REF](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="35b77-152">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="35b77-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="35b77-153">Tworzenie i edytowanie mapowania skojarzenia</span><span class="sxs-lookup"><span data-stu-id="35b77-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="35b77-154">Można określić sposób mapowania bazy danych w skojarzenie **szczegóły mapowania** okna projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="35b77-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="35b77-155">Można mapować tylko szczegółów dotyczących skojarzeń, które nie mają określone ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="35b77-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="35b77-156">Jeśli określono ograniczenia referencyjnego właściwości klucza obcego znajduje się w jednostce i szczegóły mapowania można użyć tej jednostki na kolumny klucza obcego mapuje do kontroli.</span><span class="sxs-lookup"><span data-stu-id="35b77-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="35b77-157">Utwórz mapowanie skojarzenia</span><span class="sxs-lookup"><span data-stu-id="35b77-157">Create an association mapping</span></span>

-   <span data-ttu-id="35b77-158">Kliknij prawym przyciskiem myszy skojarzenie w projekt powierzchni i wybierz **mapowania tabeli,**.</span><span class="sxs-lookup"><span data-stu-id="35b77-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="35b77-159">Spowoduje to wyświetlenie mapowanie skojarzenia w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="35b77-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="35b77-160">Kliknij przycisk **Dodaj tabelę lub widok**.</span><span class="sxs-lookup"><span data-stu-id="35b77-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="35b77-161">Listy rozwijanej pojawia się, który zawiera wszystkie tabele w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="35b77-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="35b77-162">Wybierz tabelę, do której będzie zmapowana skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="35b77-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="35b77-163">**Szczegóły mapowania** okno wyświetla obu końcach asocjacji i właściwości klucza dla typu jednostki, w każdej **zakończenia**.</span><span class="sxs-lookup"><span data-stu-id="35b77-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="35b77-164">Dla każdej właściwości klucza, kliknij przycisk **kolumny** pola, a następnie wybierz kolumnę, do którego będzie zmapowana właściwości.</span><span class="sxs-lookup"><span data-stu-id="35b77-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Szczegóły mapowania 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="35b77-166">Edytuj mapowanie skojarzenia</span><span class="sxs-lookup"><span data-stu-id="35b77-166">Edit an association mapping</span></span>

-   <span data-ttu-id="35b77-167">Kliknij prawym przyciskiem myszy skojarzenie w projekt powierzchni i wybierz **mapowania tabeli,**.</span><span class="sxs-lookup"><span data-stu-id="35b77-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="35b77-168">Spowoduje to wyświetlenie mapowanie skojarzenia w **szczegóły mapowania** okna.</span><span class="sxs-lookup"><span data-stu-id="35b77-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="35b77-169">Kliknij przycisk **mapuje &lt;nazwy tabeli&gt;**.</span><span class="sxs-lookup"><span data-stu-id="35b77-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="35b77-170">Listy rozwijanej pojawia się, który zawiera wszystkie tabele w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="35b77-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="35b77-171">Wybierz tabelę, do której będzie zmapowana skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="35b77-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="35b77-172">**Szczegóły mapowania** okno wyświetla obu końcach asocjacji i właściwości klucza dla typu jednostki, na każdym końcu.</span><span class="sxs-lookup"><span data-stu-id="35b77-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="35b77-173">Dla każdej właściwości klucza, kliknij przycisk **kolumny** pola, a następnie wybierz kolumnę, do którego będzie zmapowana właściwości.</span><span class="sxs-lookup"><span data-stu-id="35b77-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="35b77-174">Edytuj i Usuń właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="35b77-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="35b77-175">Właściwości nawigacji są właściwościami skrótów, które są używane do lokalizowania jednostek na końcach asocjacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="35b77-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="35b77-176">Podczas tworzenia skojarzenia między dwoma typami jednostki można utworzyć właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="35b77-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="35b77-177">Aby edytować właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="35b77-177">To edit navigation properties</span></span>

-   <span data-ttu-id="35b77-178">Wybierz właściwość nawigacji na powierzchni projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="35b77-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="35b77-179">W programie Visual Studio wyświetlane są informacje dotyczące właściwości nawigacji **właściwości** okna.</span><span class="sxs-lookup"><span data-stu-id="35b77-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="35b77-180">Zmień ustawienia właściwości **właściwości** okna.</span><span class="sxs-lookup"><span data-stu-id="35b77-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="35b77-181">Można usunąć właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="35b77-181">To delete navigation properties</span></span>

-   <span data-ttu-id="35b77-182">Jeśli klucze obce nie są widoczne na typy jednostek w modelu koncepcyjnym, usunięcie właściwości nawigacji może spowodować odpowiedniego skojarzenia traversable tylko w jednym kierunku lub nie traversable wcale.</span><span class="sxs-lookup"><span data-stu-id="35b77-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="35b77-183">Kliknij prawym przyciskiem myszy właściwość nawigacji w Projektancie platformy EF powierzchni i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="35b77-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
