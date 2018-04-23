---
title: Należące do typów jednostek - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a>Typy jednostek należące do firmy

>[!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.0.

Podstawowe EF umożliwia modelu typy jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek. Są one nazywane _należące do typów jednostek_. Obiekt zawierający typ jednostki będących własnością jest jego _właściciela_.

## <a name="explicit-configuration"></a>Jawne konfiguracji

Należy do jednostki, które typy nigdy nie znajdują się w podstawowej EF w modelu przez Konwencję. Można użyć `OwnsOne` metody w `OnModelCreating` lub Dodaj adnotację do typu z `OwnedAttribute` (nowy 2.1 Core EF) do konfigurowania typu jako typu należące do firmy.

W tym przykładzie adres jest typem z ma właściwości tożsamości. Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu kolejności. W `OnModelCreating`, używamy `OwnsOne` metodę, aby określić, że właściwość ShippingAddress jest własnością jednostki typu kolejności.

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

Jeśli właściwość ShippingAddress jest prywatna w typie kolejności, można użyć ciągu `OwnsOne` metody:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

W tym przykładzie używamy `OwnedAttribute` na osiągnięcie tego samego celu:

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

W programie EF Core 2.0 i 2.1 tylko właściwości nawigacji odwołania może wskazywać należących do typów. Kolekcji typów należące do firmy są nieobsługiwane. Te odwołania należące do typów zawsze relacją jeden do jednego z właścicielem, w związku z tym nie należy ich własnych wartości klucza. W poprzednim przykładzie wpisz adres nie trzeba zdefiniować właściwości klucza.  

Aby zrozumieć, jak podstawowe EF śledzi te obiekty, warto podejrzenie, że klucz podstawowy został utworzony jako [w tle właściwości](xref:core/modeling/shadow-properties) należących do typu. Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapowanie należące do typów z dzielenia tabeli

Korzystając z relacyjnych baz danych, przez Konwencję należące do typów są mapowane do tej samej tabeli jako właściciel. Wymaga to dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki. To jest typową funkcją znany jako dzielenia tabeli.

> [!TIP]
> Należące do typów przechowywane z dzielenia tabeli może być używany do bardzo podobnie jak złożonych typów są używane w EF6.

Według Konwencji EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _EntityProperty_OwnedEntityProperty_. W związku z tym właściwości adres będą wyświetlane w tabeli poleceń o nazwach ShippingAddress_Street i ShippingAddress_City.

Możesz dołączyć `HasColumnName` metodę, aby zmienić nazwę kolumny. W przypadku, gdzie adres jest właściwością publiczną będą mapowania

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Udostępnianie między wiele typów należących do tego samego typu .NET

Typ jednostki należące do firmy może być typu .NET jako inny typ należących do jednostki, w związku z tym się, że typ architektury .NET może nie być wystarczająca do identyfikacji należących do typu.

W takich przypadkach staje się właściwość wskazuje od właściciela należących do jednostki _Definiowanie nawigacji_ typu należących do jednostki. Z perspektywy EF Core definiującego nawigacji jest częścią tożsamości typu obok typ architektury .NET.   

Na przykład w następującej klasy ShippingAddress i BillingAddress są tego samego typu .NET, adres:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Aby zrozumieć, jak podstawowe EF ma odróżniać śledzonych wystąpień tych obiektów, warto wziąć pod uwagę że definiującego nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET należących do typu.

## <a name="nested-owned-types"></a>Zagnieżdżone typy należące do firmy

W tym przykładzie SzczegółyZamówień właścicielem BillingAddress i ShippingAddress, które są oba typy adres. Następnie SzczegółyZamówień właścicielem jest typ zamówienia.

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

Istnieje możliwość łańcucha `OwnsOne` metody fluent mapowanie do konfigurowania tego modelu:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Istnieje możliwość uzyskania tego samego przy użyciu złego `OwnedAttribute` na SzczegółyZamówienia i StreetAdress.

Oprócz zagnieżdżonych typów należących do typu należące do firmy można odwoływać się do regularnych jednostki. W poniższym przykładzie kraju jest regularne jednostki (tj. nie jest właścicielem):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Ta funkcja ustawia typy jednostek będących własnością oprócz typy złożone w EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Przechowywanie należące do typów w oddzielnych tabelach

Również w odróżnieniu od EF6 złożonych typów należących do typów mogą być przechowywane w osobnej tabeli od właściciela. Aby zastąpić Konwencji, który mapuje typ należących do tej samej tabeli co właściciela, należy po prostu wywołać `ToTable` i podaj inną nazwę tabeli. Poniższy przykład przypisze SzczegółyZamówień i jego dwa adresy do osobnej tabeli z kolejności:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Wykonywanie zapytania należących do typów

Podczas wykonywania zapytania właściciela należących do typów zostaną uwzględnione domyślnie. Nie jest konieczne użycie `Include` metody, nawet jeśli typy należące do firmy są przechowywane w osobnej tabeli. Następujące zapytanie oparte na modelu opisany wcześniej, będzie pobierać kolejności, SzczegółyZamówień i dwa StreeAddresses należących do wszystkich oczekujących zamówień z bazy danych:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Ograniczenia

Poniżej przedstawiono kilka ograniczeń typów jednostek należące do firmy. Niektóre z tych ograniczeń są niezbędne, aby jak należących do pracy typów, ale niektóre inne ograniczenia w momencie, które chcemy, aby usunąć w przyszłych wersjach:

### <a name="current-shortcomings"></a>Bieżący niedociągnięć
- Dziedziczenie należących do typów nie jest obsługiwane.
- Należących do typów nie może być wskazywanego przez właściwości nawigacji kolekcji
- Ponieważ korzystają z tabeli dzielenie przez domyślny należące do typów również mają następujące ograniczenia, chyba że jawnie mapowany do innej tabeli:
   - Nie mogą należeć do typu pochodnego
   - Definiowanie właściwości nawigacji nie można ustawić wartości null (tj. należące do typów w tej samej tabeli zawsze są wymagane)

### <a name="by-design-restrictions"></a>Ograniczenia w projekcie
- Nie można utworzyć `DbSet<T>`
- Nie można wywołać `Entity<T>()` należących do typu na `ModelBuilder`
