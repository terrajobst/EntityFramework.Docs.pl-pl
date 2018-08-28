---
title: Dziennik zmian wpływających na dostawcy — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998365"
---
# <a name="provider-impacting-changes"></a>Zmiany wpływające na dostawcy

Ta strona zawiera linki do przeprowadzanych na repozytorium programu EF Core, które mogą wymagać autorzy innych dostawców bazy danych, aby reagować żądań ściągnięcia. Jest do udostępniania punktu wyjściowego dla autorów istniejącego dostawcy baz danych innych firm, podczas aktualizowania ich dostawcy do nowej wersji.

Rozpoczynamy ten dziennik zmian z 2.1 do wersji 2.2. Przed 2.1 użyliśmy [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiety na nasze problemy i żądania ściągnięcia.

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>Zmiany tylko do testów

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Zezwalaj na możliwe do dostosowania ograniczników SQL w testach
  * Testowanie zmiany, które umożliwiają nieścisłym porównania punktu zmiennoprzecinkowego w BuiltInDataTypesTestBase
  * Zmiany testów, które umożliwiają zapytania testy, aby być ponownie używane, za pomocą różnych ograniczników SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Dodaj testy DbFunction testów specyfikacji relacyjnych
  * Taki sposób, że te testy mogą być uruchamiane względem wszystkich dostawców bazy danych
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Czyszczenie testowego przełączania do Async
  * Usuń `Wait` wywołań, niepotrzebne async i zmienić nazwy niektórych metod testowych
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Ujednolicenie infrastrukturę testowania rejestrowania
  * Dodano `CreateListLoggerFactory` i usunąć niektóre poprzedniego infrastruktury rejestrowania, który będzie wymagać dostawców przy użyciu tych testów, reakcji
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 — Uruchamianie więcej testów zapytania synchronicznie i asynchronicznie
  * Nazwy testów i uwzględniając uległ zmianie, co będzie wymagać dostawców przy użyciu tych testów, reakcji
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Zmiana nazwy tego modelu ComplexNavigations
  * Za pomocą tych testów dostawców może być konieczne reagowanie
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Zwracania kontekstu danej puli zamiast usuwania w testy funkcjonalne
  * Ta zmiana obejmuje niektóre refaktoryzacji testów, które mogą wymagać dostawców reagować


#### <a name="test-and-product-code-changes"></a>Test i produktu zmian w kodzie

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Skonsolidować RelationalTypeMapping.Clone metody
  * Zmiany w 2.1 RelationalTypeMapping dozwolone dla uproszczenia w klasach pochodnych. Firma Microsoft uważa, nie zostało to istotne do dostawców, ale dostawców korzystać z zalet tej zmiany w ich typ pochodny mapowania klas.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Oznakowane lub nazwanego zapytania
  * Dodaje infrastrukturę na potrzeby znakowania zapytań LINQ i posiadanie tych tagów, są wyświetlane jako komentarze w SQL. Może to wymagać dostawców reagowania generowanie kodu SQL.
