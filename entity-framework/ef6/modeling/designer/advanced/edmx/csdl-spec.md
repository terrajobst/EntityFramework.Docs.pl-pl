---
title: Specyfikacja CSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418778"
---
# <a name="csdl-specification"></a><span data-ttu-id="6eaa3-102">Specyfikacja CSDL</span><span class="sxs-lookup"><span data-stu-id="6eaa3-102">CSDL Specification</span></span>
<span data-ttu-id="6eaa3-103">Język definicji schematu koncepcyjnego (CSDL) to język oparty na języku XML, który opisuje jednostki, relacje i funkcje, które tworzą model koncepcyjny aplikacji opartej na danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="6eaa3-104">Ten model koncepcyjny może być używany przez Entity Framework lub Usługi danych programu WCF.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="6eaa3-105">Metadane, które są opisane w CSDL, są używane przez Entity Framework do mapowania jednostek i relacji, które są zdefiniowane w modelu koncepcyjnym ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="6eaa3-106">Aby uzyskać więcej informacji, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) i [Specyfikacja MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="6eaa3-107">CSDL jest implementacją Entity Framework Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="6eaa3-108">W aplikacji Entity Framework metadane modelu koncepcyjnego są ładowane z pliku. csdl (zapisanym w CSDL) do wystąpienia elementu System. Data. Metadata. Edm. EdmItemCollection i są dostępne przy użyciu metod w Klasa System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="6eaa3-109">Entity Framework używa metadanych modelu koncepcyjnego do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla źródła danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="6eaa3-110">Projektant EF przechowuje informacje o modelu koncepcyjnym w pliku. edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="6eaa3-111">W czasie kompilacji program Dr Designer używa informacji w pliku. edmx, aby utworzyć plik. csdl, który jest wymagany przez Entity Framework w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="6eaa3-112">Wersje CSDL są odróżniane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="6eaa3-113">Wersja CSDL</span><span class="sxs-lookup"><span data-stu-id="6eaa3-113">CSDL Version</span></span> | <span data-ttu-id="6eaa3-114">Przestrzeń nazw XML</span><span class="sxs-lookup"><span data-stu-id="6eaa3-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="6eaa3-115">CSDL, wersja 1</span><span class="sxs-lookup"><span data-stu-id="6eaa3-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="6eaa3-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="6eaa3-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="6eaa3-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="6eaa3-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="6eaa3-118">Element Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-118">Association Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-119">Element **Association** definiuje relację między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="6eaa3-120">Skojarzenie musi określać typy jednostek, które są uwzględnione w relacji, oraz liczbę typów jednostek na każdym końcu relacji, która jest znana jako liczebność.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="6eaa3-121">Liczebność elementu end skojarzenia może mieć wartość jeden (1), zero lub jeden (0.. 1) lub wiele (\*).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="6eaa3-122">Te informacje są określone w dwóch podrzędnych elementach end.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="6eaa3-123">Do wystąpień typu jednostki na jednym końcu skojarzenia można uzyskać dostęp za poorednictwem właściwości nawigacji lub kluczy obcych, jeśli są one uwidocznione w typie jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="6eaa3-124">W aplikacji wystąpienie skojarzenia reprezentuje określone skojarzenie między wystąpieniami typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="6eaa3-125">Wystąpienia skojarzeń są logicznie pogrupowane w zestawie skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="6eaa3-126">Element **Association** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-127">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-128">End (dokładnie dwa elementy)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="6eaa3-129">ReferentialConstraint (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-130">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-131">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-131">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-132">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Association** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="6eaa3-133">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-133">Attribute Name</span></span> | <span data-ttu-id="6eaa3-134">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-134">Is Required</span></span> | <span data-ttu-id="6eaa3-135">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="6eaa3-136">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-136">**Name**</span></span>       | <span data-ttu-id="6eaa3-137">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-137">Yes</span></span>         | <span data-ttu-id="6eaa3-138">Nazwa skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-139">Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6eaa3-140">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-141">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-142">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-142">Example</span></span>

<span data-ttu-id="6eaa3-143">Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** , gdy klucze obce nie zostały ujawnione w typach jednostek **klienta** i **zamówienia** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6eaa3-144">Wartości **liczebności** dla każdego **końca** skojarzenia wskazują, że wiele **zamówień** może być skojarzonych z **klientem**, ale tylko jeden **Klient** może być skojarzony z **kolejnością**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6eaa3-145">Ponadto element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i zostały załadowane do obiektu ObjectContext, zostaną usunięte, jeśli **Klient** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="6eaa3-146">Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** , gdy klucze obce zostały ujawnione w typach jednostek **klienta** i **zamówienia** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6eaa3-147">W przypadku wyeksponowanych kluczy obcych relacja między jednostkami jest zarządzana za pomocą elementu **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="6eaa3-148">Odpowiedni element AssociationSetMapping nie jest konieczny do mapowania tego skojarzenia ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="6eaa3-149">AssociationSet, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-150">Element **AssociationSet** w języku definicji schematu koncepcyjnego (CSDL) to logiczny kontener dla wystąpień skojarzeń tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="6eaa3-151">Zestaw skojarzeń zawiera definicję grupowania wystąpień skojarzeń, aby można je było mapować do źródła danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="6eaa3-152">Element **AssociationSet** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-153">Dokumentacja (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-154">End (dokładnie wymagane dwa elementy)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="6eaa3-155">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6eaa3-156">Atrybut **Association** określa typ skojarzenia, które zawiera zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="6eaa3-157">Zestawy jednostek, które tworzą końce zestawu skojarzeń, są określone z dokładnie dwoma podrzędnymi elementami **End** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-158">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-158">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-159">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="6eaa3-160">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-160">Attribute Name</span></span>  | <span data-ttu-id="6eaa3-161">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-161">Is Required</span></span> | <span data-ttu-id="6eaa3-162">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-163">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-163">**Name**</span></span>        | <span data-ttu-id="6eaa3-164">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-164">Yes</span></span>         | <span data-ttu-id="6eaa3-165">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-165">The name of the entity set.</span></span> <span data-ttu-id="6eaa3-166">Wartość atrybutu **name** nie może być taka sama jak wartość atrybutu **skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="6eaa3-167">**Skojarzenie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-167">**Association**</span></span> | <span data-ttu-id="6eaa3-168">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-168">Yes</span></span>         | <span data-ttu-id="6eaa3-169">W pełni kwalifikowana nazwa skojarzenia, z którą zestaw skojarzeń zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="6eaa3-170">Skojarzenie musi znajdować się w tej samej przestrzeni nazw co zestaw skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-171">Do elementu **AssociationSet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="6eaa3-172">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-173">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-174">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-174">Example</span></span>

<span data-ttu-id="6eaa3-175">Poniższy przykład pokazuje element **EntityContainer** z dwoma elementami **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="6eaa3-176">CollectionType — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-177">Element **CollectionType** w języku definicji schematu koncepcyjnego (CSDL) określa, że parametr funkcji lub typ zwracany funkcji jest kolekcją.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="6eaa3-178">Element **CollectionType** może być elementem podrzędnym elementu Parameter lub ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="6eaa3-179">Typ kolekcji można określić przy użyciu atrybutu **typu** lub jednego z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-180">**CollectionType**</span></span>
-   <span data-ttu-id="6eaa3-181">referenceType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-181">ReferenceType</span></span>
-   <span data-ttu-id="6eaa3-182">RowType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-182">RowType</span></span>
-   <span data-ttu-id="6eaa3-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6eaa3-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-184">Model nie zostanie zweryfikowany, jeśli typ kolekcji jest określony zarówno z atrybutem **typu** , jak i elementem podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-185">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-185">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-186">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6eaa3-187">Należy zauważyć, że atrybuty **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**i **Collation** mają zastosowanie tylko do kolekcji **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6eaa3-188">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-188">Attribute Name</span></span>                                                          | <span data-ttu-id="6eaa3-189">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-189">Is Required</span></span> | <span data-ttu-id="6eaa3-190">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-191">**Type**</span></span>                                                                | <span data-ttu-id="6eaa3-192">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-192">No</span></span>          | <span data-ttu-id="6eaa3-193">Typ kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6eaa3-194">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-194">**Nullable**</span></span>                                                            | <span data-ttu-id="6eaa3-195">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-195">No</span></span>          | <span data-ttu-id="6eaa3-196">**Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="6eaa3-197">> W CSDL V1, właściwość typu złożonego musi mieć `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="6eaa3-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="6eaa3-199">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-199">No</span></span>          | <span data-ttu-id="6eaa3-200">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="6eaa3-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="6eaa3-202">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-202">No</span></span>          | <span data-ttu-id="6eaa3-203">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="6eaa3-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="6eaa3-205">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-205">No</span></span>          | <span data-ttu-id="6eaa3-206">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="6eaa3-207">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-207">**Precision**</span></span>                                                           | <span data-ttu-id="6eaa3-208">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-208">No</span></span>          | <span data-ttu-id="6eaa3-209">Precyzja wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6eaa3-210">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-210">**Scale**</span></span>                                                               | <span data-ttu-id="6eaa3-211">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-211">No</span></span>          | <span data-ttu-id="6eaa3-212">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-213">**SRID**</span></span>                                                                | <span data-ttu-id="6eaa3-214">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-214">No</span></span>          | <span data-ttu-id="6eaa3-215">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-216">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="6eaa3-217">   Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="6eaa3-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-218">**Unicode**</span></span>                                                             | <span data-ttu-id="6eaa3-219">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-219">No</span></span>          | <span data-ttu-id="6eaa3-220">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="6eaa3-221">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-221">**Collation**</span></span>                                                           | <span data-ttu-id="6eaa3-222">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-222">No</span></span>          | <span data-ttu-id="6eaa3-223">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-224">Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6eaa3-225">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-226">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-227">Example</span></span>

<span data-ttu-id="6eaa3-228">Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję typów jednostek **osób** (jak określono w atrybucie **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6eaa3-229">Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6eaa3-230">Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja akceptuje jako parametr Kolekcja typów jednostek **działu** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="6eaa3-231">ComplexType — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-232">Element **complexType** definiuje strukturę danych składającą się z właściwości **EdmSimpleType** lub innych typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="6eaa3-233">  Typ złożony może być właściwością typu jednostki lub innego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="6eaa3-234">Typ złożony jest podobny do typu jednostki, w którym typ złożony definiuje dane.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="6eaa3-235">Istnieją jednak pewne kluczowe różnice między typami złożonymi i typami jednostek:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="6eaa3-236">Typy złożone nie mają tożsamości (lub kluczy) i dlatego nie mogą występować niezależnie.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="6eaa3-237">Typy złożone mogą istnieć tylko jako właściwości typów jednostek lub innych typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="6eaa3-238">Typy złożone nie mogą uczestniczyć w skojarzeniach.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="6eaa3-239">Żadne zakończenie skojarzenia nie może być typu złożonego, dlatego właściwości nawigacji nie mogą być zdefiniowane dla typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="6eaa3-240">Właściwość typu złożonego nie może mieć wartości null, ale właściwości skalarne typu złożonego mogą być ustawione na wartość null.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="6eaa3-241">Element **complexType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-242">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-243">Właściwość (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6eaa3-244">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6eaa3-245">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **complexType** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="6eaa3-246">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="6eaa3-247">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-247">Is Required</span></span> | <span data-ttu-id="6eaa3-248">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-249">Name (Nazwa)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-249">Name</span></span>                                                                                                           | <span data-ttu-id="6eaa3-250">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-250">Yes</span></span>         | <span data-ttu-id="6eaa3-251">Nazwa typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-251">The name of the complex type.</span></span> <span data-ttu-id="6eaa3-252">Nazwa typu złożonego nie może być taka sama jak nazwa innego typu złożonego, typu jednostki lub skojarzenia znajdującego się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="6eaa3-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="6eaa3-254">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-254">No</span></span>          | <span data-ttu-id="6eaa3-255">Nazwa innego typu złożonego, który jest typem podstawowym typu złożonego, który jest definiowany.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="6eaa3-256">> Ten atrybut nie ma zastosowania w CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6eaa3-257">Dziedziczenie dla typów złożonych nie jest obsługiwane w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="6eaa3-258">Abstract</span><span class="sxs-lookup"><span data-stu-id="6eaa3-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="6eaa3-259">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-259">No</span></span>          | <span data-ttu-id="6eaa3-260">**Wartość true** lub **false** (wartość domyślna) w zależności od tego, czy typ złożony jest typem abstrakcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="6eaa3-261">> Ten atrybut nie ma zastosowania w CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6eaa3-262">Typy złożone w tej wersji nie mogą być typami abstrakcyjnymi.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-263">Do elementu **complexType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="6eaa3-264">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-265">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-266">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-266">Example</span></span>

<span data-ttu-id="6eaa3-267">W poniższym przykładzie przedstawiono typ złożony, **adres**, z właściwościami **EdmSimpleType** **StreetAddress**, **miasto**, **StateOrProvince**, **Country**i **KodPocztowy**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="6eaa3-268">Aby zdefiniować **adres** typu złożonego (powyżej) jako właściwość typu jednostki, należy zadeklarować typ właściwości w definicji typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="6eaa3-269">Poniższy przykład przedstawia Właściwość **Address** jako typ złożony w typie podmiotu (**Wydawca**):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="6eaa3-270">DefiningExpression, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-271">Element **DefiningExpression** w języku definicji schematu koncepcyjnego (CSDL) zawiera wyrażenie Entity SQL, które definiuje funkcję w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="6eaa3-272">W celu sprawdzenia poprawności element **DefiningExpression** może zawierać dowolną zawartość.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="6eaa3-273">Jednak Entity Framework zgłosi wyjątek w czasie wykonywania, jeśli element **DefiningExpression** nie zawiera prawidłowych Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-274">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-274">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-275">Do elementu **DefiningExpression** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="6eaa3-276">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-277">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-278">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-278">Example</span></span>

<span data-ttu-id="6eaa3-279">W poniższym przykładzie użyto elementu **DefiningExpression** , aby zdefiniować funkcję, która zwraca liczbę lat od momentu opublikowania książki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="6eaa3-280">Zawartość elementu **DefiningExpression** jest zapisywana w Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="6eaa3-281">Element zależny (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-282">Element **zależny** w języku definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint i definiuje zależne zakończenie ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="6eaa3-283">Element **ReferentialConstraint** definiuje funkcje podobne do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6eaa3-284">W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6eaa3-285">Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6eaa3-286">Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6eaa3-287">Elementy **PropertyRef** są używane do określania, które klucze odwołują się do głównego celu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="6eaa3-288">Element **zależny** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-289">PropertyRef (co najmniej jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6eaa3-290">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-291">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-291">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-292">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **zależnego** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="6eaa3-293">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-293">Attribute Name</span></span> | <span data-ttu-id="6eaa3-294">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-294">Is Required</span></span> | <span data-ttu-id="6eaa3-295">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-296">**Rola**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-296">**Role**</span></span>       | <span data-ttu-id="6eaa3-297">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-297">Yes</span></span>         | <span data-ttu-id="6eaa3-298">Nazwa typu jednostki na elemencie zależnym końca skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-299">Do elementu **zależnego** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="6eaa3-300">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-301">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-302">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-302">Example</span></span>

<span data-ttu-id="6eaa3-303">Poniższy przykład przedstawia element **ReferentialConstraint** , który jest używany jako część definicji skojarzenia **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6eaa3-304">Właściwość **publisherID** typu jednostki **książki** tworzy zależne zakończenie ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="6eaa3-305">Element dokumentacji (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-306">Element **dokumentacji** w języku definicji schematu koncepcyjnego (CSDL) może służyć do dostarczania informacji dotyczących obiektu, który jest zdefiniowany w elemencie nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="6eaa3-307">W pliku. edmx, gdy element **dokumentacji** jest elementem podrzędnym elementu, który pojawia się jako obiekt na powierzchni projektowej programu EF Designer (na przykład jednostki, skojarzenia lub właściwości), zawartość elementu **dokumentacji** pojawi się w oknie **Właściwości** programu Visual Studio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="6eaa3-308">Element **dokumentacji** może zawierać następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-309">**Podsumowanie**: Krótki opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="6eaa3-310">(zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-310">(zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-311">**LongDescription**: obszerny opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="6eaa3-312">(zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-312">(zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-313">Elementy adnotacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-313">Annotation elements.</span></span> <span data-ttu-id="6eaa3-314">(zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-315">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-315">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-316">Do elementu **dokumentacji** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="6eaa3-317">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-318">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-319">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-319">Example</span></span>

<span data-ttu-id="6eaa3-320">Poniższy przykład pokazuje element **dokumentacji** jako element podrzędny elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="6eaa3-321">Jeśli Poniższy fragment kodu znajduje się w zawartości CSDL pliku. edmx, zawartość **podsumowania** i elementów **longDescription** pojawi się w oknie **Właściwości** programu Visual Studio po kliknięciu `Customer` typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="6eaa3-322">End — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-322">End Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-323">Element **End** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu Association lub AssociationSet elementu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="6eaa3-324">W każdym przypadku rola elementu **końcowego** jest inna, a odpowiednie atrybuty są różne.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="6eaa3-325">End — element jako element podrzędny elementu Association</span><span class="sxs-lookup"><span data-stu-id="6eaa3-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="6eaa3-326">Element **End** (jako element podrzędny elementu **Association** ) identyfikuje typ jednostki na jednym końcu skojarzenia i liczbę wystąpień typu jednostki, które mogą istnieć na tym końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="6eaa3-327">Punkty końcowe skojarzenia są zdefiniowane jako część skojarzenia; skojarzenie musi mieć dokładnie dwa punkty końcowe skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="6eaa3-328">Do wystąpienia typu jednostki na jednym końcu skojarzenia można uzyskać dostęp za poorednictwem właściwości nawigacji lub kluczy obcych, jeśli są one uwidocznione w typie jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="6eaa3-329">Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-330">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-331">OnDelete (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-332">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-333">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-333">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-334">W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="6eaa3-335">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-335">Attribute Name</span></span>   | <span data-ttu-id="6eaa3-336">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-336">Is Required</span></span> | <span data-ttu-id="6eaa3-337">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-338">**Type**</span></span>         | <span data-ttu-id="6eaa3-339">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-339">Yes</span></span>         | <span data-ttu-id="6eaa3-340">Nazwa typu jednostki na jednym końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="6eaa3-341">**Rola**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-341">**Role**</span></span>         | <span data-ttu-id="6eaa3-342">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-342">No</span></span>          | <span data-ttu-id="6eaa3-343">Nazwa punktu końcowego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-343">A name for the association end.</span></span> <span data-ttu-id="6eaa3-344">Jeśli nie podano nazwy, zostanie użyta nazwa typu jednostki na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="6eaa3-345">**Kardynalność**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-345">**Multiplicity**</span></span> | <span data-ttu-id="6eaa3-346">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-346">Yes</span></span>         | <span data-ttu-id="6eaa3-347">**1**, **0.. 1**lub **\*** w zależności od liczby wystąpień typu jednostki, które mogą znajdować się na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="6eaa3-348">**1** wskazuje, że dokładnie jedno wystąpienie typu jednostki istnieje na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="6eaa3-349">**0.. 1** oznacza, że na końcu skojarzenia istnieją wystąpienia typu jednostki, które są równe zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="6eaa3-350">**\*** wskazuje, że na końcu skojarzenia istnieje zero, jedno lub więcej wystąpień typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-351">Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6eaa3-352">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-353">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-354">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-354">Example</span></span>

<span data-ttu-id="6eaa3-355">Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6eaa3-356">Wartości **liczebności** dla każdego **końca** skojarzenia wskazują, że wiele **zamówień** może być skojarzonych z **klientem**, ale tylko jeden **Klient** może być skojarzony z **kolejnością**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6eaa3-357">Ponadto element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i które zostały załadowane do obiektu ObjectContext, zostaną usunięte, jeśli **Klient** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="6eaa3-358">End — element jako element podrzędny elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="6eaa3-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="6eaa3-359">Element **końcowy** określa jeden koniec zestawu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="6eaa3-360">Element **AssociationSet** musi zawierać dwa elementy **End** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="6eaa3-361">Informacje zawarte w elemencie **End** są używane w mapowaniu zestawu skojarzenia ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="6eaa3-362">Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-363">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-364">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-365">Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6eaa3-366">Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-367">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-367">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-368">W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="6eaa3-369">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-369">Attribute Name</span></span> | <span data-ttu-id="6eaa3-370">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-370">Is Required</span></span> | <span data-ttu-id="6eaa3-371">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-372">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-372">**EntitySet**</span></span>  | <span data-ttu-id="6eaa3-373">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-373">Yes</span></span>         | <span data-ttu-id="6eaa3-374">Nazwa elementu **EntitySet** , który definiuje jeden koniec elementu nadrzędnego obiektu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="6eaa3-375">Element **EntitySet** musi być zdefiniowany w tym samym kontenerze jednostki co element nadrzędny **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="6eaa3-376">**Rola**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-376">**Role**</span></span>       | <span data-ttu-id="6eaa3-377">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-377">No</span></span>          | <span data-ttu-id="6eaa3-378">Nazwa punktu końcowego zestawu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-378">The name of the association set end.</span></span> <span data-ttu-id="6eaa3-379">Jeśli atrybut **roli** nie jest używany, nazwa punktu końcowego zestawu skojarzenia będzie nazwą zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-380">Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6eaa3-381">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-382">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-383">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-383">Example</span></span>

<span data-ttu-id="6eaa3-384">Poniższy przykład pokazuje element **EntityContainer** z dwoma elementami **AssociationSet** , z których każdy ma dwa elementy **End** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="6eaa3-385">EntityContainer, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-386">Element **EntityContainer** w języku definicji schematu koncepcyjnego (CSDL) to logiczny kontener dla zestawów jednostek, zestawów skojarzeń i importów funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="6eaa3-387">Kontener jednostek modelu koncepcyjnego mapuje do kontenera jednostek modelu magazynu za pomocą elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="6eaa3-388">Kontener jednostek modelu magazynu opisuje strukturę bazy danych: zestawy jednostek opisują tabele, zestawy skojarzeń opisują ograniczenia FOREIGN KEY i importów funkcji opisują procedury składowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="6eaa3-389">Element **EntityContainer** może mieć zero lub jeden element dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="6eaa3-390">Jeśli element **dokumentacji** jest obecny, musi poprzedzać wszystkie elementy **EntitySet**, **AssociationSet**i **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="6eaa3-391">Element **EntityContainer** może mieć zero lub więcej następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-392">Elementy</span><span class="sxs-lookup"><span data-stu-id="6eaa3-392">EntitySet</span></span>
-   <span data-ttu-id="6eaa3-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="6eaa3-393">AssociationSet</span></span>
-   <span data-ttu-id="6eaa3-394">Element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="6eaa3-394">FunctionImport</span></span>
-   <span data-ttu-id="6eaa3-395">Elementy adnotacji</span><span class="sxs-lookup"><span data-stu-id="6eaa3-395">Annotation elements</span></span>

<span data-ttu-id="6eaa3-396">Można rozwinąć element **EntityContainer** , aby dołączyć zawartość innego **obiektu EntityContainer** znajdującego się w tej samej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="6eaa3-397">Aby dołączyć zawartość innego **obiektu EntityContainer**, w elemencie odwołującego się do **obiektu EntityContainer** ustaw wartość atrybutu **extends** na nazwę elementu **EntityContainer** , który ma zostać uwzględniony.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="6eaa3-398">Wszystkie elementy podrzędne dołączonego elementu **EntityContainer** będą traktowane jako elementy podrzędne elementu **EntityContainer** , do którego się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-399">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-399">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-400">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **using** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6eaa3-401">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-401">Attribute Name</span></span> | <span data-ttu-id="6eaa3-402">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-402">Is Required</span></span> | <span data-ttu-id="6eaa3-403">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="6eaa3-404">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-404">**Name**</span></span>       | <span data-ttu-id="6eaa3-405">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-405">Yes</span></span>         | <span data-ttu-id="6eaa3-406">Nazwa kontenera jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="6eaa3-407">**Poszerza**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-407">**Extends**</span></span>    | <span data-ttu-id="6eaa3-408">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-408">No</span></span>          | <span data-ttu-id="6eaa3-409">Nazwa innego kontenera jednostek w tej samej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-410">Do elementu **EntityContainer** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="6eaa3-411">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-412">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-413">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-413">Example</span></span>

<span data-ttu-id="6eaa3-414">Poniższy przykład pokazuje element **EntityContainer** , który definiuje trzy zestawy jednostek i dwa zestawy kojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="6eaa3-415">Element EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-416">Element **EntitySet** w języku definicji schematu koncepcyjnego jest kontenerem logicznym dla wystąpień typu jednostki i wystąpień dowolnego typu pochodzącego od tego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="6eaa3-417">Relacja między typem jednostki a zestawem jednostek jest analogiczna do relacji między wierszem a tabelą w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="6eaa3-418">Podobnie jak w przypadku wiersza, typ jednostki definiuje zestaw powiązanych danych i, podobnie jak tabela, zestaw jednostek zawiera wystąpienia tej definicji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="6eaa3-419">Zestaw jednostek zawiera konstrukcję do grupowania wystąpień typu jednostki, aby można je było zamapować na powiązane struktury danych w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="6eaa3-420">Można zdefiniować więcej niż jeden zestaw jednostek dla określonego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-421">Program Dr Designer nie obsługuje modeli koncepcyjnych, które zawierają wiele zestawów jednostek na typ.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="6eaa3-422">Element **EntitySet** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-423">Element dokumentacji (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-424">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-425">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-425">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-426">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="6eaa3-427">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-427">Attribute Name</span></span> | <span data-ttu-id="6eaa3-428">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-428">Is Required</span></span> | <span data-ttu-id="6eaa3-429">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-430">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-430">**Name**</span></span>       | <span data-ttu-id="6eaa3-431">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-431">Yes</span></span>         | <span data-ttu-id="6eaa3-432">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="6eaa3-433">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-433">**EntityType**</span></span> | <span data-ttu-id="6eaa3-434">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-434">Yes</span></span>         | <span data-ttu-id="6eaa3-435">W pełni kwalifikowana nazwa typu jednostki, dla którego zestaw jednostek zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-436">Do elementu **EntitySet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="6eaa3-437">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-438">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-439">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-439">Example</span></span>

<span data-ttu-id="6eaa3-440">Poniższy przykład pokazuje element **EntityContainer** z trzema elementami **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="6eaa3-441">Istnieje możliwość zdefiniowania wielu zestawów jednostek na typ (MEST).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="6eaa3-442">W poniższym przykładzie zdefiniowano kontener jednostek z dwoma zestawami jednostek dla typu jednostki **książki** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="6eaa3-443">EntityType — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-444">Element **EntityType** reprezentuje strukturę koncepcji najwyższego poziomu, taką jak klient lub zamówienie, w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="6eaa3-445">Typ jednostki to szablon dla wystąpień typów jednostek w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="6eaa3-446">Każdy szablon zawiera następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="6eaa3-447">Unikatowa nazwa.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-447">A unique name.</span></span> <span data-ttu-id="6eaa3-448">(Wymagane).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-448">(Required.)</span></span>
-   <span data-ttu-id="6eaa3-449">Klucz jednostki, który jest zdefiniowany przez jedną lub więcej właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="6eaa3-450">(Wymagane).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-450">(Required.)</span></span>
-   <span data-ttu-id="6eaa3-451">Właściwości zawierające dane.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-451">Properties for containing data.</span></span> <span data-ttu-id="6eaa3-452">(opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-452">(Optional.)</span></span>
-   <span data-ttu-id="6eaa3-453">Właściwości nawigacji, które umożliwiają nawigację z jednego końca skojarzenia z drugim.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="6eaa3-454">(opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-454">(Optional.)</span></span>

<span data-ttu-id="6eaa3-455">W aplikacji wystąpienie typu jednostki reprezentuje określony obiekt (na przykład konkretny klient lub zamówienie).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="6eaa3-456">Każde wystąpienie typu jednostki musi mieć unikatowy klucz jednostki w ramach zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="6eaa3-457">Dwa wystąpienia typu jednostki są uważane za równe tylko wtedy, gdy są one tego samego typu, a wartości ich kluczy jednostek są takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="6eaa3-458">Element **EntityType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-459">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-460">Klucz (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-461">Właściwość (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6eaa3-462">Element NavigationProperty (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="6eaa3-463">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-464">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-464">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-465">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="6eaa3-466">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="6eaa3-467">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-467">Is Required</span></span> | <span data-ttu-id="6eaa3-468">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-469">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="6eaa3-470">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-470">Yes</span></span>         | <span data-ttu-id="6eaa3-471">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="6eaa3-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="6eaa3-473">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-473">No</span></span>          | <span data-ttu-id="6eaa3-474">Nazwa innego typu jednostki, który jest typem podstawowym typu jednostki, który jest definiowany.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="6eaa3-475">**Streszczeń**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="6eaa3-476">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-476">No</span></span>          | <span data-ttu-id="6eaa3-477">**Wartość true** lub **false**, w zależności od tego, czy typ jednostki jest typem abstrakcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="6eaa3-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="6eaa3-479">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-479">No</span></span>          | <span data-ttu-id="6eaa3-480">**Prawda** lub **Fałsz** w zależności od tego, czy typem jednostki jest otwarty typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="6eaa3-481">> Atrybut **OpenType** ma zastosowanie tylko do typów jednostek, które są zdefiniowane w modelach koncepcyjnych, które są używane z ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-482">Do elementu **EntityType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="6eaa3-483">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-484">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-485">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-485">Example</span></span>

<span data-ttu-id="6eaa3-486">Poniższy przykład pokazuje element **EntityType** z trzema elementami **Właściwości** i dwoma elementami **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="6eaa3-487">EnumType, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-488">Element **EnumType** reprezentuje typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="6eaa3-489">Element **EnumType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-490">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-491">Członek (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="6eaa3-492">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-493">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-493">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-494">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EnumType** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="6eaa3-495">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-495">Attribute Name</span></span>     | <span data-ttu-id="6eaa3-496">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-496">Is Required</span></span> | <span data-ttu-id="6eaa3-497">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-498">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-498">**Name**</span></span>           | <span data-ttu-id="6eaa3-499">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-499">Yes</span></span>         | <span data-ttu-id="6eaa3-500">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6eaa3-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-501">**IsFlags**</span></span>        | <span data-ttu-id="6eaa3-502">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-502">No</span></span>          | <span data-ttu-id="6eaa3-503">**Wartość true** lub **false**, w zależności od tego, czy typ wyliczeniowy może być używany jako zestaw flag.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="6eaa3-504">Wartość domyślna to **false.** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="6eaa3-505">**Podstawowytype**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-505">**UnderlyingType**</span></span> | <span data-ttu-id="6eaa3-506">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-506">No</span></span>          | <span data-ttu-id="6eaa3-507">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** lub **EDM.** bajty definiujące zakres wartości typu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="6eaa3-508">  Domyślny typ podstawowy elementów wyliczenia to **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-509">Do elementu **EnumType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="6eaa3-510">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-511">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-512">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-512">Example</span></span>

<span data-ttu-id="6eaa3-513">Poniższy przykład pokazuje element **EnumType** z trzema elementami **składowymi** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="6eaa3-514">Element Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-514">Function Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-515">Element **Function** w języku definicji schematu koncepcyjnego (CSDL) służy do definiowania lub deklarowania funkcji w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="6eaa3-516">Funkcja jest definiowana przy użyciu elementu DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="6eaa3-517">Element **Function** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-518">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-519">Parametr (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="6eaa3-520">DefiningExpression (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-521">ReturnValue (funkcja) (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-522">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6eaa3-523">Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** (Function) lub atrybutu **ReturnType** (patrz poniżej), ale nie obu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6eaa3-524">Możliwe typy zwracane to dowolne EdmSimpleType, typ jednostki, typ złożony, typ wiersza lub typ ref (lub kolekcja jednego z tych typów).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-525">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-525">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-526">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Function** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="6eaa3-527">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-527">Attribute Name</span></span> | <span data-ttu-id="6eaa3-528">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-528">Is Required</span></span> | <span data-ttu-id="6eaa3-529">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6eaa3-530">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-530">**Name**</span></span>       | <span data-ttu-id="6eaa3-531">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-531">Yes</span></span>         | <span data-ttu-id="6eaa3-532">Nazwa funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-532">The name of the function.</span></span>          |
| <span data-ttu-id="6eaa3-533">**Atrybuty**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-533">**ReturnType**</span></span> | <span data-ttu-id="6eaa3-534">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-534">No</span></span>          | <span data-ttu-id="6eaa3-535">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-536">Do elementu **Function** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="6eaa3-537">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-538">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-539">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-539">Example</span></span>

<span data-ttu-id="6eaa3-540">W poniższym przykładzie użyto elementu **Function** do zdefiniowania funkcji, która zwraca liczbę lat od czasu zatrudnienia instruktora.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="6eaa3-541">FunctionImport, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-542">Element **FunctionImport** w języku definicji schematu koncepcyjnego (CSDL) reprezentuje funkcję, która jest zdefiniowana w źródle danych, ale jest dostępna dla obiektów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="6eaa3-543">Na przykład element Function w modelu magazynu może służyć do reprezentowania procedury składowanej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="6eaa3-544">Element **FunctionImport** w modelu koncepcyjnym reprezentuje odpowiednią funkcję w aplikacji Entity Framework i jest mapowany na funkcję modelu magazynu za pomocą elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="6eaa3-545">Gdy funkcja jest wywoływana w aplikacji, odpowiednia procedura składowana jest wykonywana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="6eaa3-546">Element **FunctionImport** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-547">Dokumentacja (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-548">Parametr (co najmniej jeden element jest dozwolony)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-549">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-550">ReturnType (FunctionImport) (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="6eaa3-551">Dla każdego parametru akceptowanego przez funkcję należy zdefiniować jeden element **parametru** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="6eaa3-552">Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** (FunctionImport) lub atrybutu **ReturnType** (patrz poniżej), ale nie obu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6eaa3-553">Wartość zwracanego typu musi być kolekcją EdmSimpleType, EntityType lub ComplexType.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-554">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-554">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-555">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6eaa3-556">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-556">Attribute Name</span></span>   | <span data-ttu-id="6eaa3-557">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-557">Is Required</span></span> | <span data-ttu-id="6eaa3-558">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-559">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-559">**Name**</span></span>         | <span data-ttu-id="6eaa3-560">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-560">Yes</span></span>         | <span data-ttu-id="6eaa3-561">Nazwa zaimportowanej funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="6eaa3-562">**Atrybuty**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-562">**ReturnType**</span></span>   | <span data-ttu-id="6eaa3-563">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-563">No</span></span>          | <span data-ttu-id="6eaa3-564">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-564">The type that the function returns.</span></span> <span data-ttu-id="6eaa3-565">Nie używaj tego atrybutu, jeśli funkcja nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="6eaa3-566">W przeciwnym razie wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="6eaa3-567">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-567">**EntitySet**</span></span>    | <span data-ttu-id="6eaa3-568">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-568">No</span></span>          | <span data-ttu-id="6eaa3-569">Jeśli funkcja zwraca kolekcję typów jednostek, wartość **obiektu EntitySet** musi być zestawem jednostek, do którego należy kolekcja.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6eaa3-570">W przeciwnym razie atrybut **EntitySet** nie może być używany.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="6eaa3-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-571">**IsComposable**</span></span> | <span data-ttu-id="6eaa3-572">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-572">No</span></span>          | <span data-ttu-id="6eaa3-573">Jeśli wartość jest równa true, funkcja jest możliwa do przetworzenia (funkcja zwracająca tabelę) i może być użyta w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="6eaa3-574">  Wartość domyślna to **false**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-575">Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6eaa3-576">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-577">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-578">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-578">Example</span></span>

<span data-ttu-id="6eaa3-579">Poniższy przykład pokazuje element **FunctionImport** , który akceptuje jeden parametr i zwraca kolekcję typów jednostek:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="6eaa3-580">Key — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-580">Key Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-581">Element **Key** jest elementem podrzędnym elementu EntityType i definiuje *klucz jednostki* (właściwość lub zestaw właściwości typu jednostki, który określa tożsamość).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6eaa3-582">Właściwości tworzące klucz jednostki są wybierane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="6eaa3-583">Wartości właściwości klucza jednostki muszą jednoznacznie identyfikować wystąpienie typu jednostki w ramach zestawu jednostek w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="6eaa3-584">Właściwości tworzące klucz jednostki powinny być wybierane w celu zagwarantowania unikatowej wartości wystąpień w zestawie jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="6eaa3-585">Element **Key** definiuje klucz jednostki przez odwołanie do co najmniej jednej właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="6eaa3-586">Element **Key** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-587">PropertyRef (co najmniej jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6eaa3-588">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-589">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-589">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-590">Do elementu **Key** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="6eaa3-591">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-592">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-593">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-593">Example</span></span>

<span data-ttu-id="6eaa3-594">W poniższym przykładzie zdefiniowano typ jednostki o nazwie **książka**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="6eaa3-595">Klucz jednostki jest definiowany przez odwołanie się do właściwości **ISBN** typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6eaa3-596">Właściwość **ISBN** jest dobrym wyborem dla klucza jednostki, ponieważ międzynarodowy numer książki standardowej (ISBN) jednoznacznie identyfikuje książkę.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="6eaa3-597">W poniższym przykładzie przedstawiono typ jednostki (**autor**), który ma klucz jednostki składający się z dwóch właściwości: **Nazwa** i **adres**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="6eaa3-598">Użycie **nazwy** i **adresu** dla klucza jednostki jest uzasadnionym wyborem, ponieważ dwa autorów o tej samej nazwie jest mało prawdopodobne w tym samym adresie.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="6eaa3-599">Jednak wybór dla klucza jednostki nie gwarantuje jednowzględnie unikatowych kluczy jednostki w zestawie jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="6eaa3-600">W takim przypadku zaleca się dodanie właściwości, takiej jak **AUTHORID**, która może służyć do unikatowej identyfikacji autora.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="6eaa3-601">Element członkowski (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-601">Member Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-602">Element **członkowski** jest elementem podrzędnym elementu EnumType i definiuje element członkowski typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-603">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-603">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-604">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6eaa3-605">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-605">Attribute Name</span></span> | <span data-ttu-id="6eaa3-606">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-606">Is Required</span></span> | <span data-ttu-id="6eaa3-607">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-608">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-608">**Name**</span></span>       | <span data-ttu-id="6eaa3-609">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-609">Yes</span></span>         | <span data-ttu-id="6eaa3-610">Nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6eaa3-611">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-611">**Value**</span></span>      | <span data-ttu-id="6eaa3-612">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-612">No</span></span>          | <span data-ttu-id="6eaa3-613">Wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-613">The value of the member.</span></span> <span data-ttu-id="6eaa3-614">Domyślnie pierwszy element członkowski ma wartość 0, a wartość każdego kolejnego modułu wyliczającego jest zwiększana o 1.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="6eaa3-615">Może istnieć wiele elementów członkowskich z tymi samymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-616">Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6eaa3-617">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-618">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-619">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-619">Example</span></span>

<span data-ttu-id="6eaa3-620">Poniższy przykład pokazuje element **EnumType** z trzema elementami **składowymi** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="6eaa3-621">Element NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-622">Element **NavigationProperty** definiuje właściwość nawigacji, która zawiera odwołanie do drugiego końca skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="6eaa3-623">W przeciwieństwie do właściwości zdefiniowanych za pomocą elementu właściwości, właściwości nawigacji nie definiują kształtu i charakterystyki danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="6eaa3-624">Umożliwiają one nawigowanie po skojarzeniu między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="6eaa3-625">Należy zauważyć, że właściwości nawigacji są opcjonalne na obu typach jednostek na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="6eaa3-626">Jeśli zdefiniujesz właściwość nawigacji na jednym typie jednostki na końcu skojarzenia, nie musisz definiować właściwości nawigacji dla typu jednostki na drugim końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="6eaa3-627">Typ danych zwracanych przez właściwość nawigacji jest określany przez liczebność jego zdalnego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="6eaa3-628">Załóżmy na przykład, że właściwość nawigacji, **OrdersNavProp**, istnieje w typie jednostki **klienta** i przechodzi do skojarzenia jeden-do-wielu między **klientem** i **kolejnością**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="6eaa3-629">Ze względu na to, że zdalne skojarzenie dla właściwości nawigacji ma liczebność wiele (\*), jego typ danych jest kolekcją ( **kolejności**).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="6eaa3-630">Podobnie, jeśli właściwość nawigacji, **CustomerNavProp**, istnieje w typie podmiotu **zamówienia** , jego typem danych byłby **Klient** , ponieważ liczebność zdalnego zakończenia to jeden (1).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="6eaa3-631">Element **NavigationProperty** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-632">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-633">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-634">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-634">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-635">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="6eaa3-636">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-636">Attribute Name</span></span>   | <span data-ttu-id="6eaa3-637">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-637">Is Required</span></span> | <span data-ttu-id="6eaa3-638">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-639">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-639">**Name**</span></span>         | <span data-ttu-id="6eaa3-640">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-640">Yes</span></span>         | <span data-ttu-id="6eaa3-641">Nazwa właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="6eaa3-642">**Relacje**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-642">**Relationship**</span></span> | <span data-ttu-id="6eaa3-643">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-643">Yes</span></span>         | <span data-ttu-id="6eaa3-644">Nazwa skojarzenia znajdującego się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="6eaa3-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-645">**ToRole**</span></span>       | <span data-ttu-id="6eaa3-646">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-646">Yes</span></span>         | <span data-ttu-id="6eaa3-647">Koniec skojarzenia, na którym kończy się nawigacja.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="6eaa3-648">Wartość atrybutu **ToRole** musi być taka sama jak wartość jednego z atrybutów **roli** zdefiniowanych na jednym z punktów końcowych skojarzenia (zdefiniowana w elemencie AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="6eaa3-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-649">**FromRole**</span></span>     | <span data-ttu-id="6eaa3-650">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-650">Yes</span></span>         | <span data-ttu-id="6eaa3-651">Koniec skojarzenia, z którego rozpoczyna się nawigacja.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="6eaa3-652">Wartość atrybutu **FromRole** musi być taka sama jak wartość jednego z atrybutów **roli** zdefiniowanych na jednym z punktów końcowych skojarzenia (zdefiniowana w elemencie AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-653">Do elementu **NavigationProperty** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="6eaa3-654">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-655">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-656">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-656">Example</span></span>

<span data-ttu-id="6eaa3-657">W poniższym przykładzie zdefiniowano typ jednostki (**książkę**) z dwiema właściwościami nawigacji (**PublishedBy** i **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="6eaa3-658">OnDelete — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-659">Element **onDelete** w języku definicji schematu koncepcyjnego (CSDL) definiuje zachowanie, które jest połączone ze skojarzeniem.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="6eaa3-660">Jeśli atrybut **Action** ma ustawioną wartość **kaskadową** na jednym końcu skojarzenia, powiązane typy jednostek na drugim końcu skojarzenia są usuwane, gdy typ jednostki na pierwszym końcu zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="6eaa3-661">Jeśli skojarzenie między dwoma typami jednostek jest relacją klucz podstawowy-do-podstawowego, wówczas załadowany obiekt zależny jest usuwany, gdy obiekt Principal na drugim końcu skojarzenia zostanie usunięty bez względu na specyfikację **onDelete** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="6eaa3-662">Element **onDelete** ma wpływ tylko na zachowanie aplikacji w czasie wykonywania. nie ma to wpływu na zachowanie w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="6eaa3-663">Zachowanie zdefiniowane w źródle danych powinno być takie samo jak zachowanie zdefiniowane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="6eaa3-664">Element **onDelete** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-665">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-666">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-667">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-667">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-668">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **onDelete** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="6eaa3-669">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-669">Attribute Name</span></span> | <span data-ttu-id="6eaa3-670">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-670">Is Required</span></span> | <span data-ttu-id="6eaa3-671">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-672">**Akcja**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-672">**Action**</span></span>     | <span data-ttu-id="6eaa3-673">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-673">Yes</span></span>         | <span data-ttu-id="6eaa3-674">**Kaskada** lub **none**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-674">**Cascade** or **None**.</span></span> <span data-ttu-id="6eaa3-675">W przypadku usunięcia **kaskadowych**typów jednostek zależnych zostaną usunięte, gdy typ podmiotu zabezpieczeń zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="6eaa3-676">Jeśli **nie**, typy jednostek zależnych nie zostaną usunięte, gdy typ podmiotu zabezpieczeń zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-677">Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6eaa3-678">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-679">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-680">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-680">Example</span></span>

<span data-ttu-id="6eaa3-681">Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6eaa3-682">Element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i zostały załadowane do obiektu ObjectContext, zostaną usunięte po usunięciu **klienta** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="6eaa3-683">Parameter — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-684">Element **Parameter** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu FunctionImport lub funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="6eaa3-685">Aplikacja elementu FunctionImport</span><span class="sxs-lookup"><span data-stu-id="6eaa3-685">FunctionImport Element Application</span></span>

<span data-ttu-id="6eaa3-686">Element **Parameter** (jako element podrzędny elementu **FunctionImport** ) służy do definiowania parametrów wejściowych i wyjściowych dla importów funkcji, które są zadeklarowane w CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="6eaa3-687">Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-688">Dokumentacja (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-689">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-690">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-690">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-691">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6eaa3-692">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-692">Attribute Name</span></span> | <span data-ttu-id="6eaa3-693">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-693">Is Required</span></span> | <span data-ttu-id="6eaa3-694">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-695">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-695">**Name**</span></span>       | <span data-ttu-id="6eaa3-696">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-696">Yes</span></span>         | <span data-ttu-id="6eaa3-697">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6eaa3-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-698">**Type**</span></span>       | <span data-ttu-id="6eaa3-699">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-699">Yes</span></span>         | <span data-ttu-id="6eaa3-700">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-700">The parameter type.</span></span> <span data-ttu-id="6eaa3-701">Wartość musi być typu **EDMSimpleType** lub złożonego, który znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="6eaa3-702">**Wyst**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-702">**Mode**</span></span>       | <span data-ttu-id="6eaa3-703">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-703">No</span></span>          | <span data-ttu-id="6eaa3-704">**W**, **out**lub **Inout** , w zależności od tego, czy parametr jest parametrem wejściowym, wyjściowym lub wejściowym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="6eaa3-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-705">**MaxLength**</span></span>  | <span data-ttu-id="6eaa3-706">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-706">No</span></span>          | <span data-ttu-id="6eaa3-707">Maksymalna dozwolona długość parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="6eaa3-708">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-708">**Precision**</span></span>  | <span data-ttu-id="6eaa3-709">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-709">No</span></span>          | <span data-ttu-id="6eaa3-710">Precyzja parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-711">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-711">**Scale**</span></span>      | <span data-ttu-id="6eaa3-712">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-712">No</span></span>          | <span data-ttu-id="6eaa3-713">Skala parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="6eaa3-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-714">**SRID**</span></span>       | <span data-ttu-id="6eaa3-715">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-715">No</span></span>          | <span data-ttu-id="6eaa3-716">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-717">Prawidłowe tylko dla parametrów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="6eaa3-718">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-719">Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6eaa3-720">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-721">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-722">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-722">Example</span></span>

<span data-ttu-id="6eaa3-723">Poniższy przykład pokazuje element **FunctionImport** z jednym elementem podrzędnym **parametru** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="6eaa3-724">Funkcja akceptuje jeden parametr wejściowy i zwraca kolekcję typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="6eaa3-725">Aplikacja elementu funkcji</span><span class="sxs-lookup"><span data-stu-id="6eaa3-725">Function Element Application</span></span>

<span data-ttu-id="6eaa3-726">Element **Parameter** (jako element podrzędny elementu **Function** ) definiuje parametry dla funkcji, które są zdefiniowane lub deklarowane w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="6eaa3-727">Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-728">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="6eaa3-729">CollectionType (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="6eaa3-730">ReferenceType (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="6eaa3-731">RowType (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-732">Tylko jeden z elementów **CollectionType**, **ReferenceType**lub **RowType** może być elementem podrzędnym elementu **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="6eaa3-733">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-734">Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6eaa3-735">Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-736">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-736">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-737">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6eaa3-738">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-738">Attribute Name</span></span>   | <span data-ttu-id="6eaa3-739">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-739">Is Required</span></span> | <span data-ttu-id="6eaa3-740">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-741">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-741">**Name**</span></span>         | <span data-ttu-id="6eaa3-742">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-742">Yes</span></span>         | <span data-ttu-id="6eaa3-743">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6eaa3-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-744">**Type**</span></span>         | <span data-ttu-id="6eaa3-745">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-745">No</span></span>          | <span data-ttu-id="6eaa3-746">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-746">The parameter type.</span></span> <span data-ttu-id="6eaa3-747">Parametr może być dowolnym z następujących typów (lub kolekcji tych typów):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="6eaa3-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="6eaa3-749">typ jednostki</span><span class="sxs-lookup"><span data-stu-id="6eaa3-749">entity type</span></span> <br/> <span data-ttu-id="6eaa3-750">typ złożony</span><span class="sxs-lookup"><span data-stu-id="6eaa3-750">complex type</span></span> <br/> <span data-ttu-id="6eaa3-751">Typ wiersza</span><span class="sxs-lookup"><span data-stu-id="6eaa3-751">row type</span></span> <br/> <span data-ttu-id="6eaa3-752">typ odwołania</span><span class="sxs-lookup"><span data-stu-id="6eaa3-752">reference type</span></span>                             |
| <span data-ttu-id="6eaa3-753">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-753">**Nullable**</span></span>     | <span data-ttu-id="6eaa3-754">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-754">No</span></span>          | <span data-ttu-id="6eaa3-755">**Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość **null** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="6eaa3-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-756">**DefaultValue**</span></span> | <span data-ttu-id="6eaa3-757">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-757">No</span></span>          | <span data-ttu-id="6eaa3-758">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6eaa3-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-759">**MaxLength**</span></span>    | <span data-ttu-id="6eaa3-760">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-760">No</span></span>          | <span data-ttu-id="6eaa3-761">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-762">**FixedLength**</span></span>  | <span data-ttu-id="6eaa3-763">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-763">No</span></span>          | <span data-ttu-id="6eaa3-764">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6eaa3-765">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-765">**Precision**</span></span>    | <span data-ttu-id="6eaa3-766">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-766">No</span></span>          | <span data-ttu-id="6eaa3-767">Precyzja wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6eaa3-768">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-768">**Scale**</span></span>        | <span data-ttu-id="6eaa3-769">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-769">No</span></span>          | <span data-ttu-id="6eaa3-770">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6eaa3-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-771">**SRID**</span></span>         | <span data-ttu-id="6eaa3-772">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-772">No</span></span>          | <span data-ttu-id="6eaa3-773">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-774">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6eaa3-775">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6eaa3-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-776">**Unicode**</span></span>      | <span data-ttu-id="6eaa3-777">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-777">No</span></span>          | <span data-ttu-id="6eaa3-778">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6eaa3-779">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-779">**Collation**</span></span>    | <span data-ttu-id="6eaa3-780">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-780">No</span></span>          | <span data-ttu-id="6eaa3-781">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-782">Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6eaa3-783">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-784">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-785">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-785">Example</span></span>

<span data-ttu-id="6eaa3-786">Poniższy przykład pokazuje element **funkcji** , który używa jednego **parametru** elementu podrzędnego do definiowania parametru funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="6eaa3-787">Element Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-788">Element **Principal** w języku definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje główne zakończenie ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="6eaa3-789">Element **ReferentialConstraint** definiuje funkcje podobne do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6eaa3-790">W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6eaa3-791">Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6eaa3-792">Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6eaa3-793">Elementy **PropertyRef** są używane do określania, które klucze są przywoływane przez zakończenie zależne.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="6eaa3-794">Element **Principal** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-795">PropertyRef (co najmniej jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6eaa3-796">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-797">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-797">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-798">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **głównego** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="6eaa3-799">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-799">Attribute Name</span></span> | <span data-ttu-id="6eaa3-800">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-800">Is Required</span></span> | <span data-ttu-id="6eaa3-801">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-802">**Rola**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-802">**Role**</span></span>       | <span data-ttu-id="6eaa3-803">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-803">Yes</span></span>         | <span data-ttu-id="6eaa3-804">Nazwa typu jednostki na końcu elementu głównego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-805">Do elementu **Principal** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="6eaa3-806">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-807">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-808">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-808">Example</span></span>

<span data-ttu-id="6eaa3-809">Poniższy przykład przedstawia element **ReferentialConstraint** , który jest częścią definicji skojarzenia **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6eaa3-810">Właściwość **ID** typu jednostki **wydawcy** wykonuje główne zakończenie ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="6eaa3-811">Element właściwości (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-811">Property Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-812">Element **Property** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu EntityType, elementu complexType lub elementu RowType.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="6eaa3-813">Elementy EntityType i ComplexType — aplikacje</span><span class="sxs-lookup"><span data-stu-id="6eaa3-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="6eaa3-814">Elementy **Właściwości** (jako elementy podrzędne elementu **EntityType** lub **complexType** ) definiują kształt i charakterystykę danych, które będzie zawierać wystąpienie typu jednostki lub wystąpienie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="6eaa3-815">Właściwości w modelu koncepcyjnym są analogiczne do właściwości, które są zdefiniowane w klasie.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="6eaa3-816">W taki sam sposób, w jaki właściwości klasy definiują kształt klasy i zawierają informacje o obiektach, właściwości w modelu koncepcyjnym definiują kształt typu jednostki i zawierają informacje o wystąpieniach typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="6eaa3-817">Element **Property** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-818">Element dokumentacji (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-819">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6eaa3-820">Następujące aspekty można zastosować do elementu **Property** : **nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="6eaa3-821">Aspektami są atrybuty XML, które dostarczają informacji o sposobie przechowywania wartości właściwości w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-822">Zestawy reguł mogą być stosowane tylko do właściwości typu **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-823">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-823">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-824">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6eaa3-825">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-825">Attribute Name</span></span>                                                         | <span data-ttu-id="6eaa3-826">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-826">Is Required</span></span> | <span data-ttu-id="6eaa3-827">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-828">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-828">**Name**</span></span>                                                               | <span data-ttu-id="6eaa3-829">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-829">Yes</span></span>         | <span data-ttu-id="6eaa3-830">Nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-831">**Type**</span></span>                                                               | <span data-ttu-id="6eaa3-832">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-832">Yes</span></span>         | <span data-ttu-id="6eaa3-833">Typ wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-833">The type of the property value.</span></span> <span data-ttu-id="6eaa3-834">Typ wartości właściwości musi być typem **EDMSimpleType** lub złożonym (wskazywanym przez w pełni kwalifikowaną nazwą), która znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="6eaa3-835">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-835">**Nullable**</span></span>                                                           | <span data-ttu-id="6eaa3-836">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-836">No</span></span>          | <span data-ttu-id="6eaa3-837">**Wartość true** (wartość domyślna) lub <strong>Fałsz</strong> w zależności od tego, czy właściwość może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="6eaa3-838">> W obszarze CSDL V1 właściwość typu złożonego musi mieć `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="6eaa3-840">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-840">No</span></span>          | <span data-ttu-id="6eaa3-841">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6eaa3-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="6eaa3-843">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-843">No</span></span>          | <span data-ttu-id="6eaa3-844">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="6eaa3-846">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-846">No</span></span>          | <span data-ttu-id="6eaa3-847">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6eaa3-848">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-848">**Precision**</span></span>                                                          | <span data-ttu-id="6eaa3-849">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-849">No</span></span>          | <span data-ttu-id="6eaa3-850">Precyzja wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6eaa3-851">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-851">**Scale**</span></span>                                                              | <span data-ttu-id="6eaa3-852">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-852">No</span></span>          | <span data-ttu-id="6eaa3-853">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6eaa3-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-854">**SRID**</span></span>                                                               | <span data-ttu-id="6eaa3-855">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-855">No</span></span>          | <span data-ttu-id="6eaa3-856">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-857">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6eaa3-858">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6eaa3-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-859">**Unicode**</span></span>                                                            | <span data-ttu-id="6eaa3-860">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-860">No</span></span>          | <span data-ttu-id="6eaa3-861">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6eaa3-862">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-862">**Collation**</span></span>                                                          | <span data-ttu-id="6eaa3-863">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-863">No</span></span>          | <span data-ttu-id="6eaa3-864">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="6eaa3-865">**Obsługują**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="6eaa3-866">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-866">No</span></span>          | <span data-ttu-id="6eaa3-867">**Brak** (wartość domyślna) lub **stała**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="6eaa3-868">Jeśli wartość jest ustawiona na **stała**, wartość właściwości zostanie użyta podczas optymistycznej kontroli współbieżności.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-869">Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6eaa3-870">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-871">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-872">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-872">Example</span></span>

<span data-ttu-id="6eaa3-873">Poniższy przykład pokazuje element **EntityType** z trzema elementami **Właściwości** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6eaa3-874">Poniższy przykład pokazuje element **complexType** z pięcioma elementami **Właściwości** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="6eaa3-875">Aplikacja elementu RowType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-875">RowType Element Application</span></span>

<span data-ttu-id="6eaa3-876">Elementy **Właściwości** (jako elementy podrzędne elementu **RowType** ) definiują kształt i charakterystykę danych, które mogą być przesyłane lub zwracane z funkcji zdefiniowanej przez model.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="6eaa3-877">Element **Property** może mieć dokładnie jeden z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-878">CollectionType</span></span>
-   <span data-ttu-id="6eaa3-879">referenceType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-879">ReferenceType</span></span>
-   <span data-ttu-id="6eaa3-880">RowType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-880">RowType</span></span>

<span data-ttu-id="6eaa3-881">Element **Property** może mieć dowolną liczbę elementów podrzędnych adnotacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-882">Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-883">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-883">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-884">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6eaa3-885">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-885">Attribute Name</span></span>                                                     | <span data-ttu-id="6eaa3-886">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-886">Is Required</span></span> | <span data-ttu-id="6eaa3-887">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-888">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-888">**Name**</span></span>                                                           | <span data-ttu-id="6eaa3-889">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-889">Yes</span></span>         | <span data-ttu-id="6eaa3-890">Nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-891">**Type**</span></span>                                                           | <span data-ttu-id="6eaa3-892">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-892">Yes</span></span>         | <span data-ttu-id="6eaa3-893">Typ wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-894">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-894">**Nullable**</span></span>                                                       | <span data-ttu-id="6eaa3-895">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-895">No</span></span>          | <span data-ttu-id="6eaa3-896">**Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6eaa3-897">> W CSDL 1 właściwość typu złożonego musi mieć `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6eaa3-899">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-899">No</span></span>          | <span data-ttu-id="6eaa3-900">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6eaa3-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="6eaa3-902">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-902">No</span></span>          | <span data-ttu-id="6eaa3-903">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="6eaa3-905">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-905">No</span></span>          | <span data-ttu-id="6eaa3-906">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6eaa3-907">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-907">**Precision**</span></span>                                                      | <span data-ttu-id="6eaa3-908">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-908">No</span></span>          | <span data-ttu-id="6eaa3-909">Precyzja wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6eaa3-910">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-910">**Scale**</span></span>                                                          | <span data-ttu-id="6eaa3-911">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-911">No</span></span>          | <span data-ttu-id="6eaa3-912">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6eaa3-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-913">**SRID**</span></span>                                                           | <span data-ttu-id="6eaa3-914">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-914">No</span></span>          | <span data-ttu-id="6eaa3-915">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-916">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6eaa3-917">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6eaa3-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-918">**Unicode**</span></span>                                                        | <span data-ttu-id="6eaa3-919">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-919">No</span></span>          | <span data-ttu-id="6eaa3-920">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6eaa3-921">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-921">**Collation**</span></span>                                                      | <span data-ttu-id="6eaa3-922">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-922">No</span></span>          | <span data-ttu-id="6eaa3-923">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-924">Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6eaa3-925">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-926">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6eaa3-927">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-927">Example</span></span>

<span data-ttu-id="6eaa3-928">Poniższy przykład pokazuje elementy **Właściwości** używane do definiowania kształtu zwracanego typu funkcji zdefiniowanej przez model.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="6eaa3-929">PropertyRef, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-930">Element **PropertyRef** w języku definicji schematu koncepcyjnego (CSDL) odwołuje się do właściwości typu jednostki, aby wskazać, że właściwość wykona jedną z następujących ról:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="6eaa3-931">Część klucza jednostki (właściwości lub zestawu właściwości typu jednostki, które określają tożsamość).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6eaa3-932">Do zdefiniowania klucza jednostki można użyć co najmniej jednego elementu **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="6eaa3-933">Zależne lub główne zakończenie ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="6eaa3-934">Element **PropertyRef** może zawierać tylko elementy adnotacji (zero lub więcej) jako elementy podrzędne.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-935">Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-936">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-936">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-937">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="6eaa3-938">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-938">Attribute Name</span></span> | <span data-ttu-id="6eaa3-939">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-939">Is Required</span></span> | <span data-ttu-id="6eaa3-940">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="6eaa3-941">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-941">**Name**</span></span>       | <span data-ttu-id="6eaa3-942">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-942">Yes</span></span>         | <span data-ttu-id="6eaa3-943">Nazwa właściwości, której dotyczy odwołanie.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-944">Do elementu **PropertyRef** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="6eaa3-945">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-946">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-947">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-947">Example</span></span>

<span data-ttu-id="6eaa3-948">W poniższym przykładzie zdefiniowano typ jednostki (**książkę**).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="6eaa3-949">Klucz jednostki jest definiowany przez odwołanie się do właściwości **ISBN** typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="6eaa3-950">W następnym przykładzie dwa elementy **PropertyRef** są używane do wskazywania, że dwie właściwości (**ID** i **publisherID**) są podmiotem zabezpieczeń i zależnymi końcami ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="6eaa3-951">ReferenceType — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-952">Element **ReferenceType** w języku definicji schematu koncepcyjnego (CSDL) określa odwołanie do typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="6eaa3-953">Element **ReferenceType** może być elementem podrzędnym następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="6eaa3-954">ReturnType (funkcja)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="6eaa3-955">Parametr</span><span class="sxs-lookup"><span data-stu-id="6eaa3-955">Parameter</span></span>
-   <span data-ttu-id="6eaa3-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-956">CollectionType</span></span>

<span data-ttu-id="6eaa3-957">Element **ReferenceType** jest używany podczas definiowania parametru lub zwracanego typu dla funkcji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="6eaa3-958">Element **ReferenceType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-959">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-960">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-961">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-961">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-962">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="6eaa3-963">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-963">Attribute Name</span></span> | <span data-ttu-id="6eaa3-964">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-964">Is Required</span></span> | <span data-ttu-id="6eaa3-965">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="6eaa3-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-966">**Type**</span></span>       | <span data-ttu-id="6eaa3-967">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-967">Yes</span></span>         | <span data-ttu-id="6eaa3-968">Nazwa typu obiektu, do którego występuje odwołanie.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-969">Do elementu **ReferenceType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="6eaa3-970">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-971">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-972">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-972">Example</span></span>

<span data-ttu-id="6eaa3-973">W poniższym przykładzie pokazano element **ReferenceType** używany jako element podrzędny elementu **Parameter** w funkcji zdefiniowanej przez model, która akceptuje odwołanie do typu jednostki **osoby** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="6eaa3-974">W poniższym przykładzie pokazano element **ReferenceType** używany jako element podrzędny elementu **ReturnType** (Function) w funkcji zdefiniowanej przez model, która zwraca odwołanie do typu jednostki **osoby** :</span><span class="sxs-lookup"><span data-stu-id="6eaa3-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="6eaa3-975">ReferentialConstraint, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-976">Element **ReferentialConstraint** w języku definicji schematu koncepcyjnego (CSDL) definiuje funkcję podobną do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6eaa3-977">W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6eaa3-978">Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6eaa3-979">Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="6eaa3-980">Jeśli klucz obcy, który jest narażony na jeden typ jednostki odwołuje się do właściwości innego typu jednostki, element **ReferentialConstraint** definiuje skojarzenie między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="6eaa3-981">Ponieważ element **ReferentialConstraint** zawiera informacje o tym, jak są powiązane dwa typy jednostek, w języku specyfikacji mapowania (MSL) nie jest wymagany odpowiedni element AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="6eaa3-982">Skojarzenie między dwoma typami jednostek, które nie mają uwidocznionych kluczy obcych musi mieć odpowiedni element **AssociationSetMapping** , aby mapować informacje o skojarzeniach ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="6eaa3-983">Jeśli klucz obcy nie jest uwidoczniony dla typu jednostki, element **ReferentialConstraint** może definiować tylko ograniczenie PRIMARY KEY-to-PRIMARY KEY dla typu jednostki i innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="6eaa3-984">Element **ReferentialConstraint** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-985">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-986">Podmiot zabezpieczeń (dokładnie jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="6eaa3-987">Zależne (dokładnie jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="6eaa3-988">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-989">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-989">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-990">Element **ReferentialConstraint** może mieć dowolną liczbę atrybutów adnotacji (niestandardowe atrybuty XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="6eaa3-991">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-992">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-993">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-993">Example</span></span>

<span data-ttu-id="6eaa3-994">Poniższy przykład przedstawia element **ReferentialConstraint** , który jest używany jako część definicji skojarzenia **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="6eaa3-995">ReturnType (funkcja) — element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-996">Element **ReturnType** (Function) w języku definicji schematu koncepcyjnego (CSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie Function.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="6eaa3-997">Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6eaa3-998">Zwracanymi typami mogą być dowolne **EdmSimpleType**, typ jednostki, typ złożony, typ wiersza, typ odwołania lub kolekcja jednego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="6eaa3-999">Zwracany typ funkcji można określić przy użyciu atrybutu **typu** **ReturnValue** (funkcja) lub z jednym z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1000">CollectionType</span></span>
-   <span data-ttu-id="6eaa3-1001">referenceType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1001">ReferenceType</span></span>
-   <span data-ttu-id="6eaa3-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-1003">Model nie zostanie zweryfikowany w przypadku określenia typu zwracanego funkcji z atrybutem **typu** **ReturnValue** (Function) i jednym z elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1004">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1004">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1005">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="6eaa3-1006">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1006">Attribute Name</span></span> | <span data-ttu-id="6eaa3-1007">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1007">Is Required</span></span> | <span data-ttu-id="6eaa3-1008">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6eaa3-1009">**Atrybuty**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1009">**ReturnType**</span></span> | <span data-ttu-id="6eaa3-1010">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1010">No</span></span>          | <span data-ttu-id="6eaa3-1011">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-1012">Do elementu **ReturnType** (Function) można stosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="6eaa3-1013">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1014">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-1015">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1015">Example</span></span>

<span data-ttu-id="6eaa3-1016">W poniższym przykładzie użyto elementu **Function** do zdefiniowania funkcji, która zwraca liczbę lat, w których książka była drukowana.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="6eaa3-1017">Zwróć uwagę, że typ zwracany jest określony przez atrybut **Type** elementu **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="6eaa3-1018">ReturnType (FunctionImport), element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-1019">Element **ReturnType** (FunctionImport) w języku definicji schematu koncepcyjnego (CSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="6eaa3-1020">Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6eaa3-1021">Zwracane typy mogą być dowolną kolekcją typu jednostki, typu złożonego lub **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="6eaa3-1022">Zwracany typ funkcji jest określany przy użyciu atrybutu **Type** elementu **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1023">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1023">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1024">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="6eaa3-1025">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1025">Attribute Name</span></span> | <span data-ttu-id="6eaa3-1026">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1026">Is Required</span></span> | <span data-ttu-id="6eaa3-1027">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1028">**Type**</span></span>       | <span data-ttu-id="6eaa3-1029">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1029">No</span></span>          | <span data-ttu-id="6eaa3-1030">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1030">The type that the function returns.</span></span> <span data-ttu-id="6eaa3-1031">Wartość musi być kolekcją typu ComplexType, EntityType lub EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="6eaa3-1032">**Elementy**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1032">**EntitySet**</span></span>  | <span data-ttu-id="6eaa3-1033">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1033">No</span></span>          | <span data-ttu-id="6eaa3-1034">Jeśli funkcja zwraca kolekcję typów jednostek, wartość **obiektu EntitySet** musi być zestawem jednostek, do którego należy kolekcja.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6eaa3-1035">W przeciwnym razie atrybut **EntitySet** nie może być używany.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-1036">Do elementu **ReturnType** (FunctionImport) można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="6eaa3-1037">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1038">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-1039">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1039">Example</span></span>

<span data-ttu-id="6eaa3-1040">Poniższy przykład używa elementu **FunctionImport** , który zwraca książki i wydawców.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="6eaa3-1041">Należy zauważyć, że funkcja zwraca dwa zestawy wyników i w związku z tym dwa elementy **ReturnType** (FunctionImport) są określone.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="6eaa3-1042">RowType, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-1043">Element **RowType** w języku definicji schematu koncepcyjnego (CSDL) definiuje nienazwaną strukturę jako parametr lub typ zwracany dla funkcji zdefiniowanej w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="6eaa3-1044">Element **RowType** może być elementem podrzędnym następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="6eaa3-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1045">CollectionType</span></span>
-   <span data-ttu-id="6eaa3-1046">Parametr</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1046">Parameter</span></span>
-   <span data-ttu-id="6eaa3-1047">ReturnType (funkcja)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1047">ReturnType (Function)</span></span>

<span data-ttu-id="6eaa3-1048">Element **RowType** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-1049">Właściwość (co najmniej jedna)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1049">Property (one or more)</span></span>
-   <span data-ttu-id="6eaa3-1050">Elementy adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1051">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1051">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1052">Do elementu **RowType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="6eaa3-1053">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1054">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-1055">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1055">Example</span></span>

<span data-ttu-id="6eaa3-1056">Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="6eaa3-1057">Element schematu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-1058">Element **schematu** jest głównym elementem definicji modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="6eaa3-1059">Zawiera definicje obiektów, funkcji i kontenerów, które tworzą model koncepcyjny.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="6eaa3-1060">Element **schematu** może zawierać zero lub więcej z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-1061">Używanie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1061">Using</span></span>
-   <span data-ttu-id="6eaa3-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1062">EntityContainer</span></span>
-   <span data-ttu-id="6eaa3-1063">Typ entityType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1063">EntityType</span></span>
-   <span data-ttu-id="6eaa3-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1064">EnumType</span></span>
-   <span data-ttu-id="6eaa3-1065">Skojarzenie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1065">Association</span></span>
-   <span data-ttu-id="6eaa3-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1066">ComplexType</span></span>
-   <span data-ttu-id="6eaa3-1067">Funkcja</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1067">Function</span></span>

<span data-ttu-id="6eaa3-1068">Element **schematu** może zawierać zero lub jeden z elementów adnotacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-1069">Element **Function** i elementy adnotacji są dozwolone tylko w CSDL v2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="6eaa3-1070">Element **schematu** używa atrybutu **przestrzeni nazw** do definiowania przestrzeni nazw dla typu jednostki, typu złożonego i obiektów skojarzenia w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="6eaa3-1071">W przestrzeni nazw żadne dwa obiekty nie mogą mieć takiej samej nazwy.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="6eaa3-1072">Przestrzenie nazw mogą obejmować wiele elementów **schematu** i wiele plików. csdl.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="6eaa3-1073">Przestrzeń nazw modelu koncepcyjnego różni się od przestrzeni nazw XML elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="6eaa3-1074">Przestrzeń nazw modelu koncepcyjnego (zgodnie z definicją atrybutu **Namespace** ) jest kontenerem logicznym dla typów jednostek, typów złożonych i typów skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="6eaa3-1075">Przestrzeń nazw XML (wskazywana przez atrybut **xmlns** ) elementu **schematu** jest domyślną przestrzenią nazw dla elementów podrzędnych i atrybutów elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="6eaa3-1076">Przestrzenie nazw XML w formularzu https://schemas.microsoft.com/ado/YYYY/MM/edm (gdzie RRRR i MM reprezentują odpowiednio rok i miesiąc) są zarezerwowane dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1077">Elementy niestandardowe i atrybuty nie mogą znajdować się w przestrzeniach nazw, które mają ten formularz.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1078">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1078">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1079">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="6eaa3-1080">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1080">Attribute Name</span></span> | <span data-ttu-id="6eaa3-1081">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1081">Is Required</span></span> | <span data-ttu-id="6eaa3-1082">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-1083">**Przestrzeń nazw**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1083">**Namespace**</span></span>  | <span data-ttu-id="6eaa3-1084">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1084">Yes</span></span>         | <span data-ttu-id="6eaa3-1085">Przestrzeń nazw modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="6eaa3-1086">Wartość atrybutu **Namespace** służy do tworzenia w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="6eaa3-1087">Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w prostej przestrzeni nazw. przykład. model, wówczas w pełni kwalifikowana nazwa elementu **EntityType** to SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="6eaa3-1088">Następujące ciągi nie mogą być używane jako wartość atrybutu **Namespace** : **system**, **przejściowy**lub **EDM**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="6eaa3-1089">Wartość atrybutu **przestrzeni nazw** nie może być taka sama jak wartość atrybutu **Namespace** w elemencie schematu SSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="6eaa3-1090">**Użyj**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1090">**Alias**</span></span>      | <span data-ttu-id="6eaa3-1091">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1091">No</span></span>          | <span data-ttu-id="6eaa3-1092">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6eaa3-1093">Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w prostej. przykładowej przestrzeni nazw, a wartość atrybutu **alias** jest *modelem*, można użyć model. Customer jako w pełni kwalifikowanej nazwy typu **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-1094">Do elementu **schematu** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="6eaa3-1095">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1096">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-1097">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1097">Example</span></span>

<span data-ttu-id="6eaa3-1098">Poniższy przykład pokazuje element **schematu** , który zawiera element **EntityContainer** , dwa elementy **EntityType** i jeden element **skojarzenia** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="6eaa3-1099">TypeRef, element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-1100">Element **TypeRef** w języku definicji schematu koncepcyjnego (CSDL) zawiera odwołanie do istniejącego typu nazwanego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="6eaa3-1101">Element **TypeRef** może być elementem podrzędnym elementu CollectionType, który służy do określenia, że funkcja ma kolekcję jako parametr lub typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="6eaa3-1102">Element **TypeRef** może mieć następujące elementy podrzędne (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6eaa3-1103">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6eaa3-1104">Elementy adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1105">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1105">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1106">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="6eaa3-1107">Należy zauważyć, że atrybuty **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**i **Collation** mają zastosowanie tylko do **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6eaa3-1108">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="6eaa3-1109">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1109">Is Required</span></span> | <span data-ttu-id="6eaa3-1110">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1111">**Type**</span></span>                                                           | <span data-ttu-id="6eaa3-1112">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1112">No</span></span>          | <span data-ttu-id="6eaa3-1113">Nazwa typu, do którego się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="6eaa3-1114">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="6eaa3-1115">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1115">No</span></span>          | <span data-ttu-id="6eaa3-1116">**Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6eaa3-1117">> W CSDL 1 właściwość typu złożonego musi mieć `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6eaa3-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6eaa3-1119">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1119">No</span></span>          | <span data-ttu-id="6eaa3-1120">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6eaa3-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="6eaa3-1122">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1122">No</span></span>          | <span data-ttu-id="6eaa3-1123">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6eaa3-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="6eaa3-1125">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1125">No</span></span>          | <span data-ttu-id="6eaa3-1126">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6eaa3-1127">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1127">**Precision**</span></span>                                                      | <span data-ttu-id="6eaa3-1128">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1128">No</span></span>          | <span data-ttu-id="6eaa3-1129">Precyzja wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6eaa3-1130">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1130">**Scale**</span></span>                                                          | <span data-ttu-id="6eaa3-1131">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1131">No</span></span>          | <span data-ttu-id="6eaa3-1132">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6eaa3-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1133">**SRID**</span></span>                                                           | <span data-ttu-id="6eaa3-1134">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1134">No</span></span>          | <span data-ttu-id="6eaa3-1135">Identyfikator odwołania do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6eaa3-1136">Prawidłowe tylko dla właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6eaa3-1137">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6eaa3-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="6eaa3-1139">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1139">No</span></span>          | <span data-ttu-id="6eaa3-1140">**Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6eaa3-1141">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1141">**Collation**</span></span>                                                      | <span data-ttu-id="6eaa3-1142">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1142">No</span></span>          | <span data-ttu-id="6eaa3-1143">Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-1144">Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6eaa3-1145">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1146">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-1147">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1147">Example</span></span>

<span data-ttu-id="6eaa3-1148">Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **TypeRef** (jako element podrzędny elementu **CollectionType** ), aby określić, że funkcja akceptuje kolekcję typów jednostek **działu** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="6eaa3-1149">Używanie elementu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="6eaa3-1150">Element **using** w języku definicji schematu koncepcyjnego (CSDL) importuje zawartość modelu koncepcyjnego, który istnieje w innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="6eaa3-1151">Ustawiając wartość atrybutu **Namespace** , można odwoływać się do typów jednostek, typów złożonych i typów skojarzeń, które są zdefiniowane w innym modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="6eaa3-1152">Więcej niż jeden element **using** może być elementem podrzędnym elementu **schematu** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-1153">Element **using** w CSDL nie działa tak samo jak instrukcja **using** w języku programowania.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="6eaa3-1154">Importując przestrzeń nazw za pomocą instrukcji **using** w języku programowania, nie ma wpływu na obiekty w pierwotnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="6eaa3-1155">W obszarze CSDL zaimportowana przestrzeń nazw może zawierać typ jednostki pochodzący od typu jednostki w pierwotnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="6eaa3-1156">Może to mieć wpływ na zestawy jednostek zadeklarowane w pierwotnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="6eaa3-1157">Element **using** może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="6eaa3-1158">Dokumentacja (dozwolone zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6eaa3-1159">Elementy adnotacji (dozwolone zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6eaa3-1160">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1160">Applicable Attributes</span></span>

<span data-ttu-id="6eaa3-1161">W poniższej tabeli opisano atrybuty, które można zastosować do elementu **using** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6eaa3-1162">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1162">Attribute Name</span></span> | <span data-ttu-id="6eaa3-1163">Jest wymagana</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1163">Is Required</span></span> | <span data-ttu-id="6eaa3-1164">Wartość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-1165">**Przestrzeń nazw**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1165">**Namespace**</span></span>  | <span data-ttu-id="6eaa3-1166">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1166">Yes</span></span>         | <span data-ttu-id="6eaa3-1167">Nazwa zaimportowanej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="6eaa3-1168">**Użyj**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1168">**Alias**</span></span>      | <span data-ttu-id="6eaa3-1169">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1169">Yes</span></span>         | <span data-ttu-id="6eaa3-1170">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6eaa3-1171">Chociaż ten atrybut jest wymagany, nie jest wymagane, aby był używany zamiast nazwy przestrzeni nazw do kwalifikowania nazw obiektów.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6eaa3-1172">Do elementu **using** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="6eaa3-1173">Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6eaa3-1174">W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6eaa3-1175">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1175">Example</span></span>

<span data-ttu-id="6eaa3-1176">Poniższy przykład demonstruje element **using** używany do importowania przestrzeni nazw, która jest zdefiniowana w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="6eaa3-1177">Należy zauważyć, że przestrzeń nazw dla elementu **schematu** jest wyświetlana `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="6eaa3-1178">Właściwość `Address` **obiektu EntityType** `Publisher`jest typu złożonego, który jest zdefiniowany w `ExtendedBooksModel` przestrzeni nazw (zaimportowany za **pomocą elementu using** ).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="6eaa3-1179">Atrybuty adnotacji (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="6eaa3-1180">Atrybuty adnotacji w języku definicji schematu koncepcyjnego (CSDL) to niestandardowe atrybuty XML w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="6eaa3-1181">Oprócz posiadania prawidłowej struktury XML, następujące elementy muszą mieć wartość true atrybutów adnotacji:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="6eaa3-1182">Atrybuty adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6eaa3-1183">Do danego elementu CSDL można zastosować więcej niż jeden atrybut adnotacji.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="6eaa3-1184">W pełni kwalifikowane nazwy jakichkolwiek dwóch atrybutów adnotacji nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="6eaa3-1185">Atrybuty adnotacji mogą służyć do dostarczania dodatkowych metadanych dotyczących elementów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6eaa3-1186">Do metadanych zawartych w elementach adnotacji można uzyskać dostęp w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-1187">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1187">Example</span></span>

<span data-ttu-id="6eaa3-1188">Poniższy przykład pokazuje element **EntityType** z atrybutem adnotacji (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="6eaa3-1189">W przykładzie pokazano również element adnotacji zastosowany do elementu typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="6eaa3-1190">Poniższy kod pobiera metadane w atrybucie adnotacji i zapisuje go w konsoli programu:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="6eaa3-1191">W powyższym kodzie przyjęto założenie, że plik `School.csdl` znajduje się w katalogu wyjściowym projektu i dodano następujące instrukcje `Imports` i `Using` do projektu:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="6eaa3-1192">Elementy adnotacji (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="6eaa3-1193">Elementy adnotacji w języku definicji schematu koncepcyjnego (CSDL) to niestandardowe elementy XML w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="6eaa3-1194">Oprócz posiadania prawidłowej struktury XML, następujące elementy adnotacji muszą być prawdziwe:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="6eaa3-1195">Elementy adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6eaa3-1196">Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="6eaa3-1197">W pełni kwalifikowane nazwy jakichkolwiek dwóch elementów adnotacji nie mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="6eaa3-1198">Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych danego elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="6eaa3-1199">Elementy adnotacji mogą służyć do dostarczania dodatkowych metadanych dotyczących elementów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6eaa3-1200">Począwszy od .NET Framework w wersji 4, metadane zawarte w elementach adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-1201">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1201">Example</span></span>

<span data-ttu-id="6eaa3-1202">Poniższy przykład pokazuje element **EntityType** z elementem adnotacji (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="6eaa3-1203">Przykład pokazuje również atrybut adnotacji zastosowany do elementu typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="6eaa3-1204">Poniższy kod pobiera metadane w elemencie adnotacji i zapisuje go w konsoli programu:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="6eaa3-1205">W powyższym kodzie założono, że plik szkoły. csdl znajduje się w katalogu wyjściowym projektu i dodano następujące `Imports` i instrukcje `Using` do projektu:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="6eaa3-1206">Typy modelu koncepcyjnego (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="6eaa3-1207">Język definicji schematu koncepcyjnego (CSDL) obsługuje zestaw abstrakcyjnych typów danych pierwotnych o nazwie **EDMSimpleTypes**, które definiują właściwości w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="6eaa3-1208">**EDMSimpleTypes** to serwery proxy dla typów danych pierwotnych, które są obsługiwane w magazynie lub środowisku hostingu.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="6eaa3-1209">W poniższej tabeli przedstawiono typy danych pierwotnych, które są obsługiwane przez CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="6eaa3-1210">W tabeli wymieniono również zestawy reguł, które mogą być stosowane do poszczególnych **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="6eaa3-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="6eaa3-1212">Opis</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1212">Description</span></span>                                                | <span data-ttu-id="6eaa3-1213">Odpowiednie aspekty</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="6eaa3-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="6eaa3-1215">Zawiera dane binarne.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="6eaa3-1216">MaxLength, FixedLength, nullable, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="6eaa3-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="6eaa3-1218">Zawiera wartość **true** lub **false**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="6eaa3-1219">Dopuszcza wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="6eaa3-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="6eaa3-1221">Zawiera 8-bitową liczbę całkowitą bez znaku.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="6eaa3-1222">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="6eaa3-1224">Przedstawia datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="6eaa3-1225">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="6eaa3-1227">Zawiera datę i godzinę przesunięcia w minutach od GMT.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="6eaa3-1228">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="6eaa3-1230">Zawiera wartość liczbową ze stałą dokładnością i skalą.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="6eaa3-1231">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="6eaa3-1233">Zawiera liczbę zmiennoprzecinkową z dokładnością do 15 cyfr</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="6eaa3-1234">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="6eaa3-1236">Zawiera liczbę zmiennoprzecinkową z dokładnością do 7 cyfr.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="6eaa3-1237">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="6eaa3-1239">Zawiera unikatowy identyfikator 16-bajtowy.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="6eaa3-1240">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="6eaa3-1242">Zawiera wartość 16-bitową liczbę całkowitą ze znakiem.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="6eaa3-1243">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="6eaa3-1245">Zawiera podpisaną 32-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="6eaa3-1246">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="6eaa3-1248">Zawiera podpisaną 64-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="6eaa3-1249">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1250">**EDM.**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="6eaa3-1251">Zawiera 8-bitową liczbę całkowitą ze znakiem.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="6eaa3-1252">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1253">**Edm.String**</span></span>                   | <span data-ttu-id="6eaa3-1254">Zawiera dane znakowe.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1254">Contains character data.</span></span>                                   | <span data-ttu-id="6eaa3-1255">Unicode, FixedLength, MaxLength, Collation, Precision, nullable, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="6eaa3-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="6eaa3-1257">Zawiera godzinę.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="6eaa3-1258">Precyzja, wartość null, wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6eaa3-1259">**EDM. Geography**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="6eaa3-1260">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="6eaa3-1262">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="6eaa3-1264">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="6eaa3-1266">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="6eaa3-1268">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="6eaa3-1270">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="6eaa3-1272">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1273">**EDM. Geographycollection**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="6eaa3-1274">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1275">**EDM. geometria**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="6eaa3-1276">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="6eaa3-1278">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="6eaa3-1280">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="6eaa3-1282">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="6eaa3-1284">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="6eaa3-1286">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="6eaa3-1288">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6eaa3-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="6eaa3-1290">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="6eaa3-1291">Zestawy reguł (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1291">Facets (CSDL)</span></span>

<span data-ttu-id="6eaa3-1292">Aspekty w języku definicji schematu koncepcyjnego (CSDL) przedstawiają ograniczenia dotyczące właściwości typów jednostek i typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="6eaa3-1293">Zestawy reguł są wyświetlane jako atrybuty XML dla następujących elementów CSDL:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="6eaa3-1294">Właściwość</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1294">Property</span></span>
-   <span data-ttu-id="6eaa3-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1295">TypeRef</span></span>
-   <span data-ttu-id="6eaa3-1296">Parametr</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1296">Parameter</span></span>

<span data-ttu-id="6eaa3-1297">W poniższej tabeli opisano aspekty, które są obsługiwane w CSDL.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="6eaa3-1298">Wszystkie aspekty są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1298">All facets are optional.</span></span> <span data-ttu-id="6eaa3-1299">Niektóre z wymienionych poniżej zestawów reguł są używane przez Entity Framework podczas generowania bazy danych z modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="6eaa3-1300">Aby uzyskać informacje na temat typów danych w modelu koncepcyjnym, zobacz typy modelu koncepcyjnego (CSDL).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="6eaa3-1301">Aspekcie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1301">Facet</span></span>               | <span data-ttu-id="6eaa3-1302">Opis</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="6eaa3-1303">Informacje zawarte w tym artykule dotyczą</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="6eaa3-1304">Używany do generowania bazy danych</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1304">Used for the database generation</span></span> | <span data-ttu-id="6eaa3-1305">Używane przez środowisko uruchomieniowe</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="6eaa3-1306">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1306">**Collation**</span></span>       | <span data-ttu-id="6eaa3-1307">Określa sekwencję sortowania (lub sekwencję sortowania), która ma być używana podczas wykonywania operacji porównania i porządkowania na wartościach właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="6eaa3-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6eaa3-1309">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1309">Yes</span></span>                              | <span data-ttu-id="6eaa3-1310">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1310">No</span></span>                  |
| <span data-ttu-id="6eaa3-1311">**Obsługują**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="6eaa3-1312">Wskazuje, że wartość właściwości powinna być używana do optymistycznych kontroli współbieżności.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="6eaa3-1313">Wszystkie właściwości **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6eaa3-1314">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1314">No</span></span>                               | <span data-ttu-id="6eaa3-1315">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1315">Yes</span></span>                 |
| <span data-ttu-id="6eaa3-1316">**Domyślne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1316">**Default**</span></span>         | <span data-ttu-id="6eaa3-1317">Określa wartość domyślną właściwości, jeśli nie podano żadnej wartości podczas tworzenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="6eaa3-1318">Wszystkie właściwości **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6eaa3-1319">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1319">Yes</span></span>                              | <span data-ttu-id="6eaa3-1320">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1320">Yes</span></span>                 |
| <span data-ttu-id="6eaa3-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1321">**FixedLength**</span></span>     | <span data-ttu-id="6eaa3-1322">Określa, czy długość wartości właściwości może się różnić.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="6eaa3-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6eaa3-1324">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1324">Yes</span></span>                              | <span data-ttu-id="6eaa3-1325">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1325">No</span></span>                  |
| <span data-ttu-id="6eaa3-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1326">**MaxLength**</span></span>       | <span data-ttu-id="6eaa3-1327">Określa maksymalną długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="6eaa3-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6eaa3-1329">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1329">Yes</span></span>                              | <span data-ttu-id="6eaa3-1330">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1330">No</span></span>                  |
| <span data-ttu-id="6eaa3-1331">**Wymaga**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1331">**Nullable**</span></span>        | <span data-ttu-id="6eaa3-1332">Określa, czy właściwość może mieć wartość **null** .</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="6eaa3-1333">Wszystkie właściwości **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6eaa3-1334">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1334">Yes</span></span>                              | <span data-ttu-id="6eaa3-1335">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1335">Yes</span></span>                 |
| <span data-ttu-id="6eaa3-1336">**Dokładne**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1336">**Precision**</span></span>       | <span data-ttu-id="6eaa3-1337">Dla właściwości typu **Decimal**określa liczbę cyfr, jaką może mieć wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="6eaa3-1338">Dla właściwości typu **Time**, **DateTime**i **DateTimeOffset**określa liczbę cyfr ułamkowych części sekundy wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="6eaa3-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="6eaa3-1340">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1340">Yes</span></span>                              | <span data-ttu-id="6eaa3-1341">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1341">No</span></span>                  |
| <span data-ttu-id="6eaa3-1342">**Skalowanie**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1342">**Scale**</span></span>           | <span data-ttu-id="6eaa3-1343">Określa liczbę cyfr z prawej strony punktu dziesiętnego dla wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="6eaa3-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="6eaa3-1345">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1345">Yes</span></span>                              | <span data-ttu-id="6eaa3-1346">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1346">No</span></span>                  |
| <span data-ttu-id="6eaa3-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1347">**SRID**</span></span>            | <span data-ttu-id="6eaa3-1348">Określa identyfikator systemu odwołań do systemu przestrzennego.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="6eaa3-1349">Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="6eaa3-1350">**EDM. Geography, EDM. geographyPoint względem, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. Geographycollection, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="6eaa3-1351">Nie</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1351">No</span></span>                               | <span data-ttu-id="6eaa3-1352">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1352">Yes</span></span>                 |
| <span data-ttu-id="6eaa3-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1353">**Unicode**</span></span>         | <span data-ttu-id="6eaa3-1354">Wskazuje, czy wartość właściwości jest przechowywana w formacie Unicode.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="6eaa3-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6eaa3-1356">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1356">Yes</span></span>                              | <span data-ttu-id="6eaa3-1357">Yes</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="6eaa3-1358">Podczas generowania bazy danych z modelu koncepcyjnego Kreator generowania bazy danych rozpozna wartość atrybutu **StoreGeneratedPattern** w elemencie **Property** , jeśli znajduje się w następującej przestrzeni nazw: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="6eaa3-1359">Obsługiwane wartości atrybutu to **tożsamość** i **obliczana**.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="6eaa3-1360">Wartość **tożsamości** spowoduje utworzenie kolumny bazy danych o wartości tożsamości wygenerowanej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="6eaa3-1361">Wartość **obliczana** spowoduje wygenerowanie kolumny z wartością obliczaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="6eaa3-1362">Przykład</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1362">Example</span></span>

<span data-ttu-id="6eaa3-1363">Poniższy przykład przedstawia aspekty stosowane do właściwości typu jednostki:</span><span class="sxs-lookup"><span data-stu-id="6eaa3-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
