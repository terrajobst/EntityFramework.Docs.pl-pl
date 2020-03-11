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
# <a name="testing-with-sqlite"></a><span data-ttu-id="1a818-102">Testowanie za pomocą systemu SQLite</span><span class="sxs-lookup"><span data-stu-id="1a818-102">Testing with SQLite</span></span>

<span data-ttu-id="1a818-103">Program SQLite ma tryb w pamięci, który umożliwia używanie oprogramowania SQLite do pisania testów względem relacyjnej bazy danych, bez konieczności wykonywania dodatkowych operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1a818-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="1a818-104">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tego artykułu można wyświetlić w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="1a818-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="1a818-105">Przykładowy scenariusz testowania</span><span class="sxs-lookup"><span data-stu-id="1a818-105">Example testing scenario</span></span>

<span data-ttu-id="1a818-106">Należy wziąć pod uwagę następujące usługi, dzięki którym kod aplikacji może wykonywać pewne operacje związane ze blogami.</span><span class="sxs-lookup"><span data-stu-id="1a818-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="1a818-107">Wewnętrznie używa `DbContext`, które nawiązuje połączenie z bazą danych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1a818-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="1a818-108">Warto przystąpić do zamiany tego kontekstu w celu nawiązania połączenia z bazą danych programu SQLite w pamięci, dzięki czemu możemy pisać wydajne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonywania wielu zadań, aby utworzyć test w podwójnej części kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1a818-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="1a818-109">Przygotuj swój kontekst</span><span class="sxs-lookup"><span data-stu-id="1a818-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="1a818-110">Unikaj konfigurowania dwóch dostawców baz danych</span><span class="sxs-lookup"><span data-stu-id="1a818-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="1a818-111">W testach zamierzasz zewnętrznie skonfigurować kontekst do korzystania z dostawcy inMemory.</span><span class="sxs-lookup"><span data-stu-id="1a818-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="1a818-112">W przypadku konfigurowania dostawcy bazy danych przez zastępowanie `OnConfiguring` w kontekście, należy dodać kod warunkowy, aby upewnić się, że tylko dostawca bazy danych nie został jeszcze skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="1a818-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="1a818-113">Jeśli używasz ASP.NET Core, ten kod nie powinien być potrzebny, ponieważ dostawca bazy danych jest skonfigurowany poza kontekstem (w Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="1a818-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="1a818-114">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="1a818-114">Add a constructor for testing</span></span>

<span data-ttu-id="1a818-115">Najprostszym sposobem na włączenie testowania względem innej bazy danych jest zmodyfikowanie kontekstu, aby uwidocznić konstruktora, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1a818-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="1a818-116">`DbContextOptions<TContext>` informuje kontekst wszystkich jego ustawień, takich jak baza danych, z którą ma zostać nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="1a818-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="1a818-117">Jest to ten sam obiekt, który jest zbudowany przez uruchomienie metody onconfiguring w Twoim kontekście.</span><span class="sxs-lookup"><span data-stu-id="1a818-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="1a818-118">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="1a818-118">Writing tests</span></span>

<span data-ttu-id="1a818-119">Klucz do testowania przy użyciu tego dostawcy to możliwość poinformowania kontekstu o konieczności użycia oprogramowania SQLite i kontrolowania zakresu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1a818-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="1a818-120">Zakres bazy danych jest kontrolowany przez otwieranie i zamykanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="1a818-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="1a818-121">Baza danych jest objęta zakresem czasu, przez który połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="1a818-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="1a818-122">Zwykle potrzebna jest czysta baza danych dla każdej metody testowej.</span><span class="sxs-lookup"><span data-stu-id="1a818-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="1a818-123">Aby użyć `SqliteConnection()` i metody rozszerzenia `.UseSqlite()`, odwołuje się do pakietu NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="1a818-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
