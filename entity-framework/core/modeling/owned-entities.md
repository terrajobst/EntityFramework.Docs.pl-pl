---
title: Typy jednostek posiadanych — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149132"
---
# <a name="owned-entity-types"></a><span data-ttu-id="106a5-102">Posiadane typy jednostek</span><span class="sxs-lookup"><span data-stu-id="106a5-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="106a5-103">Ta funkcja jest nowa w EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="106a5-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="106a5-104">EF Core umożliwia modelowanie typów jednostek, które mogą być wyświetlane tylko w przypadku właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="106a5-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="106a5-105">Są one nazywane _własnością typów jednostek_.</span><span class="sxs-lookup"><span data-stu-id="106a5-105">These are called _owned entity types_.</span></span> <span data-ttu-id="106a5-106">Jednostką zawierającą typ jednostki będącej właścicielem jest jej _właścicielem_.</span><span class="sxs-lookup"><span data-stu-id="106a5-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="106a5-107">Posiadane jednostki są zasadniczo częścią właściciela i nie mogą istnieć bez niej, są koncepcyjnie podobne do [agregacji](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="106a5-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="106a5-108">Konfiguracja jawna</span><span class="sxs-lookup"><span data-stu-id="106a5-108">Explicit configuration</span></span>

<span data-ttu-id="106a5-109">Typy jednostek będących własnością nie są nigdy uwzględniane przez EF Core w modelu według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="106a5-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="106a5-110">Możesz użyć `OwnsOne` metody w `OnModelCreating` lub dodać adnotację do typu z `OwnedAttribute` (Nowość w EF Core 2,1), aby skonfigurować typ jako typ będący własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="106a5-111">W tym przykładzie `StreetAddress` jest typem bez właściwości Identity.</span><span class="sxs-lookup"><span data-stu-id="106a5-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="106a5-112">Jest ona używana jako właściwość typu zamówienia, aby określić adres wysyłkowy dla konkretnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="106a5-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="106a5-113">Możemy użyć, `OwnedAttribute` aby traktować go jako jednostkę będącą własnością, gdy występuje odwołanie z innego typu jednostki:</span><span class="sxs-lookup"><span data-stu-id="106a5-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="106a5-114">Istnieje `OwnsOne` również możliwość użycia metody w `OnModelCreating` , aby określić, że `ShippingAddress` właściwość jest jednostką `Order` będącą własnością typu jednostki i aby skonfigurować dodatkowe aspekty w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="106a5-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="106a5-115">Jeśli właściwość jest prywatna `Order` w typie, można użyć wersji `OwnsOne` ciągu metody: `ShippingAddress`</span><span class="sxs-lookup"><span data-stu-id="106a5-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="106a5-116">Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) , aby uzyskać więcej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="106a5-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="106a5-117">Klucze niejawne</span><span class="sxs-lookup"><span data-stu-id="106a5-117">Implicit keys</span></span>

