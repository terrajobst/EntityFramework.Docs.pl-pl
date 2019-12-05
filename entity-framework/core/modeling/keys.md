---
title: Klucze (podstawowe) — EF Core
description: Jak skonfigurować klucze dla typów jednostek podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824619"
---
# <a name="keys-primary"></a><span data-ttu-id="1b607-103">Klucze (podstawowe)</span><span class="sxs-lookup"><span data-stu-id="1b607-103">Keys (primary)</span></span>

<span data-ttu-id="1b607-104">Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="1b607-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="1b607-105">W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*.</span><span class="sxs-lookup"><span data-stu-id="1b607-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="1b607-106">Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="1b607-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="1b607-107">Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="1b607-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="1b607-108">Konwencje</span><span class="sxs-lookup"><span data-stu-id="1b607-108">Conventions</span></span>

<span data-ttu-id="1b607-109">Domyślnie właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="1b607-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="1b607-110">[Typy jednostek należących](xref:core/modeling/owned-entities) do siebie używają różnych reguł do definiowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="1b607-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1b607-111">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="1b607-111">Data Annotations</span></span>

<span data-ttu-id="1b607-112">Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="1b607-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="1b607-113">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="1b607-113">Fluent API</span></span>

<span data-ttu-id="1b607-114">Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="1b607-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="1b607-115">Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym).</span><span class="sxs-lookup"><span data-stu-id="1b607-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="1b607-116">Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="1b607-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="1b607-117">Typy i wartości kluczy</span><span class="sxs-lookup"><span data-stu-id="1b607-117">Key types and values</span></span>

<span data-ttu-id="1b607-118">EF Core obsługuje używanie właściwości dowolnego typu pierwotnego jako klucza podstawowego, w tym `string`, `Guid`, `byte[]` i innych.</span><span class="sxs-lookup"><span data-stu-id="1b607-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="1b607-119">Ale nie są one obsługiwane przez wszystkie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1b607-119">But not all databases support them.</span></span> <span data-ttu-id="1b607-120">W niektórych przypadkach wartości klucza można przekonwertować na typ obsługiwany automatycznie. w przeciwnym razie konwersja powinna być [określona ręcznie](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="1b607-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="1b607-121">Właściwości klucza muszą zawsze mieć wartość inną niż domyślna podczas dodawania nowej jednostki do kontekstu, ale niektóre typy zostaną [wygenerowane przez bazę danych](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="1b607-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="1b607-122">W takim przypadku EF podejmie próbę wygenerowania wartości tymczasowej, gdy jednostka zostanie dodana do celów śledzenia.</span><span class="sxs-lookup"><span data-stu-id="1b607-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="1b607-123">Po wywołaniu [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) wartość tymczasowa zostanie zastąpiona wartością wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1b607-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="1b607-124">Jeśli właściwość klucza ma wartość wygenerowaną przez bazę danych i określono wartość inną niż domyślna, gdy zostanie dodana jednostka, EF przyjmie, że jednostka już istnieje w bazie danych i spróbuje ją zaktualizować zamiast wstawiania nowej.</span><span class="sxs-lookup"><span data-stu-id="1b607-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="1b607-125">Aby tego uniknąć, Wyłącz generowanie wartości lub zobacz [, jak określić wartości jawne dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="1b607-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>