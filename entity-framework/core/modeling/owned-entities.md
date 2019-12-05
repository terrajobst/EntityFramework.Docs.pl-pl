---
title: Typy jednostek posiadanych — EF Core
description: Jak skonfigurować własne typy jednostek lub agregacje podczas korzystania z Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 7b6d1b3bccbfceb85f03a580ba03a45984d29c74
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824597"
---
# <a name="owned-entity-types"></a><span data-ttu-id="92d6f-103">Posiadane typy jednostek</span><span class="sxs-lookup"><span data-stu-id="92d6f-103">Owned Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="92d6f-104">Ta funkcja jest nowa w EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="92d6f-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="92d6f-105">EF Core umożliwia modelowanie typów jednostek, które mogą być wyświetlane tylko w przypadku właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="92d6f-105">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="92d6f-106">Są one nazywane _własnością typów jednostek_.</span><span class="sxs-lookup"><span data-stu-id="92d6f-106">These are called _owned entity types_.</span></span> <span data-ttu-id="92d6f-107">Jednostką zawierającą typ jednostki będącej właścicielem jest jej _właścicielem_.</span><span class="sxs-lookup"><span data-stu-id="92d6f-107">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="92d6f-108">Posiadane jednostki są zasadniczo częścią właściciela i nie mogą istnieć bez niej, są koncepcyjnie podobne do [agregacji](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="92d6f-108">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span> <span data-ttu-id="92d6f-109">Oznacza to, że typ własnością jest z definicji po stronie zależnej relacji z właścicielem.</span><span class="sxs-lookup"><span data-stu-id="92d6f-109">This means that the owned type is by definition on the dependent side of the relationship with the owner.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="92d6f-110">Konfiguracja jawna</span><span class="sxs-lookup"><span data-stu-id="92d6f-110">Explicit configuration</span></span>

<span data-ttu-id="92d6f-111">Typy jednostek będących własnością nie są nigdy uwzględniane przez EF Core w modelu według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="92d6f-111">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="92d6f-112">Można użyć metody `OwnsOne` w `OnModelCreating` lub dodać adnotację typu z `OwnedAttribute` (Nowość w EF Core 2,1), aby skonfigurować typ jako typ będący własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-112">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="92d6f-113">W tym przykładzie `StreetAddress` jest typem bez właściwości Identity.</span><span class="sxs-lookup"><span data-stu-id="92d6f-113">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="92d6f-114">Jest ona używana jako właściwość typu zamówienia, aby określić adres wysyłkowy dla konkretnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="92d6f-114">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="92d6f-115">Możemy użyć `OwnedAttribute`, aby traktować go jako jednostkę będącą własnością, gdy występuje odwołanie z innego typu jednostki:</span><span class="sxs-lookup"><span data-stu-id="92d6f-115">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="92d6f-116">Możliwe jest również użycie metody `OwnsOne` w `OnModelCreating`, aby określić, że właściwość `ShippingAddress` jest jednostką będącą własnością typu jednostki `Order` i aby skonfigurować dodatkowe aspekty w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="92d6f-116">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="92d6f-117">Jeśli właściwość `ShippingAddress` jest prywatna w typie `Order`, można użyć wersji ciągu `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="92d6f-117">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="92d6f-118">Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) , aby uzyskać więcej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="92d6f-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="92d6f-119">Klucze niejawne</span><span class="sxs-lookup"><span data-stu-id="92d6f-119">Implicit keys</span></span>

