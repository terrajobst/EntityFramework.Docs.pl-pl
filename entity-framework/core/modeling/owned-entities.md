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
# <a name="owned-entity-types"></a>Posiadane typy jednostek

>[!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.0.

EF Core umożliwia modelu typów jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek. Są to tak zwane _posiadane typy jednostek_. Obiekt zawierający typ jednostki należące do firmy jest jego _właściciela_.

## <a name="explicit-configuration"></a>Jawne konfiguracji

Własność jednostki typy nigdy nie znajdują się przez platformę EF Core w modelu przez Konwencję. Można użyć `OwnsOne` method in Class metoda `OnModelCreating` lub dodawać adnotacje do typu z `OwnedAttribute` (nowe w programie EF Core 2.1) Aby skonfigurować typ jako typ należące do firmy.

W tym przykładzie adres jest typem, za pomocą żadnej właściwości tożsamości. Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu zamówienia. W `OnModelCreating`, używamy `OwnsOne` metodę, aby określić, że właściwość ShippingAddress jest jednostki należące do typu zamówienia.

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

Jeśli właściwość ShippingAddress jest prywatna w typie kolejności, można użyć ciągu wersję `OwnsOne` metody:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

W tym przykładzie używamy `OwnedAttribute` do osiągnięcia tego samego celu:

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

## <a name="implicit-keys"></a>Niejawne kluczy

W programie EF Core 2.0 i 2.1 tylko odwołanie do właściwości nawigacji może wskazywać należących do typów. Kolekcji typów będących własnością nie są obsługiwane. Odwołanie do tych urządzeń będących własnością, typy zawsze muszą mieć relację jeden do jednego z właścicielem, dlatego nie potrzebują własne wartości klucza. W poprzednim przykładzie wpisz adres nie trzeba zdefiniować właściwość klucza.  

Aby dowiedzieć się, jak EF Core śledzi te obiekty, warto Pomyśl, czy klucz podstawowy został utworzony jako [w tle właściwość](xref:core/modeling/shadow-properties) należących do typu. Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.      

## <a name="mapping-owned-types-with-table-splitting"></a>Posiadane typy z dzielenia tabeli mapowania

Korzystając z relacyjnych baz danych, zgodnie z Konwencją należące do typów są mapowane na tej samej tabeli jako właściciel. Ta migracja wymaga dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki. Jest to typowe funkcje znane jako dzielenie tabeli.

> [!TIP]
> Posiadane typy przechowywane z dzielenia tabeli mogą być używane, bardzo podobnie jak złożonych typów są używane w EF6.

Zgodnie z Konwencją programu EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _EntityProperty_OwnedEntityProperty_. W związku z tym właściwości adres będą wyświetlane w tabeli zamówienia o nazwach ShippingAddress_Street i ShippingAddress_City.

Możesz dołączyć `HasColumnName` metodę, aby zmienić nazwę kolumny. W przypadku, gdy adres jest właściwością publiczną byłoby mapowania

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Udostępnianie tego samego typu .NET wśród wielu typów należące do firmy

Typ jednostki należące do firmy może być tego samego typu .NET jako inny typ jednostki należące do firmy, w związku z tym, że typ architektury .NET może nie być wystarczająca do identyfikowania typu należące do firmy.

W takich przypadkach staje się właściwość wskazuje od właściciela do jednostki należące do firmy _Definiowanie nawigacji_ typu jednostki należące do firmy. Z perspektywy programu EF Core definiujące Nawigacja jest częścią tożsamości typu wraz z typem platformy .NET.   

Na przykład w następującej klasy ShippingAddress i BillingAddress są tego samego typu .NET, adres:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Aby dowiedzieć się, jak EF Core pomoże wyróżnić śledzonych wystąpień tych obiektów, może być wyobrazić sobie, że definiujące nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET typu należące do firmy.

## <a name="nested-owned-types"></a>Zagnieżdżone typy należące do firmy

W tym przykładzie OrderDetails jest właścicielem BillingAddress i ShippingAddress, które są oba typy adres. Następnie OrderDetails jest własnością typ zamówienia.

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

Istnieje możliwość łańcucha `OwnsOne` metody w mapowaniu fluent, aby skonfigurować ten model:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Istnieje możliwość osiągnąć używając tej samej kolejności `OwnedAttribute` zarówno OrderDetails, jak i StreetAdress.

Oprócz zagnieżdżonych typów należące do firmy typem należące do firmy można odwoływać się do regularnego jednostki. W poniższym przykładzie kraj jest regularne jednostki należących do firmy:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Ta funkcja ustawia EF6 posiadane typy jednostek oprócz typów złożonych.

## <a name="storing-owned-types-in-separate-tables"></a>Przechowywanie należące do typów w oddzielnych tabelach

Również inaczej niż w przypadku typów złożonych EF6 posiadane typy mogą być przechowywane w osobnej tabeli od właściciela. Aby zastąpić Konwencji, która mapuje typ należących do tej samej tabeli jako właściciela, możesz po prostu wywołać `ToTable` i podaj inną nazwę tabeli. Poniższy przykład spowoduje mapowania OrderDetails i jego dwa adresy na osobnej tabeli z zamówienia:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a>Wykonywanie zapytania dotyczącego typów należące do firmy

Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie. Nie jest konieczne użycie `Include` metody, nawet jeśli posiadane typy są przechowywane w osobnej tabeli. Na podstawie modelu opisany wcześniej, następujące zapytanie będzie pobierać zamówienie, OrderDetails i dwa StreetAddresses należących do wszystkich zamówień oczekujące z bazy danych:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Ograniczenia

Niektórych z tych ograniczeń mają zasadnicze znaczenie jak należących do pracy typów jednostek, ale niektóre inne ograniczenia, że firma Microsoft może być niemożliwe do usunięcia w przyszłych wersjach:

### <a name="shortcomings-in-previous-versions"></a>Braków w poprzednich wersjach
- W programie EF Core 2.0 tego celu należące do typów jednostek nie można zadeklarować w typach pochodny jednostki, chyba że jednostki należące do firmy są jawnie mapowany do osobnej tabeli z hierarchii właściciela. To ograniczenie zostało usunięte w programie EF Core 2.1

### <a name="current-shortcomings"></a>Bieżący wad
- Hierarchii dziedziczenia, które obejmują posiadane typy jednostek nie są obsługiwane.
- Posiadane typy jednostek nie może być wskazywanej przez właściwości nawigacji kolekcji (tylko odwołanie do tego są aktualnie obsługiwane)
- Tego do typów jednostek, nie może być pusty, chyba że jawnie są mapowane na tabelę oddzielnych od właściciela
- Wystąpienia elementu posiadane typy jednostek nie może być współużytkowana przez wiele właścicieli (jest to dobrze znanych scenariusz obiektów wartości, które nie mogą zostać zaimplementowane przy użyciu posiadane typy jednostek)

### <a name="by-design-restrictions"></a>Ograniczenia według projektu
- Nie można utworzyć `DbSet<T>`
- Nie można wywołać `Entity<T>()` z typem należące do firmy na `ModelBuilder`
