---
title: Testowanie za pomocą SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996871"
---
# <a name="testing-with-sqlite"></a>Testowanie za pomocą SQLite

Bazy danych SQLite ma trybie w pamięci, która pozwala na potrzeby pisania testów przeciwko relacyjnej bazy danych bez konieczności operacji bazy danych SQLite.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub

## <a name="example-testing-scenario"></a>Przykładowy scenariusz testowania

Należy wziąć pod uwagę następujące usługa, która umożliwia wykonywanie operacji związanych z blogów kodu aplikacji. Używa wewnętrznie `DbContext` łączący się z bazą danych programu SQL Server. Należałoby wymiany tego kontekstu, aby połączyć się z bazą danych SQLite w pamięci, możemy napisać efektywne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonać dużo pracy, aby utworzyć test podwójnego kontekstu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Przygotuj kontekstu

### <a name="avoid-configuring-two-database-providers"></a>Należy unikać konfigurowania dwóch dostawców bazy danych

W testach ma zewnętrznie skonfigurować kontekst do użycia dostawcy InMemory. Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać niektórych kod warunkowy, aby upewnić się, można tylko skonfigurować dostawcy bazy danych, gdy nie została już skonfigurowana.

> [!TIP]  
> Jeśli używasz platformy ASP.NET Core, następnie nie należy tego kodu od dostawcy bazy danych skonfigurowano poza kontekstem (w pliku Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Dodaj Konstruktor do testowania

Najprostszym sposobem, aby umożliwić testowanie z inną bazą danych jest zmodyfikowanie kontekst ujawniać Konstruktor, który akceptuje `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontekstu, wszystkie jego ustawienia, takie jak której bazy danych, aby nawiązać połączenie. Jest to ten sam obiekt, który jest wbudowana, uruchamiając metoda OnConfiguring w kontekście usługi.

## <a name="writing-tests"></a>Pisanie testów

Kluczem do testowania przy użyciu tego dostawcy jest możliwość opowiadania kontekstu do używania bazy danych SQLite i kontrolowanie zakresu bazy danych w pamięci. Zakres bazy danych jest kontrolowana przez otwierające i zamykające połączenia. Baza danych jest ograniczony do czasu trwania, że połączenie jest otwarte. Zazwyczaj chcesz czyste bazy danych dla każdej metody testowej.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
