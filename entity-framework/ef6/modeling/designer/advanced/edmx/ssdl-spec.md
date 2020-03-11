---
title: Specyfikacja SSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418726"
---
# <a name="ssdl-specification"></a><span data-ttu-id="a790e-102">Specyfikacja SSDL</span><span class="sxs-lookup"><span data-stu-id="a790e-102">SSDL Specification</span></span>
<span data-ttu-id="a790e-103">Język definicji schematu magazynu (SSDL) to język oparty na języku XML, który opisuje model magazynu aplikacji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a790e-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="a790e-104">W aplikacji Entity Framework metadane modelu magazynu są ładowane z pliku SSDL (zapisywanego w plikach SSDL) do wystąpienia elementu System. Data. Metadata. Edm. StoreItemCollection i są dostępne przy użyciu metod w Klasa System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="a790e-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="a790e-105">Entity Framework używa metadanych modelu magazynu do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="a790e-106">Entity Framework Designer (programu EF Designer) przechowuje informacje o modelu magazynu w pliku edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="a790e-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="a790e-107">W czasie kompilacji Entity Designer używa informacji w pliku. edmx, aby utworzyć plik SSDL, który jest wymagany przez Entity Framework w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a790e-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="a790e-108">Wersje plików SSDL są odróżniane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="a790e-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="a790e-109">Wersja SSDL</span><span class="sxs-lookup"><span data-stu-id="a790e-109">SSDL Version</span></span> | <span data-ttu-id="a790e-110">Przestrzeń nazw XML</span><span class="sxs-lookup"><span data-stu-id="a790e-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="a790e-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="a790e-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="a790e-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="a790e-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="a790e-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="a790e-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="a790e-114">Element Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-114">Association Element (SSDL)</span></span>

