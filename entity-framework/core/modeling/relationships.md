---
title: Relacje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655675"
---
# <a name="relationships"></a><span data-ttu-id="6a1c9-102">Relacje</span><span class="sxs-lookup"><span data-stu-id="6a1c9-102">Relationships</span></span>

<span data-ttu-id="6a1c9-103">Relacja określa, jak dwie jednostki są powiązane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="6a1c9-104">W relacyjnej bazie danych jest to reprezentowane przez ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="6a1c9-105">Większość przykładów w tym artykule używa relacji jeden-do-wielu, aby przedstawić koncepcje.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="6a1c9-106">Przykłady relacji jeden-do-jednego i wiele-do-wielu można znaleźć w sekcji [inne wzorce relacji](#other-relationship-patterns) na końcu artykułu.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="6a1c9-107">Definicja warunków</span><span class="sxs-lookup"><span data-stu-id="6a1c9-107">Definition of Terms</span></span>

<span data-ttu-id="6a1c9-108">Istnieje kilka terminów używanych do opisywania relacji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="6a1c9-109">**Jednostka zależna:** Jest to jednostka, która zawiera właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="6a1c9-110">Czasami określana jako "podrzędny" relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="6a1c9-111">**Podmiot podmiotu zabezpieczeń:** Jest to jednostka, która zawiera właściwości klucza podstawowego/alternatywnego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="6a1c9-112">Czasami określana jako "Parent" relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="6a1c9-113">**Klucz obcy:** Właściwości w jednostce zależnej, która jest używana do przechowywania wartości właściwości klucza podmiotu, z którą jest powiązana jednostka.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="6a1c9-114">**Klucz podmiotu zabezpieczeń:** Właściwości, które jednoznacznie identyfikują jednostkę podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="6a1c9-115">Może to być klucz podstawowy lub klucz alternatywny.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="6a1c9-116">**Właściwość nawigacji:** Właściwość zdefiniowana w jednostkach głównych i/lub zależnych, która zawiera odwołania do powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="6a1c9-117">**Właściwość nawigacji kolekcji:** Właściwość nawigacji, która zawiera odwołania do wielu powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="6a1c9-118">**Właściwość nawigacji odwołania:** Właściwość nawigacji, która zawiera odwołanie do pojedynczej powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="6a1c9-119">**Właściwość nawigacji odwrotnej:** Omawiając konkretną właściwość nawigacji, ten termin odnosi się do właściwości nawigacji na drugim końcu relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="6a1c9-120">Poniższa lista kodu przedstawia relację jeden do wielu między `Blog` i `Post`</span><span class="sxs-lookup"><span data-stu-id="6a1c9-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="6a1c9-121">`Post` jest jednostką zależną</span><span class="sxs-lookup"><span data-stu-id="6a1c9-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="6a1c9-122">`Blog` jest jednostką główną</span><span class="sxs-lookup"><span data-stu-id="6a1c9-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="6a1c9-123">`Post.BlogId` jest kluczem obcym</span><span class="sxs-lookup"><span data-stu-id="6a1c9-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="6a1c9-124">`Blog.BlogId` jest kluczem podmiotu zabezpieczeń (w tym przypadku jest to klucz podstawowy, a nie klucz alternatywny)</span><span class="sxs-lookup"><span data-stu-id="6a1c9-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="6a1c9-125">`Post.Blog` jest właściwością nawigacji referencyjnej</span><span class="sxs-lookup"><span data-stu-id="6a1c9-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="6a1c9-126">`Blog.Posts` jest właściwością nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="6a1c9-127">`Post.Blog` jest właściwością nawigacji odwrotnej `Blog.Posts` (i odwrotnie)</span><span class="sxs-lookup"><span data-stu-id="6a1c9-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="6a1c9-128">Konwencje</span><span class="sxs-lookup"><span data-stu-id="6a1c9-128">Conventions</span></span>

<span data-ttu-id="6a1c9-129">Zgodnie z Konwencją zostanie utworzona relacja, gdy w typie zostanie wykryta właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="6a1c9-130">Właściwość jest uznawana za właściwość nawigacji, jeśli typ wskazujący nie może być mapowany jako typ skalarny przez bieżącego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="6a1c9-131">Relacje, które są odnajdywane według Konwencji, będą zawsze ukierunkowane na klucz podstawowy jednostki głównej.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="6a1c9-132">Aby określić klucz alternatywny, należy przeprowadzić dodatkową konfigurację przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="6a1c9-133">W pełni zdefiniowane relacje</span><span class="sxs-lookup"><span data-stu-id="6a1c9-133">Fully Defined Relationships</span></span>

<span data-ttu-id="6a1c9-134">Najbardziej typowym wzorcem relacji jest posiadanie właściwości nawigacji zdefiniowanych na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="6a1c9-135">Jeśli para właściwości nawigacji zostanie znaleziona między dwoma typami, zostaną one skonfigurowane jako właściwości nawigacji odwrotnej tej samej relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="6a1c9-136">Jeśli jednostka zależna zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`lub `<principal entity name><primary key property name>`, zostanie ona skonfigurowana jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="6a1c9-137">Jeśli istnieje wiele właściwości nawigacji zdefiniowanych między dwoma typami (to jest, więcej niż jedna para elementów nawigacyjnych, które wskazują na siebie), nie zostaną utworzone żadne relacje w ramach Konwencji i trzeba będzie ręcznie skonfigurować je w celu określenia, jak pary właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="6a1c9-138">Brak właściwości klucza obcego</span><span class="sxs-lookup"><span data-stu-id="6a1c9-138">No Foreign Key Property</span></span>

<span data-ttu-id="6a1c9-139">Chociaż zaleca się zdefiniowanie właściwości klucza obcego w klasie jednostki zależnej, nie jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="6a1c9-140">Jeśli właściwość klucza obcego nie zostanie znaleziona, właściwość klucza obcego cienia zostanie wprowadzona z nazwą `<navigation property name><principal key property name>` (zobacz [Właściwości cienia](shadow-properties.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="6a1c9-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="6a1c9-141">Właściwość pojedynczej nawigacji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-141">Single Navigation Property</span></span>

<span data-ttu-id="6a1c9-142">Dołączenie tylko jednej właściwości nawigacji (bez nawigacji odwrotnej i brak właściwości klucza obcego) nie wystarcza do zdefiniowania relacji przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="6a1c9-143">Można również mieć pojedynczą właściwość nawigacji i właściwość klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="6a1c9-144">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="6a1c9-144">Cascade Delete</span></span>

<span data-ttu-id="6a1c9-145">Według Konwencji kaskadowe usuwanie zostanie ustawione na *kaskadowe* dla wymaganych relacji i *ClientSetNull* dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="6a1c9-146">*Kaskadowo* oznacza również usunięcie jednostek zależnych.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="6a1c9-147">*ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci, pozostaną bez zmian i muszą zostać ręcznie usunięte lub zaktualizowane, aby wskazywały prawidłową jednostkę podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="6a1c9-148">W przypadku jednostek, które są ładowane do pamięci, EF Core podejmie próbę ustawienia właściwości klucza obcego na wartość null.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="6a1c9-149">Zapoznaj się z sekcją [wymagane i opcjonalne relacje](#required-and-optional-relationships) , aby uzyskać różnicę między wymaganymi i opcjonalnymi relacjami.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="6a1c9-150">Zobacz [Kaskada Delete](../saving/cascade-delete.md) , aby uzyskać więcej informacji na temat różnych zachowań usuwania i ustawień domyślnych używanych przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6a1c9-151">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="6a1c9-151">Data Annotations</span></span>

<span data-ttu-id="6a1c9-152">Istnieją dwie adnotacje dotyczące danych, których można użyć do skonfigurowania relacji, `[ForeignKey]` i `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="6a1c9-153">Są one dostępne w przestrzeni nazw `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="6a1c9-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="6a1c9-154">[ForeignKey]</span></span>

<span data-ttu-id="6a1c9-155">Możesz użyć adnotacji danych, aby skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="6a1c9-156">Jest to zazwyczaj wykonywane, gdy właściwość klucza obcego nie zostanie odnaleziona według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="6a1c9-157">Adnotacja `[ForeignKey]` może zostać umieszczona na każdej właściwości nawigacji w relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="6a1c9-158">Nie musi ona być zgodna z właściwością nawigacji w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="6a1c9-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="6a1c9-159">[InverseProperty]</span></span>

<span data-ttu-id="6a1c9-160">Możesz użyć adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji dla jednostek zależnych i głównych.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="6a1c9-161">Jest to zazwyczaj wykonywane, gdy istnieje więcej niż jedna para właściwości nawigacji między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="6a1c9-162">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="6a1c9-162">Fluent API</span></span>

<span data-ttu-id="6a1c9-163">Aby skonfigurować relację w interfejsie API Fluent, należy rozpocząć od zidentyfikowania właściwości nawigacji, które tworzą relację.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="6a1c9-164">`HasOne` lub `HasMany` identyfikuje właściwość nawigacji dla typu jednostki, w którym rozpoczyna się konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="6a1c9-165">Następnie utworzysz łańcuch wywołań do `WithOne` lub `WithMany`, aby zidentyfikować nawigację odwrotną.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="6a1c9-166">`HasOne`/`WithOne` są używane do właściwości nawigacji referencyjnej i `HasMany`/`WithMany` są używane na potrzeby właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="6a1c9-167">Właściwość pojedynczej nawigacji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-167">Single Navigation Property</span></span>

<span data-ttu-id="6a1c9-168">Jeśli masz tylko jedną właściwość nawigacji, istnieją bezparametryczne przeciążenia `WithOne` i `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="6a1c9-169">Oznacza to, że istnieje koncepcyjnie odwołanie lub kolekcja na drugim końcu relacji, ale nie ma żadnej właściwości nawigacji zawartej w klasie Entity.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="6a1c9-170">Klucz obcy</span><span class="sxs-lookup"><span data-stu-id="6a1c9-170">Foreign Key</span></span>

<span data-ttu-id="6a1c9-171">Korzystając z interfejsu API Fluent, można skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="6a1c9-172">Poniższy list kodu przedstawia sposób konfigurowania złożonego klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="6a1c9-173">Możesz użyć przeciążenia ciągu `HasForeignKey(...)`, aby skonfigurować właściwość Shadow jako klucz obcy (Aby uzyskać więcej informacji, zobacz [Właściwości cienia](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="6a1c9-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="6a1c9-174">Zalecamy jawne dodanie właściwości Shadow do modelu przed użyciem go jako klucza obcego (jak pokazano poniżej).</span><span class="sxs-lookup"><span data-stu-id="6a1c9-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="6a1c9-175">Bez właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-175">Without Navigation Property</span></span>

<span data-ttu-id="6a1c9-176">Nie trzeba podawać właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="6a1c9-177">Możesz po prostu podać klucz obcy po jednej stronie relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="6a1c9-178">Klucz podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="6a1c9-178">Principal Key</span></span>

<span data-ttu-id="6a1c9-179">Jeśli chcesz, aby klucz obcy odwołuje się do właściwości innej niż klucz podstawowy, możesz użyć interfejsu API Fluent, aby skonfigurować właściwość klucza podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="6a1c9-180">Właściwość, którą konfigurujesz jako klucz podmiotu zabezpieczeń, zostanie automatycznie skonfigurowana jako klucz alternatywny (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="6a1c9-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="6a1c9-181">Poniższy list kodu przedstawia sposób konfigurowania złożonego klucza podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-181">The following code listing shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="6a1c9-182">Kolejność, w której określane są właściwości klucza podmiotu zabezpieczeń, musi być zgodna z kolejnością, w której są określone dla klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="6a1c9-183">Wymagane i opcjonalne relacje</span><span class="sxs-lookup"><span data-stu-id="6a1c9-183">Required and Optional Relationships</span></span>

<span data-ttu-id="6a1c9-184">Korzystając z interfejsu API Fluent, można określić, czy relacja jest wymagana, czy opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="6a1c9-185">Ostatecznie to kontroluje, czy właściwość klucza obcego jest wymagana czy opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="6a1c9-186">Jest to najbardziej przydatne w przypadku używania klucza obcego stanu w tle.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="6a1c9-187">Jeśli masz właściwość klucza obcego w klasie jednostki, wymagana jest konieczność relacji w zależności od tego, czy właściwość klucza obcego jest wymagana, czy opcjonalna (zobacz [Właściwości wymagane i opcjonalne,](required-optional.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="6a1c9-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a><span data-ttu-id="6a1c9-188">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="6a1c9-188">Cascade Delete</span></span>

<span data-ttu-id="6a1c9-189">Korzystając z interfejsu API Fluent, można jawnie skonfigurować zachowanie kaskadowego usuwania dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="6a1c9-190">Szczegółowe omówienie każdej z tych opcji można znaleźć w sekcji zapisywanie [kaskadowe](../saving/cascade-delete.md) dla zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="6a1c9-191">Inne wzorce relacji</span><span class="sxs-lookup"><span data-stu-id="6a1c9-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="6a1c9-192">Jeden do jednego</span><span class="sxs-lookup"><span data-stu-id="6a1c9-192">One-to-one</span></span>

<span data-ttu-id="6a1c9-193">Relacja jeden do jednego ma właściwość nawigacji odwołania po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="6a1c9-194">Są one zgodne z tymi samymi konwencjami co relacje jeden-do-wielu, ale na właściwości klucza obcego jest wprowadzany unikatowy indeks, aby upewnić się, że tylko jedna z nich jest zależna od każdego podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="6a1c9-195">Dr wybierze jedną z jednostek, która będzie zależna od możliwości wykrywania właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="6a1c9-196">W przypadku wybrania niewłaściwej jednostki jako zależnej można użyć interfejsu API Fluent, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="6a1c9-197">Podczas konfigurowania relacji z interfejsem API Fluent należy używać metod `HasOne` i `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="6a1c9-198">Podczas konfigurowania klucza obcego należy określić typ podmiotu zależnego — Zwróć uwagę na parametr ogólny podany do `HasForeignKey` na poniższej liście.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="6a1c9-199">W relacji jeden-do-wielu jest jasne, że jednostka z nawigacją referencyjną jest zależna, a jeden z kolekcją jest podmiotem zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="6a1c9-200">Nie jest to jednak w relacji jeden-do-jednego — dlatego trzeba ją jawnie zdefiniować.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="6a1c9-201">Wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="6a1c9-201">Many-to-many</span></span>

<span data-ttu-id="6a1c9-202">Relacje wiele-do-wielu bez klasy Entity do reprezentowania tabeli sprzężenia nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="6a1c9-203">Istnieje jednak możliwość reprezentowania relacji wiele-do-wielu poprzez dołączenie klasy jednostki dla tabeli sprzężenia i mapowanie dwóch oddzielnych relacji jeden-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="6a1c9-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
