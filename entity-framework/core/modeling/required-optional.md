---
title: Właściwości wymagane i opcjonalne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655662"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="19375-102">Właściwości wymagane i opcjonalne</span><span class="sxs-lookup"><span data-stu-id="19375-102">Required and Optional Properties</span></span>

<span data-ttu-id="19375-103">Właściwość jest uważana za opcjonalną, jeśli jest poprawna, aby zawierała `null`.</span><span class="sxs-lookup"><span data-stu-id="19375-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="19375-104">Jeśli `null` nie jest prawidłową wartością do przypisania do właściwości, zostanie ona uznana za właściwość wymaganą.</span><span class="sxs-lookup"><span data-stu-id="19375-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="19375-105">Podczas mapowania na schemat relacyjnej bazy danych, wymagane właściwości są tworzone jako kolumny niedopuszczające wartości null, a właściwości opcjonalne są tworzone jako kolumny dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="19375-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="19375-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="19375-106">Conventions</span></span>

<span data-ttu-id="19375-107">Zgodnie z Konwencją właściwość, której typ .NET może zawierać wartość null, zostanie skonfigurowana jako opcjonalna, natomiast właściwości, których typ .NET nie może zawierać wartości null, zostaną skonfigurowane zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="19375-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="19375-108">Na przykład wszystkie właściwości z typami wartości .NET (`int`, `decimal`, `bool`itp.) są skonfigurowane jako wymagane, a wszystkie właściwości z typami wartości null platformy .NET (`int?`, `decimal?`, `bool?`itp.) są konfigurowane jako opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="19375-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="19375-109">C#8 wprowadzono nową funkcję o nazwie [typu referencyjnego nullable](/dotnet/csharp/tutorials/nullable-reference-types), która pozwala na dodawanie adnotacji do typów referencyjnych, wskazując, czy są one prawidłowe, aby zawierały wartość null.</span><span class="sxs-lookup"><span data-stu-id="19375-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="19375-110">Ta funkcja jest domyślnie wyłączona, a jeśli ta opcja jest włączona, modyfikuje zachowanie EF Core w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="19375-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="19375-111">Jeśli typy odwołań do wartości null są wyłączone (wartość domyślna), wszystkie właściwości z typami odwołań platformy .NET są konfigurowane jako opcjonalne według Konwencji (np. `string`).</span><span class="sxs-lookup"><span data-stu-id="19375-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="19375-112">Jeśli typy odwołań do wartości null są włączone, właściwości zostaną skonfigurowane na podstawie C# wartości null typu .net: `string?` zostanie skonfigurowany jako opcjonalny, a `string` zostanie skonfigurowany zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="19375-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="19375-113">W poniższym przykładzie przedstawiono typ jednostki z wymaganymi i opcjonalnymi właściwościami z włączoną funkcją odwołania do wartości null (ustawienie domyślne) i włączony:</span><span class="sxs-lookup"><span data-stu-id="19375-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="19375-114">Bez wartości null typów referencyjnych (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="19375-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="19375-115">Z typami odwołań dopuszczających wartość null</span><span class="sxs-lookup"><span data-stu-id="19375-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="19375-116">Zaleca się użycie typów referencyjnych dopuszczających wartość null, ponieważ powoduje to C# przeznaczenie wartości zerowej wyrażonej w kodzie do modelu EF Core i bazy danych, a także eliminuje użycie interfejsu API Fluent lub adnotacji danych w celu wyrażenia tego samego pojęcia dwa razy.</span><span class="sxs-lookup"><span data-stu-id="19375-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="19375-117">Należy zachować ostrożność podczas włączania typów referencyjnych dopuszczających wartość null w istniejącym projekcie: właściwości typu referencyjnego, które zostały wcześniej skonfigurowane jako opcjonalne, zostaną teraz skonfigurowane zgodnie z wymaganiami, chyba że zostaną jawnie dodane do wartości null.</span><span class="sxs-lookup"><span data-stu-id="19375-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="19375-118">W przypadku zarządzania schematem relacyjnej bazy danych może to spowodować wygenerowanie migracji, co pozwala zmienić wartość null kolumny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="19375-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="19375-119">Aby uzyskać więcej informacji na temat typów referencyjnych dopuszczających wartości null i sposobu ich używania z EF Core, [Zobacz stronę dedykowanej dokumentacji dla tej funkcji](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="19375-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="19375-120">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="19375-120">Configuration</span></span>

<span data-ttu-id="19375-121">Właściwość, która będzie opcjonalna w Konwencji, można skonfigurować tak, aby była wymagana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="19375-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="19375-122">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="19375-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="19375-123">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="19375-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
