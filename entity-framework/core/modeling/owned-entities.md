---
title: Posiadane typy jednostek — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069207"
---
# <a name="owned-entity-types"></a>Posiadane typy jednostek

>[!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.0.

EF Core umożliwia modelu typów jednostek, które mogą być wyświetlane tylko dla właściwości nawigacji innych typów jednostek. Są to tak zwane _posiadane typy jednostek_. Obiekt zawierający typ jednostki należące do firmy jest jego _właściciela_.

## <a name="explicit-configuration"></a>Jawne konfiguracji

Własność jednostki typy nigdy nie znajdują się przez platformę EF Core w modelu przez Konwencję. Można użyć `OwnsOne` method in Class metoda `OnModelCreating` lub dodawać adnotacje do typu z `OwnedAttribute` (nowe w programie EF Core 2.1) Aby skonfigurować typ jako typ należące do firmy.

W tym przykładzie `StreetAddress` jest typem przy użyciu żadnej właściwości tożsamości. Aby określić adres wysyłkowy dla określonej kolejności służy jako właściwość typu zamówienia.

Możemy użyć `OwnedAttribute` traktowanie jej jako należących do jednostki, gdy występuje do innego typu jednostki:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Istnieje również możliwość użycia `OwnsOne` method in Class metoda `OnModelCreating` Aby określić, że `ShippingAddress` właściwość jest własnością jednostki `Order` typu jednostki i skonfigurować dodatkowe aspekty, jeśli to konieczne.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Jeśli `ShippingAddress` właściwość jest prywatna w `Order` typu, można użyć ciągu wersję `OwnsOne` metody:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) Aby uzyskać dodatkowy kontekst. 

## <a name="implicit-keys"></a>Niejawne kluczy

Posiadane typy skonfigurowano `OwnsOne` lub odnalezione za pomocą nawigacji odwołania zawsze mieć relację jeden do jednego z właścicielem, dlatego nie potrzebują własne wartości klucza jako wartości klucza obcego są unikatowe. W poprzednim przykładzie `StreetAddress` typu nie trzeba zdefiniować właściwość klucza.  

Aby dowiedzieć się, jak EF Core śledzi te obiekty, warto Pomyśl, czy klucz podstawowy został utworzony jako [w tle właściwość](xref:core/modeling/shadow-properties) należących do typu. Wartość klucza wystąpienia typu należące do firmy będzie taka sama jak wartość klucza wystąpienia właściciela.

## <a name="collections-of-owned-types"></a>Kolekcji typów będących własnością

