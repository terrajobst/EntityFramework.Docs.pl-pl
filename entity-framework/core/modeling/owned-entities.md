---
title: Posiadane typy jednostek — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333794"
---
# <a name="owned-entity-types"></a><span data-ttu-id="421f2-102">Posiadane typy jednostek</span><span class="sxs-lookup"><span data-stu-id="421f2-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="421f2-103">Ta funkcja jest nowa w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="421f2-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="421f2-104">EF Core umożliwia modelu typów jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="421f2-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="421f2-105">Są to tak zwane _posiadane typy jednostek_.</span><span class="sxs-lookup"><span data-stu-id="421f2-105">These are called _owned entity types_.</span></span> <span data-ttu-id="421f2-106">Obiekt zawierający typ jednostki należące do firmy jest jego _właściciela_.</span><span class="sxs-lookup"><span data-stu-id="421f2-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="421f2-107">Jawne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="421f2-107">Explicit configuration</span></span>

<span data-ttu-id="421f2-108">Własność jednostki typy nigdy nie znajdują się przez platformę EF Core w modelu przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="421f2-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="421f2-109">Można użyć `OwnsOne` method in Class metoda `OnModelCreating` lub dodawać adnotacje do typu z `OwnedAttribute` (nowe w programie EF Core 2.1) Aby skonfigurować typ jako typ należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="421f2-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="421f2-110">W tym przykładzie `StreetAddress` jest typem przy użyciu żadnej właściwości tożsamości.</span><span class="sxs-lookup"><span data-stu-id="421f2-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="421f2-111">Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu zamówienia.</span><span class="sxs-lookup"><span data-stu-id="421f2-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="421f2-112">Możemy użyć `OwnedAttribute` traktowanie jej jako należących do jednostki, gdy występuje do innego typu jednostki:</span><span class="sxs-lookup"><span data-stu-id="421f2-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="421f2-113">Istnieje również możliwość użycia `OwnsOne` method in Class metoda `OnModelCreating` Aby określić, że `ShippingAddress` właściwość jest własnością jednostki `Order` typu jednostki i skonfigurować dodatkowe aspekty, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="421f2-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="421f2-114">Jeśli `ShippingAddress` właściwość jest prywatna w `Order` typu, można użyć ciągu wersję `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="421f2-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="421f2-115">Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) Aby uzyskać dodatkowy kontekst.</span><span class="sxs-lookup"><span data-stu-id="421f2-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="421f2-116">Niejawne kluczy</span><span class="sxs-lookup"><span data-stu-id="421f2-116">Implicit keys</span></span>

<span data-ttu-id="421f2-117">Posiadane typy skonfigurowano `OwnsOne` lub odnalezione za pomocą nawigacji odwołania zawsze mieć relację jeden do jednego z właścicielem, dlatego nie potrzebują własne wartości klucza jako wartości klucza obcego są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="421f2-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="421f2-118">W poprzednim przykładzie `StreetAddress` typu nie trzeba zdefiniować właściwość klucza.</span><span class="sxs-lookup"><span data-stu-id="421f2-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="421f2-119">Aby dowiedzieć się, jak EF Core śledzi te obiekty, warto Pomyśl, czy klucz podstawowy został utworzony jako [w tle właściwość](xref:core/modeling/shadow-properties) należących do typu.</span><span class="sxs-lookup"><span data-stu-id="421f2-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="421f2-120">Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.</span><span class="sxs-lookup"><span data-stu-id="421f2-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="421f2-121">Kolekcji typów będących własnością</span><span class="sxs-lookup"><span data-stu-id="421f2-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="421f2-122">Ta funkcja jest nowa w programie EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="421f2-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="421f2-123">Aby skonfigurować kolekcji typów będących własnością `OwnsMany` powinny być używane w `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="421f2-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="421f2-124">Jednak klucza podstawowego nie zostanie skonfigurowany zgodnie z Konwencją, dlatego musi być jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="421f2-124">However the primary key will not be configured by convention, so it needs to be specified explicitly.</span></span> <span data-ttu-id="421f2-125">Powszechne jest wprowadzanie klucza złożonego na użytek tych typów jednostek, zawierające klucz obcy do właściciela i dodatkowe unikatowa właściwość, która również może być w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="421f2-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="421f2-126">Posiadane typy z dzielenia tabeli mapowania</span><span class="sxs-lookup"><span data-stu-id="421f2-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="421f2-127">Korzystając z relacyjnych baz danych, według Konwencji odwołania należące do typów są mapowane na tej samej tabeli jako właściciel.</span><span class="sxs-lookup"><span data-stu-id="421f2-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="421f2-128">Ta migracja wymaga dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki.</span><span class="sxs-lookup"><span data-stu-id="421f2-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="421f2-129">Jest to typowe funkcje znane jako dzielenie tabeli.</span><span class="sxs-lookup"><span data-stu-id="421f2-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="421f2-130">Posiadane typy przechowywane z dzielenia tabeli mogą być używane, podobnie jak złożonych typów są używane w EF6.</span><span class="sxs-lookup"><span data-stu-id="421f2-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="421f2-131">Zgodnie z Konwencją programu EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="421f2-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="421f2-132">W związku z tym `StreetAddress` właściwości będą wyświetlane w tabeli "Zamówienia" o nazwach "ShippingAddress_Street" i "ShippingAddress_City".</span><span class="sxs-lookup"><span data-stu-id="421f2-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="421f2-133">Możesz dołączyć `HasColumnName` metodę, aby zmienić te kolumny:</span><span class="sxs-lookup"><span data-stu-id="421f2-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="421f2-134">Udostępnianie tego samego typu .NET wśród wielu typów należące do firmy</span><span class="sxs-lookup"><span data-stu-id="421f2-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="421f2-135">Typ jednostki należące do firmy może być tego samego typu .NET jako inny typ jednostki należące do firmy, w związku z tym, że typ architektury .NET może nie być wystarczająca do identyfikowania typu należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="421f2-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="421f2-136">W takich przypadkach staje się właściwość wskazuje od właściciela do jednostki należące do firmy _Definiowanie nawigacji_ typu jednostki należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="421f2-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="421f2-137">Z perspektywy programu EF Core definiujące Nawigacja jest częścią tożsamości typu wraz z typem platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="421f2-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="421f2-138">Na przykład w następującej klasy `ShippingAddress` i `BillingAddress` są tego samego typu .NET `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="421f2-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="421f2-139">Aby dowiedzieć się, jak EF Core pomoże wyróżnić śledzonych wystąpień tych obiektów, może być wyobrazić sobie, że definiujące nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET typu należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="421f2-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="421f2-140">Zagnieżdżone typy należące do firmy</span><span class="sxs-lookup"><span data-stu-id="421f2-140">Nested owned types</span></span>

<span data-ttu-id="421f2-141">W tym przykładzie `OrderDetails` właścicielem `BillingAddress` i `ShippingAddress`, które są zarówno `StreetAddress` typów.</span><span class="sxs-lookup"><span data-stu-id="421f2-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="421f2-142">Następnie `OrderDetails` jest własnością `DetailedOrder` typu.</span><span class="sxs-lookup"><span data-stu-id="421f2-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="421f2-143">Oprócz zagnieżdżonych typów należące do firmy typem należące do firmy można odwoływać się do regularnego jednostki, tak długo, jak należących do jednostki znajduje się na stronie zależne może być właścicielem lub innej jednostki.</span><span class="sxs-lookup"><span data-stu-id="421f2-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="421f2-144">Ta funkcja ustawia EF6 posiadane typy jednostek oprócz typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="421f2-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="421f2-145">Istnieje możliwość łańcucha `OwnsOne` metody w wywołaniu fluent, aby skonfigurować ten model:</span><span class="sxs-lookup"><span data-stu-id="421f2-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="421f2-146">Istnieje również możliwość można osiągnąć używając tej samej kolejności `OwnedAttribute` zarówno `OrderDetails` i `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="421f2-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="421f2-147">Przechowywanie należące do typów w oddzielnych tabelach</span><span class="sxs-lookup"><span data-stu-id="421f2-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="421f2-148">Również inaczej niż w przypadku typów złożonych EF6 posiadane typy mogą być przechowywane w osobnej tabeli od właściciela.</span><span class="sxs-lookup"><span data-stu-id="421f2-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="421f2-149">Aby zastąpić Konwencji, która mapuje typ należących do tej samej tabeli jako właściciela, możesz po prostu wywołać `ToTable` i podaj inną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="421f2-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="421f2-150">Poniższy przykład zmapuje `OrderDetails` i jego dwa adresy do osobnej tabeli z `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="421f2-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="421f2-151">Wykonywanie zapytania dotyczącego typów należące do firmy</span><span class="sxs-lookup"><span data-stu-id="421f2-151">Querying owned types</span></span>

<span data-ttu-id="421f2-152">Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="421f2-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="421f2-153">Nie jest konieczne użycie `Include` metody, nawet jeśli posiadane typy są przechowywane w osobnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="421f2-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="421f2-154">Na podstawie modelu opisany wcześniej, następujące zapytanie zwróci `Order`, `OrderDetails` a dwa należące do `StreetAddresses` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="421f2-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="421f2-155">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="421f2-155">Limitations</span></span>

<span data-ttu-id="421f2-156">Niektórych z tych ograniczeń mają zasadnicze znaczenie jak należących do pracy typów jednostek, ale niektóre inne ograniczenia, że firma Microsoft może być niemożliwe do usunięcia w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="421f2-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="421f2-157">Ograniczenia według projektu</span><span class="sxs-lookup"><span data-stu-id="421f2-157">By-design restrictions</span></span>
- <span data-ttu-id="421f2-158">Nie można utworzyć `DbSet<T>` dla typu należące do firmy</span><span class="sxs-lookup"><span data-stu-id="421f2-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="421f2-159">Nie można wywołać `Entity<T>()` z typem należące do firmy na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="421f2-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="421f2-160">Bieżący wad</span><span class="sxs-lookup"><span data-stu-id="421f2-160">Current shortcomings</span></span>
- <span data-ttu-id="421f2-161">Hierarchii dziedziczenia, które obejmują posiadane typy jednostek nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="421f2-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="421f2-162">Odwołanie do tego celu posiadane typy jednostek nie może być pusty, chyba że jawnie są mapowane na tabelę oddzielnych od właściciela</span><span class="sxs-lookup"><span data-stu-id="421f2-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="421f2-163">Wystąpienia elementu posiadane typy jednostek nie może być współużytkowana przez wiele właścicieli (jest to dobrze znanych scenariusz obiektów wartości, które nie mogą zostać zaimplementowane przy użyciu posiadane typy jednostek)</span><span class="sxs-lookup"><span data-stu-id="421f2-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="421f2-164">Braków w poprzednich wersjach</span><span class="sxs-lookup"><span data-stu-id="421f2-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="421f2-165">W programie EF Core 2.0 tego celu należące do typów jednostek nie można zadeklarować w typach pochodny jednostki, chyba że jednostki należące do firmy są jawnie mapowany do osobnej tabeli z hierarchii właściciela.</span><span class="sxs-lookup"><span data-stu-id="421f2-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="421f2-166">To ograniczenie zostało usunięte w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="421f2-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="421f2-167">W programie EF Core 2.0 i 2.1 odwoływać się tylko były obsługiwane tego typom należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="421f2-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="421f2-168">To ograniczenie zostało usunięte w programie EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="421f2-168">This limitation has been removed in EF Core 2.2</span></span>
