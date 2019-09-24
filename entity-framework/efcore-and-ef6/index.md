---
title: Porównaj Entity Framework 6 i Entity Framework Core
description: Zawiera wskazówki dotyczące wyboru między Entity Framework 6 i Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 3d2f72e64e6846d2d8bb6d4d507e04090287114d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198011"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

Entity Framework to Mapowanie obiektowo-relacyjne (O/RM) dla platformy .NET. W tym artykule porównano dwie wersje: Entity Framework 6 i Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) to wypróbowana i przetestowana technologia dostępu do danych. Najpierw została wydana w 2008 w ramach .NET Framework 3,5 SP1 i Visual Studio 2008 SP1. Począwszy od wersji 4,1, jest ona dostarczana jako pakiet NuGet [EntityFramework](https://www.nuget.org/packages/EntityFramework/) . EF6 działa na .NET Framework 4. x i .NET Core z 3,0.

EF6 nadal jest obsługiwanym produktem i nadal będzie widział poprawki błędów i drobne ulepszenia.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) to pełny zapis EF6, który został po raz pierwszy opublikowany w 2016. Jest ona dostarczana z pakietami NuGet, głównymi jest [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core jest produktem dla wielu platform, który działa w środowisku .NET Core.

EF Core zaprojektowano w celu udostępnienia środowiska deweloperskiego podobnego do EF6. Większość interfejsów API najwyższego poziomu pozostaje taka sama, więc EF Core będą znane deweloperom, którzy korzystali z EF6.

## <a name="feature-comparison"></a>Porównanie funkcji

EF Core oferuje nowe funkcje, które nie zostaną zaimplementowane w EF6 (takie jak [klucze alternatywne](xref:core/modeling/alternate-keys), [aktualizacje wsadowe](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements)i [mieszany klient/Ocena bazy danych w zapytaniach LINQ](xref:core/querying/client-eval)). Ale ponieważ jest to nowa baza kodu, nie ma także niektórych funkcji, które EF6.

W poniższych tabelach porównano funkcje dostępne w EF Core i EF6. Jest to porównanie wysokiego poziomu i nie wykazuje każdej funkcji ani nie wyjaśnia różnic między tą samą funkcją w różnych wersjach EF.

Kolumna EF Core wskazuje wersję produktu, w której pierwsza pojawiła się funkcja.

### <a name="creating-a-model"></a>Tworzenie modelu

| **Funkcja**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Podstawowe mapowanie klas                                   | Tak      | 1.0                                   |
| Konstruktory z parametrami                          |          | 2.1                                   |
| Konwersje wartości właściwości                            |          | 2.1                                   |
| Mapowane typy bez kluczy                             |          | 2.1                                   |
| Konwencje                                           | Tak      | 1.0                                   |
| Konwencje niestandardowe                                    | Tak      | 1,0 (częściowa)                         |
| Adnotacje danych                                      | Tak      | 1.0                                   |
| Interfejs API Fluent                                            | Tak      | 1.0                                   |
| Strukturze Tabela na hierarchię (TPH)                | Tak      | 1.0                                   |
| Strukturze Tabela na typ (TPT)                     | Tak      |                                       |
| Strukturze Tabela na konkretną klasę (TPC)           | Tak      |                                       |
| Właściwości stanu cienia                               |          | 1.0                                   |
| Klucze alternatywne                                        |          | 1.0                                   |
| Jednostka "wiele do wielu" bez sprzężenia                      | Tak      |                                       |
| Generowanie klucza: Baza danych                              | Tak      | 1.0                                   |
| Generowanie klucza: Klient                                |          | 1.0                                   |
| Typy złożone/należące                                   | Tak      | 2.0                                   |
| Dane przestrzenne                                          | Tak      | 2.2                                   |
| Graficzna wizualizacja modelu                      | Tak      |                                       |
| Edytor modelu graficznego                                | Tak      |                                       |
| Format modelu: Kod                                    | Tak      | 1.0                                   |
| Format modelu: EDMX (XML)                              | Tak      |                                       |
| Utwórz model z bazy danych: Wiersz polecenia              | Tak      | 1.0                                   |
| Utwórz model z bazy danych: Kreator VS                 | Tak      |                                       |
| Aktualizuj model z bazy danych                            | Częściowe  |                                       |
| Globalne filtry zapytań                                  |          | 2.0                                   |
| Podział tabeli                                       | Tak      | 2.0                                   |
| Dzielenie jednostek                                      | Tak      |                                       |
| Mapowanie funkcji skalarnej bazy danych                      | Słabo     | 2.0                                   |
| Mapowanie pól                                         |          | 1.1                                   |
| Typy odwołań do wartościC# null (8,0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Wykonanie zapytania o dane

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| zapytania LINQ                                          | Tak      | 1,0 (w toku dla złożonych zapytań) |
| Możliwe do odczytu wygenerowane SQL                                | Słabo     | 1.0                                   |
| Tłumaczenie GroupBy                                   | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: Eager                           | Tak      | 1.0                                   |
| Ładowanie powiązanych danych: Eager ładowanie dla typów pochodnych |          | 2.1                                   |
| Ładowanie powiązanych danych: Lazy                            | Tak      | 2.1                                   |
| Ładowanie powiązanych danych: Wprost                        | Tak      | 1.1                                   |
| Nieprzetworzone zapytania SQL: Typy jednostek                         | Tak      | 1.0                                   |
| Nieprzetworzone zapytania SQL: Typy jednostek z mniejszą ilością                 | Tak      | 2.1                                   |
| Nieprzetworzone zapytania SQL: Tworzenie za pomocą LINQ                  |          | 1.0                                   |
| Jawne skompilowane zapytania                           | Słabo     | 2.0                                   |
| Język zapytań tekstowych (Entity SQL)                | Tak      |                                       |
| await foreach (C# 8,0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Zapisywanie danych

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Śledzenie zmian: Migawka                             | Tak      | 1.0                                   |
| Śledzenie zmian: Powiadomienia                         | Tak      | 1.0                                   |
| Śledzenie zmian: Serwery proxy                              | Tak      |                                       |
| Uzyskiwanie dostępu do śledzonego stanu                               | Tak      | 1.0                                   |
| Optymistyczna współbieżność                                | Tak      | 1.0                                   |
| Transakcje                                          | Tak      | 1.0                                   |
| Partia instrukcji                                |          | 1.0                                   |
| Mapowanie procedury składowanej                              | Tak      |                                       |
| Rozłączone interfejsy API niskiego poziomu grafu                     | Słabo     | 1.0                                   |
| Zakończenie odłączonego wykresu                         |          | 1,0 (częściowa)                         |

### <a name="other-features"></a>Inne funkcje

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migracje                                            | Tak      | 1.0                                   |
| Interfejsy API tworzenia/usuwania bazy danych                       | Tak      | 1.0                                   |
| Dane inicjatora                                             | Tak      | 2.1                                   |
| Odporność połączenia                                 | Tak      | 1.1                                   |
| Punkty zaczepienia cyklu życia (zdarzenia, przechwycenia)                | Tak      |                                       |
| Rejestrowanie proste (Database. log)                         | Tak      |                                       |
| Buforowanie kontekstu DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Dostawcy baz danych

| **Funkcja**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Tak      | 1.0                                   |
| MySQL                                                 | Tak      | 1.0                                   |
| PostgreSQL                                            | Tak      | 1.0                                   |
| Oracle                                                | Tak      | 1.0                                   |
| SQLite                                                | Tak      | 1.0                                   |
| SQL Server Compact                                    | Tak      | 1,0 <sup>(1)</sup>                    |
| DB2                                                   | Tak      | 1.0                                   |
| Firebird                                              | Tak      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(1)</sup>                    |
| Cosmos DB                                             |          | 3.0                                   |
| W pamięci (do testowania)                               |          | 1.0                                   |

<sup>1</sup> dostawcy SQL Server Compact i Jet pracują tylko na .NET Framework (nie na platformie .NET Core).

### <a name="net-implementations"></a>Implementacje platformy .NET

| **Funkcja**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Tak                | 1,0 (usunięte w 3,0)                  |
| .NET Core                                             | Tak (dodano w 6,3) | 1.0                                   |
| Narzędzie mono & Xamarin                                        |                    | 1,0 (w toku)                     |
| Platforma UWP                                                   |                    | 1,0 (w toku)                     |

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Rozważ użycie EF Core dla nowej aplikacji, jeśli są spełnione oba z następujących warunków:
* Aplikacja wymaga możliwości platformy .NET Core. Aby uzyskać więcej informacji, zobacz [Wybieranie między platformą .NET Core i .NET Framework dla aplikacji serwerowych](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core obsługuje wszystkie funkcje wymagane przez aplikację. Jeśli brakuje odpowiedniej funkcji, zapoznaj się z [planem EF Core](xref:core/what-is-new/index) , aby dowiedzieć się, czy istnieją plany obsługi tego programu w przyszłości. 

Rozważ użycie EF6, jeśli są spełnione oba z następujących warunków:
* Aplikacja będzie działać w systemie Windows i w .NET Framework 4,0 lub nowszej.
* EF6 obsługuje wszystkie funkcje wymagane przez aplikację.

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

Ze względu na podstawowe zmiany w EF Core nie zalecamy przeniesienia aplikacji EF6 do EF Core, chyba że istnieje istotny powód wprowadzenia zmiany. Jeśli chcesz przejść do EF Core, aby korzystać z nowych funkcji, upewnij się, że masz świadomość jego ograniczeń. Aby uzyskać więcej informacji, zobacz [przenoszenie z Ef6 do EF Core](porting/index.md). **Przechodzenie z EF6 do EF Core jest bardziej portem niż uaktualnienie.**

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zapoznaj się z dokumentacją:
* <xref:core/index>
* <xref:ef6/index>
