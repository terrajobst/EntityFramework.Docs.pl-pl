---
title: "Porównanie cech EF Core & EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 696ff2c8ec788c08880ecb3b07e10dc081b0323b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Porównanie cech EF Core i EF6

W poniższej tabeli porównano funkcje dostępne w EF Core i EF6. Jest on przeznaczony do zapewniają wysokiego poziomu porównania i nie listy wszystkich funkcji lub spróbuj podać szczegółowe informacje na temat możliwych różnic między sposób działania takie same funkcji.

Kolumna EF Core zawiera numer wersji produktu, w którym najpierw pojawił się funkcja.

| **Tworzenie modelu** |**EF 6** |**Podstawowe EF** |
|-|-|-|
| Mapowania klasy podstawowe                         | Tak | 1.0 |
| Konwencje                                 | Tak | 1.0 |
| Konwencje niestandardowych                          | Tak | 1.0 (częściowe) |
| Adnotacje danych                            | Tak | 1.0 |
| Interfejsu API Fluent                                  | Tak | 1.0 |
| Dziedziczenie: Tabela hierarchii (TPH)      | Tak | 1.0 |
| Dziedziczenie: Tabela na typ (TPT)           | Tak |     |
| Dziedziczenie: Tabel na konkretną klasę (TPC) | Tak |     |
| Właściwości stanu w tle                     |     | 1.0 |
| Klucze alternatywne                              |     | 1.0 |
| Wiele do wielu bez sprzężenia jednostki            | Tak |     |
| Generowanie klucza: bazy danych                    | Tak | 1.0 |
| Generowanie klucza: klienta                      |     | 1.0 |
| Typy złożone/własnością                         | Tak | 2.0 |
| Dane przestrzenne                                | Tak |     |
| Graficzny wizualizacji modelu            | Tak |     |
| Edytor graficzny model                      | Tak |     |
| Format modelu: kod:                          | Tak | 1.0 |
| Format modelu: EDMX (XML)                    | Tak |     |
| Utwórz model z bazy danych: wiersza polecenia    | Tak | 1.0 |
| Utwórz model z bazy danych: Kreator VS       | Tak |     |
| Aktualizuj model z bazy danych                  | Częściowe | |
| Zapytanie globalne filtry                        |     | 2.0 |
| Dzielenie tabeli                             | Tak | 2.0 |
| Podziel jednostki                            | Tak |     |
| Mapowanie funkcji skalarnych bazy danych            | Słabo | 2.0 |
| Mapowanie pól                               |     | 1.1 |
| | | |
| **Wykonywanie zapytania na danych** |**EF6** |**Podstawowe EF** |
| zapytania LINQ                                | Tak | 1.0 (w toku dla złożonych zapytań) |
| Czytelny wygenerowanym SQL                      | Słabo | 1.0 |
| Ocena mieszanych klient serwer              |     | 1.0 |
| Podczas ładowania danych powiązanych: wczesny                 | Tak | 1.0 |
| Podczas ładowania danych powiązanych: powolne                  | Tak |     |
| Podczas ładowania danych powiązanych: jawne              | Tak | 1.1 |
| Nieprzetworzone kwerendy SQL: typy modelu                | Tak | 1.0 |
| Nieprzetworzona kwerendy SQL: typy niezawierająca modelu            | Tak |     |
| Nieprzetworzona kwerendy SQL: tworzenie za pomocą LINQ        |     | 1.0 |
| Jawnie skompilowane zapytania                 | Słabo | 2.0 |
| | | |
| **Zapisywanie danych** |**EF6** |**Podstawowe EF** |
| Śledzenie zmian: migawki                   | Tak | 1.0 |
| Śledzenie zmian: powiadomień               | Tak | 1.0 |
| Uzyskiwanie dostępu do śledzonych stanu                     | Tak | 1.0 |
| Optymistycznej współbieżności                      | Tak | 1.0 |
| Transakcje                                | Tak | 1.0 |
| Przetwarzanie wsadowe instrukcji                      |     | 1.0 |
| Procedura składowana                            | Tak |     |
| Odłączony niskiego poziomu interfejsy API programu graph           | Słabo | 1.0 |
| Wykres odłączonego End-to-end               |     | 1.0 (częściowe) |
| | | |
| **Inne funkcje** |**EF6** |**Podstawowe EF** |
| Migracje                                  | Tak | 1.0 |
| Interfejsy API tworzenia/usuwania bazy danych             | Tak | 1.0 |
| Dane inicjatora                                   | Tak |     |
| Elastyczność połączenia                       | Tak | 1.1 |
| Cykl życia przechwytuje (zdarzenia, przejęcie)      | Tak |     |
| Buforowanie typu DbContext                           |     | 2.0 |
| | | |
| **Dostawcy bazy danych** |**EF6**|**Podstawowe EF** |
| SQL Server                                  | Tak | 1.0 |
| MySQL                                       | Tak | 1.0 |
| PostgreSQL                                  | Tak | 1.0 |
| Oracle                                      | Tak | 1.0 (tylko płatnej<sup>(1)</sup>) |
| SQLite                                      | Tak | 1.0 |
| SQL Compact                                 | Tak | 1.0 <sup>(2)</sup> |
| BAZY DANYCH DB2                                         | Tak |     |
| Pamięć (na potrzeby testowania)                      |     | 1.0 |
| | | |
| **Platformy** |**EF6** |**Podstawowe EF** |
| .NET framework (konsoli, WinForms, WPF, ASP.NET) | Tak | 1.0 |
| Oprogramowanie .NET core (Konsola, platformy ASP.NET Core)           |     | 1.0 |
| Mono & Xamarin                              |     | 1.0 (w toku) |
| Platforma UWP                                         |     | 1.0 (w toku) |

<sup>1</sup> trwa praca wolnego oficjalnego dostawcę dla Oracle.
<sup>2</sup> dostawcy programu SQL Server Compact działa tylko w programie .NET Framework (nie .NET Core).
