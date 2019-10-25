---
title: Pola zapasowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811879"
---
# <a name="backing-fields"></a><span data-ttu-id="53bfe-102">Pola zapasowe</span><span class="sxs-lookup"><span data-stu-id="53bfe-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="53bfe-103">Ta funkcja jest nowa w EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="53bfe-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="53bfe-104">Pola zapasowe umożliwiają EF odczyt i/lub zapis do pola, a nie właściwości.</span><span class="sxs-lookup"><span data-stu-id="53bfe-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="53bfe-105">Może to być przydatne, gdy hermetyzacja w klasie jest używana do ograniczania użycia i/lub podniesienia semantyki dostępu do danych przez kod aplikacji, ale wartość powinna zostać odczytana i/lub zapisywana w bazie danych bez użycia tych ograniczeń/ usprawni.</span><span class="sxs-lookup"><span data-stu-id="53bfe-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="53bfe-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="53bfe-106">Conventions</span></span>

<span data-ttu-id="53bfe-107">Według Konwencji następujące pola zostaną odnalezione jako pola zapasowe dla danej właściwości (wymienione w kolejności pierwszeństwa).</span><span class="sxs-lookup"><span data-stu-id="53bfe-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="53bfe-108">Pola są odnajdywane tylko dla właściwości, które są uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="53bfe-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="53bfe-109">Aby uzyskać więcej informacji na temat właściwości uwzględnionych w modelu, zobacz [m.in. & z wyłączeniem właściwości](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="53bfe-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="53bfe-110">W przypadku skonfigurowania pola zapasowego program Dr zapisuje bezpośrednio do tego pola podczas materializacji wystąpienia jednostek z bazy danych (zamiast używać metody ustawiającej właściwość).</span><span class="sxs-lookup"><span data-stu-id="53bfe-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="53bfe-111">Jeśli EF musi odczytać lub zapisać wartość w innym czasie, będzie używać właściwości, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="53bfe-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="53bfe-112">Na przykład jeśli EF musi zaktualizować wartość właściwości, zostanie użyta Metoda ustawiająca właściwość, jeśli jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="53bfe-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="53bfe-113">Jeśli właściwość jest tylko do odczytu, nastąpi zapis do pola.</span><span class="sxs-lookup"><span data-stu-id="53bfe-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="53bfe-114">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="53bfe-114">Data Annotations</span></span>

<span data-ttu-id="53bfe-115">Nie można skonfigurować pól zapasowych przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="53bfe-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="53bfe-116">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="53bfe-116">Fluent API</span></span>

<span data-ttu-id="53bfe-117">Za pomocą interfejsu API Fluent można skonfigurować pole zapasowe dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="53bfe-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="53bfe-118">Kontrolowanie, gdy pole jest używane</span><span class="sxs-lookup"><span data-stu-id="53bfe-118">Controlling when the field is used</span></span>

<span data-ttu-id="53bfe-119">Można skonfigurować, kiedy dr używa pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="53bfe-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="53bfe-120">Zobacz [Wyliczenie PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) dla obsługiwanych opcji.</span><span class="sxs-lookup"><span data-stu-id="53bfe-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="53bfe-121">Pola bez właściwości</span><span class="sxs-lookup"><span data-stu-id="53bfe-121">Fields without a property</span></span>

<span data-ttu-id="53bfe-122">Można również utworzyć w modelu Właściwość koncepcyjną, która nie ma odpowiedniej właściwości CLR w klasie Entity, ale zamiast tego używa pola do przechowywania danych w jednostce.</span><span class="sxs-lookup"><span data-stu-id="53bfe-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="53bfe-123">Różni się to od [Właściwości cienia](shadow-properties.md), gdzie dane są przechowywane w monitorze zmian.</span><span class="sxs-lookup"><span data-stu-id="53bfe-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="53bfe-124">Zwykle jest to używane, jeśli Klasa jednostki używa metod do pobierania/ustawiania wartości.</span><span class="sxs-lookup"><span data-stu-id="53bfe-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="53bfe-125">Można nadać EF nazwę pola w interfejsie API `Property(...)`.</span><span class="sxs-lookup"><span data-stu-id="53bfe-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="53bfe-126">Jeśli nie ma żadnej właściwości o podaną nazwę, EF będzie szukać pola.</span><span class="sxs-lookup"><span data-stu-id="53bfe-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="53bfe-127">Jeśli w klasie Entity nie ma właściwości, można użyć metody `EF.Property(...)` w zapytaniu LINQ, aby odwołać się do właściwości, która jest koncepcyjnie częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="53bfe-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