<span data-ttu-id="92d6f-120">Typy posiadane skonfigurowane z `OwnsOne` lub odnajdywane za pomocą nawigacji referencyjnej zawsze mają relację jeden do jednego z właścicielem, dlatego nie potrzebują własnych wartości klucza, ponieważ wartości klucza obcego są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="92d6f-120">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="92d6f-121">W poprzednim przykładzie typ `StreetAddress` nie musi definiować właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="92d6f-121">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="92d6f-122">Aby zrozumieć, w jaki sposób EF Core śledzi te obiekty, warto wiedzieć, że klucz podstawowy został utworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) dla typu będącego własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-122">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="92d6f-123">Wartość klucza wystąpienia typu będącego właścicielem będzie taka sama, jak wartość klucza wystąpienia właściciela.</span><span class="sxs-lookup"><span data-stu-id="92d6f-123">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="92d6f-124">Kolekcje typów posiadanych</span><span class="sxs-lookup"><span data-stu-id="92d6f-124">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="92d6f-125">Ta funkcja jest nowa w EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="92d6f-125">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="92d6f-126">Aby skonfigurować kolekcję typów posiadanych, użyj `OwnsMany` w `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="92d6f-126">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="92d6f-127">Typy należące do muszą być kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="92d6f-127">Owned types need a primary key.</span></span> <span data-ttu-id="92d6f-128">Jeśli nie ma żadnych właściwości dobrych kandydatów dla typu .NET, EF Core może próbować utworzyć jeden.</span><span class="sxs-lookup"><span data-stu-id="92d6f-128">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="92d6f-129">Jednak jeśli typy posiadane są zdefiniowane za pomocą kolekcji, nie wystarczy tylko utworzyć właściwość Shadow do działania jako klucz obcy w właścicielu i kluczu podstawowym danego wystąpienia, jak w przypadku `OwnsOne`: może istnieć wiele wystąpień typu należącego do użytkownika. właściciel, a w związku z tym klucz właściciela nie wystarcza do zapewnienia unikatowej tożsamości dla każdego należącego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="92d6f-129">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="92d6f-130">Poniżej przedstawiono dwa najbardziej proste rozwiązania tego problemu:</span><span class="sxs-lookup"><span data-stu-id="92d6f-130">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="92d6f-131">Definiowanie wieloskładnikowego klucza podstawowego na nowej właściwości niezależnie od klucza obcego, który wskazuje na właściciela.</span><span class="sxs-lookup"><span data-stu-id="92d6f-131">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="92d6f-132">Zawarte wartości powinny być unikatowe dla wszystkich właścicieli (np. Jeśli nadrzędna {1} ma {1}podrzędne, {2} nadrzędne nie może mieć {1}podrzędnego), więc wartość nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="92d6f-132">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="92d6f-133">Ponieważ klucz obcy nie jest częścią klucza podstawowego, jego wartości mogą być zmieniane, więc można przenieść element podrzędny z jednego elementu nadrzędnego do innego, ale zazwyczaj jest on zwykle związany z semantyką agregowania.</span><span class="sxs-lookup"><span data-stu-id="92d6f-133">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="92d6f-134">Użycie klucza obcego i dodatkowej właściwości jako klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="92d6f-134">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="92d6f-135">Dodatkowa wartość właściwości musi teraz być unikatowa dla danego elementu nadrzędnego (w przypadku, gdy {1} nadrzędny ma {1,1} podrzędnego, {2} nadrzędne nadal mogą mieć {2,1}podrzędne).</span><span class="sxs-lookup"><span data-stu-id="92d6f-135">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="92d6f-136">Dzięki wprowadzeniu części klucza obcego w kluczu podstawowym relacja między właścicielem a jednostką będącą własnością jest niezmienna i odzwierciedla lepszą semantykę.</span><span class="sxs-lookup"><span data-stu-id="92d6f-136">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="92d6f-137">Jest to EF Core domyślnie.</span><span class="sxs-lookup"><span data-stu-id="92d6f-137">This is what EF Core does by default.</span></span>

<span data-ttu-id="92d6f-138">W tym przykładzie użyjemy klasy `Distributor`:</span><span class="sxs-lookup"><span data-stu-id="92d6f-138">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="92d6f-139">Domyślnie klucz podstawowy używany do tego typu, do którego odwołuje się właściwość nawigacji `ShippingCenters`, będzie `("DistributorId", "Id")`, gdzie `"DistributorId"` jest kluczem obcy, a `"Id"` jest unikatową wartością `int`.</span><span class="sxs-lookup"><span data-stu-id="92d6f-139">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="92d6f-140">Aby skonfigurować inne `HasKey`wywołań PK:</span><span class="sxs-lookup"><span data-stu-id="92d6f-140">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="92d6f-141">Przed EF Core 3,0 `WithOwner()` Metoda nie istnieje, dlatego należy usunąć to wywołanie.</span><span class="sxs-lookup"><span data-stu-id="92d6f-141">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span> <span data-ttu-id="92d6f-142">Ponadto klucz podstawowy nie został odnaleziony automatycznie, więc zawsze został określony.</span><span class="sxs-lookup"><span data-stu-id="92d6f-142">Also the primary key was not discovered automatically so it always had be specified.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="92d6f-143">Mapowanie typów posiadanych przy użyciu dzielenia tabeli</span><span class="sxs-lookup"><span data-stu-id="92d6f-143">Mapping owned types with table splitting</span></span>

