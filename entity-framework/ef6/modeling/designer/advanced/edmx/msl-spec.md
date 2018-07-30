---
title: Specyfikacja MSL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912785"
---
# <a name="msl-specification"></a><span data-ttu-id="f8326-102">Specyfikacja MSL</span><span class="sxs-lookup"><span data-stu-id="f8326-102">MSL Specification</span></span>
<span data-ttu-id="f8326-103">Mapowanie specyfikacji języka (MSL) to język oparty na formacie XML, który opisuje mapowanie między modelu koncepcyjnym i modelu magazynu aplikacji platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f8326-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="f8326-104">W aplikacji Entity Framework mapowania metadanych jest ładowany z pliku MSL albo identyfikatorem (zapisany w pliku MSL) w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f8326-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="f8326-105">Entity Framework używa mapowania metadanych w czasie wykonywania do przekształcania zapytania względem modelu koncepcyjnego polecenia specyficzne dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="f8326-106">Entity Framework Designer (Projektant EF) przechowuje informacje dotyczące mapowania w pliku edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="f8326-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="f8326-107">W czasie kompilacji Projektant jednostki używa informacji w pliku edmx można utworzyć pliku MSL albo identyfikatorem, które są wymagane przez Entity Framework w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="f8326-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="f8326-108">Nazwy wszystkich koncepcje lub typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8326-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f8326-109">Aby uzyskać informacji na temat modelu koncepcyjnego nazwa przestrzeni nazw, zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="f8326-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="f8326-110">Aby dowiedzieć się, nazwa przestrzeni nazw w modelu magazynu, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="f8326-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="f8326-111">Wersje MSL są zróżnicowane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="f8326-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="f8326-112">Wersja pliku MSL</span><span class="sxs-lookup"><span data-stu-id="f8326-112">MSL Version</span></span> | <span data-ttu-id="f8326-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="f8326-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="f8326-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="f8326-114">MSL v1</span></span>      | <span data-ttu-id="f8326-115">Nazwa urn: schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="f8326-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="f8326-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="f8326-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="f8326-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="f8326-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="f8326-118">W elemencie aliasu (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-118">Alias Element (MSL)</span></span>

<span data-ttu-id="f8326-119">**Alias** element w mapowaniu language specification (MSL) jest elementem podrzędnym elementu mapowania, który jest używany do definiowania aliasy koncepcyjny model i magazynu modeli w przypadku przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8326-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="f8326-120">Nazwy wszystkich koncepcje lub typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8326-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f8326-121">Dla informacji o modelu koncepcyjnego nazwa przestrzeni nazw Zobacz Element schematu (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="f8326-122">Informacje o nazwie przestrzeni nazw w modelu magazynu na ten temat można znaleźć w elemencie schematu (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="f8326-123">**Alias** elementu nie może mieć elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f8326-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-124">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-124">Applicable Attributes</span></span>

<span data-ttu-id="f8326-125">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **Alias** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="f8326-126">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-126">Attribute Name</span></span> | <span data-ttu-id="f8326-127">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-127">Is Required</span></span> | <span data-ttu-id="f8326-128">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="f8326-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="f8326-129">**Key**</span></span>        | <span data-ttu-id="f8326-130">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-130">Yes</span></span>         | <span data-ttu-id="f8326-131">Alias przestrzeni nazw, który jest określony przez **wartość** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f8326-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="f8326-132">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="f8326-132">**Value**</span></span>      | <span data-ttu-id="f8326-133">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-133">Yes</span></span>         | <span data-ttu-id="f8326-134">Przestrzeń nazw, dla których wartość **klucz** element jest aliasem.</span><span class="sxs-lookup"><span data-stu-id="f8326-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="f8326-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-135">Example</span></span>

<span data-ttu-id="f8326-136">W poniższym przykładzie przedstawiono **Alias** element, który definiuje alias, `c`, dla których typy zostały zdefiniowane w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="f8326-137">Punkt końcowy skojarzenia elementu (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="f8326-138">**Punkt końcowy skojarzenia** element w mapowaniu language specification (MSL) jest używany, gdy funkcji modyfikacji typu jednostki w modelu koncepcyjnym są mapowane do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="f8326-139">Jeśli modyfikacja procedury składowanej przyjmuje parametr, którego wartość jest przechowywana w właściwość skojarzenia **punkt końcowy skojarzenia** element mapuje wartości właściwości do parametru.</span><span class="sxs-lookup"><span data-stu-id="f8326-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="f8326-140">Aby uzyskać więcej informacji zobacz przykład poniżej.</span><span class="sxs-lookup"><span data-stu-id="f8326-140">For more information, see the example below.</span></span>

<span data-ttu-id="f8326-141">Aby uzyskać więcej informacji na temat mapowania funkcji modyfikacji typów jednostek do procedur składowanych, zobacz Element ModificationFunctionMapping (MSL) i wskazówki: mapowanie jednostki do procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="f8326-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="f8326-142">**Punkt końcowy skojarzenia** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-144">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-144">Applicable Attributes</span></span>

<span data-ttu-id="f8326-145">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **punkt końcowy skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="f8326-146">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-146">Attribute Name</span></span>     | <span data-ttu-id="f8326-147">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-147">Is Required</span></span> | <span data-ttu-id="f8326-148">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-149">**Obiekt AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="f8326-149">**AssociationSet**</span></span> | <span data-ttu-id="f8326-150">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-150">Yes</span></span>         | <span data-ttu-id="f8326-151">Nazwa skojarzenia, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="f8326-152">**From**</span><span class="sxs-lookup"><span data-stu-id="f8326-152">**From**</span></span>           | <span data-ttu-id="f8326-153">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-153">Yes</span></span>         | <span data-ttu-id="f8326-154">Wartość **FromRole** atrybut właściwość nawigacji, która odnosi się do skojarzenia mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="f8326-155">Aby uzyskać więcej informacji zobacz element NavigationProperty — Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="f8326-156">**To**</span><span class="sxs-lookup"><span data-stu-id="f8326-156">**To**</span></span>             | <span data-ttu-id="f8326-157">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-157">Yes</span></span>         | <span data-ttu-id="f8326-158">Wartość **ToRole** atrybut właściwość nawigacji, która odnosi się do skojarzenia mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="f8326-159">Aby uzyskać więcej informacji zobacz element NavigationProperty — Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="f8326-160">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-160">Example</span></span>

<span data-ttu-id="f8326-161">Należy wziąć pod uwagę następujące typie modelu koncepcyjnego entity:</span><span class="sxs-lookup"><span data-stu-id="f8326-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="f8326-162">Należy również rozważyć następującą procedurę składowaną:</span><span class="sxs-lookup"><span data-stu-id="f8326-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="f8326-163">Aby zamapować funkcję aktualizacji `Course` jednostki do tej procedury składowanej, należy podać wartość **DepartmentID** parametru.</span><span class="sxs-lookup"><span data-stu-id="f8326-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="f8326-164">Wartość `DepartmentID` nie odpowiada na właściwość typu jednostki; znajduje się w skojarzeniu niezależnie od którego mapowanie jest następująca:</span><span class="sxs-lookup"><span data-stu-id="f8326-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="f8326-165">Poniższy kod przedstawia **punkt końcowy skojarzenia** element używany do mapowania **DepartmentID** właściwość **klucza Obcego\_kurs\_działu** skojarzenie, aby **UpdateCourse** procedury składowanej (do którego funkcja aktualizacji **kurs** typ jednostki jest mapowany):</span><span class="sxs-lookup"><span data-stu-id="f8326-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="f8326-166">Element AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-167">**AssociationSetMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między skojarzenia koncepcyjny kolumn w modelu i tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="f8326-168">Skojarzeń w modelu koncepcyjnym są typy, których właściwości reprezentują podstawowe i obce kolumny klucza w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="f8326-169">**AssociationSetMapping** element używa dwóch elementów właściwości EndProperty, aby zdefiniować mapowania między właściwościami typu skojarzenia i kolumn w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="f8326-170">Warunki można umieścić na te mapowania z elementem warunku.</span><span class="sxs-lookup"><span data-stu-id="f8326-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="f8326-171">Mapowanie insert, update i delete funkcji w celu skojarzenia do procedur składowanych w bazie danych za pomocą elementu ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="f8326-172">Za pomocą ciągu jednostki SQL w elemencie QueryView, należy zdefiniować tylko do odczytu mapowania między skojarzenia i kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-173">Jeśli nie zdefiniowano ograniczenie referencyjne dla skojarzenia w modelu koncepcyjnym, skojarzenie nie muszą być mapowane z **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="f8326-174">Jeśli **AssociationSetMapping** element jest obecny dla skojarzenia, która ma ograniczenie referencyjne, mapowania zdefiniowane w **AssociationSetMapping** element zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="f8326-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="f8326-175">Aby uzyskać więcej informacji zobacz Element ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="f8326-176">**AssociationSetMapping** element może mieć następujące elementy podrzędne</span><span class="sxs-lookup"><span data-stu-id="f8326-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="f8326-177">Obiekt QueryView (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="f8326-178">Właściwości EndProperty (zero lub dwóch)</span><span class="sxs-lookup"><span data-stu-id="f8326-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="f8326-179">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="f8326-180">ModificationFunctionMapping (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-181">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-181">Applicable Attributes</span></span>

<span data-ttu-id="f8326-182">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f8326-183">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-183">Attribute Name</span></span>     | <span data-ttu-id="f8326-184">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-184">Is Required</span></span> | <span data-ttu-id="f8326-185">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-186">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-186">**Name**</span></span>           | <span data-ttu-id="f8326-187">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-187">Yes</span></span>         | <span data-ttu-id="f8326-188">Nazwa zestawu skojarzeń modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="f8326-189">**Nazwa typu**</span><span class="sxs-lookup"><span data-stu-id="f8326-189">**TypeName**</span></span>       | <span data-ttu-id="f8326-190">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-190">No</span></span>          | <span data-ttu-id="f8326-191">Przestrzeń nazw kwalifikowana nazwa typu skojarzenia modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="f8326-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="f8326-192">**StoreEntitySet**</span></span> | <span data-ttu-id="f8326-193">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-193">No</span></span>          | <span data-ttu-id="f8326-194">Nazwa tabeli, która jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="f8326-195">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-195">Example</span></span>

<span data-ttu-id="f8326-196">W poniższym przykładzie przedstawiono **AssociationSetMapping** elementu, w którym **klucza Obcego\_kurs\_działu** skojarzeń w modelu koncepcyjnego jest mapowany na  **Kurs** tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="f8326-197">Mapowania między właściwościami typu skojarzenia i kolumn w tabeli są określone w podrzędnej **właściwości EndProperty** elementów.</span><span class="sxs-lookup"><span data-stu-id="f8326-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="f8326-198">Element ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="f8326-199">A **ComplexProperty** element w mapowaniu language specification (MSL) definiuje mapowanie między właściwość typu złożonego w modelu koncepcyjnego entity kolumny Typ i tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="f8326-200">Mapowania kolumn właściwości są określone w elementy ScalarProperty podrzędne.</span><span class="sxs-lookup"><span data-stu-id="f8326-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="f8326-201">**ComplexType** property — element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-202">ScalarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="f8326-203">**ComplexProperty** (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="f8326-204">ComplextTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f8326-205">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-206">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-206">Applicable Attributes</span></span>

<span data-ttu-id="f8326-207">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ComplexProperty** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="f8326-208">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-208">Attribute Name</span></span> | <span data-ttu-id="f8326-209">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-209">Is Required</span></span> | <span data-ttu-id="f8326-210">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-211">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-211">**Name**</span></span>       | <span data-ttu-id="f8326-212">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-212">Yes</span></span>         | <span data-ttu-id="f8326-213">Nazwa właściwości złożonej typu jednostki w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="f8326-214">**Nazwa typu**</span><span class="sxs-lookup"><span data-stu-id="f8326-214">**TypeName**</span></span>   | <span data-ttu-id="f8326-215">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-215">No</span></span>          | <span data-ttu-id="f8326-216">Przestrzeń nazw kwalifikowana nazwa typu właściwości modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="f8326-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="f8326-217">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-217">Example</span></span>

<span data-ttu-id="f8326-218">Poniższy przykład jest oparty na modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-218">The following example is based on the School model.</span></span> <span data-ttu-id="f8326-219">Następujący typ złożony został dodany do modelu koncepcyjnego:</span><span class="sxs-lookup"><span data-stu-id="f8326-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="f8326-220">**LastName** i **FirstName** właściwości **osoby** typu jednostki zostały zamienione na jedną właściwość złożona, **nazwa**:</span><span class="sxs-lookup"><span data-stu-id="f8326-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="f8326-221">Pokazano w poniższym pliku MSL **ComplexProperty** element używany do mapowania **nazwa** właściwości do kolumn w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="f8326-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="f8326-222">Element ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-223">**ComplexTypeMapping** elementu w mapowaniu language specification (MSL) jest elementem podrzędnym elementu ResultMapping i definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnym i procedury składowanej w źródłowym Baza danych, gdy spełnione są następujące:</span><span class="sxs-lookup"><span data-stu-id="f8326-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="f8326-224">Importowanie funkcji zwraca koncepcyjny typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="f8326-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="f8326-225">Nazwy kolumn zwrócone przez procedurę składowaną nie odpowiadają dokładnie nazwy właściwości na typ złożony.</span><span class="sxs-lookup"><span data-stu-id="f8326-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="f8326-226">Domyślnie mapowania między kolumnami zwrócone przez procedurę składowaną i typ złożony opiera się na nazwy kolumn i właściwości.</span><span class="sxs-lookup"><span data-stu-id="f8326-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="f8326-227">Nazwy kolumn nie odpowiadają dokładnie nazw właściwości, należy użyć **ComplexTypeMapping** elementu, aby zdefiniować mapowanie.</span><span class="sxs-lookup"><span data-stu-id="f8326-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="f8326-228">Aby uzyskać przykład domyślnego mapowania Zobacz Element FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="f8326-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="f8326-229">**ComplexTypeMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-230">ScalarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-231">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-231">Applicable Attributes</span></span>

<span data-ttu-id="f8326-232">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ComplexTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="f8326-233">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-233">Attribute Name</span></span> | <span data-ttu-id="f8326-234">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-234">Is Required</span></span> | <span data-ttu-id="f8326-235">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="f8326-236">**Nazwa typu**</span><span class="sxs-lookup"><span data-stu-id="f8326-236">**TypeName**</span></span>   | <span data-ttu-id="f8326-237">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-237">Yes</span></span>         | <span data-ttu-id="f8326-238">Nazwa kwalifikowana przez obszar nazw typ złożony, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-239">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-239">Example</span></span>

<span data-ttu-id="f8326-240">Należy wziąć pod uwagę następujące procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="f8326-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="f8326-241">Należy również rozważyć następujący typ złożony modelu koncepcyjnego:</span><span class="sxs-lookup"><span data-stu-id="f8326-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="f8326-242">Aby można było utworzyć importu funkcji, który zwraca wystąpień poprzedniego typu złożonego, mapowanie między kolumnami zwrócone przez procedurę składowaną i typ jednostki musi być zdefiniowany w **ComplexTypeMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="f8326-243">Warunek, Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-243">Condition Element (MSL)</span></span>

<span data-ttu-id="f8326-244">**Warunek** elementu w mapowaniu language specification (MSL) powoduje warunków mapowania między modelu koncepcyjnego i podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="f8326-245">Mapowania, która jest zdefiniowana w węźle XML jest nieprawidłowy, jeśli wszystkie warunki, jako element podrzędny określonego w **warunek** elementów, są spełnione.</span><span class="sxs-lookup"><span data-stu-id="f8326-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="f8326-246">W przeciwnym razie mapowanie jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="f8326-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="f8326-247">Na przykład, jeśli MappingFragment element zawiera jeden lub więcej **warunek** elementy podrzędne, mapowanie zdefiniowane w ramach **MappingFragment** węzeł będzie obowiązywał tylko jeśli wszystkie warunki podrzędnych  **Warunek** elementy są spełnione.</span><span class="sxs-lookup"><span data-stu-id="f8326-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="f8326-248">Każdy warunek można zastosować do jednej **nazwa** (nazwa właściwości jednostki modelu koncepcyjnego, określony przez **nazwa** atrybut), lub **ColumnName** (nazwa kolumny w Określona baza danych, przez **ColumnName** atrybutu).</span><span class="sxs-lookup"><span data-stu-id="f8326-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="f8326-249">Gdy **nazwa** ma ustawioną wartość atrybutu, warunek jest sprawdzany na podstawie wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="f8326-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="f8326-250">Gdy **ColumnName** ma ustawioną wartość atrybutu, warunek jest sprawdzana względem wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="f8326-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="f8326-251">Tylko jeden z **nazwa** lub **ColumnName** atrybutu można określić w **warunek** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-252">Gdy **warunek** element jest używany w elemencie FunctionImportMapping tylko **nazwa** atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f8326-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="f8326-253">**Warunek** element może być elementem podrzędnym następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="f8326-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="f8326-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="f8326-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="f8326-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-255">ComplexProperty</span></span>
-   <span data-ttu-id="f8326-256">Obiekcie EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="f8326-256">EntitySetMapping</span></span>
-   <span data-ttu-id="f8326-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="f8326-257">MappingFragment</span></span>
-   <span data-ttu-id="f8326-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f8326-258">EntityTypeMapping</span></span>

<span data-ttu-id="f8326-259">**Warunek** element może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f8326-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-260">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-260">Applicable Attributes</span></span>

<span data-ttu-id="f8326-261">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **warunek** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="f8326-262">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-262">Attribute Name</span></span> | <span data-ttu-id="f8326-263">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-263">Is Required</span></span> | <span data-ttu-id="f8326-264">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-265">**NazwaKolumny**</span><span class="sxs-lookup"><span data-stu-id="f8326-265">**ColumnName**</span></span> | <span data-ttu-id="f8326-266">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-266">No</span></span>          | <span data-ttu-id="f8326-267">Nazwa kolumny tabeli, którego wartość jest używana do warunku.</span><span class="sxs-lookup"><span data-stu-id="f8326-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="f8326-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="f8326-268">**IsNull**</span></span>     | <span data-ttu-id="f8326-269">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-269">No</span></span>          | <span data-ttu-id="f8326-270">**Wartość true,** lub **False**.</span><span class="sxs-lookup"><span data-stu-id="f8326-270">**True** or **False**.</span></span> <span data-ttu-id="f8326-271">Jeśli wartość jest **True** , a wartość kolumny jest **o wartości null**, lub jeśli wartość jest **False** i wartość kolumny jest **null**, warunek jest prawdziwy .</span><span class="sxs-lookup"><span data-stu-id="f8326-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="f8326-272">W przeciwnym razie warunek ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="f8326-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="f8326-273">**IsNull** i **wartość** atrybutów nie można użyć w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="f8326-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="f8326-274">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="f8326-274">**Value**</span></span>      | <span data-ttu-id="f8326-275">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-275">No</span></span>          | <span data-ttu-id="f8326-276">Wartość, z którym wartość kolumny jest porównywany.</span><span class="sxs-lookup"><span data-stu-id="f8326-276">The value with which the column value is compared.</span></span> <span data-ttu-id="f8326-277">Jeśli wartości są takie same, warunek jest prawdziwy.</span><span class="sxs-lookup"><span data-stu-id="f8326-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="f8326-278">W przeciwnym razie warunek ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="f8326-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="f8326-279">**IsNull** i **wartość** atrybutów nie można użyć w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="f8326-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="f8326-280">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-280">**Name**</span></span>       | <span data-ttu-id="f8326-281">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-281">No</span></span>          | <span data-ttu-id="f8326-282">Nazwa właściwości jednostki modelu koncepcyjnego, którego wartość jest używana do warunku.</span><span class="sxs-lookup"><span data-stu-id="f8326-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="f8326-283">Ten atrybut nie ma zastosowania Jeśli **warunek** element jest używany w elemencie FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="f8326-284">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-284">Example</span></span>

<span data-ttu-id="f8326-285">W poniższym przykładzie przedstawiono **warunek** elementy jako elementy podrzędne **MappingFragment** elementów.</span><span class="sxs-lookup"><span data-stu-id="f8326-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="f8326-286">Gdy **DataZatrudnienia** nie ma wartości null i **EnrollmentDate** jest wartość null, dane zamapowane między **SchoolModel.Instructor** typu i **PersonID**i **DataZatrudnienia** kolumn **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="f8326-287">Gdy **EnrollmentDate** nie ma wartości null i **DataZatrudnienia** jest wartość null, dane zamapowane między **SchoolModel.Student** typu i **PersonID** i **rejestracji** kolumn **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="f8326-288">Element DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="f8326-289">**DeleteFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji usuwania w typie jednostki lub skojarzeń w modelu koncepcyjnym procedurę składowaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f8326-290">Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f8326-291">Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-292">Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.</span><span class="sxs-lookup"><span data-stu-id="f8326-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="f8326-293">Stosowane do EntityTypeMapping DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="f8326-294">W przypadku zastosowania do elementu EntityTypeMapping **DeleteFunction** element mapy funkcji usuwania typu jednostki w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f8326-295">**DeleteFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **EntityTypeMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="f8326-296">Punkt końcowy skojarzenia (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f8326-297">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f8326-298">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f8326-299">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-299">Applicable Attributes</span></span>

<span data-ttu-id="f8326-300">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **DeleteFunction** element, jeśli jest stosowany do **EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f8326-301">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-301">Attribute Name</span></span>            | <span data-ttu-id="f8326-302">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-302">Is Required</span></span> | <span data-ttu-id="f8326-303">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-304">**FunctionName**</span></span>          | <span data-ttu-id="f8326-305">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-305">Yes</span></span>         | <span data-ttu-id="f8326-306">Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji usuwania.</span><span class="sxs-lookup"><span data-stu-id="f8326-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="f8326-307">Procedura składowana musi być zadeklarowany w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f8326-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="f8326-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f8326-309">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-309">No</span></span>          | <span data-ttu-id="f8326-310">Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.</span><span class="sxs-lookup"><span data-stu-id="f8326-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f8326-311">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-311">Example</span></span>

<span data-ttu-id="f8326-312">Poniższy przykład jest oparty na modelu szkoły i pokazuje **DeleteFunction** element mapowania funkcji usuwania **osoby** typu jednostki **DeletePerson** Procedura składowana.</span><span class="sxs-lookup"><span data-stu-id="f8326-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="f8326-313">**DeletePerson** procedury składowanej jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="f8326-314">Stosowane do AssociationSetMapping DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="f8326-315">W przypadku zastosowania do elementu AssociationSetMapping **DeleteFunction** element mapy funkcji usuwania skojarzenia w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f8326-316">**DeleteFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **AssociationSetMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="f8326-317">Właściwości EndProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f8326-318">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-318">Applicable Attributes</span></span>

<span data-ttu-id="f8326-319">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **DeleteFunction** element, jeśli jest stosowany do **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f8326-320">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-320">Attribute Name</span></span>            | <span data-ttu-id="f8326-321">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-321">Is Required</span></span> | <span data-ttu-id="f8326-322">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-323">**FunctionName**</span></span>          | <span data-ttu-id="f8326-324">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-324">Yes</span></span>         | <span data-ttu-id="f8326-325">Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji usuwania.</span><span class="sxs-lookup"><span data-stu-id="f8326-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="f8326-326">Procedura składowana musi być zadeklarowany w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f8326-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="f8326-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f8326-328">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-328">No</span></span>          | <span data-ttu-id="f8326-329">Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.</span><span class="sxs-lookup"><span data-stu-id="f8326-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f8326-330">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-330">Example</span></span>

<span data-ttu-id="f8326-331">Poniższy przykład jest oparty na modelu szkoły i pokazuje **DeleteFunction** element używany do mapowania funkcji usuwania **CourseInstructor** skojarzenia  **DeleteCourseInstructor** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="f8326-332">**DeleteCourseInstructor** procedury składowanej jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="f8326-333">Element właściwości EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="f8326-334">**Właściwości EndProperty** element w mapowaniu language specification (MSL) definiuje mapowanie między punktu końcowego lub funkcją Modyfikowanie skojarzenia typu modelu koncepcyjnego i podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="f8326-335">Mapowanie kolumny właściwości jest określone w elemencie ScalarProperty podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="f8326-336">Gdy **właściwości EndProperty** element jest używany do definiowania mapowania dla elementu end skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="f8326-337">Gdy **właściwości EndProperty** element jest używany do definiowania mapowania dla funkcji modyfikacji skojarzenia modelu koncepcyjnego, jest elementem podrzędnym elementu InsertFunction lub DeleteFunction elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="f8326-338">**Właściwości EndProperty** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-339">ScalarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-340">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-340">Applicable Attributes</span></span>

<span data-ttu-id="f8326-341">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **właściwości EndProperty** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="f8326-342">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-342">Attribute Name</span></span> | <span data-ttu-id="f8326-343">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-343">Is Required</span></span> | <span data-ttu-id="f8326-344">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="f8326-345">Nazwa</span><span class="sxs-lookup"><span data-stu-id="f8326-345">Name</span></span>           | <span data-ttu-id="f8326-346">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-346">Yes</span></span>         | <span data-ttu-id="f8326-347">Nazwa elementu end skojarzenia, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-348">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-348">Example</span></span>

<span data-ttu-id="f8326-349">W poniższym przykładzie przedstawiono **AssociationSetMapping** elementu, w którym **klucza Obcego\_kurs\_działu** skojarzeń w modelu koncepcyjnym jest mapowany na **Kurs** tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="f8326-350">Mapowania między właściwościami typu skojarzenia i kolumn w tabeli są określone w podrzędnej **właściwości EndProperty** elementów.</span><span class="sxs-lookup"><span data-stu-id="f8326-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-351">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-351">Example</span></span>

<span data-ttu-id="f8326-352">W poniższym przykładzie przedstawiono **właściwości EndProperty** mapowania funkcji wstawiania i usuwania skojarzenia elementu (**CourseInstructor**) do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="f8326-353">Funkcje, które są mapowane na są deklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="f8326-354">Element elemencie EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-355">**Elemencie EntityContainerMapping** elementu w specyfikacji języka (MSL) mapowania mapuje kontener jednostek w modelu koncepcyjnym kontener jednostek w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="f8326-356">**Elemencie EntityContainerMapping** element jest elementem podrzędnym elementu mapowania.</span><span class="sxs-lookup"><span data-stu-id="f8326-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="f8326-357">**Elemencie EntityContainerMapping** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="f8326-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f8326-358">Obiekcie EntitySetMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="f8326-359">AssociationSetMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="f8326-360">FunctionImportMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-361">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-361">Applicable Attributes</span></span>

<span data-ttu-id="f8326-362">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **elemencie EntityContainerMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="f8326-363">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-363">Attribute Name</span></span>            | <span data-ttu-id="f8326-364">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-364">Is Required</span></span> | <span data-ttu-id="f8326-365">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="f8326-366">**StorageModelContainer**</span></span> | <span data-ttu-id="f8326-367">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-367">Yes</span></span>         | <span data-ttu-id="f8326-368">Nazwa kontenera magazynu jednostki modelu, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="f8326-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="f8326-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="f8326-370">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-370">Yes</span></span>         | <span data-ttu-id="f8326-371">Nazwa kontenera jednostek modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="f8326-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="f8326-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="f8326-373">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-373">No</span></span>          | <span data-ttu-id="f8326-374">**Wartość true,** lub **False**.</span><span class="sxs-lookup"><span data-stu-id="f8326-374">**True** or **False**.</span></span> <span data-ttu-id="f8326-375">Jeśli **False**, są generowane nie widoków aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="f8326-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="f8326-376">Ten atrybut powinien być ustawiony na **False** przypadku mapowania tylko do odczytu, która będzie nieprawidłowa, ponieważ dane mogą być nie obustronne pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="f8326-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="f8326-377">Wartość domyślna to **True**.</span><span class="sxs-lookup"><span data-stu-id="f8326-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-378">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-378">Example</span></span>

<span data-ttu-id="f8326-379">W poniższym przykładzie przedstawiono **elemencie EntityContainerMapping** element, który mapuje **SchoolModelEntities** container (kontener modelu koncepcyjnego entity) do  **SchoolModelStoreContainer** container (kontener jednostek modelu magazynu):</span><span class="sxs-lookup"><span data-stu-id="f8326-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="f8326-380">Element obiekcie EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-381">**Obiekcie EntitySetMapping** elementu w specyfikacji języka (MSL) mapy wszystkie typy w jednostce modelu koncepcyjnego równa jednostki mapowania zestawów w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="f8326-382">Zestawu jednostek w modelu koncepcyjnego to kontener logiczny dla wystąpień jednostek z tego samego typu (i jego typów pochodnych).</span><span class="sxs-lookup"><span data-stu-id="f8326-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="f8326-383">Zestawu jednostek w modelu magazynu reprezentuje tabelę lub widok w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="f8326-384">Zestaw jednostek modelu koncepcyjnego jest określony przez wartość **nazwa** atrybutu **obiekcie EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="f8326-385">Mapowany do tabeli lub widoku jest określona przez **StoreEntitySet** atrybutu w każdym elemencie MappingFragment podrzędnej lub w **obiekcie EntitySetMapping** samego elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="f8326-386">**Obiekcie EntitySetMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-387">EntityTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f8326-388">Obiekt QueryView (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="f8326-389">MappingFragment (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-390">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-390">Applicable Attributes</span></span>

<span data-ttu-id="f8326-391">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **obiekcie EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="f8326-392">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-392">Attribute Name</span></span>           | <span data-ttu-id="f8326-393">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-393">Is Required</span></span> | <span data-ttu-id="f8326-394">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-395">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-395">**Name**</span></span>                 | <span data-ttu-id="f8326-396">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-396">Yes</span></span>         | <span data-ttu-id="f8326-397">Nazwa zestawu jednostek modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="f8326-398">**Element TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="f8326-398">**TypeName** **1**</span></span>       | <span data-ttu-id="f8326-399">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-399">No</span></span>          | <span data-ttu-id="f8326-400">Nazwa typu jednostki modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="f8326-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="f8326-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="f8326-402">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-402">No</span></span>          | <span data-ttu-id="f8326-403">Nazwa zestawu jednostek modelu magazynu, który jest mapowany na.</span><span class="sxs-lookup"><span data-stu-id="f8326-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="f8326-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="f8326-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="f8326-405">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-405">No</span></span>          | <span data-ttu-id="f8326-406">**Wartość true,** lub **False** w zależności od tego, czy tylko unikatowe wiersze są zwracane.</span><span class="sxs-lookup"><span data-stu-id="f8326-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="f8326-407">Jeśli ten atrybut jest ustawiony na **True**, **GenerateUpdateViews** atrybut elementu w elemencie EntityContainerMapping musi być równa **False**.</span><span class="sxs-lookup"><span data-stu-id="f8326-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="f8326-408">**1** **TypeName** i **StoreEntitySet** atrybuty można zamiast elementów podrzędnych EntityTypeMapping i MappingFragment mapowania typu pojedynczy element na pojedynczą tabelę.</span><span class="sxs-lookup"><span data-stu-id="f8326-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="f8326-409">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-409">Example</span></span>

<span data-ttu-id="f8326-410">W poniższym przykładzie przedstawiono **obiekcie EntitySetMapping** element, który mapuje trzy typy (typ podstawowy i dwa typy pochodne) w **kursów** zestaw jednostek model koncepcyjny do trzech różnych tabel w podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="f8326-411">Tabele są określone przez **StoreEntitySet** atrybutu w każdym **MappingFragment** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="f8326-412">Element EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-413">**EntityTypeMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między typem jednostki w modelu koncepcyjnym i tabel lub widoków w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="f8326-414">Dla informacji na temat typów jednostek modelu koncepcyjnego i podstawowej bazy danych tabel lub widoków Zobacz Element EntityType (CSDL) i elementu obiektu EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="f8326-415">Typ jednostki modelu koncepcyjnego, który jest mapowany jest określony przez **TypeName** atrybutu **EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="f8326-416">Tabela lub widok, który jest mapowany jest określona przez **StoreEntitySet** atrybutu element MappingFragment podrzędny.</span><span class="sxs-lookup"><span data-stu-id="f8326-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="f8326-417">ModificationFunctionMapping, element podrzędny może być używane do mapowania Wstawianie, aktualizowanie lub usuwanie funkcji typów jednostek do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="f8326-418">**EntityTypeMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-419">MappingFragment (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="f8326-420">ModificationFunctionMapping (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="f8326-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-421">ScalarProperty</span></span>
-   <span data-ttu-id="f8326-422">Warunek</span><span class="sxs-lookup"><span data-stu-id="f8326-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-423">**MappingFragment** i **ModificationFunctionMapping** elementy nie mogą być elementami podrzędnymi **EntityTypeMapping** element w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="f8326-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="f8326-424">**ScalarProperty** i **warunek** elementów może zawierać tylko elementy podrzędne **EntityTypeMapping** elementu, gdy jest używany w elemencie FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-425">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-425">Applicable Attributes</span></span>

<span data-ttu-id="f8326-426">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f8326-427">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-427">Attribute Name</span></span> | <span data-ttu-id="f8326-428">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-428">Is Required</span></span> | <span data-ttu-id="f8326-429">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-430">**Nazwa typu**</span><span class="sxs-lookup"><span data-stu-id="f8326-430">**TypeName**</span></span>   | <span data-ttu-id="f8326-431">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-431">Yes</span></span>         | <span data-ttu-id="f8326-432">Przestrzeń nazw kwalifikowana nazwa typu jednostki modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="f8326-433">Jeśli typ jest abstrakcyjny ani typem pochodnym, wartość musi być `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="f8326-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-434">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-434">Example</span></span>

<span data-ttu-id="f8326-435">W poniższym przykładzie pokazano element obiekcie EntitySetMapping z dwóch podrzędnych **EntityTypeMapping** elementów.</span><span class="sxs-lookup"><span data-stu-id="f8326-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="f8326-436">W pierwszym **EntityTypeMapping** elementu **SchoolModel.Person** typ jednostki jest mapowany na **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="f8326-437">W drugim **EntityTypeMapping** element, funkcja aktualizacji z **SchoolModel.Person** typu jest mapowany do procedury składowanej **UpdatePerson**, w bazie danych .</span><span class="sxs-lookup"><span data-stu-id="f8326-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-438">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-438">Example</span></span>

<span data-ttu-id="f8326-439">W kolejnym przykładzie pokazano mapowanie hierarchii typów, w którym główny typ jest abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="f8326-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="f8326-440">Zwróć uwagę na użycie `IsOfType` składnia **TypeName** atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f8326-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="f8326-441">Element FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-442">**FunctionImportMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnego i procedury składowanej lub funkcji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="f8326-443">Importy funkcja musi być zadeklarowana w modelu koncepcyjnym i procedur składowanych musi być zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="f8326-444">Aby uzyskać więcej informacji zobacz Element FunctionImport (CSDL) i funkcji — Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-445">Domyślnie jeśli import funkcja zwraca modelu koncepcyjnego typu jednostki lub typ złożony, następnie nazwy kolumn zwrócone przez procedurę składowaną podstawowej musi dokładnie odpowiadać nazwy właściwości w typie modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="f8326-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="f8326-446">Jeśli nazwy kolumn nie odpowiadają dokładnie nazw właściwości, mapowanie musi być zdefiniowany w elemencie ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="f8326-447">**FunctionImportMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-448">ResultMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-449">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-449">Applicable Attributes</span></span>

<span data-ttu-id="f8326-450">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **FunctionImportMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="f8326-451">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-451">Attribute Name</span></span>         | <span data-ttu-id="f8326-452">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-452">Is Required</span></span> | <span data-ttu-id="f8326-453">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="f8326-454">**FunctionImportName**</span></span> | <span data-ttu-id="f8326-455">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-455">Yes</span></span>         | <span data-ttu-id="f8326-456">Nazwa funkcji importowania w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="f8326-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-457">**FunctionName**</span></span>       | <span data-ttu-id="f8326-458">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-458">Yes</span></span>         | <span data-ttu-id="f8326-459">Nazwa kwalifikowana przez obszar nazw funkcji w modelu magazynu, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-460">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-460">Example</span></span>

<span data-ttu-id="f8326-461">Poniższy przykład jest oparty na modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-461">The following example is based on the School model.</span></span> <span data-ttu-id="f8326-462">Rozważmy następującą funkcję w modelu magazynu:</span><span class="sxs-lookup"><span data-stu-id="f8326-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="f8326-463">Należy również rozważyć importowanie tej funkcji w modelu koncepcyjnym:</span><span class="sxs-lookup"><span data-stu-id="f8326-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="f8326-464">W poniższych pokazano przykład **FunctionImportMapping** element używany do mapowania funkcji i funkcji importu powyżej ze sobą:</span><span class="sxs-lookup"><span data-stu-id="f8326-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="f8326-465">Element InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="f8326-466">**InsertFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji wstawiania w typie jednostki lub skojarzeń w modelu koncepcyjnym procedurę składowaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f8326-467">Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f8326-468">Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-469">Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.</span><span class="sxs-lookup"><span data-stu-id="f8326-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="f8326-470">**InsertFunction** element może być elementem podrzędnym elementu ModificationFunctionMapping i zastosowane do elementu EntityTypeMapping lub AssociationSetMapping element.</span><span class="sxs-lookup"><span data-stu-id="f8326-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="f8326-471">Stosowane do EntityTypeMapping InsertFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="f8326-472">W przypadku zastosowania do elementu EntityTypeMapping **InsertFunction** element mapy typu jednostki w modelu koncepcyjnym funkcji wstawiania do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f8326-473">**InsertFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **EntityTypeMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="f8326-474">Punkt końcowy skojarzenia (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f8326-475">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f8326-476">ResultBinding (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="f8326-477">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f8326-478">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-478">Applicable Attributes</span></span>

<span data-ttu-id="f8326-479">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **InsertFunction** elementu, gdy jest stosowany do **EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f8326-480">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-480">Attribute Name</span></span>            | <span data-ttu-id="f8326-481">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-481">Is Required</span></span> | <span data-ttu-id="f8326-482">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-483">**FunctionName**</span></span>          | <span data-ttu-id="f8326-484">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-484">Yes</span></span>         | <span data-ttu-id="f8326-485">Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji wstawiania.</span><span class="sxs-lookup"><span data-stu-id="f8326-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="f8326-486">Procedura składowana musi być zadeklarowany w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f8326-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="f8326-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f8326-488">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-488">No</span></span>          | <span data-ttu-id="f8326-489">Nazwa parametru output, która zwraca liczbę wierszy dotyczy.</span><span class="sxs-lookup"><span data-stu-id="f8326-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f8326-490">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-490">Example</span></span>

<span data-ttu-id="f8326-491">Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania typu jednostki osoby **InsertPerson** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="f8326-492">**InsertPerson** procedury składowanej jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="f8326-493">Stosowane do AssociationSetMapping InsertFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="f8326-494">W przypadku zastosowania do elementu AssociationSetMapping **InsertFunction** element mapy funkcji wstawiania skojarzenia w modelu koncepcyjnym do procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f8326-495">**InsertFunction** element może mieć następujące elementy podrzędne, po zastosowaniu do **AssociationSetMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="f8326-496">Właściwości EndProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f8326-497">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-497">Applicable Attributes</span></span>

<span data-ttu-id="f8326-498">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **InsertFunction** element, jeśli jest stosowany do **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f8326-499">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-499">Attribute Name</span></span>            | <span data-ttu-id="f8326-500">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-500">Is Required</span></span> | <span data-ttu-id="f8326-501">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-502">**FunctionName**</span></span>          | <span data-ttu-id="f8326-503">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-503">Yes</span></span>         | <span data-ttu-id="f8326-504">Przestrzeń nazw kwalifikowana nazwa procedury składowanej, na który jest mapowany funkcji wstawiania.</span><span class="sxs-lookup"><span data-stu-id="f8326-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="f8326-505">Procedura składowana musi być zadeklarowany w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f8326-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="f8326-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f8326-507">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-507">No</span></span>          | <span data-ttu-id="f8326-508">Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.</span><span class="sxs-lookup"><span data-stu-id="f8326-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f8326-509">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-509">Example</span></span>

<span data-ttu-id="f8326-510">Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania **CourseInstructor** skojarzenia  **InsertCourseInstructor** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="f8326-511">**InsertCourseInstructor** procedury składowanej jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="f8326-512">Mapowanie elementu (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="f8326-513">**Mapowania** element w mapowaniu language specification (MSL) zawiera informacje umożliwiające mapowanie obiektów, które są zdefiniowane w model koncepcyjny do bazy danych (zgodnie z opisem w modelu pamięci masowej).</span><span class="sxs-lookup"><span data-stu-id="f8326-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="f8326-514">Aby uzyskać więcej informacji zobacz specyfikacja CSDL i specyfikacja SSDL.</span><span class="sxs-lookup"><span data-stu-id="f8326-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="f8326-515">**Mapowania** element jest elementem głównym specyfikacji mapowania.</span><span class="sxs-lookup"><span data-stu-id="f8326-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="f8326-516">Przestrzeń nazw XML do mapowania specyfikacje jest http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="f8326-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="f8326-517">Element mapowania może używać następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="f8326-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f8326-518">Alias (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="f8326-519">Elemencie EntityContainerMapping (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="f8326-520">Nazwy koncepcje i typów modelu magazynu, które są określone w pliku MSL musi być kwalifikowana przy użyciu ich nazw odpowiednich przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8326-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f8326-521">Dla informacji o modelu koncepcyjnego nazwa przestrzeni nazw Zobacz Element schematu (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="f8326-522">Informacje o nazwie przestrzeni nazw w modelu magazynu na ten temat można znaleźć w elemencie schematu (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="f8326-523">Aliasy dla przestrzeni nazw, które są używane w pliku MSL można zdefiniować w elemencie aliasu.</span><span class="sxs-lookup"><span data-stu-id="f8326-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-524">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-524">Applicable Attributes</span></span>

<span data-ttu-id="f8326-525">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **mapowanie** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="f8326-526">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-526">Attribute Name</span></span> | <span data-ttu-id="f8326-527">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-527">Is Required</span></span> | <span data-ttu-id="f8326-528">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="f8326-529">**Miejsce**</span><span class="sxs-lookup"><span data-stu-id="f8326-529">**Space**</span></span>      | <span data-ttu-id="f8326-530">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-530">Yes</span></span>         | <span data-ttu-id="f8326-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="f8326-531">**C-S**.</span></span> <span data-ttu-id="f8326-532">To jest wartość stałą i nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="f8326-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-533">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-533">Example</span></span>

<span data-ttu-id="f8326-534">W poniższym przykładzie przedstawiono **mapowanie** element, który opiera się na części modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="f8326-535">Aby uzyskać więcej informacji na temat modelu szkoły zobacz Przewodnik Szybki Start (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="f8326-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="f8326-536">Element MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="f8326-537">**MappingFragment** element w mapowaniu language specification (MSL) definiuje mapowanie między właściwościami typu jednostki modelu koncepcyjnego i tabeli lub widoku w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="f8326-538">Dla informacji na temat typów jednostek modelu koncepcyjnego i podstawowej bazy danych tabel lub widoków Zobacz Element EntityType (CSDL) i elementu obiektu EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="f8326-539">**MappingFragment** może być elementem podrzędnym elementu EntityTypeMapping lub element w obiekcie EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="f8326-540">**MappingFragment** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-541">ComplexType (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="f8326-542">ScalarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="f8326-543">Warunek (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-544">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-544">Applicable Attributes</span></span>

<span data-ttu-id="f8326-545">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **MappingFragment** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="f8326-546">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-546">Attribute Name</span></span>          | <span data-ttu-id="f8326-547">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-547">Is Required</span></span> | <span data-ttu-id="f8326-548">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="f8326-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="f8326-550">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-550">Yes</span></span>         | <span data-ttu-id="f8326-551">Nazwa tabeli lub widoku, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="f8326-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="f8326-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="f8326-553">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-553">No</span></span>          | <span data-ttu-id="f8326-554">**Wartość true,** lub **False** w zależności od tego, czy tylko unikatowe wiersze są zwracane.</span><span class="sxs-lookup"><span data-stu-id="f8326-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="f8326-555">Jeśli ten atrybut jest ustawiony na **True**, **GenerateUpdateViews** atrybut elementu w elemencie EntityContainerMapping musi być równa **False**.</span><span class="sxs-lookup"><span data-stu-id="f8326-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-556">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-556">Example</span></span>

<span data-ttu-id="f8326-557">W poniższym przykładzie przedstawiono **MappingFragment** element jako element podrzędny **EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="f8326-558">W tym przykładzie właściwości **kurs** typu w modelu koncepcyjnym są mapowane na kolumny **kurs** tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-559">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-559">Example</span></span>

<span data-ttu-id="f8326-560">W poniższym przykładzie przedstawiono **MappingFragment** element jako element podrzędny **obiekcie EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="f8326-561">Tak jak w przykładzie powyżej właściwości **kurs** typu w modelu koncepcyjnym są mapowane na kolumny **kurs** tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="f8326-562">Element ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-563">**ModificationFunctionMapping** elementu w specyfikacji języka (MSL) mapowania mapuje insert, aktualizowanie i usuwanie funkcji typu modelu koncepcyjnego entity do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="f8326-564">**ModificationFunctionMapping** element można również mapować insert i usuwanie funkcji wiele do wielu skojarzeń w modelu koncepcyjnym do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="f8326-565">Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f8326-566">Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-567">Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.</span><span class="sxs-lookup"><span data-stu-id="f8326-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="f8326-568">W przypadku funkcji modyfikacji dla jednej jednostki w hierarchii dziedziczenia są mapowane do procedur składowanych, funkcji modyfikacji dla wszystkich typów w hierarchii musi być zamapowany na procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="f8326-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="f8326-569">**ModificationFunctionMapping** element może być elementem podrzędnym elementu EntityTypeMapping lub AssociationSetMapping element.</span><span class="sxs-lookup"><span data-stu-id="f8326-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="f8326-570">**ModificationFunctionMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-571">DeleteFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="f8326-572">InsertFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="f8326-573">UpdateFunction (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="f8326-574">Atrybuty nie są stosowane do **ModificationFunctionMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="f8326-575">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-575">Example</span></span>

<span data-ttu-id="f8326-576">W poniższym przykładzie przedstawiono zestaw mapowanie dla jednostek **osób** zestawu jednostek w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="f8326-577">Oprócz mapowania kolumny **osoby** typu jednostki, mapowanie Wstawianie, aktualizowanie i usuwanie funkcji **osoby** typu są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="f8326-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="f8326-578">Funkcje, które są mapowane na są deklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-579">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-579">Example</span></span>

<span data-ttu-id="f8326-580">W poniższym przykładzie pokazano skojarzenia mapowanie dla zestawu **CourseInstructor** skojarzeń w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="f8326-581">Oprócz mapowania kolumny **CourseInstructor** skojarzenia, mapowanie funkcje insert i delete **CourseInstructor** skojarzeń są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="f8326-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="f8326-582">Funkcje, które są mapowane na są deklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="f8326-583">Obiekt QueryView — Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="f8326-584">**QueryView** elementu w mapowaniu language specification (MSL) definiuje tylko do odczytu mapowanie między typie jednostki lub skojarzeń w modelu koncepcyjnego i tabelę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="f8326-585">Mapowanie jest zdefiniowana za pomocą zapytanie SQL jednostki, które zostaną ocenione względem model magazynu i express zestawu pod względem jednostki lub skojarzeń w modelu koncepcyjnym wyników.</span><span class="sxs-lookup"><span data-stu-id="f8326-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="f8326-586">Ponieważ widoków zapytań są tylko do odczytu, nie możesz użyć polecenia standardowych aktualizacji można zaktualizować typów, które są definiowane przez widoki kwerendę.</span><span class="sxs-lookup"><span data-stu-id="f8326-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="f8326-587">Należy tworzyć aktualizacje do tych typów, przy użyciu funkcji modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f8326-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="f8326-588">Aby uzyskać więcej informacji, zobacz jak: funkcje modyfikacji mapy do procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="f8326-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-589">W **QueryView** element, wyrażenia jednostki SQL, które zawierają **GroupBy**, agregacje grupy lub właściwości nawigacji nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f8326-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="f8326-590">**QueryView** element może być elementem podrzędnym elementu obiekcie EntitySetMapping lub AssociationSetMapping elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="f8326-591">W pierwszym przypadku widok zapytania definiuje mapowanie tylko do odczytu dla jednostki w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="f8326-592">W tym ostatnim przypadku widok zapytania definiuje mapowanie tylko do odczytu dla skojarzenia w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-593">Jeśli **AssociationSetMapping** element jest skojarzenie z ograniczeniem referencyjnym, **AssociationSetMapping** element jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="f8326-594">Aby uzyskać więcej informacji zobacz Element ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="f8326-595">**QueryView** elementu nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f8326-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-596">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-596">Applicable Attributes</span></span>

<span data-ttu-id="f8326-597">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **QueryView** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="f8326-598">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-598">Attribute Name</span></span> | <span data-ttu-id="f8326-599">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-599">Is Required</span></span> | <span data-ttu-id="f8326-600">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-601">**Nazwa typu**</span><span class="sxs-lookup"><span data-stu-id="f8326-601">**TypeName**</span></span>   | <span data-ttu-id="f8326-602">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-602">No</span></span>          | <span data-ttu-id="f8326-603">Nazwa typu modelu koncepcyjnego, który jest mapowany w widoku zapytania.</span><span class="sxs-lookup"><span data-stu-id="f8326-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-604">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-604">Example</span></span>

<span data-ttu-id="f8326-605">W poniższym przykładzie przedstawiono **QueryView** element jako element podrzędny elementu **obiekcie EntitySetMapping** elementu i definiuje mapowanie widoku zapytania **działu** typu jednostki w Model szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="f8326-606">Ponieważ zapytanie zwraca tylko podzestaw elementów członkowskich **działu** typu w modelu magazynu **działu** typu w modelu szkoły została zmodyfikowana oparte na tym mapowanie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f8326-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-607">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-607">Example</span></span>

<span data-ttu-id="f8326-608">W następnym przykładzie pokazano **QueryView** element jako element podrzędny **AssociationSetMapping** elementu i definiuje mapowanie tylko do odczytu dla `FK_Course_Department` skojarzeń w modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="f8326-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="f8326-609">Komentarze</span><span class="sxs-lookup"><span data-stu-id="f8326-609">Comments</span></span>

<span data-ttu-id="f8326-610">Można zdefiniować widoki kwerendę w następujących scenariuszach:</span><span class="sxs-lookup"><span data-stu-id="f8326-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="f8326-611">Zdefiniuj jednostki w modelu koncepcyjnym, który nie zawiera wszystkich właściwości jednostki w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="f8326-612">Obejmuje to właściwości, które nie mają przypisane wartości domyślne, a nie obsługują **null** wartości.</span><span class="sxs-lookup"><span data-stu-id="f8326-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="f8326-613">Mapowanie kolumn obliczanych w modelu magazynu do właściwości typów jednostek w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="f8326-614">Zdefiniuj mapowania, gdzie warunków używanych do jednostek w partycji w modelu koncepcyjnym nie są oparte na równości.</span><span class="sxs-lookup"><span data-stu-id="f8326-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="f8326-615">Po określeniu warunkowego mapowania przy użyciu **warunek** elementu podanym warunku musi być równa określonej wartości.</span><span class="sxs-lookup"><span data-stu-id="f8326-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="f8326-616">Aby uzyskać więcej informacji zobacz Element warunek (MSL).</span><span class="sxs-lookup"><span data-stu-id="f8326-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="f8326-617">Tej samej kolumny w modelu magazynu są mapowane na wiele typów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="f8326-618">Wiele typów są mapowane na tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="f8326-619">Definiowanie skojarzeń w modelu koncepcyjnym, które nie są oparte na klucze obce w relacyjnej schematu.</span><span class="sxs-lookup"><span data-stu-id="f8326-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="f8326-620">Użyj niestandardowej logiki biznesowej do ustawiania wartości właściwości w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="f8326-621">Na przykład można mapować wartość ciągu "T" w źródle danych na wartość **true**, wartość logiczna, w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="f8326-622">Zdefiniuj filtry warunkowe dla wyników zapytań.</span><span class="sxs-lookup"><span data-stu-id="f8326-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="f8326-623">Wymuszanie mniej ograniczeń danych w modelu koncepcyjnym niż w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="f8326-624">Na przykład można wprowadzone właściwości w modelu koncepcyjnym dopuszczającego wartość null, nawet jeśli nie obsługuje kolumn, na który jest mapowany **null**wartości.</span><span class="sxs-lookup"><span data-stu-id="f8326-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="f8326-625">Podczas definiowania widoków zapytań dla jednostek obowiązują następujące zastrzeżenia:</span><span class="sxs-lookup"><span data-stu-id="f8326-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="f8326-626">Widoki kwerendę są tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="f8326-626">Query views are read-only.</span></span> <span data-ttu-id="f8326-627">Można tworzyć tylko aktualizacje do jednostek przy użyciu funkcji modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f8326-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="f8326-628">Po zdefiniowaniu typu jednostki, widok zapytania można również definiować wszystkie powiązane jednostki przez widoki kwerendę.</span><span class="sxs-lookup"><span data-stu-id="f8326-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="f8326-629">Kiedy mapujesz skojarzenia typu wiele do wielu z jednostką w modelu magazynu, który reprezentuje tabelę łącze w schemacie relacyjnych należy zdefiniować **QueryView** element **AssociationSetMapping** element do tej tabeli łącza.</span><span class="sxs-lookup"><span data-stu-id="f8326-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="f8326-630">Widoki kwerendę muszą być zdefiniowane dla wszystkich typów w hierarchii typów.</span><span class="sxs-lookup"><span data-stu-id="f8326-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="f8326-631">Można to zrobić w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f8326-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="f8326-632">Za pomocą jednego **QueryView** element, który określa pojedyncze zapytanie jednostki SQL, które zwraca sumę wszystkich typów jednostek w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f8326-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="f8326-633">Za pomocą jednego **QueryView** element, który określa pojedyncze zapytanie jednostki SQL, które używa operatora wielkości liter do zwrócenia określonego typu w hierarchii na podstawie określonego warunku.</span><span class="sxs-lookup"><span data-stu-id="f8326-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="f8326-634">Przy użyciu dodatkowego **QueryView** elementu dla określonego typu w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f8326-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="f8326-635">W takim przypadku należy użyć **TypeName** atrybutu **QueryView** elementu, aby określić typ jednostki dla każdego widoku.</span><span class="sxs-lookup"><span data-stu-id="f8326-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="f8326-636">Po zdefiniowaniu widok zapytania nie można określić **StorageSetName** atrybutu na **obiekcie EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="f8326-637">Po zdefiniowaniu widok zapytania **obiekcie EntitySetMapping**element nie może również zawierać **właściwość** mapowania.</span><span class="sxs-lookup"><span data-stu-id="f8326-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="f8326-638">Element ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="f8326-639">**ResultBinding** elementu w specyfikacji języka (MSL) mapowania mapuje wartości kolumn, które są zwracane przez procedury składowane do właściwości jednostki w modelu koncepcyjnym w przypadku funkcji modyfikacji typu jednostki są mapowane do przechowywanej procedury przedstawione w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="f8326-640">Na przykład, kiedy wartość w kolumnie tożsamości jest zwracany przez wstawiania procedurą składowaną, **ResultBinding** element mapuje zwracanej wartości do właściwości typu jednostki w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f8326-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="f8326-641">**ResultBinding** element może być element podrzędny elementu InsertFunction lub UpdateFunction element.</span><span class="sxs-lookup"><span data-stu-id="f8326-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="f8326-642">**ResultBinding** elementu nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f8326-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-643">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-643">Applicable Attributes</span></span>

<span data-ttu-id="f8326-644">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ResultBinding** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="f8326-645">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-645">Attribute Name</span></span> | <span data-ttu-id="f8326-646">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-646">Is Required</span></span> | <span data-ttu-id="f8326-647">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-648">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-648">**Name**</span></span>       | <span data-ttu-id="f8326-649">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-649">Yes</span></span>         | <span data-ttu-id="f8326-650">Nazwa właściwości jednostki w modelu koncepcyjnym, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="f8326-651">**NazwaKolumny**</span><span class="sxs-lookup"><span data-stu-id="f8326-651">**ColumnName**</span></span> | <span data-ttu-id="f8326-652">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-652">Yes</span></span>         | <span data-ttu-id="f8326-653">Nazwa kolumny mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="f8326-654">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-654">Example</span></span>

<span data-ttu-id="f8326-655">Poniższy przykład jest oparty na modelu szkoły i pokazuje **InsertFunction** element używany do mapowania funkcji wstawiania **osoby** typu jednostki **InsertPerson** Procedura składowana.</span><span class="sxs-lookup"><span data-stu-id="f8326-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="f8326-656">( **InsertPerson** procedury składowanej jest pokazany poniżej i jest zadeklarowana w modelu magazynu.) A **ResultBinding** element jest używany do mapowania wartości kolumny, która jest zwracana przez procedurę składowaną (**NewPersonID**) z właściwością typu jednostki (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="f8326-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="f8326-657">W tym artykule opisano języka Transact-SQL **InsertPerson** procedura składowana:</span><span class="sxs-lookup"><span data-stu-id="f8326-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="f8326-658">Element ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="f8326-659">**ResultMapping** elementu w mapowaniu language specification (MSL) definiuje mapowanie między importowanie funkcji, w modelu koncepcyjnym i procedurę składowaną w bazie danych, gdy spełnione są następujące:</span><span class="sxs-lookup"><span data-stu-id="f8326-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="f8326-660">Importowanie funkcji zwraca modelu koncepcyjnego typu jednostki lub typ złożony.</span><span class="sxs-lookup"><span data-stu-id="f8326-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="f8326-661">Nazwy kolumn zwrócone przez procedurę składowaną nie odpowiadają dokładnie nazwy właściwości w typie jednostki lub typ złożony.</span><span class="sxs-lookup"><span data-stu-id="f8326-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="f8326-662">Domyślnie mapowania między kolumnami zwrócone przez procedurę składowaną i typie jednostki lub typ złożony opiera się na nazwy kolumn i właściwości.</span><span class="sxs-lookup"><span data-stu-id="f8326-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="f8326-663">Nazwy kolumn nie odpowiadają dokładnie nazw właściwości, należy użyć **ResultMapping** elementu, aby zdefiniować mapowanie.</span><span class="sxs-lookup"><span data-stu-id="f8326-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="f8326-664">Aby uzyskać przykład domyślnego mapowania Zobacz Element FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="f8326-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="f8326-665">**ResultMapping** element jest elementem podrzędnym elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="f8326-666">**ResultMapping** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-667">EntityTypeMapping (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f8326-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f8326-668">ComplexTypeMapping</span></span>

<span data-ttu-id="f8326-669">Atrybuty nie są stosowane do **ResultMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="f8326-670">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-670">Example</span></span>

<span data-ttu-id="f8326-671">Należy wziąć pod uwagę następujące procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="f8326-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="f8326-672">Ponadto należy wziąć pod uwagę następujące typie modelu koncepcyjnego entity:</span><span class="sxs-lookup"><span data-stu-id="f8326-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="f8326-673">Aby można było utworzyć importu funkcji, który zwraca wystąpienia poprzedni typ jednostki, mapowanie między kolumnami zwrócone przez procedurę składowaną i typ jednostki musi być zdefiniowany w **ResultMapping** elementu:</span><span class="sxs-lookup"><span data-stu-id="f8326-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="f8326-674">Element ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="f8326-675">**ScalarProperty** elementu w specyfikacji języka (MSL) mapowania mapuje właściwość typu jednostki modelu koncepcyjnego, typu złożonego lub skojarzenie tabeli kolumna lub parametr procedurę składowaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="f8326-676">Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f8326-677">Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="f8326-678">**ScalarProperty** element może być elementem podrzędnym następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="f8326-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="f8326-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="f8326-679">MappingFragment</span></span>
-   <span data-ttu-id="f8326-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-680">InsertFunction</span></span>
-   <span data-ttu-id="f8326-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-681">UpdateFunction</span></span>
-   <span data-ttu-id="f8326-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="f8326-682">DeleteFunction</span></span>
-   <span data-ttu-id="f8326-683">Właściwości EndProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-683">EndProperty</span></span>
-   <span data-ttu-id="f8326-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="f8326-684">ComplexProperty</span></span>
-   <span data-ttu-id="f8326-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="f8326-685">ResultMapping</span></span>

<span data-ttu-id="f8326-686">Jako element podrzędny elementu **MappingFragment**, **ComplexProperty**, lub **właściwości EndProperty** elementu **ScalarProperty** element mapy właściwości w modelu koncepcyjnym z kolumną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="f8326-687">Jako element podrzędny elementu **InsertFunction**, **UpdateFunction**, lub **DeleteFunction** elementu **ScalarProperty** element mapy właściwości w modelu koncepcyjnym do parametru procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="f8326-688">**ScalarProperty** elementu nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="f8326-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-689">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-689">Applicable Attributes</span></span>

<span data-ttu-id="f8326-690">Atrybuty, które są stosowane do **ScalarProperty** elementu różnią się w zależności od roli tego elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="f8326-691">W poniższej tabeli opisano atrybuty, które są stosowane, kiedy **ScalarProperty** element jest używany do mapowania właściwości modelu koncepcyjnego z kolumną w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="f8326-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="f8326-692">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-692">Attribute Name</span></span> | <span data-ttu-id="f8326-693">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-693">Is Required</span></span> | <span data-ttu-id="f8326-694">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="f8326-695">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-695">**Name**</span></span>       | <span data-ttu-id="f8326-696">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-696">Yes</span></span>         | <span data-ttu-id="f8326-697">Nazwa właściwości modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="f8326-698">**NazwaKolumny**</span><span class="sxs-lookup"><span data-stu-id="f8326-698">**ColumnName**</span></span> | <span data-ttu-id="f8326-699">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-699">Yes</span></span>         | <span data-ttu-id="f8326-700">Nazwa kolumny tabeli, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="f8326-701">W poniższej tabeli opisano atrybuty, które mają zastosowanie do **ScalarProperty** elementu, gdy jest używany do mapowania właściwości modelu koncepcyjnego parametru procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="f8326-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="f8326-702">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-702">Attribute Name</span></span>    | <span data-ttu-id="f8326-703">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-703">Is Required</span></span> | <span data-ttu-id="f8326-704">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-705">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="f8326-705">**Name**</span></span>          | <span data-ttu-id="f8326-706">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-706">Yes</span></span>         | <span data-ttu-id="f8326-707">Nazwa właściwości modelu koncepcyjnego, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="f8326-708">**Nazwa parametru**</span><span class="sxs-lookup"><span data-stu-id="f8326-708">**ParameterName**</span></span> | <span data-ttu-id="f8326-709">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-709">Yes</span></span>         | <span data-ttu-id="f8326-710">Nazwa parametru, który jest mapowany.</span><span class="sxs-lookup"><span data-stu-id="f8326-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="f8326-711">**Wersja**</span><span class="sxs-lookup"><span data-stu-id="f8326-711">**Version**</span></span>       | <span data-ttu-id="f8326-712">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-712">No</span></span>          | <span data-ttu-id="f8326-713">**Bieżący** lub **oryginalnego** w zależności od tego, czy bieżąca wartość lub oryginalnej wartości właściwości powinny być używane do kontroli współbieżności.</span><span class="sxs-lookup"><span data-stu-id="f8326-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="f8326-714">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-714">Example</span></span>

<span data-ttu-id="f8326-715">W poniższym przykładzie przedstawiono **ScalarProperty** element używany na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="f8326-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="f8326-716">Do mapowania właściwości **osoby** typ jednostki do kolumn **osoby**tabeli.</span><span class="sxs-lookup"><span data-stu-id="f8326-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="f8326-717">Do mapowania właściwości **osoby** typu jednostki parametrom **UpdatePerson** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="f8326-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="f8326-718">Procedury składowane są deklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="f8326-719">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-719">Example</span></span>

<span data-ttu-id="f8326-720">W następnym przykładzie pokazano **ScalarProperty** element używany do mapowania Wstawianie i usuwanie funkcji skojarzenia model koncepcyjny do procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="f8326-721">Procedury składowane są deklarowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="f8326-722">Element UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f8326-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="f8326-723">**UpdateFunction** elementu w specyfikacji języka (MSL) mapowania mapuje funkcji aktualizacji typu jednostki w modelu koncepcyjnym procedurę składowaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8326-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f8326-724">Musi być zadeklarowany procedur składowanych do modyfikacji, które funkcje są mapowane w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f8326-725">Aby uzyskać więcej informacji zobacz Element — funkcja (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f8326-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="f8326-726">Jeśli nie mapują wszystkie trzy insert, update lub delete operacje typu jednostki, do procedur składowanych, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane w czasie wykonywania, i jest generowany, UpdateException.</span><span class="sxs-lookup"><span data-stu-id="f8326-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="f8326-727">**UpdateFunction** element może być elementem podrzędnym elementu ModificationFunctionMapping i stosowane do elementu EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="f8326-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="f8326-728">**UpdateFunction** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="f8326-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="f8326-729">Punkt końcowy skojarzenia (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f8326-730">ComplexProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f8326-731">ResultBinding (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="f8326-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="f8326-732">ScarlarProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="f8326-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f8326-733">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="f8326-733">Applicable Attributes</span></span>

<span data-ttu-id="f8326-734">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **UpdateFunction** elementu.</span><span class="sxs-lookup"><span data-stu-id="f8326-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="f8326-735">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="f8326-735">Attribute Name</span></span>            | <span data-ttu-id="f8326-736">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="f8326-736">Is Required</span></span> | <span data-ttu-id="f8326-737">Wartość</span><span class="sxs-lookup"><span data-stu-id="f8326-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f8326-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f8326-738">**FunctionName**</span></span>          | <span data-ttu-id="f8326-739">Tak</span><span class="sxs-lookup"><span data-stu-id="f8326-739">Yes</span></span>         | <span data-ttu-id="f8326-740">Przestrzeń nazw kwalifikowana nazwa procedury składowanej zmapowany funkcji update.</span><span class="sxs-lookup"><span data-stu-id="f8326-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="f8326-741">Procedura składowana musi być zadeklarowany w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f8326-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="f8326-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f8326-743">Nie</span><span class="sxs-lookup"><span data-stu-id="f8326-743">No</span></span>          | <span data-ttu-id="f8326-744">Nazwa parametru output, która zwraca liczbę uwzględnionych wierszy.</span><span class="sxs-lookup"><span data-stu-id="f8326-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="f8326-745">Przykład</span><span class="sxs-lookup"><span data-stu-id="f8326-745">Example</span></span>

<span data-ttu-id="f8326-746">Poniższy przykład jest oparty na modelu szkoły i pokazuje **UpdateFunction** element używany do mapowania funkcji aktualizacji **osoby** typu jednostki **UpdatePerson** Procedura składowana.</span><span class="sxs-lookup"><span data-stu-id="f8326-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="f8326-747">**UpdatePerson** procedury składowanej jest zadeklarowana w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="f8326-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
