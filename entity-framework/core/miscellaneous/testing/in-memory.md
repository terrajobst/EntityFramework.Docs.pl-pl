---
title: Testowanie za pomocą InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995590"
---
# <a name="testing-with-inmemory"></a>Testowanie za pomocą InMemory

Dostawca InMemory jest przydatne, gdy chcesz przetestować składników za pomocą coś zbliżony łączenia z bazą danych rzeczywistych, bez potrzeby operacje rzeczywistej bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory nie jest relacyjna baza danych

EF podstawowej bazy danych dostawców ma być relacyjnych baz danych. InMemory została zaprojektowana jako ogólnego przeznaczenia bazy danych w celu testowania i nie jest przeznaczony do naśladować relacyjnej bazy danych.

Przykłady to między innymi:
* InMemory umożliwia zapisują dane, który mógłby naruszyć ograniczenia integralności referencyjnej w relacyjnej bazie danych.

* Jeśli używasz DefaultValueSql(string) właściwości w modelu, jest interfejs API relacyjnej bazy danych, nie odniesie żadnego skutku podczas uruchamiania InMemory.

> [!TIP]  
> Dla wielu testów do celów różnice te nie będą znaczenia. Jednak jeśli chcesz przetestować coś, co bardziej zachowuje się jak true relacyjnej bazy danych, należy rozważyć użycie [trybu w pamięci SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowych

Należy wziąć pod uwagę następujące usługi, która umożliwia aplikacji kod, aby wykonać pewne operacje związane z blogów. Wewnętrznie używa `DbContext` które nawiązuje połączenie z bazą danych programu SQL Server. Należałoby wymiany z tym kontekstem do łączenia z bazą danych InMemory, tak aby nie możemy zapisać wydajne testów dla tej usługi bez konieczności modyfikowania kodu lub dużo pracy w celu utworzenia testu podwójne kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotowanie kontekst

### <a name="avoid-configuring-two-database-providers"></a>Należy unikać konfigurowania dwóch dostawców bazy danych

W testach zamierzasz skonfigurować zewnętrznie kontekst do użycia dostawcy InMemory. Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać kodu warunkowego, upewnij się, tylko Konfigurowanie dostawcy bazy danych, jeśli nie ma już skonfigurowany.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Jeśli używasz platformy ASP.NET Core powinien nie należy tego kodu od dostawcy bazy danych jest już skonfigurowana poza kontekstem (w pliku Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem, aby umożliwić testowanie z innej bazy danych jest zmodyfikowanie kontekst do udostępnienia konstruktora akceptującego `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Określa, że kontekst wszystkie jego ustawienia, takie jak bazy danych do nawiązania połączenia. Jest to ten sam obiekt skompilowanego za pomocą metody OnConfiguring w kontekście użytkownika.

## <a name="writing-tests"></a>Pisanie testów

Kluczem do testowania przy użyciu tego dostawcy jest możliwość Poinformuj kontekst do używania dostawcy InMemory i kontrolowanie zakresu bazy danych w pamięci. Zwykle ma czystą bazy danych dla każdej metody.

Oto przykład klasa testowa, która używa bazy danych InMemory. Każda metoda testu określa unikatową nazwę bazy danych, co oznacza, że każda metoda charakteryzuje się własną bazę danych InMemory.

>[!TIP]
> Aby użyć `.UseInMemoryDatabase()` — metoda rozszerzenia, odwołanie do pakietu NuGet `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
