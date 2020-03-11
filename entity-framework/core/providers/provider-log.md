---
title: Dziennik zmian wpływających na dostawcę — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417823"
---
# <a name="provider-impacting-changes"></a>Zmiany wpływające na dostawcę

Ta strona zawiera linki do żądań ściągnięcia wykonanych w repozytorium EF Core, które mogą wymagać od autorów innych dostawców baz danych. Zamiarem jest zapewnienie punktu wyjścia dla autorów istniejących baz danych innych firm podczas aktualizowania dostawcy do nowej wersji.

Uruchamiamy ten dziennik z zmianami z 2,1 do 2,2. Przed 2,1 korzystamy z [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiet dotyczących problemów i żądań ściągnięcia.

## <a name="22-----30"></a>2.2 ---> 3.0

Należy zauważyć, że wiele [zmian na poziomie aplikacji](../what-is-new/ef-core-3.0/breaking-changes.md) również ma wpływ na dostawców.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * Usunięto przestarzałe interfejsy API i zwinięte przeciążenia parametrów opcjonalnych
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * Usunięto przestarzałe interfejsy API
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * Podklasy CharTypeMapping mogły zostać przerwane ze względu na zmiany zachowania wymagane do naprawienia kilku usterek w implementacji podstawowej.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * Dodano klasę bazową dla IDatabaseModelFactory i Zaktualizowano ją tak, aby korzystała z obiektu parametr w celu ograniczenia przyszłych podziałów.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * Użycie obiektów parametrów w MigrationsSqlGenerator, aby wyeliminować przyszłe przerwy.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * Jawna konfiguracja poziomów dziennika wymagała pewnych zmian w interfejsach API, których mogą używać dostawcy. W odróżnieniu od tego, czy dostawcy używają infrastruktury rejestrowania bezpośrednio, ta zmiana może spowodować jej uszkodzenie. Ponadto dostawcy, którzy korzystają z infrastruktury (która będzie publiczna), będą musieli uzyskać od `LoggingDefinitions` lub `RelationalLoggingDefinitions`. Przykłady można znaleźć w SQL Server i dostawcach znajdujących się w pamięci.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * Ciągi zasobów podstawowe, relacyjne i abstrakcji są teraz publiczne.
  * `CoreLoggerExtensions` i `RelationalLoggerExtensions` są teraz publiczne. Dostawcy powinni używać tych interfejsów API podczas rejestrowania zdarzeń, które są zdefiniowane na poziomie podstawowym lub relacyjnym. Nie uzyskuj dostępu do zasobów rejestrowania bezpośrednio; są one nadal wewnętrzne.
  * `IRawSqlCommandBuilder` zmieniono z usługi pojedynczej na usługę objętą zakresem
  * `IMigrationsSqlGenerator` zmieniono z usługi pojedynczej na usługę objętą zakresem
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * Infrastruktura do tworzenia poleceń relacyjnych została udostępniona publicznie, więc może być bezpiecznie używana przez dostawców i nieznacznie rozbudowana.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * `ILazyLoader` zmienił się z usługi w zakresie na przejściową
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * `IUpdateSqlGenerator` zmieniono z usługi w zakresie na pojedynczą usługę
  * Ponadto `ISingletonUpdateSqlGenerator` został usunięty
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * Wiele kodów wewnętrznych, które były używane przez dostawców, została teraz udostępniona jako publiczna
  * Nie powinien już być necssary do odwoływania się do `IndentedStringBuilder`, ponieważ został on rozstawiony z miejsc, które go wybrały
  * Użycie `NonCapturingLazyInitializer` należy zastąpić `LazyInitializer` z BCL
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * Ta zmiana jest w pełni objęta dokumentem zmiany w aplikacji. W przypadku dostawców może to mieć większy wpływ, ponieważ testowanie rdzenia EF może często skutkować tym problemem, dlatego infrastruktura testowa została zmieniona w taki sposób, aby było to mniej możliwe.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` został uproszczony
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * Translacja StartsWith zmieniła się w sposób, w jaki dostawcy mogą potrzebować/potrzebować do reagowania
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * Usługi zestawu Konwencji zostały zmienione. Dostawcy powinni teraz dziedziczyć z obu "ProviderConventionSet" lub "RelationalConventionSet".
  * Dostosowania można dodać za pomocą usług `IConventionSetCustomizer`, ale jest to przeznaczone do użycia przez inne rozszerzenia, a nie dostawców.
  * Konwencje używane w czasie wykonywania powinny zostać rozwiązane z `IConventionSetBuilder`.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * Umieszczanie danych zostało rozsiane w publicznym interfejsie API, aby uniknąć konieczności używania typów wewnętrznych. Ma to wpływ tylko na dostawców nierelacyjnych, ponieważ umieszczanie jest obsługiwane przez podstawową klasę relacyjną dla wszystkich dostawców relacyjnych.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Zmiany wyłącznie testowe

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057> — Zezwalaj na dostosowywalne delimeters języka SQL w testach
  * Zmiany testowe, które zezwalają na nieścisłe porównania zmiennoprzecinkowe w BuiltInDataTypesTestBase
  * Zmiany testowe, które zezwalają na ponowne używanie testów zapytania z różnymi delimeters SQL
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072>-Dodawanie testów funkcji dbdo testów specyfikacji relacyjnej
  * Takie testy mogą być uruchamiane dla wszystkich dostawców baz danych
* <https://github.com/aspnet/EntityFrameworkCore/pull/12362> — oczyszczanie testów asynchronicznych
  * Usuwanie wywołań `Wait`, niepotrzebnych Async i nazw niektórych metod testowych
* infrastruktura testowa rejestrowania <https://github.com/aspnet/EntityFrameworkCore/pull/12666>-ujednolicania
  * Dodano `CreateListLoggerFactory` i usunięto część poprzedniej infrastruktury rejestrowania, która będzie wymagała dostawców korzystających z tych testów w celu reagowania
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500> — wykonywanie większej liczby testów zapytań synchronicznie i asynchronicznie
  * Zmieniono nazwy i operacje refaktoryzacji, które będą wymagały dostawców korzystających z tych testów w celu reagowania
* <https://github.com/aspnet/EntityFrameworkCore/pull/12766> zmianę nazwy nawigacji w modelu ComplexNavigations
  * Dostawcy korzystający z tych testów mogą potrzebować reakcji
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141> — zwracanie kontekstu do puli zamiast usuwania z testów funkcjonalnych
  * Ta zmiana obejmuje niektóre refaktoryzacje testów, które mogą wymagać od dostawców reagowania

### <a name="test-and-product-code-changes"></a>Zmiany kodu testu i produktu

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109> — konsolidowanie metod RelationalTypeMapping. klonowania
  * Zmiany w 2,1 do RelationalTypeMapping są dozwolone dla uproszczenia w klasach pochodnych. Nie uważamy, że ta zmiana została przerwana dla dostawców, ale dostawcy mogą korzystać z tej zmiany w klasach mapowania typów pochodnych.
* zapytania z tagami <https://github.com/aspnet/EntityFrameworkCore/pull/12069> lub nazwanymi
  * Dodaje infrastrukturę do tagowania zapytań LINQ i że Tagi te są wyświetlane jako komentarze w SQL. Może to wymagać od dostawców reakcji na generowanie kodu SQL.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115> — obsługa danych przestrzennych za pośrednictwem NKTY przerwania
  * Zezwala na rejestrację mapowań typów i translatorów elementów członkowskich poza dostawcą
    * Dostawcy muszą wywoływać bazę. FindMapping () w ich implementacji ITypeMappingSource do pracy
  * Postępuj zgodnie z tym wzorcem, aby dodać do dostawcy obsługę przestrzenną, która jest spójna wśród dostawców.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199> — Dodaj ulepszone debugowanie dla tworzenia dostawcy usług
  * Umożliwia DbContextOptionsExtensions zaimplementowanie nowego interfejsu, który może pomóc użytkownikom zrozumieć, dlaczego wewnętrzny dostawca usług został ponownie skompilowany.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289> — dodaje interfejs API wyłączania do użycia przez kontrole kondycji
  * Ten żądania ściągnięcia dodaje koncepcję `CanConnect`, która będzie używana przez ASP.NET Core kontrole kondycji w celu ustalenia, czy baza danych jest dostępna. Domyślnie implementacja relacyjna wywołuje tylko `Exist`, ale dostawcy mogą zaimplementować coś innego, jeśli jest to konieczne. Dostawcy nierelacyjni będą musieli zaimplementować nowy interfejs API, aby Sprawdzenie kondycji było możliwe.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306> — zaktualizuj bazę RelationalTypeMapping Base, aby nie ustawić rozmiaru DbParameter
  * Zatrzymaj ustawienie domyślnego rozmiaru, ponieważ może to spowodować obcięcie. Dostawca może potrzebować dodać własną logikę, jeśli należy ustawić rozmiar.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372>-RevEng: Zawsze określaj typ kolumny dla kolumn dziesiętnych
  * Należy zawsze skonfigurować typ kolumny dla kolumn dziesiętnych w kodzie szkieletowym, a nie konfigurować według Konwencji.
  * Dostawcy nie powinni wymagać żadnych zmian na ich końcu.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469> — dodaje CaseExpression do generowania wyrażeń przypadków SQL
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648> — dodaje możliwość określania mapowań typów na SqlFunctionExpression w celu usprawnienia typu magazynów argumentów i wyników.
