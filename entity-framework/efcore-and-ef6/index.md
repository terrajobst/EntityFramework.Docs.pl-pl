---
title: Porównaj EF6 i EF Core
description: Wskazówki dotyczące wybierania między EF6 i EF Core.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419648"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

## <a name="ef-core"></a>EF Core

Entity Framework Core ([EF Core](../core/index.md)) jest nowoczesnym obiektem mapowania bazy danych dla platformy .NET. Obsługuje zapytania LINQ, śledzenie zmian, aktualizacje i migracje schematu.

EF Core współpracuje z usługami SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL i wieloma innymi bazami danych za pomocą [modelu wtyczki dostawcy bazy danych](../core/providers/index.md).

## <a name="ef6"></a>EF6

Entity Framework 6 ([Ef6](../ef6/index.md)) to Mapowanie obiektowo-relacyjnego zaprojektowane dla .NET Framework, ale z obsługą platformy .NET Core. EF6 to stabilny, obsługiwany produkt, ale nie jest już aktywnie opracowywany.

## <a name="feature-comparison"></a>Porównanie funkcji

EF Core oferuje nowe funkcje, które nie zostaną zaimplementowane w EF6. Jednak nie wszystkie funkcje EF6 są obecnie zaimplementowane w EF Core.

W poniższych tabelach porównano funkcje dostępne w EF Core i EF6. Jest to porównanie wysokiego poziomu i nie zawiera list każdej funkcji ani wyjaśnia różnic między tą samą funkcją w różnych wersjach EF.

Kolumna EF Core wskazuje wersję produktu, w której pierwsza pojawiła się funkcja.

### <a name="creating-a-model"></a>Tworzenie modelu

