---
title: Specyfikacja SSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490353"
---
# <a name="ssdl-specification"></a><span data-ttu-id="9cce7-102">Specyfikacja SSDL</span><span class="sxs-lookup"><span data-stu-id="9cce7-102">SSDL Specification</span></span>
<span data-ttu-id="9cce7-103">Język definicji schematu Store (SSDL) to oparty na standardzie XML język, który opisuje model magazynu w aplikacji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cce7-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="9cce7-104">W aplikacji Entity Framework metadanych modelu magazynu jest ładowany z pliku ssdl (napisanego w SSDL) do wystąpienia System.Data.Metadata.Edm.StoreItemCollection i są dostępne za pomocą metody Klasa System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="9cce7-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="9cce7-105">Entity Framework używa magazynu metadanych modelu do przekształcania zapytania względem modelu koncepcyjnego polecenia specyficzne dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="9cce7-106">Entity Framework Designer (Projektant EF) przechowuje informacje o modelu magazynu w pliku edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="9cce7-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="9cce7-107">W czasie kompilacji w Projektancie jednostki używa informacji w pliku edmx można utworzyć pliku ssdl, które są wymagane przez Entity Framework w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9cce7-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="9cce7-108">Wersje SSDL są zróżnicowane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="9cce7-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="9cce7-109">Wersja SSDL</span><span class="sxs-lookup"><span data-stu-id="9cce7-109">SSDL Version</span></span> | <span data-ttu-id="9cce7-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="9cce7-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="9cce7-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="9cce7-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="9cce7-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="9cce7-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="9cce7-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="9cce7-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="9cce7-114">Elementu Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-114">Association Element (SSDL)</span></span>

