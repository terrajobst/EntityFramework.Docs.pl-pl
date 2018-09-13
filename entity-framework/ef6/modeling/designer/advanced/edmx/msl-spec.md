---
title: Specyfikacja MSL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 9519155422d8542d4a14bc1c612e91ebc22bf15e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490561"
---
# <a name="msl-specification"></a>Specyfikacja MSL
Mapowanie specyfikacji języka (MSL) to język oparty na formacie XML, który opisuje mapowanie między modelu koncepcyjnym i modelu magazynu aplikacji platformy Entity Framework.

W aplikacji Entity Framework mapowania metadanych jest ładowany z pliku MSL albo identyfikatorem (zapisany w pliku MSL) w czasie kompilacji. Entity Framework używa mapowania metadanych w czasie wykonywania do przekształcania zapytania względem modelu koncepcyjnego polecenia specyficzne dla magazynu.

Entity Framework Designer (Projektant EF) przechowuje informacje dotyczące mapowania w pliku edmx w czasie projektowania. W czasie kompilacji Projektant jednostki używa informacji w pliku edmx można utworzyć pliku MSL albo identyfikatorem, które są wymagane przez Entity Framework w czasie wykonywania

Nazwy wszystkich koncepcje lub typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw. Aby uzyskać informacji na temat modelu koncepcyjnego nazwa przestrzeni nazw, zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Aby dowiedzieć się, nazwa przestrzeni nazw w modelu magazynu, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Wersje MSL są zróżnicowane według przestrzeni nazw XML.

| Wersja pliku MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | Nazwa urn: schemas-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>W elemencie aliasu (MSL)

**Alias** element w mapowaniu language specification (MSL) jest elementem podrzędnym elementu mapowania, który jest używany do definiowania aliasy koncepcyjny model i magazynu modeli w przypadku przestrzeni nazw. Nazwy wszystkich koncepcje lub typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw. Dla informacji o modelu koncepcyjnego nazwa przestrzeni nazw Zobacz Element schematu (CSDL). Informacje o nazwie przestrzeni nazw w modelu magazynu na ten temat można znaleźć w elemencie schematu (SSDL).

**Alias** elementu nie może mieć elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **Alias** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Tak         | Alias przestrzeni nazw, który jest określony przez **wartość** atrybutu. |
| **Wartość**      | Tak         | Przestrzeń nazw, dla których wartość **klucz** element jest aliasem.     |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **Alias** element, który definiuje alias, `c`, dla których typy zostały zdefiniowane w modelu koncepcyjnym.

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a>Punkt końcowy skojarzenia elementu (MSL)

**Punkt końcowy skojarzenia** element w mapowaniu language specification (MSL) jest używany, gdy funkcji modyfikacji typu jednostki w modelu koncepcyjnym są mapowane do procedur składowanych w bazie danych. Jeśli modyfikacja procedury składowanej przyjmuje parametr, którego wartość jest przechowywana w właściwość skojarzenia **punkt końcowy skojarzenia** element mapuje wartości właściwości do parametru. Aby uzyskać więcej informacji zobacz przykład poniżej.

Aby uzyskać więcej informacji na temat mapowania funkcji modyfikacji typów jednostek do procedur składowanych, zobacz Element ModificationFunctionMapping (MSL) i wskazówki: mapowanie jednostki do procedur składowanych.

**Punkt końcowy skojarzenia** element może mieć następujące elementy podrzędne:

-   ScalarProperty

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **punkt końcowy skojarzenia** elementu.

| Nazwa atrybutu     | Jest wymagany | Wartość                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Obiekt AssociationSet** | Tak         | Nazwa skojarzenia, który jest mapowany.                                                                                                                                 |
| **From**           | Tak         | Wartość **FromRole** atrybut właściwość nawigacji, która odnosi się do skojarzenia mapowany. Aby uzyskać więcej informacji zobacz element NavigationProperty — Element (CSDL). |
| **To**             | Tak         | Wartość **ToRole** atrybut właściwość nawigacji, która odnosi się do skojarzenia mapowany. Aby uzyskać więcej informacji zobacz element NavigationProperty — Element (CSDL).   |

### <a name="example"></a>Przykład

Należy wziąć pod uwagę następujące typie modelu koncepcyjnego entity:

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

Należy również rozważyć następującą procedurę składowaną:

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

Aby zamapować funkcję aktualizacji `Course` jednostki do tej procedury składowanej, należy podać wartość **DepartmentID** parametru. Wartość `DepartmentID` nie odpowiada na właściwość typu jednostki; znajduje się w skojarzeniu niezależnie od którego mapowanie jest następująca:

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

Poniższy kod przedstawia **punkt końcowy skojarzenia** element używany do mapowania **DepartmentID** właściwość **klucza Obcego\_kurs\_działu** skojarzenie, aby **UpdateCourse** procedury składowanej (do którego funkcja aktualizacji **kurs** typ jednostki jest mapowany):

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a>Element AssociationSetMapping (MSL)

**AssociationSetMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między skojarzenia koncepcyjny kolumn w modelu i tabeli w bazie danych.

Skojarzeń w modelu koncepcyjnym są typy, których właściwości reprezentują podstawowe i obce kolumny klucza w bazie danych. **AssociationSetMapping** element używa dwóch elementów właściwości EndProperty, aby zdefiniować mapowania między właściwościami typu skojarzenia i kolumn w bazie danych. Warunki można umieścić na te mapowania z elementem warunku. Mapowanie insert, update i delete funkcji w celu skojarzenia do procedur składowanych w bazie danych za pomocą elementu ModificationFunctionMapping. Za pomocą ciągu jednostki SQL w elemencie QueryView, należy zdefiniować tylko do odczytu mapowania między skojarzenia i kolumn w tabeli.

