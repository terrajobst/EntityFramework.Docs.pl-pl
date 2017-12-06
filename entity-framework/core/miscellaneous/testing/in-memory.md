---
title: "Testowanie za pomocą InMemory - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="28074-102">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="28074-102">Testing with InMemory</span></span>

<span data-ttu-id="28074-103">Dostawca InMemory jest przydatne, gdy chcesz przetestować składników za pomocą coś zbliżony łączenia z bazą danych rzeczywistych, bez potrzeby operacje rzeczywistej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="28074-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="28074-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="28074-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="28074-105">InMemory nie jest relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="28074-105">InMemory is not a relational database</span></span>

<span data-ttu-id="28074-106">EF podstawowej bazy danych dostawców ma być relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="28074-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="28074-107">InMemory została zaprojektowana jako ogólnego przeznaczenia bazy danych w celu testowania i nie jest przeznaczony do naśladować relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="28074-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="28074-108">Przykłady to między innymi:</span><span class="sxs-lookup"><span data-stu-id="28074-108">Some examples of this include:</span></span>
* <span data-ttu-id="28074-109">InMemory umożliwia zapisują dane, który mógłby naruszyć ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="28074-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="28074-110">Jeśli używasz DefaultValueSql(string) właściwości w modelu, jest interfejs API relacyjnej bazy danych, nie odniesie żadnego skutku podczas uruchamiania InMemory.</span><span class="sxs-lookup"><span data-stu-id="28074-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="28074-111">Dla wielu testów do celów różnice te nie będą znaczenia.</span><span class="sxs-lookup"><span data-stu-id="28074-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="28074-112">Jednak jeśli chcesz przetestować coś, co bardziej zachowuje się jak true relacyjnej bazy danych, należy rozważyć użycie [trybu w pamięci SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="28074-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="28074-113">Przykładowy scenariusz testowych</span><span class="sxs-lookup"><span data-stu-id="28074-113">Example testing scenario</span></span>

<span data-ttu-id="28074-114">Należy wziąć pod uwagę następujące usługi, która umożliwia aplikacji kod, aby wykonać pewne operacje związane z blogów.</span><span class="sxs-lookup"><span data-stu-id="28074-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="28074-115">Wewnętrznie używa `DbContext` które nawiązuje połączenie z bazą danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="28074-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="28074-116">Należałoby wymiany z tym kontekstem do łączenia z bazą danych InMemory, tak aby nie możemy zapisać wydajne testów dla tej usługi bez konieczności modyfikowania kodu lub dużo pracy w celu utworzenia testu podwójne kontekstu.</span><span class="sxs-lookup"><span data-stu-id="28074-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="28074-117">Przygotowanie kontekst</span><span class="sxs-lookup"><span data-stu-id="28074-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="28074-118">Należy unikać konfigurowania dwóch dostawców bazy danych</span><span class="sxs-lookup"><span data-stu-id="28074-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="28074-119">W testach zamierzasz skonfigurować zewnętrznie kontekst do użycia dostawcy InMemory.</span><span class="sxs-lookup"><span data-stu-id="28074-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="28074-120">Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać kodu warunkowego, upewnij się, tylko Konfigurowanie dostawcy bazy danych, jeśli nie ma już skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="28074-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="28074-121">Jeśli używasz platformy ASP.NET Core powinien nie należy tego kodu od dostawcy bazy danych jest już skonfigurowana poza kontekstem (w pliku Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="28074-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="28074-122">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="28074-122">Add a constructor for testing</span></span>

<span data-ttu-id="28074-123">Najprostszym sposobem, aby umożliwić testowanie z innej bazy danych jest zmodyfikowanie kontekst do udostępnienia konstruktora akceptującego `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="28074-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="28074-124">`DbContextOptions<TContext>`Określa, że kontekst wszystkie jego ustawienia, takie jak bazy danych do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="28074-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="28074-125">Jest to ten sam obiekt skompilowanego za pomocą metody OnConfiguring w kontekście użytkownika.</span><span class="sxs-lookup"><span data-stu-id="28074-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="28074-126">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="28074-126">Writing tests</span></span>

<span data-ttu-id="28074-127">Kluczem do testowania przy użyciu tego dostawcy jest możliwość Poinformuj kontekst do używania dostawcy InMemory i kontrolowanie zakresu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="28074-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="28074-128">Zwykle ma czystą bazy danych dla każdej metody.</span><span class="sxs-lookup"><span data-stu-id="28074-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="28074-129">Oto przykład klasa testowa, która używa bazy danych InMemory.</span><span class="sxs-lookup"><span data-stu-id="28074-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="28074-130">Każda metoda testu określa unikatową nazwę bazy danych, co oznacza, że każda metoda charakteryzuje się własną bazę danych InMemory.</span><span class="sxs-lookup"><span data-stu-id="28074-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="28074-131">Aby użyć `.UseInMemoryDatabase()` — metoda rozszerzenia, odwołanie do pakietu Nuget `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="28074-131">To use the `.UseInMemoryDatabase()` extension method, reference the Nuget package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
