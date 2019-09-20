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
# <a name="owned-entity-types"></a>Posiadane typy jednostek

>[!NOTE]
> Ta funkcja jest nowa w EF Core 2,0.

EF Core umożliwia modelowanie typów jednostek, które mogą być wyświetlane tylko w przypadku właściwości nawigacji innych typów jednostek. Są one nazywane _własnością typów jednostek_. Jednostką zawierającą typ jednostki będącej właścicielem jest jej _właścicielem_.

Posiadane jednostki są zasadniczo częścią właściciela i nie mogą istnieć bez niej, są koncepcyjnie podobne do [agregacji](https://martinfowler.com/bliki/DDD_Aggregate.html).

## <a name="explicit-configuration"></a>Konfiguracja jawna

Typy jednostek będących własnością nie są nigdy uwzględniane przez EF Core w modelu według Konwencji. Możesz użyć `OwnsOne` metody w `OnModelCreating` lub dodać adnotację do typu z `OwnedAttribute` (Nowość w EF Core 2,1), aby skonfigurować typ jako typ będący własnością.

W tym przykładzie `StreetAddress` jest typem bez właściwości Identity. Jest ona używana jako właściwość typu zamówienia, aby określić adres wysyłkowy dla konkretnej kolejności.

Możemy użyć, `OwnedAttribute` aby traktować go jako jednostkę będącą własnością, gdy występuje odwołanie z innego typu jednostki:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Istnieje `OwnsOne` również możliwość użycia metody w `OnModelCreating` , aby określić, że `ShippingAddress` właściwość jest jednostką `Order` będącą własnością typu jednostki i aby skonfigurować dodatkowe aspekty w razie potrzeby.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Jeśli właściwość jest prywatna `Order` w typie, można użyć wersji `OwnsOne` ciągu metody: `ShippingAddress`

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) , aby uzyskać więcej kontekstu. 

## <a name="implicit-keys"></a>Klucze niejawne

Typy własnością skonfigurowane z `OwnsOne` lub odnalezione za pomocą nawigacji odwołania zawsze mają relację jeden do jednego z właścicielem, dlatego nie potrzebują własnych wartości klucza, ponieważ wartości klucza obcego są unikatowe. W poprzednim przykładzie `StreetAddress` typ nie musi definiować właściwości klucza.  

Aby zrozumieć, w jaki sposób EF Core śledzi te obiekty, warto wiedzieć, że klucz podstawowy został utworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) dla typu będącego własnością. Wartość klucza wystąpienia typu będącego właścicielem będzie taka sama, jak wartość klucza wystąpienia właściciela.

## <a name="collections-of-owned-types"></a>Kolekcje typów posiadanych

> [!NOTE]
> Ta funkcja jest nowa w EF Core 2,2.

Aby skonfigurować kolekcję typów będących własnością, `OwnsMany` Użyj `OnModelCreating`w.

Typy należące do muszą być kluczem podstawowym. Jeśli nie ma żadnych właściwości dobrych kandydatów dla typu .NET, EF Core może próbować utworzyć jeden. Jednak jeśli typy posiadane są zdefiniowane za pomocą kolekcji, nie wystarczy tylko utworzyć właściwość Shadow do działania jako klucz obcy w właścicielu i kluczu podstawowym danego wystąpienia, jak w przypadku `OwnsOne`: może istnieć wiele wystąpień typu należącego do użytkownika Każdy właściciel, w związku z czym klucz właściciela nie wystarcza do zapewnienia unikatowej tożsamości dla każdego należącego wystąpienia.

Poniżej przedstawiono dwa najbardziej proste rozwiązania tego problemu:
- Definiowanie wieloskładnikowego klucza podstawowego na nowej właściwości niezależnie od klucza obcego, który wskazuje na właściciela. Zawarte wartości muszą być unikatowe dla wszystkich właścicieli (na {1} przykład jeśli element nadrzędny ma element podrzędny {1}, a element nadrzędny {2} nie może mieć {1}elementu podrzędnego), więc wartość nie ma znaczenia. Ponieważ klucz obcy nie jest częścią klucza podstawowego, jego wartości mogą być zmieniane, więc można przenieść element podrzędny z jednego elementu nadrzędnego do innego, ale zazwyczaj jest on zwykle związany z semantyką agregowania.
- Użycie klucza obcego i dodatkowej właściwości jako klucza złożonego. Dodatkowa wartość właściwości musi teraz być unikatowa dla danego elementu nadrzędnego (Jeśli element nadrzędny {1} ma element podrzędny {1,1} {2} , nadal może mieć element podrzędny {2,1}). Dzięki wprowadzeniu części klucza obcego w kluczu podstawowym relacja między właścicielem a jednostką będącą własnością jest niezmienna i odzwierciedla lepszą semantykę. Jest to EF Core domyślnie.

W tym przykładzie użyjemy `Distributor` klasy:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Domyślnie klucz podstawowy używany dla typu, do którego `ShippingCenters` odwołuje się właściwość nawigacji, będzie miał `("DistributorId", "Id")` , gdzie `"DistributorId"` jest kluczem klucza `"Id"` obcego i jest `int` wartością unikatową.

Aby skonfigurować inne wywołanie `HasKey`klucza podstawowego:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Przed EF Core 3,0 `WithOwner()` Metoda nie istnieje, więc należy usunąć to wywołanie.

