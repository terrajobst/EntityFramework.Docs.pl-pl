---
title: Porównanie poszczególnych funkcji programu EF Core i EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 84b40f03cdab27fd6fc68c5bb65c6e3d238f226a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967152"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Porównanie poszczególnych funkcji programu EF Core i EF6

W poniższej tabeli porównano funkcje dostępne w programu EF Core i EF6. Jest on przeznaczony do zapewniają wysokiego poziomu porównania i nie listę wszystkich funkcji lub spróbuj podać szczegółowe informacje na temat możliwych różnic między sposobu działania takie same funkcji.

Kolumna programu EF Core zawiera numer wersji produktu, w którym najpierw pojawiły się tę funkcję.

| **Tworzenie modelu**                                  | **EF 6** | **EF Core**                           |
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
|                                                       |          |                                       |
| **Wykonanie zapytania o dane**                                     | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Zapisywanie danych**                                       | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Inne funkcje**                                    | **EF6**  | **EF Core**                           |
| Migracje                                            | Tak      | 1.0                                   |
| Baza danych tworzenia/usuwania interfejsów API                       | Tak      | 1.0                                   |
| Wstępne wypełnianie danych                                             | Tak      | 2.1                                   |
| Elastyczność połączenia                                 | Tak      | 1.1                                   |
| Cykl życia przechwytuje (zdarzenia, przejmowanie)                | Tak      |                                       |
| Rejestrowanie proste (Database.Log)                         | Tak      |                                       |
| Buforowanie typu DbContext                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Dostawcy baz danych**                                | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Platformy**                                         | **EF6**  | **EF Core**                           |
| .NET framework (Konsola, WinForms, WPF, ASP.NET)      | Tak      | 1.0                                   |
| .NET core (Konsola, platformy ASP.NET Core)                     |          | 1.0                                   |
| Narzędzie mono i Xamarin                                        |          | 1.0 (w toku)                     |
| Platforma UWP                                                   |          | 1.0 (w toku)                     |

<sup>1</sup> obecnie jest dostępna płatnych dostawcy. Trwa praca bezpłatne oficjalnego dostawcę dla Oracle.
<sup>2</sup> ten dostawca działa tylko w programie .NET Framework (nie na platformie .NET Core).
