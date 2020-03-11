---
title: Specyfikacja MSL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418733"
---
# <a name="msl-specification"></a>Specyfikacja MSL
Mapowanie specyfikacji języka (MSL) to język oparty na języku XML, który opisuje mapowanie między modelem koncepcyjnym i modelem magazynu aplikacji Entity Framework.

W aplikacji Entity Framework mapowanie metadanych jest ładowane z pliku MSL (zapisywane w bibliotece MSL) w czasie kompilacji. Entity Framework używa mapowania metadanych w czasie wykonywania do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla magazynu.

Entity Framework Designer (programu EF Designer) przechowuje informacje o mapowaniu w pliku edmx w czasie projektowania. W czasie kompilacji Entity Designer używa informacji w pliku. edmx, aby utworzyć plik. MSL, który jest wymagany przez Entity Framework w czasie wykonywania

Nazwy wszystkich typów modelu koncepcyjnego lub magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według odpowiednich nazw przestrzeni nazw. Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Wersje pliku MSL są odróżniane według przestrzeni nazw XML.

| Wersja pliku MSL | Przestrzeń nazw XML                                        |
|:------------|:-----------------------------------------------------|
| MSL, wersja 1      | urn: schematys-Microsoft-com: Windows: Storage: Mapping: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL v3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Element aliasu (MSL)

Element **aliasu** w języku specyfikacji mapowania (MSL) jest elementem podrzędnym elementu Mapping, który jest używany do definiowania aliasów dla modelu koncepcyjnego i przestrzeni nazw modelu magazynu. Nazwy wszystkich typów modelu koncepcyjnego lub magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według odpowiednich nazw przestrzeni nazw. Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz element Schema (CSDL). Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz element Schema (SSDL).

Element **aliasu** nie może mieć elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **alias** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Klucz**        | Yes         | Alias dla przestrzeni nazw, który jest określony przez atrybut **Value** . |
| **Wartość**      | Yes         | Przestrzeń nazw, dla której wartość elementu **Key** jest aliasem.     |

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element **aliasu** , który definiuje alias, `c`dla typów, które są zdefiniowane w modelu koncepcyjnym.

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a>AssociationEnd — element (MSL)

Element **AssociationEnd** w języku specyfikacji mapowania (MSL) jest używany, gdy funkcje modyfikacji typu jednostki w modelu koncepcyjnym są mapowane na procedury składowane w źródłowej bazie danych. Jeśli procedura składowana modyfikacji przyjmuje parametr, którego wartość jest przechowywana we właściwości skojarzenia, element **AssociationEnd** mapuje wartość właściwości na parametr. Aby uzyskać więcej informacji, zobacz Poniższy przykład.

Aby uzyskać więcej informacji na temat mapowania funkcji modyfikacji typów jednostek do procedur składowanych, zobacz ModificationFunctionMapping element (MSL) i przewodnik: mapowanie jednostki na procedury składowane.

Element **AssociationEnd** może mieć następujące elementy podrzędne:

-   Element scalarproperty

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **AssociationEnd** .

| Nazwa atrybutu     | Jest wymagana | Wartość                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Yes         | Nazwa mapowanego skojarzenia.                                                                                                                                 |
| **Wniosek**           | Yes         | Wartość atrybutu **FromRole** właściwości nawigacji, która odnosi się do mapowanego skojarzenia. Aby uzyskać więcej informacji, zobacz element NavigationProperty (CSDL). |
| **Do**             | Yes         | Wartość atrybutu **ToRole** właściwości nawigacji, która odnosi się do mapowanego skojarzenia. Aby uzyskać więcej informacji, zobacz element NavigationProperty (CSDL).   |

### <a name="example"></a>Przykład

Rozważmy następujący typ jednostki koncepcyjnej:

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

Należy również wziąć pod uwagę następujące procedury składowane:

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

Aby zmapować funkcję Update jednostki `Course` na tę procedurę składowaną, należy podać wartość parametru **DepartmentID** . Wartość `DepartmentID` nie odpowiada właściwości typu jednostki; jest on zawarty w niezależnym skojarzeniu, którego mapowanie jest pokazane tutaj:

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

Poniższy kod przedstawia element **AssociationEnd** , który służy do mapowania właściwości **DepartmentID** **klucza obcego\_go skojarzenia działu\_go** w ramach procedury składowanej **UpdateCourse** (do której jest zamapowana funkcja Update typu jednostki **kursu** ):

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

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping — element (MSL)

Element **AssociationSetMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między skojarzeniem w modelu koncepcyjnym i kolumnami tabeli w źródłowej bazie danych.

Skojarzenia w modelu koncepcyjnym są typami, których właściwości reprezentują kolumny klucza podstawowego i obcego w źródłowej bazie danych. Element **AssociationSetMapping** używa dwóch elementów EndProperty do definiowania mapowań między właściwościami typu skojarzenia a kolumnami w bazie danych. Warunki dotyczące tych mapowań można umieścić przy użyciu elementu Condition. Mapuj funkcje INSERT, Update i DELETE dla skojarzeń z procedurami przechowywanymi w bazie danych z elementem ModificationFunctionMapping. Zdefiniuj mapowania tylko do odczytu między skojarzeniami a kolumnami tabeli przy użyciu ciągu Entity SQL w elemencie QueryView.

