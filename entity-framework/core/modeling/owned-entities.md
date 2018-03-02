---
title: "Należące do typów jednostek - EF Core"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="ce61a-102">Typy jednostek należące do firmy</span><span class="sxs-lookup"><span data-stu-id="ce61a-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="ce61a-103">Ta funkcja jest nowa w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ce61a-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ce61a-104">Podstawowe EF umożliwia modelu typy jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="ce61a-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="ce61a-105">Są one nazywane _należące do typów jednostek_.</span><span class="sxs-lookup"><span data-stu-id="ce61a-105">These are called _owned entity types_.</span></span> <span data-ttu-id="ce61a-106">Obiekt zawierający typ jednostki będących własnością jest jego _właściciela_.</span><span class="sxs-lookup"><span data-stu-id="ce61a-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="ce61a-107">Jawne konfiguracji</span><span class="sxs-lookup"><span data-stu-id="ce61a-107">Explicit configuration</span></span>

<span data-ttu-id="ce61a-108">Należy do jednostki, które typy nigdy nie znajdują się w podstawowej EF w modelu przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ce61a-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="ce61a-109">Można użyć `OwnsOne` metody w `OnModelCreating` lub Dodaj adnotację do typu z `OwnedAttrbibute` (nowy 2.1 Core EF) do konfigurowania typu jako typu należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="ce61a-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="ce61a-110">W tym przykładzie adres jest typem z ma właściwości tożsamości.</span><span class="sxs-lookup"><span data-stu-id="ce61a-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="ce61a-111">Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce61a-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="ce61a-112">W `OnModelCreating`, używamy `OwnsOne` metodę, aby określić, że właściwość ShippingAddress jest własnością jednostki typu kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce61a-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="ce61a-113">Jeśli właściwość ShippingAddress jest prywatna w typie kolejności, można użyć ciągu `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="ce61a-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="ce61a-114">W tym przykładzie używamy `OwnedAttribute` na osiągnięcie tego samego celu:</span><span class="sxs-lookup"><span data-stu-id="ce61a-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="ce61a-115">Niejawne kluczy</span><span class="sxs-lookup"><span data-stu-id="ce61a-115">Implicit keys</span></span>

<span data-ttu-id="ce61a-116">W programie EF Core 2.0 i 2.1 tylko właściwości nawigacji odwołania może wskazywać należących do typów.</span><span class="sxs-lookup"><span data-stu-id="ce61a-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="ce61a-117">Kolekcji typów należące do firmy są nieobsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ce61a-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="ce61a-118">Te odwołania należące do typów zawsze relacją jeden do jednego z właścicielem, w związku z tym nie należy ich własnych wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="ce61a-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="ce61a-119">W poprzednim przykładzie wpisz adres nie trzeba zdefiniować właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="ce61a-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="ce61a-120">Aby zrozumieć, jak podstawowe EF śledzi te obiekty, warto podejrzenie, że klucz podstawowy został utworzony jako [w tle właściwości](xref:core/modeling/shadow-properties) należących do typu.</span><span class="sxs-lookup"><span data-stu-id="ce61a-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="ce61a-121">Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.</span><span class="sxs-lookup"><span data-stu-id="ce61a-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="ce61a-122">Mapowanie należące do typów z dzielenia tabeli</span><span class="sxs-lookup"><span data-stu-id="ce61a-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="ce61a-123">Korzystając z relacyjnych baz danych, przez Konwencję należące do typów są mapowane do tej samej tabeli jako właściciel.</span><span class="sxs-lookup"><span data-stu-id="ce61a-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="ce61a-124">Wymaga to dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki.</span><span class="sxs-lookup"><span data-stu-id="ce61a-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="ce61a-125">To jest typową funkcją znany jako dzielenia tabeli.</span><span class="sxs-lookup"><span data-stu-id="ce61a-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="ce61a-126">Należące do typów przechowywane z dzielenia tabeli może być używany do bardzo podobnie jak złożonych typów są używane w EF6.</span><span class="sxs-lookup"><span data-stu-id="ce61a-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="ce61a-127">Według Konwencji EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="ce61a-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="ce61a-128">W związku z tym właściwości adres będą wyświetlane w tabeli poleceń o nazwach ShippingAddress_Street i ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="ce61a-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="ce61a-129">Możesz dołączyć `HasColumnName` metodę, aby zmienić nazwę kolumny.</span><span class="sxs-lookup"><span data-stu-id="ce61a-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="ce61a-130">W przypadku, gdzie adres jest właściwością publiczną będą mapowania</span><span class="sxs-lookup"><span data-stu-id="ce61a-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="ce61a-131">Udostępnianie między wiele typów należących do tego samego typu .NET</span><span class="sxs-lookup"><span data-stu-id="ce61a-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="ce61a-132">Typ jednostki należące do firmy może być typu .NET jako inny typ należących do jednostki, w związku z tym się, że typ architektury .NET może nie być wystarczająca do identyfikacji należących do typu.</span><span class="sxs-lookup"><span data-stu-id="ce61a-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="ce61a-133">W takich przypadkach staje się właściwość wskazuje od właściciela należących do jednostki _Definiowanie nawigacji_ typu należących do jednostki.</span><span class="sxs-lookup"><span data-stu-id="ce61a-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="ce61a-134">Z perspektywy EF Core definiującego nawigacji jest częścią tożsamości typu obok typ architektury .NET.</span><span class="sxs-lookup"><span data-stu-id="ce61a-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="ce61a-135">Na przykład w następującej klasy ShippingAddress i BillingAddress są tego samego typu .NET, adres:</span><span class="sxs-lookup"><span data-stu-id="ce61a-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="ce61a-136">Aby zrozumieć, jak podstawowe EF ma odróżniać śledzonych wystąpień tych obiektów, warto wziąć pod uwagę że definiującego nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET należących do typu.</span><span class="sxs-lookup"><span data-stu-id="ce61a-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="ce61a-137">Zagnieżdżone typy należące do firmy</span><span class="sxs-lookup"><span data-stu-id="ce61a-137">Nested owned types</span></span>

<span data-ttu-id="ce61a-138">W tym przykładzie SzczegółyZamówień właścicielem BillingAddress i ShippingAddress, które są oba typy adres.</span><span class="sxs-lookup"><span data-stu-id="ce61a-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="ce61a-139">Następnie SzczegółyZamówień właścicielem jest typ zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ce61a-139">Then OrderDetails is owned by the Order type.</span></span>

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="ce61a-140">Istnieje możliwość łańcucha `OwnsOne` metody fluent mapowanie do konfigurowania tego modelu:</span><span class="sxs-lookup"><span data-stu-id="ce61a-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="ce61a-141">Istnieje możliwość uzyskania tego samego przy użyciu złego `OwnedAttribute` na SzczegółyZamówienia i StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="ce61a-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="ce61a-142">Oprócz zagnieżdżonych typów należących do typu należące do firmy można odwoływać się do regularnych jednostki.</span><span class="sxs-lookup"><span data-stu-id="ce61a-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="ce61a-143">W poniższym przykładzie kraju jest regularne jednostki (tj. nie jest właścicielem):</span><span class="sxs-lookup"><span data-stu-id="ce61a-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="ce61a-144">Ta funkcja ustawia typy jednostek będących własnością oprócz typy złożone w EF6.</span><span class="sxs-lookup"><span data-stu-id="ce61a-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="ce61a-145">Przechowywanie należące do typów w oddzielnych tabelach</span><span class="sxs-lookup"><span data-stu-id="ce61a-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="ce61a-146">Również w odróżnieniu od EF6 złożonych typów należących do typów mogą być przechowywane w osobnej tabeli od właściciela.</span><span class="sxs-lookup"><span data-stu-id="ce61a-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="ce61a-147">Aby zastąpić Konwencji, który mapuje typ należących do tej samej tabeli co właściciela, należy po prostu wywołać `ToTable` i podaj inną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="ce61a-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="ce61a-148">Poniższy przykład przypisze SzczegółyZamówień i jego dwa adresy do osobnej tabeli z kolejności:</span><span class="sxs-lookup"><span data-stu-id="ce61a-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="ce61a-149">Wykonywanie zapytania należących do typów</span><span class="sxs-lookup"><span data-stu-id="ce61a-149">Querying owned types</span></span>

<span data-ttu-id="ce61a-150">Podczas wykonywania zapytania właściciela należących do typów zostaną uwzględnione domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ce61a-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="ce61a-151">Nie jest konieczne użycie `Include` metody, nawet jeśli typy należące do firmy są przechowywane w osobnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ce61a-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="ce61a-152">Następujące zapytanie oparte na modelu opisany wcześniej, będzie pobierać kolejności, SzczegółyZamówień i dwa StreeAddresses należących do wszystkich oczekujących zamówień z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ce61a-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="ce61a-153">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="ce61a-153">Limitations</span></span>

<span data-ttu-id="ce61a-154">Poniżej przedstawiono kilka ograniczeń typów jednostek należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="ce61a-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="ce61a-155">Niektóre z tych ograniczeń są niezbędne, aby jak należących do pracy typów, ale niektóre inne ograniczenia w momencie, które chcemy, aby usunąć w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="ce61a-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="ce61a-156">Bieżący niedociągnięć</span><span class="sxs-lookup"><span data-stu-id="ce61a-156">Current shortcomings</span></span>
- <span data-ttu-id="ce61a-157">Dziedziczenie należących do typów nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ce61a-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="ce61a-158">Należących do typów nie może być wskazywanego przez właściwości nawigacji kolekcji</span><span class="sxs-lookup"><span data-stu-id="ce61a-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="ce61a-159">Ponieważ korzystają z tabeli dzielenie przez domyślny należące do typów również mają następujące ograniczenia, chyba że jawnie mapowany do innej tabeli:</span><span class="sxs-lookup"><span data-stu-id="ce61a-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="ce61a-160">Nie mogą należeć do typu pochodnego</span><span class="sxs-lookup"><span data-stu-id="ce61a-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="ce61a-161">Definiowanie właściwości nawigacji nie można ustawić wartości null (tj. należące do typów w tej samej tabeli zawsze są wymagane)</span><span class="sxs-lookup"><span data-stu-id="ce61a-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="ce61a-162">Ograniczenia w projekcie</span><span class="sxs-lookup"><span data-stu-id="ce61a-162">By-design restrictions</span></span>
- <span data-ttu-id="ce61a-163">Nie można utworzyć `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="ce61a-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="ce61a-164">Nie można wywołać `Entity<T>()` należących do typu na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="ce61a-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