<span data-ttu-id="9cce7-115">**Skojarzenia** element język definicji schematu magazynu (SSDL) określa kolumny tabeli, które uczestniczą w ograniczenie klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="9cce7-116">Dwa elementy End wymagany element podrzędny Określ tabele końców skojarzenie i liczebność na każdym końcu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="9cce7-117">Opcjonalny element ReferentialConstraint określa głównym i zależnym końcach asocjacji, a także kolumn uczestniczących w programie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="9cce7-118">Jeśli nie **ReferentialConstraint** elementu, AssociationSetMapping element może służyć do określania mapowań kolumn dla skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="9cce7-119">**Skojarzenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-120">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-121">Końcowy (dokładnie dwa)</span><span class="sxs-lookup"><span data-stu-id="9cce7-121">End (exactly two)</span></span>
-   <span data-ttu-id="9cce7-122">ReferentialConstraint (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="9cce7-123">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-124">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-124">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-125">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="9cce7-126">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-126">Attribute Name</span></span> | <span data-ttu-id="9cce7-127">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-127">Is Required</span></span> | <span data-ttu-id="9cce7-128">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-129">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-129">**Name**</span></span>       | <span data-ttu-id="9cce7-130">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-130">Yes</span></span>         | <span data-ttu-id="9cce7-131">Nazwa odpowiedniego ograniczenie klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-132">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="9cce7-133">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-134">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-135">Example</span></span>

<span data-ttu-id="9cce7-136">W poniższym przykładzie przedstawiono **skojarzenia** element, który używa **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders**  ograniczenie klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="9cce7-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="9cce7-137">Obiekt AssociationSet — Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="9cce7-138">**AssociationSet** element język definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego między dwiema tabelami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="9cce7-139">Kolumny tabeli, które uczestniczą w ograniczenie klucza obcego są określone w elemencie Association.</span><span class="sxs-lookup"><span data-stu-id="9cce7-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="9cce7-140">**Skojarzenia** element, który odpowiada danej **AssociationSet** element jest określony w **skojarzenia** atrybutu **AssociationSet**  elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="9cce7-141">SSDL zestawów skojarzeń są mapowane do zestawów skojarzeń CSDL przez AssociationSetMapping element.</span><span class="sxs-lookup"><span data-stu-id="9cce7-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="9cce7-142">Jednak jeśli zestawu skojarzeń CSDL dla danego skojarzenia CSDL jest zdefiniowana za pomocą elementu ReferentialConstraint bez odpowiedniej **AssociationSetMapping** elementu jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="9cce7-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="9cce7-143">W takim przypadku **AssociationSetMapping** elementu, zostanie ona zastąpiona mapowania definiuje **ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="9cce7-144">**AssociationSet** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-145">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-146">END (zero lub dwóch)</span><span class="sxs-lookup"><span data-stu-id="9cce7-146">End (zero or two)</span></span>
-   <span data-ttu-id="9cce7-147">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-148">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-148">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-149">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="9cce7-150">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-150">Attribute Name</span></span>  | <span data-ttu-id="9cce7-151">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-151">Is Required</span></span> | <span data-ttu-id="9cce7-152">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-153">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-153">**Name**</span></span>        | <span data-ttu-id="9cce7-154">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-154">Yes</span></span>         | <span data-ttu-id="9cce7-155">Nazwa ograniczenia klucza obcego, że skojarzenie ustawione reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="9cce7-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="9cce7-156">**Skojarzenie**</span><span class="sxs-lookup"><span data-stu-id="9cce7-156">**Association**</span></span> | <span data-ttu-id="9cce7-157">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-157">Yes</span></span>         | <span data-ttu-id="9cce7-158">Nazwa skojarzenia, który definiuje kolumny, które uczestniczą w ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-159">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="9cce7-160">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-161">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-162">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-162">Example</span></span>

<span data-ttu-id="9cce7-163">W poniższym przykładzie przedstawiono **AssociationSet** elementu, który reprezentuje `FK_CustomerOrders` ograniczenie klucza obcego w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="9cce7-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="9cce7-164">Element CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="9cce7-165">**CollectionType** element język definicji schematu magazynu (SSDL) określa, że typ zwracany przez funkcję jest kolekcją.</span><span class="sxs-lookup"><span data-stu-id="9cce7-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="9cce7-166">**CollectionType** element jest elementem podrzędnym elementu ReturnType.</span><span class="sxs-lookup"><span data-stu-id="9cce7-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="9cce7-167">Typ kolekcji jest określony za pomocą elementu podrzędnego RowType:</span><span class="sxs-lookup"><span data-stu-id="9cce7-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="9cce7-168">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="9cce7-169">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-170">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-171">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-171">Example</span></span>

<span data-ttu-id="9cce7-172">W poniższym przykładzie pokazano funkcję, która używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy.</span><span class="sxs-lookup"><span data-stu-id="9cce7-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="9cce7-173">Element CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="9cce7-174">**CommandText** element w język definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu funkcji, która pozwala na zdefiniowanie instrukcji SQL, który jest wykonywany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="9cce7-175">**CommandText** elementu pozwala na dodawanie funkcji, która jest podobna do procedury składowanej w bazie danych, ale należy zdefiniować **CommandText** elementu w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="9cce7-176">**CommandText** elementu nie może mieć elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="9cce7-177">Treść **CommandText** element musi być prawidłową instrukcję SQL do podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="9cce7-178">Atrybuty nie są stosowane do **CommandText** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-179">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-179">Example</span></span>

<span data-ttu-id="9cce7-180">W poniższym przykładzie przedstawiono **funkcja** element z element podrzędny **CommandText** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="9cce7-181">Udostępnianie **UpdateProductInOrder** działać jako metodę dla obiektu ObjectContext importując go do modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="9cce7-182">Element DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="9cce7-183">**DefiningQuery** element język definicji schematu magazynu (SSDL) pozwala na wykonanie instrukcji SQL bezpośrednio w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="9cce7-184">**DefiningQuery** element jest najczęściej używany jak widok bazy danych, ale widok jest zdefiniowany w modelu pamięci masowej zamiast bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="9cce7-185">Widok zdefiniowany w **DefiningQuery** elementu mogą być mapowane na typ jednostki w modelu koncepcyjnym za pośrednictwem elementu obiekcie EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="9cce7-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="9cce7-186">Te mapowania są przeznaczone tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-186">These mappings are read-only.</span></span>  

<span data-ttu-id="9cce7-187">Poniższa składnia SSDL pokazuje deklaracji **EntitySet** następuje **DefiningQuery** element, który zawiera zapytanie służy do pobierania tego widoku.</span><span class="sxs-lookup"><span data-stu-id="9cce7-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="9cce7-188">Aby włączyć scenariuszach odczytu i zapisu za pośrednictwem widoków, można użyć procedur składowanych platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cce7-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="9cce7-189">Widok źródła danych lub widoku SQL jednostki można użyć jako tabeli podstawowej dla pobierania danych i przetwarzania przez procedury składowane zmiany.</span><span class="sxs-lookup"><span data-stu-id="9cce7-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="9cce7-190">Możesz użyć **DefiningQuery** elementu docelowego programu Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="9cce7-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="9cce7-191">Chociaż program SQL Server Compact 3.5 nie obsługuje procedur przechowywanych, można zaimplementować podobne funkcje za pomocą **DefiningQuery** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="9cce7-192">Jest innym miejscu, gdzie mogą być przydatne podczas tworzenia procedur składowanych do pokonania niezgodność typów danych, używany w języku programowania i te źródła danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="9cce7-193">Można napisać **DefiningQuery** które pobiera zestaw parametrów, a następnie wywołuje procedurę składowaną z innym zestawem parametrów, na przykład procedury przechowywanej, która powoduje usunięcie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="9cce7-194">Element zależne (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="9cce7-195">**Zależne** element język definicji schematu magazynu (SSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje zależne koniec ograniczenie klucza obcego (nazywany także ograniczenie referencyjne).</span><span class="sxs-lookup"><span data-stu-id="9cce7-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="9cce7-196">**Zależne** element określa kolumny (lub kolumny) w tabeli, która będzie odwoływać się do kolumny klucza podstawowego (lub kolumny).</span><span class="sxs-lookup"><span data-stu-id="9cce7-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="9cce7-197">**PropertyRef** elementy Określ kolumny, które są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="9cce7-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="9cce7-198">Główny element określa kolumny klucza podstawowego, które są przywoływane przez kolumn, które są określone w **zależne** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="9cce7-199">**Zależne** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-200">PropertyRef (jeden lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9cce7-201">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-202">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-202">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-203">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zależne** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="9cce7-204">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-204">Attribute Name</span></span> | <span data-ttu-id="9cce7-205">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-205">Is Required</span></span> | <span data-ttu-id="9cce7-206">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-207">**Rola**</span><span class="sxs-lookup"><span data-stu-id="9cce7-207">**Role**</span></span>       | <span data-ttu-id="9cce7-208">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-208">Yes</span></span>         | <span data-ttu-id="9cce7-209">Taką samą wartość jak **roli** atrybut odpowiedni element End (jeśli jest używana); w przeciwnym razie nazwę tabeli, która zawiera kolumna źródłowa odwołania.</span><span class="sxs-lookup"><span data-stu-id="9cce7-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-210">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zależne** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="9cce7-211">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-212">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-213">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-213">Example</span></span>

<span data-ttu-id="9cce7-214">W poniższym przykładzie pokazano element Association używający **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders** klucza obcego ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9cce7-215">**Zależne** element Określa **CustomerId** kolumny **kolejności** tabeli jako zależne koniec tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="9cce7-216">Element documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="9cce7-217">**Dokumentacji** element język definicji schematu magazynu (SSDL) może służyć do Podaj informacje dotyczące obiektu, który jest zdefiniowany w elemencie rodzica.</span><span class="sxs-lookup"><span data-stu-id="9cce7-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="9cce7-218">**Dokumentacji** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-219">**Podsumowanie**: Krótki opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="9cce7-220">(element zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-220">(zero or one element)</span></span>
-   <span data-ttu-id="9cce7-221">**LongDescription**: rozbudowany opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="9cce7-222">(element zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-223">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-223">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-224">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **dokumentacji** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="9cce7-225">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-226">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-227">Example</span></span>

<span data-ttu-id="9cce7-228">W poniższym przykładzie przedstawiono **dokumentacji** element jako element podrzędny elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="9cce7-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="9cce7-229">Element end (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-229">End Element (SSDL)</span></span>

<span data-ttu-id="9cce7-230">**Zakończenia** element język definicji schematu magazynu (SSDL) Określa tabelę i liczba wierszy na jednym końcu ograniczenie klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="9cce7-231">**Zakończenia** element może być elementem podrzędnym elementu Association lub elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="9cce7-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="9cce7-232">W każdym przypadku możliwe podrzędnych elementów i atrybuty stosowane są różne.</span><span class="sxs-lookup"><span data-stu-id="9cce7-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="9cce7-233">Końcowy Element jako element podrzędny elementu Association</span><span class="sxs-lookup"><span data-stu-id="9cce7-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="9cce7-234">**Zakończenia** — element (jako element podrzędny elementu **skojarzenia** elementu) Określa tabelę i liczba wierszy na końcu ograniczenie klucza obcego z **typu** i **Liczebność** odpowiednio atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9cce7-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="9cce7-235">Koniec ograniczenie klucza obcego są zdefiniowane jako część skojarzenia SSDL; skojarzenie SSDL musi mieć dokładnie punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="9cce7-236">**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-237">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-238">OnDelete (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-239">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="9cce7-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-240">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-240">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-241">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="9cce7-242">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-242">Attribute Name</span></span>   | <span data-ttu-id="9cce7-243">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-243">Is Required</span></span> | <span data-ttu-id="9cce7-244">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-245">**Typ**</span><span class="sxs-lookup"><span data-stu-id="9cce7-245">**Type**</span></span>         | <span data-ttu-id="9cce7-246">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-246">Yes</span></span>         | <span data-ttu-id="9cce7-247">W pełni kwalifikowana nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="9cce7-248">**Rola**</span><span class="sxs-lookup"><span data-stu-id="9cce7-248">**Role**</span></span>         | <span data-ttu-id="9cce7-249">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-249">No</span></span>          | <span data-ttu-id="9cce7-250">Wartość **roli** atrybutu w elemencie jednostki albo zależnych od odpowiadającego im elementu ReferentialConstraint (jeśli są używane).</span><span class="sxs-lookup"><span data-stu-id="9cce7-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="9cce7-251">**Liczebność**</span><span class="sxs-lookup"><span data-stu-id="9cce7-251">**Multiplicity**</span></span> | <span data-ttu-id="9cce7-252">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-252">Yes</span></span>         | <span data-ttu-id="9cce7-253">**1**, **od 0 do 1**, lub **\*** w zależności od liczby wierszy, które mogą być na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="9cce7-254">**1** wskazuje, że dokładnie jeden wiersz istnieje na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="9cce7-255">**od 0 do 1** wskazuje, że istnieje zero lub jeden wiersz na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="9cce7-256">**\*** oznacza, że wartość zero, jeden lub więcej wierszy na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-257">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9cce7-258">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-259">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="9cce7-260">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-260">Example</span></span>

<span data-ttu-id="9cce7-261">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **klucza Obcego\_CustomerOrders** ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9cce7-262">**Liczebność** wartościom określonym w każdym **zakończenia** elementu wskazują tę liczbę wierszy w **zamówienia** tabeli może być skojarzony z wiersza w **klientów**  tabeli, ale tylko jeden wiersz w **klientów** tabeli może być skojarzony z wiersza w **zamówienia** tabeli.</span><span class="sxs-lookup"><span data-stu-id="9cce7-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="9cce7-263">Ponadto **OnDelete** element wskazuje, że wszystkie wiersze, w **zamówienia** odwoływać się do danego wiersza w tabeli **klientów** tabeli zostanie usunięte, gdy wiersz **klientów** tabeli zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="9cce7-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="9cce7-264">Końcowy Element jako element podrzędny elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9cce7-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="9cce7-265">**Zakończenia** — element (jako element podrzędny elementu **AssociationSet** elementu) Określa tabelę na jednym końcu ograniczenie klucza obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="9cce7-266">**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-267">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-268">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-269">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-269">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-270">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="9cce7-271">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-271">Attribute Name</span></span> | <span data-ttu-id="9cce7-272">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-272">Is Required</span></span> | <span data-ttu-id="9cce7-273">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-274">**Obiekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="9cce7-274">**EntitySet**</span></span>  | <span data-ttu-id="9cce7-275">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-275">Yes</span></span>         | <span data-ttu-id="9cce7-276">Nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="9cce7-277">**Rola**</span><span class="sxs-lookup"><span data-stu-id="9cce7-277">**Role**</span></span>       | <span data-ttu-id="9cce7-278">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-278">No</span></span>          | <span data-ttu-id="9cce7-279">Wartość jednej z **roli** atrybuty określone w jednym **zakończenia** elementu odpowiednie skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-280">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9cce7-281">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-282">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="9cce7-283">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-283">Example</span></span>

<span data-ttu-id="9cce7-284">W poniższym przykładzie przedstawiono **EntityContainer** element z **AssociationSet** elementu przy użyciu dwóch **zakończenia** elementy:</span><span class="sxs-lookup"><span data-stu-id="9cce7-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="9cce7-285">Element EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="9cce7-286">**EntityContainer** element język definicji schematu magazynu (SSDL) zawiera opis struktury źródła danych w aplikacji Entity Framework: zestawy jednostek SSDL (zdefiniowanymi w elementów EntitySet) reprezentują tabele w bazy danych, typy jednostek SSDL (zdefiniowany w elementach typu EntityType) reprezentują wierszy w tabeli i zestawów skojarzeń (zdefiniowany w obiekcie AssociationSet elementy) reprezentują ograniczenia klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="9cce7-287">Kontener jednostek modelu magazynu jest mapowany na model koncepcyjny kontener jednostek, za pośrednictwem elementu w elemencie EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="9cce7-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="9cce7-288">**EntityContainer** element może mieć zero lub jeden elementów dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="9cce7-289">Jeśli **dokumentacji** element jest obecny, musi poprzedzać wszystkie inne elementy podrzędne.</span><span class="sxs-lookup"><span data-stu-id="9cce7-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="9cce7-290">**EntityContainer** element może mieć zero lub więcej z następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-291">Obiekt EntitySet</span><span class="sxs-lookup"><span data-stu-id="9cce7-291">EntitySet</span></span>
-   <span data-ttu-id="9cce7-292">Obiekt AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9cce7-292">AssociationSet</span></span>
-   <span data-ttu-id="9cce7-293">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-294">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-294">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-295">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="9cce7-296">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-296">Attribute Name</span></span> | <span data-ttu-id="9cce7-297">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-297">Is Required</span></span> | <span data-ttu-id="9cce7-298">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-299">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-299">**Name**</span></span>       | <span data-ttu-id="9cce7-300">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-300">Yes</span></span>         | <span data-ttu-id="9cce7-301">Nazwa kontenera jednostek.</span><span class="sxs-lookup"><span data-stu-id="9cce7-301">The name of the entity container.</span></span> <span data-ttu-id="9cce7-302">Ta nazwa nie może zawierać kropek (.).</span><span class="sxs-lookup"><span data-stu-id="9cce7-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-303">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="9cce7-304">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-305">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-306">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-306">Example</span></span>

<span data-ttu-id="9cce7-307">W poniższym przykładzie przedstawiono **EntityContainer** element, który definiuje dwa zestawy jednostek i jedno skojarzenie zestawu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="9cce7-308">Należy zauważyć, że nazwy jednostki typu i skojarzenia typów są kwalifikowana przez nazwę przestrzeni nazw modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="9cce7-309">Element EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="9cce7-310">**EntitySet** element język definicji schematu magazynu (SSDL) reprezentuje tabelę lub widok w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="9cce7-311">Element EntityType SSDL reprezentuje wiersz w tabeli lub widoku.</span><span class="sxs-lookup"><span data-stu-id="9cce7-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="9cce7-312">**EntityType** atrybutu **EntitySet** element określa typ jednostki SSDL konkretnego, który reprezentuje wierszy w zestawie jednostek SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="9cce7-313">Mapowanie między zestaw jednostek CSDL a SSDL zestaw jednostek jest określona w obiekcie EntitySetMapping elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="9cce7-314">**EntitySet** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-315">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-316">DefiningQuery (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-317">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-318">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-318">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-319">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="9cce7-320">Niektóre atrybuty (niewymienione w tym) może być kwalifikowana za pomocą **przechowywania** aliasu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="9cce7-321">Te atrybuty są używane przez kreatora Model aktualizacji, podczas aktualizowania modelu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="9cce7-322">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-322">Attribute Name</span></span> | <span data-ttu-id="9cce7-323">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-323">Is Required</span></span> | <span data-ttu-id="9cce7-324">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-325">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-325">**Name**</span></span>       | <span data-ttu-id="9cce7-326">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-326">Yes</span></span>         | <span data-ttu-id="9cce7-327">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="9cce7-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="9cce7-328">**Typ entityType**</span><span class="sxs-lookup"><span data-stu-id="9cce7-328">**EntityType**</span></span> | <span data-ttu-id="9cce7-329">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-329">Yes</span></span>         | <span data-ttu-id="9cce7-330">W pełni kwalifikowana nazwa typu jednostki, dla której zestaw jednostek zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="9cce7-331">**Schemat**</span><span class="sxs-lookup"><span data-stu-id="9cce7-331">**Schema**</span></span>     | <span data-ttu-id="9cce7-332">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-332">No</span></span>          | <span data-ttu-id="9cce7-333">Schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="9cce7-334">**Tabela**</span><span class="sxs-lookup"><span data-stu-id="9cce7-334">**Table**</span></span>      | <span data-ttu-id="9cce7-335">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-335">No</span></span>          | <span data-ttu-id="9cce7-336">Tabela bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="9cce7-337">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="9cce7-338">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-339">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-340">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-340">Example</span></span>

<span data-ttu-id="9cce7-341">W poniższym przykładzie przedstawiono **EntityContainer** element, który ma dwa **EntitySet** elementy i jeden **AssociationSet** elementu:</span><span class="sxs-lookup"><span data-stu-id="9cce7-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="9cce7-342">Element EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="9cce7-343">**EntityType** element język definicji schematu magazynu (SSDL) reprezentuje wiersz w tabeli lub widoku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="9cce7-344">Element EntitySet SSDL reprezentuje tabelę lub widok, w którym występują wiersze.</span><span class="sxs-lookup"><span data-stu-id="9cce7-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="9cce7-345">**EntityType** atrybutu **EntitySet** element określa typ jednostki SSDL konkretnego, który reprezentuje wierszy w zestawie jednostek SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="9cce7-346">Mapowanie między typem encji SSDL i typ jednostki CSDL jest określony w elemencie EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="9cce7-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="9cce7-347">**EntityType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-348">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-349">Klucz (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="9cce7-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="9cce7-350">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-351">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-351">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-352">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="9cce7-353">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-353">Attribute Name</span></span> | <span data-ttu-id="9cce7-354">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-354">Is Required</span></span> | <span data-ttu-id="9cce7-355">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-356">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-356">**Name**</span></span>       | <span data-ttu-id="9cce7-357">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-357">Yes</span></span>         | <span data-ttu-id="9cce7-358">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="9cce7-358">The name of the entity type.</span></span> <span data-ttu-id="9cce7-359">Ta wartość jest zwykle taka sama jak nazwa tabeli, w którym typ jednostki reprezentuje wiersz.</span><span class="sxs-lookup"><span data-stu-id="9cce7-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="9cce7-360">Ta wartość może zawierać nie kropki (.).</span><span class="sxs-lookup"><span data-stu-id="9cce7-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-361">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="9cce7-362">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-363">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-364">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-364">Example</span></span>

<span data-ttu-id="9cce7-365">W poniższym przykładzie przedstawiono **EntityType** elementu o dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="9cce7-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="9cce7-366">Function — Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-366">Function Element (SSDL)</span></span>

<span data-ttu-id="9cce7-367">**Funkcja** element język definicji schematu magazynu (SSDL) określa procedury przechowywanej, która istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="9cce7-368">**Funkcja** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-369">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-370">Parametr (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="9cce7-371">CommandText (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="9cce7-372">ReturnType (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="9cce7-373">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="9cce7-374">Zwracana typ dla funkcji, należy określić albo **ReturnType** element lub **ReturnType** atrybutu (patrz poniżej), ale nie oba.</span><span class="sxs-lookup"><span data-stu-id="9cce7-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="9cce7-375">Procedury składowane, które są określone w modelu magazynu można zaimportować do modelu koncepcyjnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="9cce7-376">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="9cce7-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="9cce7-377">**Funkcja** elementu może również służyć do definiowania funkcji niestandardowych w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-378">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-378">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-379">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **funkcja** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="9cce7-380">Niektóre atrybuty (niewymienione w tym) może być kwalifikowana za pomocą **przechowywania** aliasu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="9cce7-381">Te atrybuty są używane przez kreatora Model aktualizacji, podczas aktualizowania modelu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="9cce7-382">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-382">Attribute Name</span></span>             | <span data-ttu-id="9cce7-383">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-383">Is Required</span></span> | <span data-ttu-id="9cce7-384">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-385">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-385">**Name**</span></span>                   | <span data-ttu-id="9cce7-386">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-386">Yes</span></span>         | <span data-ttu-id="9cce7-387">Nazwa procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9cce7-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="9cce7-388">**ReturnType**</span></span>             | <span data-ttu-id="9cce7-389">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-389">No</span></span>          | <span data-ttu-id="9cce7-390">Zwracany typ procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="9cce7-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="9cce7-391">**Aggregate**</span></span>              | <span data-ttu-id="9cce7-392">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-392">No</span></span>          | <span data-ttu-id="9cce7-393">**Wartość true,** Jeśli procedura składowana ma zwracać wartości zagregowanej; w przeciwnym razie **False**.</span><span class="sxs-lookup"><span data-stu-id="9cce7-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="9cce7-394">**Wbudowane**</span><span class="sxs-lookup"><span data-stu-id="9cce7-394">**BuiltIn**</span></span>                | <span data-ttu-id="9cce7-395">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-395">No</span></span>          | <span data-ttu-id="9cce7-396">**Wartość true,** Jeśli funkcja jest wbudowaną<sup>1</sup> funkcji; w przeciwnym razie **False**.</span><span class="sxs-lookup"><span data-stu-id="9cce7-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="9cce7-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="9cce7-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="9cce7-398">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-398">No</span></span>          | <span data-ttu-id="9cce7-399">Nazwa procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9cce7-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="9cce7-400">**NiladicFunction**</span></span>        | <span data-ttu-id="9cce7-401">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-401">No</span></span>          | <span data-ttu-id="9cce7-402">**Wartość true,** Jeśli funkcja znajduje się bez parametrów<sup>2</sup> funkcji; **False** inaczej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="9cce7-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="9cce7-403">**IsComposable**</span></span>           | <span data-ttu-id="9cce7-404">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-404">No</span></span>          | <span data-ttu-id="9cce7-405">**Wartość true,** Jeśli funkcja jest konfigurowalna<sup>3</sup> funkcji; **False** inaczej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="9cce7-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="9cce7-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="9cce7-407">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-407">No</span></span>          | <span data-ttu-id="9cce7-408">Wyliczenie, które definiuje semantyka typów, używany do rozpoznawania przeciążenia funkcji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="9cce7-409">Wyliczenia jest zdefiniowany w manifeście dostawcy dla definicji funkcji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="9cce7-410">Wartość domyślna to **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="9cce7-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="9cce7-411">**Schemat**</span><span class="sxs-lookup"><span data-stu-id="9cce7-411">**Schema**</span></span>                 | <span data-ttu-id="9cce7-412">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-412">No</span></span>          | <span data-ttu-id="9cce7-413">Nazwa schematu, w którym zdefiniowano procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="9cce7-414"><sup>1</sup> wbudowana funkcja jest funkcją, która jest zdefiniowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="9cce7-415">Aby uzyskać informacje na temat funkcji, które są zdefiniowane w modelu magazynu Zobacz Element CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="9cce7-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="9cce7-416"><sup>2</sup> funkcję bez parametrów jest funkcją, która przyjmuje żadnych parametrów i, gdy zostanie wywołana, nie wymaga nawiasów.</span><span class="sxs-lookup"><span data-stu-id="9cce7-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="9cce7-417"><sup>3</sup> dwie funkcje są konfigurowalna, jeśli jedna funkcja może zwracać dane wejściowe dla innych funkcji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="9cce7-418">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **funkcja** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="9cce7-419">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-420">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-421">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-421">Example</span></span>

<span data-ttu-id="9cce7-422">W poniższym przykładzie przedstawiono **funkcja** element, który odpowiada **UpdateOrderQuantity** procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="9cce7-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="9cce7-423">Procedura składowana akceptuje dwa parametry i zwracać wartości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="9cce7-424">Kluczowym elementem (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-424">Key Element (SSDL)</span></span>

<span data-ttu-id="9cce7-425">**Klucz** element język definicji schematu magazynu (SSDL) reprezentuje klucz podstawowy tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="9cce7-426">**Klucz** jest elementem podrzędnym elementu EntityType, który reprezentuje wiersz w tabeli.</span><span class="sxs-lookup"><span data-stu-id="9cce7-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="9cce7-427">Klucz podstawowy jest zdefiniowany w **klucz** elementu odwołując się do elementów właściwości, które są zdefiniowane na **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="9cce7-428">**Klucz** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-429">PropertyRef (jeden lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9cce7-430">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-430">Annotation elements</span></span>

<span data-ttu-id="9cce7-431">Atrybuty nie są stosowane do **klucz** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-432">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-432">Example</span></span>

<span data-ttu-id="9cce7-433">W poniższym przykładzie przedstawiono **EntityType** element z kluczem, który odwołuje się do jednej właściwości:</span><span class="sxs-lookup"><span data-stu-id="9cce7-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="9cce7-434">Element OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="9cce7-435">**OnDelete** element język definicji schematu magazynu (SSDL) odzwierciedla zachowanie bazy danych, po usunięciu wiersza, który uczestniczy w ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="9cce7-436">Jeśli ustawiono akcję **Cascade**, a następnie również zostaną usunięte wiersze, które odwołują się wiersz, który jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="9cce7-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="9cce7-437">Jeśli ustawiono akcję **Brak**, a następnie nie zostaną również usunięte wiersze, które odwołują się wiersz, który jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="9cce7-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="9cce7-438">**OnDelete** element jest elementem podrzędnym elementu End.</span><span class="sxs-lookup"><span data-stu-id="9cce7-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="9cce7-439">**OnDelete** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-440">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-441">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-442">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-442">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-443">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="9cce7-444">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-444">Attribute Name</span></span> | <span data-ttu-id="9cce7-445">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-445">Is Required</span></span> | <span data-ttu-id="9cce7-446">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-447">**Akcja**</span><span class="sxs-lookup"><span data-stu-id="9cce7-447">**Action**</span></span>     | <span data-ttu-id="9cce7-448">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-448">Yes</span></span>         | <span data-ttu-id="9cce7-449">**Kaskadowe** lub **Brak**.</span><span class="sxs-lookup"><span data-stu-id="9cce7-449">**Cascade** or **None**.</span></span> <span data-ttu-id="9cce7-450">(Wartość **ograniczeniami** jest prawidłowy, ale ma takie samo zachowanie jako **Brak**.)</span><span class="sxs-lookup"><span data-stu-id="9cce7-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-451">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="9cce7-452">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-453">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-454">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-454">Example</span></span>

<span data-ttu-id="9cce7-455">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **klucza Obcego\_CustomerOrders** ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9cce7-456">**OnDelete** element wskazuje, że wszystkie wiersze w **zamówienia** odwoływać się do danego wiersza w tabeli **klientów** tabeli zostanie usunięte, gdy wiersz **Klientów** tabeli zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="9cce7-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="9cce7-457">Parameter — Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="9cce7-458">**Parametru** język definicji schematu magazynu (SSDL) element jest elementem podrzędnym elementu funkcji, który określa parametry dla procedury przechowywanej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="9cce7-459">**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-460">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-461">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-462">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-462">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-463">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="9cce7-464">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-464">Attribute Name</span></span> | <span data-ttu-id="9cce7-465">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-465">Is Required</span></span> | <span data-ttu-id="9cce7-466">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-467">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-467">**Name**</span></span>       | <span data-ttu-id="9cce7-468">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-468">Yes</span></span>         | <span data-ttu-id="9cce7-469">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="9cce7-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="9cce7-470">**Typ**</span><span class="sxs-lookup"><span data-stu-id="9cce7-470">**Type**</span></span>       | <span data-ttu-id="9cce7-471">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-471">Yes</span></span>         | <span data-ttu-id="9cce7-472">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="9cce7-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="9cce7-473">**Tryb**</span><span class="sxs-lookup"><span data-stu-id="9cce7-473">**Mode**</span></span>       | <span data-ttu-id="9cce7-474">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-474">No</span></span>          | <span data-ttu-id="9cce7-475">**W**, **się**, lub **InOut** w zależności od tego, czy parametr jest danych wejściowych, danych wyjściowych lub parametr input/output.</span><span class="sxs-lookup"><span data-stu-id="9cce7-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="9cce7-476">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="9cce7-476">**MaxLength**</span></span>  | <span data-ttu-id="9cce7-477">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-477">No</span></span>          | <span data-ttu-id="9cce7-478">Maksymalna długość parametru.</span><span class="sxs-lookup"><span data-stu-id="9cce7-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9cce7-479">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="9cce7-479">**Precision**</span></span>  | <span data-ttu-id="9cce7-480">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-480">No</span></span>          | <span data-ttu-id="9cce7-481">Dokładność parametru.</span><span class="sxs-lookup"><span data-stu-id="9cce7-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="9cce7-482">**Skala**</span><span class="sxs-lookup"><span data-stu-id="9cce7-482">**Scale**</span></span>      | <span data-ttu-id="9cce7-483">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-483">No</span></span>          | <span data-ttu-id="9cce7-484">Skala parametru.</span><span class="sxs-lookup"><span data-stu-id="9cce7-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="9cce7-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9cce7-485">**SRID**</span></span>       | <span data-ttu-id="9cce7-486">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-486">No</span></span>          | <span data-ttu-id="9cce7-487">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9cce7-488">Prawidłowy tylko dla parametrów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="9cce7-489">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9cce7-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-490">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="9cce7-491">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-492">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-493">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-493">Example</span></span>

<span data-ttu-id="9cce7-494">W poniższym przykładzie przedstawiono **funkcja** element, który ma dwa **parametru** elementy, które określają parametry wejściowe:</span><span class="sxs-lookup"><span data-stu-id="9cce7-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="9cce7-495">Element jednostki (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="9cce7-496">**Jednostki** element język definicji schematu magazynu (SSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje główny koniec ograniczenie klucza obcego (nazywany także ograniczenie referencyjne).</span><span class="sxs-lookup"><span data-stu-id="9cce7-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="9cce7-497">**Jednostki** element określa kolumny klucza podstawowego (lub kolumny) w tabeli, która odwołuje się do niej innej kolumny (lub kolumny).</span><span class="sxs-lookup"><span data-stu-id="9cce7-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="9cce7-498">**PropertyRef** elementy Określ kolumny, które są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="9cce7-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="9cce7-499">Element zależne Określa kolumny, które odwołują się kolumny klucza podstawowego, które są określone w **jednostki** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="9cce7-500">**Jednostki** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="9cce7-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9cce7-501">PropertyRef (jeden lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9cce7-502">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-503">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-503">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-504">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **jednostki** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="9cce7-505">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-505">Attribute Name</span></span> | <span data-ttu-id="9cce7-506">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-506">Is Required</span></span> | <span data-ttu-id="9cce7-507">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-508">**Rola**</span><span class="sxs-lookup"><span data-stu-id="9cce7-508">**Role**</span></span>       | <span data-ttu-id="9cce7-509">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-509">Yes</span></span>         | <span data-ttu-id="9cce7-510">Taką samą wartość jak **roli** atrybut odpowiedni element End (jeśli jest używana); w przeciwnym razie nazwę tabeli, która zawiera odwołuje się do kolumny.</span><span class="sxs-lookup"><span data-stu-id="9cce7-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-511">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **jednostki** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="9cce7-512">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-513">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-514">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-514">Example</span></span>

<span data-ttu-id="9cce7-515">W poniższym przykładzie pokazano element Association używający **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders** klucza obcego ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9cce7-516">**Jednostki** element Określa **CustomerId** kolumny **klienta** tabeli jako główny koniec tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="9cce7-517">Property — Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-517">Property Element (SSDL)</span></span>

<span data-ttu-id="9cce7-518">**Właściwość** element język definicji schematu magazynu (SSDL) reprezentuje kolumnę w tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="9cce7-519">**Właściwość** elementy są elementami podrzędnymi typu EntityType elementy, które reprezentują wierszy w tabeli.</span><span class="sxs-lookup"><span data-stu-id="9cce7-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="9cce7-520">Każdy **właściwość** elementu zdefiniowanego w **EntityType** element reprezentuje kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="9cce7-521">A **właściwość** elementu nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-522">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-522">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-523">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="9cce7-524">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-524">Attribute Name</span></span>            | <span data-ttu-id="9cce7-525">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-525">Is Required</span></span> | <span data-ttu-id="9cce7-526">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-527">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-527">**Name**</span></span>                  | <span data-ttu-id="9cce7-528">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-528">Yes</span></span>         | <span data-ttu-id="9cce7-529">Nazwa odpowiednią kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="9cce7-530">**Typ**</span><span class="sxs-lookup"><span data-stu-id="9cce7-530">**Type**</span></span>                  | <span data-ttu-id="9cce7-531">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-531">Yes</span></span>         | <span data-ttu-id="9cce7-532">Typ odpowiednią kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="9cce7-533">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="9cce7-533">**Nullable**</span></span>              | <span data-ttu-id="9cce7-534">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-534">No</span></span>          | <span data-ttu-id="9cce7-535">**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy odpowiednia kolumna może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="9cce7-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="9cce7-536">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="9cce7-536">**DefaultValue**</span></span>          | <span data-ttu-id="9cce7-537">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-537">No</span></span>          | <span data-ttu-id="9cce7-538">Wartość domyślna w kolumnie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9cce7-539">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="9cce7-539">**MaxLength**</span></span>             | <span data-ttu-id="9cce7-540">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-540">No</span></span>          | <span data-ttu-id="9cce7-541">Maksymalna długość odpowiednią kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="9cce7-542">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="9cce7-542">**FixedLength**</span></span>           | <span data-ttu-id="9cce7-543">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-543">No</span></span>          | <span data-ttu-id="9cce7-544">**Wartość true,** lub **False** w zależności od tego, czy odpowiadająca wartość w kolumnie będą przechowywane jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="9cce7-545">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="9cce7-545">**Precision**</span></span>             | <span data-ttu-id="9cce7-546">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-546">No</span></span>          | <span data-ttu-id="9cce7-547">Dokładność odpowiednią kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="9cce7-548">**Skala**</span><span class="sxs-lookup"><span data-stu-id="9cce7-548">**Scale**</span></span>                 | <span data-ttu-id="9cce7-549">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-549">No</span></span>          | <span data-ttu-id="9cce7-550">Skala odpowiednią kolumnę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="9cce7-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="9cce7-551">**Unicode**</span></span>               | <span data-ttu-id="9cce7-552">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-552">No</span></span>          | <span data-ttu-id="9cce7-553">**Wartość true,** lub **False** w zależności od tego, czy odpowiadająca wartość w kolumnie będą przechowywane jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="9cce7-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="9cce7-554">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="9cce7-554">**Collation**</span></span>             | <span data-ttu-id="9cce7-555">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-555">No</span></span>          | <span data-ttu-id="9cce7-556">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="9cce7-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9cce7-557">**SRID**</span></span>                  | <span data-ttu-id="9cce7-558">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-558">No</span></span>          | <span data-ttu-id="9cce7-559">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9cce7-560">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9cce7-561">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9cce7-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9cce7-562">**Element StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="9cce7-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="9cce7-563">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-563">No</span></span>          | <span data-ttu-id="9cce7-564">**Brak**, **tożsamości** (jeśli odpowiadająca wartość w kolumnie jest tożsamość, która jest generowana w bazie danych) lub **obliczane** (jeśli odpowiadająca wartość w kolumnie jest obliczana w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="9cce7-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="9cce7-565">Nie obowiązuje dla właściwości RowType.</span><span class="sxs-lookup"><span data-stu-id="9cce7-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-566">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="9cce7-567">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-568">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-569">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-569">Example</span></span>

<span data-ttu-id="9cce7-570">W poniższym przykładzie przedstawiono **EntityType** element z dwóch podrzędnych **właściwość** elementy:</span><span class="sxs-lookup"><span data-stu-id="9cce7-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="9cce7-571">Element PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="9cce7-572">**PropertyRef** element język definicji schematu magazynu (SSDL) odwołuje się do właściwości zdefiniowany dla elementu EntityType, aby wskazać, że właściwość wykona jedną z następujących ról:</span><span class="sxs-lookup"><span data-stu-id="9cce7-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="9cce7-573">Być częścią klucza podstawowego w tabeli, która **EntityType** reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="9cce7-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="9cce7-574">Co najmniej jeden **PropertyRef** elementy mogą być używane do definiowania klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9cce7-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="9cce7-575">Aby uzyskać więcej informacji zobacz element klucza.</span><span class="sxs-lookup"><span data-stu-id="9cce7-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="9cce7-576">Być zakończenia zależnych lub jednostki z ograniczeniem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="9cce7-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="9cce7-577">Aby uzyskać więcej informacji zobacz ReferentialConstraint element.</span><span class="sxs-lookup"><span data-stu-id="9cce7-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="9cce7-578">**PropertyRef** element może mieć tylko następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="9cce7-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="9cce7-579">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-580">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-581">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-581">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-582">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="9cce7-583">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-583">Attribute Name</span></span> | <span data-ttu-id="9cce7-584">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-584">Is Required</span></span> | <span data-ttu-id="9cce7-585">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="9cce7-586">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="9cce7-586">**Name**</span></span>       | <span data-ttu-id="9cce7-587">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-587">Yes</span></span>         | <span data-ttu-id="9cce7-588">Nazwa właściwości, której dotyczy odwołanie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="9cce7-589">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="9cce7-590">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9cce7-591">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-592">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-592">Example</span></span>

<span data-ttu-id="9cce7-593">W poniższym przykładzie przedstawiono **PropertyRef** element używany do definiowania klucza podstawowego, odwołując się do właściwości, która jest zdefiniowana na **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="9cce7-594">Element ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="9cce7-595">**ReferentialConstraint** element język definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego (nazywane również ograniczenia integralności referencyjnej) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="9cce7-596">Kończy się głównym i zależnym ograniczenia są odpowiednio określone przez elementy podrzędne jednostki i zależnych od ustawień lokalnych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="9cce7-597">Kolumn, które uczestniczą w głównym i zależnym kończy się są przywoływane z elementami PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="9cce7-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="9cce7-598">**ReferentialConstraint** element jest podrzędnym elementem opcjonalnym elementu Association.</span><span class="sxs-lookup"><span data-stu-id="9cce7-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="9cce7-599">Jeśli **ReferentialConstraint** element nie jest używany do mapowania ograniczenie klucza obcego, który jest określony w **skojarzenia** AssociationSetMapping element musi być użyty w tym elemencie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="9cce7-600">**ReferentialConstraint** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="9cce7-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="9cce7-601">Dokumentacja (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9cce7-602">Podmiot zabezpieczeń (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="9cce7-603">Zależnych od ustawień lokalnych (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="9cce7-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="9cce7-604">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-605">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-605">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-606">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="9cce7-607">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-608">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-609">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-609">Example</span></span>

<span data-ttu-id="9cce7-610">W poniższym przykładzie przedstawiono **skojarzenia** element, który używa **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders**  ograniczenie klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="9cce7-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="9cce7-611">Element ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="9cce7-612">**ReturnType** element język definicji schematu magazynu (SSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w **funkcja** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="9cce7-613">Można również określić typ zwracany z funkcji **ReturnType** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="9cce7-614">Zwracany typ funkcji jest określony za pomocą **typu** atrybutu lub **ReturnType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="9cce7-615">**ReturnType** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="9cce7-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="9cce7-616">Typ CollectionType (po jednym)</span><span class="sxs-lookup"><span data-stu-id="9cce7-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="9cce7-617">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="9cce7-618">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9cce7-619">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-620">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-620">Example</span></span>

<span data-ttu-id="9cce7-621">W poniższym przykładzie użyto **funkcja** zwracającego Kolekcja wierszy.</span><span class="sxs-lookup"><span data-stu-id="9cce7-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="9cce7-622">Element RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="9cce7-623">A **RowType** element język definicji schematu magazynu (SSDL) definiuje strukturę nienazwane jako zwracany typ funkcji zdefiniowanych w magazynie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="9cce7-624">A **RowType** element jest elementem podrzędnym **CollectionType** elementu:</span><span class="sxs-lookup"><span data-stu-id="9cce7-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="9cce7-625">A **RowType** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="9cce7-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="9cce7-626">Właściwości (jeden lub więcej)</span><span class="sxs-lookup"><span data-stu-id="9cce7-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="9cce7-627">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-627">Example</span></span>

<span data-ttu-id="9cce7-628">W poniższym przykładzie pokazano funkcję magazynu, która używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).</span><span class="sxs-lookup"><span data-stu-id="9cce7-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="9cce7-629">Element schematu (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="9cce7-630">**Schematu** element w język definicji schematu magazynu (SSDL) jest głównym elementem definicję modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="9cce7-631">Zawiera definicje dla obiektów, funkcji i kontenerów, które tworzą model magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="9cce7-632">**Schematu** element może zawierać zero lub więcej z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="9cce7-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="9cce7-633">Skojarzenie</span><span class="sxs-lookup"><span data-stu-id="9cce7-633">Association</span></span>
-   <span data-ttu-id="9cce7-634">Typ entityType</span><span class="sxs-lookup"><span data-stu-id="9cce7-634">EntityType</span></span>
-   <span data-ttu-id="9cce7-635">Obiekt EntityContainer</span><span class="sxs-lookup"><span data-stu-id="9cce7-635">EntityContainer</span></span>
-   <span data-ttu-id="9cce7-636">Funkcja</span><span class="sxs-lookup"><span data-stu-id="9cce7-636">Function</span></span>

<span data-ttu-id="9cce7-637">**Schematu** element używa **Namespace** atrybut do definiowania przestrzeni nazw dla obiektów typu i skojarzenia jednostki w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="9cce7-638">W przestrzeni nazw nie dwa obiekty mogą mieć takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="9cce7-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="9cce7-639">Przestrzeń nazw modelu magazynu różni się od przestrzeni nazw XML **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="9cce7-640">Przestrzeń nazw modelu magazynu (zgodnie z definicją **Namespace** atrybutu) to logiczny kontener przeznaczony dla typów jednostek i typów skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="9cce7-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="9cce7-641">Przestrzeń nazw XML (wskazywanym przez **xmlns** atrybutu) z **schematu** element jest domyślny obszar nazw dla elementów podrzędnych i atrybutów **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="9cce7-642">Obszary nazw XML w postaci http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (gdzie RRRR i MM stanowi rok i miesiąc odpowiednio) są zarezerwowane dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="9cce7-643">Niestandardowe elementy i atrybuty nie może być w przestrzeni nazw, które mają postać.</span><span class="sxs-lookup"><span data-stu-id="9cce7-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9cce7-644">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="9cce7-644">Applicable Attributes</span></span>

<span data-ttu-id="9cce7-645">W poniższej tabeli opisano atrybuty mogą być stosowane do **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="9cce7-646">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="9cce7-646">Attribute Name</span></span>            | <span data-ttu-id="9cce7-647">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="9cce7-647">Is Required</span></span> | <span data-ttu-id="9cce7-648">Wartość</span><span class="sxs-lookup"><span data-stu-id="9cce7-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="9cce7-649">**Namespace**</span></span>             | <span data-ttu-id="9cce7-650">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-650">Yes</span></span>         | <span data-ttu-id="9cce7-651">Przestrzeń nazw modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-651">The namespace of the storage model.</span></span> <span data-ttu-id="9cce7-652">Wartość **Namespace** atrybut jest używany w celu utworzenia w pełni kwalifikowana nazwa typu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="9cce7-653">Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw ExampleModel.Store, a następnie w pełni kwalifikowana nazwa **EntityType** jest ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="9cce7-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="9cce7-654">Nie można użyć następujących ciągów jako wartość pozycji **Namespace** atrybut: **systemu**, **przejściowy**, lub **Edm**.</span><span class="sxs-lookup"><span data-stu-id="9cce7-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="9cce7-655">Wartość **Namespace** atrybut nie może być taka sama jak wartość **Namespace** atrybutu w elemencie CSDL Schema.</span><span class="sxs-lookup"><span data-stu-id="9cce7-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="9cce7-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="9cce7-656">**Alias**</span></span>                 | <span data-ttu-id="9cce7-657">Nie</span><span class="sxs-lookup"><span data-stu-id="9cce7-657">No</span></span>          | <span data-ttu-id="9cce7-658">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9cce7-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="9cce7-659">Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw ExampleModel.Store i wartość **Alias** atrybut jest *StorageModel*, wówczas można użyć StorageModel.Customer jako w pełni kwalifikowana nazwa **typu EntityType.**</span><span class="sxs-lookup"><span data-stu-id="9cce7-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="9cce7-660">**Dostawcy**</span><span class="sxs-lookup"><span data-stu-id="9cce7-660">**Provider**</span></span>              | <span data-ttu-id="9cce7-661">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-661">Yes</span></span>         | <span data-ttu-id="9cce7-662">Dostawca danych.</span><span class="sxs-lookup"><span data-stu-id="9cce7-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="9cce7-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="9cce7-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="9cce7-664">Tak</span><span class="sxs-lookup"><span data-stu-id="9cce7-664">Yes</span></span>         | <span data-ttu-id="9cce7-665">Token, który wskazuje dostawcy, które manifest dostawcy, aby powrócić.</span><span class="sxs-lookup"><span data-stu-id="9cce7-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="9cce7-666">Nie format tokenu jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="9cce7-666">No format for the token is defined.</span></span> <span data-ttu-id="9cce7-667">Wartości dla tokenu są definiowane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="9cce7-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="9cce7-668">Uzyskać informacji dotyczących tokenów manifestu dostawcy programu SQL Server zobacz SqlClient programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cce7-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="9cce7-669">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-669">Example</span></span>

<span data-ttu-id="9cce7-670">W poniższym przykładzie przedstawiono **schematu** element, który zawiera **EntityContainer** element, dwa **EntityType** elementów, a drugi **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="9cce7-671">Atrybuty adnotacji</span><span class="sxs-lookup"><span data-stu-id="9cce7-671">Annotation Attributes</span></span>

<span data-ttu-id="9cce7-672">Atrybuty adnotacji w język definicji schematu magazynu (SSDL) są niestandardowych atrybutów XML w modelu magazynu, które zapewniają dodatkowe metadane na temat elementów w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="9cce7-673">Oprócz prawidłowe struktury XML, obowiązują następujące ograniczenia adnotacji atrybutów:</span><span class="sxs-lookup"><span data-stu-id="9cce7-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="9cce7-674">Atrybuty adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="9cce7-675">W pełni kwalifikowanej nazwy wszelkie atrybuty dwóch adnotacji nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="9cce7-676">Więcej niż jeden atrybut adnotacji można stosować do danego elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="9cce7-677">W czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm możliwy jest metadanych elementów adnotacji.</span><span class="sxs-lookup"><span data-stu-id="9cce7-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-678">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-678">Example</span></span>

<span data-ttu-id="9cce7-679">W poniższym przykładzie pokazano element EntityType, który ma atrybut adnotacja zastosowana do **OrderId** właściwości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="9cce7-680">Przykład pokazują również dodawane do elementu adnotacji **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="9cce7-681">Elementów adnotacji (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="9cce7-682">Elementy adnotacji w język definicji schematu magazynu (SSDL) są niestandardowe elementy XML w modelu magazynu, które zapewniają dodatkowe metadane na temat modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="9cce7-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="9cce7-683">Oprócz prawidłowe struktury XML, elementów adnotacji obowiązują następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="9cce7-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="9cce7-684">Elementów adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="9cce7-685">W pełni kwalifikowanej nazwy dowolne elementy dwóch adnotacji nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="9cce7-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="9cce7-686">Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych danego elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="9cce7-687">Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="9cce7-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="9cce7-688">Począwszy od programu .NET Framework w wersji 4, metadanych elementów adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="9cce7-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9cce7-689">Przykład</span><span class="sxs-lookup"><span data-stu-id="9cce7-689">Example</span></span>

<span data-ttu-id="9cce7-690">W poniższym przykładzie pokazano element EntityType, który ma element adnotacji (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="9cce7-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="9cce7-691">W przykładzie pokazano również zastosować atrybut adnotacji **OrderId** właściwości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="9cce7-692">Zestawy reguł (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9cce7-692">Facets (SSDL)</span></span>

<span data-ttu-id="9cce7-693">Aspekty w język definicji schematu magazynu (SSDL) reprezentują ograniczenia dotyczące typów kolumn, które są określone w elementach właściwości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="9cce7-694">Zestawy reguł są implementowane jako atrybuty XML w **właściwość** elementów.</span><span class="sxs-lookup"><span data-stu-id="9cce7-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="9cce7-695">W poniższej tabeli opisano aspekty, które są obsługiwane przez SSDL:</span><span class="sxs-lookup"><span data-stu-id="9cce7-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="9cce7-696">zestaw reguł</span><span class="sxs-lookup"><span data-stu-id="9cce7-696">Facet</span></span>           | <span data-ttu-id="9cce7-697">Opis</span><span class="sxs-lookup"><span data-stu-id="9cce7-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9cce7-698">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="9cce7-698">**Collation**</span></span>   | <span data-ttu-id="9cce7-699">Określa kolejność sortowania (lub sekwencji sortowania) do użycia podczas przeprowadzania porównania i kolejność operacji na wartościach właściwości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="9cce7-700">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="9cce7-700">**FixedLength**</span></span> | <span data-ttu-id="9cce7-701">Określa, czy długość wartości kolumny mogą się różnić.</span><span class="sxs-lookup"><span data-stu-id="9cce7-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="9cce7-702">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="9cce7-702">**MaxLength**</span></span>   | <span data-ttu-id="9cce7-703">Określa maksymalną długość wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="9cce7-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="9cce7-704">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="9cce7-704">**Precision**</span></span>   | <span data-ttu-id="9cce7-705">Dla właściwości typu **dziesiętna**, określa liczbę cyfr, może mieć wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="9cce7-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="9cce7-706">Dla właściwości typu **czasu**, **daty/godziny**, i **DateTimeOffset**, określa liczbę cyfr ułamkowych części sekundy w wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="9cce7-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="9cce7-707">**Skala**</span><span class="sxs-lookup"><span data-stu-id="9cce7-707">**Scale**</span></span>       | <span data-ttu-id="9cce7-708">Określa liczbę cyfr po prawej stronie przecinka dziesiętnego dla wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="9cce7-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="9cce7-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="9cce7-709">**Unicode**</span></span>     | <span data-ttu-id="9cce7-710">Wskazuje, czy wartość kolumny jest zapisywana w formacie Unicode.</span><span class="sxs-lookup"><span data-stu-id="9cce7-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
