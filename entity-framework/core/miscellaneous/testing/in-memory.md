---
title: Testowanie za pomocą InMemory — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900265"
---
# <a name="testing-with-inmemory"></a>Testowanie za pomocą InMemory

Dostawca InMemory jest przydatne, gdy chcesz przetestować składników za pomocą coś, co przybliża z bazą danych rzeczywistych, bez konieczności operacji bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory nie jest relacyjnej bazy danych

Dostawcy baz danych programu EF Core nie trzeba być relacyjnych baz danych. InMemory została zaprojektowana jako bazy danych ogólnego przeznaczenia do testowania i nie jest przeznaczony do naśladowania relacyjnej bazy danych.

Niektóre przykłady to między innymi:

* InMemory pozwoli na zapisywanie danych, który mógłby naruszyć ograniczenia integralności referencyjnej w relacyjnej bazie danych.
* Jeśli używasz DefaultValueSql(string) właściwości w modelu jest interfejs API relacyjnej bazy danych i nie odniesie skutku przy uruchamianiu InMemory.
* [Współbieżność za pośrednictwem wiersza i znacznik czasu: wersja](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` lub `IsRowVersion`) nie jest obsługiwane. Nie [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) zostanie zgłoszony, jeśli aktualizacja jest wykonywane przy użyciu starego tokenu współbieżności.

> [!TIP]  
> Różnice te nie będą znaczenia, dla wielu celów testowych. Jednak jeśli chcesz przetestować względem elementu, który zachowuje się bardziej jak true relacyjnej bazy danych, rozważ użycie [trybie w pamięci z bazy danych SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowania

Należy wziąć pod uwagę następujące usługa, która umożliwia wykonywanie operacji związanych z blogów kodu aplikacji. Używa wewnętrznie `DbContext` łączący się z bazą danych programu SQL Server. Należałoby wymiany z tym kontekstem do nawiązania połączenia z bazą InMemory tak, aby firma Microsoft można zapisać efektywne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonać dużo pracy, aby utworzyć test podwójnego kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotuj kontekstu

### <a name="avoid-configuring-two-database-providers"></a>Należy unikać konfigurowania dwóch dostawców bazy danych

W testach ma zewnętrznie skonfigurować kontekst do użycia dostawcy InMemory. Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać niektórych kod warunkowy, aby upewnić się, można tylko skonfigurować dostawcy bazy danych, gdy nie została już skonfigurowana.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Jeśli używasz platformy ASP.NET Core, następnie nie należy tego kodu od dostawcy bazy danych jest już skonfigurowana poza kontekstem (w pliku Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem, aby umożliwić testowanie z inną bazą danych jest zmodyfikowanie kontekst ujawniać Konstruktor, który akceptuje `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontekstu, wszystkie jego ustawienia, takie jak której bazy danych, aby nawiązać połączenie. Jest to ten sam obiekt, który jest wbudowana, uruchamiając metoda OnConfiguring w kontekście usługi.

## <a name="writing-tests"></a>Pisanie testów

Kluczem do testowania przy użyciu tego dostawcy jest możliwość opowiadania kontekst do użycia dostawcy InMemory i kontrolowanie zakresu bazy danych w pamięci. Zazwyczaj chcesz czyste bazy danych dla każdej metody testowej.

Oto przykład klasy testowej, korzystającej z bazy danych w pamięci. Każdej metody testowej Określa unikatową nazwę bazy danych, co oznacza, że każda metoda charakteryzuje się własną bazę danych w pamięci.

>[!TIP]
> Aby użyć `.UseInMemoryDatabase()` metodę rozszerzenia, odwołanie do pakietu NuGet `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
