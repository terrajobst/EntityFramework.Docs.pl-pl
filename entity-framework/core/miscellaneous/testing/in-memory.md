---
title: Testowanie za pomocą inMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416510"
---
# <a name="testing-with-inmemory"></a>Testowanie za pomocą bazy danych InMemory

Dostawca inMemory jest przydatny, gdy chce się testować składniki przy użyciu czegoś przybliżonego łączącego się z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji bazy danych.

> [!TIP]  
> [Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>Brak pamięci to relacyjna baza danych

Dostawcy bazy danych EF Core nie muszą być relacyjnymi bazami danych. Pamięć jest zaprojektowana jako baza danych ogólnego przeznaczenia do testowania i nie jest przeznaczona do naśladowania relacyjnej bazy danych.

Oto kilka przykładów tego przykładu:

* InMemory pozwoli zaoszczędzić dane, które mogłyby naruszać ograniczenia więzów integralności w relacyjnej bazie danych.
* Jeśli używasz DefaultValueSql (String) dla właściwości w modelu, jest to interfejs API relacyjnej bazy danych i nie będzie mieć żadnego efektu w przypadku uruchamiania w odniesieniu do pamięci.
* [Współbieżność za pośrednictwem sygnatury czasowej/wersji wiersza](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` lub `IsRowVersion`) nie jest obsługiwana. Jeśli aktualizacja zostanie wykonana przy użyciu starego tokenu współbieżności, nie zostanie zgłoszony żaden [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .

> [!TIP]  
> W wielu celach testowych te różnice nie będą miały znaczenia. Jeśli jednak chcesz przeprowadzić test względem elementu, który zachowuje się podobnie jak w przypadku prawdziwej relacyjnej bazy danych, rozważ użycie [trybu w pamięci programu SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowania

Należy wziąć pod uwagę następujące usługi, dzięki którym kod aplikacji może wykonywać pewne operacje związane ze blogami. Wewnętrznie używa `DbContext`, które nawiązuje połączenie z bazą danych SQL Server. Warto przystąpić do zamiany tego kontekstu w celu nawiązania połączenia z bazą danych inMemory, dzięki czemu możemy pisać wydajne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonywania wielu zadań, aby utworzyć test w podwójnej części kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotuj swój kontekst

### <a name="avoid-configuring-two-database-providers"></a>Unikaj konfigurowania dwóch dostawców baz danych

W testach zamierzasz zewnętrznie skonfigurować kontekst do korzystania z dostawcy inMemory. W przypadku konfigurowania dostawcy bazy danych przez zastępowanie `OnConfiguring` w kontekście, należy dodać kod warunkowy, aby upewnić się, że tylko dostawca bazy danych nie został jeszcze skonfigurowany.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Jeśli używasz ASP.NET Core, ten kod nie powinien być potrzebny, ponieważ dostawca bazy danych jest już skonfigurowany poza kontekstem (w Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem na włączenie testowania względem innej bazy danych jest zmodyfikowanie kontekstu, aby uwidocznić konstruktora, który akceptuje `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontekst wszystkich jego ustawień, takich jak baza danych, z którą ma zostać nawiązane połączenie. Jest to ten sam obiekt, który jest zbudowany przez uruchomienie metody onconfiguring w Twoim kontekście.

## <a name="writing-tests"></a>Pisanie testów

Kluczem do testowania za pomocą tego dostawcy jest możliwość poinformowania kontekstu o konieczności użycia dostawcy inMemory i kontrolowania zakresu bazy danych znajdującej się w pamięci. Zwykle potrzebna jest czysta baza danych dla każdej metody testowej.

Oto przykład klasy testowej, która korzysta z bazy danych inMemory. Każda metoda testowa określa unikatową nazwę bazy danych, co oznacza, że każda metoda ma własną bazę danych inMemory.

>[!TIP]
> Aby użyć metody rozszerzenia `.UseInMemoryDatabase()`, odwołuje się do pakietu NuGet [Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