> [!NOTE]
> Jeśli ograniczenie referencyjne jest zdefiniowane dla skojarzenia w modelu koncepcyjnym, skojarzenie nie musi być mapowane przy użyciu elementu **AssociationSetMapping** . Jeśli element **AssociationSetMapping** jest obecny dla skojarzenia, które ma ograniczenie referencyjne, mapowania zdefiniowane w elemencie **AssociationSetMapping** zostaną zignorowane. Aby uzyskać więcej informacji, zobacz ReferentialConstraint element (CSDL).

Element **AssociationSetMapping** może mieć następujące elementy podrzędne

-   QueryView (zero lub jeden)
-   EndProperty (zero lub dwa)
-   Warunek (zero lub więcej)
-   ModificationFunctionMapping (zero lub jeden)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSetMapping** .

| Nazwa atrybutu     | Jest wymagana | Wartość                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Nazwa**           | Yes         | Nazwa mapowanego zestawu skojarzeń modelu koncepcyjnego.                      |
| **Nazwa**       | Nie          | Kwalifikowana nazwa przestrzeni nazw typu powiązania modelu koncepcyjnego, który jest mapowany. |
| **StoreEntitySet** | Nie          | Nazwa mapowanej tabeli.                                                 |

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element **AssociationSetMapping** , w którym w modelu koncepcyjnym zostanie zamapowany skojarzenie " **\_y działu\_kursu** , który jest mapowany do tabeli **kursów** w bazie danych. Mapowania między właściwościami typu skojarzenia i kolumnami tabeli są określone w podrzędnych elementach **EndProperty** .

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

## <a name="complexproperty-element-msl"></a>ComplexProperty — element (MSL)

Element **ComplexProperty** w języku specyfikacji mapowania (MSL) definiuje mapowanie między właściwością typu złożonego w typie jednostki modelu koncepcyjnego a kolumnami tabeli w źródłowej bazie danych. Mapowania kolumn właściwości są określone w podrzędnych elementach element scalarproperty.

Element właściwości **complexType** może mieć następujące elementy podrzędne:

-   Element scalarproperty (zero lub więcej)
-   **ComplexProperty** (zero lub więcej)
-   ComplextTypeMapping (zero lub więcej)
-   Warunek (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **ComplexProperty** :

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nazwa**       | Yes         | Nazwa właściwości złożonej typu jednostki w modelu koncepcyjnym, który jest mapowany. |
| **Nazwa**   | Nie          | Kwalifikowana nazwa obszaru nazw typu właściwości modelu koncepcyjnego.                              |

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

Właściwości **LastName** i **FirstName** typu podmiotu **osoby** zostały zastąpione jedną złożoną właściwością. **Nazwa**:

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

W poniższym pliku MSL przedstawiono element **ComplexProperty** używany do mapowania właściwości **name** na kolumny w źródłowej bazie danych:

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

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping — element (MSL)

Element **ComplexTypeMapping** w języku specyfikacji mapowania (MSL) jest elementem podrzędnym elementu ResultMapping i definiuje mapowanie między importem funkcji w modelu koncepcyjnym i procedurą przechowywaną w źródłowej bazie danych, gdy są spełnione następujące warunki:

-   Import funkcji zwraca typ złożony koncepcyjnie.
-   Nazwy kolumn zwracanych przez procedurę składowaną nie są dokładnie zgodne z nazwami właściwości typu złożonego.

Domyślnie mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem złożonym opiera się na nazwach kolumn i właściwości. Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, należy użyć elementu **ComplexTypeMapping** , aby zdefiniować mapowanie. Przykład mapowania domyślnego można znaleźć w temacie FunctionImportMapping element (MSL).

Element **ComplexTypeMapping** może mieć następujące elementy podrzędne:

-   Element scalarproperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **ComplexTypeMapping** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Nazwa**   | Yes         | Kwalifikowana nazwa przestrzeni nazw typu złożonego, który jest zamapowany. |

### <a name="example"></a>Przykład

Weź pod uwagę następujące procedury składowane:

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

Należy również wziąć pod uwagę następujący typ złożony modelu koncepcyjnego:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Aby można było utworzyć import funkcji, który zwraca wystąpienia poprzedniego typu złożonego, mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki musi być zdefiniowane w elemencie **ComplexTypeMapping** :

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

## <a name="condition-element-msl"></a>Condition — element (MSL)

Element **Condition** języka specyfikacji mapowania (MSL) umieszcza warunki mapowania między modelem koncepcyjnym a podstawową bazą danych. Mapowanie zdefiniowane w węźle XML jest prawidłowe, jeśli są spełnione wszystkie warunki określone w elementach **warunku** podrzędnego. W przeciwnym razie mapowanie jest nieprawidłowe. Na przykład, jeśli element element mappingfragment zawiera jeden lub więcej elementów podrzędnych **warunku** , Mapowanie zdefiniowane w węźle **element mappingfragment** będzie prawidłowe tylko wtedy, gdy spełnione są wszystkie **warunki elementów podrzędnych warunków.**

Każdy warunek może być stosowany do **nazwy** (nazwy właściwości jednostki koncepcyjnej, określonej przez atrybut **nazwy** ) lub do **ColumnName** (nazwa kolumny w bazie danych, określona przez atrybut **ColumnName** ). Gdy atrybut **name** jest ustawiony, warunek jest sprawdzany względem wartości właściwości Entity. Gdy atrybut **ColumnName** jest ustawiony, warunek jest sprawdzany względem wartości kolumny. Tylko jeden z atrybutów **name** lub **ColumnName** może być określony w elemencie **Condition** .

> [!NOTE]
> Gdy element **Condition** jest używany w elemencie FunctionImportMapping, tylko atrybut **name** nie ma zastosowania.

**Warunek Condition** może być elementem podrzędnym następujących elementów:

-   AssociationSetMapping
-   ComplexProperty
-   Elementu EntitySetMapping
-   Element mappingfragment
-   EntityTypeMapping

Element **Condition** nie może mieć elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **Condition** :

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Nie          | Nazwa kolumny tabeli, której wartość jest używana do obliczania warunku.                                                                                                                                                                                                                   |
| **IsNull**     | Nie          | **True** lub **False**. Jeśli wartość jest **równa true** , a wartość kolumny ma wartość **null**lub jeśli wartość jest równa **false** , a wartość kolumny nie jest **równa null**, warunek ma wartość true. W przeciwnym razie warunek ma wartość false. <br/> Nie można jednocześnie używać atrybutów **IsNull** i **Value** . |
| **Wartość**      | Nie          | Wartość, z którą jest porównywana wartość kolumny. Jeśli wartości są takie same, warunek ma wartość true. W przeciwnym razie warunek ma wartość false. <br/> Nie można jednocześnie używać atrybutów **IsNull** i **Value** .                                                                       |
| **Nazwa**       | Nie          | Nazwa właściwości jednostki modelu koncepcyjnego, której wartość jest używana do obliczania warunku. <br/> Ten atrybut nie ma zastosowania, jeśli element **Condition** jest używany w elemencie FunctionImportMapping.                                                                           |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje elementy **Condition** jako elementy podrzędne elementów **element mappingfragment** . Gdy **HireDate** nie ma wartości null **i EnrollmentDate** ma wartość null, dane są mapowane **między SchoolModel. typ instruktora** i kolumny **PersonID** i **HireDate** tabeli **Person** . Gdy **EnrollmentDate** nie ma wartości null i **HireDate** ma wartość null, dane są mapowane **między SchoolModel. studenta** i kolumny **PersonID** i **Enrollment** w tabeli **Person** .

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

## <a name="deletefunction-element-msl"></a>DeleteFunction — element (MSL)

Element **DeleteFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję delete typu jednostki lub skojarzenia w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych. Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).

