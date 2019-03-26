---
title: Dziennik zmian wpływających na dostawcy — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 1133976d8d25e4099b64a1a30a8d2066ff3f6cd7
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419669"
---
# <a name="provider-impacting-changes"></a>Zmiany wpływające na dostawcy

Ta strona zawiera linki do przeprowadzanych na repozytorium programu EF Core, które mogą wymagać autorzy innych dostawców bazy danych, aby reagować żądań ściągnięcia. Jest do udostępniania punktu wyjściowego dla autorów istniejącego dostawcy baz danych innych firm, podczas aktualizowania ich dostawcy do nowej wersji.

Rozpoczynamy ten dziennik zmian z 2.1 do wersji 2.2. Przed 2.1 użyliśmy [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiety na nasze problemy i żądania ściągnięcia.

## <a name="22-----30"></a>2.2 ---> 3.0

Należy pamiętać, który wiele [istotne zmiany na poziomie aplikacji](../what-is-new/ef-core-3.0/breaking-changes.md) wpływa także na dostawców.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * Usunięto przestarzałe interfejsy API i przeciążenia zwinięty parametr opcjonalny
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Usunięto przestarzałe interfejsy API
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Podklasy CharTypeMapping mogła zostać przerwane z powodu zmian zachowania wymagane do naprawiania kilka błędów w podstawowej implementacji.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Dodaje klasę bazową dla IDatabaseModelFactory i zaktualizować go do użycia obiektu parametr, aby uniknąć przerw w przyszłości.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Używane obiekty parametru w MigrationsSqlGenerator, aby uniknąć przerw w przyszłości.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Jawne konfiguracji poziomy dziennika wymagane pewne zmiany do interfejsów API, który używa dostawcy. W szczególności dostawców korzystania z infrastruktury rejestrowania bezpośrednio, następnie ta zmiana może spowodować przerwanie używające. Ponadto tych dostawców, którzy korzystają z infrastruktury, (które będą widoczne publicznie) przyszłości musi pochodzić od `LoggingDefinitions` lub `RelationalLoggingDefinitions`. Zobacz programu SQL Server i dostawców w pamięci na potrzeby przykładów.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Ciągi zasobów Core, relacyjne i abstrakcje teraz są publiczne.
  * `CoreLoggerExtensions` i `RelationalLoggerExtensions` są teraz w publicznej. Dostawcy powinni używać tych interfejsów API, gdy rejestrowanie zdarzeń, które są zdefiniowane na poziomie relacyjna lub core. Brak dostępu do rejestrowania zasobów bezpośrednio; są to nadal wewnętrznego.
  * `IRawSqlCommandBuilder` zmienił się z pojedynczego wystąpienia usługi do usługi o określonym zakresie
  * `IMigrationsSqlGenerator` zmienił się z pojedynczego wystąpienia usługi do usługi o określonym zakresie
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * Infrastruktura do tworzenia poleceń relacyjnych zostanie on opublikowany, może być bezpiecznie używany przez dostawców i nieco zaprojektowane od nowa.
  * `IRelationalCommandBuilderFactory`zmienił się z pojedynczego wystąpienia usługi do usługi o określonym zakresie
  * `IShaperCommandContextFactory` zmienił się z pojedynczego wystąpienia usługi do usługi o określonym zakresie
  * `ISelectExpressionFactory` zmienił się z pojedynczego wystąpienia usługi do usługi o określonym zakresie
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` zmienił się z usługi o określonym zakresie z usługą przejściowe
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` została zmieniona z usługi o określonym zakresie do pojedynczego wystąpienia usługi
  * Ponadto `ISingletonUpdateSqlGenerator` został usunięty
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Wiele kodu wewnętrznego, który był używany przez dostawców teraz stało się publiczny
  * Nie powinien być necssary k odkazu `IndentedStringBuilder` , ponieważ ma została uwzględniona poza miejsc, które go dostępne
  * Sposoby użycia `NonCapturingLazyInitializer` powinien zostać zamieniony `LazyInitializer` z BCL.
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Ta zmiana jest szczegółowo opisane w aplikacji przełomowe zmiany dokumentu. W przypadku dostawców może być to więcej wpływu na testowanie programu EF core często może powodować Niezastosowanie tego problemu, więc infrastrukturę testowania została zmieniona na upewnij, że mniej prawdopodobne.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` został uproszczony
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * StartsWith tłumaczenia została zmieniona w taki sposób, że dostawcy może chcesz/konieczność react

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Zmiany tylko do testów

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Zezwalaj na możliwe do dostosowania ograniczników SQL w testach
  * Testowanie zmiany, które umożliwiają nieścisłym porównania punktu zmiennoprzecinkowego w BuiltInDataTypesTestBase
  * Zmiany testów, które umożliwiają zapytania testy, aby być ponownie używane, za pomocą różnych ograniczników SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Dodaj testy DbFunction testów specyfikacji relacyjnych
  * Taki sposób, że te testy mogą być uruchamiane względem wszystkich dostawców bazy danych
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Czyszczenie testowego przełączania do Async
  * Usuń `Wait` wywołań, niepotrzebne async i zmienić nazwy niektórych metod testowych
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Ujednolicenie infrastrukturę testowania rejestrowania
  * Dodano `CreateListLoggerFactory` i usunąć niektóre poprzedniego infrastruktury rejestrowania, który będzie wymagać dostawców przy użyciu tych testów, reakcji
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) — Uruchamianie więcej testów zapytania synchronicznie i asynchronicznie
  * Nazwy testów i uwzględniając uległ zmianie, co będzie wymagać dostawców przy użyciu tych testów, reakcji
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Zmiana nazwy tego modelu ComplexNavigations
  * Za pomocą tych testów dostawców może być konieczne reagowanie
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Zwracania kontekstu danej puli zamiast usuwania w testy funkcjonalne
  * Ta zmiana obejmuje niektóre refaktoryzacji testów, które mogą wymagać dostawców reagować


### <a name="test-and-product-code-changes"></a>Test i produktu zmian w kodzie

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Skonsolidować RelationalTypeMapping.Clone metody
  * Zmiany w 2.1 RelationalTypeMapping dozwolone dla uproszczenia w klasach pochodnych. Firma Microsoft uważa, nie zostało to istotne do dostawców, ale dostawców korzystać z zalet tej zmiany w ich typ pochodny mapowania klas.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Oznakowane lub nazwanego zapytania
  * Dodaje infrastrukturę na potrzeby znakowania zapytań LINQ i posiadanie tych tagów, są wyświetlane jako komentarze w SQL. Może to wymagać dostawców reagowania generowanie kodu SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) — Obsługuje dane przestrzenne za pośrednictwem nkty przerwania
  * Umożliwia mapowania typów i elementów członkowskich tłumaczy do zarejestrowania poza dostawcy
    * Dostawcy musi wywołać podstawowej. FindMapping() w ich realizacji ITypeMappingSource, aby działał
  * Postępuj zgodnie z tego wzorca, aby dodać obsługę przestrzenne do dostawcą, który jest spójny w ramach dostawcy.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) — Dodawanie Ulepszone debugowanie Tworzenie dostawcy usługi
  * Umożliwia DbContextOptionsExtensions wdrożyć nowy interfejs, który pomaga zrozumieć, dlaczego dostawcy usługi wewnętrznej jest przebudowany osób
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Dodaje CanConnect interfejsu API do użytku przez kontrole kondycji
  * To żądanie Ściągnięcia dodaje koncepcji `CanConnect` który będzie używany przez platformy ASP.NET Core kondycji sprawdza, czy baza danych jest dostępna. Domyślnie, implementacja relacyjnych po prostu wywołuje funkcję `Exist`, ale dostawców można zaimplementować inny, jeśli to konieczne. Nierelacyjne dostawców należy zaimplementować nowy interfejs API w kolejności dla kontroli kondycji może być używany.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) — Aktualizowanie podstawowy RelationalTypeMapping nie ustawiać DbParameter rozmiar
  * Zatrzymaj, ustawianie rozmiaru domyślnie, ponieważ może to spowodować obcięcie. Dostawców może być konieczne dodanie własnych logiki, jeśli rozmiar musi być ustawiona.
* https://github.com/aspnet/EntityFrameworkCore/pull/13372 -RevEng: Zawsze określać typ kolumny dla kolumny dziesiętna
  * Zawsze należy skonfigurować typ kolumny dla kolumny dziesiętną w utworzony szkielet kodu, zamiast konfigurować zgodnie z Konwencją.
  * Dostawców nie powinna wymagać zmiany po ich stronie.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Dodaje CaseExpression generowania wyrażenia SQL CASE
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Dodaje możliwość określenia mapowania typów na SqlFunctionExpression w celu zwiększenia magazynu wnioskowanie argumentów i wyników.
