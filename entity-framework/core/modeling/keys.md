---
title: Klucze — EF Core
description: Jak skonfigurować klucze dla typów jednostek podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502009"
---
# <a name="keys"></a><span data-ttu-id="4fe15-103">Klucze</span><span class="sxs-lookup"><span data-stu-id="4fe15-103">Keys</span></span>

<span data-ttu-id="4fe15-104">Klucz służy jako unikatowy identyfikator dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="4fe15-104">A key serves as a unique identifier for each entity instance.</span></span> <span data-ttu-id="4fe15-105">Większość jednostek w EF ma jeden klucz, który mapuje na koncepcję *klucza podstawowego* w relacyjnych bazach danych (dla jednostek bez kluczy, zobacz [jednostki o niemniejszej](xref:core/modeling/keyless-entity-types)liczbie).</span><span class="sxs-lookup"><span data-stu-id="4fe15-105">Most entities in EF have a single key, which maps to the concept of a *primary key* in relational databases (for entities without keys, see [Keyless entities](xref:core/modeling/keyless-entity-types)).</span></span> <span data-ttu-id="4fe15-106">Jednostki mogą mieć dodatkowe klucze poza kluczem podstawowym (zobacz [klucze alternatywne](#alternate-keys) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="4fe15-106">Entities can have additional keys beyond the primary key (see [Alternate Keys](#alternate-keys) for more information).</span></span>

<span data-ttu-id="4fe15-107">Zgodnie z Konwencją właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz podstawowy jednostki.</span><span class="sxs-lookup"><span data-stu-id="4fe15-107">By convention, a property named `Id` or `<type name>Id` will be configured as the primary key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> <span data-ttu-id="4fe15-108">[Typy jednostek należących](xref:core/modeling/owned-entities) do siebie używają różnych reguł do definiowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="4fe15-108">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

<span data-ttu-id="4fe15-109">Jedną właściwość można skonfigurować jako klucz podstawowy jednostki w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4fe15-109">You can configure a single property to be the primary key of an entity as follows:</span></span>

## <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="4fe15-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="4fe15-110">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="4fe15-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="4fe15-111">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

<span data-ttu-id="4fe15-112">Można również skonfigurować wiele właściwości jako klucz jednostki — jest to nazywane kluczem złożonym.</span><span class="sxs-lookup"><span data-stu-id="4fe15-112">You can also configure multiple properties to be the key of an entity - this is known as a composite key.</span></span> <span data-ttu-id="4fe15-113">Klucze złożone można skonfigurować tylko za pomocą interfejsu API Fluent; konwencje nigdy nie będą konfigurować klucza złożonego i nie można użyć adnotacji danych, aby je skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="4fe15-113">Composite keys can only be configured using the Fluent API; conventions will never setup a composite key, and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a><span data-ttu-id="4fe15-114">Nazwa klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="4fe15-114">Primary key name</span></span>

<span data-ttu-id="4fe15-115">Według Konwencji w kluczach podstawowych relacyjnych baz danych tworzone są nazwy `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="4fe15-115">By convention, on relational databases primary keys are created with the name `PK_<type name>`.</span></span> <span data-ttu-id="4fe15-116">Nazwę ograniczenia PRIMARY KEY można skonfigurować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4fe15-116">You can configure the name of the primary key constraint as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a><span data-ttu-id="4fe15-117">Typy i wartości kluczy</span><span class="sxs-lookup"><span data-stu-id="4fe15-117">Key types and values</span></span>

<span data-ttu-id="4fe15-118">Chociaż EF Core obsługuje używanie właściwości dowolnego typu pierwotnego jako klucza podstawowego, w tym `string`, `Guid`, `byte[]` i innych, nie wszystkie bazy danych obsługują wszystkie typy jako klucze.</span><span class="sxs-lookup"><span data-stu-id="4fe15-118">While EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others, not all databases support all types as keys.</span></span> <span data-ttu-id="4fe15-119">W niektórych przypadkach wartości klucza można przekonwertować na typ obsługiwany automatycznie. w przeciwnym razie konwersja powinna być [określona ręcznie](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="4fe15-119">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="4fe15-120">Właściwości klucza muszą zawsze mieć wartość inną niż domyślna podczas dodawania nowej jednostki do kontekstu, ale niektóre typy zostaną [wygenerowane przez bazę danych](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="4fe15-120">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="4fe15-121">W takim przypadku EF podejmie próbę wygenerowania wartości tymczasowej, gdy jednostka zostanie dodana do celów śledzenia.</span><span class="sxs-lookup"><span data-stu-id="4fe15-121">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="4fe15-122">Po wywołaniu [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) wartość tymczasowa zostanie zastąpiona wartością wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4fe15-122">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="4fe15-123">Jeśli właściwość klucza ma swoją wartość wygenerowaną przez bazę danych i określono wartość inną niż domyślna podczas dodawania jednostki, EF przyjmie, że jednostka już istnieje w bazie danych i spróbuje ją zaktualizować zamiast wstawiania nowej.</span><span class="sxs-lookup"><span data-stu-id="4fe15-123">If a key property has its value generated by the database and a non-default value is specified when an entity is added, then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="4fe15-124">Aby tego uniknąć, Wyłącz generowanie wartości lub zobacz [, jak określić wartości jawne dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="4fe15-124">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>

## <a name="alternate-keys"></a><span data-ttu-id="4fe15-125">Klucze alternatywne</span><span class="sxs-lookup"><span data-stu-id="4fe15-125">Alternate Keys</span></span>

<span data-ttu-id="4fe15-126">Alternatywny klucz służy jako alternatywny unikatowy identyfikator dla każdego wystąpienia jednostki oprócz klucza podstawowego; może być używany jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="4fe15-126">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key; it can be used as the target of a relationship.</span></span> <span data-ttu-id="4fe15-127">W przypadku korzystania z relacyjnej bazy danych mapowanie do koncepcji unikatowego indeksu/ograniczenia w kolumnach klucza alternatywnego oraz co najmniej jedno ograniczenie klucza obcego odwołujące się do kolumn.</span><span class="sxs-lookup"><span data-stu-id="4fe15-127">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]
> <span data-ttu-id="4fe15-128">Jeśli chcesz tylko wymusić unikatowość w kolumnie, zdefiniuj unikatowy indeks, a nie klucz alternatywny (zobacz [indeksy](indexes.md)).</span><span class="sxs-lookup"><span data-stu-id="4fe15-128">If you just want to enforce uniqueness on a column, define a unique index rather than an alternate key (see [Indexes](indexes.md)).</span></span> <span data-ttu-id="4fe15-129">W EF klucze alternatywne są tylko do odczytu i zapewniają dodatkową semantykę za pośrednictwem unikatowych indeksów, ponieważ mogą one być używane jako obiekty docelowe klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="4fe15-129">In EF, alternate keys are read-only and provide additional semantics over unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="4fe15-130">Klucze alternatywne są zwykle wprowadzane w razie potrzeby i nie trzeba ich ręcznie konfigurować.</span><span class="sxs-lookup"><span data-stu-id="4fe15-130">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="4fe15-131">Przy użyciu konwencji klucz alternatywny jest wprowadzany w przypadku identyfikowania właściwości, która nie jest kluczem podstawowym jako element docelowy relacji.</span><span class="sxs-lookup"><span data-stu-id="4fe15-131">By convention, an alternate key is introduced for you when you identify a property which isn't the primary key as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

<span data-ttu-id="4fe15-132">Można również skonfigurować pojedynczą właściwość jako klucz alternatywny:</span><span class="sxs-lookup"><span data-stu-id="4fe15-132">You can also configure a single property to be an alternate key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

<span data-ttu-id="4fe15-133">Możesz również skonfigurować wiele właściwości jako klucz alternatywny (nazywany kluczem alternatywnym):</span><span class="sxs-lookup"><span data-stu-id="4fe15-133">You can also configure multiple properties to be an alternate key (known as a composite alternate key):</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

<span data-ttu-id="4fe15-134">Na koniec, zgodnie z Konwencją, indeks i ograniczenie, które są wprowadzone dla klucza alternatywnego, będą nazwane `AK_<type name>_<property name>` (dla złożonych kluczy alternatywnych `<property name>` stać się podkreśleniem rozdzielanym podkreśleniami listy nazw właściwości).</span><span class="sxs-lookup"><span data-stu-id="4fe15-134">Finally, by convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>` (for composite alternate keys `<property name>` becomes an underscore separated list of property names).</span></span> <span data-ttu-id="4fe15-135">Można skonfigurować nazwę indeksu klucza alternatywnego i ograniczenia UNIQUE:</span><span class="sxs-lookup"><span data-stu-id="4fe15-135">You can configure the name of the alternate key's index and unique constraint:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
