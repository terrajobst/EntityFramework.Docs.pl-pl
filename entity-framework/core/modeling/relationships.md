---
title: Relacje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: a53a862cc2443a1c4461aa287def100284635f26
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994945"
---
# <a name="relationships"></a><span data-ttu-id="3bcea-102">Relacje</span><span class="sxs-lookup"><span data-stu-id="3bcea-102">Relationships</span></span>

<span data-ttu-id="3bcea-103">Relacja definiuje sposób dwie jednostki powiązane są ze sobą.</span><span class="sxs-lookup"><span data-stu-id="3bcea-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="3bcea-104">W relacyjnej bazie danych jest reprezentowane przez ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="3bcea-105">Większość przykładów w tym artykule umożliwiają zademonstrowania pojęć relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="3bcea-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="3bcea-106">Zobacz przykłady relacji jeden do jednego i wiele do wielu [inne wzorce relacji](#other-relationship-patterns) sekcji na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3bcea-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="3bcea-107">Definicje terminów</span><span class="sxs-lookup"><span data-stu-id="3bcea-107">Definition of Terms</span></span>

<span data-ttu-id="3bcea-108">Istnieje wiele terminy używane do opisywania relacji</span><span class="sxs-lookup"><span data-stu-id="3bcea-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="3bcea-109">**Jednostki zależne:** jest to obiekt, który zawiera właściwości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="3bcea-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="3bcea-110">Czasami określane jako "podrzędne" w relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="3bcea-111">**Jednostka główna:** to jednostka, która zawiera właściwości klucza podstawowego/alternatywny.</span><span class="sxs-lookup"><span data-stu-id="3bcea-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="3bcea-112">Czasami określane jako "parent" w relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="3bcea-113">**Klucz obcy:** właściwości w jednostce zależne, który jest używany do przechowywania wartości właściwości klucza jednostki powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="3bcea-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="3bcea-114">**Klucz jednostki:** właściwości, który unikatowo identyfikuje główną jednostki.</span><span class="sxs-lookup"><span data-stu-id="3bcea-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="3bcea-115">Może to być kluczem podstawowym lub unikatowym.</span><span class="sxs-lookup"><span data-stu-id="3bcea-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="3bcea-116">**Właściwość nawigacji:** właściwości zdefiniowane w jednostce podmiotu zabezpieczeń i/lub zależne, który zawiera odwołania do powiązanych entity(s).</span><span class="sxs-lookup"><span data-stu-id="3bcea-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="3bcea-117">**Właściwości nawigacji kolekcji:** właściwość nawigacji, który zawiera odwołania do wielu powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="3bcea-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="3bcea-118">**Odwoływać się do właściwości nawigacji:** właściwość nawigacji, która zawiera odwołanie do pojedynczego obiektu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="3bcea-119">**Właściwość nawigacji odwrotność:** Omawiając właściwości określonej nawigacji, określenie to odnosi się do właściwości nawigacji na końcu relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="3bcea-120">W poniższym fragmencie kodu przedstawiono relację jeden do wielu między `Blog` i `Post`</span><span class="sxs-lookup"><span data-stu-id="3bcea-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="3bcea-121">`Post` to jednostka zależna</span><span class="sxs-lookup"><span data-stu-id="3bcea-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="3bcea-122">`Blog` to jednostka główna</span><span class="sxs-lookup"><span data-stu-id="3bcea-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="3bcea-123">`Post.BlogId` jest to klucz obcy</span><span class="sxs-lookup"><span data-stu-id="3bcea-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="3bcea-124">`Blog.BlogId` to klucz jednostki (w tym przypadku jest kluczem podstawowym, a nie klucza alternatywnego)</span><span class="sxs-lookup"><span data-stu-id="3bcea-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="3bcea-125">`Post.Blog` jest to właściwość nawigacji odwołania</span><span class="sxs-lookup"><span data-stu-id="3bcea-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="3bcea-126">`Blog.Posts` jest to właściwość nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="3bcea-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="3bcea-127">`Post.Blog` jest właściwość nawigacji odwrotność `Blog.Posts` (i na odwrót)</span><span class="sxs-lookup"><span data-stu-id="3bcea-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="3bcea-128">Konwencje</span><span class="sxs-lookup"><span data-stu-id="3bcea-128">Conventions</span></span>

<span data-ttu-id="3bcea-129">Zgodnie z Konwencją będzie można utworzyć relacji, po wykryte na typ właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="3bcea-130">Właściwość jest traktowana jako właściwość nawigacji, jeśli nie można zamapować typu, który wskazuje jako typ skalarny przez bieżącego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3bcea-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="3bcea-131">Relacje, które zostały wykryte przez Konwencję zawsze będą ukierunkowane na klucz podstawowy jednostki głównej.</span><span class="sxs-lookup"><span data-stu-id="3bcea-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="3bcea-132">Pod kątem klucza alternatywnego, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3bcea-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="3bcea-133">W pełni zdefiniowanych relacji</span><span class="sxs-lookup"><span data-stu-id="3bcea-133">Fully Defined Relationships</span></span>

<span data-ttu-id="3bcea-134">Najczęstszym wzorcem dla relacji jest zdefiniowane na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależne właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="3bcea-135">Jeśli para właściwości nawigacji znajduje się między dwoma typami, następnie one zostaną skonfigurowane jako właściwości nawigacji odwrotność tej samej relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="3bcea-136">Jeśli jednostka zależna zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`, lub `<principal entity name><primary key property name>` , a następnie zostaną skonfigurowane jako klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="3bcea-137">Czy wiele właściwości nawigacji zdefiniowane między dwoma typami (czyli więcej niż jedną parę distinct źródłem, które wskazują na siebie nawzajem), następnie relacje nie zostanie utworzony zgodnie z Konwencją i trzeba będzie ręcznie skonfigurować je do identyfikowania sposób, w jaki pary właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="3bcea-138">Nie właściwości klucza obcego</span><span class="sxs-lookup"><span data-stu-id="3bcea-138">No Foreign Key Property</span></span>

<span data-ttu-id="3bcea-139">Mimo że zaleca się mieć właściwość klucza obcego zdefiniowanej w klasie jednostki zależne, nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3bcea-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="3bcea-140">Jeśli zostanie znalezione nie właściwość klucza obcego, zostaną wprowadzone właściwości klucza obcego w tle o nazwie `<navigation property name><principal key property name>` (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="3bcea-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="3bcea-141">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="3bcea-141">Single Navigation Property</span></span>

<span data-ttu-id="3bcea-142">Łącznie z tylko jedną właściwość nawigacji (nie odwrotność nawigacji i nie właściwości klucza obcego) jest wystarczający, aby mieć relacji zdefiniowanych przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="3bcea-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="3bcea-143">Można również mieć właściwości pojedynczej wartości i właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="3bcea-144">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="3bcea-144">Cascade Delete</span></span>

<span data-ttu-id="3bcea-145">Zgodnie z Konwencją, usuwanie kaskadowe zostanie ustawiony *Cascade* dla relacji wymagane i *ClientSetNull* dla relacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="3bcea-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="3bcea-146">*Kaskadowe* oznacza, że jednostki zależne również zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="3bcea-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="3bcea-147">*ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci pozostaną bez zmian i muszą być ręcznie usunięty lub poinformowani o prawidłową jednostkę główną.</span><span class="sxs-lookup"><span data-stu-id="3bcea-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="3bcea-148">W przypadku jednostek, które są ładowane do pamięci programu EF Core będzie podejmować próby równa właściwości klucza obcego o wartości null.</span><span class="sxs-lookup"><span data-stu-id="3bcea-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="3bcea-149">Zobacz [relacje wymaganych i opcjonalnych](#required-and-optional-relationships) sekcji różnicę między relacje wymaganych i opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="3bcea-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="3bcea-150">Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) więcej szczegółów na temat różnych Usuń zachowania i wartości domyślne używane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="3bcea-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3bcea-151">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3bcea-151">Data Annotations</span></span>

<span data-ttu-id="3bcea-152">Istnieją dwa adnotacji danych, które mogą być używane do konfigurowania relacji `[ForeignKey]` i `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="3bcea-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="3bcea-153">[Klucza obcego]</span><span class="sxs-lookup"><span data-stu-id="3bcea-153">[ForeignKey]</span></span>

<span data-ttu-id="3bcea-154">Korzystanie z adnotacji danych, aby skonfigurować, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="3bcea-155">Zazwyczaj jest to wykonywane, gdy właściwość klucza obcego nie został odnaleziony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="3bcea-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="3bcea-156">`[ForeignKey]` Adnotacji można umieścić we właściwości nawigacji, albo w relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="3bcea-157">Nie musi przejść na właściwość nawigacji w klasie jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="3bcea-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="3bcea-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="3bcea-158">[InverseProperty]</span></span>

<span data-ttu-id="3bcea-159">Korzystanie z adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji jednostki zależnej i głównej.</span><span class="sxs-lookup"><span data-stu-id="3bcea-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="3bcea-160">Zazwyczaj jest to wykonywane, gdy istnieje więcej niż jedną parę właściwości nawigacji między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="3bcea-161">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="3bcea-161">Fluent API</span></span>

<span data-ttu-id="3bcea-162">Aby skonfigurować relację w interfejsie API Fluent, możesz rozpocząć, określając właściwości nawigacji, które tworzą relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="3bcea-163">`HasOne` lub `HasMany` identyfikuje właściwość nawigacji typu jednostki, rozpoczynamy od konfiguracji na.</span><span class="sxs-lookup"><span data-stu-id="3bcea-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="3bcea-164">Można następnie połączyć w łańcuch po wywołaniu `WithOne` lub `WithMany` do identyfikowania odwrotność nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="3bcea-165">`HasOne`/`WithOne` są używane dla właściwości nawigacji odwołania i `HasMany` / `WithMany` są używane dla właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="3bcea-166">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="3bcea-166">Single Navigation Property</span></span>

<span data-ttu-id="3bcea-167">Jeśli masz tylko jedną właściwość nawigacji, a następnie istnieją przeciążenia bez parametrów `WithOne` i `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="3bcea-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="3bcea-168">Oznacza to, że ma pod względem koncepcyjnym odwołanie lub kolekcji na końcu relacji, ale nie ma właściwości nawigacji zawarta w klasie jednostki.</span><span class="sxs-lookup"><span data-stu-id="3bcea-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="3bcea-169">Klucz obcy</span><span class="sxs-lookup"><span data-stu-id="3bcea-169">Foreign Key</span></span>

<span data-ttu-id="3bcea-170">Interfejs Fluent API umożliwiają skonfigurowanie, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="3bcea-171">W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożonego klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="3bcea-172">Można użyć ciągu przeciążenia `HasForeignKey(...)` do skonfigurowania właściwości w tle jako klucz obcy (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="3bcea-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="3bcea-173">Zalecane jest jawne dodanie właściwości w tle do modelu, zanim użyjesz go jako klucza obcego (jak pokazano poniżej).</span><span class="sxs-lookup"><span data-stu-id="3bcea-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="3bcea-174">Klucz jednostki</span><span class="sxs-lookup"><span data-stu-id="3bcea-174">Principal Key</span></span>

<span data-ttu-id="3bcea-175">Jeśli chcesz, aby klucz obcy, aby odwoływać się do właściwości innego niż klucz podstawowy, można użyć interfejsu API Fluent do skonfigurowania właściwości klucza podmiotu zabezpieczeń dla relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="3bcea-176">Właściwość, którą można skonfigurować jako głównej będą kluczowe automatycznie należy skonfigurować jako klucza alternatywnego (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="3bcea-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="3bcea-177">W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożony klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="3bcea-177">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="3bcea-178">Kolejność, w której zostaną określone jednostki właściwości klucza musi być zgodna kolejność, w którym są określone w kluczu obcym.</span><span class="sxs-lookup"><span data-stu-id="3bcea-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="3bcea-179">Relacje wymagane i opcjonalne</span><span class="sxs-lookup"><span data-stu-id="3bcea-179">Required and Optional Relationships</span></span>

<span data-ttu-id="3bcea-180">Aby określić, czy relacja jest wymagane lub opcjonalne, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3bcea-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="3bcea-181">Ostatecznie to określa, czy właściwość klucza obcego jest wymagane lub opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="3bcea-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="3bcea-182">Jest to najbardziej przydatne, korzystając z kluczem obcym stanu w tle.</span><span class="sxs-lookup"><span data-stu-id="3bcea-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="3bcea-183">Jeśli masz właściwości klucza obcego w klasie jednostki, a następnie requiredness relacji jest określana na podstawie tego, czy właściwość klucza obcego jest wymagane lub opcjonalne (zobacz [wymagane i opcjonalne właściwości](required-optional.md) Aby uzyskać więcej informacji informacje o).</span><span class="sxs-lookup"><span data-stu-id="3bcea-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="3bcea-184">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="3bcea-184">Cascade Delete</span></span>

<span data-ttu-id="3bcea-185">Interfejs Fluent API umożliwiają skonfigurowanie jawnie zachowanie usuwanie kaskadowe określonej relacji.</span><span class="sxs-lookup"><span data-stu-id="3bcea-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="3bcea-186">Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) sekcji Zapisywanie danych szczegółowe omówienie każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="3bcea-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="3bcea-187">Inne wzorce relacji</span><span class="sxs-lookup"><span data-stu-id="3bcea-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="3bcea-188">Jeden do jednego</span><span class="sxs-lookup"><span data-stu-id="3bcea-188">One-to-one</span></span>

<span data-ttu-id="3bcea-189">Relacje jeden-do-jednego ma odwołanie do właściwości nawigacji po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="3bcea-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="3bcea-190">Używają tej samej Konwencji co relacji jeden do wielu, ale unikatowy indeks został wprowadzony na właściwość klucza obcego, aby upewnić się, że tylko jeden zależnych od powiązany jest każdy podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3bcea-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="3bcea-191">EF wybrać jeden z jednostki, które jest zależne od oparte na możliwość wykrywania właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3bcea-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="3bcea-192">Jeśli problem jednostki jest wybierany jako zależnych od ustawień lokalnych, można użyć Fluent API, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="3bcea-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="3bcea-193">Podczas konfigurowania relacja z interfejsem API Fluent, możesz użyć `HasOne` i `WithOne` metody.</span><span class="sxs-lookup"><span data-stu-id="3bcea-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="3bcea-194">Podczas konfigurowania klucza obcego, należy określić typ jednostki zależne - Zwróć uwagę, parametr generyczny udostępniane `HasForeignKey` na liście poniżej.</span><span class="sxs-lookup"><span data-stu-id="3bcea-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="3bcea-195">W przypadku relacji jeden do wielu jest jasne, czy jednostka z nawigacją odwołanie jest zależnych od ustawień lokalnych, jak i z kolekcji jest podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3bcea-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="3bcea-196">Ale to nie jest tak w przypadku relacji jeden do jednego — dlatego trzeba jawnie zdefiniować go.</span><span class="sxs-lookup"><span data-stu-id="3bcea-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="3bcea-197">Wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="3bcea-197">Many-to-many</span></span>

<span data-ttu-id="3bcea-198">Relacje wiele do wielu, bez klasę jednostki, do reprezentowania tabelę sprzężenia nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3bcea-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="3bcea-199">Jednak może reprezentować relacji wiele do wielu, umieszczając klasę jednostki dla tabeli sprzężenia i dwie oddzielne relacje jeden do wielu mapowania.</span><span class="sxs-lookup"><span data-stu-id="3bcea-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
