---
title: Porównaj EF6 i EF Core
description: Wskazówki dotyczące wyboru między EF6 i EF Core.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419648"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

## <a name="ef-core"></a>EF Core

Entity Framework Core ([EF Core](../core/index.md)) jest nowoczesnym maperem bazy danych obiektów dla platformy .NET. Obsługuje zapytania LINQ, śledzenie zmian, aktualizacje i migracje schematu.

EF Core współpracuje z PROGRAMAMI SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL i wieloma innych bazami danych za pośrednictwem [modelu wtyczki dostawcy bazy danych.](../core/providers/index.md)

## <a name="ef6"></a>EF6

Entity Framework 6 ([EF6](../ef6/index.md)) jest obiektokrężną maperką przeznaczoną dla platformy .NET Framework, ale z obsługą platformy .NET Core. EF6 jest stabilnym, obsługiwanym produktem, ale nie jest już aktywnie rozwijany.

## <a name="feature-comparison"></a>Porównanie funkcji

EF Core oferuje nowe funkcje, które nie zostaną zaimplementowane w EF6. Jednak nie wszystkie funkcje EF6 są obecnie implementowane w EF Core.

W poniższych tabelach porównano funkcje dostępne w EF Core i EF6. Jest to porównanie wysokiego poziomu i nie zawiera listy wszystkich funkcji ani nie wyjaśnia różnic między tą samą funkcją w różnych wersjach EF.

Kolumna EF Core wskazuje wersję produktu, w której po raz pierwszy pojawiła się funkcja.

### <a name="creating-a-model"></a>Tworzenie modelu

