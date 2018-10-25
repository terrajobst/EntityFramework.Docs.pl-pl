---
title: Porównanie platformy Entity Framework 6 i platformy Entity Framework Core
description: Wskazówki dotyczące jak dokonać wyboru między Entity Framework 6 i programem Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 0f9f0d4708fa283855eddf2cfc231b37356e413e
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022353"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

Entity Framework to maper obiektowo relacyjny (O/RM) dla platformy .NET. W tym artykule porównano dwie wersje: Entity Framework 6 i programem Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) to technologia dostępu do danych i przetestowanej. Najpierw został wydany w 2008 roku jako część .NET Framework 3.5 SP1 i Visual Studio 2008 z dodatkiem SP1. Począwszy od wersji 4.1 jest dostarczana jako [EntityFramework](https://www.nuget.org/packages/EntityFramework/) pakietu NuGet. EF6 jest uruchamiany w środowisku .NET Framework 4.x, co oznacza, że działa tylko na Windows. 

Programy EF6 w dalszym ciągu być obsługiwane produktu i będą nadal widzieć poprawki błędów i drobne ulepszenia.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) jest pełne ponowne zapisywanie adresów EF6, która została po raz pierwszy w 2016. Wysłaniem go w pakietach Nuget, głównym jest jeden [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core jest produktu dla wielu platform, które można uruchamiać na platformy .NET Core lub .NET Framework.

EF Core zaprojektowano tak, aby zapewnić środowisko programistyczne, podobnie jak EF6. Większość interfejsów API najwyższego poziomu pozostają takie same, EF Core współdziałaniu znane deweloperów, którzy korzystali z platformy EF6.

## <a name="feature-comparison"></a>Porównanie funkcji

EF Core oferuje nowe funkcje, które nie będą realizowane w EF6 (takie jak [klucze alternatywne](xref:core/modeling/alternate-keys), [aktualizacji zbiorczych](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements), i [mieszanego ocena bazy danych i klienta w zapytaniach LINQ](xref:core/querying/client-eval). Ale ponieważ jest on nowy kod podstawowy, mu również pewne funkcje, które ma EF6.

W poniższej tabeli porównano funkcje dostępne w programu EF Core i EF6. Jest to porównania ogólne i nie listę wszystkich funkcji lub wyjaśniono różnice między tej samej funkcji w różnych wersjach programu EF.

Kolumna programu EF Core wskazuje wersję produktu, w którym najpierw pojawiły się tę funkcję.

### <a name="creating-a-model"></a>Tworzenie modelu

| **Funkcja**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapowanie klasy podstawowe                                   | Tak      | 1.0                                   |
| Konstruktorów z parametrami                          |          | 2.1                                   |
| Konwersje wartości właściwości                            |          | 2.1                                   |
| Mapowanych typach bez kluczy (typy zapytań)               |          | 2.1                                   |
| Konwencje                                           | Tak      | 1.0                                   |
| Konwencje niestandardowe                                    | Tak      | 1.0 (częściowa Obsługa)                         |
| Adnotacje danych                                      | Tak      | 1.0                                   |
| Interfejs Fluent API                                            | Tak      | 1.0                                   |
| Dziedziczenie: Tabel na hierarchii (TPH)                | Tak      | 1.0                                   |
| Dziedziczenie: Tabela według typu (TPT)                     | Tak      |                                       |
| Dziedziczenie: Tabel na konkretnej klasy (TPC)           | Tak      |                                       |
| Właściwości stanu w tle                               |          | 1.0                                   |
| Klucze alternatywne                                        |          | 1.0                                   |
| Wiele do wielu bez łączenia jednostek                      | Tak      |                                       |
| Generowanie klucza: bazy danych                              | Tak      | 1.0                                   |
| Generowanie klucza: klienta                                |          | 1.0                                   |
| Typy złożone/właścicielem                                   | Tak      | 2.0                                   |
| Dane przestrzenne                                          | Tak      |                                       |
| Wizualizacja graficzna modelu                      | Tak      |                                       |
| Graficzny Edytor kodu                                | Tak      |                                       |
| Wzór format: kod                                    | Tak      | 1.0                                   |
| Wzór format: EDMX (XML)                              | Tak      |                                       |
| Tworzenie modelu z bazy danych: wiersza polecenia              | Tak      | 1.0                                   |
| Tworzenie modelu z bazy danych: Kreator programu VS                 | Tak      |                                       |
| Aktualizowanie modelu z bazy danych                            | Częściowe  |                                       |
| Filtry zapytań globalnych                                  |          | 2.0                                   |
| Dzielenie tabeli                                       | Tak      | 2.0                                   |
| Podział jednostki                                      | Tak      |                                       |
| Mapowanie funkcji skalarnej bazy danych                      | Słabo     | 2.0                                   |
| Mapowanie pól                                         |          | 1.1                                   |

### <a name="querying-data"></a>Wykonanie zapytania o dane

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| zapytania LINQ                                          | Tak      | 1.0 (w toku dla złożonych zapytań) |
| Elementu Readable wygenerowanego kodu SQL                                | Słabo     | 1.0                                   |
| Ocena mieszane klient serwer                        |          | 1.0                                   |
| GroupBy tłumaczenia                                   | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: Eager                           | Tak      | 1.0                                   |
| Ładowanie powiązanych danych: Eager ładowania dla typów pochodnych |          | 2.1                                   |
| Ładowanie powiązanych danych: powolne                            | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: jawne                        | Tak      | 1.1                                   |
| Pierwotne zapytania SQL: typy jednostek                         | Tak      | 1.0                                   |
| Pierwotne zapytania SQL: typy innego niż jednostka (typy zapytań)       | Tak      | 2.1                                   |
| Pierwotne zapytania SQL: tworzenie za pomocą LINQ                  |          | 1.0                                   |
| Zapytania skompilowane jawnie                           | Słabo     | 2.0                                   |
| Język zapytań tekstowych (jednostki SQL)                | Tak      |                                       |

### <a name="saving-data"></a>Zapisywanie danych

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Śledzenie zmian: migawki                             | Tak      | 1.0                                   |
| Śledzenie zmian: powiadomień                         | Tak      | 1.0                                   |
| Śledzenie zmian: serwery proxy                              | Tak      |                                       |
| Uzyskiwanie dostępu do śledzonych stanu                               | Tak      | 1.0                                   |
| Optymistyczna współbieżność                                | Tak      | 1.0                                   |
| Transakcje                                          | Tak      | 1.0                                   |
| Przetwarzanie wsadowe instrukcji                                |          | 1.0                                   |
| Mapowanie procedur składowanych                              | Tak      |                                       |
| Odłączony niskiego poziomu interfejsy API grafów                     | Słabo     | 1.0                                   |
| Wykres odłączonego End-to-end                         |          | 1.0 (częściowa Obsługa)                         |

### <a name="other-features"></a>Inne funkcje

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migracje                                            | Tak      | 1.0                                   |
| Baza danych tworzenia/usuwania interfejsów API                       | Tak      | 1.0                                   |
| Wstępne wypełnianie danych                                             | Tak      | 2.1                                   |
| Elastyczność połączenia                                 | Tak      | 1.1                                   |
| Cykl życia przechwytuje (zdarzenia, przejmowanie)                | Tak      |                                       |
| Rejestrowanie proste (Database.Log)                         | Tak      |                                       |
| Buforowanie typu DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Dostawcy baz danych

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Tak      | 1.0                                   |
| MySQL                                                 | Tak      | 1.0                                   |
| PostgreSQL                                            | Tak      | 1.0                                   |
| Oracle                                                | Tak      | 1.0 <sup>(1)</sup>                    |
| Bazy danych SQLite                                                | Tak      | 1.0                                   |
| SQL Server Compact                                    | Tak      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Tak      | 1.0                                   |
| Firebird                                              | Tak      | 2.0                                   |
| Jet (Microsoft Access)                                |          | w wersji 2.0 <sup>(2)</sup>                    |
| Pamięć (do testowania)                               |          | 1.0                                   |

<sup>1</sup> obecnie jest dostępna dla rozwiązania Oracle płatnych dostawcy. Trwa praca bezpłatne oficjalnego dostawcę dla Oracle.

<sup>2</sup> dostawcy programu SQL Server Compact i Jet działają tylko w programie .NET Framework (nie na platformie .NET Core).

### <a name="net-implementations"></a>Implementacje platformy .NET

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| .NET framework (Konsola, WinForms, WPF, ASP.NET)      | Tak      | 1.0                                   |
| .NET core (Konsola, platformy ASP.NET Core)                     |          | 1.0                                   |
| Narzędzie mono i Xamarin                                        |          | 1.0 (w toku)                     |
| Platforma UWP                                                   |          | 1.0 (w toku)                     |

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Należy wziąć pod uwagę przy użyciu programu EF Core dla nowej aplikacji, jeśli są spełnione oba poniższe warunki:
* Aplikacja musi możliwości platformy .NET Core. Aby uzyskać więcej informacji, zobacz [Wybieranie między programami .NET Core i .NET Framework dla aplikacji serwerowych](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core obsługuje wszystkie funkcje, których wymaga aplikacja. Jeśli brakuje żądanej funkcji, sprawdź [harmonogram działania dla platformy EF Core](xref:core/what-is-new/roadmap) Aby dowiedzieć się, jeśli istnieją plany pomocy technicznej w przyszłości. 

Należy wziąć pod uwagę przy użyciu platformy EF6, jeśli są spełnione oba poniższe warunki:
* Aplikacja będzie uruchamiana na Windows i program .NET Framework 4.0 lub nowszy.
* EF6 obsługuje wszystkie funkcje, których wymaga aplikacja.

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

Ze względu na fundamentalne zmiany w programie EF Core zaleca się przeniesienie aplikacji EF6 do programu EF Core, chyba że istnieje istotny powód, aby wprowadzić zmianę. Jeśli chcesz przejść do programu EF Core, aby korzystać z nowych funkcji, upewnij się, że masz świadomość jego pewne ograniczenia. Aby uzyskać więcej informacji, zobacz [przenoszenie z programu EF6 do programu EF Core](porting/index.md). **Przenoszenie z programu EF6 do programu EF Core jest więcej niż uaktualnienie portu.** 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz dokumentację:
* <xref:core/index>
* <xref:ef6/index>