<span data-ttu-id="92d6f-144">W przypadku korzystania z relacyjnych baz danych przez domyślne typy odwołań są mapowane na tę samą tabelę co właściciel.</span><span class="sxs-lookup"><span data-stu-id="92d6f-144">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="92d6f-145">Wymaga to podziału tabeli w dwóch: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych jednostki będącej własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-145">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="92d6f-146">Jest to typowa funkcja znana jako [podział tabeli](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="92d6f-146">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="92d6f-147">Domyślnie EF Core nazwy kolumn bazy danych dla właściwości typu jednostki będącej własnością, po _Navigation_OwnedEntityProperty_wzorca.</span><span class="sxs-lookup"><span data-stu-id="92d6f-147">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="92d6f-148">W związku z tym właściwości `StreetAddress` będą wyświetlane w tabeli "Orders" z nazwami "ShippingAddress_Street" i "ShippingAddress_City".</span><span class="sxs-lookup"><span data-stu-id="92d6f-148">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="92d6f-149">Za pomocą metody `HasColumnName` można zmienić nazwy tych kolumn:</span><span class="sxs-lookup"><span data-stu-id="92d6f-149">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> <span data-ttu-id="92d6f-150">Większość normalnych metod konfiguracji typu jednostki, takich jak [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) , można wywołać w taki sam sposób.</span><span class="sxs-lookup"><span data-stu-id="92d6f-150">Most of the normal entity type configuration methods like [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) can be called in the same way.</span></span>

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="92d6f-151">Współużytkowanie tego samego typu .NET w wielu typach posiadanych</span><span class="sxs-lookup"><span data-stu-id="92d6f-151">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="92d6f-152">Typ jednostki będącej właścicielem może być tego samego typu .NET, co inny typ jednostki będącej własnością, dlatego typ .NET może być niewystarczający do zidentyfikowania typu będącego własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-152">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="92d6f-153">W takich przypadkach Właściwość wskazująca, że obiekt będący właścicielem, jest _określany jako Nawigacja_ typu jednostki będącej własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-153">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="92d6f-154">Z perspektywy EF Core, Definiowanie nawigacji jest częścią tożsamości typu obok typu .NET.</span><span class="sxs-lookup"><span data-stu-id="92d6f-154">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="92d6f-155">Na przykład w poniższej klasie `ShippingAddress` i `BillingAddress` są oba te same typy .NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="92d6f-155">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="92d6f-156">Aby zrozumieć, w jaki sposób EF Core będzie odróżniać śledzone wystąpienia tych obiektów, warto zastanowić się, że definiująca Nawigacja stanie się częścią klucza wystąpienia obok wartości klucza właściciela i typu .NET należącego do typu.</span><span class="sxs-lookup"><span data-stu-id="92d6f-156">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="92d6f-157">Zagnieżdżone typy należące</span><span class="sxs-lookup"><span data-stu-id="92d6f-157">Nested owned types</span></span>

<span data-ttu-id="92d6f-158">W tym przykładzie `OrderDetails` należące do `BillingAddress` i `ShippingAddress`, które są oba typy `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="92d6f-158">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="92d6f-159">A następnie `OrderDetails` należy do typu `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="92d6f-159">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="92d6f-160">Każda Nawigacja do typu posiadanego definiuje osobny typ jednostki z całkowicie niezależną konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="92d6f-160">Each navigation to an owned type defines a separate entity type with completely independent configuration.</span></span>

<span data-ttu-id="92d6f-161">Oprócz zagnieżdżonych typów będących własnością, typ własnością może odwoływać się do zwykłej jednostki, może być właścicielem lub inną jednostką, o ile posiadana jednostka znajduje się na stronie zależnej.</span><span class="sxs-lookup"><span data-stu-id="92d6f-161">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="92d6f-162">Ta funkcja umożliwia określenie typów jednostek posiadanych poza typami złożonymi w EF6.</span><span class="sxs-lookup"><span data-stu-id="92d6f-162">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="92d6f-163">Istnieje możliwość łańcucha metody `OwnsOne` w wywołaniu Fluent, aby skonfigurować ten model:</span><span class="sxs-lookup"><span data-stu-id="92d6f-163">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="92d6f-164">Zwróć uwagę na wywołanie `WithOwner` użyte do skonfigurowania właściwości nawigacji wskazującej na właściciela.</span><span class="sxs-lookup"><span data-stu-id="92d6f-164">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span> <span data-ttu-id="92d6f-165">Aby skonfigurować nawigację do typu jednostki właściciela, który nie jest częścią relacji własności `WithOwner()` należy wywołać bez żadnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="92d6f-165">To configure a navigation to the owner entity type that's not part of the ownership relationship `WithOwner()` should be called without any arguments.</span></span>