| **Funkcja**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapowanie klas podstawowych                                   | Tak      | 1.0                                   |
| Konstruktory z parametrami                          |          | 2.1                                   |
| Konwersje wartości nieruchomości                            |          | 2.1                                   |
| Mapowane typy bez klawiszy                             |          | 2.1                                   |
| Konwencja                                           | Tak      | 1.0                                   |
| Konwencje niestandardowe                                    | Tak      | 1.0 (częściowe; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Adnotacje danych                                      | Tak      | 1.0                                   |
| Płynne API                                            | Tak      | 1.0                                   |
| Dziedziczenie: tabela na hierarchię (TPH)                | Tak      | 1.0                                   |
| Dziedziczenie: Tabela dla typu (TPT)                     | Tak      | Planowane na 5,0 ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Dziedziczenie: Tabela na klasę betonu (TPC)           | Tak      | Rozciąganie na 5,0 ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Właściwości stanu cienia                               |          | 1.0                                   |
| Klawisze alternatywne                                        |          | 1.0                                   |
| Wiele do wielu nawigacji                              | Tak      | Planowane na 5,0 ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Jednostka wiele do wielu bez sprzężenia                      | Tak      | Na zaległości ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Generowanie kluczy: Baza danych                              | Tak      | 1.0                                   |
| Generowanie klucza: Klient                                |          | 1.0                                   |
| Typy złożone/należące do                                   | Tak      | 2.0                                   |
| Dane przestrzenne                                          | Tak      | 2.2                                   |
| Format modelu: Kod                                    | Tak      | 1.0                                   |
| Tworzenie modelu z bazy danych: Wiersz polecenia              | Tak      | 1.0                                   |
| Aktualizowanie modelu z bazy danych                            | Częściowo  | Na zaległości ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Globalne filtry zapytań                                  |          | 2.0                                   |
| Dzielenie tabeli                                       | Tak      | 2.0                                   |
| Podział jednostek                                      | Tak      | Rozciąganie na 5,0 ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Mapowanie funkcji skalarnych bazy danych                      | Słabo     | 2.0                                   |
| Mapowanie pola                                         |          | 1.1                                   |
| Typy odwołań do wartości null (C# 8.0)                     |          | 3.0                                   |
| Graficzna wizualizacja modelu                      | Tak      | Brak planowanego wsparcia <sup>(2)</sup>     |
| Edytor modeli graficznych                                | Tak      | Brak planowanego wsparcia <sup>(2)</sup>     |
| Format modelu: EDMX (XML)                              | Tak      | Brak planowanego wsparcia <sup>(2)</sup>     |
| Tworzenie modelu z bazy danych: Kreator programu VS                 | Tak      | Brak planowanego wsparcia <sup>(2)</sup>     |

### <a name="querying-data"></a>Wykonywanie zapytań na danych

| **Funkcja**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| zapytania LINQ                                          | Tak      | 1.0                                   |
| Czytelny wygenerowany SQL                                | Słabo     | 1.0                                   |
| Tłumaczenie GroupBy                                   | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: Chętni                           | Tak      | 1.0                                   |
| Ładowanie powiązanych danych: Wczesne ładowanie dla typów pochodnych |          | 2.1                                   |
| Ładowanie powiązanych danych: Leniwy                            | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: Jawne                        | Tak      | 1.1                                   |
| Nieprzetworzone kwerendy SQL: typy encji                         | Tak      | 1.0                                   |
| Nieprzetworzone kwerendy SQL: typy jednostek bezklucjałów                 | Tak      | 2.1                                   |
| Nieprzetworzone zapytania SQL: komponowanie z linq                  |          | 1.0                                   |
| Jawnie skompilowane zapytania                           | Słabo     | 2.0                                   |
| czekać na foreach (C# 8.0)                                |          | 3.0                                   |
| Tekstowy język kwerendy (Entity SQL)                | Tak      | Brak planowanego wsparcia <sup>(2)</sup>     |

### <a name="saving-data"></a>Zapisywanie danych

| **Funkcja**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Śledzenie zmian: migawka                             | Tak      | 1.0                                   |
| Śledzenie zmian: powiadomienie                         | Tak      | 1.0                                   |
| Śledzenie zmian: Serwery proxy                              | Tak      | Scalone dla 5,0 ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| Uzyskiwanie dostępu do śledzonego stanu                               | Tak      | 1.0                                   |
| Optymistyczna współbieżność                                | Tak      | 1.0                                   |
| Transakcje                                          | Tak      | 1.0                                   |
| Składanie posadeń                                |          | 1.0                                   |
| Mapowanie procedur składowanych                              | Tak      | Na zaległości ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Odłączone interfejsy API niskiego poziomu wykresu                     | Słabo     | 1.0                                   |
| Odłączony wykres Od końca do końca                         |          | 1.0 (częściowe; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Pozostałe funkcje

| **Funkcja**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migracje                                            | Tak      | 1.0                                   |
| Interfejsy API tworzenia/usuwania bazy danych                       | Tak      | 1.0                                   |
| Dane materiału siewnego                                             | Tak      | 2.1                                   |
| Elastyczność połączenia                                 | Tak      | 1.1                                   |
| Interceptory                                          | Tak      | 3.0                                   |
| Zdarzenia                                                | Tak      | 3.0 (częściowe; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Proste rejestrowanie (database.log)                         | Tak      | Scalone dla 5.0 ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| Buforowanie DbContext                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>Dostawcy baz danych <sup>(3)</sup>

| **Funkcja**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Tak      | 1.0                                   |
| MySQL                                                 | Tak      | 1.0                                   |
| PostgreSQL                                            | Tak      | 1.0                                   |
| Oracle                                                | Tak      | 1.0                                   |
| SQLite                                                | Tak      | 1.0                                   |
| SQL Server Compact                                    | Tak      | 1,0 <sup>(4)</sup>                    |
| DB2                                                   | Tak      | 1.0                                   |
| Firebird                                              | Tak      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| W pamięci (do testowania)                               |          | 1.0                                   |

<sup>1</sup> Cele stretch nie są prawdopodobne, aby zostać osiągnięte dla danego wydania. Jeśli jednak wszystko pójdzie dobrze, postaramy się je wciągnąć.

<sup>2</sup> Niektóre funkcje EF6 nie zostaną zaimplementowane w EF Core. Funkcje te zależą od bazowego modelu danych jednostki EF6 (EDM) i/lub są złożonymi funkcjami o stosunkowo niskim zwrocie z inwestycji. Zawsze mile widziane opinie, ale podczas gdy EF Core umożliwia wiele rzeczy, które nie są możliwe w EF6, jest odwrotnie nie wykonalne dla EF Core do obsługi wszystkich funkcji EF6.

<sup>3</sup> Dostawcy baz danych EF Core zaimplementowani przez strony trzecie mogą być opóźnieni w aktualizacji do nowych głównych wersji EF Core. Zobacz [dostawców baz danych, aby](../core/providers/index.md) uzyskać więcej informacji.

<sup>4</sup> Dostawcy programu SQL Server Compact i Jet działają tylko w programie .NET Framework (a nie w programie .NET Core).

### <a name="supported-platforms"></a>Obsługiwane platformy

EF Core 3.1 działa na .NET Core i .NET Framework, za pomocą .NET Standard 2.0. Jednak EF Core 5.0 nie będzie działać w programie .NET Framework. Zobacz [Platformy,](../core/platforms/index.md) aby uzyskać więcej informacji.

Ef6.4 działa na .NET Core i .NET Framework, za pośrednictwem multi-targeting.

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych zastosowań

Użyj EF Core na .NET Core dla wszystkich nowych aplikacji, chyba że aplikacja potrzebuje czegoś, co jest [obsługiwane tylko w programie .NET Framework.](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

EF Core nie zastępuje EF6. Przejście z EF6 do EF Core będzie prawdopodobnie wymagać zmian w aplikacji.

Podczas przenoszenia aplikacji EF6 do platformy .NET Core:
* Kontynuuj korzystanie z EF6, jeśli kod dostępu do danych jest stabilny i prawdopodobnie nie ewoluuje lub potrzebujesz nowych funkcji.
* Port do EF Core, jeśli kod dostępu do danych ewoluuje lub jeśli aplikacja potrzebuje nowych funkcji dostępnych tylko w EF Core.
* Przenoszenie do EF Core jest również często wykonywane dla wydajności. Jednak nie wszystkie scenariusze są szybsze, więc najpierw zrobić niektóre profilowania.

Aby uzyskać więcej informacji, zobacz [Przenoszenie z EF6 do EF Core.](porting/index.md)
