---
title: Nowe funkcje w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: a71aa01e81d9830d7b9e6cb01c200851100a15df
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628431"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nowe funkcje zawarte w EF Core 3,0 (obecnie w wersji zapoznawczej)

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, a mimo to, że firma Microsoft podejmie próbę zapewnienia aktualności tej strony, może nie odzwierciedlać naszych najnowszych planów przez cały czas.

Poniższa lista zawiera najważniejsze nowe funkcje planowane dla EF Core 3,0.
Większość z tych funkcji nie jest uwzględnionych w bieżącej wersji zapoznawczej, ale staje się dostępna w miarę postępu w kierunku do wersji RTM.

Przyczyną jest to, że na początku wydania koncentrujemy się na wdrażaniu planowanych [zmian](xref:core/what-is-new/ef-core-3.0/breaking-changes).
Wiele z tych istotnych zmian jest ulepszonych do EF Core.
Wiele innych jest wymaganych do odblokowania dalszych ulepszeń. 

Aby zapoznać się z pełną listą poprawek i ulepszeń błędów, można zobaczyć [to zapytanie w naszym monitorze problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Ulepszenia LINQ 

[Śledzenie problemu #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.

LINQ pozwala pisać zapytania bazy danych bez opuszczania wybranego języka, korzystając z informacji o typie rozbudowanym do pobierania funkcji IntelliSense i sprawdzania typu w czasie kompilacji.
Jednak LINQ pozwala także pisać nieograniczoną liczbę skomplikowanych zapytań, która zawsze była bardzo ogromnym wyzwaniem dla dostawców LINQ.
W kilku pierwszych wersjach EF Core firma Microsoft rozwiązała, że w części poprzez ustalenie, jakie fragmenty zapytania można przetłumaczyć na SQL, a następnie przez umożliwienie pozostałej części zapytania w pamięci na komputerze klienckim.
To wykonanie po stronie klienta może być pożądane w niektórych sytuacjach, ale w wielu innych przypadkach może to spowodować niewydajne zapytania, które mogą nie zostać zidentyfikowane do czasu wdrożenia aplikacji w środowisku produkcyjnym.
W EF Core 3,0 planujemy przeprowadzenia głębokich zmian w sposobie działania naszej implementacji LINQ i sposobach ich przetestowania.
Cele są bardziej niezawodne (na przykład w celu uniknięcia przerywania zapytań w wersjach poprawek), aby umożliwić poprawne tłumaczenie większej liczby wyrażeń na SQL, generowanie wydajnych zapytań w większej liczbie przypadków i zapobieganie wykryciu niewydajnych zapytań.

## <a name="cosmos-db-support"></a>Obsługa Cosmos DB 

[Śledzenie problemu #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Ta funkcja jest uwzględniona w bieżącej wersji zapoznawczej, ale nie została jeszcze ukończona. 

Pracujemy nad dostawcą Cosmos DB na potrzeby EF Core, aby umożliwić deweloperom zaznajomionym z modelem programowania EF łatwe kierowanie Azure Cosmos DB jako bazy danych aplikacji.
Celem jest, aby niektóre zalety Cosmos DB, takich jak dystrybucja globalna, "zawsze włączone" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.
Dostawca umożliwi korzystanie z większości funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w programie Cosmos DB.
Rozpocząłmy ten nakład pracy przed EF Core 2,2 i wprowadziliśmy [dostępne wersje zapoznawcze dostawcy](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Nowy plan polega na dalszym tworzeniu dostawcy wraz z EF Core 3,0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne

[Śledzenie problemu #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Ta funkcja zostanie wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.

Rozważmy następujący model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

Począwszy od EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zamapowana na tę samą tabelę, będzie możliwe dodanie `Order` bez `OrderDetails` i wszystkich `OrderDetails` właściwości, z wyjątkiem tego, że klucz podstawowy zostanie zmapowany na kolumny dopuszczające wartość null.

Podczas wykonywania zapytania, EF Core zostanie ustawiona `OrderDetails` na `null` , Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.

## <a name="c-80-support"></a>C#Obsługa 8,0

[](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
Problem ze śledzeniem #12047[śledzenia problemów #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.

Chcemy, aby nasi klienci korzystali z niektórych [nowych funkcji w C# 8,0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) takich jak strumienie asynchroniczne (w `await foreach`tym) i typy referencyjne dopuszczające wartość null podczas korzystania z EF Core.

## <a name="reverse-engineering-of-database-views"></a>Odtwarzanie widoków bazy danych

[Śledzenie problemu #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Ta funkcja nie jest uwzględniona w bieżącej wersji zapoznawczej.

[Typy zapytań](xref:core/modeling/query-types)wprowadzone w EF Core 2,1 i uznawane za typy jednostek bez kluczy w EF Core 3,0 reprezentują dane, które można odczytać z bazy danych, ale nie można ich zaktualizować.
Ta cecha sprawia, że najlepiej pasuje do widoków bazy danych w większości scenariuszy, dlatego planujemy zautomatyzować tworzenie typów jednostek bez kluczy podczas odtwarzania widoków bazy danych.

## <a name="property-bag-entities"></a>Jednostki zbioru właściwości

[Śledzenie problemów #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) i [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej. 

Ta funkcja służy do włączania jednostek, które przechowują dane we właściwościach indeksowanych zamiast zwykłych właściwości, a także z możliwością używania wystąpień tej samej klasy .NET (potencjalnie jako prostej `Dictionary<string, object>`jako) do reprezentowania różnych typów jednostek w tym samym modelu EF Core.
Ta funkcja to krok do obsługi relacji wiele-do-wielu bez jednostki sprzężenia ([#1368 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), która jest jednym z najbardziej żądanych ulepszeń EF Core.

## <a name="ef-63-on-net-core"></a>Dr 6,3 na platformie .NET Core

[Śledzenie problemu EF6 # 271](https://github.com/aspnet/EntityFramework6/issues/271)

Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej. 

Firma Microsoft zdaje sobie sprawę, że w wielu istniejących aplikacjach są używane poprzednie wersje EF, a ich przenoszenie do EF Core tylko w celu wykorzystania platformy .NET Core może czasami wymagać znacznego wysiłku.
Z tego powodu będziemy dostosowywać następną wersję EF 6 do uruchamiania na platformie .NET Core 3,0.
Robimy to w celu ułatwienia przenoszenia istniejących aplikacji przy minimalnych zmianach.
Istnieją pewne ograniczenia. Przykład:
- Będzie wymagała od nowych dostawców pracy z innymi bazami danych, poza uwzględnioną SQL Server obsługą platformy .NET Core
- Obsługa przestrzenna z SQL Server nie będzie włączona

Należy pamiętać, że na tym etapie nie są planowane żadne nowe funkcje programu EF 6.