> [!NOTE]
> Jeśli nie zdefiniowano ograniczenie referencyjne dla skojarzenia w modelu koncepcyjnym, skojarzenie nie muszą być mapowane z **AssociationSetMapping** elementu. Jeśli **AssociationSetMapping** element jest obecny dla skojarzenia, która ma ograniczenie referencyjne, mapowania zdefiniowane w **AssociationSetMapping** element zostaną zignorowane. Aby uzyskać więcej informacji zobacz Element ReferentialConstraint (CSDL).

**AssociationSetMapping** element może mieć następujące elementy podrzędne

-   Obiekt QueryView (zero lub jeden)
-   Właściwości EndProperty (zero lub dwóch)
-   Warunek (zero lub więcej)
-   ModificationFunctionMapping (zero lub jeden)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSetMapping** elementu.

| Nazwa atrybutu     | Jest wymagany | Wartość                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Nazwa**           | Tak         | Nazwa zestawu skojarzeń modelu koncepcyjnego, który jest mapowany.                      |
| **Nazwa typu**       | Nie          | Przestrzeń nazw kwalifikowana nazwa typu skojarzenia modelu koncepcyjnego, który jest mapowany. |
| **StoreEntitySet** | Nie          | Nazwa tabeli, która jest mapowany.                                                 |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **AssociationSetMapping** elementu, w którym **klucza Obcego\_kurs\_działu** skojarzeń w modelu koncepcyjnego jest mapowany na  **Kurs** tabeli w bazie danych. Mapowania między właściwościami typu skojarzenia i kolumn w tabeli są określone w podrzędnej **właściwości EndProperty** elementów.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
``` 

## <a name="complexproperty-element-msl"></a>Element ComplexProperty (MSL)

A **ComplexProperty** element w mapowaniu language specification (MSL) definiuje mapowanie między właściwość typu złożonego w modelu koncepcyjnego entity kolumny Typ i tabeli w bazie danych. Mapowania kolumn właściwości są określone w elementy ScalarProperty podrzędne.

**ComplexType** property — element może mieć następujące elementy podrzędne:

-   ScalarProperty (zero lub więcej)
-   **ComplexProperty** (zero lub więcej)
-   ComplextTypeMapping (zero lub więcej)
-   Warunek (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ComplexProperty** elementu:

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości złożonej typu jednostki w modelu koncepcyjnym, który jest mapowany. |
| **Nazwa typu**   | Nie          | Przestrzeń nazw kwalifikowana nazwa typu właściwości modelu koncepcyjnego.                              |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły. Następujący typ złożony został dodany do modelu koncepcyjnego:

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

**LastName** i **FirstName** właściwości **osoby** typu jednostki zostały zamienione na jedną właściwość złożona, **nazwa**:

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

Pokazano w poniższym pliku MSL **ComplexProperty** element używany do mapowania **nazwa** właściwości do kolumn w bazie danych:

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a>Element ComplexTypeMapping (MSL)

**ComplexTypeMapping** elementu w mapowaniu language specification (MSL) jest elementem podrzędnym elementu ResultMapping i definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnym i procedury składowanej w źródłowym Baza danych, gdy spełnione są następujące:

-   Importowanie funkcji zwraca koncepcyjny typu złożonego.
-   Nazwy kolumn zwrócone przez procedurę składowaną nie odpowiadają dokładnie nazwy właściwości na typ złożony.

Domyślnie mapowania między kolumnami zwrócone przez procedurę składowaną i typ złożony opiera się na nazwy kolumn i właściwości. Nazwy kolumn nie odpowiadają dokładnie nazw właściwości, należy użyć **ComplexTypeMapping** elementu, aby zdefiniować mapowanie. Aby uzyskać przykład domyślnego mapowania Zobacz Element FunctionImportMapping (MSL).

**ComplexTypeMapping** element może mieć następujące elementy podrzędne:

-   ScalarProperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ComplexTypeMapping** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Nazwa typu**   | Tak         | Nazwa kwalifikowana przez obszar nazw typ złożony, który jest mapowany. |

### <a name="example"></a>Przykład

Należy wziąć pod uwagę następujące procedury składowanej:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Należy również rozważyć następujący typ złożony modelu koncepcyjnego:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Aby można było utworzyć importu funkcji, który zwraca wystąpień poprzedniego typu złożonego, mapowanie między kolumnami zwrócone przez procedurę składowaną i typ jednostki musi być zdefiniowany w **ComplexTypeMapping** elementu:

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a>Warunek, Element (MSL)

**Warunek** elementu w mapowaniu language specification (MSL) powoduje warunków mapowania między modelu koncepcyjnego i podstawowej bazy danych. Mapowania, która jest zdefiniowana w węźle XML jest nieprawidłowy, jeśli wszystkie warunki, jako element podrzędny określonego w **warunek** elementów, są spełnione. W przeciwnym razie mapowanie jest nieprawidłowy. Na przykład, jeśli MappingFragment element zawiera jeden lub więcej **warunek** elementy podrzędne, mapowanie zdefiniowane w ramach **MappingFragment** węzeł będzie obowiązywał tylko jeśli wszystkie warunki podrzędnych  **Warunek** elementy są spełnione.

Każdy warunek można zastosować do jednej **nazwa** (nazwa właściwości jednostki modelu koncepcyjnego, określony przez **nazwa** atrybut), lub **ColumnName** (nazwa kolumny w Określona baza danych, przez **ColumnName** atrybutu). Gdy **nazwa** ma ustawioną wartość atrybutu, warunek jest sprawdzany na podstawie wartości właściwości jednostki. Gdy **ColumnName** ma ustawioną wartość atrybutu, warunek jest sprawdzana względem wartości kolumny. Tylko jeden z **nazwa** lub **ColumnName** atrybutu można określić w **warunek** elementu.

> [!NOTE]
> Gdy **warunek** element jest używany w elemencie FunctionImportMapping tylko **nazwa** atrybut nie ma zastosowania.

**Warunek** element może być elementem podrzędnym następujące elementy:

-   AssociationSetMapping
-   ComplexProperty
-   Obiekcie EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

**Warunek** element może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **warunek** elementu:

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NazwaKolumny** | Nie          | Nazwa kolumny tabeli, którego wartość jest używana do warunku.                                                                                                                                                                                                                   |
| **IsNull**     | Nie          | **Wartość true,** lub **False**. Jeśli wartość jest **True** , a wartość kolumny jest **o wartości null**, lub jeśli wartość jest **False** i wartość kolumny jest **null**, warunek jest prawdziwy . W przeciwnym razie warunek ma wartość false. <br/> **IsNull** i **wartość** atrybutów nie można użyć w tym samym czasie. |
| **Wartość**      | Nie          | Wartość, z którym wartość kolumny jest porównywany. Jeśli wartości są takie same, warunek jest prawdziwy. W przeciwnym razie warunek ma wartość false. <br/> **IsNull** i **wartość** atrybutów nie można użyć w tym samym czasie.                                                                       |
| **Nazwa**       | Nie          | Nazwa właściwości jednostki modelu koncepcyjnego, którego wartość jest używana do warunku. <br/> Ten atrybut nie ma zastosowania Jeśli **warunek** element jest używany w elemencie FunctionImportMapping.                                                                           |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **warunek** elementy jako elementy podrzędne **MappingFragment** elementów. Gdy **DataZatrudnienia** nie ma wartości null i **EnrollmentDate** jest wartość null, dane zamapowane między **SchoolModel.Instructor** typu i **PersonID**i **DataZatrudnienia** kolumn **osoby** tabeli. Gdy **EnrollmentDate** nie ma wartości null i **DataZatrudnienia** jest wartość null, dane zamapowane między **SchoolModel.Student** typu i **PersonID** i **rejestracji** kolumn **osoby** tabeli.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a>Element DeleteFunction (MSL)

**DeleteFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji usuwania w typie jednostki lub skojarzeń w modelu koncepcyjnym procedurę składowaną w bazie danych. Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu. Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).

> [!NOTE]
> Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.

### <a name="deletefunction-applied-to-entitytypemapping"></a>Stosowane do EntityTypeMapping DeleteFunction

W przypadku zastosowania do elementu EntityTypeMapping **DeleteFunction** element mapy funkcji usuwania typu jednostki w modelu koncepcyjnym do procedury składowanej.

**DeleteFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **EntityTypeMapping** elementu:

-   Punkt końcowy skojarzenia (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   ScarlarProperty (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **DeleteFunction** element, jeśli jest stosowany do **EntityTypeMapping** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **functionName**          | Tak         | Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji usuwania. Procedura składowana musi być zadeklarowany w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **DeleteFunction** element mapowania funkcji usuwania **osoby** typu jednostki **DeletePerson** Procedura składowana. **DeletePerson** procedury składowanej jest zadeklarowana w modelu magazynu.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a>Stosowane do AssociationSetMapping DeleteFunction

W przypadku zastosowania do elementu AssociationSetMapping **DeleteFunction** element mapy funkcji usuwania skojarzenia w modelu koncepcyjnym do procedury składowanej.

**DeleteFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **AssociationSetMapping** elementu:

-   Właściwości EndProperty

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **DeleteFunction** element, jeśli jest stosowany do **AssociationSetMapping** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **functionName**          | Tak         | Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji usuwania. Procedura składowana musi być zadeklarowany w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **DeleteFunction** element używany do mapowania funkcji usuwania **CourseInstructor** skojarzenia  **DeleteCourseInstructor** procedury składowanej. **DeleteCourseInstructor** procedury składowanej jest zadeklarowana w modelu magazynu.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a>Element właściwości EndProperty (MSL)

**Właściwości EndProperty** element w mapowaniu language specification (MSL) definiuje mapowanie między punktu końcowego lub funkcją Modyfikowanie skojarzenia typu modelu koncepcyjnego i podstawowej bazy danych. Mapowanie kolumny właściwości jest określone w elemencie ScalarProperty podrzędnym.

Gdy **właściwości EndProperty** element jest używany do definiowania mapowania dla elementu end skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu AssociationSetMapping. Gdy **właściwości EndProperty** element jest używany do definiowania mapowania dla funkcji modyfikacji skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu InsertFunction lub DeleteFunction elementu.

**Właściwości EndProperty** element może mieć następujące elementy podrzędne:

-   ScalarProperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **właściwości EndProperty** elementu:

| Nazwa atrybutu | Jest wymagany | Wartość                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Nazwa           | Tak         | Nazwa elementu end skojarzenia, który jest mapowany. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **AssociationSetMapping** elementu, w którym **klucza Obcego\_kurs\_działu** skojarzeń w modelu koncepcyjnym jest mapowany na **Kurs** tabeli w bazie danych. Mapowania między właściwościami typu skojarzenia i kolumn w tabeli są określone w podrzędnej **właściwości EndProperty** elementów.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **właściwości EndProperty** mapowania funkcji wstawiania i usuwania skojarzenia elementu (**CourseInstructor**) do procedur składowanych w bazie danych. Funkcje, które są mapowane na są deklarowane w modelu magazynu.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a>Element elemencie EntityContainerMapping (MSL)

**Elemencie EntityContainerMapping** elementu w specyfikacji języka (MSL) mapowania mapuje kontener jednostek w modelu koncepcyjnym kontener jednostek w modelu magazynu. **Elemencie EntityContainerMapping** element jest elementem podrzędnym elementu mapowania.

**Elemencie EntityContainerMapping** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Obiekcie EntitySetMapping (zero lub więcej)
-   AssociationSetMapping (zero lub więcej)
-   FunctionImportMapping (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **elemencie EntityContainerMapping** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Tak         | Nazwa kontenera magazynu jednostki modelu, który jest mapowany.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Tak         | Nazwa kontenera jednostek modelu koncepcyjnego, który jest mapowany.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Nie          | **Wartość true,** lub **False**. Jeśli **False**, są generowane nie widoków aktualizacji. Ten atrybut powinien być ustawiony na **False** przypadku mapowania tylko do odczytu, która będzie nieprawidłowa, ponieważ dane mogą być nie obustronne pomyślnie. <br/> Wartość domyślna to **True**. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **elemencie EntityContainerMapping** element, który mapuje **SchoolModelEntities** container (kontener modelu koncepcyjnego entity) do  **SchoolModelStoreContainer** container (kontener jednostek modelu magazynu):

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a>Element obiekcie EntitySetMapping (MSL)

**Obiekcie EntitySetMapping** elementu w specyfikacji języka (MSL) mapy wszystkie typy w jednostce modelu koncepcyjnego równa jednostki mapowania zestawów w modelu magazynu. Zestawu jednostek w modelu koncepcyjnego to kontener logiczny dla wystąpień jednostek z tego samego typu (i jego typów pochodnych). Zestawu jednostek w modelu magazynu reprezentuje tabelę lub widok w bazie danych. Zestaw jednostek modelu koncepcyjnego jest określony przez wartość **nazwa** atrybutu **obiekcie EntitySetMapping** elementu. Mapowany do tabeli lub widoku jest określona przez **StoreEntitySet** atrybutu w każdym elemencie MappingFragment podrzędnej lub w **obiekcie EntitySetMapping** samego elementu.

**Obiekcie EntitySetMapping** element może mieć następujące elementy podrzędne:

-   EntityTypeMapping (zero lub więcej)
-   Obiekt QueryView (zero lub jeden)
-   MappingFragment (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **obiekcie EntitySetMapping** elementu.

| Nazwa atrybutu           | Jest wymagany | Wartość                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                 | Tak         | Nazwa zestawu jednostek modelu koncepcyjnego, który jest mapowany.                                                                                                                                                             |
| **Element TypeName** **1**       | Nie          | Nazwa typu jednostki modelu koncepcyjnego, który jest mapowany.                                                                                                                                                            |
| **StoreEntitySet** **1** | Nie          | Nazwa zestawu jednostek modelu magazynu, który jest mapowany na.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Nie          | **Wartość true,** lub **False** w zależności od tego, czy tylko unikatowe wiersze są zwracane. <br/> Jeśli ten atrybut jest ustawiony na **True**, **GenerateUpdateViews** atrybut elementu w elemencie EntityContainerMapping musi być równa **False**. |

 

**1** **TypeName** i **StoreEntitySet** atrybuty można zamiast elementów podrzędnych EntityTypeMapping i MappingFragment mapowania typu pojedynczy element na pojedynczą tabelę.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **obiekcie EntitySetMapping** element, który mapuje trzy typy (typ podstawowy i dwa typy pochodne) w **kursów** zestaw jednostek model koncepcyjny do trzech różnych tabel w podstawowej bazy danych. Tabele są określone przez **StoreEntitySet** atrybutu w każdym **MappingFragment** elementu.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a>Element EntityTypeMapping (MSL)

**EntityTypeMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między typem jednostki w modelu koncepcyjnym i tabel lub widoków w bazie danych. Dla informacji na temat typów jednostek modelu koncepcyjnego i podstawowej bazy danych tabel lub widoków Zobacz Element EntityType (CSDL) i elementu obiektu EntitySet (SSDL). Typ jednostki modelu koncepcyjnego, który jest mapowany jest określony przez **TypeName** atrybutu **EntityTypeMapping** elementu. Tabela lub widok, który jest mapowany jest określona przez **StoreEntitySet** atrybutu element MappingFragment podrzędny.

ModificationFunctionMapping, element podrzędny może być używane do mapowania Wstawianie, aktualizowanie lub usuwanie funkcji typów jednostek do procedur składowanych w bazie danych.

**EntityTypeMapping** element może mieć następujące elementy podrzędne:

-   MappingFragment (zero lub więcej)
-   ModificationFunctionMapping (zero lub jeden)
-   ScalarProperty
-   Warunek

> [!NOTE]
> **MappingFragment** i **ModificationFunctionMapping** elementy nie mogą być elementami podrzędnymi **EntityTypeMapping** element w tym samym czasie.


> [!NOTE]
> **ScalarProperty** i **warunek** elementów może zawierać tylko elementy podrzędne **EntityTypeMapping** elementu, gdy jest używany w elemencie FunctionImportMapping.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityTypeMapping** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa typu**   | Tak         | Przestrzeń nazw kwalifikowana nazwa typu jednostki modelu koncepcyjnego, który jest mapowany. <br/> Jeśli typ jest abstrakcyjny ani typem pochodnym, wartość musi być `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element obiekcie EntitySetMapping z dwóch podrzędnych **EntityTypeMapping** elementów. W pierwszym **EntityTypeMapping** elementu **SchoolModel.Person** typ jednostki jest mapowany na **osoby** tabeli. W drugim **EntityTypeMapping** element, funkcja aktualizacji z **SchoolModel.Person** typu jest mapowany do procedury składowanej **UpdatePerson**, w bazie danych .

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Przykład