<span data-ttu-id="92d6f-166">Można osiągnąć wynik przy użyciu `OwnedAttribute` na obu `OrderDetails` i `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="92d6f-166">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAddress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="92d6f-167">Przechowywanie typów posiadanych w oddzielnych tabelach</span><span class="sxs-lookup"><span data-stu-id="92d6f-167">Storing owned types in separate tables</span></span>

<span data-ttu-id="92d6f-168">Również w przeciwieństwie do EF6 typów złożonych, typy własnością mogą być przechowywane w oddzielnej tabeli od właściciela.</span><span class="sxs-lookup"><span data-stu-id="92d6f-168">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="92d6f-169">W celu przesłonięcia Konwencji, która mapuje typ należący do tej samej tabeli co właściciel, można po prostu wywołać `ToTable` i podać inną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="92d6f-169">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="92d6f-170">Poniższy przykład mapuje `OrderDetails` i jego dwa adresy w oddzielną tabelę od `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="92d6f-170">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

<span data-ttu-id="92d6f-171">W tym celu można również użyć `TableAttribute`, ale należy zauważyć, że może to się nie powieść, jeśli istnieje wiele nawigacji do tego typu, ponieważ w takim przypadku wiele typów jednostek będzie mapowanych do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="92d6f-171">It is also possible to use the `TableAttribute` to accomplish this, but note that this would fail if there are multiple navigations to the owned type since in that case multiple entity types would be mapped to the same table.</span></span>

## <a name="querying-owned-types"></a><span data-ttu-id="92d6f-172">Wykonywanie zapytania dotyczącego typów posiadanych</span><span class="sxs-lookup"><span data-stu-id="92d6f-172">Querying owned types</span></span>

<span data-ttu-id="92d6f-173">Podczas wykonywania zapytania dotyczącego właściciela typy będą uwzględniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="92d6f-173">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="92d6f-174">Nie jest konieczne używanie metody `Include`, nawet jeśli typy posiadane są przechowywane w oddzielnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="92d6f-174">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="92d6f-175">W oparciu o model opisany przed, następujące zapytanie uzyska `Order`, `OrderDetails` i dwa należące do `StreetAddresses` z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="92d6f-175">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="92d6f-176">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="92d6f-176">Limitations</span></span>

<span data-ttu-id="92d6f-177">Niektóre z tych ograniczeń mają wpływ na sposób działania typów jednostek, ale niektóre inne są ograniczeniami, które możemy usunąć w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="92d6f-177">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="92d6f-178">Ograniczenia przez projektowanie</span><span class="sxs-lookup"><span data-stu-id="92d6f-178">By-design restrictions</span></span>

- <span data-ttu-id="92d6f-179">Nie można utworzyć `DbSet<T>` dla typu będącego własnością</span><span class="sxs-lookup"><span data-stu-id="92d6f-179">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="92d6f-180">Nie można wywołać `Entity<T>()` z typem posiadanym na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="92d6f-180">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="92d6f-181">Bieżące nieprawidłowości</span><span class="sxs-lookup"><span data-stu-id="92d6f-181">Current shortcomings</span></span>

- <span data-ttu-id="92d6f-182">Typy jednostek będących własnością nie mogą mieć hierarchii dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="92d6f-182">Owned entity types cannot have inheritance hierarchies</span></span>
- <span data-ttu-id="92d6f-183">Nawigacja referencyjna do typów jednostek będących własnością nie może mieć wartości null, chyba że są jawnie zamapowane na osobną tabelę od właściciela</span><span class="sxs-lookup"><span data-stu-id="92d6f-183">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="92d6f-184">Wystąpienia typów jednostek będących własnością nie mogą być współużytkowane przez wielu właścicieli (jest to dobrze znany scenariusz dla obiektów wartości, których nie można zaimplementować przy użyciu posiadanych typów jednostek)</span><span class="sxs-lookup"><span data-stu-id="92d6f-184">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="92d6f-185">Braki w poprzednich wersjach</span><span class="sxs-lookup"><span data-stu-id="92d6f-185">Shortcomings in previous versions</span></span>

- <span data-ttu-id="92d6f-186">W EF Core 2,0 elementy nawigacyjne należące do typów jednostek posiadanych nie mogą być deklarowane w pochodnych typach jednostek, chyba że należące do niej jednostki są jawnie mapowane do oddzielnej tabeli z hierarchii właściciela.</span><span class="sxs-lookup"><span data-stu-id="92d6f-186">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="92d6f-187">To ograniczenie zostało usunięte w EF Core 2,1</span><span class="sxs-lookup"><span data-stu-id="92d6f-187">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="92d6f-188">W EF Core 2,0 i 2,1 obsługiwane są tylko nawigacji do typów będących własnością.</span><span class="sxs-lookup"><span data-stu-id="92d6f-188">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="92d6f-189">To ograniczenie zostało usunięte w EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="92d6f-189">This limitation has been removed in EF Core 2.2</span></span>
