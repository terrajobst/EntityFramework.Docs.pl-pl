---
title: Relacje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929940"
---
# <a name="relationships"></a><span data-ttu-id="ba218-102">Relacje</span><span class="sxs-lookup"><span data-stu-id="ba218-102">Relationships</span></span>

<span data-ttu-id="ba218-103">Relacja definiuje sposób dwie jednostki powiązane są ze sobą.</span><span class="sxs-lookup"><span data-stu-id="ba218-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="ba218-104">W relacyjnej bazie danych jest reprezentowane przez ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ba218-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="ba218-105">Większość przykładów w tym artykule umożliwiają zademonstrowania pojęć relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="ba218-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="ba218-106">Zobacz przykłady relacji jeden do jednego i wiele do wielu [inne wzorce relacji](#other-relationship-patterns) sekcji na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="ba218-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="ba218-107">Definicje terminów</span><span class="sxs-lookup"><span data-stu-id="ba218-107">Definition of Terms</span></span>

<span data-ttu-id="ba218-108">Istnieje wiele terminy używane do opisywania relacji</span><span class="sxs-lookup"><span data-stu-id="ba218-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="ba218-109">**Jednostki zależne:** To jest jednostka, która zawiera właściwości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="ba218-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="ba218-110">Czasami określane jako "podrzędne" w relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="ba218-111">**Jednostka główna:** To jest jednostka, która zawiera właściwości klucza podstawowego/alternatywny.</span><span class="sxs-lookup"><span data-stu-id="ba218-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="ba218-112">Czasami określane jako "parent" w relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="ba218-113">**Klucz obcy:** Właściwości w jednostce zależne, który jest używany do przechowywania wartości właściwości klucza jednostki powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="ba218-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="ba218-114">**Klucz jednostki:** Właściwość, która jednoznacznie identyfikuje główną jednostki.</span><span class="sxs-lookup"><span data-stu-id="ba218-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="ba218-115">Może to być kluczem podstawowym lub unikatowym.</span><span class="sxs-lookup"><span data-stu-id="ba218-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="ba218-116">**Właściwość nawigacji:** Właściwości zdefiniowane w jednostce podmiotu zabezpieczeń i/lub zależne, który zawiera odwołania do powiązanych entity(s).</span><span class="sxs-lookup"><span data-stu-id="ba218-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="ba218-117">**Właściwości nawigacji kolekcji:** Właściwość nawigacji, który zawiera odwołania do wielu powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ba218-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="ba218-118">**Odwołanie do właściwości nawigacji:** Właściwość nawigacji, która zawiera odwołanie do pojedynczego obiektu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="ba218-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="ba218-119">**Właściwość nawigacji odwrotność:** Omawiając właściwości określonej nawigacji, określenie to odnosi się do właściwości nawigacji na końcu relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="ba218-120">W poniższym fragmencie kodu przedstawiono relację jeden do wielu między `Blog` i `Post`</span><span class="sxs-lookup"><span data-stu-id="ba218-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="ba218-121">`Post` to jednostka zależna</span><span class="sxs-lookup"><span data-stu-id="ba218-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="ba218-122">`Blog` to jednostka główna</span><span class="sxs-lookup"><span data-stu-id="ba218-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="ba218-123">`Post.BlogId` jest to klucz obcy</span><span class="sxs-lookup"><span data-stu-id="ba218-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="ba218-124">`Blog.BlogId` to klucz jednostki (w tym przypadku jest kluczem podstawowym, a nie klucza alternatywnego)</span><span class="sxs-lookup"><span data-stu-id="ba218-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="ba218-125">`Post.Blog` jest to właściwość nawigacji odwołania</span><span class="sxs-lookup"><span data-stu-id="ba218-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="ba218-126">`Blog.Posts` jest to właściwość nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="ba218-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="ba218-127">`Post.Blog` jest właściwość nawigacji odwrotność `Blog.Posts` (i na odwrót)</span><span class="sxs-lookup"><span data-stu-id="ba218-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="ba218-128">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ba218-128">Conventions</span></span>

<span data-ttu-id="ba218-129">Zgodnie z Konwencją będzie można utworzyć relacji, po wykryte na typ właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="ba218-130">Właściwość jest traktowana jako właściwość nawigacji, jeśli nie można zamapować typu, który wskazuje jako typ skalarny przez bieżącego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba218-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="ba218-131">Relacje, które zostały wykryte przez Konwencję zawsze będą ukierunkowane na klucz podstawowy jednostki głównej.</span><span class="sxs-lookup"><span data-stu-id="ba218-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="ba218-132">Pod kątem klucza alternatywnego, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ba218-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="ba218-133">W pełni zdefiniowanych relacji</span><span class="sxs-lookup"><span data-stu-id="ba218-133">Fully Defined Relationships</span></span>

<span data-ttu-id="ba218-134">Najczęstszym wzorcem dla relacji jest zdefiniowane na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależne właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="ba218-135">Jeśli para właściwości nawigacji znajduje się między dwoma typami, następnie one zostaną skonfigurowane jako właściwości nawigacji odwrotność tej samej relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="ba218-136">Jeśli jednostka zależna zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`, lub `<principal entity name><primary key property name>` , a następnie zostaną skonfigurowane jako klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ba218-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="ba218-137">Czy wiele właściwości nawigacji zdefiniowane między dwoma typami (czyli więcej niż jedną parę distinct źródłem, które wskazują na siebie nawzajem), następnie relacje nie zostanie utworzony zgodnie z Konwencją i trzeba będzie ręcznie skonfigurować je do identyfikowania sposób, w jaki pary właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="ba218-138">Nie właściwości klucza obcego</span><span class="sxs-lookup"><span data-stu-id="ba218-138">No Foreign Key Property</span></span>

<span data-ttu-id="ba218-139">Mimo że zaleca się mieć właściwość klucza obcego zdefiniowanej w klasie jednostki zależne, nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ba218-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="ba218-140">Jeśli zostanie znalezione nie właściwość klucza obcego, zostaną wprowadzone właściwości klucza obcego w tle o nazwie `<navigation property name><principal key property name>` (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="ba218-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="ba218-141">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="ba218-141">Single Navigation Property</span></span>

<span data-ttu-id="ba218-142">Łącznie z tylko jedną właściwość nawigacji (nie odwrotność nawigacji i nie właściwości klucza obcego) jest wystarczający, aby mieć relacji zdefiniowanych przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ba218-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="ba218-143">Można również mieć właściwości pojedynczej wartości i właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ba218-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="ba218-144">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="ba218-144">Cascade Delete</span></span>

<span data-ttu-id="ba218-145">Zgodnie z Konwencją, usuwanie kaskadowe zostanie ustawiony *Cascade* dla relacji wymagane i *ClientSetNull* dla relacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="ba218-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="ba218-146">*Kaskadowe* oznacza, że jednostki zależne również zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="ba218-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="ba218-147">*ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci pozostaną bez zmian i muszą być ręcznie usunięty lub poinformowani o prawidłową jednostkę główną.</span><span class="sxs-lookup"><span data-stu-id="ba218-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="ba218-148">W przypadku jednostek, które są ładowane do pamięci programu EF Core będzie podejmować próby równa właściwości klucza obcego o wartości null.</span><span class="sxs-lookup"><span data-stu-id="ba218-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="ba218-149">Zobacz [relacje wymaganych i opcjonalnych](#required-and-optional-relationships) sekcji różnicę między relacje wymaganych i opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="ba218-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="ba218-150">Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) więcej szczegółów na temat różnych Usuń zachowania i wartości domyślne używane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ba218-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ba218-151">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="ba218-151">Data Annotations</span></span>

<span data-ttu-id="ba218-152">Istnieją dwa adnotacji danych, które mogą być używane do konfigurowania relacji `[ForeignKey]` i `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="ba218-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="ba218-153">Są one dostępne w `System.ComponentModel.DataAnnotations.Schema` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ba218-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="ba218-154">[Klucza obcego]</span><span class="sxs-lookup"><span data-stu-id="ba218-154">[ForeignKey]</span></span>

<span data-ttu-id="ba218-155">Korzystanie z adnotacji danych, aby skonfigurować, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="ba218-156">Zazwyczaj jest to wykonywane, gdy właściwość klucza obcego nie został odnaleziony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ba218-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="ba218-157">`[ForeignKey]` Adnotacji można umieścić we właściwości nawigacji, albo w relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="ba218-158">Nie musi przejść na właściwość nawigacji w klasie jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="ba218-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="ba218-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="ba218-159">[InverseProperty]</span></span>

<span data-ttu-id="ba218-160">Korzystanie z adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji jednostki zależnej i głównej.</span><span class="sxs-lookup"><span data-stu-id="ba218-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="ba218-161">Zazwyczaj jest to wykonywane, gdy istnieje więcej niż jedną parę właściwości nawigacji między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="ba218-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="ba218-162">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="ba218-162">Fluent API</span></span>

<span data-ttu-id="ba218-163">Aby skonfigurować relację w interfejsie API Fluent, możesz rozpocząć, określając właściwości nawigacji, które tworzą relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="ba218-164">`HasOne` lub `HasMany` identyfikuje właściwość nawigacji typu jednostki, rozpoczynamy od konfiguracji na.</span><span class="sxs-lookup"><span data-stu-id="ba218-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="ba218-165">Można następnie połączyć w łańcuch po wywołaniu `WithOne` lub `WithMany` do identyfikowania odwrotność nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="ba218-166">`HasOne`/`WithOne` są używane dla właściwości nawigacji odwołania i `HasMany` / `WithMany` są używane dla właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ba218-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="ba218-167">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="ba218-167">Single Navigation Property</span></span>

<span data-ttu-id="ba218-168">Jeśli masz tylko jedną właściwość nawigacji, a następnie istnieją przeciążenia bez parametrów `WithOne` i `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="ba218-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="ba218-169">Oznacza to, że ma pod względem koncepcyjnym odwołanie lub kolekcji na końcu relacji, ale nie ma właściwości nawigacji zawarta w klasie jednostki.</span><span class="sxs-lookup"><span data-stu-id="ba218-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="ba218-170">Klucz obcy</span><span class="sxs-lookup"><span data-stu-id="ba218-170">Foreign Key</span></span>

<span data-ttu-id="ba218-171">Interfejs Fluent API umożliwiają skonfigurowanie, których właściwość powinna być używana jako właściwość klucza obcego dla danej relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="ba218-172">W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożonego klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ba218-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="ba218-173">Można użyć ciągu przeciążenia `HasForeignKey(...)` do skonfigurowania właściwości w tle jako klucz obcy (zobacz [właściwości w tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="ba218-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="ba218-174">Zalecane jest jawne dodanie właściwości w tle do modelu, zanim użyjesz go jako klucza obcego (jak pokazano poniżej).</span><span class="sxs-lookup"><span data-stu-id="ba218-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="ba218-175">Klucz jednostki</span><span class="sxs-lookup"><span data-stu-id="ba218-175">Principal Key</span></span>

<span data-ttu-id="ba218-176">Jeśli chcesz, aby klucz obcy, aby odwoływać się do właściwości innego niż klucz podstawowy, można użyć interfejsu API Fluent do skonfigurowania właściwości klucza podmiotu zabezpieczeń dla relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-176">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="ba218-177">Właściwość, którą można skonfigurować jako głównej będą kluczowe automatycznie należy skonfigurować jako klucza alternatywnego (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="ba218-177">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="ba218-178">W poniższym fragmencie kodu przedstawiono sposób konfigurowania złożony klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="ba218-178">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="ba218-179">Kolejność, w której zostaną określone jednostki właściwości klucza musi być zgodna kolejność, w którym są określone w kluczu obcym.</span><span class="sxs-lookup"><span data-stu-id="ba218-179">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="ba218-180">Relacje wymagane i opcjonalne</span><span class="sxs-lookup"><span data-stu-id="ba218-180">Required and Optional Relationships</span></span>

<span data-ttu-id="ba218-181">Aby określić, czy relacja jest wymagane lub opcjonalne, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ba218-181">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="ba218-182">Ostatecznie to określa, czy właściwość klucza obcego jest wymagane lub opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="ba218-182">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="ba218-183">Jest to najbardziej przydatne, korzystając z kluczem obcym stanu w tle.</span><span class="sxs-lookup"><span data-stu-id="ba218-183">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="ba218-184">Jeśli masz właściwości klucza obcego w klasie jednostki, a następnie requiredness relacji jest określana na podstawie tego, czy właściwość klucza obcego jest wymagane lub opcjonalne (zobacz [wymagane i opcjonalne właściwości](required-optional.md) Aby uzyskać więcej informacji informacje o).</span><span class="sxs-lookup"><span data-stu-id="ba218-184">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="ba218-185">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="ba218-185">Cascade Delete</span></span>

<span data-ttu-id="ba218-186">Interfejs Fluent API umożliwiają skonfigurowanie jawnie zachowanie usuwanie kaskadowe określonej relacji.</span><span class="sxs-lookup"><span data-stu-id="ba218-186">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="ba218-187">Zobacz [usuwanie kaskadowe](../saving/cascade-delete.md) sekcji Zapisywanie danych szczegółowe omówienie każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="ba218-187">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="ba218-188">Inne wzorce relacji</span><span class="sxs-lookup"><span data-stu-id="ba218-188">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="ba218-189">Jeden do jednego</span><span class="sxs-lookup"><span data-stu-id="ba218-189">One-to-one</span></span>

<span data-ttu-id="ba218-190">Relacje jeden-do-jednego ma odwołanie do właściwości nawigacji po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="ba218-190">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="ba218-191">Używają tej samej Konwencji co relacji jeden do wielu, ale unikatowy indeks został wprowadzony na właściwość klucza obcego, aby upewnić się, że tylko jeden zależnych od powiązany jest każdy podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="ba218-191">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="ba218-192">EF wybrać jeden z jednostki, które jest zależne od oparte na możliwość wykrywania właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ba218-192">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="ba218-193">Jeśli problem jednostki jest wybierany jako zależnych od ustawień lokalnych, można użyć Fluent API, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="ba218-193">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="ba218-194">Podczas konfigurowania relacja z interfejsem API Fluent, możesz użyć `HasOne` i `WithOne` metody.</span><span class="sxs-lookup"><span data-stu-id="ba218-194">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="ba218-195">Podczas konfigurowania klucza obcego, należy określić typ jednostki zależne - Zwróć uwagę, parametr generyczny udostępniane `HasForeignKey` na liście poniżej.</span><span class="sxs-lookup"><span data-stu-id="ba218-195">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="ba218-196">W przypadku relacji jeden do wielu jest jasne, czy jednostka z nawigacją odwołanie jest zależnych od ustawień lokalnych, jak i z kolekcji jest podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="ba218-196">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="ba218-197">Ale to nie jest tak w przypadku relacji jeden do jednego — dlatego trzeba jawnie zdefiniować go.</span><span class="sxs-lookup"><span data-stu-id="ba218-197">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="ba218-198">Wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="ba218-198">Many-to-many</span></span>

<span data-ttu-id="ba218-199">Relacje wiele do wielu, bez klasę jednostki, do reprezentowania tabelę sprzężenia nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ba218-199">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="ba218-200">Jednak może reprezentować relacji wiele do wielu, umieszczając klasę jednostki dla tabeli sprzężenia i dwie oddzielne relacje jeden do wielu mapowania.</span><span class="sxs-lookup"><span data-stu-id="ba218-200">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