<span data-ttu-id="106a5-118">Typy własnością skonfigurowane z `OwnsOne` lub odnalezione za pomocą nawigacji odwołania zawsze mają relację jeden do jednego z właścicielem, dlatego nie potrzebują własnych wartości klucza, ponieważ wartości klucza obcego są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="106a5-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="106a5-119">W poprzednim przykładzie `StreetAddress` typ nie musi definiować właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="106a5-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="106a5-120">Aby zrozumieć, w jaki sposób EF Core śledzi te obiekty, warto wiedzieć, że klucz podstawowy został utworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) dla typu będącego własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="106a5-121">Wartość klucza wystąpienia typu będącego właścicielem będzie taka sama, jak wartość klucza wystąpienia właściciela.</span><span class="sxs-lookup"><span data-stu-id="106a5-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="106a5-122">Kolekcje typów posiadanych</span><span class="sxs-lookup"><span data-stu-id="106a5-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="106a5-123">Ta funkcja jest nowa w EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="106a5-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="106a5-124">Aby skonfigurować kolekcję typów będących własnością, `OwnsMany` Użyj `OnModelCreating`w.</span><span class="sxs-lookup"><span data-stu-id="106a5-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="106a5-125">Typy należące do muszą być kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="106a5-125">Owned types need a primary key.</span></span> <span data-ttu-id="106a5-126">Jeśli nie ma żadnych właściwości dobrych kandydatów dla typu .NET, EF Core może próbować utworzyć jeden.</span><span class="sxs-lookup"><span data-stu-id="106a5-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="106a5-127">Jednak jeśli typy posiadane są zdefiniowane za pomocą kolekcji, nie wystarczy tylko utworzyć właściwość Shadow do działania jako klucz obcy w właścicielu i kluczu podstawowym danego wystąpienia, jak w przypadku `OwnsOne`: może istnieć wiele wystąpień typu należącego do użytkownika Każdy właściciel, w związku z czym klucz właściciela nie wystarcza do zapewnienia unikatowej tożsamości dla każdego należącego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="106a5-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="106a5-128">Poniżej przedstawiono dwa najbardziej proste rozwiązania tego problemu:</span><span class="sxs-lookup"><span data-stu-id="106a5-128">The two most straightforward solutions to this are:</span></span>
- <span data-ttu-id="106a5-129">Definiowanie wieloskładnikowego klucza podstawowego na nowej właściwości niezależnie od klucza obcego, który wskazuje na właściciela.</span><span class="sxs-lookup"><span data-stu-id="106a5-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="106a5-130">Zawarte wartości muszą być unikatowe dla wszystkich właścicieli (na {1} przykład jeśli element nadrzędny ma element podrzędny {1}, a element nadrzędny {2} nie może mieć {1}elementu podrzędnego), więc wartość nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="106a5-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="106a5-131">Ponieważ klucz obcy nie jest częścią klucza podstawowego, jego wartości mogą być zmieniane, więc można przenieść element podrzędny z jednego elementu nadrzędnego do innego, ale zazwyczaj jest on zwykle związany z semantyką agregowania.</span><span class="sxs-lookup"><span data-stu-id="106a5-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="106a5-132">Użycie klucza obcego i dodatkowej właściwości jako klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="106a5-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="106a5-133">Dodatkowa wartość właściwości musi teraz być unikatowa dla danego elementu nadrzędnego (Jeśli element nadrzędny {1} ma element podrzędny {1,1} {2} , nadal może mieć element podrzędny {2,1}).</span><span class="sxs-lookup"><span data-stu-id="106a5-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="106a5-134">Dzięki wprowadzeniu części klucza obcego w kluczu podstawowym relacja między właścicielem a jednostką będącą własnością jest niezmienna i odzwierciedla lepszą semantykę.</span><span class="sxs-lookup"><span data-stu-id="106a5-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="106a5-135">Jest to EF Core domyślnie.</span><span class="sxs-lookup"><span data-stu-id="106a5-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="106a5-136">W tym przykładzie użyjemy `Distributor` klasy:</span><span class="sxs-lookup"><span data-stu-id="106a5-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="106a5-137">Domyślnie klucz podstawowy używany dla typu, do którego `ShippingCenters` odwołuje się właściwość nawigacji, będzie miał `("DistributorId", "Id")` , gdzie `"DistributorId"` jest kluczem klucza `"Id"` obcego i jest `int` wartością unikatową.</span><span class="sxs-lookup"><span data-stu-id="106a5-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="106a5-138">Aby skonfigurować inne wywołanie `HasKey`klucza podstawowego:</span><span class="sxs-lookup"><span data-stu-id="106a5-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="106a5-139">Przed EF Core 3,0 `WithOwner()` Metoda nie istnieje, więc należy usunąć to wywołanie.</span><span class="sxs-lookup"><span data-stu-id="106a5-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="106a5-140">Mapowanie typów posiadanych przy użyciu dzielenia tabeli</span><span class="sxs-lookup"><span data-stu-id="106a5-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="106a5-141">W przypadku korzystania z relacyjnych baz danych przez domyślne typy odwołań są mapowane na tę samą tabelę co właściciel.</span><span class="sxs-lookup"><span data-stu-id="106a5-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="106a5-142">Wymaga to podziału tabeli w dwóch: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych jednostki będącej własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="106a5-143">Jest to typowa funkcja znana jako [podział tabeli](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="106a5-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="106a5-144">Domyślnie EF Core nazwy kolumn bazy danych dla właściwości typu jednostki będącej własnością, po _Navigation_OwnedEntityProperty_wzorca.</span><span class="sxs-lookup"><span data-stu-id="106a5-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="106a5-145">W związku z tym właściwościbędąwyświetlanewtabeli"Orders"znazwami"ShippingAddress_Street"i"ShippingAddress_City".`StreetAddress`</span><span class="sxs-lookup"><span data-stu-id="106a5-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="106a5-146">Możesz użyć `HasColumnName` metody, aby zmienić nazwy tych kolumn:</span><span class="sxs-lookup"><span data-stu-id="106a5-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="106a5-147">Współużytkowanie tego samego typu .NET w wielu typach posiadanych</span><span class="sxs-lookup"><span data-stu-id="106a5-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="106a5-148">Typ jednostki będącej właścicielem może być tego samego typu .NET, co inny typ jednostki będącej własnością, dlatego typ .NET może być niewystarczający do zidentyfikowania typu będącego własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="106a5-149">W takich przypadkach Właściwość wskazująca, że obiekt będący właścicielem, jest _określany jako Nawigacja_ typu jednostki będącej własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="106a5-150">Z perspektywy EF Core, Definiowanie nawigacji jest częścią tożsamości typu obok typu .NET.</span><span class="sxs-lookup"><span data-stu-id="106a5-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="106a5-151">Na przykład w poniższej klasie `ShippingAddress` i `BillingAddress` są oba te same typy `StreetAddress`.NET:</span><span class="sxs-lookup"><span data-stu-id="106a5-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="106a5-152">Aby zrozumieć, w jaki sposób EF Core będzie odróżniać śledzone wystąpienia tych obiektów, warto zastanowić się, że definiująca Nawigacja stanie się częścią klucza wystąpienia obok wartości klucza właściciela i typu .NET należącego do typu.</span><span class="sxs-lookup"><span data-stu-id="106a5-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="106a5-153">Zagnieżdżone typy należące</span><span class="sxs-lookup"><span data-stu-id="106a5-153">Nested owned types</span></span>

