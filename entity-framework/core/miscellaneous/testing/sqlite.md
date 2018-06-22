---
title: Testowanie za pomocą SQLite - EF Core
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
ms.locfileid: "26054173"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="ee125-102">Testowanie za pomocą SQLite</span><span class="sxs-lookup"><span data-stu-id="ee125-102">Testing with SQLite</span></span>

<span data-ttu-id="ee125-103">SQLite ma tryb w pamięci, która pozwala na potrzeby zapisu testy względem relacyjnej bazy danych, bez potrzeby operacje rzeczywistej bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ee125-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="ee125-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="ee125-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="ee125-105">Przykładowy scenariusz testowych</span><span class="sxs-lookup"><span data-stu-id="ee125-105">Example testing scenario</span></span>

<span data-ttu-id="ee125-106">Należy wziąć pod uwagę następujące usługi, która umożliwia aplikacji kod, aby wykonać pewne operacje związane z blogów.</span><span class="sxs-lookup"><span data-stu-id="ee125-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="ee125-107">Wewnętrznie używa `DbContext` które nawiązuje połączenie z bazą danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ee125-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="ee125-108">Należałoby wymiany z tym kontekstem do połączenia z bazą danych SQLite w pamięci, tak aby nie możemy zapisać wydajne testów dla tej usługi bez konieczności modyfikowania kodu lub dużo pracy w celu utworzenia testu podwójne kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ee125-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="ee125-109">Przygotowanie kontekst</span><span class="sxs-lookup"><span data-stu-id="ee125-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="ee125-110">Należy unikać konfigurowania dwóch dostawców bazy danych</span><span class="sxs-lookup"><span data-stu-id="ee125-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="ee125-111">W testach zamierzasz skonfigurować zewnętrznie kontekst do użycia dostawcy InMemory.</span><span class="sxs-lookup"><span data-stu-id="ee125-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="ee125-112">Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać kodu warunkowego, upewnij się, tylko Konfigurowanie dostawcy bazy danych, jeśli nie ma już skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="ee125-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="ee125-113">Jeśli używasz platformy ASP.NET Core powinien nie należy tego kodu od dostawcy bazy danych skonfigurowano poza kontekstem (w pliku Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="ee125-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="ee125-114">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="ee125-114">Add a constructor for testing</span></span>

<span data-ttu-id="ee125-115">Najprostszym sposobem, aby umożliwić testowanie z innej bazy danych jest zmodyfikowanie kontekst do udostępnienia konstruktora akceptującego `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ee125-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="ee125-116">`DbContextOptions<TContext>`Określa, że kontekst wszystkie jego ustawienia, takie jak bazy danych do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="ee125-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="ee125-117">Jest to ten sam obiekt skompilowanego za pomocą metody OnConfiguring w kontekście użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ee125-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="ee125-118">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="ee125-118">Writing tests</span></span>

<span data-ttu-id="ee125-119">Kluczem do testowania przy użyciu tego dostawcy jest możliwość Poinformuj kontekstu SQLite i kontrolowanie zakresu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ee125-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="ee125-120">Zakres bazy danych jest kontrolowana przez otwarcie i zamknięcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="ee125-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="ee125-121">Bazy danych jest zakresem czasu trwania, że połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="ee125-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="ee125-122">Zwykle ma czystą bazy danych dla każdej metody.</span><span class="sxs-lookup"><span data-stu-id="ee125-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