W kolejnym przykładzie pokazano mapowanie hierarchii typów, w którym główny typ jest abstrakcyjny. Zwróć uwagę na użycie `IsOfType` składnia **TypeName** atrybutów.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a>Element FunctionImportMapping (MSL)

**FunctionImportMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnego i procedury składowanej lub funkcji w bazie danych. Importy funkcja musi być zadeklarowana w modelu koncepcyjnym i procedur składowanych musi być zadeklarowana w modelu magazynu. Aby uzyskać więcej informacji zobacz Element FunctionImport (CSDL) i funkcji — Element (SSDL).

> [!NOTE]
> Domyślnie jeśli import funkcja zwraca modelu koncepcyjnego typu jednostki lub typ złożony, następnie nazwy kolumn zwrócone przez procedurę składowaną podstawowej musi dokładnie odpowiadać nazwy właściwości w typie modelu koncepcyjnego. Jeśli nazwy kolumn nie odpowiadają dokładnie nazw właściwości, mapowanie musi być zdefiniowany w elemencie ResultMapping.

**FunctionImportMapping** element może mieć następujące elementy podrzędne:

-   ResultMapping (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **FunctionImportMapping** elementu:

| Nazwa atrybutu         | Jest wymagany | Wartość                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Tak         | Nazwa funkcji importowania w modelu koncepcyjnym, który jest mapowany.           |
| **functionName**       | Tak         | Nazwa kwalifikowana przez obszar nazw funkcji w modelu magazynu, który jest mapowany. |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły. Rozważmy następującą funkcję w modelu magazynu:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Należy również rozważyć importowanie tej funkcji w modelu koncepcyjnym:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

W poniższych pokazano przykład **FunctionImportMapping** element używany do mapowania funkcji i funkcji importu powyżej ze sobą:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Element InsertFunction (MSL)

**InsertFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji wstawiania w typie jednostki lub skojarzeń w modelu koncepcyjnym procedurę składowaną w bazie danych. Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu. Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).