<span data-ttu-id="106a5-154">W tym przykładzie `OrderDetails` są `BillingAddress` to `ShippingAddress`elementy należące do obu `StreetAddress` typów.</span><span class="sxs-lookup"><span data-stu-id="106a5-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="106a5-155">Następnie `OrderDetails`należy do typu.`DetailedOrder`</span><span class="sxs-lookup"><span data-stu-id="106a5-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="106a5-156">Oprócz zagnieżdżonych typów będących własnością, typ własnością może odwoływać się do zwykłej jednostki, może być właścicielem lub inną jednostką, o ile posiadana jednostka znajduje się na stronie zależnej.</span><span class="sxs-lookup"><span data-stu-id="106a5-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="106a5-157">Ta funkcja umożliwia określenie typów jednostek posiadanych poza typami złożonymi w EF6.</span><span class="sxs-lookup"><span data-stu-id="106a5-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="106a5-158">Istnieje możliwość łańcucha `OwnsOne` metody w wywołaniu Fluent, aby skonfigurować ten model:</span><span class="sxs-lookup"><span data-stu-id="106a5-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="106a5-159">Zwróć uwagę `WithOwner` na wywołanie użyte do skonfigurowania właściwości nawigacji wskazującej na właściciela.</span><span class="sxs-lookup"><span data-stu-id="106a5-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="106a5-160">Można osiągnąć wynik przy użyciu `OwnedAttribute` obu `OrderDetails` i `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="106a5-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="106a5-161">Przechowywanie typów posiadanych w oddzielnych tabelach</span><span class="sxs-lookup"><span data-stu-id="106a5-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="106a5-162">Również w przeciwieństwie do EF6 typów złożonych, typy własnością mogą być przechowywane w oddzielnej tabeli od właściciela.</span><span class="sxs-lookup"><span data-stu-id="106a5-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="106a5-163">W celu zastąpienia Konwencji, która mapuje typ należący do tej samej tabeli co właściciel, można po prostu wywołać `ToTable` i podać inną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="106a5-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="106a5-164">Poniższy przykład mapuje `OrderDetails` i jego dwa adresy na osobną tabelę z `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="106a5-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="106a5-165">Wykonywanie zapytania dotyczącego typów posiadanych</span><span class="sxs-lookup"><span data-stu-id="106a5-165">Querying owned types</span></span>

