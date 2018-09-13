---
title: Specyfikacja CSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: f5bf0dc75a8195e9af979c9e044f36171f46c9b7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490522"
---
# <a name="csdl-specification"></a><span data-ttu-id="0f6d9-102">Specyfikacja CSDL</span><span class="sxs-lookup"><span data-stu-id="0f6d9-102">CSDL Specification</span></span>
<span data-ttu-id="0f6d9-103">Język definicji schematu koncepcyjnego (CSDL) to oparty na standardzie XML język, który opisuje jednostki, relacje i funkcje, które tworzą model koncepcyjny aplikacji opartych na danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="0f6d9-104">Ten model koncepcyjny może służyć przez Entity Framework lub usługi danych WCF.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="0f6d9-105">Metadane opisujące z CSDL jest używany przez program Entity Framework do mapowania jednostek i relacji, które są zdefiniowane w modelu koncepcyjnym ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="0f6d9-106">Aby uzyskać więcej informacji, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) i [Specyfikacja MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="0f6d9-107">CSDL to implementacja programu Entity Framework modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="0f6d9-108">W aplikacji Entity Framework metadanych modelu koncepcyjnego jest ładowany z pliku .csdl (napisanego w CSDL) do wystąpienia System.Data.Metadata.Edm.EdmItemCollection i są dostępne za pomocą metody Klasa System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="0f6d9-109">Entity Framework używa modelu koncepcyjnego metadanych do translacji zapytania względem modelu koncepcyjnego polecenia specyficznymi dla źródła danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="0f6d9-110">W Projektancie platformy EF przechowuje informacje o modelu koncepcyjnego w pliku edmx w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="0f6d9-111">W czasie kompilacji projektancie platformy EF używa informacji w pliku edmx można utworzyć pliku .csdl, które są wymagane przez Entity Framework w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="0f6d9-112">Wersje CSDL są zróżnicowane według przestrzeni nazw XML.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="0f6d9-113">Wersja CSDL</span><span class="sxs-lookup"><span data-stu-id="0f6d9-113">CSDL Version</span></span> | <span data-ttu-id="0f6d9-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="0f6d9-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="0f6d9-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="0f6d9-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="0f6d9-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="0f6d9-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="0f6d9-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="0f6d9-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="0f6d9-118">Elementu Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-118">Association Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-119">**Skojarzenia** element definiuje relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="0f6d9-120">Skojarzenia należy określić typy jednostek, które są zaangażowane w relacji i możliwa liczba typów jednostek na każdym końcu relacji, który jest znany jako liczebności.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="0f6d9-121">Liczebność elementu end skojarzenia mogą mieć wartość równą jeden (1), zero lub jeden (od 0 do 1) lub wielu (\*).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="0f6d9-122">Informacja ta jest określona w dwa elementy End podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="0f6d9-123">Wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub kluczy obcych, jeśli są one widoczne na typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="0f6d9-124">W aplikacji wystąpienia skojarzenia reprezentuje określone skojarzenia między wystąpieniami typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="0f6d9-125">Skojarzenie wystąpienia są logicznie pogrupowane w zestawie skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="0f6d9-126">**Skojarzenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-127">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-128">Końcowy (elementy dokładnie 2)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="0f6d9-129">ReferentialConstraint (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-130">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-131">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-131">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-132">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="0f6d9-133">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-133">Attribute Name</span></span> | <span data-ttu-id="0f6d9-134">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-134">Is Required</span></span> | <span data-ttu-id="0f6d9-135">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="0f6d9-136">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-136">**Name**</span></span>       | <span data-ttu-id="0f6d9-137">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-137">Yes</span></span>         | <span data-ttu-id="0f6d9-138">Nazwa skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-139">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="0f6d9-140">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-141">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-142">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-142">Example</span></span>

<span data-ttu-id="0f6d9-143">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenie po klucze obce nie być narażone na **klienta** i  **Kolejność** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="0f6d9-144">**Liczebność** wartości dla każdej **zakończenia** skojarzenia wskazują tę liczbę **zamówienia** może być skojarzony z **klienta**, ale tylko jeden **klienta** może być skojarzony z **kolejności**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="0f6d9-145">Ponadto **OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i zostały załadowane do obiektu ObjectContext zostaną usunięte. Jeśli **klienta** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="0f6d9-146">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenie po klucze obce być narażone na **klienta** i  **Kolejność** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="0f6d9-147">Za pomocą kluczy obcych widoczne, relacji między jednostkami odbywa się za pomocą **ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="0f6d9-148">Odpowiedni element AssociationSetMapping nie jest konieczne do mapowania tego skojarzenia ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="0f6d9-149">Obiekt AssociationSet — Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-150">**AssociationSet** element język definicji schematu koncepcyjnego (CSDL) to kontener logiczny dla wystąpień skojarzeń tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="0f6d9-151">Zestaw skojarzeń zawiera definicję dla grupowania wystąpień skojarzeń, dzięki czemu mogą być mapowane do źródła danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="0f6d9-152">**AssociationSet** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-153">Dokumentacja (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-154">Końcowy (dokładnie dwa elementy wymagane)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="0f6d9-155">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="0f6d9-156">**Skojarzenia** atrybut określa typ powiązania, który zawiera zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="0f6d9-157">Zestawy jednostek, składających się kończy się zestawu skojarzeń są określane za pomocą podrzędnej dokładnie dwóch **zakończenia** elementów.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-158">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-158">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-159">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="0f6d9-160">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-160">Attribute Name</span></span>  | <span data-ttu-id="0f6d9-161">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-161">Is Required</span></span> | <span data-ttu-id="0f6d9-162">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-163">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-163">**Name**</span></span>        | <span data-ttu-id="0f6d9-164">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-164">Yes</span></span>         | <span data-ttu-id="0f6d9-165">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-165">The name of the entity set.</span></span> <span data-ttu-id="0f6d9-166">Wartość **nazwa** atrybut nie może być taka sama jak wartość **skojarzenia** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="0f6d9-167">**Skojarzenie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-167">**Association**</span></span> | <span data-ttu-id="0f6d9-168">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-168">Yes</span></span>         | <span data-ttu-id="0f6d9-169">W pełni kwalifikowana nazwa skojarzenia, które zestawu skojarzeń zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="0f6d9-170">Skojarzenie musi być w tej samej przestrzeni nazw jako zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-171">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="0f6d9-172">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-173">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-174">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-174">Example</span></span>

<span data-ttu-id="0f6d9-175">W poniższym przykładzie przedstawiono **EntityContainer** elementu przy użyciu dwóch **AssociationSet** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="0f6d9-176">Element CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-177">**CollectionType** element język definicji schematu koncepcyjnego (CSDL) określa, że parametr funkcji lub funkcji zwracany typ jest kolekcją.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="0f6d9-178">**CollectionType** element może być elementem podrzędnym elementu parametru lub element ReturnType (funkcja).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="0f6d9-179">Typ kolekcji można określić za pomocą **typu** atrybutu lub jedną z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-180">**Typ CollectionType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-180">**CollectionType**</span></span>
-   <span data-ttu-id="0f6d9-181">Element referenceType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-181">ReferenceType</span></span>
-   <span data-ttu-id="0f6d9-182">RowType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-182">RowType</span></span>
-   <span data-ttu-id="0f6d9-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="0f6d9-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-184">Model nie zostanie przeprowadzona Weryfikacja Jeśli typ kolekcji jest określony zarówno **typu** atrybut i nie zawiera elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-185">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-185">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-186">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f6d9-187">Należy pamiętać, że **DefaultValue**, **MaxLength**, **FixedLength**, **dokładności**, **skalowania**,  **Unicode**, i **sortowania** atrybuty dotyczą tylko kolekcje **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="0f6d9-188">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-188">Attribute Name</span></span>                                                          | <span data-ttu-id="0f6d9-189">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-189">Is Required</span></span> | <span data-ttu-id="0f6d9-190">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-191">**Type**</span></span>                                                                | <span data-ttu-id="0f6d9-192">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-192">No</span></span>          | <span data-ttu-id="0f6d9-193">Typ kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f6d9-194">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-194">**Nullable**</span></span>                                                            | <span data-ttu-id="0f6d9-195">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-195">No</span></span>          | <span data-ttu-id="0f6d9-196">**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="0f6d9-197">> W CSDL v1 musi mieć właściwość typu złożonego `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="0f6d9-198">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="0f6d9-199">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-199">No</span></span>          | <span data-ttu-id="0f6d9-200">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="0f6d9-201">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="0f6d9-202">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-202">No</span></span>          | <span data-ttu-id="0f6d9-203">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="0f6d9-204">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="0f6d9-205">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-205">No</span></span>          | <span data-ttu-id="0f6d9-206">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="0f6d9-207">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-207">**Precision**</span></span>                                                           | <span data-ttu-id="0f6d9-208">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-208">No</span></span>          | <span data-ttu-id="0f6d9-209">Dokładność wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="0f6d9-210">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-210">**Scale**</span></span>                                                               | <span data-ttu-id="0f6d9-211">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-211">No</span></span>          | <span data-ttu-id="0f6d9-212">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-213">**SRID**</span></span>                                                                | <span data-ttu-id="0f6d9-214">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-214">No</span></span>          | <span data-ttu-id="0f6d9-215">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-216">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="0f6d9-217">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="0f6d9-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-218">**Unicode**</span></span>                                                             | <span data-ttu-id="0f6d9-219">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-219">No</span></span>          | <span data-ttu-id="0f6d9-220">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="0f6d9-221">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-221">**Collation**</span></span>                                                           | <span data-ttu-id="0f6d9-222">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-222">No</span></span>          | <span data-ttu-id="0f6d9-223">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-224">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f6d9-225">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-226">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-227">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-227">Example</span></span>

<span data-ttu-id="0f6d9-228">W poniższym przykładzie pokazano funkcję definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca kolekcję **osoby** typów jednostek (jak określono za pomocą **ElementType** atrybutu).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="0f6d9-229">W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="0f6d9-230">W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja przyjmuje jako parametr zbiór **działu** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="0f6d9-231">Element ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-232">A **ComplexType** element definiuje strukturę danych składające się z **EdmSimpleType** właściwości lub inne typy złożone.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="0f6d9-233">Typ złożony mogą być właściwość typu jednostki lub innego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="0f6d9-234">Typ złożony jest podobny dla typu jednostki, w tym, że typ złożony definiuje dane.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="0f6d9-235">Jednakże istnieją niektóre podstawowe różnice między typy złożone i typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="0f6d9-236">Typy złożone nie mają tożsamości (lub kluczy) i dlatego nie mogą istnieć niezależnie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="0f6d9-237">Typy złożone może istnieć tylko jako właściwości typów jednostek lub inne typy złożone.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="0f6d9-238">Typy złożone nie mogą uczestniczyć w skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="0f6d9-239">Ani end elementu association może być to typ złożony, i w związku z tym nie można zdefiniować właściwości nawigacji dla typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="0f6d9-240">Właściwość typu złożonego nie może mieć wartość null, jeśli właściwości skalarne typu złożonego każdego można ustawić na wartość null.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="0f6d9-241">A **ComplexType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-242">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-243">Właściwości (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="0f6d9-244">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="0f6d9-245">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ComplexType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="0f6d9-246">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="0f6d9-247">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-247">Is Required</span></span> | <span data-ttu-id="0f6d9-248">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-249">Nazwa</span><span class="sxs-lookup"><span data-stu-id="0f6d9-249">Name</span></span>                                                                                                           | <span data-ttu-id="0f6d9-250">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-250">Yes</span></span>         | <span data-ttu-id="0f6d9-251">Nazwa typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-251">The name of the complex type.</span></span> <span data-ttu-id="0f6d9-252">Nazwa typu złożonego nie może być taka sama jak nazwa innego typu złożonego, typ jednostki lub skojarzenia, który znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="0f6d9-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="0f6d9-254">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-254">No</span></span>          | <span data-ttu-id="0f6d9-255">Nazwa innego typu złożonego, który jest typ bazowy typ złożony, który jest definiowany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="0f6d9-256">> Ten atrybut nie ma zastosowania w wersji 1 CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="0f6d9-257">Dziedziczenia dla typów złożonych nie jest obsługiwane w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="0f6d9-258">Abstrakcyjny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="0f6d9-259">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-259">No</span></span>          | <span data-ttu-id="0f6d9-260">**Wartość true,** lub **False** (wartość domyślna) w zależności od tego, czy typ złożony jest typem abstrakcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="0f6d9-261">> Ten atrybut nie ma zastosowania w wersji 1 CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="0f6d9-262">Typy złożone w tej wersji nie może być typów abstrakcyjnych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-263">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ComplexType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="0f6d9-264">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-265">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-266">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-266">Example</span></span>

<span data-ttu-id="0f6d9-267">Poniższy przykład przedstawia typ złożony, **adres**, za pomocą **EdmSimpleType** właściwości **adres**, **Miasto**,  **StanLubProwincja**, **kraju**, i **KodPocztowy**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="0f6d9-268">Aby zdefiniować typ złożony **adres** (p. wyżej) jako właściwość typu jednostki, należy zadeklarować typ właściwości w definicji typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="0f6d9-269">W poniższym przykładzie przedstawiono **adres** właściwość typu złożonego typu encji (**wydawcy**):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="0f6d9-270">Element DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-271">**DefiningExpression** element język definicji schematu koncepcyjnego (CSDL) zawiera wyrażenie SQL jednostki, które definiuje funkcję w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="0f6d9-272">Do celów sprawdzania poprawności **DefiningExpression** element może zawierać dowolną zawartość.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="0f6d9-273">Jednak Entity Framework spowoduje zgłoszenie wyjątku w czasie wykonywania przypadku **DefiningExpression** element nie zawiera nieprawidłową SQL jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-274">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-274">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-275">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **DefiningExpression** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="0f6d9-276">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-277">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-278">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-278">Example</span></span>

<span data-ttu-id="0f6d9-279">W poniższym przykładzie użyto **DefiningExpression** elementu, aby zdefiniować funkcję, która zwraca liczbę lat, ponieważ została opublikowana książki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="0f6d9-280">Zawartość **DefiningExpression** elementu są zapisywane w języku SQL jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="0f6d9-281">Element zależne (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-282">**Zależne** element język definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint i definiuje zależne koniec ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="0f6d9-283">A **ReferentialConstraint** element definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f6d9-284">W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f6d9-285">Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f6d9-286">Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="0f6d9-287">**PropertyRef** elementy są używane do określenia, których kluczy odwoływać się do zakończenia podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="0f6d9-288">**Zależne** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-289">PropertyRef (jeden lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f6d9-290">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-291">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-291">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-292">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zależne** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="0f6d9-293">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-293">Attribute Name</span></span> | <span data-ttu-id="0f6d9-294">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-294">Is Required</span></span> | <span data-ttu-id="0f6d9-295">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-296">**Rola**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-296">**Role**</span></span>       | <span data-ttu-id="0f6d9-297">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-297">Yes</span></span>         | <span data-ttu-id="0f6d9-298">Nazwa typu jednostki, na końcu zależne skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-299">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zależne** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="0f6d9-300">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-301">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-302">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-302">Example</span></span>

<span data-ttu-id="0f6d9-303">W poniższym przykładzie przedstawiono **ReferentialConstraint** używany jako część definicji elementu **PublishedBy** skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="0f6d9-304">**PublisherId** właściwość **książki** typu jednostki tworzy zależne koniec ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="0f6d9-305">Element documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-306">**Dokumentacji** element język definicji schematu koncepcyjnego (CSDL) może służyć do Podaj informacje dotyczące obiektu, który jest zdefiniowany w elemencie rodzica.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="0f6d9-307">W pliku edmx gdy **dokumentacji** element jest elementem podrzędnym elementu, który pojawia się jako obiekt na powierzchni projektowej w Projektancie platformy EF (np. jednostek, skojarzeń lub właściwości), zawartość **dokumentacji**  element będzie wyświetlany jako w programie Visual Studio **właściwości** okna dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="0f6d9-308">**Dokumentacji** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-309">**Podsumowanie**: Krótki opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="0f6d9-310">(element zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-310">(zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-311">**LongDescription**: rozbudowany opis elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="0f6d9-312">(element zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-312">(zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-313">Elementy adnotacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-313">Annotation elements.</span></span> <span data-ttu-id="0f6d9-314">(elementy zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-315">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-315">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-316">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **dokumentacji** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="0f6d9-317">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-318">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-319">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-319">Example</span></span>

<span data-ttu-id="0f6d9-320">W poniższym przykładzie przedstawiono **dokumentacji** element jako element podrzędny elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="0f6d9-321">Gdyby poniższy fragment kodu w CSDL edmx zawartości pliku, zawartość **Podsumowanie** i **LongDescription** elementy pojawią się w programie Visual Studio **właściwości** okna po kliknięciu `Customer` typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="0f6d9-322">Element end (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-322">End Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-323">**Zakończenia** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu Association lub elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="0f6d9-324">W każdym przypadku rolę **zakończenia** różni się elementu i atrybuty stosowane są różne.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="0f6d9-325">Końcowy Element jako element podrzędny elementu Association</span><span class="sxs-lookup"><span data-stu-id="0f6d9-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="0f6d9-326">**Zakończenia** — element (jako element podrzędny elementu **skojarzenia** elementu) identyfikuje typ jednostki na jednym końcu asocjacji i liczbę wystąpień typu jednostki, które może znajdować się na końcu tego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="0f6d9-327">Skojarzenia są zdefiniowane jako część skojarzenia; Skojarzenie musi mieć dokładnie dwa punkty końcowe skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="0f6d9-328">Wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub kluczy obcych, jeśli są one widoczne na typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="0f6d9-329">**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-330">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-331">OnDelete (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-332">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-333">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-333">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-334">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="0f6d9-335">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-335">Attribute Name</span></span>   | <span data-ttu-id="0f6d9-336">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-336">Is Required</span></span> | <span data-ttu-id="0f6d9-337">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-338">**Type**</span></span>         | <span data-ttu-id="0f6d9-339">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-339">Yes</span></span>         | <span data-ttu-id="0f6d9-340">Nazwa typu jednostki, na jednym końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="0f6d9-341">**Rola**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-341">**Role**</span></span>         | <span data-ttu-id="0f6d9-342">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-342">No</span></span>          | <span data-ttu-id="0f6d9-343">Nazwa dla elementu end skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-343">A name for the association end.</span></span> <span data-ttu-id="0f6d9-344">Jeśli nie podano żadnej nazwy, nazwa typu jednostki na punkt końcowy będzie używany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="0f6d9-345">**Liczebność**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-345">**Multiplicity**</span></span> | <span data-ttu-id="0f6d9-346">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-346">Yes</span></span>         | <span data-ttu-id="0f6d9-347">**1**, **od 0 do 1**, lub **\*** w zależności od liczby wystąpień typów jednostek, które mogą być na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="0f6d9-348">**1** oznacza tego wystąpienia typu dokładnie jedną jednostkę istnieje na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="0f6d9-349">**od 0 do 1** oznacza, że na końcu skojarzenia zera lub jednego wystąpienia typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="0f6d9-350">**\*** oznacza, że wartość zero, jeden lub więcej wystąpień typu jednostki na końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-351">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="0f6d9-352">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-353">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-354">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-354">Example</span></span>

<span data-ttu-id="0f6d9-355">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="0f6d9-356">**Liczebność** wartości dla każdej **zakończenia** skojarzenia wskazują tę liczbę **zamówienia** może być skojarzony z **klienta**, ale tylko jeden **klienta** może być skojarzony z **kolejności**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="0f6d9-357">Ponadto **OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i które zostały załadowane do obiektu ObjectContext będzie Jeśli usunięto **klienta** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="0f6d9-358">Końcowy Element jako element podrzędny elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="0f6d9-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="0f6d9-359">**Zakończenia** element określa jeden z punktów końcowych zestaw skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="0f6d9-360">**AssociationSet** musi zawierać dwa **zakończenia** elementów.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="0f6d9-361">Informacje zawarte w **zakończenia** element jest używany w mapowaniu skojarzenia Ustaw ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="0f6d9-362">**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-363">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-364">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-365">Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="0f6d9-366">Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-367">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-367">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-368">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="0f6d9-369">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-369">Attribute Name</span></span> | <span data-ttu-id="0f6d9-370">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-370">Is Required</span></span> | <span data-ttu-id="0f6d9-371">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-372">**Obiekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-372">**EntitySet**</span></span>  | <span data-ttu-id="0f6d9-373">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-373">Yes</span></span>         | <span data-ttu-id="0f6d9-374">Nazwa **EntitySet** element, który definiuje jeden z punktów końcowych nadrzędnego **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="0f6d9-375">**EntitySet** element musi być zdefiniowany w tym samym kontenerze jednostki jako element nadrzędny **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="0f6d9-376">**Rola**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-376">**Role**</span></span>       | <span data-ttu-id="0f6d9-377">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-377">No</span></span>          | <span data-ttu-id="0f6d9-378">Nazwa skojarzenia zestawu zakończenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-378">The name of the association set end.</span></span> <span data-ttu-id="0f6d9-379">Jeśli **roli** atrybut nie jest używany, nazwę punktu końcowego zestawu skojarzenia będzie nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-380">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="0f6d9-381">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-382">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-383">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-383">Example</span></span>

<span data-ttu-id="0f6d9-384">W poniższym przykładzie przedstawiono **EntityContainer** elementu przy użyciu dwóch **AssociationSet** przy użyciu dwóch elementów **zakończenia** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="0f6d9-385">Element EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-386">**EntityContainer** element język definicji schematu koncepcyjnego (CSDL) jest kontenerem logicznym, zestawów encji, zestawów skojarzeń i Importy — funkcja.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="0f6d9-387">Kontener jednostek modelu koncepcyjnego mapuje do kontenera magazynu modelu jednostki za pośrednictwem elementu w elemencie EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="0f6d9-388">Kontener jednostek modelu magazynu w tym artykule opisano struktury bazy danych: zestawy jednostek opisują tabel, zestawów skojarzeń opisano ograniczenia klucza obcego i Importy funkcji opisano procedury składowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="0f6d9-389">**EntityContainer** element może mieć zero lub jeden elementów dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="0f6d9-390">Jeśli **dokumentacji** element jest obecny, musi poprzedzać wszystkie **EntitySet**, **AssociationSet**, i **FunctionImport** elementów.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="0f6d9-391">**EntityContainer** element może mieć zero lub więcej z następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-392">Obiekt EntitySet</span><span class="sxs-lookup"><span data-stu-id="0f6d9-392">EntitySet</span></span>
-   <span data-ttu-id="0f6d9-393">Obiekt AssociationSet</span><span class="sxs-lookup"><span data-stu-id="0f6d9-393">AssociationSet</span></span>
-   <span data-ttu-id="0f6d9-394">Element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="0f6d9-394">FunctionImport</span></span>
-   <span data-ttu-id="0f6d9-395">Elementów adnotacji</span><span class="sxs-lookup"><span data-stu-id="0f6d9-395">Annotation elements</span></span>

<span data-ttu-id="0f6d9-396">Możesz rozszerzyć **EntityContainer** element, aby uwzględnić zawartość innego **EntityContainer** który mieści się w tej samej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="0f6d9-397">Aby uwzględnić zawartość innego **EntityContainer**, w odwołujących się **obiektu EntityContainer** elementu, ustaw wartość **rozszerza** atrybutu Nazwa  **Obiekt EntityContainer** element, który chcesz dołączyć.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="0f6d9-398">Wszystkie elementy podrzędne elementu dołączonej **EntityContainer** elementu będą traktowane jako elementy podrzędne odwołujących się **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-399">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-399">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-400">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **Using** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="0f6d9-401">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-401">Attribute Name</span></span> | <span data-ttu-id="0f6d9-402">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-402">Is Required</span></span> | <span data-ttu-id="0f6d9-403">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="0f6d9-404">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-404">**Name**</span></span>       | <span data-ttu-id="0f6d9-405">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-405">Yes</span></span>         | <span data-ttu-id="0f6d9-406">Nazwa kontenera jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="0f6d9-407">**Rozszerza**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-407">**Extends**</span></span>    | <span data-ttu-id="0f6d9-408">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-408">No</span></span>          | <span data-ttu-id="0f6d9-409">Nazwa innego kontenera jednostek w ramach tej samej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-410">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="0f6d9-411">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-412">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-413">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-413">Example</span></span>

<span data-ttu-id="0f6d9-414">W poniższym przykładzie przedstawiono **EntityContainer** element, który definiuje trzy zestawów encji i zestawów skojarzeń dwa.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="0f6d9-415">Element EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-416">**EntitySet** element język definicji schematu koncepcyjnego to kontener logiczny dla wystąpień tego typu jednostki i wystąpień dowolnego typu, który jest tworzony na podstawie tego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="0f6d9-417">Relacja między typem encji i zestaw jednostek jest analogiczne do relację wiersz tabeli w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="0f6d9-418">Np. wiersz typ jednostki definiuje zestaw powiązanych danych i jak tabela, zestaw jednostek zawiera wystąpienia tej definicji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="0f6d9-419">Zestaw jednostek zapewnia konstrukcję grupowanie wystąpień typów jednostek, dzięki czemu mogą być mapowane do pokrewne struktury danych w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="0f6d9-420">Może być określona więcej niż jeden zestaw jednostek dla typu określonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-421">W Projektancie platformy EF nie obsługuje modelami koncepcyjnymi, które zawierają wiele zestawów jednostek według typu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="0f6d9-422">**EntitySet** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-423">Element documentation (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-424">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-425">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-425">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-426">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="0f6d9-427">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-427">Attribute Name</span></span> | <span data-ttu-id="0f6d9-428">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-428">Is Required</span></span> | <span data-ttu-id="0f6d9-429">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-430">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-430">**Name**</span></span>       | <span data-ttu-id="0f6d9-431">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-431">Yes</span></span>         | <span data-ttu-id="0f6d9-432">Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="0f6d9-433">**Typ entityType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-433">**EntityType**</span></span> | <span data-ttu-id="0f6d9-434">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-434">Yes</span></span>         | <span data-ttu-id="0f6d9-435">W pełni kwalifikowana nazwa typu jednostki, dla której zestaw jednostek zawiera wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-436">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="0f6d9-437">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-438">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-439">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-439">Example</span></span>

<span data-ttu-id="0f6d9-440">W poniższym przykładzie przedstawiono **EntityContainer** element z trzech **EntitySet** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="0f6d9-441">Istnieje możliwość definiowania wielu zestawów jednostek według typu (MEST).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="0f6d9-442">W poniższym przykładzie zdefiniowano kontener jednostek z dwoma zestawami jednostki dla **książki** typ jednostki:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="0f6d9-443">Element EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-444">**EntityType** element reprezentuje strukturę koncepcji najwyższego poziomu, takich jak klient lub kolejność, w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="0f6d9-445">Typ jednostki jest szablonem dla wystąpień typów jednostek w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="0f6d9-446">Każdy szablon zawiera następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="0f6d9-447">Unikatowa nazwa.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-447">A unique name.</span></span> <span data-ttu-id="0f6d9-448">(Wymagane).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-448">(Required.)</span></span>
-   <span data-ttu-id="0f6d9-449">Klucz jednostki, który jest definiowany przez jedną lub więcej właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="0f6d9-450">(Wymagane).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-450">(Required.)</span></span>
-   <span data-ttu-id="0f6d9-451">Właściwości zawierający dane.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-451">Properties for containing data.</span></span> <span data-ttu-id="0f6d9-452">(Opcjonalnie).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-452">(Optional.)</span></span>
-   <span data-ttu-id="0f6d9-453">Właściwości nawigacji, które umożliwiają nawigację z jednym końcu asocjacji na drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="0f6d9-454">(Opcjonalnie).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-454">(Optional.)</span></span>

<span data-ttu-id="0f6d9-455">W aplikacji wystąpienia typu jednostki reprezentuje określonego obiektu (na przykład konkretnego klienta lub zamówienia).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="0f6d9-456">Każde wystąpienie typu jednostki musi mieć unikatowy klucz w ramach zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="0f6d9-457">Dwa wystąpienia typu jednostki są traktowane jako równe tylko wtedy, gdy są one tego samego typu i wartości kluczy podmiotu są takie same.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="0f6d9-458">**EntityType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-459">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-460">Klucz (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-461">Właściwości (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="0f6d9-462">Element NavigationProperty (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="0f6d9-463">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-464">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-464">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-465">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="0f6d9-466">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="0f6d9-467">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-467">Is Required</span></span> | <span data-ttu-id="0f6d9-468">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-469">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="0f6d9-470">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-470">Yes</span></span>         | <span data-ttu-id="0f6d9-471">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="0f6d9-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="0f6d9-473">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-473">No</span></span>          | <span data-ttu-id="0f6d9-474">Nazwa innego typu jednostki, który jest typem podstawowym typu jednostki, który jest definiowany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="0f6d9-475">**Abstrakcyjny**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="0f6d9-476">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-476">No</span></span>          | <span data-ttu-id="0f6d9-477">**Wartość true,** lub **False**, w zależności od tego, czy typ obiektu jest typem abstrakcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="0f6d9-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="0f6d9-479">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-479">No</span></span>          | <span data-ttu-id="0f6d9-480">**Wartość true,** lub **False** w zależności od tego, czy typ jednostki jest typem jednostki open.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="0f6d9-481">> **OpenType** atrybut ma zastosowanie tylko do typów jednostek, które są zdefiniowane w modelach koncepcyjnych, które są używane z architektury ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-482">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="0f6d9-483">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-484">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-485">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-485">Example</span></span>

<span data-ttu-id="0f6d9-486">W poniższym przykładzie przedstawiono **EntityType** element z trzech **właściwość** elementów i dwóch **element NavigationProperty** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="0f6d9-487">Element EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-488">**EnumType** element reprezentuje Typ wyliczany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="0f6d9-489">**EnumType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-490">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-491">Element członkowski (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="0f6d9-492">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-493">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-493">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-494">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EnumType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="0f6d9-495">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-495">Attribute Name</span></span>     | <span data-ttu-id="0f6d9-496">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-496">Is Required</span></span> | <span data-ttu-id="0f6d9-497">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-498">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-498">**Name**</span></span>           | <span data-ttu-id="0f6d9-499">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-499">Yes</span></span>         | <span data-ttu-id="0f6d9-500">Nazwa typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="0f6d9-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-501">**IsFlags**</span></span>        | <span data-ttu-id="0f6d9-502">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-502">No</span></span>          | <span data-ttu-id="0f6d9-503">**Wartość true,** lub **False**, w zależności od tego, czy typ wyliczeniowy może służyć jako zestaw flag.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="0f6d9-504">Wartość domyślna to **wartość False.**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="0f6d9-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-505">**UnderlyingType**</span></span> | <span data-ttu-id="0f6d9-506">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-506">No</span></span>          | <span data-ttu-id="0f6d9-507">**Edm.Byte**, **Edm.Int16**, **typem Edm.Int32**, **Edm.Int64** lub **Edm.SByte** ogranicza zakres wartości tego typu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="0f6d9-508">Domyślny typ podstawowy wyliczenia elementów to **typem Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-509">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EnumType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="0f6d9-510">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-511">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-512">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-512">Example</span></span>

<span data-ttu-id="0f6d9-513">W poniższym przykładzie przedstawiono **EnumType** element z trzech **elementu członkowskiego** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="0f6d9-514">Function — Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-514">Function Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-515">**Funkcja** element język definicji schematu koncepcyjnego (CSDL) jest używany do definiowania lub deklarowania funkcji w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="0f6d9-516">Funkcja jest zdefiniowana za pomocą elementu DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="0f6d9-517">A **funkcja** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-518">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-519">Parametr (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="0f6d9-520">DefiningExpression (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-521">ReturnType (funkcja) (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-522">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="0f6d9-523">Zwracana typ dla funkcji, należy określić albo **ReturnType** — element (funkcja) lub **ReturnType** atrybutu (patrz poniżej), ale nie oba.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="0f6d9-524">Możliwe zwracane typy są wszelkie EdmSimpleType, jednostki typu, typu złożonego, typ wiersza lub typu referencyjnego (lub zbiór jednego z następujących typów).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-525">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-525">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-526">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **funkcja** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="0f6d9-527">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-527">Attribute Name</span></span> | <span data-ttu-id="0f6d9-528">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-528">Is Required</span></span> | <span data-ttu-id="0f6d9-529">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="0f6d9-530">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-530">**Name**</span></span>       | <span data-ttu-id="0f6d9-531">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-531">Yes</span></span>         | <span data-ttu-id="0f6d9-532">Nazwa funkcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-532">The name of the function.</span></span>          |
| <span data-ttu-id="0f6d9-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-533">**ReturnType**</span></span> | <span data-ttu-id="0f6d9-534">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-534">No</span></span>          | <span data-ttu-id="0f6d9-535">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-536">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **funkcja** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="0f6d9-537">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-538">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-539">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-539">Example</span></span>

<span data-ttu-id="0f6d9-540">W poniższym przykładzie użyto **funkcja** elementu, aby zdefiniować funkcję, która zwraca liczbę lat, ponieważ został zatrudniony pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="0f6d9-541">Element FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-542">**FunctionImport** element język definicji schematu koncepcyjnego (CSDL) reprezentuje funkcję, która jest zdefiniowana w źródle danych, ale dostępne obiekty za pośrednictwem modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="0f6d9-543">Na przykład elementem funkcji w modelu magazynu może służyć do reprezentowania procedurę składowaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="0f6d9-544">A **FunctionImport** elementu w modelu koncepcyjnym reprezentuje odpowiednich funkcji w aplikacji Entity Framework i jest mapowany do funkcji modelu magazynu przy użyciu elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="0f6d9-545">Gdy funkcja jest wywoływana w aplikacji, odpowiednie procedury składowanej jest wykonywany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="0f6d9-546">**FunctionImport** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-547">Dokumentacja (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-548">Parametr (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-549">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-550">ReturnType (FunctionImport) (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="0f6d9-551">Jeden **parametru** element powinien być zdefiniowany dla każdego parametru, który akceptuje funkcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="0f6d9-552">Zwracana typ dla funkcji, należy określić albo **ReturnType** elementu (element FunctionImport) lub **ReturnType** atrybutu (patrz poniżej), ale nie oba.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="0f6d9-553">Wartość zwracany typ musi być kolekcją EdmSimpleType, EntityType lub ComplexType.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-554">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-554">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-555">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="0f6d9-556">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-556">Attribute Name</span></span>   | <span data-ttu-id="0f6d9-557">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-557">Is Required</span></span> | <span data-ttu-id="0f6d9-558">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-559">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-559">**Name**</span></span>         | <span data-ttu-id="0f6d9-560">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-560">Yes</span></span>         | <span data-ttu-id="0f6d9-561">Nazwa funkcji zaimportowane.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="0f6d9-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-562">**ReturnType**</span></span>   | <span data-ttu-id="0f6d9-563">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-563">No</span></span>          | <span data-ttu-id="0f6d9-564">Typ, który zwraca funkcja.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-564">The type that the function returns.</span></span> <span data-ttu-id="0f6d9-565">Nie należy używać tego atrybutu, jeśli funkcja nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="0f6d9-566">W przeciwnym razie wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="0f6d9-567">**Obiekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-567">**EntitySet**</span></span>    | <span data-ttu-id="0f6d9-568">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-568">No</span></span>          | <span data-ttu-id="0f6d9-569">Jeśli funkcja zwraca kolekcję jednostek typy wartości **EntitySet** musi być zestaw jednostek do której należy kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="0f6d9-570">W przeciwnym razie **EntitySet** atrybutu nie może być używany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="0f6d9-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-571">**IsComposable**</span></span> | <span data-ttu-id="0f6d9-572">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-572">No</span></span>          | <span data-ttu-id="0f6d9-573">Jeśli wartość jest równa true, funkcja jest konfigurowalna (funkcji zwracającej tabelę) i mogą być używane w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="0f6d9-574">Wartość domyślna to **false**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-575">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="0f6d9-576">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-577">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-578">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-578">Example</span></span>

<span data-ttu-id="0f6d9-579">W poniższym przykładzie przedstawiono **FunctionImport** element, który przyjmuje jeden parametr i zwraca kolekcję typów jednostek:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="0f6d9-580">Kluczowym elementem (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-580">Key Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-581">**Klucz** element jest elementem podrzędnym elementu EntityType i definiuje *klucz jednostki* (właściwość lub ustaw właściwości typu jednostki, które określają tożsamość).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="0f6d9-582">Właściwości, które tworzą klucz jednostki są wybierane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="0f6d9-583">Wartości właściwości klucza jednostki musi jednoznacznie identyfikują wystąpienie jednostki typu w obrębie jednostki ustawienie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="0f6d9-584">Należy wybrać właściwości, które tworzą klucz jednostki w celu zagwarantowania unikatowości wystąpień w zestawie jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="0f6d9-585">**Klucz** element definiuje klucz jednostki, odwołując się do przynajmniej jednej z właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="0f6d9-586">**Klucz** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-587">PropertyRef (jeden lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f6d9-588">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-589">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-589">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-590">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **klucz** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="0f6d9-591">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-592">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-593">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-593">Example</span></span>

<span data-ttu-id="0f6d9-594">Poniższy przykład definiuje typ jednostki o nazwie **książki**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="0f6d9-595">Klucz jednostki jest zdefiniowany, odwołując się do **ISBN** właściwość typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="0f6d9-596">**ISBN** właściwość jest dobrym wyborem dla klucza jednostki, ponieważ International Standard książki numer (ISBN) unikatowo identyfikuje książki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="0f6d9-597">Poniższy przykład przedstawia typ jednostki (**Autor**) zawierający klucz jednostki, która składa się z dwóch właściwości **nazwa** i **adres**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="0f6d9-598">Za pomocą **nazwa** i **adres** jednostki klucz jest wyboru rozsądny, ponieważ dwóch autorów o takiej samej nazwie jest mało prawdopodobne na żywo na ten sam adres.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="0f6d9-599">Jednak ten wybór dla klucza jednostki absolutnie nie gwarantuje klucze unikatowe jednostki w zestawie jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="0f6d9-600">Dodawanie właściwości, takie jak **wartość IDAutora**, który może być używany do jednoznacznego identyfikowania Autor może w tym przypadku zalecane.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="0f6d9-601">Element członkowski (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-601">Member Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-602">**Elementu członkowskiego** element jest elementem podrzędnym elementu EnumType i definiuje składową typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-603">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-603">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-604">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="0f6d9-605">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-605">Attribute Name</span></span> | <span data-ttu-id="0f6d9-606">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-606">Is Required</span></span> | <span data-ttu-id="0f6d9-607">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-608">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-608">**Name**</span></span>       | <span data-ttu-id="0f6d9-609">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-609">Yes</span></span>         | <span data-ttu-id="0f6d9-610">Nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="0f6d9-611">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-611">**Value**</span></span>      | <span data-ttu-id="0f6d9-612">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-612">No</span></span>          | <span data-ttu-id="0f6d9-613">Wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-613">The value of the member.</span></span> <span data-ttu-id="0f6d9-614">Domyślnie pierwszy element członkowski ma wartość 0, a wartość każdego kolejnych moduł wyliczający jest zwiększana o 1.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="0f6d9-615">Może istnieć wiele składowych o tej samej wartości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-616">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="0f6d9-617">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-618">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-619">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-619">Example</span></span>

<span data-ttu-id="0f6d9-620">W poniższym przykładzie przedstawiono **EnumType** element z trzech **elementu członkowskiego** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="0f6d9-621">Element NavigationProperty — Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-622">A **element NavigationProperty** element definiuje właściwość nawigacji, który zawiera dokumentację na drugim końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="0f6d9-623">W odróżnieniu od właściwości zdefiniowane przy użyciu elementu właściwości właściwości nawigacji definiuje kształt i właściwości danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="0f6d9-624">Zapewniają one sposób przechodzenia skojarzenie między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="0f6d9-625">Należy pamiętać, że właściwości nawigacji są opcjonalne na obu typów jednostek na końcach asocjacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="0f6d9-626">Jeśli zdefiniujesz właściwości nawigacji na jednym typie jednostek na końcu asocjacji, ma Zdefiniuj właściwość nawigacji typu jednostki na drugim końcu skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="0f6d9-627">Typ danych zwracanych przez właściwość nawigacji jest ustalana liczebność jej punkt końcowy skojarzenia zdalnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="0f6d9-628">Na przykład, załóżmy, że właściwość nawigacji, **OrdersNavProp**, istnieje na **klienta** typu jednostki i przechodzi skojarzenia typu jeden do wielu między **klienta** i  **Kolejność**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="0f6d9-629">Ponieważ punkt końcowy skojarzenia zdalnego dla właściwości nawigacji ma liczebność wiele (\*), jego typ danych to kolekcja (z **kolejności**).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="0f6d9-630">Podobnie, jeśli właściwość nawigacji, **CustomerNavProp**, istnieje na **kolejności** typu jednostki, będzie jego typu danych **klienta** ponieważ liczebności zakończenia zdalnego jeden (1).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="0f6d9-631">A **element NavigationProperty** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-632">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-633">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-634">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-634">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-635">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **element NavigationProperty** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="0f6d9-636">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-636">Attribute Name</span></span>   | <span data-ttu-id="0f6d9-637">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-637">Is Required</span></span> | <span data-ttu-id="0f6d9-638">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-639">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-639">**Name**</span></span>         | <span data-ttu-id="0f6d9-640">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-640">Yes</span></span>         | <span data-ttu-id="0f6d9-641">Nazwa właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="0f6d9-642">**Relacja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-642">**Relationship**</span></span> | <span data-ttu-id="0f6d9-643">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-643">Yes</span></span>         | <span data-ttu-id="0f6d9-644">Nazwa skojarzenia, które znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="0f6d9-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-645">**ToRole**</span></span>       | <span data-ttu-id="0f6d9-646">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-646">Yes</span></span>         | <span data-ttu-id="0f6d9-647">Koniec asocjacji, na której kończy się nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="0f6d9-648">Wartość **ToRole** atrybut musi być taka sama jak wartość jednego z **roli** atrybuty zdefiniowane w jednym z punktów końcowych skojarzenia (zdefiniowanej w elemencie punkt końcowy skojarzenia).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="0f6d9-649">**Wartości elementów FromRole**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-649">**FromRole**</span></span>     | <span data-ttu-id="0f6d9-650">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-650">Yes</span></span>         | <span data-ttu-id="0f6d9-651">Koniec skojarzenia, w którym rozpoczyna się nawigacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="0f6d9-652">Wartość **FromRole** atrybut musi być taka sama jak wartość jednego z **roli** atrybuty zdefiniowane w jednym z punktów końcowych skojarzenia (zdefiniowanej w elemencie punkt końcowy skojarzenia).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-653">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **element NavigationProperty** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="0f6d9-654">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-655">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-656">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-656">Example</span></span>

<span data-ttu-id="0f6d9-657">W poniższym przykładzie zdefiniowano typ jednostki (**książki**) z dwóch właściwości nawigacji (**PublishedBy** i **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="0f6d9-658">Element OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-659">**OnDelete** element język definicji schematu koncepcyjnego (CSDL) definiuje zachowanie, która jest połączona z skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="0f6d9-660">Jeśli **akcji** ma ustawioną wartość atrybutu **Cascade** na jednym końcu asocjacji powiązanych typów jednostek na drugim końcu skojarzenia zostaną usunięte po usunięciu typu jednostki, które na pierwszy punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="0f6d9-661">Jeśli skojarzenie między dwoma typami encji jest relacji klucza podstawowego klucza do podstawowej, a następnie załadować obiektu zależnego zostanie usunięty po usunięciu obiekt podmiotu zabezpieczeń na drugim końcu skojarzenia niezależnie od wartości **OnDelete** Specyfikacja.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="0f6d9-662">**OnDelete** element ma wpływ tylko na zachowanie środowiska uruchomieniowego aplikacji; nie ma wpływu na zachowanie w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="0f6d9-663">Zachowanie zdefiniowane w źródle danych powinna być taka sama jak zachowanie zdefiniowane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="0f6d9-664">**OnDelete** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-665">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-666">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-667">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-667">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-668">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="0f6d9-669">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-669">Attribute Name</span></span> | <span data-ttu-id="0f6d9-670">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-670">Is Required</span></span> | <span data-ttu-id="0f6d9-671">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-672">**Akcja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-672">**Action**</span></span>     | <span data-ttu-id="0f6d9-673">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-673">Yes</span></span>         | <span data-ttu-id="0f6d9-674">**Kaskadowe** lub **Brak**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-674">**Cascade** or **None**.</span></span> <span data-ttu-id="0f6d9-675">Jeśli **Cascade**, typy jednostek zależnych zostaną usunięte po usunięciu typ podmiotu zabezpieczeń jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="0f6d9-676">Jeśli **Brak**, typy jednostek zależnych nie zostaną usunięte po usunięciu typ podmiotu zabezpieczeń jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-677">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="0f6d9-678">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-679">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-680">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-680">Example</span></span>

<span data-ttu-id="0f6d9-681">W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="0f6d9-682">**OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i zostały załadowane do obiektu ObjectContext zostaną usunięte po  **Klient** zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="0f6d9-683">Parameter — Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-684">**Parametru** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu FunctionImport lub elemencie Function.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="0f6d9-685">FunctionImport Element aplikacji</span><span class="sxs-lookup"><span data-stu-id="0f6d9-685">FunctionImport Element Application</span></span>

<span data-ttu-id="0f6d9-686">A **parametru** — element (jako element podrzędny elementu **FunctionImport** elementu) służy do definiowania parametry wejściowe i wyjściowe w przypadku importowania funkcji, które są zadeklarowane w CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="0f6d9-687">**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-688">Dokumentacja (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-689">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-690">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-690">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-691">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="0f6d9-692">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-692">Attribute Name</span></span> | <span data-ttu-id="0f6d9-693">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-693">Is Required</span></span> | <span data-ttu-id="0f6d9-694">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-695">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-695">**Name**</span></span>       | <span data-ttu-id="0f6d9-696">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-696">Yes</span></span>         | <span data-ttu-id="0f6d9-697">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f6d9-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-698">**Type**</span></span>       | <span data-ttu-id="0f6d9-699">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-699">Yes</span></span>         | <span data-ttu-id="0f6d9-700">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-700">The parameter type.</span></span> <span data-ttu-id="0f6d9-701">Wartość musi być **EDMSimpleType** lub typ złożony, który znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="0f6d9-702">**Tryb**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-702">**Mode**</span></span>       | <span data-ttu-id="0f6d9-703">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-703">No</span></span>          | <span data-ttu-id="0f6d9-704">**W**, **się**, lub **InOut** w zależności od tego, czy parametr jest danych wejściowych, danych wyjściowych lub parametr input/output.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="0f6d9-705">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-705">**MaxLength**</span></span>  | <span data-ttu-id="0f6d9-706">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-706">No</span></span>          | <span data-ttu-id="0f6d9-707">Maksymalna dozwolona liczba znaków parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="0f6d9-708">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-708">**Precision**</span></span>  | <span data-ttu-id="0f6d9-709">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-709">No</span></span>          | <span data-ttu-id="0f6d9-710">Dokładność parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-711">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-711">**Scale**</span></span>      | <span data-ttu-id="0f6d9-712">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-712">No</span></span>          | <span data-ttu-id="0f6d9-713">Skala parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="0f6d9-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-714">**SRID**</span></span>       | <span data-ttu-id="0f6d9-715">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-715">No</span></span>          | <span data-ttu-id="0f6d9-716">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-717">Prawidłowy tylko dla parametrów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="0f6d9-718">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-719">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="0f6d9-720">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-721">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-722">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-722">Example</span></span>

<span data-ttu-id="0f6d9-723">W poniższym przykładzie przedstawiono **FunctionImport** elementu za pomocą jednego **parametru** elementu podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="0f6d9-724">Funkcja przyjmuje jeden parametr wejściowy i zwraca kolekcję typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="0f6d9-725">Funkcja elementu aplikacji</span><span class="sxs-lookup"><span data-stu-id="0f6d9-725">Function Element Application</span></span>

<span data-ttu-id="0f6d9-726">A **parametru** — element (jako element podrzędny elementu **funkcja** elementu) definiuje parametry dla funkcji, które zdefiniowana lub zadeklarowana w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="0f6d9-727">**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-728">Dokumentacja (zero lub jeden elementy)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="0f6d9-729">Typ CollectionType (zero lub jeden elementy)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="0f6d9-730">Element ReferenceType (zero lub jeden elementy)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="0f6d9-731">RowType (zero lub jeden elementy)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-732">Tylko jeden z **CollectionType**, **ReferenceType**, lub **RowType** elementy mogą być elementem podrzędnym **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="0f6d9-733">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-734">Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="0f6d9-735">Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-736">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-736">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-737">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="0f6d9-738">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-738">Attribute Name</span></span>   | <span data-ttu-id="0f6d9-739">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-739">Is Required</span></span> | <span data-ttu-id="0f6d9-740">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-741">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-741">**Name**</span></span>         | <span data-ttu-id="0f6d9-742">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-742">Yes</span></span>         | <span data-ttu-id="0f6d9-743">Nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f6d9-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-744">**Type**</span></span>         | <span data-ttu-id="0f6d9-745">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-745">No</span></span>          | <span data-ttu-id="0f6d9-746">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-746">The parameter type.</span></span> <span data-ttu-id="0f6d9-747">Parametr może być dowolny z następujących typów (lub kolekcje z tych typów):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="0f6d9-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="0f6d9-749">Typ jednostki</span><span class="sxs-lookup"><span data-stu-id="0f6d9-749">entity type</span></span> <br/> <span data-ttu-id="0f6d9-750">Typ złożony</span><span class="sxs-lookup"><span data-stu-id="0f6d9-750">complex type</span></span> <br/> <span data-ttu-id="0f6d9-751">Typ wiersza</span><span class="sxs-lookup"><span data-stu-id="0f6d9-751">row type</span></span> <br/> <span data-ttu-id="0f6d9-752">typ odwołania</span><span class="sxs-lookup"><span data-stu-id="0f6d9-752">reference type</span></span>                             |
| <span data-ttu-id="0f6d9-753">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-753">**Nullable**</span></span>     | <span data-ttu-id="0f6d9-754">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-754">No</span></span>          | <span data-ttu-id="0f6d9-755">**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć **null** wartość.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="0f6d9-756">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-756">**DefaultValue**</span></span> | <span data-ttu-id="0f6d9-757">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-757">No</span></span>          | <span data-ttu-id="0f6d9-758">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f6d9-759">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-759">**MaxLength**</span></span>    | <span data-ttu-id="0f6d9-760">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-760">No</span></span>          | <span data-ttu-id="0f6d9-761">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-762">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-762">**FixedLength**</span></span>  | <span data-ttu-id="0f6d9-763">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-763">No</span></span>          | <span data-ttu-id="0f6d9-764">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f6d9-765">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-765">**Precision**</span></span>    | <span data-ttu-id="0f6d9-766">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-766">No</span></span>          | <span data-ttu-id="0f6d9-767">Dokładność wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f6d9-768">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-768">**Scale**</span></span>        | <span data-ttu-id="0f6d9-769">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-769">No</span></span>          | <span data-ttu-id="0f6d9-770">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f6d9-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-771">**SRID**</span></span>         | <span data-ttu-id="0f6d9-772">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-772">No</span></span>          | <span data-ttu-id="0f6d9-773">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-774">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f6d9-775">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f6d9-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-776">**Unicode**</span></span>      | <span data-ttu-id="0f6d9-777">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-777">No</span></span>          | <span data-ttu-id="0f6d9-778">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f6d9-779">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-779">**Collation**</span></span>    | <span data-ttu-id="0f6d9-780">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-780">No</span></span>          | <span data-ttu-id="0f6d9-781">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-782">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="0f6d9-783">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-784">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-785">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-785">Example</span></span>

<span data-ttu-id="0f6d9-786">W poniższym przykładzie przedstawiono **funkcja** element, który korzysta z jednego **parametru** elementu podrzędnego, aby zdefiniować parametr funkcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="0f6d9-787">Element jednostki (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-788">**Jednostki** element język definicji schematu koncepcyjnego (CSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje główny koniec ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="0f6d9-789">A **ReferentialConstraint** element definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f6d9-790">W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f6d9-791">Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f6d9-792">Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="0f6d9-793">**PropertyRef** elementy są używane do określania, klucze, które są przywoływane przez zależne zakończenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="0f6d9-794">**Jednostki** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-795">PropertyRef (jeden lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f6d9-796">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-797">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-797">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-798">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **jednostki** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="0f6d9-799">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-799">Attribute Name</span></span> | <span data-ttu-id="0f6d9-800">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-800">Is Required</span></span> | <span data-ttu-id="0f6d9-801">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-802">**Rola**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-802">**Role**</span></span>       | <span data-ttu-id="0f6d9-803">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-803">Yes</span></span>         | <span data-ttu-id="0f6d9-804">Nazwa typu jednostki, na końcu jednostki skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-805">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **jednostki** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="0f6d9-806">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-807">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-808">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-808">Example</span></span>

<span data-ttu-id="0f6d9-809">W poniższym przykładzie przedstawiono **ReferentialConstraint** element, który jest częścią definicji **PublishedBy** skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="0f6d9-810">**Identyfikator** właściwość **wydawcy** typu jednostki stanowi główny koniec ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="0f6d9-811">Property — Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-811">Property Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-812">**Właściwość** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu EntityType, ComplexType element lub RowType element.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="0f6d9-813">Obiekt EntityType i ComplexType elementu aplikacji</span><span class="sxs-lookup"><span data-stu-id="0f6d9-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="0f6d9-814">**Właściwość** elementów (jako elementy podrzędne **EntityType** lub **ComplexType** elementy) zdefiniuj kształt i charakterystyki danych, która będzie zawierać wystąpienia typu jednostki lub typ złożony .</span><span class="sxs-lookup"><span data-stu-id="0f6d9-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="0f6d9-815">Właściwości w modelu koncepcyjnym są analogiczne do właściwości, które są zdefiniowane w klasie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="0f6d9-816">W ten sam sposób, że właściwości w klasie określenia kształtu elementu klasy i zawierają informacje o obiektach właściwości w modelu koncepcyjnym określenia kształtu typu jednostki i zawierają informacje na temat wystąpień typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="0f6d9-817">**Właściwość** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-818">Element documentation (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-819">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="0f6d9-820">Następujące aspekty mogą być stosowane do **właściwość** element: **Nullable**, **DefaultValue**, **MaxLength**,  **Wartości**, **dokładności**, **skalowania**, **Unicode**, **sortowania**,  **Właściwość ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="0f6d9-821">Zestawy reguł są atrybuty XML, które zawierają informacje dotyczące sposobu wartości właściwości są przechowywane w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-822">Aspektami może być stosowany tylko do właściwości typu **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-823">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-823">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-824">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="0f6d9-825">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-825">Attribute Name</span></span>                                                         | <span data-ttu-id="0f6d9-826">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-826">Is Required</span></span> | <span data-ttu-id="0f6d9-827">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-828">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-828">**Name**</span></span>                                                               | <span data-ttu-id="0f6d9-829">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-829">Yes</span></span>         | <span data-ttu-id="0f6d9-830">Nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-831">**Type**</span></span>                                                               | <span data-ttu-id="0f6d9-832">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-832">Yes</span></span>         | <span data-ttu-id="0f6d9-833">Typ wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-833">The type of the property value.</span></span> <span data-ttu-id="0f6d9-834">Typ wartości właściwości musi być **EDMSimpleType** lub typ złożony (wskazywanym przez w pełni kwalifikowaną nazwą), który znajduje się w zakresie modelu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="0f6d9-835">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-835">**Nullable**</span></span>                                                           | <span data-ttu-id="0f6d9-836">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-836">No</span></span>          | <span data-ttu-id="0f6d9-837">**Wartość true,** (wartość domyślna) lub <strong>False</strong> w zależności od tego, czy właściwość może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="0f6d9-838">> W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-839">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="0f6d9-840">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-840">No</span></span>          | <span data-ttu-id="0f6d9-841">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f6d9-842">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="0f6d9-843">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-843">No</span></span>          | <span data-ttu-id="0f6d9-844">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-845">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="0f6d9-846">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-846">No</span></span>          | <span data-ttu-id="0f6d9-847">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f6d9-848">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-848">**Precision**</span></span>                                                          | <span data-ttu-id="0f6d9-849">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-849">No</span></span>          | <span data-ttu-id="0f6d9-850">Dokładność wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f6d9-851">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-851">**Scale**</span></span>                                                              | <span data-ttu-id="0f6d9-852">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-852">No</span></span>          | <span data-ttu-id="0f6d9-853">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f6d9-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-854">**SRID**</span></span>                                                               | <span data-ttu-id="0f6d9-855">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-855">No</span></span>          | <span data-ttu-id="0f6d9-856">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-857">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f6d9-858">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f6d9-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-859">**Unicode**</span></span>                                                            | <span data-ttu-id="0f6d9-860">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-860">No</span></span>          | <span data-ttu-id="0f6d9-861">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f6d9-862">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-862">**Collation**</span></span>                                                          | <span data-ttu-id="0f6d9-863">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-863">No</span></span>          | <span data-ttu-id="0f6d9-864">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="0f6d9-865">**Właściwość ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="0f6d9-866">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-866">No</span></span>          | <span data-ttu-id="0f6d9-867">**Brak** (wartość domyślna) lub **stałe**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="0f6d9-868">Jeśli wartość jest równa **stałe**, zostanie użyta wartość właściwości w kontroli optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-869">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="0f6d9-870">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-871">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-872">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-872">Example</span></span>

<span data-ttu-id="0f6d9-873">W poniższym przykładzie przedstawiono **EntityType** element z trzech **właściwość** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="0f6d9-874">W poniższym przykładzie przedstawiono **ComplexType** element z pięciu **właściwość** elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="0f6d9-875">RowType Element aplikacji</span><span class="sxs-lookup"><span data-stu-id="0f6d9-875">RowType Element Application</span></span>

<span data-ttu-id="0f6d9-876">**Właściwość** elementów (jako element podrzędny elementu **RowType** elementu) zdefiniuj kształt i charakterystyki dane, które mogą być przekazywane do lub zwracany z funkcji definiowanych przez model.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="0f6d9-877">**Właściwość** element może mieć dokładnie jeden z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-878">Typ CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-878">CollectionType</span></span>
-   <span data-ttu-id="0f6d9-879">Element referenceType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-879">ReferenceType</span></span>
-   <span data-ttu-id="0f6d9-880">RowType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-880">RowType</span></span>

<span data-ttu-id="0f6d9-881">**Właściwość** element może mieć żadnych liczba elementów podrzędnych w adnotacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-882">Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-883">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-883">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-884">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="0f6d9-885">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-885">Attribute Name</span></span>                                                     | <span data-ttu-id="0f6d9-886">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-886">Is Required</span></span> | <span data-ttu-id="0f6d9-887">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-888">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-888">**Name**</span></span>                                                           | <span data-ttu-id="0f6d9-889">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-889">Yes</span></span>         | <span data-ttu-id="0f6d9-890">Nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-891">**Type**</span></span>                                                           | <span data-ttu-id="0f6d9-892">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-892">Yes</span></span>         | <span data-ttu-id="0f6d9-893">Typ wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-894">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-894">**Nullable**</span></span>                                                       | <span data-ttu-id="0f6d9-895">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-895">No</span></span>          | <span data-ttu-id="0f6d9-896">**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="0f6d9-897">> W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-898">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="0f6d9-899">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-899">No</span></span>          | <span data-ttu-id="0f6d9-900">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f6d9-901">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="0f6d9-902">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-902">No</span></span>          | <span data-ttu-id="0f6d9-903">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-904">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="0f6d9-905">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-905">No</span></span>          | <span data-ttu-id="0f6d9-906">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f6d9-907">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-907">**Precision**</span></span>                                                      | <span data-ttu-id="0f6d9-908">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-908">No</span></span>          | <span data-ttu-id="0f6d9-909">Dokładność wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f6d9-910">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-910">**Scale**</span></span>                                                          | <span data-ttu-id="0f6d9-911">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-911">No</span></span>          | <span data-ttu-id="0f6d9-912">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f6d9-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-913">**SRID**</span></span>                                                           | <span data-ttu-id="0f6d9-914">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-914">No</span></span>          | <span data-ttu-id="0f6d9-915">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-916">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f6d9-917">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f6d9-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-918">**Unicode**</span></span>                                                        | <span data-ttu-id="0f6d9-919">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-919">No</span></span>          | <span data-ttu-id="0f6d9-920">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f6d9-921">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-921">**Collation**</span></span>                                                      | <span data-ttu-id="0f6d9-922">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-922">No</span></span>          | <span data-ttu-id="0f6d9-923">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-924">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="0f6d9-925">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-926">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f6d9-927">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-927">Example</span></span>

<span data-ttu-id="0f6d9-928">W poniższym przykładzie przedstawiono **właściwość** elementy służące do określenia kształtu elementu zwracany typ funkcji definiowanych przez model.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="0f6d9-929">Element PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-930">**PropertyRef** element język definicji schematu koncepcyjnego (CSDL) odwołuje się do właściwości typu jednostki, aby wskazać, że właściwość wykona jedną z następujących ról:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="0f6d9-931">Część klucza jednostki (właściwość lub ustaw właściwości typu jednostki, które określają tożsamość).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="0f6d9-932">Co najmniej jeden **PropertyRef** elementy mogą być używane do definiowania klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="0f6d9-933">Koniec zależnych lub jednostki ograniczenia referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="0f6d9-934">**PropertyRef** element może mieć tylko elementów adnotacji (zero lub więcej) jako elementy podrzędne.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-935">Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-936">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-936">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-937">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="0f6d9-938">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-938">Attribute Name</span></span> | <span data-ttu-id="0f6d9-939">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-939">Is Required</span></span> | <span data-ttu-id="0f6d9-940">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="0f6d9-941">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-941">**Name**</span></span>       | <span data-ttu-id="0f6d9-942">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-942">Yes</span></span>         | <span data-ttu-id="0f6d9-943">Nazwa właściwości, której dotyczy odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-944">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="0f6d9-945">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-946">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-947">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-947">Example</span></span>

<span data-ttu-id="0f6d9-948">Poniższy przykład definiuje typ jednostki (**książki**).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="0f6d9-949">Klucz jednostki jest zdefiniowany, odwołując się do **ISBN** właściwość typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="0f6d9-950">W następnym przykładzie dwóch **PropertyRef** elementy są używane w celu wskazania, że dwie właściwości (**identyfikator** i **PublisherId**) są głównym i zależnym końców typu odwołanie ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="0f6d9-951">Element ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-952">**ReferenceType** element język definicji schematu koncepcyjnego (CSDL) Określa odwołanie do typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="0f6d9-953">**ReferenceType** element może być elementem podrzędnym następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="0f6d9-954">ReturnType (funkcja)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="0f6d9-955">Parametr</span><span class="sxs-lookup"><span data-stu-id="0f6d9-955">Parameter</span></span>
-   <span data-ttu-id="0f6d9-956">Typ CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-956">CollectionType</span></span>

<span data-ttu-id="0f6d9-957">**ReferenceType** element jest używany podczas definiowania parametr lub zwracany typ funkcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="0f6d9-958">A **ReferenceType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-959">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-960">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-961">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-961">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-962">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReferenceType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="0f6d9-963">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-963">Attribute Name</span></span> | <span data-ttu-id="0f6d9-964">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-964">Is Required</span></span> | <span data-ttu-id="0f6d9-965">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="0f6d9-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-966">**Type**</span></span>       | <span data-ttu-id="0f6d9-967">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-967">Yes</span></span>         | <span data-ttu-id="0f6d9-968">Nazwa typu jednostki, do którego nastąpiło odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-969">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReferenceType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="0f6d9-970">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-971">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-972">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-972">Example</span></span>

<span data-ttu-id="0f6d9-973">W poniższym przykładzie przedstawiono **ReferenceType** element używany jako element podrzędny elementu **parametru** elementu w funkcji definiowanych przez model, który akceptuje odwołanie do **osoby** jednostki Typ:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="0f6d9-974">W poniższym przykładzie przedstawiono **ReferenceType** element używany jako element podrzędny elementu **ReturnType** — element (funkcja) w funkcji definiowanych przez model, który zwraca odwołanie do **osoby**typ jednostki:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="0f6d9-975">Element ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-976">A **ReferentialConstraint** element w język definicji schematu koncepcyjnego (CSDL) definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f6d9-977">W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f6d9-978">Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f6d9-979">Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="0f6d9-980">Jeśli klucz obcy, która jest widoczna w jednej jednostce typu odwołuje się do właściwości na inny typ jednostki, **ReferentialConstraint** element definiuje skojarzenie między tymi typami dwie jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="0f6d9-981">Ponieważ **ReferentialConstraint** element udostępnia dowiedzieć się, jak dwa typy jednostek są powiązane z nie odpowiadającego elementu AssociationSetMapping niezbędne jest w języku specyfikacji mapowanie (MSL).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="0f6d9-982">Skojarzenie między dwoma typami jednostek, które nie mają klucze obce udostępniane musi mieć odpowiedni **AssociationSetMapping** element, aby zamapować informacji o skojarzeniu ze źródłem danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="0f6d9-983">Jeśli klucz obcy nie jest uwidaczniana typu encji **ReferentialConstraint** elementu można zdefiniować tylko klucz do podstawowego ograniczenia klucza podstawowego między typem jednostki i innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="0f6d9-984">A **ReferentialConstraint** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-985">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-986">Podmiot zabezpieczeń (dokładnie jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="0f6d9-987">Zależnych od ustawień lokalnych (dokładnie jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="0f6d9-988">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-989">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-989">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-990">**ReferentialConstraint** element może mieć dowolną liczbę adnotacji atrybutów (niestandardowe atrybuty XML).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="0f6d9-991">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-992">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-993">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-993">Example</span></span>

<span data-ttu-id="0f6d9-994">W poniższym przykładzie przedstawiono **ReferentialConstraint** używany jako część definicji elementu **PublishedBy** skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="0f6d9-995">Element ReturnType (funkcja) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-996">**ReturnType** (funkcja) element w język definicji schematu koncepcyjnego (CSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w elemencie Function.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="0f6d9-997">Można również określić typ zwracany z funkcji **ReturnType** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="0f6d9-998">Zwraca typy mogą być **EdmSimpleType**, typ jednostki, typu złożonego, typ wiersza, typu referencyjnego lub zbiór jednego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="0f6d9-999">Zwracany typ funkcji można określić za pomocą albo **typu** atrybutu **ReturnType** elementu (funkcja), czy jeden z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-1000">Typ CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1000">CollectionType</span></span>
-   <span data-ttu-id="0f6d9-1001">Element referenceType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1001">ReferenceType</span></span>
-   <span data-ttu-id="0f6d9-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-1003">Model nie zostanie zweryfikowany, jeśli określisz, że funkcja zwracany typ zarówno **typu** atrybutu **ReturnType** elementu (funkcja) i jeden z elementów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1004">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1004">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1005">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReturnType** — element (funkcja).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="0f6d9-1006">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1006">Attribute Name</span></span> | <span data-ttu-id="0f6d9-1007">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1007">Is Required</span></span> | <span data-ttu-id="0f6d9-1008">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="0f6d9-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1009">**ReturnType**</span></span> | <span data-ttu-id="0f6d9-1010">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1010">No</span></span>          | <span data-ttu-id="0f6d9-1011">Typ zwracany przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-1012">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** — element (funkcja).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="0f6d9-1013">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1014">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-1015">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1015">Example</span></span>

<span data-ttu-id="0f6d9-1016">W poniższym przykładzie użyto **funkcja** elementu, aby zdefiniować funkcji, która zwraca liczbę lat książki znajdował się w drukowania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="0f6d9-1017">Należy zauważyć, że typ zwracany jest określony przez **typu** atrybutu **ReturnType** — element (funkcja).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="0f6d9-1018">Element ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-1019">**ReturnType** elementu (element FunctionImport) w język definicji schematu koncepcyjnego (CSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w elemencie FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="0f6d9-1020">Można również określić typ zwracany z funkcji **ReturnType** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="0f6d9-1021">Zwraca typy mogą być dowolnej kolekcji typu jednostki, typu złożonego lub **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="0f6d9-1022">Zwracany typ funkcji jest określony za pomocą **typu** atrybutu **ReturnType** elementu (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1023">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1023">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1024">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReturnType** elementu (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="0f6d9-1025">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1025">Attribute Name</span></span> | <span data-ttu-id="0f6d9-1026">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1026">Is Required</span></span> | <span data-ttu-id="0f6d9-1027">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1028">**Type**</span></span>       | <span data-ttu-id="0f6d9-1029">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1029">No</span></span>          | <span data-ttu-id="0f6d9-1030">Typ, który zwraca funkcja.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1030">The type that the function returns.</span></span> <span data-ttu-id="0f6d9-1031">Wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="0f6d9-1032">**Obiekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1032">**EntitySet**</span></span>  | <span data-ttu-id="0f6d9-1033">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1033">No</span></span>          | <span data-ttu-id="0f6d9-1034">Jeśli funkcja zwraca kolekcję jednostek typy wartości **EntitySet** musi być zestaw jednostek do której należy kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="0f6d9-1035">W przeciwnym razie **EntitySet** atrybutu nie może być używany.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-1036">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** elementu (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="0f6d9-1037">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1038">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-1039">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1039">Example</span></span>

<span data-ttu-id="0f6d9-1040">W poniższym przykładzie użyto **FunctionImport** zwracającego książki i wydawców.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="0f6d9-1041">Należy pamiętać, że funkcja zwraca dwa zestawy wyników i w związku z tym dwa **ReturnType** są określone elementy (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="0f6d9-1042">Element RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-1043">A **RowType** element w język definicji schematu koncepcyjnego (CSDL) definiuje strukturę nienazwane jako parametr lub zwracany typ funkcji zdefiniowanych w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="0f6d9-1044">A **RowType** element może być elementem podrzędnym elementu następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="0f6d9-1045">Typ CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1045">CollectionType</span></span>
-   <span data-ttu-id="0f6d9-1046">Parametr</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1046">Parameter</span></span>
-   <span data-ttu-id="0f6d9-1047">ReturnType (funkcja)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1047">ReturnType (Function)</span></span>

<span data-ttu-id="0f6d9-1048">A **RowType** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-1049">Właściwości (jeden lub więcej)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1049">Property (one or more)</span></span>
-   <span data-ttu-id="0f6d9-1050">Elementów adnotacji (zero lub więcej)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1051">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1051">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1052">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **RowType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="0f6d9-1053">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1054">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-1055">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1055">Example</span></span>

<span data-ttu-id="0f6d9-1056">W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="0f6d9-1057">Element schematu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-1058">**Schematu** element jest elementem głównym modelu koncepcyjnego definicji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="0f6d9-1059">Zawiera definicje dla obiektów, funkcji i kontenerów, które tworzą modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="0f6d9-1060">**Schematu** element może zawierać zero lub więcej z następujących elementów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-1061">za pomocą</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1061">Using</span></span>
-   <span data-ttu-id="0f6d9-1062">Obiekt EntityContainer</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1062">EntityContainer</span></span>
-   <span data-ttu-id="0f6d9-1063">Typ entityType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1063">EntityType</span></span>
-   <span data-ttu-id="0f6d9-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1064">EnumType</span></span>
-   <span data-ttu-id="0f6d9-1065">Skojarzenie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1065">Association</span></span>
-   <span data-ttu-id="0f6d9-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1066">ComplexType</span></span>
-   <span data-ttu-id="0f6d9-1067">Funkcja</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1067">Function</span></span>

<span data-ttu-id="0f6d9-1068">A **schematu** element może zawierać zero lub jeden elementów adnotacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-1069">**Funkcja** element i adnotacji elementy są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="0f6d9-1070">**Schematu** element używa **Namespace** atrybut do definiowania przestrzeni nazw dla typu jednostki, typu złożonego i skojarzenia obiekty w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="0f6d9-1071">W przestrzeni nazw nie dwa obiekty mogą mieć takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="0f6d9-1072">Przestrzenie nazw może obejmować wiele **schematu** elementów i wiele plików .csdl.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="0f6d9-1073">Model koncepcyjny przestrzeni nazw różni się od przestrzeni nazw XML **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="0f6d9-1074">Model koncepcyjny przestrzeni nazw (zgodnie z definicją **Namespace** atrybutu) to logiczny kontener przeznaczony dla typów jednostek, typy złożone i typy stowarzyszenie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="0f6d9-1075">Przestrzeń nazw XML (wskazywanym przez **xmlns** atrybutu) z **schematu** element jest domyślny obszar nazw dla elementów podrzędnych i atrybutów **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="0f6d9-1076">Obszary nazw XML w postaci http://schemas.microsoft.com/ado/YYYY/MM/edm (gdzie RRRR i MM stanowi rok i miesiąc odpowiednio) są zarezerwowane dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1077">Niestandardowe elementy i atrybuty nie może być w przestrzeni nazw, które mają postać.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1078">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1078">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1079">W poniższej tabeli opisano atrybuty mogą być stosowane do **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="0f6d9-1080">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1080">Attribute Name</span></span> | <span data-ttu-id="0f6d9-1081">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1081">Is Required</span></span> | <span data-ttu-id="0f6d9-1082">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1083">**Namespace**</span></span>  | <span data-ttu-id="0f6d9-1084">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1084">Yes</span></span>         | <span data-ttu-id="0f6d9-1085">Przestrzeń nazw modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="0f6d9-1086">Wartość **Namespace** atrybut jest używany w celu utworzenia w pełni kwalifikowana nazwa typu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="0f6d9-1087">Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw Simple.Example.Model, a następnie w pełni kwalifikowana nazwa **EntityType** jest SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="0f6d9-1088">Nie można użyć następujących ciągów jako wartość pozycji **Namespace** atrybut: **systemu**, **przejściowy**, lub **Edm**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="0f6d9-1089">Wartość **Namespace** atrybut nie może być taka sama jak wartość **Namespace** atrybutu w elemencie schematu SSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="0f6d9-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1090">**Alias**</span></span>      | <span data-ttu-id="0f6d9-1091">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1091">No</span></span>          | <span data-ttu-id="0f6d9-1092">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="0f6d9-1093">Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw Simple.Example.Model i wartość **Alias** atrybut jest *modelu*, wówczas można użyć Model.Customer jako w pełni kwalifikowana nazwa **typu EntityType.**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-1094">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="0f6d9-1095">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1096">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-1097">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1097">Example</span></span>

<span data-ttu-id="0f6d9-1098">W poniższym przykładzie przedstawiono **schematu** element, który zawiera **EntityContainer** element, dwa **EntityType** elementów, a drugi **skojarzenia** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="0f6d9-1099">Element TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-1100">**TypeRef** element język definicji schematu koncepcyjnego (CSDL) zawiera odwołanie do istniejącego typu nazwanego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="0f6d9-1101">**TypeRef** element może być elementem podrzędnym elementu CollectionType, który jest używany do określenia, czy funkcja jest kolekcją jako parametr lub zwracany typ.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="0f6d9-1102">A **TypeRef** element może mieć następujących elementów podrzędnych (w podanej kolejności):</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f6d9-1103">Dokumentacja (zero lub jeden element)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f6d9-1104">Elementów adnotacji (zero lub więcej elementów)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1105">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1105">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1106">W poniższej tabeli opisano atrybuty, które mogą być stosowane do **TypeRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="0f6d9-1107">Należy pamiętać, że **DefaultValue**, **MaxLength**, **FixedLength**, **dokładności**, **skalowania**,  **Unicode**, i **sortowania** atrybuty dotyczą tylko **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="0f6d9-1108">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="0f6d9-1109">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1109">Is Required</span></span> | <span data-ttu-id="0f6d9-1110">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1111">**Type**</span></span>                                                           | <span data-ttu-id="0f6d9-1112">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1112">No</span></span>          | <span data-ttu-id="0f6d9-1113">Nazwa typu, do którego nastąpiło odwołanie.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="0f6d9-1114">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="0f6d9-1115">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1115">No</span></span>          | <span data-ttu-id="0f6d9-1116">**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="0f6d9-1117">> W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f6d9-1118">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="0f6d9-1119">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1119">No</span></span>          | <span data-ttu-id="0f6d9-1120">Wartość domyślna właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f6d9-1121">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="0f6d9-1122">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1122">No</span></span>          | <span data-ttu-id="0f6d9-1123">Maksymalna długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f6d9-1124">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="0f6d9-1125">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1125">No</span></span>          | <span data-ttu-id="0f6d9-1126">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f6d9-1127">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1127">**Precision**</span></span>                                                      | <span data-ttu-id="0f6d9-1128">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1128">No</span></span>          | <span data-ttu-id="0f6d9-1129">Dokładność wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f6d9-1130">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1130">**Scale**</span></span>                                                          | <span data-ttu-id="0f6d9-1131">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1131">No</span></span>          | <span data-ttu-id="0f6d9-1132">Skala wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f6d9-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1133">**SRID**</span></span>                                                           | <span data-ttu-id="0f6d9-1134">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1134">No</span></span>          | <span data-ttu-id="0f6d9-1135">Identyfikator odwołania przestrzennego systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f6d9-1136">Prawidłowy tylko w przypadku właściwości typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f6d9-1137">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f6d9-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="0f6d9-1139">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1139">No</span></span>          | <span data-ttu-id="0f6d9-1140">**Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f6d9-1141">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1141">**Collation**</span></span>                                                      | <span data-ttu-id="0f6d9-1142">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1142">No</span></span>          | <span data-ttu-id="0f6d9-1143">Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-1144">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f6d9-1145">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1146">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-1147">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1147">Example</span></span>

<span data-ttu-id="0f6d9-1148">W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **TypeRef** — element (jako element podrzędny elementu **CollectionType** elementu) do określenia, że funkcja ta akceptuje zbiór  **Dział** typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="0f6d9-1149">Za pomocą elementu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="0f6d9-1150">**Using** element język definicji schematu koncepcyjnego (CSDL) importuje zawartość modelu koncepcyjnego, który znajduje się w innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="0f6d9-1151">Ustawiając wartość **Namespace** atrybut, można się odwoływać do typów jednostek, typy złożone i typy skojarzenia, które są zdefiniowane w modelu koncepcyjnym innego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="0f6d9-1152">Więcej niż jeden **Using** element może być elementem podrzędnym **schematu** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-1153">**Using** element CSDL nie działa dokładnie tak jak **przy użyciu** instrukcji w języku programowania.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="0f6d9-1154">Importując przestrzeń nazw z **przy użyciu** instrukcji w języku programowania, możesz nie wpływają na obiekty w oryginalnym przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="0f6d9-1155">W CSDL importowanych przestrzeni nazw może zawierać typ jednostki, który pochodzi od typu jednostki w oryginalnym przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="0f6d9-1156">Może to wpłynąć na zestawy jednostek, zadeklarowany w oryginalnym przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="0f6d9-1157">**Using** element może mieć następujące elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="0f6d9-1158">Dokumentacja (zero lub jeden elementy dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f6d9-1159">Elementów adnotacji (zero lub więcej elementów dozwolone)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f6d9-1160">Odpowiednie atrybuty</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1160">Applicable Attributes</span></span>

<span data-ttu-id="0f6d9-1161">W poniższej tabeli opisano atrybuty mogą być stosowane do **Using** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="0f6d9-1162">Nazwa atrybutu</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1162">Attribute Name</span></span> | <span data-ttu-id="0f6d9-1163">Jest wymagany</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1163">Is Required</span></span> | <span data-ttu-id="0f6d9-1164">Wartość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1165">**Namespace**</span></span>  | <span data-ttu-id="0f6d9-1166">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1166">Yes</span></span>         | <span data-ttu-id="0f6d9-1167">Nazwa importowanych przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="0f6d9-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1168">**Alias**</span></span>      | <span data-ttu-id="0f6d9-1169">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1169">Yes</span></span>         | <span data-ttu-id="0f6d9-1170">Identyfikator używany zamiast nazwy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="0f6d9-1171">Mimo że ten atrybut jest wymagany, nie jest wymagane, aby z niego korzystać zamiast nazwy przestrzeni nazw kwalifikowania nazwy obiektów.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f6d9-1172">Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **Using** elementu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="0f6d9-1173">Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f6d9-1174">W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f6d9-1175">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1175">Example</span></span>

<span data-ttu-id="0f6d9-1176">W poniższym przykładzie pokazano **Using** elementu używane do importowania przestrzeni nazw, która jest definiowane w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="0f6d9-1177">Należy pamiętać, że przestrzeń nazw dla **schematu** elementu wyświetlana jest `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="0f6d9-1178">`Address` Właściwość `Publisher` **EntityType** to typ złożony, który jest zdefiniowany w `ExtendedBooksModel` przestrzeni nazw (zaimportowane wraz z **Using** elementu).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="0f6d9-1179">Atrybuty adnotacji (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="0f6d9-1180">Atrybuty adnotacji w język definicji schematu koncepcyjnego (CSDL) są niestandardowe atrybuty XML w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="0f6d9-1181">Oprócz prawidłowe struktury XML, wymaga spełnienia następujących warunków adnotacji atrybutów:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="0f6d9-1182">Atrybuty adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="0f6d9-1183">Więcej niż jeden atrybut adnotacji można stosować do danego elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="0f6d9-1184">W pełni kwalifikowanej nazwy wszelkie atrybuty dwóch adnotacji nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="0f6d9-1185">Atrybuty adnotacji może służyć do zapewnienia dodatkowe metadane na temat elementów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="0f6d9-1186">W czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm możliwy jest metadanych elementów adnotacji.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-1187">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1187">Example</span></span>

<span data-ttu-id="0f6d9-1188">W poniższym przykładzie przedstawiono **EntityType** element z atrybutem adnotacji (**CustomAttribute —**).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="0f6d9-1189">W przykładzie pokazano również element adnotacji stosowane do elementu typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="0f6d9-1190">Poniższy kod służy do pobierania metadanych w atrybucie adnotacji i zapisuje go do konsoli:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="0f6d9-1191">Powyższy kod zakłada, że `School.csdl` plik znajduje się w katalogu wyjściowego projektu i dodano następujące `Imports` i `Using` instrukcje do projektu:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="0f6d9-1192">Elementów adnotacji (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="0f6d9-1193">Elementy adnotacji w język definicji schematu koncepcyjnego (CSDL) są niestandardowe elementy XML w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="0f6d9-1194">Oprócz prawidłowe struktury XML, wymaga spełnienia następujących warunków elementów adnotacji:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="0f6d9-1195">Elementów adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="0f6d9-1196">Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="0f6d9-1197">W pełni kwalifikowanej nazwy dowolne elementy dwóch adnotacji nie może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="0f6d9-1198">Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych danego elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="0f6d9-1199">Elementów adnotacji może służyć do zapewnienia dodatkowe metadane na temat elementów w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="0f6d9-1200">Począwszy od programu .NET Framework w wersji 4, metadanych elementów adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-1201">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1201">Example</span></span>

<span data-ttu-id="0f6d9-1202">W poniższym przykładzie przedstawiono **EntityType** element z element adnotacji (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="0f6d9-1203">Przykład pokazują również adnotacji zastosowany do elementu typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="0f6d9-1204">Poniższy kod służy do pobierania metadanych w elemencie adnotacji i zapisuje go do konsoli:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="0f6d9-1205">Powyższy kod założono, że plik School.csdl znajduje się w katalogu wyjściowego projektu, i dodano następujące `Imports` i `Using` instrukcje do projektu:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="0f6d9-1206">Model koncepcyjny typów (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="0f6d9-1207">Język definicji schematu koncepcyjnego (CSDL) obsługuje zestaw abstrakcyjne pierwotne typy danych, nazywane **EDMSimpleTypes**, który definiują właściwości w modelu koncepcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="0f6d9-1208">**EDMSimpleTypes** serwerów proxy dla typów danych pierwotnych, które są obsługiwane w środowisku hostingu lub magazynu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="0f6d9-1209">Poniższa tabela zawiera listę typów danych pierwotnych, które są obsługiwane przez CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="0f6d9-1210">W tabeli przedstawiono listę zestawów reguł, które mogą być stosowane do każdego **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="0f6d9-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="0f6d9-1212">Opis</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1212">Description</span></span>                                                | <span data-ttu-id="0f6d9-1213">Zastosowanie zestawów reguł</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="0f6d9-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="0f6d9-1215">Zawiera dane binarne.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="0f6d9-1216">Element MaxLength, wartości null, domyślne</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="0f6d9-1217">**Typem Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="0f6d9-1218">Zawiera wartość **true** lub **false**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="0f6d9-1219">Wartość null, domyślne</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="0f6d9-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="0f6d9-1221">Zawiera wartość Liczba całkowita bez znaku 8-bitowych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="0f6d9-1222">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="0f6d9-1224">Reprezentuje datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="0f6d9-1225">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="0f6d9-1227">Zawiera datę i godzinę w ciągu kilku minut od GMT przesunięcia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="0f6d9-1228">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="0f6d9-1230">Zawiera wartość liczbową ze stałym dokładności i skali.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="0f6d9-1231">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="0f6d9-1233">Zawiera zmiennoprzecinkowa numer z dokładnością do 15 cyfr</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="0f6d9-1234">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="0f6d9-1236">Zawiera zmiennoprzecinkowej liczba z 7-cyfrowy dokładnością.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="0f6d9-1237">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="0f6d9-1239">Zawiera unikatowy identyfikator 16-bajtowy.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="0f6d9-1240">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="0f6d9-1242">Zawiera wartość liczby całkowitej ze znakiem 16-bitowych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="0f6d9-1243">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1244">**Typem Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="0f6d9-1245">Zawiera wartość całkowita 32-bitowa.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="0f6d9-1246">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="0f6d9-1248">Zawiera wartość całkowita 64-bitowa.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="0f6d9-1249">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="0f6d9-1251">Zawiera wartość całkowita 8-bitowa.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="0f6d9-1252">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1253">**Edm.String**</span></span>                   | <span data-ttu-id="0f6d9-1254">Zawiera dane znaków.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1254">Contains character data.</span></span>                                   | <span data-ttu-id="0f6d9-1255">Unicode dla wpisu, MaxLength, sortowanie, dokładności, dopuszczającego wartość null, domyślne</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="0f6d9-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="0f6d9-1257">Zawiera porze dnia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="0f6d9-1258">Precyzja dopuszczającego wartość null, domyślny</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f6d9-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="0f6d9-1260">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="0f6d9-1262">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="0f6d9-1264">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="0f6d9-1266">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="0f6d9-1268">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="0f6d9-1270">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="0f6d9-1272">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="0f6d9-1274">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="0f6d9-1276">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="0f6d9-1278">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="0f6d9-1280">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="0f6d9-1282">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="0f6d9-1284">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="0f6d9-1286">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="0f6d9-1288">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f6d9-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="0f6d9-1290">Wartość null, domyślnie, SRID</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="0f6d9-1291">Zestawy reguł (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1291">Facets (CSDL)</span></span>

<span data-ttu-id="0f6d9-1292">Aspekty w język definicji schematu koncepcyjnego (CSDL) reprezentują ograniczenia dotyczące właściwości typów jednostek i typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="0f6d9-1293">Zestawy reguł są wyświetlane jako atrybuty XML w następujących elementów CSDL:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="0f6d9-1294">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1294">Property</span></span>
-   <span data-ttu-id="0f6d9-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1295">TypeRef</span></span>
-   <span data-ttu-id="0f6d9-1296">Parametr</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1296">Parameter</span></span>

<span data-ttu-id="0f6d9-1297">W poniższej tabeli opisano aspekty, które są obsługiwane przez CSDL.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="0f6d9-1298">Wszystkie zestawy reguł są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1298">All facets are optional.</span></span> <span data-ttu-id="0f6d9-1299">Niektóre aspekty wymienione poniżej są używane przez program Entity Framework, podczas generowania bazy danych na podstawie modelu koncepcyjnego.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6d9-1300">Informacje o typach danych w modelu koncepcyjnym na ten temat można znaleźć w koncepcyjny modelu typy (CSDL).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="0f6d9-1301">zestaw reguł</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1301">Facet</span></span>               | <span data-ttu-id="0f6d9-1302">Opis</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="0f6d9-1303">Informacje zawarte w tym artykule dotyczą</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="0f6d9-1304">Użyto do generowania bazy danych</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1304">Used for the database generation</span></span> | <span data-ttu-id="0f6d9-1305">Używane przez środowisko uruchomieniowe</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="0f6d9-1306">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1306">**Collation**</span></span>       | <span data-ttu-id="0f6d9-1307">Określa kolejność sortowania (lub sekwencji sortowania) do użycia podczas przeprowadzania porównania i kolejność operacji na wartościach właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="0f6d9-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f6d9-1309">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1309">Yes</span></span>                              | <span data-ttu-id="0f6d9-1310">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1310">No</span></span>                  |
| <span data-ttu-id="0f6d9-1311">**Właściwość ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="0f6d9-1312">Wskazuje, że wartość właściwości powinien być używany w celu pomyślnych kontroli współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="0f6d9-1313">Wszystkie **EDMSimpleType** właściwości</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f6d9-1314">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1314">No</span></span>                               | <span data-ttu-id="0f6d9-1315">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1315">Yes</span></span>                 |
| <span data-ttu-id="0f6d9-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1316">**Default**</span></span>         | <span data-ttu-id="0f6d9-1317">Określa domyślną wartość właściwości, jeśli nie dostarczono żadnej wartości dla wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="0f6d9-1318">Wszystkie **EDMSimpleType** właściwości</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f6d9-1319">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1319">Yes</span></span>                              | <span data-ttu-id="0f6d9-1320">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1320">Yes</span></span>                 |
| <span data-ttu-id="0f6d9-1321">**Wartości**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1321">**FixedLength**</span></span>     | <span data-ttu-id="0f6d9-1322">Określa, czy długość wartości właściwości mogą się różnić.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="0f6d9-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f6d9-1324">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1324">Yes</span></span>                              | <span data-ttu-id="0f6d9-1325">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1325">No</span></span>                  |
| <span data-ttu-id="0f6d9-1326">**Element maxLength**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1326">**MaxLength**</span></span>       | <span data-ttu-id="0f6d9-1327">Określa maksymalną długość wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="0f6d9-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f6d9-1329">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1329">Yes</span></span>                              | <span data-ttu-id="0f6d9-1330">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1330">No</span></span>                  |
| <span data-ttu-id="0f6d9-1331">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1331">**Nullable**</span></span>        | <span data-ttu-id="0f6d9-1332">Określa, czy właściwość może mieć **null** wartość.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="0f6d9-1333">Wszystkie **EDMSimpleType** właściwości</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f6d9-1334">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1334">Yes</span></span>                              | <span data-ttu-id="0f6d9-1335">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1335">Yes</span></span>                 |
| <span data-ttu-id="0f6d9-1336">**Precyzja**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1336">**Precision**</span></span>       | <span data-ttu-id="0f6d9-1337">Dla właściwości typu **dziesiętna**, określa liczbę cyfr, może mieć wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="0f6d9-1338">Dla właściwości typu **czasu**, **daty/godziny**, i **DateTimeOffset**, określa liczbę cyfr ułamkowych części sekundy w wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="0f6d9-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="0f6d9-1340">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1340">Yes</span></span>                              | <span data-ttu-id="0f6d9-1341">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1341">No</span></span>                  |
| <span data-ttu-id="0f6d9-1342">**Skala**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1342">**Scale**</span></span>           | <span data-ttu-id="0f6d9-1343">Określa liczbę cyfr po prawej stronie przecinka dziesiętnego dla wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="0f6d9-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="0f6d9-1345">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1345">Yes</span></span>                              | <span data-ttu-id="0f6d9-1346">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1346">No</span></span>                  |
| <span data-ttu-id="0f6d9-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1347">**SRID**</span></span>            | <span data-ttu-id="0f6d9-1348">Określa identyfikator System przestrzenne odwołanie do systemu.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="0f6d9-1349">Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="0f6d9-1350">**Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="0f6d9-1351">Nie</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1351">No</span></span>                               | <span data-ttu-id="0f6d9-1352">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1352">Yes</span></span>                 |
| <span data-ttu-id="0f6d9-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1353">**Unicode**</span></span>         | <span data-ttu-id="0f6d9-1354">Wskazuje, czy wartość właściwości jest przechowywana jako Unicode.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="0f6d9-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f6d9-1356">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1356">Yes</span></span>                              | <span data-ttu-id="0f6d9-1357">Tak</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="0f6d9-1358">Podczas generowania bazę danych z modelu koncepcyjnego, Kreator bazy danych Generowanie rozpoznaje wartość **parametru StoreGeneratedPattern** atrybutu na **właściwość** elementu, jeśli znajduje się w następujących przestrzeń nazw: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="0f6d9-1359">Obsługiwane wartości dla atrybutu to **tożsamości** i **obliczane**.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="0f6d9-1360">Wartość **tożsamości** dadzą kolumny bazy danych przy użyciu wartości tożsamości, który jest generowany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="0f6d9-1361">Wartość **obliczane** dadzą kolumna z wartością, która jest kolumną obliczaną w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="0f6d9-1362">Przykład</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1362">Example</span></span>

<span data-ttu-id="0f6d9-1363">Poniższy przykład przedstawia aspektami stosowany do właściwości typu jednostki:</span><span class="sxs-lookup"><span data-stu-id="0f6d9-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
