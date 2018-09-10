---
title: Definiowanie zapytania — projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 8415a265cdbe078422e0467ee97da955a81b873d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250975"
---
# <a name="defining-query---ef-designer"></a>Definiowanie zapytania — projektancie platformy EF
W tym instruktażu przedstawiono sposób dodawania, definiując kwerendy i odpowiednia jednostka typu do modelu, używając projektancie platformy EF. Definiowanie zapytania jest najczęściej używany do zapewnia funkcje podobne do dostarczony przez widok bazy danych, ale widok jest zdefiniowany w modelu, a nie bazy danych. Definiowanie zapytań pozwala wykonać instrukcję SQL, który jest określony w **DefiningQuery** element z pliku edmx. Aby uzyskać więcej informacji, zobacz **DefiningQuery** w [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Korzystając z Definiowanie zapytań, należy zdefiniować typ jednostki w modelu. Typ jednostki jest używany do powierzchni dane udostępniane przez definiowanie zapytań. Należy pamiętać, że udostępniane za pośrednictwem tego typu jednostki danych tylko do odczytu.

Nie można wykonać zapytań sparametryzowanych jako Definiowanie zapytań. Jednak dane można zaktualizować przez mapowanie insert, update i delete funkcji typu jednostki który uwypukli najistotniejsze dane do procedur składowanych. Aby uzyskać więcej informacji, zobacz [wstawiania, aktualizowania i usuwania przy użyciu procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md).

W tym temacie przedstawiono sposób wykonywania następujących zadań.

-   Dodaj zapytanie definiujące
-   Dodaj typ jednostki do modelu
-   Definiowanie zapytania do typu jednostki mapy

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Ten przewodnik korzysta z programu Visual Studio 2012 lub nowszego.

-   Otwórz program Visual Studio.
-   Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **aplikację Konsolową** szablonu.
-   Wprowadź **DefiningQuerySample** jako nazwę projektu i kliknij przycisk **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Tworzenie modelu, w oparciu o bazę danych School

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **DefiningQueryModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij przycisk nowe połączenie. W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W oknie dialogowym Wybierz obiekty bazy danych sprawdź **tabel** węzła. Spowoduje to dodanie wszystkich tabel do **School** modelu.
-   Kliknij przycisk **Zakończ**.
-   W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **DefiningQueryModel.edmx** plik i wybierz **Otwórz za pomocą...** .
-   Wybierz **Edytor (tekstu) XML**.

    ![Edytor XML](~/ef6/media/xmleditor.png)

-   Kliknij przycisk **tak** Jeśli zostanie wyświetlony monit z następującym komunikatem:

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Dodaj zapytanie definiujące

W tym kroku, które zostaną użyte zapytanie edytora XML, aby dodać definiujące, a następnie wpisz jednostki SSDL części pliku edmx. 

-   Dodaj **EntitySet** elementu SSDL części pliku edmx (wiersz 5 do 13). Określ następujące ustawienia:
    -   Tylko **nazwa** i **EntityType** atrybuty **EntitySet** są określony element.
    -   W pełni kwalifikowana nazwa typu jednostki jest używany w **EntityType** atrybutu.
    -   Instrukcja SQL do wykonania jest określona w **DefiningQuery** elementu.

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

-   Dodaj **EntityType** element do sekcji SSDL edmx. Plik jak pokazano poniżej. Należy pamiętać o następujących kwestiach:
    -   Wartość **nazwa** atrybut odnosi się do wartości **EntityType** atrybutu w **EntitySet** element powyżej, mimo że w pełni kwalifikowana nazwa Typ jednostki jest używany w **EntityType** atrybutu.
    -   Nazwy właściwości odpowiadają nazwom kolumny zwróconą przez instrukcję SQL w **DefiningQuery** elementu (powyżej).
    -   W tym przykładzie klucz jednostki składa się z trzech właściwości w celu zapewnienia unikalną wartość kluczową.

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
> Jeśli później uruchomić **Kreator modelu aktualizacji** okno dialogowe, wszelkie zmiany wprowadzone do modelu magazynu, w tym Definiowanie zapytań, zostaną zastąpione.

 

## <a name="add-an-entity-type-to-the-model"></a>Dodaj typ jednostki do modelu

W tym kroku zostanie dodany typ jednostki do modelu koncepcyjnego, za pomocą projektanta EF.  Należy pamiętać o następujących kwestiach:

-   **Nazwa** jednostki odnosi się do wartości **EntityType** atrybutu w **EntitySet** powyżej elementu.
-   Nazwy właściwości odpowiadają nazwom kolumny zwróconą przez instrukcję SQL w **DefiningQuery** powyżej elementu.
-   W tym przykładzie klucz jednostki składa się z trzech właściwości w celu zapewnienia unikalną wartość kluczową.

W Projektancie platformy EF, należy otworzyć modelu.

-   Kliknij dwukrotnie DefiningQueryModel.edmx.
-   Powiedz **tak** się następujący komunikat:

    ![Ostrzeżenie 2](~/ef6/media/warning2.png)

 

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.

-   Kliknij prawym przyciskiem myszy Projektanta powierzchni i wybierz **Dodaj nowe**-&gt;**jednostki...** .
-   Określ **GradeReport** dla nazwy jednostki i **CourseID** dla **właściwości klucza**.
-   Kliknij prawym przyciskiem myszy **GradeReport** jednostki, a następnie wybierz pozycję **Dodaj nowe** - &gt; **właściwość skalarną**.
-   Zmienić domyślną nazwę właściwości do **FirstName**.
-   Dodaj inną właściwość skalarną i określ **LastName** dla nazwy.
-   Dodaj inną właściwość skalarną i określ **klasy korporacyjnej** dla nazwy.
-   W **właściwości** oknie zmiany **klasy korporacyjnej**firmy **typu** właściwości **dziesiętna**.
-   Wybierz **FirstName** i **LastName** właściwości.
-   W **właściwości** oknie zmiany **EntityKey** wartość właściwości **True**.

W rezultacie, następujące elementy zostały dodane do **CSDL** części pliku edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Definiowanie zapytania do typu jednostki mapy

W tym kroku użyjemy, okno Szczegóły mapowania, aby zamapować koncepcyjnej i typy jednostek magazynu.

-   Kliknij prawym przyciskiem myszy **GradeReport** jednostki na projekt powierzchni i wybierz **mapowania tabeli,**.  
    **Szczegóły mapowania** zostanie wyświetlone okno.
-   Wybierz **GradeReport** z **&lt;Dodaj tabelę lub widok&gt;** listy rozwijanej (znajdującej się w **tabeli**s).  
    Domyślne mapowania między koncepcyjnej i magazynu **GradeReport** typu jednostki są wyświetlane.  
    ![Mapowanie Details3](~/ef6/media/mappingdetails.png)

W rezultacie **obiekcie EntitySetMapping** element zostanie dodany do sekcji mapowania pliku edmx. 

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

-   Kompilowanie aplikacji.

 

## <a name="call-the-defining-query-in-your-code"></a>Wywołaj Definiowanie zapytania w kodzie

Definiowanie zapytania można teraz wykonać przy użyciu **GradeReport** typu jednostki. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