| **Funkcja**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Podstawowe mapowanie klas                                   | Yes      | 1.0                                   |
| Konstruktory z parametrami                          |          | 2.1                                   |
| Konwersje wartości właściwości                            |          | 2.1                                   |
| Mapowane typy bez kluczy                             |          | 2.1                                   |
| Konwencja                                           | Yes      | 1.0                                   |
| Konwencje niestandardowe                                    | Yes      | 1,0 (częściowa; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Adnotacje danych                                      | Yes      | 1.0                                   |
| Interfejs API Fluent                                            | Yes      | 1.0                                   |
| Dziedziczenie: tabela na hierarchię (TPH)                | Yes      | 1.0                                   |
| Dziedziczenie: tabela na typ (TPT)                     | Yes      | Planowane dla 5,0 ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Dziedziczenie: tabela na konkretną klasę (TPC)           | Yes      | Rozciągnij dla 5,0 ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Właściwości stanu cienia                               |          | 1.0                                   |
| Klucze alternatywne                                        |          | 1.0                                   |
| Nawigacja wiele do wielu                              | Yes      | Planowane dla 5,0 ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Jednostka "wiele do wielu" bez sprzężenia                      | Yes      | W zaległości ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Generowanie klucza: baza danych                              | Yes      | 1.0                                   |
| Generowanie klucza: klient                                |          | 1.0                                   |
| Typy złożone/należące                                   | Yes      | 2.0                                   |
| Dane przestrzenne                                          | Yes      | 2.2                                   |
| Format modelu: kod                                    | Yes      | 1.0                                   |
| Utwórz model z bazy danych: wiersz polecenia              | Yes      | 1.0                                   |
| Aktualizuj model z bazy danych                            | Częściowo  | W zaległości ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Globalne filtry zapytań                                  |          | 2.0                                   |
| Podział tabeli                                       | Yes      | 2.0                                   |
| Dzielenie jednostek                                      | Yes      | Rozciągnij dla 5,0 ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Mapowanie funkcji skalarnej bazy danych                      | Słabo     | 2.0                                   |
| Mapowanie pól                                         |          | 1.1                                   |
| Typy odwołań do wartościC# null (8,0)                     |          | 3.0                                   |
| Graficzna wizualizacja modelu                      | Yes      | Nie zaplanowano pomocy technicznej <sup>(2)</sup>     |
| Edytor modelu graficznego                                | Yes      | Nie zaplanowano pomocy technicznej <sup>(2)</sup>     |
| Format modelu: EDMX (XML)                              | Yes      | Nie zaplanowano pomocy technicznej <sup>(2)</sup>     |
| Utwórz model z bazy danych: Kreator programu VS                 | Yes      | Nie zaplanowano pomocy technicznej <sup>(2)</sup>     |

### <a name="querying-data"></a>Wykonywanie zapytań na danych

| **Funkcja**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| zapytania LINQ                                          | Yes      | 1.0                                   |
| Możliwe do odczytu wygenerowane SQL                                | Słabo     | 1.0                                   |
| Tłumaczenie GroupBy                                   | Yes      | 2.1                                   |
| Ładowanie powiązanych danych: eager                           | Yes      | 1.0                                   |
| Ładowanie powiązanych danych: eager ładowania dla typów pochodnych |          | 2.1                                   |
| Ładowanie powiązanych danych: z opóźnieniem                            | Yes      | 2.1                                   |
| Ładowanie powiązanych danych: jawne                        | Yes      | 1.1                                   |
| Nieprzetworzone zapytania SQL: typy jednostek                         | Yes      | 1.0                                   |
| Nieprzetworzone zapytania SQL: typy jednostek bez typu                 | Yes      | 2.1                                   |
| Nieprzetworzone zapytania SQL: tworzenie przy użyciu LINQ                  |          | 1.0                                   |
| Jawne skompilowane zapytania                           | Słabo     | 2.0                                   |
| await foreach (C# 8,0)                                |          | 3.0                                   |
| Język zapytań tekstowych (Entity SQL)                | Yes      | Nie zaplanowano pomocy technicznej <sup>(2)</sup>     |

### <a name="saving-data"></a>Zapisywanie danych

| **Funkcja**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Śledzenie zmian: migawka                             | Yes      | 1.0                                   |
| Śledzenie zmian: powiadomienie                         | Yes      | 1.0                                   |
| Śledzenie zmian: proxy                              | Yes      | Scalone dla 5,0 ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| Uzyskiwanie dostępu do śledzonego stanu                               | Yes      | 1.0                                   |
| Optymistyczna współbieżność                                | Yes      | 1.0                                   |
| Transakcje                                          | Yes      | 1.0                                   |
| Partia instrukcji                                |          | 1.0                                   |
| Mapowanie procedury składowanej                              | Yes      | W zaległości ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Rozłączone interfejsy API niskiego poziomu grafu                     | Słabo     | 1.0                                   |
| Zakończenie odłączonego wykresu                         |          | 1,0 (częściowa; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Pozostałe funkcje

| **Funkcja**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migracje                                            | Yes      | 1.0                                   |
| Interfejsy API tworzenia/usuwania bazy danych                       | Yes      | 1.0                                   |
| Dane inicjatora                                             | Yes      | 2.1                                   |
| Elastyczność połączenia                                 | Yes      | 1.1                                   |
| Interceptory                                          | Yes      | 3.0                                   |
| Zdarzenia                                                | Yes      | 3,0 (częściowa; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Rejestrowanie proste (Database. log)                         | Yes      | Scalone dla 5,0 ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| Buforowanie kontekstu DbContext                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>Dostawcy bazy danych <sup>(3)</sup>

| **Funkcja**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Oprogramowanie SQL Server                                            | Yes      | 1.0                                   |
| MySQL                                                 | Yes      | 1.0                                   |
| PostgreSQL                                            | Yes      | 1.0                                   |
| Oracle                                                | Yes      | 1.0                                   |
| SQLite                                                | Yes      | 1.0                                   |
| SQL Server Compact                                    | Yes      | 1,0 <sup>(4)</sup>                    |
| DB2                                                   | Yes      | 1.0                                   |
| Firebird                                              | Yes      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| W pamięci (do testowania)                               |          | 1.0                                   |

<sup>1</sup> cele rozciągające nie są możliwe do osiągnięcia dla danej wersji. Jeśli jednak coś się nie powiedzie, spróbujemy je ściągnąć w.

<sup>2</sup> niektóre funkcje Ef6 nie zostaną zaimplementowane w EF Core. Te funkcje są zależne od EF6's podstawowych Entity Data Model (EDM) i/lub są złożonymi funkcjami ze względu na stosunkowo niski zwrot z inwestycji. Zawsze będziemy powiadamiać o opiniach, ale chociaż EF Core pozwala na wiele rzeczy, które nie są możliwe w EF6, nie jest możliwe, aby EF Core do obsługi wszystkich funkcji EF6.

<sup>3</sup> EF Core dostawcy baz danych implementowani przez inne firmy mogą opóźnić aktualizację do nowych głównych wersji EF Core. Aby uzyskać więcej informacji, zobacz [dostawcy bazy danych](../core/providers/index.md) .

<sup>4</sup> dostawcy SQL Server Compact i Jet pracują tylko na .NET Framework (nie na platformie .NET Core).

### <a name="supported-platforms"></a>Obsługiwane platformy

EF Core 3,1 działa na platformie .NET Core i .NET Framework przy użyciu .NET Standard 2,0. Jednak EF Core 5,0 nie będzie działać na .NET Framework. Zobacz [platformę](../core/platforms/index.md) , aby uzyskać więcej szczegółów.

EF 6.4 działa na platformie .NET Core i .NET Framework przy użyciu wiele obiektów docelowych.

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Użyj EF Core na platformie .NET Core dla wszystkich nowych aplikacji, chyba że aplikacja wymaga elementu, który jest [obsługiwany tylko w .NET Framework](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

EF Core nie jest zamiennikiem porzucenia dla EF6. Przechodzenie z EF6 do EF Core prawdopodobnie wymaga wprowadzenia zmian w aplikacji.

Podczas przemieszczania aplikacji EF6 do platformy .NET Core:
* Kontynuuj korzystanie z EF6, jeśli kod dostępu do danych jest stabilny i prawdopodobnie nie będzie mógł rozwijać ani potrzebować nowych funkcji.
* Port do EF Core, jeśli kod dostępu do danych jest rozwijający lub jeśli aplikacja wymaga nowych funkcji dostępnych tylko w programie EF Core.
* Przenoszenie do EF Core jest również często wykonywane w celu uzyskania wydajności. Nie wszystkie scenariusze są jednak szybsze, więc najpierw należy wykonać pewne czynności profilowania.

Aby uzyskać więcej informacji [, zobacz Przenoszenie z Ef6 do EF Core](porting/index.md) .
