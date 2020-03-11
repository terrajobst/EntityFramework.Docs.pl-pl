---
title: Definiowanie zapytania-Dr Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418799"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="45618-102">Definiowanie zapytania-Dr Designer</span><span class="sxs-lookup"><span data-stu-id="45618-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="45618-103">W tym instruktażu pokazano, jak dodać zapytanie definiujące i odpowiedni typ jednostki do modelu przy użyciu narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="45618-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="45618-104">Definiowanie zapytania jest często używane w celu zapewnienia funkcjonalności podobnej do tej, która jest udostępniana przez widok bazy danych, ale widok jest zdefiniowany w modelu, a nie w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="45618-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="45618-105">Definiowanie zapytania pozwala wykonać instrukcję SQL, która jest określona w **DefiningQuery** elementu pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="45618-106">Aby uzyskać więcej informacji, zobacz **DefiningQuery** w [specyfikacji SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="45618-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="45618-107">Podczas korzystania z definiowania zapytań należy również zdefiniować typ jednostki w modelu.</span><span class="sxs-lookup"><span data-stu-id="45618-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="45618-108">Typ jednostki jest używany do prezentowania danych przez definiowanie zapytania.</span><span class="sxs-lookup"><span data-stu-id="45618-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="45618-109">Należy pamiętać, że dane, które są udostępniane przez ten typ jednostki, są tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="45618-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="45618-110">Zapytania sparametryzowane nie mogą być wykonywane jako definiujące zapytania.</span><span class="sxs-lookup"><span data-stu-id="45618-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="45618-111">Dane można jednak zaktualizować, mapując funkcje INSERT, Update i DELETE klasy Entity, która wyświetla dane w procedurach składowanych.</span><span class="sxs-lookup"><span data-stu-id="45618-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="45618-112">Aby uzyskać więcej informacji, zobacz [Wstawianie, aktualizowanie i usuwanie za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="45618-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="45618-113">W tym temacie przedstawiono sposób wykonywania następujących zadań.</span><span class="sxs-lookup"><span data-stu-id="45618-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="45618-114">Dodawanie zapytania definiującego</span><span class="sxs-lookup"><span data-stu-id="45618-114">Add a Defining Query</span></span>
-   <span data-ttu-id="45618-115">Dodawanie typu jednostki do modelu</span><span class="sxs-lookup"><span data-stu-id="45618-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="45618-116">Mapowanie zapytania definiującego na typ jednostki</span><span class="sxs-lookup"><span data-stu-id="45618-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45618-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="45618-117">Prerequisites</span></span>

<span data-ttu-id="45618-118">W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="45618-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="45618-119">Najnowsza wersja programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45618-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="45618-120">[Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="45618-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="45618-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="45618-121">Set up the Project</span></span>

<span data-ttu-id="45618-122">W tym instruktażu jest używany program Visual Studio 2012 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="45618-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="45618-123">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45618-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="45618-124">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="45618-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="45618-125">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **Aplikacja konsolowa** .</span><span class="sxs-lookup"><span data-stu-id="45618-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="45618-126">Wprowadź **DefiningQuerySample** jako nazwę projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="45618-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="45618-127">Tworzenie modelu opartego na bazie danych szkoły</span><span class="sxs-lookup"><span data-stu-id="45618-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="45618-128">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="45618-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="45618-129">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="45618-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="45618-130">W polu Nazwa pliku wprowadź **DefiningQueryModel. edmx** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="45618-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="45618-131">W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="45618-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="45618-132">Kliknij pozycję nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="45618-132">Click New Connection.</span></span> <span data-ttu-id="45618-133">W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="45618-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="45618-134">Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="45618-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="45618-135">W oknie dialogowym Wybierz obiekty bazy danych sprawdź, czy **tabele** węzła.</span><span class="sxs-lookup"><span data-stu-id="45618-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="45618-136">Spowoduje to dodanie wszystkich tabel do modelu **szkoły** .</span><span class="sxs-lookup"><span data-stu-id="45618-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="45618-137">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="45618-137">Click **Finish**.</span></span>
-   <span data-ttu-id="45618-138">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy plik **DefiningQueryModel. edmx** i wybierz polecenie **Otwórz za pomocą...** .</span><span class="sxs-lookup"><span data-stu-id="45618-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="45618-139">Wybierz **Edytor XML (tekst)** .</span><span class="sxs-lookup"><span data-stu-id="45618-139">Select **XML (Text) Editor**.</span></span>

    ![Edytor XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="45618-141">Kliknij przycisk **tak** , jeśli zostanie wyświetlony monit z następującym komunikatem:</span><span class="sxs-lookup"><span data-stu-id="45618-141">Click **Yes** if prompted with the following message:</span></span>

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="45618-143">Dodawanie zapytania definiującego</span><span class="sxs-lookup"><span data-stu-id="45618-143">Add a Defining Query</span></span>

<span data-ttu-id="45618-144">W tym kroku użyjesz edytora XML, aby dodać zapytanie definiujące i typ jednostki do sekcji SSDL pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="45618-145">Dodaj element **EntitySet** do sekcji SSDL w pliku edmx (wiersz 5 – 13).</span><span class="sxs-lookup"><span data-stu-id="45618-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="45618-146">Określ następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="45618-146">Specify the following:</span></span>
    -   <span data-ttu-id="45618-147">Określono tylko **atrybuty ** i **EntityType** elementu **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="45618-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="45618-148">W pełni kwalifikowana nazwa typu jednostki jest używana w atrybucie  **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="45618-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="45618-149">Instrukcja SQL do wykonania jest określona w elemencie **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="45618-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="45618-150">Dodaj element **EntityType** do sekcji SSDL w obiekcie. edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="45618-151">plik, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="45618-151">file as shown below.</span></span> <span data-ttu-id="45618-152">Należy pamiętać o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="45618-152">Note the following:</span></span>
    -   <span data-ttu-id="45618-153">Wartość atrybutu **name** odnosi się do wartości atrybutu **EntityType** w elemencie **EntitySet** powyżej, chociaż w atrybucie **EntityType** jest używana w pełni kwalifikowana nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="45618-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="45618-154">Nazwy właściwości odpowiadają nazwom kolumn zwracanym przez instrukcję SQL w elemencie **DefiningQuery** (powyżej).</span><span class="sxs-lookup"><span data-stu-id="45618-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="45618-155">W tym przykładzie klucz jednostki składa się z trzech właściwości, aby zapewnić unikatową wartość klucza.</span><span class="sxs-lookup"><span data-stu-id="45618-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="45618-156">Jeśli później zostanie uruchomione okno dialogowe **Kreatora aktualizacji** , wszelkie zmiany wprowadzone do modelu magazynu, w tym Definiowanie zapytań, zostaną nadpisywane.</span><span class="sxs-lookup"><span data-stu-id="45618-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="45618-157">Dodawanie typu jednostki do modelu</span><span class="sxs-lookup"><span data-stu-id="45618-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="45618-158">W tym kroku dodamy typ jednostki do modelu koncepcyjnego za pomocą narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="45618-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="45618-159"> Zwróć uwagę na następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="45618-159"> Note the following:</span></span>

-   <span data-ttu-id="45618-160">**Nazwa** jednostki odpowiada wartości atrybutu **EntityType** w powyższym elemencie **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="45618-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="45618-161">Nazwy właściwości odpowiadają nazwom kolumn zwracanym przez instrukcję SQL w powyższym elemencie **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="45618-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="45618-162">W tym przykładzie klucz jednostki składa się z trzech właściwości, aby zapewnić unikatową wartość klucza.</span><span class="sxs-lookup"><span data-stu-id="45618-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="45618-163">Otwórz model w programie Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="45618-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="45618-164">Kliknij dwukrotnie plik DefiningQueryModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="45618-165">Załóżmy **,** że zostanie wyświetlony następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="45618-165">Say **Yes** to the following message:</span></span>

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="45618-167">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="45618-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="45618-168">Kliknij prawym przyciskiem myszy powierzchnię projektanta i wybierz polecenie **Dodaj nową**-&gt;**jednostki.** .</span><span class="sxs-lookup"><span data-stu-id="45618-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="45618-169">Określ **GradeReport** dla nazwy jednostki i **CourseID** dla **właściwości klucza**.</span><span class="sxs-lookup"><span data-stu-id="45618-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="45618-170">Kliknij prawym przyciskiem myszy jednostkę **GradeReport** i wybierz pozycję **dodaj nową**-&gt; **Właściwość skalarna**.</span><span class="sxs-lookup"><span data-stu-id="45618-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="45618-171">Zmień domyślną nazwę właściwości na **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="45618-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="45618-172">Dodaj kolejną Właściwość skalarną i podaj **LastName** dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="45618-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="45618-173">Dodaj kolejną Właściwość skalarną i określ **klasy** dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="45618-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="45618-174">W oknie **Właściwości** Zmień właściwość **Typ** **klasy**na **Decimal**.</span><span class="sxs-lookup"><span data-stu-id="45618-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="45618-175">Wybierz właściwości **FirstName** i **LastName** .</span><span class="sxs-lookup"><span data-stu-id="45618-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="45618-176">W oknie **Właściwości** Zmień wartość właściwości **EntityKey** na **true**.</span><span class="sxs-lookup"><span data-stu-id="45618-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="45618-177">W związku z tym następujące elementy zostały dodane do sekcji **CSDL** pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="45618-178">Mapowanie zapytania definiującego na typ jednostki</span><span class="sxs-lookup"><span data-stu-id="45618-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="45618-179">W tym kroku będziemy używać okna Szczegóły mapowania do mapowania typów jednostek koncepcyjnych i magazynowych.</span><span class="sxs-lookup"><span data-stu-id="45618-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="45618-180">Kliknij prawym przyciskiem myszy jednostkę **GradeReport** na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.</span><span class="sxs-lookup"><span data-stu-id="45618-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="45618-181">Zostanie wyświetlone okno **szczegóły mapowania** .</span><span class="sxs-lookup"><span data-stu-id="45618-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="45618-182">Wybierz pozycję **GradeReport** z **&lt;Dodaj tabelę lub widok&gt;** listy rozwijanej (znajdującej się w **tabeli**s).</span><span class="sxs-lookup"><span data-stu-id="45618-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="45618-183">Zostaną wyświetlone domyślne mapowania między typem jednostki **GradeReport** koncepcyjnej i magazynu.</span><span class="sxs-lookup"><span data-stu-id="45618-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="45618-184">![mapowanie Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="45618-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="45618-185">W związku z tym element **elementu EntitySetMapping** jest dodawany do sekcji mapowania pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="45618-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="45618-186">Skompiluj aplikację.</span><span class="sxs-lookup"><span data-stu-id="45618-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="45618-187">Wywoływanie definiowania zapytania w kodzie</span><span class="sxs-lookup"><span data-stu-id="45618-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="45618-188">Można teraz wykonać Definiowanie zapytania przy użyciu typu jednostki **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="45618-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
