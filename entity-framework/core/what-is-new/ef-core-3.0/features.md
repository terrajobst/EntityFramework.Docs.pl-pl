---
title: Nowe funkcje programu EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: b6774f615b04bf9579aac5dea217e7321631da0c
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829190"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nowych funkcji dostępnych w programie EF Core 3.0 (obecnie w wersji zapoznawczej)

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.

Poniższa lista zawiera główne nowe funkcje usunięte EF Core 3.0.
Większość tych funkcji nie są uwzględnione w bieżącej wersji zapoznawczej, ale będą dostępne w miarę wprowadzania postępów w wersji RTM.

Przyczyną jest to, że na początku wersji koncentrujemy się na implementowaniu planowane [istotne zmiany w](xref:core/what-is-new/ef-core-3.0/breaking-changes).
Wiele z tych przełomowe zmiany dotyczą poprawy do programu EF Core własnych.
Wiele innych są wymagane, aby odblokować dalsze ulepszenia. 

Aby uzyskać pełną listę poprawek usterek i ulepszeń, widać [tego zapytania w naszym narzędzie do śledzenia problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Ulepszenia zapytań LINQ 

[Śledzenie problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.

LINQ umożliwia tworzenie zapytań bazy danych bez opuszczania usługi z wybranego języka, wykorzystując zaawansowane wpisz informacje funkcji IntelliSense i sprawdzanie typów w czasie kompilacji.
Ale LINQ można również napisać nieograniczonej liczby zapytań skomplikowane, a który zawsze było ogromnym wyzwaniom dla dostawców LINQ.
W pierwszym kilka wersji programu EF Core możemy rozwiązać, w części za ustalenie, jakie części zapytania mogą być tłumaczone do bazy danych SQL, a następnie, umożliwiając pozostałe kwerenda do wykonania w pamięci na komputerze klienckim.
Wykonanie tego po stronie klienta może być pożądana w niektórych sytuacjach, ale w innych przypadkach może to skutkować nieefektywne zapytań, które nie mogą być określane, dopóki aplikacja jest wdrażana w środowisku produkcyjnym.
W programie EF Core 3.0 planujemy wprowadzić głęboki zmiany sposobu działania naszej implementacji LINQ i jak możemy przetestować.
Cele są aby działał on bardziej niezawodnie (na przykład, aby uniknąć przerywanie zapytania w wersjach poprawki), aby włączyć Translacja wyrażeń więcej poprawnie w bazie SQL, można wygenerować wydajne zapytania w większości przypadków i uniemożliwić nieefektywne zapytania przechodząc niewykryte.

## <a name="cosmos-db-support"></a>Obsługa usługi cosmos DB 

[Śledzenie problem #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Ta funkcja jest dołączona w bieżącej wersji zapoznawczej, ale nie została jeszcze ukończona. 

Pracujemy nad dostawcę usługi Cosmos DB dla platformy EF Core, aby umożliwić deweloperom zapoznać się z modelem programistyczne EF łatwo Kieruj usługi Azure Cosmos DB jako bazę danych aplikacji.
Celem jest zapewnienie niektóre korzyści wynikające z usługi Cosmos DB, takie jak dystrybucji globalnej, "zawsze włączone" dostępność, elastyczną skalowalność i małego opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.
Dostawca umożliwi większość funkcji EF Core, takie jak automatyczna zmiana, śledzenie, LINQ i konwersji wartości, przy użyciu interfejsu SQL API w usłudze Cosmos DB.
Rozpoczęliśmy ten nakład pracy przed programem EF Core 2.2, i [Wprowadziliśmy pewne wersji dostawcy dostępne w wersji zapoznawczej](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Nowy plan jest, aby kontynuować, tworzenie dostawcy wraz z programem EF Core 3.0. 

## <a name="c-80-support"></a>C#Obsługa 8.0

[Śledzenie problem #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[śledzenia problemu #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.

Chcemy, aby klienci mogą korzystać z niektórych [nowe funkcje zostaną dodane w C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) strumieni asynchronicznych, takich jak (w tym `await foreach`) i typy referencyjne dopuszczającego wartość null podczas korzystania z programu EF Core.

## <a name="reverse-engineering-of-database-views"></a>Odtwarzanie widoki bazy danych

[Śledzenie problem #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Ta funkcja nie jest zawarty w bieżącej wersji zapoznawczej.

[Typy zapytań](xref:core/modeling/query-types), wprowadzone w programie EF Core 2.1 i uznawane za typów jednostek, bez kluczy w EF Core 3.0 to reprezentują dane, które mogą być odczytywane z bazy danych, ale nie można zaktualizować.
Cecha ta sprawia, że ich doskonale sprawdzą się w przypadku widoków bazy danych w większości przypadków, więc planujemy do zautomatyzowania tworzenia typów jednostek, bez kluczy podczas odtwarzania widoki bazy danych.

## <a name="property-bag-entities"></a>Jednostki zbioru właściwości 

[Śledzenie problem #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) i [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej. 

Ta funkcja jest zapewnienie jednostek, które przechowują dane w właściwości indeksowanych zamiast regularnego właściwości, a także o będzie mógł korzystać z wystąpień klasy .NET (potencjalnie coś tak proste, jak `Dictionary<string, object>`) do reprezentowania typów różnych jednostek w tym samym modelu platformy EF Core.
Ta funkcja jest kamień przechodzenia krok po kroku do obsługi relacji wiele do wielu, bez jednostki sprzężenia ([wystawiać #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), które jest jednym z najbardziej pożądanych ulepszenia dla platformy EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 na platformie .NET Core 

[Śledzenie EF6 problem #271](https://github.com/aspnet/EntityFramework6/issues/271)

Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej. 

Rozumiemy, że wiele istniejących aplikacji używać starszych wersji programu EF i że przenoszenia ich do programu EF Core tylko po to, aby móc korzystać z platformy .NET Core może czasem wymagać znaczących nakładów pracy.
Z tego powodu firma Microsoft będzie można dostosowanie Następna wersja programu EF 6 do uruchamiania na .NET Core 3.0.
Robimy to w celu ułatwienia przenoszenia istniejących aplikacji przy minimalnych zmianach.
Czy powstaną pewne ograniczenia. Na przykład:
- Będzie wymagać nowego dostawcy do pracy z innymi bazami danych oprócz uwzględnione Obsługa programu SQL Server na platformie .NET Core
- Nie można włączyć obsługi przestrzennych z programem SQL Server

Należy zauważyć, że nie istnieją żadne nowe funkcje, w tym momencie zaplanowała programów EF 6.
