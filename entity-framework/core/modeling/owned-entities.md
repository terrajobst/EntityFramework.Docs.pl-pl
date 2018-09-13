---
title: Posiadane typy jednostek — EF Core
author: julielerman
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 1104a8a9a4540e33624fad69c47f2f950c6669bf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489417"
---
# <a name="owned-entity-types"></a><span data-ttu-id="bed04-102">Posiadane typy jednostek</span><span class="sxs-lookup"><span data-stu-id="bed04-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="bed04-103">Ta funkcja jest nowa w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bed04-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="bed04-104">EF Core umożliwia modelu typów jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="bed04-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="bed04-105">Są to tak zwane _posiadane typy jednostek_.</span><span class="sxs-lookup"><span data-stu-id="bed04-105">These are called _owned entity types_.</span></span> <span data-ttu-id="bed04-106">Obiekt zawierający typ jednostki należące do firmy jest jego _właściciela_.</span><span class="sxs-lookup"><span data-stu-id="bed04-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="bed04-107">Jawne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="bed04-107">Explicit configuration</span></span>

<span data-ttu-id="bed04-108">Własność jednostki typy nigdy nie znajdują się przez platformę EF Core w modelu przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="bed04-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="bed04-109">Można użyć `OwnsOne` method in Class metoda `OnModelCreating` lub dodawać adnotacje do typu z `OwnedAttribute` (nowe w programie EF Core 2.1) Aby skonfigurować typ jako typ należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="bed04-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="bed04-110">W tym przykładzie adres jest typem, za pomocą żadnej właściwości tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bed04-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="bed04-111">Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu zamówienia.</span><span class="sxs-lookup"><span data-stu-id="bed04-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="bed04-112">W `OnModelCreating`, używamy `OwnsOne` metodę, aby określić, że właściwość ShippingAddress jest jednostki należące do typu zamówienia.</span><span class="sxs-lookup"><span data-stu-id="bed04-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="bed04-113">Jeśli właściwość ShippingAddress jest prywatna w typie kolejności, można użyć ciągu wersję `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="bed04-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="bed04-114">W tym przykładzie używamy `OwnedAttribute` do osiągnięcia tego samego celu:</span><span class="sxs-lookup"><span data-stu-id="bed04-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="bed04-115">Niejawne kluczy</span><span class="sxs-lookup"><span data-stu-id="bed04-115">Implicit keys</span></span>

<span data-ttu-id="bed04-116">W programie EF Core 2.0 i 2.1 tylko odwołanie do właściwości nawigacji może wskazywać należących do typów.</span><span class="sxs-lookup"><span data-stu-id="bed04-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="bed04-117">Kolekcji typów będących własnością nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bed04-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="bed04-118">Odwołanie do tych urządzeń będących własnością, typy zawsze muszą mieć relację jeden do jednego z właścicielem, dlatego nie potrzebują własne wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="bed04-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="bed04-119">W poprzednim przykładzie wpisz adres nie trzeba zdefiniować właściwość klucza.</span><span class="sxs-lookup"><span data-stu-id="bed04-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="bed04-120">Aby dowiedzieć się, jak EF Core śledzi te obiekty, warto Pomyśl, czy klucz podstawowy został utworzony jako [w tle właściwość](xref:core/modeling/shadow-properties) należących do typu.</span><span class="sxs-lookup"><span data-stu-id="bed04-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="bed04-121">Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.</span><span class="sxs-lookup"><span data-stu-id="bed04-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="bed04-122">Posiadane typy z dzielenia tabeli mapowania</span><span class="sxs-lookup"><span data-stu-id="bed04-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="bed04-123">Korzystając z relacyjnych baz danych, zgodnie z Konwencją należące do typów są mapowane na tej samej tabeli jako właściciel.</span><span class="sxs-lookup"><span data-stu-id="bed04-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="bed04-124">Ta migracja wymaga dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki.</span><span class="sxs-lookup"><span data-stu-id="bed04-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="bed04-125">Jest to typowe funkcje znane jako dzielenie tabeli.</span><span class="sxs-lookup"><span data-stu-id="bed04-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="bed04-126">Posiadane typy przechowywane z dzielenia tabeli mogą być używane, bardzo podobnie jak złożonych typów są używane w EF6.</span><span class="sxs-lookup"><span data-stu-id="bed04-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="bed04-127">Zgodnie z Konwencją programu EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="bed04-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="bed04-128">W związku z tym właściwości adres będą wyświetlane w tabeli zamówienia o nazwach ShippingAddress_Street i ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="bed04-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="bed04-129">Możesz dołączyć `HasColumnName` metodę, aby zmienić nazwę kolumny.</span><span class="sxs-lookup"><span data-stu-id="bed04-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="bed04-130">W przypadku, gdy adres jest właściwością publiczną byłoby mapowania</span><span class="sxs-lookup"><span data-stu-id="bed04-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="bed04-131">Udostępnianie tego samego typu .NET wśród wielu typów należące do firmy</span><span class="sxs-lookup"><span data-stu-id="bed04-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="bed04-132">Typ jednostki należące do firmy może być tego samego typu .NET jako inny typ jednostki należące do firmy, w związku z tym, że typ architektury .NET może nie być wystarczająca do identyfikowania typu należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="bed04-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="bed04-133">W takich przypadkach staje się właściwość wskazuje od właściciela do jednostki należące do firmy _Definiowanie nawigacji_ typu jednostki należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="bed04-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="bed04-134">Z perspektywy programu EF Core definiujące Nawigacja jest częścią tożsamości typu wraz z typem platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bed04-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="bed04-135">Na przykład w następującej klasy ShippingAddress i BillingAddress są tego samego typu .NET, adres:</span><span class="sxs-lookup"><span data-stu-id="bed04-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="bed04-136">Aby dowiedzieć się, jak EF Core pomoże wyróżnić śledzonych wystąpień tych obiektów, może być wyobrazić sobie, że definiujące nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET typu należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="bed04-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="bed04-137">Zagnieżdżone typy należące do firmy</span><span class="sxs-lookup"><span data-stu-id="bed04-137">Nested owned types</span></span>

<span data-ttu-id="bed04-138">W tym przykładzie OrderDetails jest właścicielem BillingAddress i ShippingAddress, które są oba typy adres.</span><span class="sxs-lookup"><span data-stu-id="bed04-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="bed04-139">Następnie OrderDetails jest własnością typ zamówienia.</span><span class="sxs-lookup"><span data-stu-id="bed04-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="bed04-140">Istnieje możliwość łańcucha `OwnsOne` metody w mapowaniu fluent, aby skonfigurować ten model:</span><span class="sxs-lookup"><span data-stu-id="bed04-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="bed04-141">Istnieje możliwość osiągnąć używając tej samej kolejności `OwnedAttribute` zarówno OrderDetails, jak i StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="bed04-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="bed04-142">Oprócz zagnieżdżonych typów należące do firmy typem należące do firmy można odwoływać się do regularnego jednostki.</span><span class="sxs-lookup"><span data-stu-id="bed04-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="bed04-143">W poniższym przykładzie kraj jest regularne jednostki należących do firmy:</span><span class="sxs-lookup"><span data-stu-id="bed04-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="bed04-144">Ta funkcja ustawia EF6 posiadane typy jednostek oprócz typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="bed04-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="bed04-145">Przechowywanie należące do typów w oddzielnych tabelach</span><span class="sxs-lookup"><span data-stu-id="bed04-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="bed04-146">Również inaczej niż w przypadku typów złożonych EF6 posiadane typy mogą być przechowywane w osobnej tabeli od właściciela.</span><span class="sxs-lookup"><span data-stu-id="bed04-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="bed04-147">Aby zastąpić Konwencji, która mapuje typ należących do tej samej tabeli jako właściciela, możesz po prostu wywołać `ToTable` i podaj inną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="bed04-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="bed04-148">Poniższy przykład spowoduje mapowania OrderDetails i jego dwa adresy na osobnej tabeli z zamówienia:</span><span class="sxs-lookup"><span data-stu-id="bed04-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a><span data-ttu-id="bed04-149">Wykonywanie zapytania dotyczącego typów należące do firmy</span><span class="sxs-lookup"><span data-stu-id="bed04-149">Querying owned types</span></span>

<span data-ttu-id="bed04-150">Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="bed04-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="bed04-151">Nie jest konieczne użycie `Include` metody, nawet jeśli posiadane typy są przechowywane w osobnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="bed04-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="bed04-152">Na podstawie modelu opisany wcześniej, następujące zapytanie będzie pobierać zamówienie, OrderDetails i dwa StreetAddresses należących do wszystkich zamówień oczekujące z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="bed04-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="bed04-153">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="bed04-153">Limitations</span></span>

<span data-ttu-id="bed04-154">Niektórych z tych ograniczeń mają zasadnicze znaczenie jak należących do pracy typów jednostek, ale niektóre inne ograniczenia, że firma Microsoft może być niemożliwe do usunięcia w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="bed04-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="bed04-155">Braków w poprzednich wersjach</span><span class="sxs-lookup"><span data-stu-id="bed04-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="bed04-156">W programie EF Core 2.0 tego celu należące do typów jednostek nie można zadeklarować w typach pochodny jednostki, chyba że jednostki należące do firmy są jawnie mapowany do osobnej tabeli z hierarchii właściciela.</span><span class="sxs-lookup"><span data-stu-id="bed04-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="bed04-157">To ograniczenie zostało usunięte w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="bed04-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="bed04-158">Bieżący wad</span><span class="sxs-lookup"><span data-stu-id="bed04-158">Current shortcomings</span></span>
- <span data-ttu-id="bed04-159">Hierarchii dziedziczenia, które obejmują posiadane typy jednostek nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bed04-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="bed04-160">Posiadane typy jednostek nie może być wskazywanej przez właściwości nawigacji kolekcji (tylko odwołanie do tego są aktualnie obsługiwane)</span><span class="sxs-lookup"><span data-stu-id="bed04-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="bed04-161">Tego do typów jednostek, nie może być pusty, chyba że jawnie są mapowane na tabelę oddzielnych od właściciela</span><span class="sxs-lookup"><span data-stu-id="bed04-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="bed04-162">Wystąpienia elementu posiadane typy jednostek nie może być współużytkowana przez wiele właścicieli (jest to dobrze znanych scenariusz obiektów wartości, które nie mogą zostać zaimplementowane przy użyciu posiadane typy jednostek)</span><span class="sxs-lookup"><span data-stu-id="bed04-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="bed04-163">Ograniczenia według projektu</span><span class="sxs-lookup"><span data-stu-id="bed04-163">By-design restrictions</span></span>
- <span data-ttu-id="bed04-164">Nie można utworzyć `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="bed04-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="bed04-165">Nie można wywołać `Entity<T>()` z typem należące do firmy na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="bed04-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