<span data-ttu-id="106a5-166">Podczas wykonywania zapytania dotyczącego właściciela typy będą uwzględniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="106a5-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="106a5-167">Nie jest konieczne używanie `Include` metody, nawet jeśli typy posiadane są przechowywane w oddzielnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="106a5-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="106a5-168">W oparciu o model opisany przed, otrzymasz następujące zapytanie `Order` `OrderDetails` i dwa należące `StreetAddresses` do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="106a5-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="106a5-169">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="106a5-169">Limitations</span></span>

<span data-ttu-id="106a5-170">Niektóre z tych ograniczeń mają wpływ na sposób działania typów jednostek, ale niektóre inne są ograniczeniami, które możemy usunąć w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="106a5-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="106a5-171">Ograniczenia przez projektowanie</span><span class="sxs-lookup"><span data-stu-id="106a5-171">By-design restrictions</span></span>
- <span data-ttu-id="106a5-172">Nie można utworzyć `DbSet<T>` dla typu będącego właścicielem</span><span class="sxs-lookup"><span data-stu-id="106a5-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="106a5-173">Nie można wywołać `Entity<T>()` z typem będącym własnością`ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="106a5-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="106a5-174">Bieżące nieprawidłowości</span><span class="sxs-lookup"><span data-stu-id="106a5-174">Current shortcomings</span></span>
- <span data-ttu-id="106a5-175">Hierarchie dziedziczenia, które zawierają typy jednostek będących własnością, nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="106a5-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="106a5-176">Nawigacja referencyjna do typów jednostek będących własnością nie może mieć wartości null, chyba że są jawnie zamapowane na osobną tabelę od właściciela</span><span class="sxs-lookup"><span data-stu-id="106a5-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="106a5-177">Wystąpienia typów jednostek będących własnością nie mogą być współużytkowane przez wielu właścicieli (jest to dobrze znany scenariusz dla obiektów wartości, których nie można zaimplementować przy użyciu posiadanych typów jednostek)</span><span class="sxs-lookup"><span data-stu-id="106a5-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="106a5-178">Braki w poprzednich wersjach</span><span class="sxs-lookup"><span data-stu-id="106a5-178">Shortcomings in previous versions</span></span>
- <span data-ttu-id="106a5-179">W EF Core 2,0 elementy nawigacyjne należące do typów jednostek posiadanych nie mogą być deklarowane w pochodnych typach jednostek, chyba że należące do niej jednostki są jawnie mapowane do oddzielnej tabeli z hierarchii właściciela.</span><span class="sxs-lookup"><span data-stu-id="106a5-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="106a5-180">To ograniczenie zostało usunięte w EF Core 2,1</span><span class="sxs-lookup"><span data-stu-id="106a5-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="106a5-181">W EF Core 2,0 i 2,1 obsługiwane są tylko nawigacji do typów będących własnością.</span><span class="sxs-lookup"><span data-stu-id="106a5-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="106a5-182">To ograniczenie zostało usunięte w EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="106a5-182">This limitation has been removed in EF Core 2.2</span></span>
