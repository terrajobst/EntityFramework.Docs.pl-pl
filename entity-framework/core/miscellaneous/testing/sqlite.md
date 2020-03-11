---
title: Testowanie przy użyciu oprogramowania SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417303"
---
# <a name="testing-with-sqlite"></a>Testowanie za pomocą systemu SQLite

Program SQLite ma tryb w pamięci, który umożliwia używanie oprogramowania SQLite do pisania testów względem relacyjnej bazy danych, bez konieczności wykonywania dodatkowych operacji bazy danych.

> [!TIP]  
> [Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tego artykułu można wyświetlić w witrynie GitHub

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowania

Należy wziąć pod uwagę następujące usługi, dzięki którym kod aplikacji może wykonywać pewne operacje związane ze blogami. Wewnętrznie używa `DbContext`, które nawiązuje połączenie z bazą danych SQL Server. Warto przystąpić do zamiany tego kontekstu w celu nawiązania połączenia z bazą danych programu SQLite w pamięci, dzięki czemu możemy pisać wydajne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonywania wielu zadań, aby utworzyć test w podwójnej części kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotuj swój kontekst

### <a name="avoid-configuring-two-database-providers"></a>Unikaj konfigurowania dwóch dostawców baz danych

W testach zamierzasz zewnętrznie skonfigurować kontekst do korzystania z dostawcy inMemory. W przypadku konfigurowania dostawcy bazy danych przez zastępowanie `OnConfiguring` w kontekście, należy dodać kod warunkowy, aby upewnić się, że tylko dostawca bazy danych nie został jeszcze skonfigurowany.

> [!TIP]  
> Jeśli używasz ASP.NET Core, ten kod nie powinien być potrzebny, ponieważ dostawca bazy danych jest skonfigurowany poza kontekstem (w Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem na włączenie testowania względem innej bazy danych jest zmodyfikowanie kontekstu, aby uwidocznić konstruktora, który akceptuje `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontekst wszystkich jego ustawień, takich jak baza danych, z którą ma zostać nawiązane połączenie. Jest to ten sam obiekt, który jest zbudowany przez uruchomienie metody onconfiguring w Twoim kontekście.

## <a name="writing-tests"></a>Pisanie testów

Klucz do testowania przy użyciu tego dostawcy to możliwość poinformowania kontekstu o konieczności użycia oprogramowania SQLite i kontrolowania zakresu bazy danych w pamięci. Zakres bazy danych jest kontrolowany przez otwieranie i zamykanie połączenia. Baza danych jest objęta zakresem czasu, przez który połączenie jest otwarte. Zwykle potrzebna jest czysta baza danych dla każdej metody testowej.

>[!TIP]
> Aby użyć `SqliteConnection()` i metody rozszerzenia `.UseSqlite()`, odwołuje się do pakietu NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