>[!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.2.

Aby skonfigurować kolekcji typów będących własnością `OwnsMany` powinny być używane w `OnModelCreating`. Jednak klucza podstawowego nie zostanie skonfigurowany zgodnie z Konwencją, dlatego musi być jawnie określony. Powszechne jest wprowadzanie klucza złożonego na użytek tych typów jednostek, zawierające klucz obcy do właściciela i dodatkowe unikatowa właściwość, która również może być w stanie w tle:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Posiadane typy z dzielenia tabeli mapowania

Korzystając z relacyjnych baz danych, według Konwencji odwołania należące do typów są mapowane na tej samej tabeli jako właściciel. Ta migracja wymaga dzielenia tabeli na dwie: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych należących do jednostki. Jest to typowe funkcje znane jako dzielenie tabeli.

> [!TIP]
> Posiadane typy przechowywane z dzielenia tabeli mogą być używane, podobnie jak złożonych typów są używane w EF6.

Zgodnie z Konwencją programu EF Core będzie nazwa kolumny bazy danych dla właściwości typu jednostki należące do firmy, zgodnie ze wzorcem _Navigation_OwnedEntityProperty_. W związku z tym `StreetAddress` właściwości będą wyświetlane w tabeli "Zamówienia" o nazwach "ShippingAddress_Street" i "ShippingAddress_City".

Możesz dołączyć `HasColumnName` metodę, aby zmienić te kolumny:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Udostępnianie tego samego typu .NET wśród wielu typów należące do firmy

Typ jednostki należące do firmy może być tego samego typu .NET jako inny typ jednostki należące do firmy, w związku z tym, że typ architektury .NET może nie być wystarczająca do identyfikowania typu należące do firmy.

W takich przypadkach staje się właściwość wskazuje od właściciela do jednostki należące do firmy _Definiowanie nawigacji_ typu jednostki należące do firmy. Z perspektywy programu EF Core definiujące Nawigacja jest częścią tożsamości typu wraz z typem platformy .NET.   

Na przykład w następującej klasy `ShippingAddress` i `BillingAddress` są tego samego typu .NET `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Aby dowiedzieć się, jak EF Core pomoże wyróżnić śledzonych wystąpień tych obiektów, może być wyobrazić sobie, że definiujące nawigacji stał się częścią klucza wystąpienia obok wartości klucza właściciela i typ architektury .NET typu należące do firmy.

## <a name="nested-owned-types"></a>Zagnieżdżone typy należące do firmy

W tym przykładzie `OrderDetails` właścicielem `BillingAddress` i `ShippingAddress`, które są zarówno `StreetAddress` typów. Następnie `OrderDetails` jest własnością `DetailedOrder` typu.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Oprócz zagnieżdżonych typów należące do firmy typem należące do firmy można odwoływać się do regularnego jednostki, tak długo, jak należących do jednostki znajduje się na stronie zależne może być właścicielem lub innej jednostki. Ta funkcja ustawia EF6 posiadane typy jednostek oprócz typów złożonych.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Istnieje możliwość łańcucha `OwnsOne` metody w wywołaniu fluent, aby skonfigurować ten model:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Istnieje również możliwość można osiągnąć używając tej samej kolejności `OwnedAttribute` zarówno `OrderDetails` i `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Przechowywanie należące do typów w oddzielnych tabelach

Również inaczej niż w przypadku typów złożonych EF6 posiadane typy mogą być przechowywane w osobnej tabeli od właściciela. Aby zastąpić Konwencji, która mapuje typ należących do tej samej tabeli jako właściciela, możesz po prostu wywołać `ToTable` i podaj inną nazwę tabeli. Poniższy przykład zmapuje `OrderDetails` i jego dwa adresy do osobnej tabeli z `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Wykonywanie zapytania dotyczącego typów należące do firmy

Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie. Nie jest konieczne użycie `Include` metody, nawet jeśli posiadane typy są przechowywane w osobnej tabeli. Na podstawie modelu opisany wcześniej, następujące zapytanie zwróci `Order`, `OrderDetails` a dwa należące do `StreetAddresses` z bazy danych:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Ograniczenia

Niektórych z tych ograniczeń mają zasadnicze znaczenie jak należących do pracy typów jednostek, ale niektóre inne ograniczenia, że firma Microsoft może być niemożliwe do usunięcia w przyszłych wersjach:

### <a name="by-design-restrictions"></a>Ograniczenia według projektu
- Nie można utworzyć `DbSet<T>` dla typu należące do firmy
- Nie można wywołać `Entity<T>()` z typem należące do firmy na `ModelBuilder`

### <a name="current-shortcomings"></a>Bieżący wad
- Hierarchii dziedziczenia, które obejmują posiadane typy jednostek nie są obsługiwane.
- Odwołanie do tego celu posiadane typy jednostek nie może być pusty, chyba że jawnie są mapowane na tabelę oddzielnych od właściciela
- Wystąpienia elementu posiadane typy jednostek nie może być współużytkowana przez wiele właścicieli (jest to dobrze znanych scenariusz obiektów wartości, które nie mogą zostać zaimplementowane przy użyciu posiadane typy jednostek)

### <a name="shortcomings-in-previous-versions"></a>Braków w poprzednich wersjach
- W programie EF Core 2.0 tego celu należące do typów jednostek nie można zadeklarować w typach pochodny jednostki, chyba że jednostki należące do firmy są jawnie mapowany do osobnej tabeli z hierarchii właściciela. To ograniczenie zostało usunięte w programie EF Core 2.1
- W programie EF Core 2.0 i 2.1 odwoływać się tylko były obsługiwane tego typom należące do firmy. To ograniczenie zostało usunięte w programie EF Core 2.2
