---
title: Relacje — EF Core
description: Jak skonfigurować relacje między typami jednostek podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 452169c902d56fda0a65a5c2846a9b42c80640fd
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824761"
---
# <a name="relationships"></a><span data-ttu-id="432e4-103">Relacje</span><span class="sxs-lookup"><span data-stu-id="432e4-103">Relationships</span></span>

<span data-ttu-id="432e4-104">Relacja określa, jak dwie jednostki są powiązane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="432e4-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="432e4-105">W relacyjnej bazie danych jest to reprezentowane przez ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="432e4-106">Większość przykładów w tym artykule używa relacji jeden-do-wielu, aby przedstawić koncepcje.</span><span class="sxs-lookup"><span data-stu-id="432e4-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="432e4-107">Przykłady relacji jeden-do-jednego i wiele-do-wielu można znaleźć w sekcji [inne wzorce relacji](#other-relationship-patterns) na końcu artykułu.</span><span class="sxs-lookup"><span data-stu-id="432e4-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="432e4-108">Definicja warunków</span><span class="sxs-lookup"><span data-stu-id="432e4-108">Definition of terms</span></span>

<span data-ttu-id="432e4-109">Istnieje kilka terminów używanych do opisywania relacji</span><span class="sxs-lookup"><span data-stu-id="432e4-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="432e4-110">**Jednostka zależna:** Jest to jednostka, która zawiera właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="432e4-111">Czasami określana jako "podrzędny" relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="432e4-112">**Podmiot podmiotu zabezpieczeń:** Jest to jednostka, która zawiera właściwości klucza podstawowego/alternatywnego.</span><span class="sxs-lookup"><span data-stu-id="432e4-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="432e4-113">Czasami określana jako "Parent" relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="432e4-114">**Klucz obcy:** Właściwości w jednostce zależnej, które są używane do przechowywania wartości klucza podmiotu zabezpieczeń powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="432e4-114">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="432e4-115">**Klucz podmiotu zabezpieczeń:** Właściwości, które jednoznacznie identyfikują jednostkę podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-115">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="432e4-116">Może to być klucz podstawowy lub klucz alternatywny.</span><span class="sxs-lookup"><span data-stu-id="432e4-116">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="432e4-117">**Właściwość nawigacji:** Właściwość zdefiniowana dla podmiotu zabezpieczeń i/lub zależnego, która odwołuje się do powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="432e4-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="432e4-118">**Właściwość nawigacji kolekcji:** Właściwość nawigacji, która zawiera odwołania do wielu powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="432e4-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="432e4-119">**Właściwość nawigacji odwołania:** Właściwość nawigacji, która zawiera odwołanie do pojedynczej powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="432e4-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="432e4-120">**Właściwość nawigacji odwrotnej:** Omawiając konkretną właściwość nawigacji, ten termin odnosi się do właściwości nawigacji na drugim końcu relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="432e4-121">**Relacja odwołująca się do samego siebie:** Relacja, w której zależne i główne typy jednostek są takie same.</span><span class="sxs-lookup"><span data-stu-id="432e4-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="432e4-122">Poniższy kod przedstawia relację "jeden do wielu" między `Blog` i `Post`</span><span class="sxs-lookup"><span data-stu-id="432e4-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

* <span data-ttu-id="432e4-123">`Post` jest jednostką zależną</span><span class="sxs-lookup"><span data-stu-id="432e4-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="432e4-124">`Blog` jest jednostką główną</span><span class="sxs-lookup"><span data-stu-id="432e4-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="432e4-125">`Post.BlogId` jest kluczem obcym</span><span class="sxs-lookup"><span data-stu-id="432e4-125">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="432e4-126">`Blog.BlogId` jest kluczem podmiotu zabezpieczeń (w tym przypadku jest to klucz podstawowy, a nie klucz alternatywny)</span><span class="sxs-lookup"><span data-stu-id="432e4-126">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="432e4-127">`Post.Blog` jest właściwością nawigacji referencyjnej</span><span class="sxs-lookup"><span data-stu-id="432e4-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="432e4-128">`Blog.Posts` jest właściwością nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="432e4-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="432e4-129">`Post.Blog` jest właściwością nawigacji odwrotnej `Blog.Posts` (i odwrotnie)</span><span class="sxs-lookup"><span data-stu-id="432e4-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="432e4-130">Konwencje</span><span class="sxs-lookup"><span data-stu-id="432e4-130">Conventions</span></span>

<span data-ttu-id="432e4-131">Domyślnie relacja zostanie utworzona, gdy w typie zostanie wykryta właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="432e4-132">Właściwość jest uznawana za właściwość nawigacji, jeśli typ wskazujący nie może być mapowany jako typ skalarny przez bieżącego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="432e4-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="432e4-133">Relacje, które są odnajdywane według Konwencji, będą zawsze ukierunkowane na klucz podstawowy jednostki głównej.</span><span class="sxs-lookup"><span data-stu-id="432e4-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="432e4-134">Aby określić klucz alternatywny, należy przeprowadzić dodatkową konfigurację przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="432e4-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="432e4-135">W pełni zdefiniowane relacje</span><span class="sxs-lookup"><span data-stu-id="432e4-135">Fully defined relationships</span></span>

<span data-ttu-id="432e4-136">Najbardziej typowym wzorcem relacji jest posiadanie właściwości nawigacji zdefiniowanych na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="432e4-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="432e4-137">Jeśli para właściwości nawigacji zostanie znaleziona między dwoma typami, zostaną one skonfigurowane jako właściwości nawigacji odwrotnej tej samej relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="432e4-138">Jeśli jednostka zależna zawiera właściwość o nazwie Math jednego z tych wzorców, zostanie ona skonfigurowana jako klucz obcy:</span><span class="sxs-lookup"><span data-stu-id="432e4-138">If the dependent entity contains a property with a name mathing one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

<span data-ttu-id="432e4-139">W tym przykładzie wyróżnione właściwości zostaną użyte do skonfigurowania relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="432e4-140">Jeśli właściwość jest kluczem podstawowym lub jest typu niezgodnego z kluczem podmiotu zabezpieczeń, nie zostanie on skonfigurowany jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="432e4-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="432e4-141">Przed EF Core 3,0 właściwość o nazwie dokładnie taka sama jak właściwość klucza głównego [była również zgodna jako klucz obcy](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span><span class="sxs-lookup"><span data-stu-id="432e4-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="432e4-142">Brak właściwości klucza obcego</span><span class="sxs-lookup"><span data-stu-id="432e4-142">No foreign key property</span></span>

<span data-ttu-id="432e4-143">Chociaż zaleca się zdefiniowanie właściwości klucza obcego w klasie jednostki zależnej, nie jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="432e4-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="432e4-144">Jeśli właściwość klucza obcego nie zostanie znaleziona, [Właściwość klucza obcego cienia](shadow-properties.md) zostanie wprowadzona z nazwą `<navigation property name><principal key property name>` lub `<principal entity name><principal key property name>`, jeśli w typie zależnym nie ma żadnej nawigacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

<span data-ttu-id="432e4-145">W tym przykładzie klucz obcy Shadow jest `BlogId`, ponieważ oczekuje się, że nazwa nawigacji będzie nadmiarowa.</span><span class="sxs-lookup"><span data-stu-id="432e4-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="432e4-146">Jeśli właściwość o tej samej nazwie już istnieje, nazwa właściwości cienia zostanie poddana sufiksem liczbowym.</span><span class="sxs-lookup"><span data-stu-id="432e4-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="432e4-147">Właściwość pojedynczej nawigacji</span><span class="sxs-lookup"><span data-stu-id="432e4-147">Single navigation property</span></span>

<span data-ttu-id="432e4-148">Dołączenie tylko jednej właściwości nawigacji (bez nawigacji odwrotnej i brak właściwości klucza obcego) nie wystarcza do zdefiniowania relacji przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="432e4-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="432e4-149">Można również mieć pojedynczą właściwość nawigacji i właściwość klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="432e4-150">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="432e4-150">Limitations</span></span>

<span data-ttu-id="432e4-151">W przypadku zdefiniowania wielu właściwości nawigacji między dwoma typami (oznacza to, że więcej niż tylko jedna para nawigacji wskazuje na siebie) relacje reprezentowane przez właściwości nawigacji są niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="432e4-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="432e4-152">Należy ręcznie skonfigurować je, aby usunąć niejednoznaczność.</span><span class="sxs-lookup"><span data-stu-id="432e4-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="432e4-153">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="432e4-153">Cascade delete</span></span>

<span data-ttu-id="432e4-154">Według Konwencji kaskadowe usuwanie zostanie ustawione na *kaskadowe* dla wymaganych relacji i *ClientSetNull* dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="432e4-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="432e4-155">*Kaskadowo* oznacza również usunięcie jednostek zależnych.</span><span class="sxs-lookup"><span data-stu-id="432e4-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="432e4-156">*ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci, pozostaną bez zmian i muszą zostać ręcznie usunięte lub zaktualizowane, aby wskazywały prawidłową jednostkę podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="432e4-157">W przypadku jednostek, które są ładowane do pamięci, EF Core podejmie próbę ustawienia właściwości klucza obcego na wartość null.</span><span class="sxs-lookup"><span data-stu-id="432e4-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="432e4-158">Zapoznaj się z sekcją [wymagane i opcjonalne relacje](#required-and-optional-relationships) , aby uzyskać różnicę między wymaganymi i opcjonalnymi relacjami.</span><span class="sxs-lookup"><span data-stu-id="432e4-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="432e4-159">Zobacz [Kaskada Delete](../saving/cascade-delete.md) , aby uzyskać więcej informacji na temat różnych zachowań usuwania i ustawień domyślnych używanych przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="432e4-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="432e4-160">Konfiguracja ręczna</span><span class="sxs-lookup"><span data-stu-id="432e4-160">Manual configuration</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="432e4-161">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="432e4-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="432e4-162">Aby skonfigurować relację w interfejsie API Fluent, należy rozpocząć od zidentyfikowania właściwości nawigacji, które tworzą relację.</span><span class="sxs-lookup"><span data-stu-id="432e4-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="432e4-163">`HasOne` lub `HasMany` identyfikuje właściwość nawigacji dla typu jednostki, w którym rozpoczyna się konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="432e4-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="432e4-164">Następnie utworzysz łańcuch wywołań do `WithOne` lub `WithMany`, aby zidentyfikować nawigację odwrotną.</span><span class="sxs-lookup"><span data-stu-id="432e4-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="432e4-165">`HasOne`/`WithOne` są używane do właściwości nawigacji referencyjnej i `HasMany`/`WithMany` są używane na potrzeby właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="432e4-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="432e4-166">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="432e4-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="432e4-167">Możesz użyć adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji dla jednostek zależnych i głównych.</span><span class="sxs-lookup"><span data-stu-id="432e4-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="432e4-168">Jest to zazwyczaj wykonywane, gdy istnieje więcej niż jedna para właściwości nawigacji między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="432e4-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

> [!NOTE]
> <span data-ttu-id="432e4-169">Możesz użyć [Required] właściwości w jednostce zależnej, aby mieć wpływ na wymaganą relację.</span><span class="sxs-lookup"><span data-stu-id="432e4-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="432e4-170">[Wymagane] w nawigacji z jednostki głównej jest zwykle ignorowany, ale może spowodować, że jednostka stanie się jednostką zależną.</span><span class="sxs-lookup"><span data-stu-id="432e4-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="432e4-171">Adnotacje danych `[ForeignKey]` i `[InverseProperty]` są dostępne w przestrzeni nazw `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="432e4-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="432e4-172">`[Required]` jest dostępny w przestrzeni nazw `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="432e4-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="432e4-173">Właściwość pojedynczej nawigacji</span><span class="sxs-lookup"><span data-stu-id="432e4-173">Single navigation property</span></span>

<span data-ttu-id="432e4-174">Jeśli masz tylko jedną właściwość nawigacji, istnieją bezparametryczne przeciążenia `WithOne` i `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="432e4-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="432e4-175">Oznacza to, że istnieje koncepcyjnie odwołanie lub kolekcja na drugim końcu relacji, ale nie ma żadnej właściwości nawigacji zawartej w klasie Entity.</span><span class="sxs-lookup"><span data-stu-id="432e4-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="432e4-176">Klucz obcy</span><span class="sxs-lookup"><span data-stu-id="432e4-176">Foreign key</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="432e4-177">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="432e4-177">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="432e4-178">Korzystając z interfejsu API Fluent, można skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="432e4-179">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="432e4-179">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="432e4-180">Możesz użyć adnotacji danych, aby skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-180">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="432e4-181">Jest to zazwyczaj wykonywane, gdy właściwość klucza obcego nie zostanie odnaleziona według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="432e4-181">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="432e4-182">Adnotacja `[ForeignKey]` może zostać umieszczona na każdej właściwości nawigacji w relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-182">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="432e4-183">Nie musi ona być zgodna z właściwością nawigacji w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="432e4-183">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="432e4-184">Właściwość określona przy użyciu `[ForeignKey]` na właściwości nawigacji nie musi istnieć typu zależnego.</span><span class="sxs-lookup"><span data-stu-id="432e4-184">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="432e4-185">W takim przypadku określona nazwa zostanie użyta do utworzenia klucza obcego cienia.</span><span class="sxs-lookup"><span data-stu-id="432e4-185">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

<span data-ttu-id="432e4-186">Poniższy kod przedstawia sposób konfigurowania złożonego klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-186">The following code shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="432e4-187">Możesz użyć przeciążenia ciągu `HasForeignKey(...)`, aby skonfigurować właściwość Shadow jako klucz obcy (Aby uzyskać więcej informacji, zobacz [Właściwości cienia](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="432e4-187">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="432e4-188">Zalecamy jawne dodanie właściwości Shadow do modelu przed użyciem go jako klucza obcego (jak pokazano poniżej).</span><span class="sxs-lookup"><span data-stu-id="432e4-188">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="432e4-189">Bez właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="432e4-189">Without navigation property</span></span>

<span data-ttu-id="432e4-190">Nie trzeba podawać właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-190">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="432e4-191">Możesz po prostu podać klucz obcy po jednej stronie relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-191">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="432e4-192">Klucz podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="432e4-192">Principal key</span></span>

<span data-ttu-id="432e4-193">Jeśli chcesz, aby klucz obcy odwołuje się do właściwości innej niż klucz podstawowy, możesz użyć interfejsu API Fluent, aby skonfigurować właściwość klucza podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-193">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="432e4-194">Właściwość, którą konfigurujesz jako klucz podmiotu zabezpieczeń, zostanie automatycznie skonfigurowana jako [klucz alternatywny](alternate-keys.md).</span><span class="sxs-lookup"><span data-stu-id="432e4-194">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="432e4-195">Poniższy kod przedstawia sposób konfigurowania złożonego klucza podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-195">The following code shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="432e4-196">Kolejność, w której określane są właściwości klucza podmiotu zabezpieczeń, musi być zgodna z kolejnością, w której są określone dla klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-196">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="432e4-197">Wymagane i opcjonalne relacje</span><span class="sxs-lookup"><span data-stu-id="432e4-197">Required and optional relationships</span></span>

<span data-ttu-id="432e4-198">Korzystając z interfejsu API Fluent, można określić, czy relacja jest wymagana, czy opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="432e4-198">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="432e4-199">Ostatecznie to kontroluje, czy właściwość klucza obcego jest wymagana czy opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="432e4-199">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="432e4-200">Jest to najbardziej przydatne w przypadku używania klucza obcego stanu w tle.</span><span class="sxs-lookup"><span data-stu-id="432e4-200">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="432e4-201">Jeśli masz właściwość klucza obcego w klasie jednostki, wymagana jest konieczność relacji w zależności od tego, czy właściwość klucza obcego jest wymagana, czy opcjonalna (zobacz [Właściwości wymagane i opcjonalne,](required-optional.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="432e4-201">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

> [!NOTE]
> <span data-ttu-id="432e4-202">Wywołanie `IsRequired(false)` powoduje również, że właściwość klucza obcego jest opcjonalna, chyba że została skonfigurowana inaczej.</span><span class="sxs-lookup"><span data-stu-id="432e4-202">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="432e4-203">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="432e4-203">Cascade delete</span></span>

<span data-ttu-id="432e4-204">Korzystając z interfejsu API Fluent, można jawnie skonfigurować zachowanie kaskadowego usuwania dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="432e4-204">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="432e4-205">Szczegółowe omówienie każdej z tych opcji można znaleźć w temacie [usuwanie kaskadowe](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="432e4-205">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="432e4-206">Inne wzorce relacji</span><span class="sxs-lookup"><span data-stu-id="432e4-206">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="432e4-207">Jeden do jednego</span><span class="sxs-lookup"><span data-stu-id="432e4-207">One-to-one</span></span>

<span data-ttu-id="432e4-208">Relacja jeden do jednego ma właściwość nawigacji odwołania po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="432e4-208">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="432e4-209">Są one zgodne z tymi samymi konwencjami co relacje jeden-do-wielu, ale na właściwości klucza obcego jest wprowadzany unikatowy indeks, aby upewnić się, że tylko jedna z nich jest zależna od każdego podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-209">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="432e4-210">Dr wybierze jedną z jednostek, która będzie zależna od możliwości wykrywania właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="432e4-210">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="432e4-211">W przypadku wybrania niewłaściwej jednostki jako zależnej można użyć interfejsu API Fluent, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="432e4-211">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="432e4-212">Podczas konfigurowania relacji z interfejsem API Fluent należy używać metod `HasOne` i `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="432e4-212">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="432e4-213">Podczas konfigurowania klucza obcego należy określić typ podmiotu zależnego — Zwróć uwagę na parametr ogólny podany do `HasForeignKey` na poniższej liście.</span><span class="sxs-lookup"><span data-stu-id="432e4-213">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="432e4-214">W relacji jeden-do-wielu jest jasne, że jednostka z nawigacją referencyjną jest zależna, a jeden z kolekcją jest podmiotem zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="432e4-214">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="432e4-215">Nie jest to jednak w relacji jeden-do-jednego — dlatego trzeba ją jawnie zdefiniować.</span><span class="sxs-lookup"><span data-stu-id="432e4-215">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="432e4-216">Wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="432e4-216">Many-to-many</span></span>

<span data-ttu-id="432e4-217">Relacje wiele-do-wielu bez klasy Entity do reprezentowania tabeli sprzężenia nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="432e4-217">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="432e4-218">Istnieje jednak możliwość reprezentowania relacji wiele-do-wielu poprzez dołączenie klasy jednostki dla tabeli sprzężenia i mapowanie dwóch oddzielnych relacji jeden-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="432e4-218">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