## <a name="mapping-owned-types-with-table-splitting"></a>Mapowanie typów posiadanych przy użyciu dzielenia tabeli

W przypadku korzystania z relacyjnych baz danych przez domyślne typy odwołań są mapowane na tę samą tabelę co właściciel. Wymaga to podziału tabeli w dwóch: niektóre kolumny będą używane do przechowywania danych właściciela, a niektóre kolumny będą używane do przechowywania danych jednostki będącej własnością. Jest to typowa funkcja znana jako [podział tabeli](table-splitting.md).

Domyślnie EF Core nazwy kolumn bazy danych dla właściwości typu jednostki będącej własnością, po _Navigation_OwnedEntityProperty_wzorca. W związku z tym właściwościbędąwyświetlanewtabeli"Orders"znazwami"ShippingAddress_Street"i"ShippingAddress_City".`StreetAddress`

Możesz użyć `HasColumnName` metody, aby zmienić nazwy tych kolumn:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Współużytkowanie tego samego typu .NET w wielu typach posiadanych

Typ jednostki będącej właścicielem może być tego samego typu .NET, co inny typ jednostki będącej własnością, dlatego typ .NET może być niewystarczający do zidentyfikowania typu będącego własnością.

W takich przypadkach Właściwość wskazująca, że obiekt będący właścicielem, jest _określany jako Nawigacja_ typu jednostki będącej własnością. Z perspektywy EF Core, Definiowanie nawigacji jest częścią tożsamości typu obok typu .NET.   

Na przykład w poniższej klasie `ShippingAddress` i `BillingAddress` są oba te same typy `StreetAddress`.NET:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Aby zrozumieć, w jaki sposób EF Core będzie odróżniać śledzone wystąpienia tych obiektów, warto zastanowić się, że definiująca Nawigacja stanie się częścią klucza wystąpienia obok wartości klucza właściciela i typu .NET należącego do typu.

## <a name="nested-owned-types"></a>Zagnieżdżone typy należące

W tym przykładzie `OrderDetails` są `BillingAddress` to `ShippingAddress`elementy należące do obu `StreetAddress` typów. Następnie `OrderDetails`należy do typu.`DetailedOrder`

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Oprócz zagnieżdżonych typów będących własnością, typ własnością może odwoływać się do zwykłej jednostki, może być właścicielem lub inną jednostką, o ile posiadana jednostka znajduje się na stronie zależnej. Ta funkcja umożliwia określenie typów jednostek posiadanych poza typami złożonymi w EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Istnieje możliwość łańcucha `OwnsOne` metody w wywołaniu Fluent, aby skonfigurować ten model:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Zwróć uwagę `WithOwner` na wywołanie użyte do skonfigurowania właściwości nawigacji wskazującej na właściciela.

Można osiągnąć wynik przy użyciu `OwnedAttribute` obu `OrderDetails` i `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Przechowywanie typów posiadanych w oddzielnych tabelach

Również w przeciwieństwie do EF6 typów złożonych, typy własnością mogą być przechowywane w oddzielnej tabeli od właściciela. W celu zastąpienia Konwencji, która mapuje typ należący do tej samej tabeli co właściciel, można po prostu wywołać `ToTable` i podać inną nazwę tabeli. Poniższy przykład mapuje `OrderDetails` i jego dwa adresy na osobną tabelę z `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Wykonywanie zapytania dotyczącego typów posiadanych

Podczas wykonywania zapytania dotyczącego właściciela typy będą uwzględniane domyślnie. Nie jest konieczne używanie `Include` metody, nawet jeśli typy posiadane są przechowywane w oddzielnej tabeli. W oparciu o model opisany przed, otrzymasz następujące zapytanie `Order` `OrderDetails` i dwa należące `StreetAddresses` do bazy danych:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Ograniczenia

Niektóre z tych ograniczeń mają wpływ na sposób działania typów jednostek, ale niektóre inne są ograniczeniami, które możemy usunąć w przyszłych wersjach:

### <a name="by-design-restrictions"></a>Ograniczenia przez projektowanie
- Nie można utworzyć `DbSet<T>` dla typu będącego właścicielem
- Nie można wywołać `Entity<T>()` z typem będącym własnością`ModelBuilder`

### <a name="current-shortcomings"></a>Bieżące nieprawidłowości
- Hierarchie dziedziczenia, które zawierają typy jednostek będących własnością, nie są obsługiwane
- Nawigacja referencyjna do typów jednostek będących własnością nie może mieć wartości null, chyba że są jawnie zamapowane na osobną tabelę od właściciela
- Wystąpienia typów jednostek będących własnością nie mogą być współużytkowane przez wielu właścicieli (jest to dobrze znany scenariusz dla obiektów wartości, których nie można zaimplementować przy użyciu posiadanych typów jednostek)

### <a name="shortcomings-in-previous-versions"></a>Braki w poprzednich wersjach
- W EF Core 2,0 elementy nawigacyjne należące do typów jednostek posiadanych nie mogą być deklarowane w pochodnych typach jednostek, chyba że należące do niej jednostki są jawnie mapowane do oddzielnej tabeli z hierarchii właściciela. To ograniczenie zostało usunięte w EF Core 2,1
- W EF Core 2,0 i 2,1 obsługiwane są tylko nawigacji do typów będących własnością. To ograniczenie zostało usunięte w EF Core 2,2
