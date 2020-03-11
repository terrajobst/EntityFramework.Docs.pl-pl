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
# <a name="defining-query---ef-designer"></a>Definiowanie zapytania-Dr Designer
W tym instruktażu pokazano, jak dodać zapytanie definiujące i odpowiedni typ jednostki do modelu przy użyciu narzędzia Dr Designer. Definiowanie zapytania jest często używane w celu zapewnienia funkcjonalności podobnej do tej, która jest udostępniana przez widok bazy danych, ale widok jest zdefiniowany w modelu, a nie w bazie danych. Definiowanie zapytania pozwala wykonać instrukcję SQL, która jest określona w **DefiningQuery** elementu pliku. edmx. Aby uzyskać więcej informacji, zobacz **DefiningQuery** w [specyfikacji SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Podczas korzystania z definiowania zapytań należy również zdefiniować typ jednostki w modelu. Typ jednostki jest używany do prezentowania danych przez definiowanie zapytania. Należy pamiętać, że dane, które są udostępniane przez ten typ jednostki, są tylko do odczytu.

Zapytania sparametryzowane nie mogą być wykonywane jako definiujące zapytania. Dane można jednak zaktualizować, mapując funkcje INSERT, Update i DELETE klasy Entity, która wyświetla dane w procedurach składowanych. Aby uzyskać więcej informacji, zobacz [Wstawianie, aktualizowanie i usuwanie za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md).

W tym temacie przedstawiono sposób wykonywania następujących zadań.

-   Dodawanie zapytania definiującego
-   Dodawanie typu jednostki do modelu
-   Mapowanie zapytania definiującego na typ jednostki

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tym instruktażu jest używany program Visual Studio 2012 lub nowszy.

-   Otwórz program Visual Studio.
-   W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.
-   W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **Aplikacja konsolowa** .
-   Wprowadź **DefiningQuerySample** jako nazwę projektu, a następnie kliknij przycisk **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Tworzenie modelu opartego na bazie danych szkoły

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **DefiningQueryModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij pozycję nowe połączenie. W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
-   W oknie dialogowym Wybierz obiekty bazy danych sprawdź, czy **tabele** węzła. Spowoduje to dodanie wszystkich tabel do modelu **szkoły** .
-   Kliknij przycisk **Zakończ**.
-   W Eksplorator rozwiązań kliknij prawym przyciskiem myszy plik **DefiningQueryModel. edmx** i wybierz polecenie **Otwórz za pomocą...** .
-   Wybierz **Edytor XML (tekst)** .

    ![Edytor XML](~/ef6/media/xmleditor.png)

-   Kliknij przycisk **tak** , jeśli zostanie wyświetlony monit z następującym komunikatem:

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Dodawanie zapytania definiującego

W tym kroku użyjesz edytora XML, aby dodać zapytanie definiujące i typ jednostki do sekcji SSDL pliku edmx. 

-   Dodaj element **EntitySet** do sekcji SSDL w pliku edmx (wiersz 5 – 13). Określ następujące ustawienia:
    -   Określono tylko **atrybuty ** i **EntityType** elementu **EntitySet** .
    -   W pełni kwalifikowana nazwa typu jednostki jest używana w atrybucie  **EntityType** .
    -   Instrukcja SQL do wykonania jest określona w elemencie **DefiningQuery** .

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

-   Dodaj element **EntityType** do sekcji SSDL w obiekcie. edmx. plik, jak pokazano poniżej. Należy pamiętać o następujących kwestiach:
    -   Wartość atrybutu **name** odnosi się do wartości atrybutu **EntityType** w elemencie **EntitySet** powyżej, chociaż w atrybucie **EntityType** jest używana w pełni kwalifikowana nazwa typu jednostki.
    -   Nazwy właściwości odpowiadają nazwom kolumn zwracanym przez instrukcję SQL w elemencie **DefiningQuery** (powyżej).
    -   W tym przykładzie klucz jednostki składa się z trzech właściwości, aby zapewnić unikatową wartość klucza.

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
> Jeśli później zostanie uruchomione okno dialogowe **Kreatora aktualizacji** , wszelkie zmiany wprowadzone do modelu magazynu, w tym Definiowanie zapytań, zostaną nadpisywane.

 

## <a name="add-an-entity-type-to-the-model"></a>Dodawanie typu jednostki do modelu

W tym kroku dodamy typ jednostki do modelu koncepcyjnego za pomocą narzędzia Dr Designer.  Zwróć uwagę na następujące kwestie:

-   **Nazwa** jednostki odpowiada wartości atrybutu **EntityType** w powyższym elemencie **EntitySet** .
-   Nazwy właściwości odpowiadają nazwom kolumn zwracanym przez instrukcję SQL w powyższym elemencie **DefiningQuery** .
-   W tym przykładzie klucz jednostki składa się z trzech właściwości, aby zapewnić unikatową wartość klucza.

Otwórz model w programie Dr Designer.

-   Kliknij dwukrotnie plik DefiningQueryModel. edmx.
-   Załóżmy **,** że zostanie wyświetlony następujący komunikat:

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.

-   Kliknij prawym przyciskiem myszy powierzchnię projektanta i wybierz polecenie **Dodaj nową**-&gt;**jednostki.** .
-   Określ **GradeReport** dla nazwy jednostki i **CourseID** dla **właściwości klucza**.
-   Kliknij prawym przyciskiem myszy jednostkę **GradeReport** i wybierz pozycję **dodaj nową**-&gt; **Właściwość skalarna**.
-   Zmień domyślną nazwę właściwości na **FirstName**.
-   Dodaj kolejną Właściwość skalarną i podaj **LastName** dla nazwy.
-   Dodaj kolejną Właściwość skalarną i określ **klasy** dla nazwy.
-   W oknie **Właściwości** Zmień właściwość **Typ** **klasy**na **Decimal**.
-   Wybierz właściwości **FirstName** i **LastName** .
-   W oknie **Właściwości** Zmień wartość właściwości **EntityKey** na **true**.

W związku z tym następujące elementy zostały dodane do sekcji **CSDL** pliku. edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Mapowanie zapytania definiującego na typ jednostki

W tym kroku będziemy używać okna Szczegóły mapowania do mapowania typów jednostek koncepcyjnych i magazynowych.

-   Kliknij prawym przyciskiem myszy jednostkę **GradeReport** na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.  
    Zostanie wyświetlone okno **szczegóły mapowania** .
-   Wybierz pozycję **GradeReport** z **&lt;Dodaj tabelę lub widok&gt;** listy rozwijanej (znajdującej się w **tabeli**s).  
    Zostaną wyświetlone domyślne mapowania między typem jednostki **GradeReport** koncepcyjnej i magazynu.  
    ![mapowanie Details3](~/ef6/media/mappingdetails.png)

W związku z tym element **elementu EntitySetMapping** jest dodawany do sekcji mapowania pliku edmx. 

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

-   Skompiluj aplikację.

 

## <a name="call-the-defining-query-in-your-code"></a>Wywoływanie definiowania zapytania w kodzie

Można teraz wykonać Definiowanie zapytania przy użyciu typu jednostki **GradeReport** . 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