> [!NOTE]
> Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.

**InsertFunction** element może być elementem podrzędnym elementu ModificationFunctionMapping i zastosowane do elementu EntityTypeMapping lub AssociationSetMapping element.

### <a name="insertfunction-applied-to-entitytypemapping"></a>Stosowane do EntityTypeMapping InsertFunction

W przypadku zastosowania do elementu EntityTypeMapping **InsertFunction** element mapy typu jednostki w modelu koncepcyjnym funkcji wstawiania do procedury składowanej.

**InsertFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **EntityTypeMapping** elementu:

-   Punkt końcowy skojarzenia (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   ResultBinding (zero lub jeden)
-   ScarlarProperty (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **InsertFunction** elementu, gdy jest stosowany do **EntityTypeMapping** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **functionName**          | Tak         | Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji wstawiania. Procedura składowana musi być zadeklarowany w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru output, która zwraca liczbę wierszy dotyczy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania typu jednostki osoby **InsertPerson** procedury składowanej. **InsertPerson** procedury składowanej jest zadeklarowana w modelu magazynu.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a>Stosowane do AssociationSetMapping InsertFunction

W przypadku zastosowania do elementu AssociationSetMapping **InsertFunction** element mapy funkcji wstawiania skojarzenia w modelu koncepcyjnym do procedury składowanej.

**InsertFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **AssociationSetMapping** elementu:

-   Właściwości EndProperty

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **InsertFunction** element, jeśli jest stosowany do **AssociationSetMapping** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **functionName**          | Tak         | Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji wstawiania. Procedura składowana musi być zadeklarowany w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania **CourseInstructor** skojarzenia  **InsertCourseInstructor** procedury składowanej. **InsertCourseInstructor** procedury składowanej jest zadeklarowana w modelu magazynu.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a>Mapowanie elementu (MSL)

**Mapowania** element w mapowaniu language specification (MSL) zawiera informacje umożliwiające mapowanie obiektów, które są zdefiniowane w model koncepcyjny do bazy danych (zgodnie z opisem w modelu pamięci masowej). Aby uzyskać więcej informacji zobacz specyfikacja CSDL i specyfikacja SSDL.

**Mapowania** element jest elementem głównym specyfikacji mapowania. Przestrzeń nazw XML do mapowania specyfikacje jest http://schemas.microsoft.com/ado/2009/11/mapping/cs.

Element mapowania może używać następujących elementów podrzędnych (w podanej kolejności):

-   Alias (zero lub więcej)
-   Elemencie EntityContainerMapping (dokładnie jeden)

Nazwy koncepcje i typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw. Dla informacji o modelu koncepcyjnego nazwa przestrzeni nazw Zobacz Element schematu (CSDL). Informacje o nazwie przestrzeni nazw w modelu magazynu na ten temat można znaleźć w elemencie schematu (SSDL). Aliasy dla przestrzeni nazw, które są używane w pliku MSL można zdefiniować w elemencie aliasu.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **mapowanie** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **miejsce**      | Tak         | **C-S**. To jest wartość stałą i nie można jej zmienić. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **mapowanie** element, który opiera się na części modelu szkoły. Aby uzyskać więcej informacji na temat modelu szkoły zobacz Przewodnik Szybki Start (Entity Framework):

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a>Element MappingFragment (MSL)

**MappingFragment** element w mapowaniu language specification (MSL) definiuje mapowanie między właściwościami typu jednostki modelu koncepcyjnego i tabeli lub widoku w bazie danych. Dla informacji na temat typów jednostek modelu koncepcyjnego i podstawowej bazy danych tabel lub widoków Zobacz Element EntityType (CSDL) i elementu obiektu EntitySet (SSDL). **MappingFragment** może być elementem podrzędnym elementu EntityTypeMapping lub element w obiekcie EntitySetMapping.

**MappingFragment** element może mieć następujące elementy podrzędne:

-   ComplexType (zero lub więcej)
-   ScalarProperty (zero lub więcej)
-   Warunek (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **MappingFragment** elementu.

| Nazwa atrybutu          | Jest wymagany | Wartość                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Tak         | Nazwa tabeli lub widoku, który jest mapowany.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Nie          | **Wartość true,** lub **False** w zależności od tego, czy tylko unikatowe wiersze są zwracane. <br/> Jeśli ten atrybut jest ustawiony na **True**, **GenerateUpdateViews** atrybut elementu w elemencie EntityContainerMapping musi być równa **False**. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **MappingFragment** element jako element podrzędny **EntityTypeMapping** elementu. W tym przykładzie właściwości **kurs** typu w modelu koncepcyjnym są mapowane na kolumny **kurs** tabeli w bazie danych.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **MappingFragment** element jako element podrzędny **obiekcie EntitySetMapping** elementu. Tak jak w przykładzie powyżej właściwości **kurs** typu w modelu koncepcyjnym są mapowane na kolumny **kurs** tabeli w bazie danych.

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a>Element ModificationFunctionMapping (MSL)

**ModificationFunctionMapping** elementu w specyfikacji języka (MSL) mapowania mapuje insert, aktualizowanie i usuwanie funkcji typu modelu koncepcyjnego entity do procedur składowanych w bazie danych. **ModificationFunctionMapping** element można również mapować insert i usuwanie funkcji wiele do wielu skojarzeń w modelu koncepcyjnym do procedur składowanych w bazie danych. Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu. Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).

> [!NOTE]
> Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.


> [!NOTE]
> W przypadku funkcji modyfikacji dla jednej jednostki w hierarchii dziedziczenia są mapowane do procedur składowanych, funkcji modyfikacji dla wszystkich typów w hierarchii musi być zamapowany na procedury składowane.

**ModificationFunctionMapping** element może być elementem podrzędnym elementu EntityTypeMapping lub AssociationSetMapping element.

**ModificationFunctionMapping** element może mieć następujące elementy podrzędne:

-   DeleteFunction (zero lub jeden)
-   InsertFunction (zero lub jeden)
-   UpdateFunction (zero lub jeden)

Atrybuty nie są stosowane do **ModificationFunctionMapping** elementu.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono zestaw mapowanie dla jednostek **osób** zestawu jednostek w modelu szkoły. Oprócz mapowania kolumny **osoby** typu jednostki, mapowanie Wstawianie, aktualizowanie i usuwanie funkcji **osoby** typu są wyświetlane. Funkcje, które są mapowane na są deklarowane w modelu magazynu.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano skojarzenia mapowanie dla zestawu **CourseInstructor** skojarzeń w modelu szkoły. Oprócz mapowania kolumny **CourseInstructor** skojarzenia, mapowanie funkcje insert i delete **CourseInstructor** skojarzeń są wyświetlane. Funkcje, które są mapowane na są deklarowane w modelu magazynu.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a>Obiekt QueryView — Element (MSL)

**QueryView** elementu w mapowaniu language specification (MSL) definiuje tylko do odczytu mapowanie między typie jednostki lub skojarzeń w modelu koncepcyjnego i tabelę w bazie danych. Mapowanie jest zdefiniowana za pomocą zapytanie SQL jednostki, które zostaną ocenione względem model magazynu i express zestawu pod względem jednostki lub skojarzeń w modelu koncepcyjnym wyników. Ponieważ widoków zapytań są tylko do odczytu, nie możesz użyć polecenia standardowych aktualizacji można zaktualizować typów, które są definiowane przez widoki kwerendę. Należy tworzyć aktualizacje do tych typów, przy użyciu funkcji modyfikacji. Aby uzyskać więcej informacji, zobacz jak: funkcje modyfikacji mapy do procedur składowanych.

> [!NOTE]
> W **QueryView** element, wyrażenia jednostki SQL, które zawierają **GroupBy**, agregacje grupy lub właściwości nawigacji nie są obsługiwane.

 

**QueryView** element może być elementem podrzędnym elementu obiekcie EntitySetMapping lub AssociationSetMapping elementu. W pierwszym przypadku widok zapytania definiuje mapowanie tylko do odczytu dla jednostki w modelu koncepcyjnym. W tym ostatnim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla skojarzenia w modelu koncepcyjnym.

> [!NOTE]
> Jeśli **AssociationSetMapping** element jest skojarzenie z ograniczeniem referencyjnym, **AssociationSetMapping** element jest ignorowany. Aby uzyskać więcej informacji zobacz Element ReferentialConstraint (CSDL).

**QueryView** elementu nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **QueryView** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Nazwa typu**   | Nie          | Nazwa typu modelu koncepcyjnego, który jest mapowany w widoku zapytania. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **QueryView** element jako element podrzędny elementu **obiekcie EntitySetMapping** elementu i definiuje mapowanie widoku zapytania **działu** typu jednostki w Model szkoły.

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

Ponieważ zapytanie zwraca tylko podzestaw elementów członkowskich **działu** typu w modelu magazynu **działu** typu w modelu szkoły została zmodyfikowana oparte na tym mapowanie w następujący sposób:

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a>Przykład

W następnym przykładzie pokazano **QueryView** element jako element podrzędny **AssociationSetMapping** elementu i definiuje mapowanie tylko do odczytu dla `FK_Course_Department` skojarzeń w modelu szkoły.

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a>Komentarze

Można zdefiniować widoki kwerendę w następujących scenariuszach:

-   Zdefiniuj jednostki w modelu koncepcyjnym, który nie zawiera wszystkich właściwości jednostki w modelu magazynu. Obejmuje to właściwości, które nie mają przypisane wartości domyślne, a nie obsługują **null** wartości.
-   Mapowanie kolumn obliczanych w modelu magazynu do właściwości typów jednostek w modelu koncepcyjnym.
-   Zdefiniuj mapowania, gdzie warunków używanych do jednostek w partycji w modelu koncepcyjnym nie są oparte na równości. Po określeniu warunkowego mapowania przy użyciu **warunek** elementu podanym warunku musi być równa określonej wartości. Aby uzyskać więcej informacji zobacz Element warunek (MSL).
-   Tej samej kolumny w modelu magazynu są mapowane na wiele typów w modelu koncepcyjnym.
-   Wiele typów są mapowane na tej samej tabeli.
-   Definiowanie skojarzeń w modelu koncepcyjnym, które nie są oparte na klucze obce w relacyjnej schematu.
-   Użyj niestandardowej logiki biznesowej do ustawiania wartości właściwości w modelu koncepcyjnym. Na przykład można mapować wartość ciągu "T" w źródle danych na wartość **true**, wartość logiczna, w modelu koncepcyjnym.
-   Zdefiniuj filtry warunkowe dla wyników zapytań.
-   Wymuszanie mniej ograniczeń danych w modelu koncepcyjnym niż w modelu magazynu. Na przykład można wprowadzone właściwości w modelu koncepcyjnym dopuszczającego wartość null, nawet jeśli nie obsługuje kolumn, na który jest mapowany **null**wartości.

Podczas definiowania widoków zapytań dla jednostek obowiązują następujące zastrzeżenia:

-   Widoki kwerendę są tylko do odczytu. Można tworzyć tylko aktualizacje do jednostek przy użyciu funkcji modyfikacji.
-   Po zdefiniowaniu typu jednostki, widok zapytania można również definiować wszystkie powiązane jednostki przez widoki kwerendę.
-   Kiedy mapujesz skojarzenia typu wiele do wielu z jednostką w modelu magazynu, który reprezentuje tabelę łącze w schemacie relacyjnych należy zdefiniować **QueryView** element **AssociationSetMapping** element do tej tabeli łącza.
-   Widoki kwerendę muszą być zdefiniowane dla wszystkich typów w hierarchii typów. Można to zrobić w następujący sposób:
-   -   Za pomocą jednego **QueryView** element, który określa pojedyncze zapytanie jednostki SQL, które zwraca sumę wszystkich typów jednostek w hierarchii.
    -   Za pomocą jednego **QueryView** element, który określa pojedyncze zapytanie jednostki SQL, które używa operatora wielkości liter do zwrócenia określonego typu w hierarchii na podstawie określonego warunku.
    -   Przy użyciu dodatkowego **QueryView** elementu dla określonego typu w hierarchii. W takim przypadku należy użyć **TypeName** atrybutu **QueryView** elementu, aby określić typ jednostki dla każdego widoku.
-   Po zdefiniowaniu widok zapytania nie można określić **StorageSetName** atrybutu na **obiekcie EntitySetMapping** elementu.
-   Po zdefiniowaniu widok zapytania **obiekcie EntitySetMapping**element nie może również zawierać **właściwość** mapowania.

## <a name="resultbinding-element-msl"></a>Element ResultBinding (MSL)

**ResultBinding** elementu w specyfikacji języka (MSL) mapowania mapuje wartości kolumn, które są zwracane przez procedury składowane do właściwości jednostki w modelu koncepcyjnym w przypadku funkcji modyfikacji typu jednostki są mapowane do przechowywanej procedury przedstawione w źródłowej bazie danych. Na przykład, kiedy wartość w kolumnie tożsamości jest zwracany przez wstawiania procedurą składowaną, **ResultBinding** element mapuje zwracanej wartości do właściwości typu jednostki w modelu koncepcyjnym.

**ResultBinding** element może być element podrzędny elementu InsertFunction lub UpdateFunction element.

**ResultBinding** elementu nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ResultBinding** elementu:

| Nazwa atrybutu | Jest wymagany | Wartość                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości jednostki w modelu koncepcyjnym, który jest mapowany. |
| **NazwaKolumny** | Tak         | Nazwa kolumny mapowany.                                          |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania **osoby** typu jednostki **InsertPerson** Procedura składowana. ( **InsertPerson** procedury składowanej jest pokazany poniżej i jest zadeklarowana w modelu magazynu.) A **ResultBinding** element jest używany do mapowania wartości kolumny, która jest zwracana przez procedurę składowaną (**NewPersonID**) z właściwością typu jednostki (**PersonID**).

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

W tym artykule opisano języka Transact-SQL **InsertPerson** procedura składowana:

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a>Element ResultMapping (MSL)

**ResultMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnym i procedurę składowaną w bazie danych, gdy spełnione są następujące:

-   Importowanie funkcji zwraca modelu koncepcyjnego typu jednostki lub typ złożony.
-   Nazwy kolumn zwrócone przez procedurę składowaną nie odpowiadają dokładnie nazwy właściwości w typie jednostki lub typ złożony.

Domyślnie mapowania między kolumnami zwrócone przez procedurę składowaną i typie jednostki lub typ złożony opiera się na nazwy kolumn i właściwości. Nazwy kolumn nie odpowiadają dokładnie nazw właściwości, należy użyć **ResultMapping** elementu, aby zdefiniować mapowanie. Aby uzyskać przykład domyślnego mapowania Zobacz Element FunctionImportMapping (MSL).

**ResultMapping** element jest elementem podrzędnym elementu FunctionImportMapping.

**ResultMapping** element może mieć następujące elementy podrzędne:

-   EntityTypeMapping (zero lub więcej)
-   ComplexTypeMapping

Atrybuty nie są stosowane do **ResultMapping** elementu.

### <a name="example"></a>Przykład

Należy wziąć pod uwagę następujące procedury składowanej:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Ponadto należy wziąć pod uwagę następujące typie modelu koncepcyjnego entity:

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

Aby można było utworzyć importu funkcji, który zwraca wystąpienia poprzedni typ jednostki, mapowanie między kolumnami zwrócone przez procedurę składowaną i typ jednostki musi być zdefiniowany w **ResultMapping** elementu:

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a>Element ScalarProperty (MSL)

**ScalarProperty** elementu w specyfikacji języka (MSL) mapowania mapuje właściwość typu jednostki modelu koncepcyjnego, typu złożonego lub skojarzenie tabeli kolumna lub parametr procedurę składowaną w bazie danych.

> [!NOTE]
> Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu. Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).

**ScalarProperty** element może być elementem podrzędnym następujące elementy:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   Właściwości EndProperty
-   ComplexProperty
-   ResultMapping

Jako element podrzędny elementu **MappingFragment**, **ComplexProperty**, lub **właściwości EndProperty** elementu **ScalarProperty** element mapy właściwości w modelu koncepcyjnym z kolumną w bazie danych. Jako element podrzędny elementu **InsertFunction**, **UpdateFunction**, lub **DeleteFunction** elementu **ScalarProperty** element mapy właściwości w modelu koncepcyjnym do parametru procedury składowanej.

**ScalarProperty** elementu nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Atrybuty, które są stosowane do **ScalarProperty** elementu różnią się w zależności od roli tego elementu.

W poniższej tabeli opisano atrybuty, które są stosowane, kiedy **ScalarProperty** element jest używany do mapowania właściwości modelu koncepcyjnego z kolumną w bazie danych:

| Nazwa atrybutu | Jest wymagany | Wartość                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości modelu koncepcyjnego, który jest mapowany. |
| **NazwaKolumny** | Tak         | Nazwa kolumny tabeli, który jest mapowany.              |

W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ScalarProperty** elementu, gdy jest używany do mapowania właściwości modelu koncepcyjnego parametru procedury składowanej:

| Nazwa atrybutu    | Jest wymagany | Wartość                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**          | Tak         | Nazwa właściwości modelu koncepcyjnego, który jest mapowany.                                                                                 |
| **Nazwa parametru** | Tak         | Nazwa parametru, który jest mapowany.                                                                                                 |
| **Wersja**       | Nie          | **Bieżący** lub **oryginalnego** w zależności od tego, czy bieżąca wartość lub oryginalnej wartości właściwości powinny być używane do kontroli współbieżności. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **ScalarProperty** element używany na dwa sposoby:

-   Do mapowania właściwości **osoby** typ jednostki do kolumn **osoby**tabeli.
-   Do mapowania właściwości **osoby** typu jednostki parametrom **UpdatePerson** procedury składowanej. Procedury składowane są deklarowane w modelu magazynu.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Przykład

W następnym przykładzie pokazano **ScalarProperty** element używany do mapowania Wstawianie i usuwanie funkcji skojarzenia model koncepcyjny do procedur składowanych w bazie danych. Procedury składowane są deklarowane w modelu magazynu.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a>Element UpdateFunction (MSL)

**UpdateFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji aktualizacji typu jednostki w modelu koncepcyjnym procedurę składowaną w bazie danych. Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu. Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).

> [!NOTE]
>  Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.

**UpdateFunction** element może być elementem podrzędnym elementu ModificationFunctionMapping i stosowane do elementu EntityTypeMapping.

**UpdateFunction** element może mieć następujące elementy podrzędne:

-   Punkt końcowy skojarzenia (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   ResultBinding (zero lub jeden)
-   ScarlarProperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **UpdateFunction** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **functionName**          | Tak         | Przestrzeń nazw kwalifikowana nazwa procedury składowanej zmapowany funkcji update. Procedura składowana musi być zadeklarowany w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.                                                                               |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje **UpdateFunction** element używany do mapowania funkcji aktualizacji **osoby** typu jednostki **UpdatePerson** Procedura składowana. **UpdatePerson** procedury składowanej jest zadeklarowana w modelu magazynu.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
