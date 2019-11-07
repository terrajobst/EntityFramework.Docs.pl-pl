---
title: Relacje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655675"
---
# <a name="relationships"></a>Relacje

Relacja określa, jak dwie jednostki są powiązane ze sobą. W relacyjnej bazie danych jest to reprezentowane przez ograniczenie klucza obcego.

> [!NOTE]  
> Większość przykładów w tym artykule używa relacji jeden-do-wielu, aby przedstawić koncepcje. Przykłady relacji jeden-do-jednego i wiele-do-wielu można znaleźć w sekcji [inne wzorce relacji](#other-relationship-patterns) na końcu artykułu.

## <a name="definition-of-terms"></a>Definicja warunków

Istnieje kilka terminów używanych do opisywania relacji

* **Jednostka zależna:** Jest to jednostka, która zawiera właściwości klucza obcego. Czasami określana jako "podrzędny" relacji.

* **Podmiot podmiotu zabezpieczeń:** Jest to jednostka, która zawiera właściwości klucza podstawowego/alternatywnego. Czasami określana jako "Parent" relacji.

* **Klucz obcy:** Właściwości w jednostce zależnej, która jest używana do przechowywania wartości właściwości klucza podmiotu, z którą jest powiązana jednostka.

* **Klucz podmiotu zabezpieczeń:** Właściwości, które jednoznacznie identyfikują jednostkę podmiotu zabezpieczeń. Może to być klucz podstawowy lub klucz alternatywny.

* **Właściwość nawigacji:** Właściwość zdefiniowana w jednostkach głównych i/lub zależnych, która zawiera odwołania do powiązanych jednostek.

  * **Właściwość nawigacji kolekcji:** Właściwość nawigacji, która zawiera odwołania do wielu powiązanych jednostek.

  * **Właściwość nawigacji odwołania:** Właściwość nawigacji, która zawiera odwołanie do pojedynczej powiązanej jednostki.

  * **Właściwość nawigacji odwrotnej:** Omawiając konkretną właściwość nawigacji, ten termin odnosi się do właściwości nawigacji na drugim końcu relacji.

Poniższa lista kodu przedstawia relację jeden do wielu między `Blog` i `Post`

* `Post` jest jednostką zależną

* `Blog` jest jednostką główną

* `Post.BlogId` jest kluczem obcym

* `Blog.BlogId` jest kluczem podmiotu zabezpieczeń (w tym przypadku jest to klucz podstawowy, a nie klucz alternatywny)

* `Post.Blog` jest właściwością nawigacji referencyjnej

* `Blog.Posts` jest właściwością nawigacji kolekcji

* `Post.Blog` jest właściwością nawigacji odwrotnej `Blog.Posts` (i odwrotnie)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją zostanie utworzona relacja, gdy w typie zostanie wykryta właściwość nawigacji. Właściwość jest uznawana za właściwość nawigacji, jeśli typ wskazujący nie może być mapowany jako typ skalarny przez bieżącego dostawcę bazy danych.

> [!NOTE]  
> Relacje, które są odnajdywane według Konwencji, będą zawsze ukierunkowane na klucz podstawowy jednostki głównej. Aby określić klucz alternatywny, należy przeprowadzić dodatkową konfigurację przy użyciu interfejsu API Fluent.

### <a name="fully-defined-relationships"></a>W pełni zdefiniowane relacje

Najbardziej typowym wzorcem relacji jest posiadanie właściwości nawigacji zdefiniowanych na obu końcach relacji i właściwości klucza obcego zdefiniowanej w klasie jednostki zależnej.

* Jeśli para właściwości nawigacji zostanie znaleziona między dwoma typami, zostaną one skonfigurowane jako właściwości nawigacji odwrotnej tej samej relacji.

* Jeśli jednostka zależna zawiera właściwość o nazwie `<primary key property name>`, `<navigation property name><primary key property name>`lub `<principal entity name><primary key property name>`, zostanie ona skonfigurowana jako klucz obcy.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Jeśli istnieje wiele właściwości nawigacji zdefiniowanych między dwoma typami (to jest, więcej niż jedna para elementów nawigacyjnych, które wskazują na siebie), nie zostaną utworzone żadne relacje w ramach Konwencji i trzeba będzie ręcznie skonfigurować je w celu określenia, jak pary właściwości nawigacji.

### <a name="no-foreign-key-property"></a>Brak właściwości klucza obcego

Chociaż zaleca się zdefiniowanie właściwości klucza obcego w klasie jednostki zależnej, nie jest to wymagane. Jeśli właściwość klucza obcego nie zostanie znaleziona, właściwość klucza obcego cienia zostanie wprowadzona z nazwą `<navigation property name><principal key property name>` (zobacz [Właściwości cienia](shadow-properties.md) , aby uzyskać więcej informacji).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Właściwość pojedynczej nawigacji

Dołączenie tylko jednej właściwości nawigacji (bez nawigacji odwrotnej i brak właściwości klucza obcego) nie wystarcza do zdefiniowania relacji przez Konwencję. Można również mieć pojedynczą właściwość nawigacji i właściwość klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Usuwanie kaskadowe

Według Konwencji kaskadowe usuwanie zostanie ustawione na *kaskadowe* dla wymaganych relacji i *ClientSetNull* dla relacji opcjonalnych. *Kaskadowo* oznacza również usunięcie jednostek zależnych. *ClientSetNull* oznacza, że jednostki zależne, które nie są ładowane do pamięci, pozostaną bez zmian i muszą zostać ręcznie usunięte lub zaktualizowane, aby wskazywały prawidłową jednostkę podmiotu zabezpieczeń. W przypadku jednostek, które są ładowane do pamięci, EF Core podejmie próbę ustawienia właściwości klucza obcego na wartość null.

Zapoznaj się z sekcją [wymagane i opcjonalne relacje](#required-and-optional-relationships) , aby uzyskać różnicę między wymaganymi i opcjonalnymi relacjami.

Zobacz [Kaskada Delete](../saving/cascade-delete.md) , aby uzyskać więcej informacji na temat różnych zachowań usuwania i ustawień domyślnych używanych przez Konwencję.

## <a name="data-annotations"></a>Adnotacje danych

Istnieją dwie adnotacje dotyczące danych, których można użyć do skonfigurowania relacji, `[ForeignKey]` i `[InverseProperty]`. Są one dostępne w przestrzeni nazw `System.ComponentModel.DataAnnotations.Schema`.

### <a name="foreignkey"></a>[ForeignKey]

Możesz użyć adnotacji danych, aby skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji. Jest to zazwyczaj wykonywane, gdy właściwość klucza obcego nie zostanie odnaleziona według Konwencji.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> Adnotacja `[ForeignKey]` może zostać umieszczona na każdej właściwości nawigacji w relacji. Nie musi ona być zgodna z właściwością nawigacji w klasie jednostki zależnej.

### <a name="inverseproperty"></a>[InverseProperty]

Możesz użyć adnotacji danych, aby skonfigurować sposób pary właściwości nawigacji dla jednostek zależnych i głównych. Jest to zazwyczaj wykonywane, gdy istnieje więcej niż jedna para właściwości nawigacji między dwoma typami jednostek.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Interfejs API Fluent

Aby skonfigurować relację w interfejsie API Fluent, należy rozpocząć od zidentyfikowania właściwości nawigacji, które tworzą relację. `HasOne` lub `HasMany` identyfikuje właściwość nawigacji dla typu jednostki, w którym rozpoczyna się konfiguracja. Następnie utworzysz łańcuch wywołań do `WithOne` lub `WithMany`, aby zidentyfikować nawigację odwrotną. `HasOne`/`WithOne` są używane do właściwości nawigacji referencyjnej i `HasMany`/`WithMany` są używane na potrzeby właściwości nawigacji kolekcji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Właściwość pojedynczej nawigacji

Jeśli masz tylko jedną właściwość nawigacji, istnieją bezparametryczne przeciążenia `WithOne` i `WithMany`. Oznacza to, że istnieje koncepcyjnie odwołanie lub kolekcja na drugim końcu relacji, ale nie ma żadnej właściwości nawigacji zawartej w klasie Entity.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Klucz obcy

Korzystając z interfejsu API Fluent, można skonfigurować właściwość, która powinna być używana jako właściwość klucza obcego dla danej relacji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

Poniższy list kodu przedstawia sposób konfigurowania złożonego klucza obcego.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Możesz użyć przeciążenia ciągu `HasForeignKey(...)`, aby skonfigurować właściwość Shadow jako klucz obcy (Aby uzyskać więcej informacji, zobacz [Właściwości cienia](shadow-properties.md) ). Zalecamy jawne dodanie właściwości Shadow do modelu przed użyciem go jako klucza obcego (jak pokazano poniżej).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Bez właściwości nawigacji

Nie trzeba podawać właściwości nawigacji. Możesz po prostu podać klucz obcy po jednej stronie relacji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Klucz podmiotu zabezpieczeń

Jeśli chcesz, aby klucz obcy odwołuje się do właściwości innej niż klucz podstawowy, możesz użyć interfejsu API Fluent, aby skonfigurować właściwość klucza podmiotu zabezpieczeń. Właściwość, którą konfigurujesz jako klucz podmiotu zabezpieczeń, zostanie automatycznie skonfigurowana jako klucz alternatywny (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

Poniższy list kodu przedstawia sposób konfigurowania złożonego klucza podmiotu zabezpieczeń.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> Kolejność, w której określane są właściwości klucza podmiotu zabezpieczeń, musi być zgodna z kolejnością, w której są określone dla klucza obcego.

### <a name="required-and-optional-relationships"></a>Wymagane i opcjonalne relacje

Korzystając z interfejsu API Fluent, można określić, czy relacja jest wymagana, czy opcjonalna. Ostatecznie to kontroluje, czy właściwość klucza obcego jest wymagana czy opcjonalna. Jest to najbardziej przydatne w przypadku używania klucza obcego stanu w tle. Jeśli masz właściwość klucza obcego w klasie jednostki, wymagana jest konieczność relacji w zależności od tego, czy właściwość klucza obcego jest wymagana, czy opcjonalna (zobacz [Właściwości wymagane i opcjonalne,](required-optional.md) Aby uzyskać więcej informacji).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>Usuwanie kaskadowe

Korzystając z interfejsu API Fluent, można jawnie skonfigurować zachowanie kaskadowego usuwania dla danej relacji.

Szczegółowe omówienie każdej z tych opcji można znaleźć w sekcji zapisywanie [kaskadowe](../saving/cascade-delete.md) dla zapisywania danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Inne wzorce relacji

### <a name="one-to-one"></a>Jeden do jednego

Relacja jeden do jednego ma właściwość nawigacji odwołania po obu stronach. Są one zgodne z tymi samymi konwencjami co relacje jeden-do-wielu, ale na właściwości klucza obcego jest wprowadzany unikatowy indeks, aby upewnić się, że tylko jedna z nich jest zależna od każdego podmiotu zabezpieczeń.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> Dr wybierze jedną z jednostek, która będzie zależna od możliwości wykrywania właściwości klucza obcego. W przypadku wybrania niewłaściwej jednostki jako zależnej można użyć interfejsu API Fluent, aby rozwiązać ten problem.

Podczas konfigurowania relacji z interfejsem API Fluent należy używać metod `HasOne` i `WithOne`.

Podczas konfigurowania klucza obcego należy określić typ podmiotu zależnego — Zwróć uwagę na parametr ogólny podany do `HasForeignKey` na poniższej liście. W relacji jeden-do-wielu jest jasne, że jednostka z nawigacją referencyjną jest zależna, a jeden z kolekcją jest podmiotem zabezpieczeń. Nie jest to jednak w relacji jeden-do-jednego — dlatego trzeba ją jawnie zdefiniować.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Wiele do wielu

Relacje wiele-do-wielu bez klasy Entity do reprezentowania tabeli sprzężenia nie są jeszcze obsługiwane. Istnieje jednak możliwość reprezentowania relacji wiele-do-wielu poprzez dołączenie klasy jednostki dla tabeli sprzężenia i mapowanie dwóch oddzielnych relacji jeden-do-wielu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
