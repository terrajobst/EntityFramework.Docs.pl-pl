---
title: Tworzenie kopii pola — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054533"
---
# <a name="backing-fields"></a><span data-ttu-id="e8570-102">Pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="e8570-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="e8570-103">Ta funkcja jest nowa w programie EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e8570-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="e8570-104">Pola zapasowy umożliwia EF do odczytu lub zapisu do pola, a nie właściwością.</span><span class="sxs-lookup"><span data-stu-id="e8570-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="e8570-105">Może to być przydatne, gdy hermetyzacji klasy jest używany do ograniczenia stosowania i/lub zwiększenia semantyki wokół dostęp do danych przez kod aplikacji, ale wartość powinna być odczytywać i/lub zapisane w bazie danych bez korzystania z tych ograniczeń / ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="e8570-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="e8570-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="e8570-106">Conventions</span></span>

<span data-ttu-id="e8570-107">Według Konwencji spowoduje odnalezienie następujących pól jak kopie pól dla danej właściwości (wymienione w kolejności priorytetu).</span><span class="sxs-lookup"><span data-stu-id="e8570-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="e8570-108">Pola są odnajdywane tylko dla właściwości, które znajdują się w modelu.</span><span class="sxs-lookup"><span data-stu-id="e8570-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="e8570-109">Aby uzyskać więcej informacji, na którym właściwości znajdują się w modelu, zobacz [tym & Wykluczanie właściwości](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="e8570-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="e8570-110">Po skonfigurowaniu polem zapasowym EF zapisze bezpośrednio do tego pola, gdy materializowania wystąpień jednostek z bazy danych (a nie za pomocą metody ustawiającej właściwości).</span><span class="sxs-lookup"><span data-stu-id="e8570-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="e8570-111">Jeśli EF musi odczytać lub zapisać wartość w innym czasie, właściwość zostanie użyty, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="e8570-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="e8570-112">Na przykład jeśli EF musi zaktualizować wartości dla właściwości, zostanie użyty metoda ustawiająca właściwości, jeśli jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="e8570-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="e8570-113">Jeśli właściwość jest tylko do odczytu, następnie go zapisze do pola.</span><span class="sxs-lookup"><span data-stu-id="e8570-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e8570-114">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="e8570-114">Data Annotations</span></span>

<span data-ttu-id="e8570-115">Tworzenie kopii pól nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="e8570-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e8570-116">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="e8570-116">Fluent API</span></span>

<span data-ttu-id="e8570-117">Interfejsu API Fluent służy do konfigurowania polem zapasowym dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="e8570-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="e8570-118">Kontrolowanie, gdy to pole jest używane</span><span class="sxs-lookup"><span data-stu-id="e8570-118">Controlling when the field is used</span></span>

<span data-ttu-id="e8570-119">Można skonfigurować podczas EF używa pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="e8570-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="e8570-120">Zobacz [PropertyAccessMode wyliczenia](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) dla obsługiwane opcje.</span><span class="sxs-lookup"><span data-stu-id="e8570-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="e8570-121">Pola bez właściwości</span><span class="sxs-lookup"><span data-stu-id="e8570-121">Fields without a property</span></span>

<span data-ttu-id="e8570-122">Można też utworzyć właściwości koncepcyjnej w modelu, które nie mają odpowiednia właściwość CLR w klasie jednostki, ale zamiast tego używa pola do przechowywania danych w jednostce.</span><span class="sxs-lookup"><span data-stu-id="e8570-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="e8570-123">Różni się to od [właściwości tle](shadow-properties.md), gdy dane są przechowywane w śledzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="e8570-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="e8570-124">To zazwyczaj będzie można użyć, jeśli klasa jednostki używa metody do pobierania/ustawiania wartości.</span><span class="sxs-lookup"><span data-stu-id="e8570-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="e8570-125">Można nadać EF nazwę pola w `Property(...)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e8570-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="e8570-126">Jeśli nie ma właściwości o podanej nazwie, EF będzie wyglądać dla pola.</span><span class="sxs-lookup"><span data-stu-id="e8570-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="e8570-127">Można również podać właściwość nazwę inną niż nazwa pola.</span><span class="sxs-lookup"><span data-stu-id="e8570-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="e8570-128">Ta nazwa jest następnie używana podczas tworzenia modelu, głównie będzie można użyć dla nazwy kolumny, który jest mapowany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e8570-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="e8570-129">Jeśli nie ma właściwości w klasie jednostki, możesz użyć `EF.Property(...)` metody w zapytaniu składnika LINQ do odwoływania się do właściwości koncepcyjnie jest częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="e8570-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
