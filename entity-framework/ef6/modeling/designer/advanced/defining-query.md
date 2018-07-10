---
title: Definiowanie zapytania — projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912698"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="1857a-102">Definiowanie zapytania — projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="1857a-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="1857a-103">W tym instruktażu przedstawiono sposób dodawania, definiując kwerendy i odpowiednia jednostka typu do modelu, używając projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="1857a-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="1857a-104">Definiowanie zapytania jest najczęściej używany do zapewnia funkcje podobne do dostarczony przez widok bazy danych, ale widok jest zdefiniowany w modelu, a nie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1857a-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="1857a-105">Definiowanie zapytań pozwala wykonać instrukcję SQL, który jest określony w **DefiningQuery** element z pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="1857a-106">Aby uzyskać więcej informacji, zobacz **DefiningQuery** w [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="1857a-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="1857a-107">Korzystając z Definiowanie zapytań, należy zdefiniować typ jednostki w modelu.</span><span class="sxs-lookup"><span data-stu-id="1857a-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="1857a-108">Typ jednostki jest używany do powierzchni dane udostępniane przez definiowanie zapytań.</span><span class="sxs-lookup"><span data-stu-id="1857a-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="1857a-109">Należy pamiętać, że udostępniane za pośrednictwem tego typu jednostki danych tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1857a-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="1857a-110">Nie można wykonać zapytań sparametryzowanych jako Definiowanie zapytań.</span><span class="sxs-lookup"><span data-stu-id="1857a-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="1857a-111">Jednak dane można zaktualizować przez mapowanie insert, update i delete funkcji typu jednostki który uwypukli najistotniejsze dane do procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="1857a-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="1857a-112">Aby uzyskać więcej informacji, zobacz [wstawiania, aktualizowania i usuwania przy użyciu procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="1857a-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="1857a-113">W tym temacie przedstawiono sposób wykonywania następujących zadań.</span><span class="sxs-lookup"><span data-stu-id="1857a-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="1857a-114">Dodaj zapytanie definiujące</span><span class="sxs-lookup"><span data-stu-id="1857a-114">Add a Defining Query</span></span>
-   <span data-ttu-id="1857a-115">Dodaj typ jednostki do modelu</span><span class="sxs-lookup"><span data-stu-id="1857a-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="1857a-116">Definiowanie zapytania do typu jednostki mapy</span><span class="sxs-lookup"><span data-stu-id="1857a-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1857a-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1857a-117">Prerequisites</span></span>

<span data-ttu-id="1857a-118">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="1857a-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="1857a-119">Najnowszą wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1857a-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="1857a-120">[Przykładowej bazy danych School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="1857a-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1857a-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="1857a-121">Set up the Project</span></span>

<span data-ttu-id="1857a-122">Ten przewodnik korzysta z programu Visual Studio 2012 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="1857a-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="1857a-123">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1857a-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="1857a-124">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1857a-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="1857a-125">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **aplikację Konsolową** szablonu.</span><span class="sxs-lookup"><span data-stu-id="1857a-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="1857a-126">Wprowadź **DefiningQuerySample** jako nazwę projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1857a-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="1857a-127">Tworzenie modelu, w oparciu o bazę danych School</span><span class="sxs-lookup"><span data-stu-id="1857a-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="1857a-128">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="1857a-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="1857a-129">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.</span><span class="sxs-lookup"><span data-stu-id="1857a-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="1857a-130">Wprowadź **DefiningQueryModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="1857a-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="1857a-131">W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1857a-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="1857a-132">Kliknij przycisk nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="1857a-132">Click New Connection.</span></span> <span data-ttu-id="1857a-133">W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1857a-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="1857a-134">Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1857a-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="1857a-135">W oknie dialogowym Wybierz obiekty bazy danych sprawdź **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="1857a-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="1857a-136">Spowoduje to dodanie wszystkich tabel do **School** modelu.</span><span class="sxs-lookup"><span data-stu-id="1857a-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="1857a-137">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="1857a-137">Click **Finish**.</span></span>
-   <span data-ttu-id="1857a-138">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **DefiningQueryModel.edmx** plik i wybierz **Otwórz za pomocą...** .</span><span class="sxs-lookup"><span data-stu-id="1857a-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="1857a-139">Wybierz **Edytor (tekstu) XML**.</span><span class="sxs-lookup"><span data-stu-id="1857a-139">Select **XML (Text) Editor**.</span></span>

    ![Edytorem XMLEditor](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="1857a-141">Kliknij przycisk **tak** Jeśli zostanie wyświetlony monit z następującym komunikatem:</span><span class="sxs-lookup"><span data-stu-id="1857a-141">Click **Yes** if prompted with the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="1857a-143">Dodaj zapytanie definiujące</span><span class="sxs-lookup"><span data-stu-id="1857a-143">Add a Defining Query</span></span>

<span data-ttu-id="1857a-144">W tym kroku, które zostaną użyte zapytanie edytora XML, aby dodać definiujące, a następnie wpisz jednostki SSDL części pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="1857a-145">Dodaj **EntitySet** elementu SSDL części pliku edmx (wiersz 5 do 13).</span><span class="sxs-lookup"><span data-stu-id="1857a-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="1857a-146">Określ następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="1857a-146">Specify the following:</span></span>
    -   <span data-ttu-id="1857a-147">Tylko **nazwa** i **EntityType** atrybuty **EntitySet** są określony element.</span><span class="sxs-lookup"><span data-stu-id="1857a-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="1857a-148">W pełni kwalifikowana nazwa typu jednostki jest używany w **EntityType** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1857a-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="1857a-149">Instrukcja SQL do wykonania jest określona w **DefiningQuery** elementu.</span><span class="sxs-lookup"><span data-stu-id="1857a-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="1857a-150">Dodaj **EntityType** element do sekcji SSDL edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="1857a-151">Plik jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="1857a-151">file as shown below.</span></span> <span data-ttu-id="1857a-152">Należy pamiętać o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="1857a-152">Note the following:</span></span>
    -   <span data-ttu-id="1857a-153">Wartość **nazwa** atrybut odnosi się do wartości **EntityType** atrybutu w **EntitySet** element powyżej, mimo że w pełni kwalifikowana nazwa Typ jednostki jest używany w **EntityType** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1857a-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="1857a-154">Nazwy właściwości odpowiadają nazwom kolumny zwróconą przez instrukcję SQL w **DefiningQuery** elementu (powyżej).</span><span class="sxs-lookup"><span data-stu-id="1857a-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="1857a-155">W tym przykładzie klucz jednostki składa się z trzech właściwości w celu zapewnienia unikalną wartość kluczową.</span><span class="sxs-lookup"><span data-stu-id="1857a-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="1857a-156">Jeśli później uruchomić **Kreator modelu aktualizacji** okno dialogowe, wszelkie zmiany wprowadzone do modelu magazynu, w tym Definiowanie zapytań, zostaną zastąpione.</span><span class="sxs-lookup"><span data-stu-id="1857a-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="1857a-157">Dodaj typ jednostki do modelu</span><span class="sxs-lookup"><span data-stu-id="1857a-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="1857a-158">W tym kroku zostanie dodany typ jednostki do modelu koncepcyjnego, za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="1857a-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="1857a-159">Należy pamiętać o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="1857a-159">Note the following:</span></span>

-   <span data-ttu-id="1857a-160">**Nazwa** jednostki odnosi się do wartości **EntityType** atrybutu w **EntitySet** powyżej elementu.</span><span class="sxs-lookup"><span data-stu-id="1857a-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="1857a-161">Nazwy właściwości odpowiadają nazwom kolumny zwróconą przez instrukcję SQL w **DefiningQuery** powyżej elementu.</span><span class="sxs-lookup"><span data-stu-id="1857a-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="1857a-162">W tym przykładzie klucz jednostki składa się z trzech właściwości w celu zapewnienia unikalną wartość kluczową.</span><span class="sxs-lookup"><span data-stu-id="1857a-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="1857a-163">W Projektancie platformy EF, należy otworzyć modelu.</span><span class="sxs-lookup"><span data-stu-id="1857a-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="1857a-164">Kliknij dwukrotnie DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="1857a-165">Powiedz **tak** się następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="1857a-165">Say **Yes** to the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

<span data-ttu-id="1857a-167">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="1857a-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="1857a-168">Kliknij prawym przyciskiem myszy Projektanta powierzchni i wybierz **Dodaj nowe**-&gt;**jednostki...** .</span><span class="sxs-lookup"><span data-stu-id="1857a-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="1857a-169">Określ **GradeReport** dla nazwy jednostki i **CourseID** dla **właściwości klucza**.</span><span class="sxs-lookup"><span data-stu-id="1857a-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="1857a-170">Kliknij prawym przyciskiem myszy **GradeReport** jednostki, a następnie wybierz pozycję **Dodaj nowe** - &gt; **właściwość skalarną**.</span><span class="sxs-lookup"><span data-stu-id="1857a-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="1857a-171">Zmienić domyślną nazwę właściwości do **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="1857a-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="1857a-172">Dodaj inną właściwość skalarną i określ **LastName** dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="1857a-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="1857a-173">Dodaj inną właściwość skalarną i określ **klasy korporacyjnej** dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="1857a-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="1857a-174">W **właściwości** oknie zmiany **klasy korporacyjnej**firmy **typu** właściwości **dziesiętna**.</span><span class="sxs-lookup"><span data-stu-id="1857a-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="1857a-175">Wybierz **FirstName** i **LastName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="1857a-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="1857a-176">W **właściwości** oknie zmiany **EntityKey** wartość właściwości **True**.</span><span class="sxs-lookup"><span data-stu-id="1857a-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="1857a-177">W rezultacie, następujące elementy zostały dodane do **CSDL** części pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="1857a-178">Definiowanie zapytania do typu jednostki mapy</span><span class="sxs-lookup"><span data-stu-id="1857a-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="1857a-179">W tym kroku użyjemy, okno Szczegóły mapowania, aby zamapować koncepcyjnej i typy jednostek magazynu.</span><span class="sxs-lookup"><span data-stu-id="1857a-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="1857a-180">Kliknij prawym przyciskiem myszy **GradeReport** jednostki na projekt powierzchni i wybierz **mapowania tabeli,**.</span><span class="sxs-lookup"><span data-stu-id="1857a-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="1857a-181">**Szczegóły mapowania** zostanie wyświetlone okno.</span><span class="sxs-lookup"><span data-stu-id="1857a-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="1857a-182">Wybierz **GradeReport** z **&lt;Dodaj tabelę lub widok&gt;** listy rozwijanej (znajdującej się w **tabeli**s).</span><span class="sxs-lookup"><span data-stu-id="1857a-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="1857a-183">Domyślne mapowania między koncepcyjnej i magazynu **GradeReport** typu jednostki są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1857a-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="1857a-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="1857a-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="1857a-185">W rezultacie **obiekcie EntitySetMapping** element zostanie dodany do sekcji mapowania pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="1857a-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="1857a-186">Kompilowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1857a-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="1857a-187">Wywołaj Definiowanie zapytania w kodzie</span><span class="sxs-lookup"><span data-stu-id="1857a-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="1857a-188">Definiowanie zapytania można teraz wykonać przy użyciu **GradeReport** typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="1857a-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
