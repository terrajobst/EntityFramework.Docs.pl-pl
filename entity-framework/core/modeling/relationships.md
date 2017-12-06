---
title: "Relacje — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a><span data-ttu-id="b5f13-102">Relacje</span><span class="sxs-lookup"><span data-stu-id="b5f13-102">Relationships</span></span>

<span data-ttu-id="b5f13-103">Relacja definiuje sposób dwie jednostki odnoszą się do siebie.</span><span class="sxs-lookup"><span data-stu-id="b5f13-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="b5f13-104">W relacyjnej bazie danych to jest reprezentowany przez ograniczenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="b5f13-105">Większość przykładów w tym artykule umożliwiają pokazują pojęcia relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="b5f13-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="b5f13-106">Przykłady relacje jeden do jednego oraz wiele do wielu można znaleźć [inne wzorce relacji](#other-relationship-patterns) sekcji na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="b5f13-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="b5f13-107">Definicje terminów</span><span class="sxs-lookup"><span data-stu-id="b5f13-107">Definition of Terms</span></span>

<span data-ttu-id="b5f13-108">Liczba terminów używanych do opisywania relacji</span><span class="sxs-lookup"><span data-stu-id="b5f13-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="b5f13-109">**Jednostki zależnej:** to jednostka, która zawiera właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="b5f13-110">Czasami określane jako 'child' relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="b5f13-111">**Jednostka główna:** to jednostka, która zawiera właściwości klucza podstawowego/alternatywny.</span><span class="sxs-lookup"><span data-stu-id="b5f13-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="b5f13-112">Czasami określane jako "parent" w relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="b5f13-113">**Klucz obcy:** właściwości w jednostki zależnej, który jest używany do przechowywania wartości właściwości klucza głównej powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="b5f13-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="b5f13-114">**Klucz główny:** właściwości, który unikatowo identyfikuje jednostki podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b5f13-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="b5f13-115">Może to być kluczem podstawowym lub unikatowym.</span><span class="sxs-lookup"><span data-stu-id="b5f13-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="b5f13-116">**Właściwość nawigacji:** właściwość zdefiniowaną w głównych i/lub zależnych jednostki, która zawiera odwołania do powiązanych entity(s).</span><span class="sxs-lookup"><span data-stu-id="b5f13-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="b5f13-117">**Właściwości nawigacji kolekcji:** właściwość nawigacji, który zawiera odwołania do wielu powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="b5f13-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="b5f13-118">**Właściwości nawigacji odwołania:** właściwość nawigacji, który zawiera odwołanie do jednego obiektu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="b5f13-119">**Odwrócona właściwość nawigacji:** Omawiając właściwości nawigacji w szczególności, określenie to odnosi się do właściwości nawigacji na drugim końcu relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="b5f13-120">Następujący przykładowy kod przedstawia relacji jeden do wielu między `Blog` i`Post`</span><span class="sxs-lookup"><span data-stu-id="b5f13-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="b5f13-121">`Post`jest jednostki zależnej</span><span class="sxs-lookup"><span data-stu-id="b5f13-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="b5f13-122">`Blog`jest główną jednostki</span><span class="sxs-lookup"><span data-stu-id="b5f13-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="b5f13-123">`Post.BlogId`jest kluczem obcym</span><span class="sxs-lookup"><span data-stu-id="b5f13-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="b5f13-124">`Blog.BlogId`jest to klucz główny (w tym przypadku jest kluczem podstawowym, a nie klucza alternatywnego)</span><span class="sxs-lookup"><span data-stu-id="b5f13-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="b5f13-125">`Post.Blog`jest właściwością nawigacji odwołania</span><span class="sxs-lookup"><span data-stu-id="b5f13-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="b5f13-126">`Blog.Posts`jest właściwością nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="b5f13-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="b5f13-127">`Post.Blog`jest odwróconą właściwość nawigacji z `Blog.Posts` (i na odwrót)</span><span class="sxs-lookup"><span data-stu-id="b5f13-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="b5f13-128">Konwencje</span><span class="sxs-lookup"><span data-stu-id="b5f13-128">Conventions</span></span>

<span data-ttu-id="b5f13-129">Według konwencji będzie można utworzyć relacji, po wykryte na typ właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="b5f13-130">Właściwość jest traktowany jako właściwość nawigacji, jeśli nie można zamapować typu, który wskazuje jako typem skalarnym przez bieżącego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b5f13-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="b5f13-131">Relacje, które zostały wykryte przez Konwencję zawsze będzie obowiązywać klucza podstawowego jednostki podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b5f13-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="b5f13-132">Pod kątem klucza alternatywnego, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b5f13-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="b5f13-133">Relacje zdefiniowane w pełni</span><span class="sxs-lookup"><span data-stu-id="b5f13-133">Fully Defined Relationships</span></span>

<span data-ttu-id="b5f13-134">Najbardziej typowe wzorca dla relacji jest zdefiniowany po obu stronach relacji i właściwości klucza obcego zdefiniowana w klasie jednostki zależnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="b5f13-135">Jeśli między dwoma typami para właściwości nawigacji, następnie one zostanie skonfigurowany jako właściwości nawigacji odwrotny tej samej relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="b5f13-136">Jeśli jednostki zależnej zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`, lub `<principal entity name><primary key property name>` , a następnie zostaną skonfigurowane jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="b5f13-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="b5f13-137">Jeśli istnieje wiele właściwości nawigacji zdefiniowane między dwoma typami (tj. więcej niż jedną parę różne nawigacji wskazujące ze sobą), następnie relacje nie zostaną utworzone przez Konwencję i konieczne będzie ręczne skonfigurowanie ich do identyfikacji sposób pary właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="b5f13-138">Nie właściwości klucza obcego</span><span class="sxs-lookup"><span data-stu-id="b5f13-138">No Foreign Key Property</span></span>

<span data-ttu-id="b5f13-139">Gdy jest zalecane, aby mieć zdefiniowany w klasie jednostki zależnej właściwości klucza obcego, nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="b5f13-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="b5f13-140">Przypadku nieznalezienia ma właściwości klucza obcego, zostaną wprowadzone tle właściwości klucza obcego o nazwie `<navigation property name><principal key property name>` (zobacz [właściwości tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="b5f13-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="b5f13-141">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="b5f13-141">Single Navigation Property</span></span>

<span data-ttu-id="b5f13-142">Łącznie z tylko jedną właściwość nawigacji (nie odwrotny nawigacji i ma właściwości klucza obcego) jest wystarczająca do zdefiniowano relacji w Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="b5f13-143">Można również mieć właściwości pojedynczej wartości i właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="b5f13-144">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="b5f13-144">Cascade Delete</span></span>

<span data-ttu-id="b5f13-145">Konwencja, usuwanie kaskadowe zostanie ustawiona do *Cascade* dla relacji wymagane i *ClientSetNull* dla relacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b5f13-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="b5f13-146">*CASCADE* oznacza jednostki zależne, również zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="b5f13-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="b5f13-147">*ClientSetNull* oznacza, że pozostanie jednostek zależnych, które nie zostały załadowane do pamięci bez zmian i muszą być ręcznie usunięty lub zaktualizowany w celu wskaż prawidłową jednostkę podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b5f13-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="b5f13-148">Dla obiektów, które są ładowane do pamięci EF Core będzie podejmować próby ustaw wartość null z właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="b5f13-149">Zobacz [wymaganych i opcjonalnych relacje](#required-and-optional-relationships) sekcji różnicę między relacje wymaganych i opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="b5f13-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="b5f13-150">Zobacz [Cascade Delete](../saving/cascade-delete.md) więcej informacji na temat różnych Usuń zachowania i wartości domyślne używane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b5f13-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b5f13-151">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="b5f13-151">Data Annotations</span></span>

<span data-ttu-id="b5f13-152">Istnieją dwa adnotacji danych, które mogą służyć do konfigurowania relacji, `[ForeignKey]` i `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="b5f13-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="b5f13-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="b5f13-153">[ForeignKey]</span></span>

<span data-ttu-id="b5f13-154">Adnotacje danych służy do konfigurowania właściwości, które należy używać jako właściwość klucza obcego w określonej relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="b5f13-155">Jest to zazwyczaj wykonywane, gdy właściwość klucza obcego nie zostanie wykryta przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b5f13-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="b5f13-156">`[ForeignKey]` Adnotacji można umieścić we właściwości nawigacji, albo w relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="b5f13-157">Nie trzeba go we właściwości nawigacji w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="b5f13-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="b5f13-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="b5f13-158">[InverseProperty]</span></span>

<span data-ttu-id="b5f13-159">Adnotacje danych służy do konfigurowania, jak pary właściwości nawigacji jednostki zależnej i głównej.</span><span class="sxs-lookup"><span data-stu-id="b5f13-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="b5f13-160">Jest to zazwyczaj wykonywane, gdy istnieje więcej niż jedna para właściwości nawigacji między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="b5f13-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="b5f13-161">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="b5f13-161">Fluent API</span></span>

<span data-ttu-id="b5f13-162">Aby skonfigurować relację w interfejsie API Fluent, należy uruchomić, określając właściwości nawigacji, które tworzą relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="b5f13-163">`HasOne`lub `HasMany` identyfikuje właściwość nawigacji typu jednostki są od konfiguracji na.</span><span class="sxs-lookup"><span data-stu-id="b5f13-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="b5f13-164">Następnie łańcucha wywołania `WithOne` lub `WithMany` do identyfikowania odwrotny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="b5f13-165">`HasOne`/`WithOne`są używane dla właściwości nawigacji odwołania oraz `HasMany` / `WithMany` są używane w przypadku właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="b5f13-166">Właściwości pojedynczej wartości</span><span class="sxs-lookup"><span data-stu-id="b5f13-166">Single Navigation Property</span></span>

<span data-ttu-id="b5f13-167">Jeśli masz tylko jedną właściwość nawigacji, istnieją przeciążenia bez parametrów metody `WithOne` i `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="b5f13-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="b5f13-168">Oznacza to, że jest koncepcyjnie odwołania lub kolekcji na końcu relacji, ale nie ma właściwości nawigacji zawarte w klasie jednostki.</span><span class="sxs-lookup"><span data-stu-id="b5f13-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="b5f13-169">Klucz obcy</span><span class="sxs-lookup"><span data-stu-id="b5f13-169">Foreign Key</span></span>

<span data-ttu-id="b5f13-170">Interfejsu API Fluent służy do konfigurowania właściwości, które należy używać jako właściwość klucza obcego w określonej relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="b5f13-171">Następujący przykładowy kod przedstawia sposób konfigurowania złożonych kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="b5f13-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="b5f13-172">Można użyć ciągu przeciążenia `HasForeignKey(...)` do skonfigurowania właściwości w tle jako klucz obcy (zobacz [właściwości tle](shadow-properties.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="b5f13-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="b5f13-173">Firma Microsoft zaleca jawnie dodawania właściwości cień do modelu, przed użyciem go jako klucz obcy (jak pokazano poniżej).</span><span class="sxs-lookup"><span data-stu-id="b5f13-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="b5f13-174">Klucza podmiotu</span><span class="sxs-lookup"><span data-stu-id="b5f13-174">Principal Key</span></span>

<span data-ttu-id="b5f13-175">Jeśli chcesz, aby klucz obcy do odwołania właściwości innych niż klucz podstawowy, można użyć interfejsu API Fluent do skonfigurowania właściwości klucza podmiotu zabezpieczeń dla tej relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="b5f13-176">Właściwość, którą można skonfigurować jako główny klucza zostanie automatycznie, należy skonfigurować jako klucza alternatywnego (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="b5f13-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="b5f13-177">Następujący przykładowy kod przedstawia sposób konfigurowania złożonych kluczy głównych.</span><span class="sxs-lookup"><span data-stu-id="b5f13-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="b5f13-178">Kolejność, w którym należy określić główny właściwości klucza muszą być zgodne kolejność, w którym są określone dla klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="b5f13-179">Relacje wymaganych i opcjonalnych</span><span class="sxs-lookup"><span data-stu-id="b5f13-179">Required and Optional Relationships</span></span>

<span data-ttu-id="b5f13-180">Aby określić, czy relacja jest wymagany lub opcjonalny, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b5f13-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="b5f13-181">Ostatecznie to określa, czy właściwość klucza obcego jest wymagany lub opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="b5f13-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="b5f13-182">Jest to użyteczna, gdy używasz klucz obcy Stan cienia.</span><span class="sxs-lookup"><span data-stu-id="b5f13-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="b5f13-183">Jeśli masz właściwości klucza obcego w klasie jednostki, requiredness relacji jest określany na podstawie tego, czy właściwość klucza obcego jest wymagany lub opcjonalny (zobacz [właściwości wymaganych i opcjonalnych](required-optional.md) Aby uzyskać więcej informacji informacje).</span><span class="sxs-lookup"><span data-stu-id="b5f13-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="b5f13-184">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="b5f13-184">Cascade Delete</span></span>

<span data-ttu-id="b5f13-185">Można użyć interfejsu API Fluent jawnie skonfigurować zachowanie usuwania kaskadowego określonej relacji.</span><span class="sxs-lookup"><span data-stu-id="b5f13-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="b5f13-186">Zobacz [Cascade Delete](../saving/cascade-delete.md) w sekcji zapisywania danych szczegółowe omówienie każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="b5f13-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="b5f13-187">Innymi wzorami relacji</span><span class="sxs-lookup"><span data-stu-id="b5f13-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="b5f13-188">Jeden do jednego</span><span class="sxs-lookup"><span data-stu-id="b5f13-188">One-to-one</span></span>

<span data-ttu-id="b5f13-189">Relacje jeden-do-jednego ma właściwości nawigacji odwołania po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="b5f13-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="b5f13-190">Wykonaj one tej samej Konwencji jako relacji jeden do wielu, ale wprowadzono unikatowego indeksu na właściwość klucza obcego w celu zapewnienia, że tylko jeden zależne od powiązany jest każdy podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b5f13-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="b5f13-191">EF wybrać jeden jednostek jako zależnego oparte na możliwość wykrywania właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b5f13-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="b5f13-192">Jeśli niewłaściwy jednostki jest wybrany jako zależnego, można użyć interfejsu API Fluent, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="b5f13-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="b5f13-193">Podczas konfigurowania relacji z interfejsu API Fluent, użyj `HasOne` i `WithOne` metody.</span><span class="sxs-lookup"><span data-stu-id="b5f13-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="b5f13-194">Podczas konfigurowania klucza obcego, należy określić typ jednostki zależnej — zauważyć przekazane do parametru ogólnego `HasForeignKey` na liście poniżej.</span><span class="sxs-lookup"><span data-stu-id="b5f13-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="b5f13-195">W relacji jeden do wielu jest jasne, czy jednostka z nawigacji odwołania jest zależnego i z kolekcji jest podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b5f13-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="b5f13-196">Ale nie jest tak w relacji jeden - dlatego trzeba jawnie definiować go.</span><span class="sxs-lookup"><span data-stu-id="b5f13-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="b5f13-197">Wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="b5f13-197">Many-to-many</span></span>

<span data-ttu-id="b5f13-198">Relacje wiele do wielu bez klasę jednostki do reprezentowania tabeli sprzężenia nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="b5f13-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="b5f13-199">Jednak może wymagać relacji wiele do wielu, umieszczając klasę jednostki dla tabeli sprzężenia i relacje jeden do wielu oddzielnych dwóch mapowania.</span><span class="sxs-lookup"><span data-stu-id="b5f13-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