> [!NOTE]
> Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction zastosowana do EntityTypeMapping

W przypadku zastosowania do elementu EntityTypeMapping element **DeleteFunction** mapuje funkcję delete typu jednostki w modelu koncepcyjnym do procedury składowanej.

Element **DeleteFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **EntityTypeMapping** :

-   AssociationEnd (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   ScarlarProperty (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **DeleteFunction** , gdy jest on stosowany do elementu **EntityTypeMapping** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Yes         | Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja Delete. Procedura składowana musi być zadeklarowana w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje, że element **DeleteFunction** jest mapowany do procedury składowanej **DeletePerson** . Procedura składowana **DeletePerson** jest zadeklarowana w modelu magazynu.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction zastosowana do AssociationSetMapping

Po zastosowaniu do elementu AssociationSetMapping element **DeleteFunction** mapuje funkcję delete skojarzenia w modelu koncepcyjnym do procedury składowanej.

Element **DeleteFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **DeleteFunction** , gdy jest on stosowany do elementu **AssociationSetMapping** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Yes         | Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja Delete. Procedura składowana musi być zadeklarowana w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje element **DeleteFunction** używany do mapowania funkcji Delete skojarzenia **CourseInstructor** do procedury składowanej **DeleteCourseInstructor** . Procedura składowana **DeleteCourseInstructor** jest zadeklarowana w modelu magazynu.

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

## <a name="endproperty-element-msl"></a>EndProperty — element (MSL)

Element **EndProperty** w języku specyfikacji mapowania (MSL) definiuje mapowanie między końcem lub funkcją modyfikacji skojarzenia modelu koncepcyjnego a podstawową bazą danych. Mapowanie kolumny właściwości jest określone w podrzędnym elemencie element scalarproperty.

Gdy element **EndProperty** jest używany do definiowania mapowania dla końca skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu AssociationSetMapping. Gdy element **EndProperty** jest używany do definiowania mapowania dla funkcji modyfikacji skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu InsertFunction lub DeleteFunction.

Element **EndProperty** może mieć następujące elementy podrzędne:

-   Element scalarproperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **EndProperty** :

| Nazwa atrybutu | Jest wymagana | Wartość                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Name (Nazwa)           | Yes         | Nazwa elementu end skojarzenia, który jest mapowany. |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono element **AssociationSetMapping** , w\_którym skojarzenie " **\_** " w modelu koncepcyjnym jest zamapowane na tabelę **kursów** w bazie danych. Mapowania między właściwościami typu skojarzenia i kolumnami tabeli są określone w podrzędnych elementach **EndProperty** .

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

Poniższy przykład przedstawia mapowanie elementu **EndProperty** funkcji INSERT i DELETE skojarzenia (**CourseInstructor**) na procedury składowane w źródłowej bazie danych. Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.

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

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping — element (MSL)

Element **EntityContainerMapping** w języku specyfikacji mapowania (MSL) mapuje kontener Entity w modelu koncepcyjnym do kontenera jednostek w modelu magazynu. Element **EntityContainerMapping** jest elementem podrzędnym elementu mapowania.

Element **EntityContainerMapping** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Elementu EntitySetMapping (zero lub więcej)
-   AssociationSetMapping (zero lub więcej)
-   FunctionImportMapping (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityContainerMapping** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Yes         | Nazwa mapowanego kontenera jednostek modelu magazynu.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Yes         | Nazwa mapowanego kontenera jednostek modelu koncepcyjnego.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Nie          | **True** lub **False**. W przypadku **wartości false**nie są generowane żadne widoki aktualizacji. Ten atrybut powinien mieć ustawioną **wartość false** , jeśli masz mapowanie tylko do odczytu, które byłoby nieprawidłowe, ponieważ dane mogły się nie przewidzieć pomyślnie. <br/> Wartość domyślna to **True**. |

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element **EntityContainerMapping** , który mapuje kontener **SchoolModelEntities** (kontener jednostki modelu koncepcyjnego) do kontenera **SchoolModelStoreContainer** (kontener jednostek modelu magazynu):

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

## <a name="entitysetmapping-element-msl"></a>Elementu EntitySetMapping — element (MSL)

Element **elementu EntitySetMapping** w języku specyfikacji mapowania (MSL) mapuje wszystkie typy w jednostce modelu koncepcyjnego na zestaw jednostek w modelu magazynu. Jednostka ustawiona w modelu koncepcyjnym jest kontenerem logicznym dla wystąpień jednostek tego samego typu (i typów pochodnych). Jednostka ustawiona w modelu magazynu reprezentuje tabelę lub widok w źródłowej bazie danych. Zestaw jednostek koncepcyjnych modelu jest określany przez wartość atrybutu **name** elementu **elementu EntitySetMapping** . Zmapowana tabela lub widok jest określona przez atrybut **StoreEntitySet** w każdym podrzędnym elemencie element mappingfragment lub w samym elemencie **elementu EntitySetMapping** .

Element **elementu EntitySetMapping** może mieć następujące elementy podrzędne:

-   EntityTypeMapping (zero lub więcej)
-   QueryView (zero lub jeden)
-   Element mappingfragment (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **elementu EntitySetMapping** .

| Nazwa atrybutu           | Jest wymagana | Wartość                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                 | Yes         | Nazwa mapowanego zestawu jednostek modelu koncepcyjnego.                                                                                                                                                             |
| **Nazwa elementu TypeName** **1**       | Nie          | Nazwa mapowanego typu jednostki modelu koncepcyjnego.                                                                                                                                                            |
| **StoreEntitySet** **1** | Nie          | Nazwa zestawu jednostek modelu magazynu, do którego jest mapowany.                                                                                                                                                             |
| **Atrybutem makecolumnsdistinct**  | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy zwracane są tylko unikatowe wiersze. <br/> Jeśli ten atrybut ma **wartość true**, atrybut **GenerateUpdateViews** elementu EntityContainerMapping musi być ustawiony na **wartość false**. |

 

**1** atrybuty **TypeName** i **StoreEntitySet** mogą być używane zamiast elementów podrzędnych EntityTypeMapping i element mappingfragment, aby mapować pojedynczy typ jednostki na pojedynczą tabelę.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element **elementu EntitySetMapping** , który mapuje trzy typy (typ podstawowy i dwa typy pochodne) w zestawie jednostek **kursów** modelu koncepcyjnego na trzy różne tabele w źródłowej bazie danych. Tabele są określane przez atrybut **StoreEntitySet** w każdym elemencie **element mappingfragment** .

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

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping — element (MSL)

Element **EntityTypeMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między typem jednostki w modelu koncepcyjnym i tabelami lub widokami w źródłowej bazie danych. Aby uzyskać informacje na temat typów jednostek modelu koncepcyjnego i tabel lub widoków bazy danych, zobacz element EntityType (CSDL) i element EntitySet (SSDL). Mapowany typ jednostki modelu koncepcyjnego jest określany przez atrybut **TypeName** elementu **EntityTypeMapping** . Mapowana tabela lub widok jest określany przez atrybut **StoreEntitySet** podrzędnego elementu element mappingfragment.

Elementu podrzędnego ModificationFunctionMapping można użyć do mapowania funkcji INSERT, Update lub DELETE typów jednostek na procedury składowane w bazie danych.

Element **EntityTypeMapping** może mieć następujące elementy podrzędne:

-   Element mappingfragment (zero lub więcej)
-   ModificationFunctionMapping (zero lub jeden)
-   Element scalarproperty
-   Warunek

> [!NOTE]
> Elementy **element mappingfragment** i **ModificationFunctionMapping** nie mogą być elementami podrzędnymi elementu **EntityTypeMapping** w tym samym czasie.


> [!NOTE]
> Elementy **element scalarproperty** i **Condition** mogą być tylko elementami podrzędnymi elementu **EntityTypeMapping** , jeśli są używane w elemencie FunctionImportMapping.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityTypeMapping** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**   | Yes         | Kwalifikowana nazwa przestrzeni nazw typu jednostki modelu koncepcyjnego, który jest mapowany. <br/> Jeśli typ jest abstrakcyjny lub typem pochodnym, wartość musi być `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element elementu EntitySetMapping z dwoma podrzędnymi elementami **EntityTypeMapping** . W pierwszym elemencie **EntityTypeMapping** typ jednostki **SchoolModel. Person** jest mapowany do tabeli **Person** . W drugim elemencie **EntityTypeMapping** funkcje aktualizacji typu **SchoolModel. Person** są mapowane na procedurę składowaną **UpdatePerson**, w bazie danych.

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

W następnym przykładzie pokazano mapowanie hierarchii typów, w której typ główny jest abstrakcyjny. Zwróć uwagę na użycie składni `IsOfType` dla atrybutów **TypeName** .

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

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping — element (MSL)

Element **FunctionImportMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między importem funkcji w modelu koncepcyjnym a przechowywaną procedurą lub funkcją w źródłowej bazie danych. Importy funkcji muszą być zadeklarowane w modelu koncepcyjnym, a procedury składowane muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz element FunctionImport (CSDL) i element funkcji (SSDL).

> [!NOTE]
> Domyślnie, jeśli import funkcji zwraca typ jednostki koncepcyjnej lub typ złożony, nazwy kolumn zwracanych przez źródłową procedurę składowaną muszą dokładnie pasować do nazw właściwości w typie modelu koncepcyjnego. Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, mapowanie musi być zdefiniowane w elemencie ResultMapping.

Element **FunctionImportMapping** może mieć następujące elementy podrzędne:

-   ResultMapping (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **FunctionImportMapping** :

| Nazwa atrybutu         | Jest wymagana | Wartość                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Yes         | Nazwa importowanej funkcji w modelu koncepcyjnym, który jest mapowany.           |
| **FunctionName**       | Yes         | Kwalifikowana nazwa przestrzeni nazw funkcji w modelu magazynu, który jest zamapowany. |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły. W modelu magazynu należy wziąć pod uwagę następującą funkcję:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Należy również wziąć pod uwagę ten import funkcji w modelu koncepcyjnym:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Poniższy przykład przedstawia element **FunctionImportMapping** używany do mapowania funkcji i importowania funkcji powyżej do siebie:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction — element (MSL)

Element **InsertFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję INSERT typu jednostki lub skojarzenia w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych. Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).

> [!NOTE]
> Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.

Element **InsertFunction** może być elementem podrzędnym elementu ModificationFunctionMapping i stosowany do elementu EntityTypeMapping lub AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction zastosowana do EntityTypeMapping

W przypadku zastosowania do elementu EntityTypeMapping element **InsertFunction** mapuje funkcję INSERT typu jednostki w modelu koncepcyjnym do procedury składowanej.

Element **InsertFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **EntityTypeMapping** :

-   AssociationEnd (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   Resultbinding (zero lub jeden)
-   ScarlarProperty (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **InsertFunction** w przypadku zastosowania do elementu **EntityTypeMapping** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Yes         | Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja INSERT. Procedura składowana musi być zadeklarowana w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru wyjściowego zwracająca liczbę odnośnych wierszy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje element **InsertFunction** używany do mapowania funkcji INSERT typu jednostki osoby do procedury składowanej **InsertPerson** . Procedura składowana **InsertPerson** jest zadeklarowana w modelu magazynu.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction zastosowana do AssociationSetMapping

W przypadku zastosowania do elementu AssociationSetMapping element **InsertFunction** mapuje funkcję INSERT skojarzenia w modelu koncepcyjnym do procedury składowanej.

Element **InsertFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **InsertFunction** , gdy jest on stosowany do elementu **AssociationSetMapping** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Yes         | Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja INSERT. Procedura składowana musi być zadeklarowana w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.                                                                               |

#### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje element **InsertFunction** używany do mapowania funkcji INSERT skojarzenia **CourseInstructor** do procedury składowanej **InsertCourseInstructor** . Procedura składowana **InsertCourseInstructor** jest zadeklarowana w modelu magazynu.

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

## <a name="mapping-element-msl"></a>Element Mapping (MSL)

Element **Mapping** w języku specyfikacji mapowania (MSL) zawiera informacje dotyczące mapowania obiektów zdefiniowanych w modelu koncepcyjnym do bazy danych (zgodnie z opisem w modelu magazynu). Aby uzyskać więcej informacji, zobacz Specyfikacja CSDL i Specyfikacja SSDL.

Element **Mapping** jest elementem głównym dla specyfikacji mapowania. Przestrzeń nazw XML dla specyfikacji mapowania jest https://schemas.microsoft.com/ado/2009/11/mapping/cs.

Element Mapping może mieć następujące elementy podrzędne (w podanej kolejności):

-   Alias (zero lub więcej)
-   EntityContainerMapping (dokładnie jeden)

Nazwy typów modelu koncepcyjnego i magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według ich nazw. Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz element Schema (CSDL). Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz element Schema (SSDL). Aliasy dla przestrzeni nazw, które są używane w pliku MSL, można zdefiniować za pomocą elementu alias.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **mapowania** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Odstęp**      | Yes         | **C-S**. Jest to stała wartość i nie można jej zmienić. |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **mapowania** , który jest oparty na części modelu szkoły. Aby uzyskać więcej informacji na temat modelu szkoły, zobacz Szybki Start (Entity Framework):

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a>Element mappingfragment — element (MSL)

Element **element mappingfragment** w języku specyfikacji mapowania (MSL) definiuje mapowanie między właściwościami typu jednostki modelu koncepcyjnego a tabelą lub widokiem w bazie danych. Aby uzyskać informacje na temat typów jednostek modelu koncepcyjnego i tabel lub widoków bazy danych, zobacz element EntityType (CSDL) i element EntitySet (SSDL). **Element mappingfragment** może być elementem podrzędnym elementu EntityTypeMapping lub elementu EntitySetMapping.

Element **element mappingfragment** może mieć następujące elementy podrzędne:

-   ComplexType (zero lub więcej)
-   Element scalarproperty (zero lub więcej)
-   Warunek (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **element mappingfragment** .

| Nazwa atrybutu          | Jest wymagana | Wartość                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Yes         | Nazwa mapowanej tabeli lub widoku.                                                                                                                                                                           |
| **Atrybutem makecolumnsdistinct** | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy zwracane są tylko unikatowe wiersze. <br/> Jeśli ten atrybut ma **wartość true**, atrybut **GenerateUpdateViews** elementu EntityContainerMapping musi być ustawiony na **wartość false**. |

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **element mappingfragment** jako obiekt podrzędny elementu **EntityTypeMapping** . W tym przykładzie właściwości typu **kursu** w modelu koncepcyjnym są mapowane na kolumny tabeli **kursów** w bazie danych.

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

Poniższy przykład przedstawia element **element mappingfragment** jako obiekt podrzędny elementu **elementu EntitySetMapping** . Tak jak w powyższym przykładzie, właściwości typu **kursu** w modelu koncepcyjnym są mapowane na kolumny tabeli **kursów** w bazie danych.

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

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping — element (MSL)

Element **ModificationFunctionMapping** w języku specyfikacji mapowania (MSL) mapuje funkcje INSERT, Update i DELETE klasy koncepcyjnej Entity model na procedury składowane w źródłowej bazie danych. Element **ModificationFunctionMapping** może również mapować funkcje INSERT i DELETE dla skojarzeń wiele-do-wielu w modelu koncepcyjnym do procedur składowanych w źródłowej bazie danych. Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).

> [!NOTE]
> Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.


> [!NOTE]
> Jeśli funkcje modyfikacji dla jednej jednostki w hierarchii dziedziczenia są mapowane na procedury składowane, wówczas funkcje modyfikacji dla wszystkich typów w hierarchii muszą być mapowane na procedury składowane.

Element **ModificationFunctionMapping** może być elementem podrzędnym elementu EntityTypeMapping lub AssociationSetMapping.

Element **ModificationFunctionMapping** może mieć następujące elementy podrzędne:

-   DeleteFunction (zero lub jeden)
-   InsertFunction (zero lub jeden)
-   UpdateFunction (zero lub jeden)

Żadne atrybuty nie mają zastosowania do elementu **ModificationFunctionMapping** .

### <a name="example"></a>Przykład

Poniższy przykład przedstawia mapowanie zestawu jednostek dla obiektu **osoby** w modelu szkoły. Oprócz mapowania kolumn dla typu podmiotu **osoby** są wyświetlane mapowania funkcji INSERT, Update i DELETE typu **osoba** . Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.

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

Poniższy przykład przedstawia mapowanie zestawu skojarzeń dla zestawu skojarzenia **CourseInstructor** w modelu szkoły. Oprócz mapowania kolumn dla skojarzenia **CourseInstructor** są wyświetlane mapowanie funkcji INSERT i DELETE skojarzenia **CourseInstructor** . Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.

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
 

 

## <a name="queryview-element-msl"></a>QueryView — element (MSL)

Element **QueryView** w języku specyfikacji mapowania (MSL) definiuje mapowanie tylko do odczytu między typem jednostki lub skojarzeniem w modelu koncepcyjnym i tabelą w źródłowej bazie danych. Mapowanie jest zdefiniowane za pomocą zapytania Entity SQL, które jest oceniane względem modelu magazynu, i można wyrazić zestaw wyników jako jednostkę lub skojarzenie w modelu koncepcyjnym. Ponieważ widoki zapytań są tylko do odczytu, nie można używać standardowych poleceń aktualizacji do aktualizowania typów, które są zdefiniowane przez widoki zapytań. Można wprowadzać aktualizacje tych typów przy użyciu funkcji modyfikacji. Aby uzyskać więcej informacji, zobacz How to: map modyfikujących Functions to procedur składowanych.

> [!NOTE]
> W elemencie **QueryView** wyrażenia Entity SQL, które zawierają właściwości **GroupBy**, Group Aggregates lub nawigacji, nie są obsługiwane.

 

Element **QueryView** może być elementem podrzędnym elementu elementu EntitySetMapping lub AssociationSetMapping. W poprzednim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla jednostki w modelu koncepcyjnym. W tym drugim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla skojarzenia w modelu koncepcyjnym.

> [!NOTE]
> Jeśli element **AssociationSetMapping** jest dla skojarzenia z ograniczeniem referencyjnym, element **AssociationSetMapping** jest ignorowany. Aby uzyskać więcej informacji, zobacz ReferentialConstraint element (CSDL).

Element **QueryView** nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **QueryView** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Nazwa**   | Nie          | Nazwa typu modelu koncepcyjnego, który jest mapowany przez widok zapytania. |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **QueryView** jako obiekt podrzędny elementu **elementu EntitySetMapping** i definiuje mapowanie widoku zapytania dla typu jednostki **działu** w modelu szkoły.

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

Ponieważ zapytanie zwraca tylko podzestaw elementów członkowskich typu **działu** w modelu magazynu, typ **działu** w modelu szkoły został zmodyfikowany na podstawie tego mapowania w następujący sposób:

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

W następnym przykładzie pokazano element **QueryView** jako elementu podrzędnego elementu **AssociationSetMapping** i definiuje mapowanie tylko do odczytu dla skojarzenia `FK_Course_Department` w modelu szkoły.

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

Widoki zapytań można definiować w celu włączenia następujących scenariuszy:

-   Zdefiniuj jednostkę w modelu koncepcyjnym, która nie zawiera wszystkich właściwości jednostki w modelu magazynu. Obejmuje to właściwości, które nie mają wartości domyślnych i nie obsługują wartości **null** .
-   Mapuj kolumny obliczane w modelu magazynu na właściwości typów jednostek w modelu koncepcyjnym.
-   Zdefiniuj mapowanie, gdzie warunki używane do partycjonowania jednostek w modelu koncepcyjnym nie są oparte na równości. Po określeniu mapowania warunkowego przy użyciu elementu **Condition** , dostarczony warunek musi być równy podanej wartości. Aby uzyskać więcej informacji, zobacz warunek element (MSL).
-   Mapuj tę samą kolumnę w modelu magazynu na wiele typów w modelu koncepcyjnym.
-   Mapuj wiele typów do tej samej tabeli.
-   Zdefiniuj skojarzenia w modelu koncepcyjnym, które nie są oparte na kluczach obcych w schemacie relacyjnym.
-   Użyj niestandardowej logiki biznesowej, aby ustawić wartość właściwości w modelu koncepcyjnym. Na przykład można zamapować wartość ciągu "T" w źródle danych na wartość **true**, a Boolean w modelu koncepcyjnym.
-   Zdefiniuj filtry warunkowe dla wyników zapytania.
-   Wymuś mniejszą liczbę ograniczeń dotyczących danych w modelu koncepcyjnym niż w modelu magazynu. Na przykład można wprowadzić właściwość w modelu koncepcyjnym nullable, nawet jeśli kolumna, do której jest zamapowany, nie obsługuje wartości **null**.

Podczas definiowania widoków zapytania dla jednostek należy wziąć pod uwagę następujące zagadnienia:

-   Widoki zapytań są tylko do odczytu. Aktualizacje jednostek można wprowadzać tylko przy użyciu funkcji modyfikacji.
-   Podczas definiowania typu jednostki za pomocą widoku zapytania, należy również zdefiniować wszystkie powiązane jednostki według widoków zapytań.
-   Podczas mapowania skojarzenia wiele-do-wielu do jednostki w modelu magazynu, która reprezentuje tabelę łączy w schemacie relacyjnym, należy zdefiniować element **QueryView** w elemencie **AssociationSetMapping** dla tej tabeli łączy.
-   Widoki zapytań muszą być zdefiniowane dla wszystkich typów w hierarchii typów. Można to zrobić w następujący sposób:
-   -   Za pomocą pojedynczego elementu **QueryView** , który określa pojedyncze zapytanie Entity SQL zwracające Unię wszystkich typów jednostek w hierarchii.
    -   Za pomocą pojedynczego elementu **QueryView** , który określa pojedyncze zapytanie Entity SQL, które używa operatora Case do zwrócenia określonego typu jednostki w hierarchii na podstawie określonego warunku.
    -   Z dodatkowym elementem **QueryView** dla określonego typu w hierarchii. W takim przypadku należy użyć atrybutu **TypeName** elementu **QueryView** , aby określić typ jednostki dla każdego widoku.
-   Gdy jest zdefiniowany widok zapytania, nie można określić atrybutu **StorageSetName** w elemencie **elementu EntitySetMapping** .
-   Gdy jest zdefiniowany widok zapytania, element **elementu EntitySetMapping**nie może również zawierać mapowań **Właściwości** .

## <a name="resultbinding-element-msl"></a>Resultbinding — element (MSL)

Element **resultbinding** w mapowaniu języka specyfikacji (MSL) mapuje wartości kolumn, które są zwracane przez procedury składowane do właściwości jednostki w modelu koncepcyjnym, gdy funkcje modyfikacji typu jednostki są mapowane na procedury składowane w źródłowej bazie danych. **Na przykład** , gdy wartość kolumny tożsamości jest zwracana przez procedurę składowaną INSERT, elementbinding mapuje zwracaną wartość na właściwość typu jednostki w modelu koncepcyjnym.

Element **resultbinding** może być elementem podrzędnym elementu InsertFunction lub UpdateFunction.

Element **resultbinding** nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **resultbinding** :

| Nazwa atrybutu | Jest wymagana | Wartość                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Nazwa**       | Yes         | Nazwa właściwości Entity w modelu koncepcyjnym, który jest mapowany. |
| **ColumnName** | Yes         | Nazwa mapowanej kolumny.                                          |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i zawiera element **InsertFunction** służący do mapowania funkcji INSERT typu podmiotu **osoby** do procedury składowanej **InsertPerson** . (Procedura składowana **InsertPerson** jest pokazana poniżej i jest zadeklarowana w modelu magazynu). Element **resultbinding** jest używany do mapowania wartości kolumny, która jest zwracana przez procedurę składowaną (**NewPersonID**) do właściwości typu jednostki (**PersonID**).

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

Następujące instrukcje języka Transact-SQL opisują procedurę składowaną **InsertPerson** :

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

## <a name="resultmapping-element-msl"></a>ResultMapping — element (MSL)

Element **ResultMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między importem funkcji w modelu koncepcyjnym i procedurą składowaną w źródłowej bazie danych, gdy są spełnione następujące warunki:

-   Import funkcji zwraca typ jednostki koncepcyjnej lub typ złożony.
-   Nazwy kolumn zwracanych przez procedurę składowaną nie są dokładnie zgodne z nazwami właściwości typu jednostki lub typu złożonego.

Domyślnie mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki lub typem złożonym jest oparte na nazwach kolumn i właściwości. Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, należy użyć elementu **ResultMapping** , aby zdefiniować mapowanie. Przykład mapowania domyślnego można znaleźć w temacie FunctionImportMapping element (MSL).

Element **ResultMapping** jest elementem podrzędnym elementu FunctionImportMapping.

Element **ResultMapping** może mieć następujące elementy podrzędne:

-   EntityTypeMapping (zero lub więcej)
-   ComplexTypeMapping

Żadne atrybuty nie mają zastosowania do elementu **ResultMapping** .

### <a name="example"></a>Przykład

Weź pod uwagę następujące procedury składowane:

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

Należy również wziąć pod uwagę następujący typ jednostki koncepcyjnej modelu:

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

Aby można było utworzyć import funkcji, który zwraca wystąpienia poprzedniego typu jednostki, mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki musi być zdefiniowane w elemencie **ResultMapping** :

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

## <a name="scalarproperty-element-msl"></a>Element scalarproperty — element (MSL)

Element **element scalarproperty** w języku specyfikacji mapowania (MSL) mapuje właściwość na typ jednostki modelu koncepcyjnego, typ złożony lub skojarzenie do kolumny tabeli lub parametru procedury składowanej w źródłowej bazie danych.

> [!NOTE]
> Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).

Element **element scalarproperty** może być elementem podrzędnym następujących elementów:

-   Element mappingfragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Jako element podrzędny elementu **element mappingfragment**, **ComplexProperty**lub **EndProperty** element **element scalarproperty** mapuje właściwość w modelu koncepcyjnym do kolumny w bazie danych. Jako element podrzędny elementu **InsertFunction**, **UpdateFunction**lub **DeleteFunction** element **element scalarproperty** mapuje właściwość w modelu koncepcyjnym do parametru procedury składowanej.

Element **element scalarproperty** nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Atrybuty, które mają zastosowanie do elementu **element scalarproperty** , różnią się w zależności od roli elementu.

W poniższej tabeli opisano atrybuty, które są stosowane, gdy element **element scalarproperty** jest używany do mapowania właściwości modelu koncepcyjnego na kolumnę w bazie danych:

| Nazwa atrybutu | Jest wymagana | Wartość                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nazwa**       | Yes         | Nazwa mapowanej właściwości modelu koncepcyjnego. |
| **ColumnName** | Yes         | Nazwa mapowanej kolumny tabeli.              |

W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **element scalarproperty** , gdy jest on używany do mapowania właściwości modelu koncepcyjnego na parametr procedury składowanej:

| Nazwa atrybutu    | Jest wymagana | Wartość                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**          | Yes         | Nazwa mapowanej właściwości modelu koncepcyjnego.                                                                                 |
| **ParameterName** | Yes         | Nazwa mapowanego parametru.                                                                                                 |
| **Wersja**       | Nie          | **Bieżący** lub **oryginalny** w zależności od tego, czy bieżąca wartość lub oryginalna wartość właściwości ma być używana do sprawdzania współbieżności. |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **element scalarproperty** używany na dwa sposoby:

-   Aby zmapować właściwości typu podmiotu **osoby** do kolumn tabeli **Person**.
-   Aby zmapować właściwości typu podmiotu **osoby** do parametrów procedury składowanej **UpdatePerson** . Procedury składowane są zadeklarowane w modelu magazynu.

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

W następnym przykładzie pokazano element **element scalarproperty** używany do mapowania funkcji INSERT i DELETE skojarzenia modelu koncepcyjnego na procedury składowane w bazie danych. Procedury składowane są zadeklarowane w modelu magazynu.

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

## <a name="updatefunction-element-msl"></a>UpdateFunction — element (MSL)

Element **UpdateFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję Update typu jednostki w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych. Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu. Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).

> [!NOTE]
>  Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.

Element **UpdateFunction** może być elementem podrzędnym elementu ModificationFunctionMapping i stosowany do elementu EntityTypeMapping.

Element **UpdateFunction** może mieć następujące elementy podrzędne:

-   AssociationEnd (zero lub więcej)
-   ComplexProperty (zero lub więcej)
-   Resultbinding (zero lub jeden)
-   ScarlarProperty (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **UpdateFunction** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Yes         | Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest zamapowana funkcja aktualizacji. Procedura składowana musi być zadeklarowana w modelu magazynu. |
| **RowsAffectedParameter** | Nie          | Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.                                                                               |

### <a name="example"></a>Przykład

Poniższy przykład jest oparty na modelu szkoły i pokazuje element **UpdateFunction** używany do mapowania funkcji Update typu jednostki **osoby** na procedurę składowaną **UpdatePerson** . Procedura składowana **UpdatePerson** jest zadeklarowana w modelu magazynu.

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
