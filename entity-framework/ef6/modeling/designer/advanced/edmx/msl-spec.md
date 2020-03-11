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
# <a name="msl-specification"></a><span data-ttu-id="69393-102">Specyfikacja MSL</span><span class="sxs-lookup"><span data-stu-id="69393-102">MSL Specification</span></span>
<span data-ttu-id="69393-103">Mapowanie specyfikacji języka (MSL) to język oparty na języku XML, który opisuje mapowanie między modelem koncepcyjnym i modelem magazynu aplikacji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="69393-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="69393-104">W aplikacji Entity Framework mapowanie metadanych jest ładowane z pliku MSL (zapisywane w bibliotece MSL) w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="69393-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="69393-105">Entity Framework używa mapowania metadanych w czasie wykonywania do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="69393-106">Entity Framework Designer (programu EF Designer) przechowuje informacje o mapowaniu w pliku edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="69393-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="69393-107">W czasie kompilacji Entity Designer używa informacji w pliku. edmx, aby utworzyć plik. MSL, który jest wymagany przez Entity Framework w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="69393-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="69393-108">Nazwy wszystkich typów modelu koncepcyjnego lub magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według odpowiednich nazw przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="69393-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="69393-109">Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="69393-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="69393-110">Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="69393-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="69393-111">Wersje pliku MSL są odróżniane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="69393-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="69393-112">Wersja pliku MSL</span><span class="sxs-lookup"><span data-stu-id="69393-112">MSL Version</span></span> | <span data-ttu-id="69393-113">Przestrzeń nazw XML</span><span class="sxs-lookup"><span data-stu-id="69393-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="69393-114">MSL, wersja 1</span><span class="sxs-lookup"><span data-stu-id="69393-114">MSL v1</span></span>      | <span data-ttu-id="69393-115">urn: schematys-Microsoft-com: Windows: Storage: Mapping: CS</span><span class="sxs-lookup"><span data-stu-id="69393-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="69393-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="69393-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="69393-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="69393-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="69393-118">Element aliasu (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-118">Alias Element (MSL)</span></span>

<span data-ttu-id="69393-119">Element **aliasu** w języku specyfikacji mapowania (MSL) jest elementem podrzędnym elementu Mapping, który jest używany do definiowania aliasów dla modelu koncepcyjnego i przestrzeni nazw modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="69393-120">Nazwy wszystkich typów modelu koncepcyjnego lub magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według odpowiednich nazw przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="69393-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="69393-121">Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz element Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="69393-122">Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz element Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="69393-123">Element **aliasu** nie może mieć elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="69393-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-124">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-124">Applicable Attributes</span></span>

<span data-ttu-id="69393-125">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **alias** .</span><span class="sxs-lookup"><span data-stu-id="69393-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="69393-126">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-126">Attribute Name</span></span> | <span data-ttu-id="69393-127">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-127">Is Required</span></span> | <span data-ttu-id="69393-128">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="69393-129">**Klucz**</span><span class="sxs-lookup"><span data-stu-id="69393-129">**Key**</span></span>        | <span data-ttu-id="69393-130">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-130">Yes</span></span>         | <span data-ttu-id="69393-131">Alias dla przestrzeni nazw, który jest określony przez atrybut **Value** .</span><span class="sxs-lookup"><span data-stu-id="69393-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="69393-132">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="69393-132">**Value**</span></span>      | <span data-ttu-id="69393-133">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-133">Yes</span></span>         | <span data-ttu-id="69393-134">Przestrzeń nazw, dla której wartość elementu **Key** jest aliasem.</span><span class="sxs-lookup"><span data-stu-id="69393-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="69393-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-135">Example</span></span>

<span data-ttu-id="69393-136">W poniższym przykładzie pokazano element **aliasu** , który definiuje alias, `c`dla typów, które są zdefiniowane w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="69393-137">AssociationEnd — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="69393-138">Element **AssociationEnd** w języku specyfikacji mapowania (MSL) jest używany, gdy funkcje modyfikacji typu jednostki w modelu koncepcyjnym są mapowane na procedury składowane w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="69393-139">Jeśli procedura składowana modyfikacji przyjmuje parametr, którego wartość jest przechowywana we właściwości skojarzenia, element **AssociationEnd** mapuje wartość właściwości na parametr.</span><span class="sxs-lookup"><span data-stu-id="69393-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="69393-140">Aby uzyskać więcej informacji, zobacz Poniższy przykład.</span><span class="sxs-lookup"><span data-stu-id="69393-140">For more information, see the example below.</span></span>

<span data-ttu-id="69393-141">Aby uzyskać więcej informacji na temat mapowania funkcji modyfikacji typów jednostek do procedur składowanych, zobacz ModificationFunctionMapping element (MSL) i przewodnik: mapowanie jednostki na procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="69393-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="69393-142">Element **AssociationEnd** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-143">Element scalarproperty</span><span class="sxs-lookup"><span data-stu-id="69393-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-144">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-144">Applicable Attributes</span></span>

<span data-ttu-id="69393-145">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="69393-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="69393-146">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-146">Attribute Name</span></span>     | <span data-ttu-id="69393-147">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-147">Is Required</span></span> | <span data-ttu-id="69393-148">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="69393-149">**AssociationSet**</span></span> | <span data-ttu-id="69393-150">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-150">Yes</span></span>         | <span data-ttu-id="69393-151">Nazwa mapowanego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="69393-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="69393-152">**Wniosek**</span><span class="sxs-lookup"><span data-stu-id="69393-152">**From**</span></span>           | <span data-ttu-id="69393-153">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-153">Yes</span></span>         | <span data-ttu-id="69393-154">Wartość atrybutu **FromRole** właściwości nawigacji, która odnosi się do mapowanego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="69393-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="69393-155">Aby uzyskać więcej informacji, zobacz element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="69393-156">**Do**</span><span class="sxs-lookup"><span data-stu-id="69393-156">**To**</span></span>             | <span data-ttu-id="69393-157">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-157">Yes</span></span>         | <span data-ttu-id="69393-158">Wartość atrybutu **ToRole** właściwości nawigacji, która odnosi się do mapowanego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="69393-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="69393-159">Aby uzyskać więcej informacji, zobacz element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="69393-160">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-160">Example</span></span>

<span data-ttu-id="69393-161">Rozważmy następujący typ jednostki koncepcyjnej:</span><span class="sxs-lookup"><span data-stu-id="69393-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="69393-162">Należy również wziąć pod uwagę następujące procedury składowane:</span><span class="sxs-lookup"><span data-stu-id="69393-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="69393-163">Aby zmapować funkcję Update jednostki `Course` na tę procedurę składowaną, należy podać wartość parametru **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="69393-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="69393-164">Wartość `DepartmentID` nie odpowiada właściwości typu jednostki; jest on zawarty w niezależnym skojarzeniu, którego mapowanie jest pokazane tutaj:</span><span class="sxs-lookup"><span data-stu-id="69393-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="69393-165">Poniższy kod przedstawia element **AssociationEnd** , który służy do mapowania właściwości **DepartmentID** **klucza obcego\_go skojarzenia działu\_go** w ramach procedury składowanej **UpdateCourse** (do której jest zamapowana funkcja Update typu jednostki **kursu** ):</span><span class="sxs-lookup"><span data-stu-id="69393-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="69393-166">AssociationSetMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="69393-167">Element **AssociationSetMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między skojarzeniem w modelu koncepcyjnym i kolumnami tabeli w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="69393-168">Skojarzenia w modelu koncepcyjnym są typami, których właściwości reprezentują kolumny klucza podstawowego i obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="69393-169">Element **AssociationSetMapping** używa dwóch elementów EndProperty do definiowania mapowań między właściwościami typu skojarzenia a kolumnami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="69393-170">Warunki dotyczące tych mapowań można umieścić przy użyciu elementu Condition.</span><span class="sxs-lookup"><span data-stu-id="69393-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="69393-171">Mapuj funkcje INSERT, Update i DELETE dla skojarzeń z procedurami przechowywanymi w bazie danych z elementem ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="69393-172">Zdefiniuj mapowania tylko do odczytu między skojarzeniami a kolumnami tabeli przy użyciu ciągu Entity SQL w elemencie QueryView.</span><span class="sxs-lookup"><span data-stu-id="69393-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="69393-173">Jeśli ograniczenie referencyjne jest zdefiniowane dla skojarzenia w modelu koncepcyjnym, skojarzenie nie musi być mapowane przy użyciu elementu **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="69393-174">Jeśli element **AssociationSetMapping** jest obecny dla skojarzenia, które ma ograniczenie referencyjne, mapowania zdefiniowane w elemencie **AssociationSetMapping** zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="69393-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="69393-175">Aby uzyskać więcej informacji, zobacz ReferentialConstraint element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="69393-176">Element **AssociationSetMapping** może mieć następujące elementy podrzędne</span><span class="sxs-lookup"><span data-stu-id="69393-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="69393-177">QueryView (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="69393-178">EndProperty (zero lub dwa)</span><span class="sxs-lookup"><span data-stu-id="69393-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="69393-179">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="69393-180">ModificationFunctionMapping (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-181">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-181">Applicable Attributes</span></span>

<span data-ttu-id="69393-182">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="69393-183">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-183">Attribute Name</span></span>     | <span data-ttu-id="69393-184">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-184">Is Required</span></span> | <span data-ttu-id="69393-185">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-186">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-186">**Name**</span></span>           | <span data-ttu-id="69393-187">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-187">Yes</span></span>         | <span data-ttu-id="69393-188">Nazwa mapowanego zestawu skojarzeń modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="69393-189">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-189">**TypeName**</span></span>       | <span data-ttu-id="69393-190">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-190">No</span></span>          | <span data-ttu-id="69393-191">Kwalifikowana nazwa przestrzeni nazw typu powiązania modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="69393-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="69393-192">**StoreEntitySet**</span></span> | <span data-ttu-id="69393-193">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-193">No</span></span>          | <span data-ttu-id="69393-194">Nazwa mapowanej tabeli.</span><span class="sxs-lookup"><span data-stu-id="69393-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="69393-195">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-195">Example</span></span>

<span data-ttu-id="69393-196">W poniższym przykładzie pokazano element **AssociationSetMapping** , w którym w modelu koncepcyjnym zostanie zamapowany skojarzenie " **\_y działu\_kursu** , który jest mapowany do tabeli **kursów** w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="69393-197">Mapowania między właściwościami typu skojarzenia i kolumnami tabeli są określone w podrzędnych elementach **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="69393-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="69393-198">ComplexProperty — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="69393-199">Element **ComplexProperty** w języku specyfikacji mapowania (MSL) definiuje mapowanie między właściwością typu złożonego w typie jednostki modelu koncepcyjnego a kolumnami tabeli w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="69393-200">Mapowania kolumn właściwości są określone w podrzędnych elementach element scalarproperty.</span><span class="sxs-lookup"><span data-stu-id="69393-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="69393-201">Element właściwości **complexType** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-202">Element scalarproperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="69393-203">**ComplexProperty** (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="69393-204">ComplextTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="69393-205">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-206">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-206">Applicable Attributes</span></span>

<span data-ttu-id="69393-207">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="69393-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="69393-208">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-208">Attribute Name</span></span> | <span data-ttu-id="69393-209">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-209">Is Required</span></span> | <span data-ttu-id="69393-210">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-211">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-211">**Name**</span></span>       | <span data-ttu-id="69393-212">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-212">Yes</span></span>         | <span data-ttu-id="69393-213">Nazwa właściwości złożonej typu jednostki w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="69393-214">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-214">**TypeName**</span></span>   | <span data-ttu-id="69393-215">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-215">No</span></span>          | <span data-ttu-id="69393-216">Kwalifikowana nazwa obszaru nazw typu właściwości modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="69393-217">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-217">Example</span></span>

<span data-ttu-id="69393-218">Poniższy przykład jest oparty na modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-218">The following example is based on the School model.</span></span> <span data-ttu-id="69393-219">Następujący typ złożony został dodany do modelu koncepcyjnego:</span><span class="sxs-lookup"><span data-stu-id="69393-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="69393-220">Właściwości **LastName** i **FirstName** typu podmiotu **osoby** zostały zastąpione jedną złożoną właściwością. **Nazwa**:</span><span class="sxs-lookup"><span data-stu-id="69393-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="69393-221">W poniższym pliku MSL przedstawiono element **ComplexProperty** używany do mapowania właściwości **name** na kolumny w źródłowej bazie danych:</span><span class="sxs-lookup"><span data-stu-id="69393-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="69393-222">ComplexTypeMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="69393-223">Element **ComplexTypeMapping** w języku specyfikacji mapowania (MSL) jest elementem podrzędnym elementu ResultMapping i definiuje mapowanie między importem funkcji w modelu koncepcyjnym i procedurą przechowywaną w źródłowej bazie danych, gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="69393-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="69393-224">Import funkcji zwraca typ złożony koncepcyjnie.</span><span class="sxs-lookup"><span data-stu-id="69393-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="69393-225">Nazwy kolumn zwracanych przez procedurę składowaną nie są dokładnie zgodne z nazwami właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="69393-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="69393-226">Domyślnie mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem złożonym opiera się na nazwach kolumn i właściwości.</span><span class="sxs-lookup"><span data-stu-id="69393-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="69393-227">Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, należy użyć elementu **ComplexTypeMapping** , aby zdefiniować mapowanie.</span><span class="sxs-lookup"><span data-stu-id="69393-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="69393-228">Przykład mapowania domyślnego można znaleźć w temacie FunctionImportMapping element (MSL).</span><span class="sxs-lookup"><span data-stu-id="69393-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="69393-229">Element **ComplexTypeMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-230">Element scalarproperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-231">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-231">Applicable Attributes</span></span>

<span data-ttu-id="69393-232">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="69393-233">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-233">Attribute Name</span></span> | <span data-ttu-id="69393-234">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-234">Is Required</span></span> | <span data-ttu-id="69393-235">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="69393-236">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-236">**TypeName**</span></span>   | <span data-ttu-id="69393-237">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-237">Yes</span></span>         | <span data-ttu-id="69393-238">Kwalifikowana nazwa przestrzeni nazw typu złożonego, który jest zamapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-239">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-239">Example</span></span>

<span data-ttu-id="69393-240">Weź pod uwagę następujące procedury składowane:</span><span class="sxs-lookup"><span data-stu-id="69393-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="69393-241">Należy również wziąć pod uwagę następujący typ złożony modelu koncepcyjnego:</span><span class="sxs-lookup"><span data-stu-id="69393-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="69393-242">Aby można było utworzyć import funkcji, który zwraca wystąpienia poprzedniego typu złożonego, mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki musi być zdefiniowane w elemencie **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="69393-243">Condition — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-243">Condition Element (MSL)</span></span>

<span data-ttu-id="69393-244">Element **Condition** języka specyfikacji mapowania (MSL) umieszcza warunki mapowania między modelem koncepcyjnym a podstawową bazą danych.</span><span class="sxs-lookup"><span data-stu-id="69393-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="69393-245">Mapowanie zdefiniowane w węźle XML jest prawidłowe, jeśli są spełnione wszystkie warunki określone w elementach **warunku** podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="69393-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="69393-246">W przeciwnym razie mapowanie jest nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="69393-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="69393-247">Na przykład, jeśli element element mappingfragment zawiera jeden lub więcej elementów podrzędnych **warunku** , Mapowanie zdefiniowane w węźle **element mappingfragment** będzie prawidłowe tylko wtedy, gdy spełnione są wszystkie **warunki elementów podrzędnych warunków.**</span><span class="sxs-lookup"><span data-stu-id="69393-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="69393-248">Każdy warunek może być stosowany do **nazwy** (nazwy właściwości jednostki koncepcyjnej, określonej przez atrybut **nazwy** ) lub do **ColumnName** (nazwa kolumny w bazie danych, określona przez atrybut **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="69393-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="69393-249">Gdy atrybut **name** jest ustawiony, warunek jest sprawdzany względem wartości właściwości Entity.</span><span class="sxs-lookup"><span data-stu-id="69393-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="69393-250">Gdy atrybut **ColumnName** jest ustawiony, warunek jest sprawdzany względem wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="69393-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="69393-251">Tylko jeden z atrybutów **name** lub **ColumnName** może być określony w elemencie **Condition** .</span><span class="sxs-lookup"><span data-stu-id="69393-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="69393-252">Gdy element **Condition** jest używany w elemencie FunctionImportMapping, tylko atrybut **name** nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="69393-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="69393-253">**Warunek Condition** może być elementem podrzędnym następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="69393-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="69393-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="69393-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="69393-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="69393-255">ComplexProperty</span></span>
-   <span data-ttu-id="69393-256">Elementu EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="69393-256">EntitySetMapping</span></span>
-   <span data-ttu-id="69393-257">Element mappingfragment</span><span class="sxs-lookup"><span data-stu-id="69393-257">MappingFragment</span></span>
-   <span data-ttu-id="69393-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="69393-258">EntityTypeMapping</span></span>

<span data-ttu-id="69393-259">Element **Condition** nie może mieć elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="69393-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-260">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-260">Applicable Attributes</span></span>

<span data-ttu-id="69393-261">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **Condition** :</span><span class="sxs-lookup"><span data-stu-id="69393-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="69393-262">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-262">Attribute Name</span></span> | <span data-ttu-id="69393-263">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-263">Is Required</span></span> | <span data-ttu-id="69393-264">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="69393-265">**ColumnName**</span></span> | <span data-ttu-id="69393-266">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-266">No</span></span>          | <span data-ttu-id="69393-267">Nazwa kolumny tabeli, której wartość jest używana do obliczania warunku.</span><span class="sxs-lookup"><span data-stu-id="69393-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="69393-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="69393-268">**IsNull**</span></span>     | <span data-ttu-id="69393-269">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-269">No</span></span>          | <span data-ttu-id="69393-270">**True** lub **False**.</span><span class="sxs-lookup"><span data-stu-id="69393-270">**True** or **False**.</span></span> <span data-ttu-id="69393-271">Jeśli wartość jest **równa true** , a wartość kolumny ma wartość **null**lub jeśli wartość jest równa **false** , a wartość kolumny nie jest **równa null**, warunek ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="69393-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="69393-272">W przeciwnym razie warunek ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="69393-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="69393-273">Nie można jednocześnie używać atrybutów **IsNull** i **Value** .</span><span class="sxs-lookup"><span data-stu-id="69393-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="69393-274">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="69393-274">**Value**</span></span>      | <span data-ttu-id="69393-275">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-275">No</span></span>          | <span data-ttu-id="69393-276">Wartość, z którą jest porównywana wartość kolumny.</span><span class="sxs-lookup"><span data-stu-id="69393-276">The value with which the column value is compared.</span></span> <span data-ttu-id="69393-277">Jeśli wartości są takie same, warunek ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="69393-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="69393-278">W przeciwnym razie warunek ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="69393-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="69393-279">Nie można jednocześnie używać atrybutów **IsNull** i **Value** .</span><span class="sxs-lookup"><span data-stu-id="69393-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="69393-280">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-280">**Name**</span></span>       | <span data-ttu-id="69393-281">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-281">No</span></span>          | <span data-ttu-id="69393-282">Nazwa właściwości jednostki modelu koncepcyjnego, której wartość jest używana do obliczania warunku.</span><span class="sxs-lookup"><span data-stu-id="69393-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="69393-283">Ten atrybut nie ma zastosowania, jeśli element **Condition** jest używany w elemencie FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="69393-284">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-284">Example</span></span>

<span data-ttu-id="69393-285">Poniższy przykład pokazuje elementy **Condition** jako elementy podrzędne elementów **element mappingfragment** .</span><span class="sxs-lookup"><span data-stu-id="69393-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="69393-286">Gdy **HireDate** nie ma wartości null **i EnrollmentDate** ma wartość null, dane są mapowane **między SchoolModel. typ instruktora** i kolumny **PersonID** i **HireDate** tabeli **Person** .</span><span class="sxs-lookup"><span data-stu-id="69393-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="69393-287">Gdy **EnrollmentDate** nie ma wartości null i **HireDate** ma wartość null, dane są mapowane **między SchoolModel. studenta** i kolumny **PersonID** i **Enrollment** w tabeli **Person** .</span><span class="sxs-lookup"><span data-stu-id="69393-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="69393-288">DeleteFunction — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="69393-289">Element **DeleteFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję delete typu jednostki lub skojarzenia w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="69393-290">Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="69393-291">Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="69393-292">Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.</span><span class="sxs-lookup"><span data-stu-id="69393-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="69393-293">DeleteFunction zastosowana do EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="69393-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="69393-294">W przypadku zastosowania do elementu EntityTypeMapping element **DeleteFunction** mapuje funkcję delete typu jednostki w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="69393-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="69393-295">Element **DeleteFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="69393-296">AssociationEnd (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="69393-297">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="69393-298">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69393-299">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-299">Applicable Attributes</span></span>

<span data-ttu-id="69393-300">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **DeleteFunction** , gdy jest on stosowany do elementu **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="69393-301">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-301">Attribute Name</span></span>            | <span data-ttu-id="69393-302">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-302">Is Required</span></span> | <span data-ttu-id="69393-303">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-304">**FunctionName**</span></span>          | <span data-ttu-id="69393-305">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-305">Yes</span></span>         | <span data-ttu-id="69393-306">Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja Delete.</span><span class="sxs-lookup"><span data-stu-id="69393-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="69393-307">Procedura składowana musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="69393-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="69393-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="69393-309">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-309">No</span></span>          | <span data-ttu-id="69393-310">Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="69393-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="69393-311">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-311">Example</span></span>

<span data-ttu-id="69393-312">Poniższy przykład jest oparty na modelu szkoły i pokazuje, że element **DeleteFunction** jest mapowany do procedury składowanej **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="69393-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="69393-313">Procedura składowana **DeletePerson** jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="69393-314">DeleteFunction zastosowana do AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="69393-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="69393-315">Po zastosowaniu do elementu AssociationSetMapping element **DeleteFunction** mapuje funkcję delete skojarzenia w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="69393-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="69393-316">Element **DeleteFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="69393-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="69393-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69393-318">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-318">Applicable Attributes</span></span>

<span data-ttu-id="69393-319">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **DeleteFunction** , gdy jest on stosowany do elementu **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="69393-320">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-320">Attribute Name</span></span>            | <span data-ttu-id="69393-321">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-321">Is Required</span></span> | <span data-ttu-id="69393-322">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-323">**FunctionName**</span></span>          | <span data-ttu-id="69393-324">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-324">Yes</span></span>         | <span data-ttu-id="69393-325">Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja Delete.</span><span class="sxs-lookup"><span data-stu-id="69393-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="69393-326">Procedura składowana musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="69393-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="69393-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="69393-328">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-328">No</span></span>          | <span data-ttu-id="69393-329">Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="69393-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="69393-330">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-330">Example</span></span>

<span data-ttu-id="69393-331">Poniższy przykład jest oparty na modelu szkoły i pokazuje element **DeleteFunction** używany do mapowania funkcji Delete skojarzenia **CourseInstructor** do procedury składowanej **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="69393-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="69393-332">Procedura składowana **DeleteCourseInstructor** jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="69393-333">EndProperty — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="69393-334">Element **EndProperty** w języku specyfikacji mapowania (MSL) definiuje mapowanie między końcem lub funkcją modyfikacji skojarzenia modelu koncepcyjnego a podstawową bazą danych.</span><span class="sxs-lookup"><span data-stu-id="69393-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="69393-335">Mapowanie kolumny właściwości jest określone w podrzędnym elemencie element scalarproperty.</span><span class="sxs-lookup"><span data-stu-id="69393-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="69393-336">Gdy element **EndProperty** jest używany do definiowania mapowania dla końca skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="69393-337">Gdy element **EndProperty** jest używany do definiowania mapowania dla funkcji modyfikacji skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu InsertFunction lub DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="69393-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="69393-338">Element **EndProperty** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-339">Element scalarproperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-340">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-340">Applicable Attributes</span></span>

<span data-ttu-id="69393-341">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="69393-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="69393-342">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-342">Attribute Name</span></span> | <span data-ttu-id="69393-343">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-343">Is Required</span></span> | <span data-ttu-id="69393-344">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="69393-345">Name (Nazwa)</span><span class="sxs-lookup"><span data-stu-id="69393-345">Name</span></span>           | <span data-ttu-id="69393-346">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-346">Yes</span></span>         | <span data-ttu-id="69393-347">Nazwa elementu end skojarzenia, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-348">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-348">Example</span></span>

<span data-ttu-id="69393-349">W poniższym przykładzie przedstawiono element **AssociationSetMapping** , w\_którym skojarzenie " **\_** " w modelu koncepcyjnym jest zamapowane na tabelę **kursów** w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="69393-350">Mapowania między właściwościami typu skojarzenia i kolumnami tabeli są określone w podrzędnych elementach **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="69393-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-351">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-351">Example</span></span>

<span data-ttu-id="69393-352">Poniższy przykład przedstawia mapowanie elementu **EndProperty** funkcji INSERT i DELETE skojarzenia (**CourseInstructor**) na procedury składowane w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="69393-353">Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="69393-354">EntityContainerMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="69393-355">Element **EntityContainerMapping** w języku specyfikacji mapowania (MSL) mapuje kontener Entity w modelu koncepcyjnym do kontenera jednostek w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="69393-356">Element **EntityContainerMapping** jest elementem podrzędnym elementu mapowania.</span><span class="sxs-lookup"><span data-stu-id="69393-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="69393-357">Element **EntityContainerMapping** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="69393-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69393-358">Elementu EntitySetMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="69393-359">AssociationSetMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="69393-360">FunctionImportMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-361">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-361">Applicable Attributes</span></span>

<span data-ttu-id="69393-362">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="69393-363">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-363">Attribute Name</span></span>            | <span data-ttu-id="69393-364">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-364">Is Required</span></span> | <span data-ttu-id="69393-365">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="69393-366">**StorageModelContainer**</span></span> | <span data-ttu-id="69393-367">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-367">Yes</span></span>         | <span data-ttu-id="69393-368">Nazwa mapowanego kontenera jednostek modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="69393-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="69393-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="69393-370">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-370">Yes</span></span>         | <span data-ttu-id="69393-371">Nazwa mapowanego kontenera jednostek modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="69393-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="69393-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="69393-373">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-373">No</span></span>          | <span data-ttu-id="69393-374">**True** lub **False**.</span><span class="sxs-lookup"><span data-stu-id="69393-374">**True** or **False**.</span></span> <span data-ttu-id="69393-375">W przypadku **wartości false**nie są generowane żadne widoki aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="69393-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="69393-376">Ten atrybut powinien mieć ustawioną **wartość false** , jeśli masz mapowanie tylko do odczytu, które byłoby nieprawidłowe, ponieważ dane mogły się nie przewidzieć pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="69393-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="69393-377">Wartość domyślna to **True**.</span><span class="sxs-lookup"><span data-stu-id="69393-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-378">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-378">Example</span></span>

<span data-ttu-id="69393-379">W poniższym przykładzie pokazano element **EntityContainerMapping** , który mapuje kontener **SchoolModelEntities** (kontener jednostki modelu koncepcyjnego) do kontenera **SchoolModelStoreContainer** (kontener jednostek modelu magazynu):</span><span class="sxs-lookup"><span data-stu-id="69393-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="69393-380">Elementu EntitySetMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="69393-381">Element **elementu EntitySetMapping** w języku specyfikacji mapowania (MSL) mapuje wszystkie typy w jednostce modelu koncepcyjnego na zestaw jednostek w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="69393-382">Jednostka ustawiona w modelu koncepcyjnym jest kontenerem logicznym dla wystąpień jednostek tego samego typu (i typów pochodnych).</span><span class="sxs-lookup"><span data-stu-id="69393-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="69393-383">Jednostka ustawiona w modelu magazynu reprezentuje tabelę lub widok w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="69393-384">Zestaw jednostek koncepcyjnych modelu jest określany przez wartość atrybutu **name** elementu **elementu EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="69393-385">Zmapowana tabela lub widok jest określona przez atrybut **StoreEntitySet** w każdym podrzędnym elemencie element mappingfragment lub w samym elemencie **elementu EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="69393-386">Element **elementu EntitySetMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-387">EntityTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="69393-388">QueryView (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="69393-389">Element mappingfragment (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-390">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-390">Applicable Attributes</span></span>

<span data-ttu-id="69393-391">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **elementu EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="69393-392">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-392">Attribute Name</span></span>           | <span data-ttu-id="69393-393">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-393">Is Required</span></span> | <span data-ttu-id="69393-394">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-395">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-395">**Name**</span></span>                 | <span data-ttu-id="69393-396">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-396">Yes</span></span>         | <span data-ttu-id="69393-397">Nazwa mapowanego zestawu jednostek modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="69393-398">**Nazwa elementu TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="69393-398">**TypeName** **1**</span></span>       | <span data-ttu-id="69393-399">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-399">No</span></span>          | <span data-ttu-id="69393-400">Nazwa mapowanego typu jednostki modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="69393-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="69393-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="69393-402">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-402">No</span></span>          | <span data-ttu-id="69393-403">Nazwa zestawu jednostek modelu magazynu, do którego jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="69393-404">**Atrybutem makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="69393-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="69393-405">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-405">No</span></span>          | <span data-ttu-id="69393-406">**Prawda** lub **Fałsz** w zależności od tego, czy zwracane są tylko unikatowe wiersze.</span><span class="sxs-lookup"><span data-stu-id="69393-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="69393-407">Jeśli ten atrybut ma **wartość true**, atrybut **GenerateUpdateViews** elementu EntityContainerMapping musi być ustawiony na **wartość false**.</span><span class="sxs-lookup"><span data-stu-id="69393-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="69393-408">**1** atrybuty **TypeName** i **StoreEntitySet** mogą być używane zamiast elementów podrzędnych EntityTypeMapping i element mappingfragment, aby mapować pojedynczy typ jednostki na pojedynczą tabelę.</span><span class="sxs-lookup"><span data-stu-id="69393-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="69393-409">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-409">Example</span></span>

<span data-ttu-id="69393-410">W poniższym przykładzie pokazano element **elementu EntitySetMapping** , który mapuje trzy typy (typ podstawowy i dwa typy pochodne) w zestawie jednostek **kursów** modelu koncepcyjnego na trzy różne tabele w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="69393-411">Tabele są określane przez atrybut **StoreEntitySet** w każdym elemencie **element mappingfragment** .</span><span class="sxs-lookup"><span data-stu-id="69393-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="69393-412">EntityTypeMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="69393-413">Element **EntityTypeMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między typem jednostki w modelu koncepcyjnym i tabelami lub widokami w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="69393-414">Aby uzyskać informacje na temat typów jednostek modelu koncepcyjnego i tabel lub widoków bazy danych, zobacz element EntityType (CSDL) i element EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="69393-415">Mapowany typ jednostki modelu koncepcyjnego jest określany przez atrybut **TypeName** elementu **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="69393-416">Mapowana tabela lub widok jest określany przez atrybut **StoreEntitySet** podrzędnego elementu element mappingfragment.</span><span class="sxs-lookup"><span data-stu-id="69393-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="69393-417">Elementu podrzędnego ModificationFunctionMapping można użyć do mapowania funkcji INSERT, Update lub DELETE typów jednostek na procedury składowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="69393-418">Element **EntityTypeMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-419">Element mappingfragment (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="69393-420">ModificationFunctionMapping (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="69393-421">Element scalarproperty</span><span class="sxs-lookup"><span data-stu-id="69393-421">ScalarProperty</span></span>
-   <span data-ttu-id="69393-422">Warunek</span><span class="sxs-lookup"><span data-stu-id="69393-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="69393-423">Elementy **element mappingfragment** i **ModificationFunctionMapping** nie mogą być elementami podrzędnymi elementu **EntityTypeMapping** w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="69393-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="69393-424">Elementy **element scalarproperty** i **Condition** mogą być tylko elementami podrzędnymi elementu **EntityTypeMapping** , jeśli są używane w elemencie FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-425">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-425">Applicable Attributes</span></span>

<span data-ttu-id="69393-426">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="69393-427">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-427">Attribute Name</span></span> | <span data-ttu-id="69393-428">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-428">Is Required</span></span> | <span data-ttu-id="69393-429">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-430">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-430">**TypeName**</span></span>   | <span data-ttu-id="69393-431">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-431">Yes</span></span>         | <span data-ttu-id="69393-432">Kwalifikowana nazwa przestrzeni nazw typu jednostki modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="69393-433">Jeśli typ jest abstrakcyjny lub typem pochodnym, wartość musi być `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="69393-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-434">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-434">Example</span></span>

<span data-ttu-id="69393-435">Poniższy przykład pokazuje element elementu EntitySetMapping z dwoma podrzędnymi elementami **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="69393-436">W pierwszym elemencie **EntityTypeMapping** typ jednostki **SchoolModel. Person** jest mapowany do tabeli **Person** .</span><span class="sxs-lookup"><span data-stu-id="69393-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="69393-437">W drugim elemencie **EntityTypeMapping** funkcje aktualizacji typu **SchoolModel. Person** są mapowane na procedurę składowaną **UpdatePerson**, w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-438">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-438">Example</span></span>

<span data-ttu-id="69393-439">W następnym przykładzie pokazano mapowanie hierarchii typów, w której typ główny jest abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="69393-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="69393-440">Zwróć uwagę na użycie składni `IsOfType` dla atrybutów **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="69393-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="69393-441">FunctionImportMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="69393-442">Element **FunctionImportMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między importem funkcji w modelu koncepcyjnym a przechowywaną procedurą lub funkcją w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="69393-443">Importy funkcji muszą być zadeklarowane w modelu koncepcyjnym, a procedury składowane muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="69393-444">Aby uzyskać więcej informacji, zobacz element FunctionImport (CSDL) i element funkcji (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="69393-445">Domyślnie, jeśli import funkcji zwraca typ jednostki koncepcyjnej lub typ złożony, nazwy kolumn zwracanych przez źródłową procedurę składowaną muszą dokładnie pasować do nazw właściwości w typie modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="69393-446">Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, mapowanie musi być zdefiniowane w elemencie ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="69393-447">Element **FunctionImportMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-448">ResultMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-449">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-449">Applicable Attributes</span></span>

<span data-ttu-id="69393-450">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="69393-451">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-451">Attribute Name</span></span>         | <span data-ttu-id="69393-452">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-452">Is Required</span></span> | <span data-ttu-id="69393-453">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="69393-454">**FunctionImportName**</span></span> | <span data-ttu-id="69393-455">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-455">Yes</span></span>         | <span data-ttu-id="69393-456">Nazwa importowanej funkcji w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="69393-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-457">**FunctionName**</span></span>       | <span data-ttu-id="69393-458">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-458">Yes</span></span>         | <span data-ttu-id="69393-459">Kwalifikowana nazwa przestrzeni nazw funkcji w modelu magazynu, który jest zamapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-460">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-460">Example</span></span>

<span data-ttu-id="69393-461">Poniższy przykład jest oparty na modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-461">The following example is based on the School model.</span></span> <span data-ttu-id="69393-462">W modelu magazynu należy wziąć pod uwagę następującą funkcję:</span><span class="sxs-lookup"><span data-stu-id="69393-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="69393-463">Należy również wziąć pod uwagę ten import funkcji w modelu koncepcyjnym:</span><span class="sxs-lookup"><span data-stu-id="69393-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="69393-464">Poniższy przykład przedstawia element **FunctionImportMapping** używany do mapowania funkcji i importowania funkcji powyżej do siebie:</span><span class="sxs-lookup"><span data-stu-id="69393-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="69393-465">InsertFunction — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="69393-466">Element **InsertFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję INSERT typu jednostki lub skojarzenia w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="69393-467">Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="69393-468">Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="69393-469">Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.</span><span class="sxs-lookup"><span data-stu-id="69393-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="69393-470">Element **InsertFunction** może być elementem podrzędnym elementu ModificationFunctionMapping i stosowany do elementu EntityTypeMapping lub AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="69393-471">InsertFunction zastosowana do EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="69393-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="69393-472">W przypadku zastosowania do elementu EntityTypeMapping element **InsertFunction** mapuje funkcję INSERT typu jednostki w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="69393-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="69393-473">Element **InsertFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="69393-474">AssociationEnd (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="69393-475">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="69393-476">Resultbinding (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="69393-477">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69393-478">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-478">Applicable Attributes</span></span>

<span data-ttu-id="69393-479">W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **InsertFunction** w przypadku zastosowania do elementu **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="69393-480">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-480">Attribute Name</span></span>            | <span data-ttu-id="69393-481">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-481">Is Required</span></span> | <span data-ttu-id="69393-482">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-483">**FunctionName**</span></span>          | <span data-ttu-id="69393-484">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-484">Yes</span></span>         | <span data-ttu-id="69393-485">Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja INSERT.</span><span class="sxs-lookup"><span data-stu-id="69393-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="69393-486">Procedura składowana musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="69393-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="69393-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="69393-488">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-488">No</span></span>          | <span data-ttu-id="69393-489">Nazwa parametru wyjściowego zwracająca liczbę odnośnych wierszy.</span><span class="sxs-lookup"><span data-stu-id="69393-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="69393-490">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-490">Example</span></span>

<span data-ttu-id="69393-491">Poniższy przykład jest oparty na modelu szkoły i pokazuje element **InsertFunction** używany do mapowania funkcji INSERT typu jednostki osoby do procedury składowanej **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="69393-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="69393-492">Procedura składowana **InsertPerson** jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="69393-493">InsertFunction zastosowana do AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="69393-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="69393-494">W przypadku zastosowania do elementu AssociationSetMapping element **InsertFunction** mapuje funkcję INSERT skojarzenia w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="69393-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="69393-495">Element **InsertFunction** może mieć następujące elementy podrzędne w przypadku zastosowania do elementu **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="69393-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="69393-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69393-497">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-497">Applicable Attributes</span></span>

<span data-ttu-id="69393-498">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **InsertFunction** , gdy jest on stosowany do elementu **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="69393-499">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-499">Attribute Name</span></span>            | <span data-ttu-id="69393-500">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-500">Is Required</span></span> | <span data-ttu-id="69393-501">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-502">**FunctionName**</span></span>          | <span data-ttu-id="69393-503">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-503">Yes</span></span>         | <span data-ttu-id="69393-504">Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest mapowana funkcja INSERT.</span><span class="sxs-lookup"><span data-stu-id="69393-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="69393-505">Procedura składowana musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="69393-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="69393-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="69393-507">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-507">No</span></span>          | <span data-ttu-id="69393-508">Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="69393-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="69393-509">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-509">Example</span></span>

<span data-ttu-id="69393-510">Poniższy przykład jest oparty na modelu szkoły i pokazuje element **InsertFunction** używany do mapowania funkcji INSERT skojarzenia **CourseInstructor** do procedury składowanej **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="69393-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="69393-511">Procedura składowana **InsertCourseInstructor** jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="69393-512">Element Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="69393-513">Element **Mapping** w języku specyfikacji mapowania (MSL) zawiera informacje dotyczące mapowania obiektów zdefiniowanych w modelu koncepcyjnym do bazy danych (zgodnie z opisem w modelu magazynu).</span><span class="sxs-lookup"><span data-stu-id="69393-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="69393-514">Aby uzyskać więcej informacji, zobacz Specyfikacja CSDL i Specyfikacja SSDL.</span><span class="sxs-lookup"><span data-stu-id="69393-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="69393-515">Element **Mapping** jest elementem głównym dla specyfikacji mapowania.</span><span class="sxs-lookup"><span data-stu-id="69393-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="69393-516">Przestrzeń nazw XML dla specyfikacji mapowania jest https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="69393-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="69393-517">Element Mapping może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="69393-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69393-518">Alias (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="69393-519">EntityContainerMapping (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="69393-520">Nazwy typów modelu koncepcyjnego i magazynu, do których odwołuje się w pliku MSL, muszą być kwalifikowane według ich nazw.</span><span class="sxs-lookup"><span data-stu-id="69393-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="69393-521">Aby uzyskać informacje o nazwie przestrzeni nazw modelu koncepcyjnego, zobacz element Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="69393-522">Aby uzyskać informacje o nazwie przestrzeni nazw modelu magazynu, zobacz element Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="69393-523">Aliasy dla przestrzeni nazw, które są używane w pliku MSL, można zdefiniować za pomocą elementu alias.</span><span class="sxs-lookup"><span data-stu-id="69393-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-524">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-524">Applicable Attributes</span></span>

<span data-ttu-id="69393-525">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **mapowania** .</span><span class="sxs-lookup"><span data-stu-id="69393-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="69393-526">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-526">Attribute Name</span></span> | <span data-ttu-id="69393-527">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-527">Is Required</span></span> | <span data-ttu-id="69393-528">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="69393-529">**Odstęp**</span><span class="sxs-lookup"><span data-stu-id="69393-529">**Space**</span></span>      | <span data-ttu-id="69393-530">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-530">Yes</span></span>         | <span data-ttu-id="69393-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="69393-531">**C-S**.</span></span> <span data-ttu-id="69393-532">Jest to stała wartość i nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="69393-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-533">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-533">Example</span></span>

<span data-ttu-id="69393-534">Poniższy przykład pokazuje element **mapowania** , który jest oparty na części modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="69393-535">Aby uzyskać więcej informacji na temat modelu szkoły, zobacz Szybki Start (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="69393-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="69393-536">Element mappingfragment — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="69393-537">Element **element mappingfragment** w języku specyfikacji mapowania (MSL) definiuje mapowanie między właściwościami typu jednostki modelu koncepcyjnego a tabelą lub widokiem w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="69393-538">Aby uzyskać informacje na temat typów jednostek modelu koncepcyjnego i tabel lub widoków bazy danych, zobacz element EntityType (CSDL) i element EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="69393-539">**Element mappingfragment** może być elementem podrzędnym elementu EntityTypeMapping lub elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="69393-540">Element **element mappingfragment** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-541">ComplexType (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="69393-542">Element scalarproperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="69393-543">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-544">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-544">Applicable Attributes</span></span>

<span data-ttu-id="69393-545">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **element mappingfragment** .</span><span class="sxs-lookup"><span data-stu-id="69393-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="69393-546">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-546">Attribute Name</span></span>          | <span data-ttu-id="69393-547">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-547">Is Required</span></span> | <span data-ttu-id="69393-548">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="69393-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="69393-550">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-550">Yes</span></span>         | <span data-ttu-id="69393-551">Nazwa mapowanej tabeli lub widoku.</span><span class="sxs-lookup"><span data-stu-id="69393-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="69393-552">**Atrybutem makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="69393-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="69393-553">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-553">No</span></span>          | <span data-ttu-id="69393-554">**Prawda** lub **Fałsz** w zależności od tego, czy zwracane są tylko unikatowe wiersze.</span><span class="sxs-lookup"><span data-stu-id="69393-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="69393-555">Jeśli ten atrybut ma **wartość true**, atrybut **GenerateUpdateViews** elementu EntityContainerMapping musi być ustawiony na **wartość false**.</span><span class="sxs-lookup"><span data-stu-id="69393-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-556">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-556">Example</span></span>

<span data-ttu-id="69393-557">Poniższy przykład przedstawia element **element mappingfragment** jako obiekt podrzędny elementu **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="69393-558">W tym przykładzie właściwości typu **kursu** w modelu koncepcyjnym są mapowane na kolumny tabeli **kursów** w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-559">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-559">Example</span></span>

<span data-ttu-id="69393-560">Poniższy przykład przedstawia element **element mappingfragment** jako obiekt podrzędny elementu **elementu EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="69393-561">Tak jak w powyższym przykładzie, właściwości typu **kursu** w modelu koncepcyjnym są mapowane na kolumny tabeli **kursów** w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="69393-562">ModificationFunctionMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="69393-563">Element **ModificationFunctionMapping** w języku specyfikacji mapowania (MSL) mapuje funkcje INSERT, Update i DELETE klasy koncepcyjnej Entity model na procedury składowane w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="69393-564">Element **ModificationFunctionMapping** może również mapować funkcje INSERT i DELETE dla skojarzeń wiele-do-wielu w modelu koncepcyjnym do procedur składowanych w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="69393-565">Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="69393-566">Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="69393-567">Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.</span><span class="sxs-lookup"><span data-stu-id="69393-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="69393-568">Jeśli funkcje modyfikacji dla jednej jednostki w hierarchii dziedziczenia są mapowane na procedury składowane, wówczas funkcje modyfikacji dla wszystkich typów w hierarchii muszą być mapowane na procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="69393-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="69393-569">Element **ModificationFunctionMapping** może być elementem podrzędnym elementu EntityTypeMapping lub AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="69393-570">Element **ModificationFunctionMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-571">DeleteFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="69393-572">InsertFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="69393-573">UpdateFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="69393-574">Żadne atrybuty nie mają zastosowania do elementu **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="69393-575">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-575">Example</span></span>

<span data-ttu-id="69393-576">Poniższy przykład przedstawia mapowanie zestawu jednostek dla obiektu **osoby** w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="69393-577">Oprócz mapowania kolumn dla typu podmiotu **osoby** są wyświetlane mapowania funkcji INSERT, Update i DELETE typu **osoba** .</span><span class="sxs-lookup"><span data-stu-id="69393-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="69393-578">Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-579">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-579">Example</span></span>

<span data-ttu-id="69393-580">Poniższy przykład przedstawia mapowanie zestawu skojarzeń dla zestawu skojarzenia **CourseInstructor** w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="69393-581">Oprócz mapowania kolumn dla skojarzenia **CourseInstructor** są wyświetlane mapowanie funkcji INSERT i DELETE skojarzenia **CourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="69393-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="69393-582">Funkcje, które są mapowane na są zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="69393-583">QueryView — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="69393-584">Element **QueryView** w języku specyfikacji mapowania (MSL) definiuje mapowanie tylko do odczytu między typem jednostki lub skojarzeniem w modelu koncepcyjnym i tabelą w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="69393-585">Mapowanie jest zdefiniowane za pomocą zapytania Entity SQL, które jest oceniane względem modelu magazynu, i można wyrazić zestaw wyników jako jednostkę lub skojarzenie w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="69393-586">Ponieważ widoki zapytań są tylko do odczytu, nie można używać standardowych poleceń aktualizacji do aktualizowania typów, które są zdefiniowane przez widoki zapytań.</span><span class="sxs-lookup"><span data-stu-id="69393-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="69393-587">Można wprowadzać aktualizacje tych typów przy użyciu funkcji modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="69393-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="69393-588">Aby uzyskać więcej informacji, zobacz How to: map modyfikujących Functions to procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="69393-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="69393-589">W elemencie **QueryView** wyrażenia Entity SQL, które zawierają właściwości **GroupBy**, Group Aggregates lub nawigacji, nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="69393-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="69393-590">Element **QueryView** może być elementem podrzędnym elementu elementu EntitySetMapping lub AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="69393-591">W poprzednim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla jednostki w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="69393-592">W tym drugim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla skojarzenia w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="69393-593">Jeśli element **AssociationSetMapping** jest dla skojarzenia z ograniczeniem referencyjnym, element **AssociationSetMapping** jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="69393-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="69393-594">Aby uzyskać więcej informacji, zobacz ReferentialConstraint element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="69393-595">Element **QueryView** nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="69393-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-596">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-596">Applicable Attributes</span></span>

<span data-ttu-id="69393-597">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="69393-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="69393-598">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-598">Attribute Name</span></span> | <span data-ttu-id="69393-599">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-599">Is Required</span></span> | <span data-ttu-id="69393-600">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="69393-601">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-601">**TypeName**</span></span>   | <span data-ttu-id="69393-602">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-602">No</span></span>          | <span data-ttu-id="69393-603">Nazwa typu modelu koncepcyjnego, który jest mapowany przez widok zapytania.</span><span class="sxs-lookup"><span data-stu-id="69393-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-604">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-604">Example</span></span>

<span data-ttu-id="69393-605">Poniższy przykład pokazuje element **QueryView** jako obiekt podrzędny elementu **elementu EntitySetMapping** i definiuje mapowanie widoku zapytania dla typu jednostki **działu** w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="69393-606">Ponieważ zapytanie zwraca tylko podzestaw elementów członkowskich typu **działu** w modelu magazynu, typ **działu** w modelu szkoły został zmodyfikowany na podstawie tego mapowania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="69393-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-607">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-607">Example</span></span>

<span data-ttu-id="69393-608">W następnym przykładzie pokazano element **QueryView** jako elementu podrzędnego elementu **AssociationSetMapping** i definiuje mapowanie tylko do odczytu dla skojarzenia `FK_Course_Department` w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="69393-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="69393-609">Komentarze</span><span class="sxs-lookup"><span data-stu-id="69393-609">Comments</span></span>

<span data-ttu-id="69393-610">Widoki zapytań można definiować w celu włączenia następujących scenariuszy:</span><span class="sxs-lookup"><span data-stu-id="69393-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="69393-611">Zdefiniuj jednostkę w modelu koncepcyjnym, która nie zawiera wszystkich właściwości jednostki w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="69393-612">Obejmuje to właściwości, które nie mają wartości domyślnych i nie obsługują wartości **null** .</span><span class="sxs-lookup"><span data-stu-id="69393-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="69393-613">Mapuj kolumny obliczane w modelu magazynu na właściwości typów jednostek w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="69393-614">Zdefiniuj mapowanie, gdzie warunki używane do partycjonowania jednostek w modelu koncepcyjnym nie są oparte na równości.</span><span class="sxs-lookup"><span data-stu-id="69393-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="69393-615">Po określeniu mapowania warunkowego przy użyciu elementu **Condition** , dostarczony warunek musi być równy podanej wartości.</span><span class="sxs-lookup"><span data-stu-id="69393-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="69393-616">Aby uzyskać więcej informacji, zobacz warunek element (MSL).</span><span class="sxs-lookup"><span data-stu-id="69393-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="69393-617">Mapuj tę samą kolumnę w modelu magazynu na wiele typów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="69393-618">Mapuj wiele typów do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="69393-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="69393-619">Zdefiniuj skojarzenia w modelu koncepcyjnym, które nie są oparte na kluczach obcych w schemacie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="69393-620">Użyj niestandardowej logiki biznesowej, aby ustawić wartość właściwości w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="69393-621">Na przykład można zamapować wartość ciągu "T" w źródle danych na wartość **true**, a Boolean w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="69393-622">Zdefiniuj filtry warunkowe dla wyników zapytania.</span><span class="sxs-lookup"><span data-stu-id="69393-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="69393-623">Wymuś mniejszą liczbę ograniczeń dotyczących danych w modelu koncepcyjnym niż w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="69393-624">Na przykład można wprowadzić właściwość w modelu koncepcyjnym nullable, nawet jeśli kolumna, do której jest zamapowany, nie obsługuje wartości **null**.</span><span class="sxs-lookup"><span data-stu-id="69393-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="69393-625">Podczas definiowania widoków zapytania dla jednostek należy wziąć pod uwagę następujące zagadnienia:</span><span class="sxs-lookup"><span data-stu-id="69393-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="69393-626">Widoki zapytań są tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="69393-626">Query views are read-only.</span></span> <span data-ttu-id="69393-627">Aktualizacje jednostek można wprowadzać tylko przy użyciu funkcji modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="69393-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="69393-628">Podczas definiowania typu jednostki za pomocą widoku zapytania, należy również zdefiniować wszystkie powiązane jednostki według widoków zapytań.</span><span class="sxs-lookup"><span data-stu-id="69393-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="69393-629">Podczas mapowania skojarzenia wiele-do-wielu do jednostki w modelu magazynu, która reprezentuje tabelę łączy w schemacie relacyjnym, należy zdefiniować element **QueryView** w elemencie **AssociationSetMapping** dla tej tabeli łączy.</span><span class="sxs-lookup"><span data-stu-id="69393-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="69393-630">Widoki zapytań muszą być zdefiniowane dla wszystkich typów w hierarchii typów.</span><span class="sxs-lookup"><span data-stu-id="69393-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="69393-631">Można to zrobić w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="69393-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="69393-632">Za pomocą pojedynczego elementu **QueryView** , który określa pojedyncze zapytanie Entity SQL zwracające Unię wszystkich typów jednostek w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="69393-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="69393-633">Za pomocą pojedynczego elementu **QueryView** , który określa pojedyncze zapytanie Entity SQL, które używa operatora Case do zwrócenia określonego typu jednostki w hierarchii na podstawie określonego warunku.</span><span class="sxs-lookup"><span data-stu-id="69393-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="69393-634">Z dodatkowym elementem **QueryView** dla określonego typu w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="69393-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="69393-635">W takim przypadku należy użyć atrybutu **TypeName** elementu **QueryView** , aby określić typ jednostki dla każdego widoku.</span><span class="sxs-lookup"><span data-stu-id="69393-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="69393-636">Gdy jest zdefiniowany widok zapytania, nie można określić atrybutu **StorageSetName** w elemencie **elementu EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="69393-637">Gdy jest zdefiniowany widok zapytania, element **elementu EntitySetMapping**nie może również zawierać mapowań **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="69393-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="69393-638">Resultbinding — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="69393-639">Element **resultbinding** w mapowaniu języka specyfikacji (MSL) mapuje wartości kolumn, które są zwracane przez procedury składowane do właściwości jednostki w modelu koncepcyjnym, gdy funkcje modyfikacji typu jednostki są mapowane na procedury składowane w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="69393-640">**Na przykład** , gdy wartość kolumny tożsamości jest zwracana przez procedurę składowaną INSERT, elementbinding mapuje zwracaną wartość na właściwość typu jednostki w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="69393-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="69393-641">Element **resultbinding** może być elementem podrzędnym elementu InsertFunction lub UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="69393-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="69393-642">Element **resultbinding** nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="69393-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-643">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-643">Applicable Attributes</span></span>

<span data-ttu-id="69393-644">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **resultbinding** :</span><span class="sxs-lookup"><span data-stu-id="69393-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="69393-645">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-645">Attribute Name</span></span> | <span data-ttu-id="69393-646">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-646">Is Required</span></span> | <span data-ttu-id="69393-647">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="69393-648">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-648">**Name**</span></span>       | <span data-ttu-id="69393-649">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-649">Yes</span></span>         | <span data-ttu-id="69393-650">Nazwa właściwości Entity w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="69393-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="69393-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="69393-651">**ColumnName**</span></span> | <span data-ttu-id="69393-652">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-652">Yes</span></span>         | <span data-ttu-id="69393-653">Nazwa mapowanej kolumny.</span><span class="sxs-lookup"><span data-stu-id="69393-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="69393-654">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-654">Example</span></span>

<span data-ttu-id="69393-655">Poniższy przykład jest oparty na modelu szkoły i zawiera element **InsertFunction** służący do mapowania funkcji INSERT typu podmiotu **osoby** do procedury składowanej **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="69393-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="69393-656">(Procedura składowana **InsertPerson** jest pokazana poniżej i jest zadeklarowana w modelu magazynu). Element **resultbinding** jest używany do mapowania wartości kolumny, która jest zwracana przez procedurę składowaną (**NewPersonID**) do właściwości typu jednostki (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="69393-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="69393-657">Następujące instrukcje języka Transact-SQL opisują procedurę składowaną **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="69393-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="69393-658">ResultMapping — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="69393-659">Element **ResultMapping** w języku specyfikacji mapowania (MSL) definiuje mapowanie między importem funkcji w modelu koncepcyjnym i procedurą składowaną w źródłowej bazie danych, gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="69393-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="69393-660">Import funkcji zwraca typ jednostki koncepcyjnej lub typ złożony.</span><span class="sxs-lookup"><span data-stu-id="69393-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="69393-661">Nazwy kolumn zwracanych przez procedurę składowaną nie są dokładnie zgodne z nazwami właściwości typu jednostki lub typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="69393-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="69393-662">Domyślnie mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki lub typem złożonym jest oparte na nazwach kolumn i właściwości.</span><span class="sxs-lookup"><span data-stu-id="69393-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="69393-663">Jeśli nazwy kolumn nie są dokładnie zgodne z nazwami właściwości, należy użyć elementu **ResultMapping** , aby zdefiniować mapowanie.</span><span class="sxs-lookup"><span data-stu-id="69393-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="69393-664">Przykład mapowania domyślnego można znaleźć w temacie FunctionImportMapping element (MSL).</span><span class="sxs-lookup"><span data-stu-id="69393-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="69393-665">Element **ResultMapping** jest elementem podrzędnym elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="69393-666">Element **ResultMapping** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-667">EntityTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="69393-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="69393-668">ComplexTypeMapping</span></span>

<span data-ttu-id="69393-669">Żadne atrybuty nie mają zastosowania do elementu **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="69393-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="69393-670">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-670">Example</span></span>

<span data-ttu-id="69393-671">Weź pod uwagę następujące procedury składowane:</span><span class="sxs-lookup"><span data-stu-id="69393-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="69393-672">Należy również wziąć pod uwagę następujący typ jednostki koncepcyjnej modelu:</span><span class="sxs-lookup"><span data-stu-id="69393-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="69393-673">Aby można było utworzyć import funkcji, który zwraca wystąpienia poprzedniego typu jednostki, mapowanie między kolumnami zwracanymi przez procedurę składowaną a typem jednostki musi być zdefiniowane w elemencie **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="69393-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="69393-674">Element scalarproperty — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="69393-675">Element **element scalarproperty** w języku specyfikacji mapowania (MSL) mapuje właściwość na typ jednostki modelu koncepcyjnego, typ złożony lub skojarzenie do kolumny tabeli lub parametru procedury składowanej w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="69393-676">Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="69393-677">Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="69393-678">Element **element scalarproperty** może być elementem podrzędnym następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="69393-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="69393-679">Element mappingfragment</span><span class="sxs-lookup"><span data-stu-id="69393-679">MappingFragment</span></span>
-   <span data-ttu-id="69393-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="69393-680">InsertFunction</span></span>
-   <span data-ttu-id="69393-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="69393-681">UpdateFunction</span></span>
-   <span data-ttu-id="69393-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="69393-682">DeleteFunction</span></span>
-   <span data-ttu-id="69393-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="69393-683">EndProperty</span></span>
-   <span data-ttu-id="69393-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="69393-684">ComplexProperty</span></span>
-   <span data-ttu-id="69393-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="69393-685">ResultMapping</span></span>

<span data-ttu-id="69393-686">Jako element podrzędny elementu **element mappingfragment**, **ComplexProperty**lub **EndProperty** element **element scalarproperty** mapuje właściwość w modelu koncepcyjnym do kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="69393-687">Jako element podrzędny elementu **InsertFunction**, **UpdateFunction**lub **DeleteFunction** element **element scalarproperty** mapuje właściwość w modelu koncepcyjnym do parametru procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="69393-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="69393-688">Element **element scalarproperty** nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="69393-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-689">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-689">Applicable Attributes</span></span>

<span data-ttu-id="69393-690">Atrybuty, które mają zastosowanie do elementu **element scalarproperty** , różnią się w zależności od roli elementu.</span><span class="sxs-lookup"><span data-stu-id="69393-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="69393-691">W poniższej tabeli opisano atrybuty, które są stosowane, gdy element **element scalarproperty** jest używany do mapowania właściwości modelu koncepcyjnego na kolumnę w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="69393-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="69393-692">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-692">Attribute Name</span></span> | <span data-ttu-id="69393-693">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-693">Is Required</span></span> | <span data-ttu-id="69393-694">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="69393-695">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-695">**Name**</span></span>       | <span data-ttu-id="69393-696">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-696">Yes</span></span>         | <span data-ttu-id="69393-697">Nazwa mapowanej właściwości modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="69393-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="69393-698">**ColumnName**</span></span> | <span data-ttu-id="69393-699">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-699">Yes</span></span>         | <span data-ttu-id="69393-700">Nazwa mapowanej kolumny tabeli.</span><span class="sxs-lookup"><span data-stu-id="69393-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="69393-701">W poniższej tabeli opisano atrybuty, które mają zastosowanie do elementu **element scalarproperty** , gdy jest on używany do mapowania właściwości modelu koncepcyjnego na parametr procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="69393-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="69393-702">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-702">Attribute Name</span></span>    | <span data-ttu-id="69393-703">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-703">Is Required</span></span> | <span data-ttu-id="69393-704">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-705">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="69393-705">**Name**</span></span>          | <span data-ttu-id="69393-706">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-706">Yes</span></span>         | <span data-ttu-id="69393-707">Nazwa mapowanej właściwości modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="69393-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="69393-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="69393-708">**ParameterName**</span></span> | <span data-ttu-id="69393-709">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-709">Yes</span></span>         | <span data-ttu-id="69393-710">Nazwa mapowanego parametru.</span><span class="sxs-lookup"><span data-stu-id="69393-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="69393-711">**Wersja**</span><span class="sxs-lookup"><span data-stu-id="69393-711">**Version**</span></span>       | <span data-ttu-id="69393-712">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-712">No</span></span>          | <span data-ttu-id="69393-713">**Bieżący** lub **oryginalny** w zależności od tego, czy bieżąca wartość lub oryginalna wartość właściwości ma być używana do sprawdzania współbieżności.</span><span class="sxs-lookup"><span data-stu-id="69393-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="69393-714">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-714">Example</span></span>

<span data-ttu-id="69393-715">Poniższy przykład pokazuje element **element scalarproperty** używany na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="69393-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="69393-716">Aby zmapować właściwości typu podmiotu **osoby** do kolumn tabeli **Person**.</span><span class="sxs-lookup"><span data-stu-id="69393-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="69393-717">Aby zmapować właściwości typu podmiotu **osoby** do parametrów procedury składowanej **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="69393-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="69393-718">Procedury składowane są zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="69393-719">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-719">Example</span></span>

<span data-ttu-id="69393-720">W następnym przykładzie pokazano element **element scalarproperty** używany do mapowania funkcji INSERT i DELETE skojarzenia modelu koncepcyjnego na procedury składowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="69393-721">Procedury składowane są zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="69393-722">UpdateFunction — element (MSL)</span><span class="sxs-lookup"><span data-stu-id="69393-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="69393-723">Element **UpdateFunction** w języku specyfikacji mapowania (MSL) mapuje funkcję Update typu jednostki w modelu koncepcyjnym do procedury składowanej w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="69393-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="69393-724">Procedury składowane, do których są mapowane funkcje modyfikacji, muszą być zadeklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="69393-725">Aby uzyskać więcej informacji, zobacz funkcja element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69393-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="69393-726">Jeśli nie wszystkie trzy operacje wstawiania, aktualizowania lub usuwania typu jednostki są mapowane na procedury składowane, Niemapowane operacje zakończą się niepowodzeniem, jeśli są wykonywane w czasie wykonywania i zostanie zgłoszony wyjątek UpdateException.</span><span class="sxs-lookup"><span data-stu-id="69393-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="69393-727">Element **UpdateFunction** może być elementem podrzędnym elementu ModificationFunctionMapping i stosowany do elementu EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="69393-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="69393-728">Element **UpdateFunction** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="69393-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="69393-729">AssociationEnd (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="69393-730">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="69393-731">Resultbinding (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="69393-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="69393-732">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="69393-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69393-733">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="69393-733">Applicable Attributes</span></span>

<span data-ttu-id="69393-734">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="69393-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="69393-735">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="69393-735">Attribute Name</span></span>            | <span data-ttu-id="69393-736">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="69393-736">Is Required</span></span> | <span data-ttu-id="69393-737">Wartość</span><span class="sxs-lookup"><span data-stu-id="69393-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69393-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="69393-738">**FunctionName**</span></span>          | <span data-ttu-id="69393-739">Yes</span><span class="sxs-lookup"><span data-stu-id="69393-739">Yes</span></span>         | <span data-ttu-id="69393-740">Kwalifikowana nazwa przestrzeni nazw procedury składowanej, do której jest zamapowana funkcja aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="69393-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="69393-741">Procedura składowana musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="69393-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="69393-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="69393-743">Nie</span><span class="sxs-lookup"><span data-stu-id="69393-743">No</span></span>          | <span data-ttu-id="69393-744">Nazwa parametru wyjściowego, który zwraca liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="69393-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="69393-745">Przykład</span><span class="sxs-lookup"><span data-stu-id="69393-745">Example</span></span>

<span data-ttu-id="69393-746">Poniższy przykład jest oparty na modelu szkoły i pokazuje element **UpdateFunction** używany do mapowania funkcji Update typu jednostki **osoby** na procedurę składowaną **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="69393-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="69393-747">Procedura składowana **UpdatePerson** jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="69393-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