<span data-ttu-id="a790e-115">Element **Association** w języku definicji schematu magazynu (SSDL) określa kolumny tabeli, które uczestniczą w ograniczeniu klucza obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="a790e-116">Dwa wymagane elementy podrzędne elementu End określają tabele na końcu skojarzenia i liczebność na każdym końcu.</span><span class="sxs-lookup"><span data-stu-id="a790e-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="a790e-117">Opcjonalny element ReferentialConstraint określa główne i zależne zakończenia skojarzenia, a także powiązane kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="a790e-118">Jeśli żaden element **ReferentialConstraint** nie istnieje, element AssociationSetMapping musi być używany do określenia mapowań kolumn dla skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="a790e-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="a790e-119">Element **Association** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-120">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-121">End (dokładnie dwa)</span><span class="sxs-lookup"><span data-stu-id="a790e-121">End (exactly two)</span></span>
-   <span data-ttu-id="a790e-122">ReferentialConstraint (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="a790e-123">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-124">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-124">Applicable Attributes</span></span>

<span data-ttu-id="a790e-125">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Association** .</span><span class="sxs-lookup"><span data-stu-id="a790e-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="a790e-126">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-126">Attribute Name</span></span> | <span data-ttu-id="a790e-127">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-127">Is Required</span></span> | <span data-ttu-id="a790e-128">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-129">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-129">**Name**</span></span>       | <span data-ttu-id="a790e-130">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-130">Yes</span></span>         | <span data-ttu-id="a790e-131">Nazwa odpowiedniego ograniczenia klucza obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-132">Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="a790e-133">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-134">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-135">Example</span></span>

<span data-ttu-id="a790e-136">Poniższy przykład pokazuje element **skojarzenia** , który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="a790e-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="a790e-137">AssociationSet, element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="a790e-138">Element **AssociationSet** w języku definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego między dwiema tabelami w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="a790e-139">Kolumny tabeli, które uczestniczą w ograniczeniu klucza obcego, są określone w elemencie skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="a790e-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="a790e-140">Element **Association** , który odnosi się do danego elementu **AssociationSet** , jest określony w atrybucie **skojarzenia** elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="a790e-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="a790e-141">Zestawy skojarzeń SSDL są mapowane do zestawów skojarzeń CSDL przez element AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="a790e-142">Jeśli jednak skojarzenie CSDL dla danego zestawu skojarzeń CSDL jest zdefiniowane za pomocą elementu ReferentialConstraint, nie jest potrzebny odpowiedni element **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="a790e-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="a790e-143">W tym przypadku, jeśli element **AssociationSetMapping** jest obecny, mapowania, które definiuje, zostaną przesłonięte przez element **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="a790e-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="a790e-144">Element **AssociationSet** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-145">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-146">Koniec (zero lub dwa)</span><span class="sxs-lookup"><span data-stu-id="a790e-146">End (zero or two)</span></span>
-   <span data-ttu-id="a790e-147">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-148">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-148">Applicable Attributes</span></span>

<span data-ttu-id="a790e-149">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="a790e-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="a790e-150">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-150">Attribute Name</span></span>  | <span data-ttu-id="a790e-151">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-151">Is Required</span></span> | <span data-ttu-id="a790e-152">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-153">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-153">**Name**</span></span>        | <span data-ttu-id="a790e-154">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-154">Yes</span></span>         | <span data-ttu-id="a790e-155">Nazwa ograniczenia klucza obcego, które reprezentuje zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="a790e-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="a790e-156">**Skojarzenie**</span><span class="sxs-lookup"><span data-stu-id="a790e-156">**Association**</span></span> | <span data-ttu-id="a790e-157">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-157">Yes</span></span>         | <span data-ttu-id="a790e-158">Nazwa skojarzenia, która definiuje kolumny, które uczestniczą w ograniczeniu klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-159">Do elementu **AssociationSet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="a790e-160">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-161">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-162">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-162">Example</span></span>

<span data-ttu-id="a790e-163">Poniższy przykład przedstawia element **AssociationSet** , który reprezentuje `FK_CustomerOrders` ograniczenie klucza obcego w źródłowej bazie danych:</span><span class="sxs-lookup"><span data-stu-id="a790e-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="a790e-164">CollectionType — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="a790e-165">Element **CollectionType** w języku definicji schematu magazynu (SSDL) określa, że zwracany typ funkcji jest kolekcją.</span><span class="sxs-lookup"><span data-stu-id="a790e-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="a790e-166">Element **CollectionType** jest elementem podrzędnym elementu ReturnType.</span><span class="sxs-lookup"><span data-stu-id="a790e-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="a790e-167">Typ kolekcji jest określany za pomocą elementu podrzędnego RowType:</span><span class="sxs-lookup"><span data-stu-id="a790e-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="a790e-168">Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="a790e-169">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-170">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-171">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-171">Example</span></span>

<span data-ttu-id="a790e-172">Poniższy przykład pokazuje funkcję, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy.</span><span class="sxs-lookup"><span data-stu-id="a790e-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="a790e-173">CommandText — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="a790e-174">Element **CommandText** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu Function, który umożliwia zdefiniowanie instrukcji SQL, która jest wykonywana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="a790e-175">Element **CommandText** umożliwia dodanie funkcji podobnego do procedury składowanej w bazie danych, ale w modelu magazynu należy zdefiniować element **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="a790e-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="a790e-176">Element **CommandText** nie może mieć elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="a790e-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="a790e-177">Treść elementu **CommandText** musi być prawidłową instrukcją SQL dla źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="a790e-178">Do elementu **CommandText** nie mają zastosowania żadne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="a790e-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-179">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-179">Example</span></span>

<span data-ttu-id="a790e-180">Poniższy przykład pokazuje element **funkcji** z elementem **CommandText** elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="a790e-181">Udostępnienie funkcji **UpdateProductInOrder** jako metody w obiekcie ObjectContext przez zaimportowanie jej do modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="a790e-182">DefiningQuery — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="a790e-183">Element **DefiningQuery** w języku definicji schematu magazynu (SSDL) umożliwia wykonywanie instrukcji SQL bezpośrednio w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="a790e-184">Element **DefiningQuery** jest często używany jak widok bazy danych, ale widok jest definiowany w modelu magazynu zamiast w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="a790e-185">Widok zdefiniowany w elemencie **DefiningQuery** można zamapować na typ jednostki w modelu koncepcyjnym za pomocą elementu elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="a790e-186">Mapowania te są tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="a790e-186">These mappings are read-only.</span></span>  

<span data-ttu-id="a790e-187">Następująca składnia SSDL pokazuje deklarację **obiektu EntitySet** , po którym następuje element **DefiningQuery** , który zawiera zapytanie użyte do pobrania widoku.</span><span class="sxs-lookup"><span data-stu-id="a790e-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="a790e-188">Za pomocą procedur składowanych w Entity Framework można włączyć scenariusze odczytu i zapisu w widokach.</span><span class="sxs-lookup"><span data-stu-id="a790e-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="a790e-189"> Można użyć widoku źródła danych lub widoku Entity SQL jako tabeli bazowej do pobierania danych i przetwarzania zmian przez procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="a790e-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="a790e-190">Można użyć elementu **DefiningQuery** , aby docelowa Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="a790e-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="a790e-191">Chociaż SQL Server Compact 3,5 nie obsługuje procedur składowanych, można zaimplementować podobną funkcjonalność przy użyciu elementu **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="a790e-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="a790e-192">Innym miejscem, gdzie może być przydatne, jest tworzenie procedur składowanych w celu pokonania niezgodności między typami danych używanymi w języku programowania a tymi źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="a790e-193">Można napisać **DefiningQuery** , który przyjmuje określony zestaw parametrów, a następnie wywołuje procedurę składowaną z innym zestawem parametrów, na przykład procedurą składowaną, która usuwa dane.</span><span class="sxs-lookup"><span data-stu-id="a790e-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="a790e-194">Element zależny (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="a790e-195">Element **zależny** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje zależne zakończenie ograniczenia klucza obcego (nazywanego również ograniczeniem referencyjnym).</span><span class="sxs-lookup"><span data-stu-id="a790e-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="a790e-196">Element **zależny** określa kolumnę (lub kolumny) w tabeli, która odwołuje się do kolumny klucza podstawowego (lub kolumn).</span><span class="sxs-lookup"><span data-stu-id="a790e-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="a790e-197">Elementy **PropertyRef** określają, do których kolumn odwołują się odwołania.</span><span class="sxs-lookup"><span data-stu-id="a790e-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="a790e-198">Główny element określa kolumny klucza podstawowego, do których odwołują się kolumny, które są określone w elemencie **zależnym** .</span><span class="sxs-lookup"><span data-stu-id="a790e-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="a790e-199">Element **zależny** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-200">PropertyRef (co najmniej jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="a790e-201">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-202">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-202">Applicable Attributes</span></span>

<span data-ttu-id="a790e-203">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **zależnego** .</span><span class="sxs-lookup"><span data-stu-id="a790e-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="a790e-204">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-204">Attribute Name</span></span> | <span data-ttu-id="a790e-205">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-205">Is Required</span></span> | <span data-ttu-id="a790e-206">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-207">**Rola**</span><span class="sxs-lookup"><span data-stu-id="a790e-207">**Role**</span></span>       | <span data-ttu-id="a790e-208">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-208">Yes</span></span>         | <span data-ttu-id="a790e-209">Taka sama wartość jak atrybut **roli** (jeśli jest używany) odpowiadającego elementu końcowego; w przeciwnym razie nazwa tabeli zawierającej kolumnę odwołania.</span><span class="sxs-lookup"><span data-stu-id="a790e-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-210">Do elementu **zależnego** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="a790e-211">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-212">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-213">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-213">Example</span></span>

<span data-ttu-id="a790e-214">Poniższy przykład pokazuje element skojarzenia, który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="a790e-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="a790e-215">Element **zależny** określa kolumnę **CustomerID** tabeli **Order** jako zależne zakończenie ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="a790e-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="a790e-216">Element dokumentacji (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="a790e-217">Element **dokumentacji** w języku definicji schematu magazynu (SSDL) może służyć do dostarczania informacji dotyczących obiektu, który jest zdefiniowany w elemencie nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="a790e-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="a790e-218">Element **dokumentacji** może zawierać następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-219">**Podsumowanie**: Krótki opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="a790e-220">(zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-220">(zero or one element)</span></span>
-   <span data-ttu-id="a790e-221">**LongDescription**: obszerny opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="a790e-222">(zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-223">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-223">Applicable Attributes</span></span>

<span data-ttu-id="a790e-224">Do elementu **dokumentacji** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="a790e-225">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-226">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-227">Example</span></span>

<span data-ttu-id="a790e-228">Poniższy przykład pokazuje element **dokumentacji** jako element podrzędny elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="a790e-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="a790e-229">End — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-229">End Element (SSDL)</span></span>

<span data-ttu-id="a790e-230">Element **End** w języku definicji schematu magazynu (SSDL) określa tabelę i liczbę wierszy na jednym końcu ograniczenia klucza obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="a790e-231">Element **End** może być elementem podrzędnym elementu Association lub AssociationSet elementu.</span><span class="sxs-lookup"><span data-stu-id="a790e-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="a790e-232">W każdym przypadku możliwe elementy podrzędne i odpowiednie atrybuty są różne.</span><span class="sxs-lookup"><span data-stu-id="a790e-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="a790e-233">End — element jako element podrzędny elementu Association</span><span class="sxs-lookup"><span data-stu-id="a790e-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="a790e-234">Element **End** (jako element podrzędny elementu **Association** ) określa tabelę i liczbę wierszy na końcu ograniczenia klucza obcego z atrybutami **Type** i **liczebnością** odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="a790e-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="a790e-235">Zakończenia ograniczenia klucza obcego są zdefiniowane jako część skojarzenia SSDL; Skojarzenie SSDL musi mieć dokładnie dwa punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="a790e-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="a790e-236">Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-237">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="a790e-238">OnDelete (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="a790e-239">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="a790e-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a790e-240">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-240">Applicable Attributes</span></span>

<span data-ttu-id="a790e-241">W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="a790e-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="a790e-242">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-242">Attribute Name</span></span>   | <span data-ttu-id="a790e-243">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-243">Is Required</span></span> | <span data-ttu-id="a790e-244">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-245">**Typ**</span><span class="sxs-lookup"><span data-stu-id="a790e-245">**Type**</span></span>         | <span data-ttu-id="a790e-246">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-246">Yes</span></span>         | <span data-ttu-id="a790e-247">W pełni kwalifikowana nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="a790e-248">**Rola**</span><span class="sxs-lookup"><span data-stu-id="a790e-248">**Role**</span></span>         | <span data-ttu-id="a790e-249">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-249">No</span></span>          | <span data-ttu-id="a790e-250">Wartość atrybutu **roli** w głównym lub zależnym elemencie odpowiedniego elementu ReferentialConstraint (jeśli jest używany).</span><span class="sxs-lookup"><span data-stu-id="a790e-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="a790e-251">**Kardynalność**</span><span class="sxs-lookup"><span data-stu-id="a790e-251">**Multiplicity**</span></span> | <span data-ttu-id="a790e-252">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-252">Yes</span></span>         | <span data-ttu-id="a790e-253">**1**, **0.. 1**lub **\*** w zależności od liczby wierszy, które mogą znajdować się na końcu ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="a790e-254">**1** wskazuje, że dokładnie jeden wiersz istnieje na końcu ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="a790e-255">**0.. 1** oznacza, że na końcu ograniczenia klucza obcego istnieje zero lub jeden wiersz.</span><span class="sxs-lookup"><span data-stu-id="a790e-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="a790e-256">**\*** wskazuje, że na końcu ograniczenia klucza obcego istnieje zero, jeden lub więcej wierszy.</span><span class="sxs-lookup"><span data-stu-id="a790e-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-257">Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="a790e-258">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-259">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="a790e-260">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-260">Example</span></span>

<span data-ttu-id="a790e-261">Poniższy przykład pokazuje element **skojarzenia** , który definiuje klucz obcy **\_CustomerOrders** ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="a790e-262">Wartości **liczebności** określone dla każdego elementu **końcowego** wskazują, że wiele wierszy w tabeli **Orders** może być skojarzonych z wierszem w tabeli **Customers** , ale tylko jeden wiersz w tabeli **Customers** może być skojarzony z wierszem w tabeli **Orders** .</span><span class="sxs-lookup"><span data-stu-id="a790e-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="a790e-263">Ponadto element **onDelete** wskazuje, że wszystkie wiersze w tabeli **Orders** , które odwołują się do określonego wiersza w tabeli **Customers** , zostaną usunięte, jeśli wiersz w tabeli **Customers** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="a790e-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="a790e-264">End — element jako element podrzędny elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="a790e-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="a790e-265">Element **End** (jako element podrzędny elementu **AssociationSet** ) określa tabelę na jednym końcu ograniczenia klucza obcego w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="a790e-266">Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-267">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-268">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a790e-269">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-269">Applicable Attributes</span></span>

<span data-ttu-id="a790e-270">W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="a790e-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="a790e-271">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-271">Attribute Name</span></span> | <span data-ttu-id="a790e-272">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-272">Is Required</span></span> | <span data-ttu-id="a790e-273">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-274">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="a790e-274">**EntitySet**</span></span>  | <span data-ttu-id="a790e-275">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-275">Yes</span></span>         | <span data-ttu-id="a790e-276">Nazwa zestawu jednostek SSDL znajdującego się na końcu ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="a790e-277">**Rola**</span><span class="sxs-lookup"><span data-stu-id="a790e-277">**Role**</span></span>       | <span data-ttu-id="a790e-278">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-278">No</span></span>          | <span data-ttu-id="a790e-279">Wartość jednego z atrybutów **roli** określonych dla jednego elementu **końcowego** odpowiedniego elementu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="a790e-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-280">Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="a790e-281">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-282">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="a790e-283">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-283">Example</span></span>

<span data-ttu-id="a790e-284">Poniższy przykład pokazuje element **EntityContainer** z elementem **AssociationSet** z dwoma elementami **End** :</span><span class="sxs-lookup"><span data-stu-id="a790e-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="a790e-285">EntityContainer — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="a790e-286">Element **EntityContainer** w języku definicji schematu magazynu (SSDL) opisuje strukturę bazowego źródła danych w aplikacji Entity Framework: zestawy jednostek SSDL (zdefiniowane w elementach EntitySet) reprezentują tabele w bazie danych, typy jednostek SSDL (zdefiniowane w elementach EntityType) reprezentują wiersze w tabeli, a zestawy skojarzeń (zdefiniowane w elementach AssociationSet) reprezentują ograniczenia klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="a790e-287">Kontener jednostek modelu magazynu mapuje do kontenera jednostek modelu koncepcyjnego za pomocą elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="a790e-288">Element **EntityContainer** może mieć zero lub jeden element dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="a790e-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="a790e-289">Jeśli element **dokumentacji** jest obecny, musi poprzedzać wszystkie inne elementy podrzędne.</span><span class="sxs-lookup"><span data-stu-id="a790e-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="a790e-290">Element **EntityContainer** może mieć zero lub więcej następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-291">Elementy</span><span class="sxs-lookup"><span data-stu-id="a790e-291">EntitySet</span></span>
-   <span data-ttu-id="a790e-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="a790e-292">AssociationSet</span></span>
-   <span data-ttu-id="a790e-293">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-294">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-294">Applicable Attributes</span></span>

<span data-ttu-id="a790e-295">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="a790e-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="a790e-296">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-296">Attribute Name</span></span> | <span data-ttu-id="a790e-297">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-297">Is Required</span></span> | <span data-ttu-id="a790e-298">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="a790e-299">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-299">**Name**</span></span>       | <span data-ttu-id="a790e-300">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-300">Yes</span></span>         | <span data-ttu-id="a790e-301">Nazwa kontenera jednostek.</span><span class="sxs-lookup"><span data-stu-id="a790e-301">The name of the entity container.</span></span> <span data-ttu-id="a790e-302">Ta nazwa nie może zawierać kropek (.).</span><span class="sxs-lookup"><span data-stu-id="a790e-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-303">Do elementu **EntityContainer** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="a790e-304">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-305">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-306">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-306">Example</span></span>

<span data-ttu-id="a790e-307">Poniższy przykład pokazuje element **EntityContainer** , który definiuje dwa zestawy jednostek i jeden zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="a790e-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="a790e-308">Należy pamiętać, że typ jednostki i nazwy typów skojarzeń są kwalifikowane według nazwy przestrzeni nazw modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="a790e-309">EntitySet — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="a790e-310">Element **EntitySet** w języku definicji schematu magazynu (SSDL) reprezentuje tabelę lub widok w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="a790e-311">Element EntityType w SSDL reprezentuje wiersz w tabeli lub widoku.</span><span class="sxs-lookup"><span data-stu-id="a790e-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="a790e-312">Atrybut **EntityType** elementu **EntitySet** określa konkretny typ jednostki SSDL, który reprezentuje wiersze w zestawie jednostek SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="a790e-313">Mapowanie między zestawem jednostek CSDL a zestawem jednostek SSDL jest określone w elemencie elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="a790e-314">Element **EntitySet** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-315">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="a790e-316">DefiningQuery (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="a790e-317">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-318">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-318">Applicable Attributes</span></span>

<span data-ttu-id="a790e-319">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="a790e-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="a790e-320">Niektóre atrybuty (niewymienione w tym miejscu) mogą być kwalifikowane aliasem **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="a790e-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="a790e-321">Te atrybuty są używane przez kreatora aktualizacji modelu podczas aktualizowania modelu.</span><span class="sxs-lookup"><span data-stu-id="a790e-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="a790e-322">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-322">Attribute Name</span></span> | <span data-ttu-id="a790e-323">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-323">Is Required</span></span> | <span data-ttu-id="a790e-324">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-325">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-325">**Name**</span></span>       | <span data-ttu-id="a790e-326">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-326">Yes</span></span>         | <span data-ttu-id="a790e-327">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="a790e-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="a790e-328">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="a790e-328">**EntityType**</span></span> | <span data-ttu-id="a790e-329">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-329">Yes</span></span>         | <span data-ttu-id="a790e-330">W pełni kwalifikowana nazwa typu jednostki, dla którego zestaw jednostek zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a790e-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="a790e-331">**Schematy**</span><span class="sxs-lookup"><span data-stu-id="a790e-331">**Schema**</span></span>     | <span data-ttu-id="a790e-332">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-332">No</span></span>          | <span data-ttu-id="a790e-333">Schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="a790e-334">**Tabela**</span><span class="sxs-lookup"><span data-stu-id="a790e-334">**Table**</span></span>      | <span data-ttu-id="a790e-335">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-335">No</span></span>          | <span data-ttu-id="a790e-336">Tabela bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="a790e-337">Do elementu **EntitySet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="a790e-338">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-339">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-340">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-340">Example</span></span>

<span data-ttu-id="a790e-341">Poniższy przykład pokazuje element **EntityContainer** , który ma dwa elementy **EntitySet** i jeden element **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="a790e-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="a790e-342">EntityType — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="a790e-343">Element **EntityType** w języku definicji schematu magazynu (SSDL) reprezentuje wiersz w tabeli lub widoku źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="a790e-344">Element EntitySet w SSDL reprezentuje tabelę lub widok, w którym występują wiersze.</span><span class="sxs-lookup"><span data-stu-id="a790e-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="a790e-345">Atrybut **EntityType** elementu **EntitySet** określa konkretny typ jednostki SSDL, który reprezentuje wiersze w zestawie jednostek SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="a790e-346">Mapowanie między typem jednostki SSDL i typem jednostki CSDL jest określone w elemencie EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="a790e-347">Element **EntityType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-348">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="a790e-349">Klucz (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="a790e-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="a790e-350">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-351">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-351">Applicable Attributes</span></span>

<span data-ttu-id="a790e-352">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="a790e-353">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-353">Attribute Name</span></span> | <span data-ttu-id="a790e-354">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-354">Is Required</span></span> | <span data-ttu-id="a790e-355">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-356">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-356">**Name**</span></span>       | <span data-ttu-id="a790e-357">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-357">Yes</span></span>         | <span data-ttu-id="a790e-358">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="a790e-358">The name of the entity type.</span></span> <span data-ttu-id="a790e-359">Ta wartość jest zwykle taka sama jak nazwa tabeli, w której typ jednostki reprezentuje wiersz.</span><span class="sxs-lookup"><span data-stu-id="a790e-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="a790e-360">Ta wartość nie może zawierać kropek (.).</span><span class="sxs-lookup"><span data-stu-id="a790e-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-361">Do elementu **EntityType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="a790e-362">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-363">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-364">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-364">Example</span></span>

<span data-ttu-id="a790e-365">Poniższy przykład pokazuje element **EntityType** z dwiema właściwościami:</span><span class="sxs-lookup"><span data-stu-id="a790e-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="a790e-366">Function — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-366">Function Element (SSDL)</span></span>

<span data-ttu-id="a790e-367">Element **Function** w języku definicji schematu magazynu (SSDL) określa procedurę składowaną, która istnieje w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="a790e-368">Element **Function** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-369">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-370">Parametr (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="a790e-371">CommandText (zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="a790e-372">ReturnType (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="a790e-373">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="a790e-374">Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** lub atrybutu **ReturnType** (patrz poniżej), ale nie obu.</span><span class="sxs-lookup"><span data-stu-id="a790e-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="a790e-375">Procedury składowane, które są określone w modelu magazynu, mogą być importowane do modelu koncepcyjnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a790e-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="a790e-376">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań przy użyciu procedur składowanych](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="a790e-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="a790e-377">Element **Function** może również służyć do definiowania funkcji niestandardowych w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-378">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-378">Applicable Attributes</span></span>

<span data-ttu-id="a790e-379">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Function** .</span><span class="sxs-lookup"><span data-stu-id="a790e-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="a790e-380">Niektóre atrybuty (niewymienione w tym miejscu) mogą być kwalifikowane aliasem **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="a790e-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="a790e-381">Te atrybuty są używane przez kreatora aktualizacji modelu podczas aktualizowania modelu.</span><span class="sxs-lookup"><span data-stu-id="a790e-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="a790e-382">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-382">Attribute Name</span></span>             | <span data-ttu-id="a790e-383">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-383">Is Required</span></span> | <span data-ttu-id="a790e-384">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-385">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-385">**Name**</span></span>                   | <span data-ttu-id="a790e-386">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-386">Yes</span></span>         | <span data-ttu-id="a790e-387">Nazwa procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="a790e-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="a790e-388">**Atrybuty**</span><span class="sxs-lookup"><span data-stu-id="a790e-388">**ReturnType**</span></span>             | <span data-ttu-id="a790e-389">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-389">No</span></span>          | <span data-ttu-id="a790e-390">Zwracany typ procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="a790e-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="a790e-391">**Agreguj**</span><span class="sxs-lookup"><span data-stu-id="a790e-391">**Aggregate**</span></span>              | <span data-ttu-id="a790e-392">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-392">No</span></span>          | <span data-ttu-id="a790e-393">**True** , jeśli procedura składowana zwraca wartość zagregowaną; w przeciwnym razie **false**.</span><span class="sxs-lookup"><span data-stu-id="a790e-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="a790e-394">**Wbudowan**</span><span class="sxs-lookup"><span data-stu-id="a790e-394">**BuiltIn**</span></span>                | <span data-ttu-id="a790e-395">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-395">No</span></span>          | <span data-ttu-id="a790e-396">**Prawda** , jeśli funkcja jest wbudowaną<sup>1</sup> funkcją; w przeciwnym razie **false**.</span><span class="sxs-lookup"><span data-stu-id="a790e-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="a790e-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="a790e-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="a790e-398">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-398">No</span></span>          | <span data-ttu-id="a790e-399">Nazwa procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="a790e-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="a790e-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="a790e-400">**NiladicFunction**</span></span>        | <span data-ttu-id="a790e-401">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-401">No</span></span>          | <span data-ttu-id="a790e-402">**Prawda** , jeśli funkcja jest funkcją niladic<sup>2</sup> ; W przeciwnym razie **zwraca wartość false** .</span><span class="sxs-lookup"><span data-stu-id="a790e-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="a790e-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="a790e-403">**IsComposable**</span></span>           | <span data-ttu-id="a790e-404">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-404">No</span></span>          | <span data-ttu-id="a790e-405">**Prawda** , jeśli funkcja jest funkcją składającą się z<sup>3</sup> ; W przeciwnym razie **zwraca wartość false** .</span><span class="sxs-lookup"><span data-stu-id="a790e-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="a790e-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="a790e-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="a790e-407">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-407">No</span></span>          | <span data-ttu-id="a790e-408">Wyliczenie, które definiuje semantykę typu służącą do rozpoznawania przeciążeń funkcji.</span><span class="sxs-lookup"><span data-stu-id="a790e-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="a790e-409">Wyliczenie jest zdefiniowane w manifeście dostawcy dla definicji funkcji.</span><span class="sxs-lookup"><span data-stu-id="a790e-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="a790e-410">Wartość domyślna to **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="a790e-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="a790e-411">**Schematy**</span><span class="sxs-lookup"><span data-stu-id="a790e-411">**Schema**</span></span>                 | <span data-ttu-id="a790e-412">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-412">No</span></span>          | <span data-ttu-id="a790e-413">Nazwa schematu, w którym zdefiniowano procedurę przechowywaną.</span><span class="sxs-lookup"><span data-stu-id="a790e-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="a790e-414"><sup>1</sup> Wbudowana funkcja jest funkcją, która jest zdefiniowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="a790e-415">Aby uzyskać informacje o funkcjach, które są zdefiniowane w modelu magazynu, zobacz element CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a790e-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="a790e-416"><sup>2</sup> funkcja niladic jest funkcją, która nie akceptuje parametrów i, gdy jest wywoływana, nie wymaga nawiasów.</span><span class="sxs-lookup"><span data-stu-id="a790e-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="a790e-417"><sup>3</sup> dwie funkcje są możliwe do przetworzenia, jeśli dane wyjściowe jednej funkcji mogą być danymi wejściowymi dla innej funkcji.</span><span class="sxs-lookup"><span data-stu-id="a790e-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="a790e-418">Do elementu **Function** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="a790e-419">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-420">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-421">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-421">Example</span></span>

<span data-ttu-id="a790e-422">Poniższy przykład pokazuje element **funkcji** , który odnosi się do procedury składowanej **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="a790e-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="a790e-423">Procedura składowana akceptuje dwa parametry i nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="a790e-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="a790e-424">Key — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-424">Key Element (SSDL)</span></span>

<span data-ttu-id="a790e-425">Element **Key** w języku definicji schematu magazynu (SSDL) reprezentuje klucz podstawowy tabeli w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="a790e-426">**Klucz** jest elementem podrzędnym elementu EntityType, który reprezentuje wiersz w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a790e-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="a790e-427">Klucz podstawowy jest zdefiniowany w elemencie **klucza** przez odwołanie do co najmniej jednego elementu właściwości, które są zdefiniowane dla elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="a790e-428">Element **Key** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-429">PropertyRef (co najmniej jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="a790e-430">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-430">Annotation elements</span></span>

<span data-ttu-id="a790e-431">Do elementu **Key** nie mają zastosowania żadne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="a790e-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-432">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-432">Example</span></span>

<span data-ttu-id="a790e-433">Poniższy przykład pokazuje element **EntityType** z kluczem, który odwołuje się do jednej właściwości:</span><span class="sxs-lookup"><span data-stu-id="a790e-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="a790e-434">Element onDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="a790e-435">Element **onDelete** w języku definicji schematu magazynu (SSDL) odzwierciedla zachowanie bazy danych, gdy usuwany jest wiersz, który uczestniczy w ograniczeniu klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="a790e-436">Jeśli akcja jest ustawiona na **Kaskada**, wiersze odwołujące się do usuwanego wiersza również zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="a790e-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="a790e-437">Jeśli akcja ma wartość **none**, wiersze odwołujące się do wiersza, który jest usuwany, nie są również usuwane.</span><span class="sxs-lookup"><span data-stu-id="a790e-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="a790e-438">Element **onDelete** jest elementem podrzędnym elementu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a790e-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="a790e-439">Element **onDelete** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-440">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-441">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-442">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-442">Applicable Attributes</span></span>

<span data-ttu-id="a790e-443">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **onDelete** .</span><span class="sxs-lookup"><span data-stu-id="a790e-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="a790e-444">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-444">Attribute Name</span></span> | <span data-ttu-id="a790e-445">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-445">Is Required</span></span> | <span data-ttu-id="a790e-446">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-447">**Akcja**</span><span class="sxs-lookup"><span data-stu-id="a790e-447">**Action**</span></span>     | <span data-ttu-id="a790e-448">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-448">Yes</span></span>         | <span data-ttu-id="a790e-449">**Kaskada** lub **none**.</span><span class="sxs-lookup"><span data-stu-id="a790e-449">**Cascade** or **None**.</span></span> <span data-ttu-id="a790e-450">( **Ograniczona** wartość jest prawidłowa, ale ma takie samo zachowanie jak **none**).</span><span class="sxs-lookup"><span data-stu-id="a790e-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-451">Do elementu **onDelete** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="a790e-452">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-453">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-454">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-454">Example</span></span>

<span data-ttu-id="a790e-455">Poniższy przykład pokazuje element **skojarzenia** , który definiuje klucz obcy **\_CustomerOrders** ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a790e-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="a790e-456">Element **onDelete** wskazuje, że wszystkie wiersze w tabeli **Orders** , które odwołują się do określonego wiersza w tabeli **Customers** , zostaną usunięte, jeśli wiersz w tabeli **Customers** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="a790e-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="a790e-457">Parameter — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="a790e-458">Element **Parameter** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu Function, który określa parametry procedury składowanej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="a790e-459">Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-460">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-461">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-462">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-462">Applicable Attributes</span></span>

<span data-ttu-id="a790e-463">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .</span><span class="sxs-lookup"><span data-stu-id="a790e-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="a790e-464">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-464">Attribute Name</span></span> | <span data-ttu-id="a790e-465">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-465">Is Required</span></span> | <span data-ttu-id="a790e-466">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-467">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-467">**Name**</span></span>       | <span data-ttu-id="a790e-468">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-468">Yes</span></span>         | <span data-ttu-id="a790e-469">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="a790e-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="a790e-470">**Typ**</span><span class="sxs-lookup"><span data-stu-id="a790e-470">**Type**</span></span>       | <span data-ttu-id="a790e-471">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-471">Yes</span></span>         | <span data-ttu-id="a790e-472">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="a790e-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="a790e-473">**Wyst**</span><span class="sxs-lookup"><span data-stu-id="a790e-473">**Mode**</span></span>       | <span data-ttu-id="a790e-474">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-474">No</span></span>          | <span data-ttu-id="a790e-475">**W**, **out**lub **Inout** , w zależności od tego, czy parametr jest parametrem wejściowym, wyjściowym lub wejściowym.</span><span class="sxs-lookup"><span data-stu-id="a790e-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="a790e-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="a790e-476">**MaxLength**</span></span>  | <span data-ttu-id="a790e-477">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-477">No</span></span>          | <span data-ttu-id="a790e-478">Maksymalna długość parametru.</span><span class="sxs-lookup"><span data-stu-id="a790e-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="a790e-479">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="a790e-479">**Precision**</span></span>  | <span data-ttu-id="a790e-480">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-480">No</span></span>          | <span data-ttu-id="a790e-481">Precyzja parametru.</span><span class="sxs-lookup"><span data-stu-id="a790e-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="a790e-482">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="a790e-482">**Scale**</span></span>      | <span data-ttu-id="a790e-483">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-483">No</span></span>          | <span data-ttu-id="a790e-484">Skala parametru.</span><span class="sxs-lookup"><span data-stu-id="a790e-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="a790e-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="a790e-485">**SRID**</span></span>       | <span data-ttu-id="a790e-486">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-486">No</span></span>          | <span data-ttu-id="a790e-487">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="a790e-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="a790e-488">Prawidłowe tylko dla parametrów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="a790e-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="a790e-489">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="a790e-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-490">Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="a790e-491">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-492">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-493">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-493">Example</span></span>

<span data-ttu-id="a790e-494">Poniższy przykład pokazuje element **funkcji** , który ma dwa elementy **parametrów** , które określają parametry wejściowe:</span><span class="sxs-lookup"><span data-stu-id="a790e-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="a790e-495">Element podmiotu zabezpieczeń (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="a790e-496">Element **Principal** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje główne zakończenie ograniczenia klucza obcego (nazywanego również ograniczeniem referencyjnym).</span><span class="sxs-lookup"><span data-stu-id="a790e-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="a790e-497">Element **Principal** określa kolumnę klucza podstawowego (lub kolumny) w tabeli, do której odwołuje się inna kolumna (lub kolumny).</span><span class="sxs-lookup"><span data-stu-id="a790e-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="a790e-498">Elementy **PropertyRef** określają, do których kolumn odwołują się odwołania.</span><span class="sxs-lookup"><span data-stu-id="a790e-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="a790e-499">Element zależny określa kolumny, które odwołują się do kolumn klucza podstawowego, które są określone w elemencie **Principal** .</span><span class="sxs-lookup"><span data-stu-id="a790e-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="a790e-500">Element **Principal** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="a790e-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a790e-501">PropertyRef (co najmniej jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="a790e-502">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-503">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-503">Applicable Attributes</span></span>

<span data-ttu-id="a790e-504">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **głównego** .</span><span class="sxs-lookup"><span data-stu-id="a790e-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="a790e-505">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-505">Attribute Name</span></span> | <span data-ttu-id="a790e-506">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-506">Is Required</span></span> | <span data-ttu-id="a790e-507">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-508">**Rola**</span><span class="sxs-lookup"><span data-stu-id="a790e-508">**Role**</span></span>       | <span data-ttu-id="a790e-509">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-509">Yes</span></span>         | <span data-ttu-id="a790e-510">Taka sama wartość jak atrybut **roli** (jeśli jest używany) odpowiadającego elementu końcowego; w przeciwnym razie nazwa tabeli, która zawiera kolumnę, do której się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="a790e-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-511">Do elementu **Principal** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="a790e-512">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-513">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-514">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-514">Example</span></span>

<span data-ttu-id="a790e-515">Poniższy przykład pokazuje element skojarzenia, który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="a790e-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="a790e-516">Element **Principal** określa kolumnę **IDKlienta** tabeli **Customer** jako główny koniec ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="a790e-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="a790e-517">Property — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-517">Property Element (SSDL)</span></span>

<span data-ttu-id="a790e-518">Element **Property** w języku definicji schematu magazynu (SSDL) reprezentuje kolumnę w tabeli w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="a790e-519">Elementy **Właściwości** są elementami podrzędnymi elementów EntityType, które reprezentują wiersze w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a790e-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="a790e-520">Każdy element **Właściwości** zdefiniowany dla elementu **EntityType** reprezentuje kolumnę.</span><span class="sxs-lookup"><span data-stu-id="a790e-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="a790e-521">Element **Właściwości** nie może mieć żadnych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="a790e-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-522">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-522">Applicable Attributes</span></span>

<span data-ttu-id="a790e-523">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="a790e-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="a790e-524">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-524">Attribute Name</span></span>            | <span data-ttu-id="a790e-525">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-525">Is Required</span></span> | <span data-ttu-id="a790e-526">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-527">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-527">**Name**</span></span>                  | <span data-ttu-id="a790e-528">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-528">Yes</span></span>         | <span data-ttu-id="a790e-529">Nazwa odpowiedniej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="a790e-530">**Typ**</span><span class="sxs-lookup"><span data-stu-id="a790e-530">**Type**</span></span>                  | <span data-ttu-id="a790e-531">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-531">Yes</span></span>         | <span data-ttu-id="a790e-532">Typ odpowiadającej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="a790e-533">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="a790e-533">**Nullable**</span></span>              | <span data-ttu-id="a790e-534">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-534">No</span></span>          | <span data-ttu-id="a790e-535">Wartość **true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy odpowiadająca kolumna może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="a790e-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="a790e-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="a790e-536">**DefaultValue**</span></span>          | <span data-ttu-id="a790e-537">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-537">No</span></span>          | <span data-ttu-id="a790e-538">Wartość domyślna odpowiedniej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="a790e-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="a790e-539">**MaxLength**</span></span>             | <span data-ttu-id="a790e-540">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-540">No</span></span>          | <span data-ttu-id="a790e-541">Maksymalna długość odpowiedniej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="a790e-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="a790e-542">**FixedLength**</span></span>           | <span data-ttu-id="a790e-543">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-543">No</span></span>          | <span data-ttu-id="a790e-544">**Prawda** lub **Fałsz** w zależności od tego, czy odpowiadająca wartość kolumny będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="a790e-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="a790e-545">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="a790e-545">**Precision**</span></span>             | <span data-ttu-id="a790e-546">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-546">No</span></span>          | <span data-ttu-id="a790e-547">Precyzja odpowiadającej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="a790e-548">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="a790e-548">**Scale**</span></span>                 | <span data-ttu-id="a790e-549">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-549">No</span></span>          | <span data-ttu-id="a790e-550">Skala odpowiadającej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="a790e-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="a790e-551">**Unicode**</span></span>               | <span data-ttu-id="a790e-552">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-552">No</span></span>          | <span data-ttu-id="a790e-553">**Prawda** lub **Fałsz** w zależności od tego, czy odpowiadająca wartość kolumny będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="a790e-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="a790e-554">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="a790e-554">**Collation**</span></span>             | <span data-ttu-id="a790e-555">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-555">No</span></span>          | <span data-ttu-id="a790e-556">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="a790e-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="a790e-557">**SRID**</span></span>                  | <span data-ttu-id="a790e-558">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-558">No</span></span>          | <span data-ttu-id="a790e-559">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="a790e-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="a790e-560">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="a790e-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="a790e-561">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="a790e-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="a790e-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="a790e-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="a790e-563">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-563">No</span></span>          | <span data-ttu-id="a790e-564">**Brak**, **tożsamość** (jeśli odpowiadająca wartość kolumny jest tożsamością wygenerowaną w bazie danych) lub **obliczana** (jeśli odpowiadająca wartość kolumny jest obliczana w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="a790e-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="a790e-565">Nieprawidłowy dla właściwości RowType.</span><span class="sxs-lookup"><span data-stu-id="a790e-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-566">Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="a790e-567">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-568">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-569">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-569">Example</span></span>

<span data-ttu-id="a790e-570">Poniższy przykład pokazuje element **EntityType** z dwoma podrzędnymi elementami **Właściwości** :</span><span class="sxs-lookup"><span data-stu-id="a790e-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="a790e-571">PropertyRef — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="a790e-572">Element **PropertyRef** w języku definicji schematu magazynu (SSDL) odwołuje się do właściwości zdefiniowanej w elemencie EntityType, aby wskazać, że właściwość wykona jedną z następujących ról:</span><span class="sxs-lookup"><span data-stu-id="a790e-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="a790e-573">Należy do klucza podstawowego tabeli, która reprezentuje element **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="a790e-574">Aby zdefiniować klucz podstawowy, można użyć co najmniej jednego elementu **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="a790e-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="a790e-575">Aby uzyskać więcej informacji, zobacz klucz element.</span><span class="sxs-lookup"><span data-stu-id="a790e-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="a790e-576">Być zależnym lub głównym końcem ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="a790e-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="a790e-577">Aby uzyskać więcej informacji, zobacz ReferentialConstraint element.</span><span class="sxs-lookup"><span data-stu-id="a790e-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="a790e-578">Element **PropertyRef** może mieć tylko następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="a790e-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="a790e-579">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-580">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-581">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-581">Applicable Attributes</span></span>

<span data-ttu-id="a790e-582">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="a790e-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="a790e-583">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-583">Attribute Name</span></span> | <span data-ttu-id="a790e-584">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-584">Is Required</span></span> | <span data-ttu-id="a790e-585">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="a790e-586">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="a790e-586">**Name**</span></span>       | <span data-ttu-id="a790e-587">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-587">Yes</span></span>         | <span data-ttu-id="a790e-588">Nazwa właściwości, której dotyczy odwołanie.</span><span class="sxs-lookup"><span data-stu-id="a790e-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="a790e-589">Do elementu **PropertyRef** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="a790e-590">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="a790e-591">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-592">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-592">Example</span></span>

<span data-ttu-id="a790e-593">Poniższy przykład przedstawia element **PropertyRef** służący do definiowania klucza podstawowego przez odwołanie do właściwości, która jest zdefiniowana dla elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="a790e-594">ReferentialConstraint — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="a790e-595">Element **ReferentialConstraint** w języku definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego (nazywane również ograniczeniem integralności referencyjnej) w źródłowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="a790e-596">Podmiot zabezpieczeń i zależne zakończenia ograniczenia są określane odpowiednio przez główne i zależne elementy podrzędne.</span><span class="sxs-lookup"><span data-stu-id="a790e-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="a790e-597">Kolumny, które uczestniczą w głównej i zależnych końcach, są przywoływane z elementami PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="a790e-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="a790e-598">Element **ReferentialConstraint** jest opcjonalnym elementem podrzędnym elementu Association.</span><span class="sxs-lookup"><span data-stu-id="a790e-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="a790e-599">Jeśli element **ReferentialConstraint** nie jest używany do mapowania ograniczenia FOREIGN KEY, które jest określone w elemencie **Association** , do tego celu należy użyć elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="a790e-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="a790e-600">Element **ReferentialConstraint** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="a790e-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="a790e-601">Dokumentacja (zero lub jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="a790e-602">Podmiot zabezpieczeń (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="a790e-603">Zależne (dokładnie jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="a790e-604">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="a790e-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-605">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-605">Applicable Attributes</span></span>

<span data-ttu-id="a790e-606">Do elementu **ReferentialConstraint** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="a790e-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="a790e-607">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-608">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-609">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-609">Example</span></span>

<span data-ttu-id="a790e-610">Poniższy przykład pokazuje element **skojarzenia** , który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="a790e-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="a790e-611">ReturnType — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="a790e-612">Element **ReturnType** w języku definicji schematu magazynu (SSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie **Function** .</span><span class="sxs-lookup"><span data-stu-id="a790e-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="a790e-613">Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .</span><span class="sxs-lookup"><span data-stu-id="a790e-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="a790e-614">Zwracany typ funkcji jest określany przy użyciu atrybutu **Type** lub **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="a790e-615">Element **ReturnType** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="a790e-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="a790e-616">CollectionType (jeden)</span><span class="sxs-lookup"><span data-stu-id="a790e-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="a790e-617">Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="a790e-618">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="a790e-619">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-620">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-620">Example</span></span>

<span data-ttu-id="a790e-621">W poniższym przykładzie zastosowano **funkcję** zwracającą kolekcję wierszy.</span><span class="sxs-lookup"><span data-stu-id="a790e-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="a790e-622">RowType — element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="a790e-623">Element **RowType** w języku definicji schematu magazynu (SSDL) definiuje nienazwaną strukturę jako zwracany typ dla funkcji zdefiniowanej w sklepie.</span><span class="sxs-lookup"><span data-stu-id="a790e-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="a790e-624">Element **RowType** jest elementem podrzędnym elementu **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="a790e-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="a790e-625">Element **RowType** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="a790e-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="a790e-626">Właściwość (co najmniej jedna)</span><span class="sxs-lookup"><span data-stu-id="a790e-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="a790e-627">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-627">Example</span></span>

<span data-ttu-id="a790e-628">Poniższy przykład pokazuje funkcję magazynu, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="a790e-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="a790e-629">Element schematu (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="a790e-630">Element **schematu** w języku definicji schematu magazynu (SSDL) jest elementem głównym definicji modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="a790e-631">Zawiera definicje obiektów, funkcji i kontenerów, które tworzą model magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="a790e-632">Element **schematu** może zawierać zero lub więcej z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="a790e-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="a790e-633">Skojarzenie</span><span class="sxs-lookup"><span data-stu-id="a790e-633">Association</span></span>
-   <span data-ttu-id="a790e-634">Typ entityType</span><span class="sxs-lookup"><span data-stu-id="a790e-634">EntityType</span></span>
-   <span data-ttu-id="a790e-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="a790e-635">EntityContainer</span></span>
-   <span data-ttu-id="a790e-636">Funkcja</span><span class="sxs-lookup"><span data-stu-id="a790e-636">Function</span></span>

<span data-ttu-id="a790e-637">Element **schematu** używa atrybutu **przestrzeni nazw** w celu zdefiniowania przestrzeni nazw dla obiektów typ jednostki i skojarzenie w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="a790e-638">W przestrzeni nazw żadne dwa obiekty nie mogą mieć takiej samej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a790e-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="a790e-639">Przestrzeń nazw modelu magazynu różni się od przestrzeni nazw XML elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="a790e-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="a790e-640">Przestrzeń nazw modelu magazynu (zgodnie z definicją atrybutu **Namespace** ) jest kontenerem logicznym dla typów jednostek i typów skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="a790e-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="a790e-641">Przestrzeń nazw XML (wskazywana przez atrybut **xmlns** ) elementu **schematu** jest domyślną przestrzenią nazw dla elementów podrzędnych i atrybutów elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="a790e-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="a790e-642">Przestrzenie nazw XML w formularzu https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (gdzie RRRR i MM reprezentują odpowiednio rok i miesiąc) są zarezerwowane dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="a790e-643">Elementy niestandardowe i atrybuty nie mogą znajdować się w przestrzeniach nazw, które mają ten formularz.</span><span class="sxs-lookup"><span data-stu-id="a790e-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a790e-644">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="a790e-644">Applicable Attributes</span></span>

<span data-ttu-id="a790e-645">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="a790e-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="a790e-646">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="a790e-646">Attribute Name</span></span>            | <span data-ttu-id="a790e-647">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="a790e-647">Is Required</span></span> | <span data-ttu-id="a790e-648">Wartość</span><span class="sxs-lookup"><span data-stu-id="a790e-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-649">**Przestrzeń nazw**</span><span class="sxs-lookup"><span data-stu-id="a790e-649">**Namespace**</span></span>             | <span data-ttu-id="a790e-650">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-650">Yes</span></span>         | <span data-ttu-id="a790e-651">Przestrzeń nazw modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-651">The namespace of the storage model.</span></span> <span data-ttu-id="a790e-652">Wartość atrybutu **Namespace** służy do tworzenia w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="a790e-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="a790e-653">Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w przestrzeni nazw ExampleModel. Store, a następnie w pełni kwalifikowana nazwa **obiektu EntityType** to ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="a790e-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="a790e-654">Następujące ciągi nie mogą być używane jako wartość atrybutu **Namespace** : **system**, **przejściowy**lub **EDM**.</span><span class="sxs-lookup"><span data-stu-id="a790e-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="a790e-655">Wartość atrybutu **przestrzeni nazw** nie może być taka sama jak wartość atrybutu **Namespace** w elemencie schematu CSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="a790e-656">**Użyj**</span><span class="sxs-lookup"><span data-stu-id="a790e-656">**Alias**</span></span>                 | <span data-ttu-id="a790e-657">Nie</span><span class="sxs-lookup"><span data-stu-id="a790e-657">No</span></span>          | <span data-ttu-id="a790e-658">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="a790e-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="a790e-659">Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w przestrzeni nazw ExampleModel. Store, a wartość atrybutu **aliasu** to *StorageModel*, można użyć StorageModel. Customer jako w pełni kwalifikowanej nazwy elementu **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="a790e-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="a790e-660">**Dostawca**</span><span class="sxs-lookup"><span data-stu-id="a790e-660">**Provider**</span></span>              | <span data-ttu-id="a790e-661">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-661">Yes</span></span>         | <span data-ttu-id="a790e-662">Dostawca danych.</span><span class="sxs-lookup"><span data-stu-id="a790e-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="a790e-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="a790e-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="a790e-664">Yes</span><span class="sxs-lookup"><span data-stu-id="a790e-664">Yes</span></span>         | <span data-ttu-id="a790e-665">Token wskazujący dostawcy, który manifest dostawcy ma zwrócić.</span><span class="sxs-lookup"><span data-stu-id="a790e-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="a790e-666">Nie zdefiniowano żadnego formatu dla tokenu.</span><span class="sxs-lookup"><span data-stu-id="a790e-666">No format for the token is defined.</span></span> <span data-ttu-id="a790e-667">Wartości dla tokenu są definiowane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="a790e-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="a790e-668">Aby uzyskać informacje na temat tokenów manifestu dostawcy SQL Server, zobacz SqlClient dla Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a790e-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="a790e-669">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-669">Example</span></span>

<span data-ttu-id="a790e-670">Poniższy przykład pokazuje element **schematu** , który zawiera element **EntityContainer** , dwa elementy **EntityType** i jeden element **skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="a790e-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="a790e-671">Atrybuty adnotacji</span><span class="sxs-lookup"><span data-stu-id="a790e-671">Annotation Attributes</span></span>

<span data-ttu-id="a790e-672">Atrybuty adnotacji w języku definicji schematu magazynu (SSDL) to niestandardowe atrybuty XML w modelu magazynu, które zapewniają dodatkowe metadane dotyczące elementów w modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="a790e-673">Oprócz posiadania prawidłowej struktury XML, następujące ograniczenia mają zastosowanie do atrybutów adnotacji:</span><span class="sxs-lookup"><span data-stu-id="a790e-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="a790e-674">Atrybuty adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="a790e-675">W pełni kwalifikowane nazwy jakichkolwiek dwóch atrybutów adnotacji nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="a790e-676">Do danego elementu SSDL można zastosować więcej niż jeden atrybut adnotacji.</span><span class="sxs-lookup"><span data-stu-id="a790e-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="a790e-677">Do metadanych zawartych w elementach adnotacji można uzyskać dostęp w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="a790e-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-678">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-678">Example</span></span>

<span data-ttu-id="a790e-679">Poniższy przykład pokazuje element EntityType, który ma atrybut adnotacji zastosowany do właściwości **IDZamówienia** .</span><span class="sxs-lookup"><span data-stu-id="a790e-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="a790e-680">W przykładzie pokazano również, że element adnotacji został dodany do elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="a790e-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="a790e-681">Elementy adnotacji (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="a790e-682">Elementy adnotacji w języku definicji schematu magazynu (SSDL) to niestandardowe elementy XML w modelu magazynu, które zapewniają dodatkowe metadane dotyczące modelu magazynu.</span><span class="sxs-lookup"><span data-stu-id="a790e-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="a790e-683">Oprócz posiadania prawidłowej struktury XML, następujące ograniczenia dotyczą elementów adnotacji:</span><span class="sxs-lookup"><span data-stu-id="a790e-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="a790e-684">Elementy adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="a790e-685">W pełni kwalifikowane nazwy jakichkolwiek dwóch elementów adnotacji nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="a790e-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="a790e-686">Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych danego elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="a790e-687">Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="a790e-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="a790e-688">Począwszy od .NET Framework w wersji 4, metadane zawarte w elementach adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="a790e-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="a790e-689">Przykład</span><span class="sxs-lookup"><span data-stu-id="a790e-689">Example</span></span>

<span data-ttu-id="a790e-690">Poniższy przykład pokazuje element EntityType, który ma element adnotacji (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="a790e-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="a790e-691">W przykładzie przedstawiono również atrybut adnotacji zastosowany do właściwości **IDZamówienia** .</span><span class="sxs-lookup"><span data-stu-id="a790e-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="a790e-692">Aspekty (SSDL)</span><span class="sxs-lookup"><span data-stu-id="a790e-692">Facets (SSDL)</span></span>

<span data-ttu-id="a790e-693">Zestawy reguł w języku definicji schematu magazynu (SSDL) przedstawiają ograniczenia dotyczące typów kolumn, które są określone w elementach właściwości.</span><span class="sxs-lookup"><span data-stu-id="a790e-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="a790e-694">Zestawy reguł są implementowane jako atrybuty XML w elementach **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="a790e-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="a790e-695">W poniższej tabeli opisano aspekty, które są obsługiwane w programie SSDL:</span><span class="sxs-lookup"><span data-stu-id="a790e-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="a790e-696">Aspekcie</span><span class="sxs-lookup"><span data-stu-id="a790e-696">Facet</span></span>           | <span data-ttu-id="a790e-697">Opis</span><span class="sxs-lookup"><span data-stu-id="a790e-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a790e-698">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="a790e-698">**Collation**</span></span>   | <span data-ttu-id="a790e-699">Określa sekwencję sortowania (lub sekwencję sortowania), która ma być używana podczas wykonywania operacji porównania i porządkowania na wartościach właściwości.</span><span class="sxs-lookup"><span data-stu-id="a790e-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="a790e-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="a790e-700">**FixedLength**</span></span> | <span data-ttu-id="a790e-701">Określa, czy długość wartości kolumny może się różnić.</span><span class="sxs-lookup"><span data-stu-id="a790e-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="a790e-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="a790e-702">**MaxLength**</span></span>   | <span data-ttu-id="a790e-703">Określa maksymalną długość wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="a790e-704">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="a790e-704">**Precision**</span></span>   | <span data-ttu-id="a790e-705">Dla właściwości typu **Decimal**określa liczbę cyfr, jaką może mieć wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="a790e-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="a790e-706">Dla właściwości typu **Time**, **DateTime**i **DateTimeOffset**określa liczbę cyfr ułamkowych części sekundy wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="a790e-707">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="a790e-707">**Scale**</span></span>       | <span data-ttu-id="a790e-708">Określa liczbę cyfr z prawej strony punktu dziesiętnego dla wartości kolumny.</span><span class="sxs-lookup"><span data-stu-id="a790e-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="a790e-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="a790e-709">**Unicode**</span></span>     | <span data-ttu-id="a790e-710">Wskazuje, czy wartość kolumny jest przechowywana w formacie Unicode.</span><span class="sxs-lookup"><span data-stu-id="a790e-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
