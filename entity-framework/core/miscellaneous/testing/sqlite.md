---
title: "Testowanie za pomocą SQLite - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>Testowanie za pomocą SQLite

SQLite ma tryb w pamięci, która pozwala na potrzeby zapisu testy względem relacyjnej bazy danych, bez potrzeby operacje rzeczywistej bazy danych SQLite.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowych

Należy wziąć pod uwagę następujące usługi, która umożliwia aplikacji kod, aby wykonać pewne operacje związane z blogów. Wewnętrznie używa `DbContext` które nawiązuje połączenie z bazą danych programu SQL Server. Należałoby wymiany z tym kontekstem do połączenia z bazą danych SQLite w pamięci, tak aby nie możemy zapisać wydajne testów dla tej usługi bez konieczności modyfikowania kodu lub dużo pracy w celu utworzenia testu podwójne kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotowanie kontekst

### <a name="avoid-configuring-two-database-providers"></a>Należy unikać konfigurowania dwóch dostawców bazy danych

W testach zamierzasz skonfigurować zewnętrznie kontekst do użycia dostawcy InMemory. Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać kodu warunkowego, upewnij się, tylko Konfigurowanie dostawcy bazy danych, jeśli nie ma już skonfigurowany.

> [!TIP]  
> Jeśli używasz platformy ASP.NET Core powinien nie należy tego kodu od dostawcy bazy danych skonfigurowano poza kontekstem (w pliku Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem, aby umożliwić testowanie z innej bazy danych jest zmodyfikowanie kontekst do udostępnienia konstruktora akceptującego `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Określa, że kontekst wszystkie jego ustawienia, takie jak bazy danych do nawiązania połączenia. Jest to ten sam obiekt skompilowanego za pomocą metody OnConfiguring w kontekście użytkownika.

## <a name="writing-tests"></a>Pisanie testów

Kluczem do testowania przy użyciu tego dostawcy jest możliwość Poinformuj kontekstu SQLite i kontrolowanie zakresu bazy danych w pamięci. Zakres bazy danych jest kontrolowana przez otwarcie i zamknięcie połączenia. Bazy danych jest zakresem czasu trwania, że połączenie jest otwarte. Zwykle ma czystą bazy danych dla każdej metody.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
