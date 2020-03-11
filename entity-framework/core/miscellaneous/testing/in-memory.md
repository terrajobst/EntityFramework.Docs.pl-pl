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
# <a name="testing-with-inmemory"></a><span data-ttu-id="7cd49-102">Testowanie za pomocą bazy danych InMemory</span><span class="sxs-lookup"><span data-stu-id="7cd49-102">Testing with InMemory</span></span>

<span data-ttu-id="7cd49-103">Dostawca inMemory jest przydatny, gdy chce się testować składniki przy użyciu czegoś przybliżonego łączącego się z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7cd49-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="7cd49-104">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="7cd49-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="7cd49-105">Brak pamięci to relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="7cd49-105">InMemory is not a relational database</span></span>

<span data-ttu-id="7cd49-106">Dostawcy bazy danych EF Core nie muszą być relacyjnymi bazami danych.</span><span class="sxs-lookup"><span data-stu-id="7cd49-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="7cd49-107">Pamięć jest zaprojektowana jako baza danych ogólnego przeznaczenia do testowania i nie jest przeznaczona do naśladowania relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7cd49-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="7cd49-108">Oto kilka przykładów tego przykładu:</span><span class="sxs-lookup"><span data-stu-id="7cd49-108">Some examples of this include:</span></span>

* <span data-ttu-id="7cd49-109">InMemory pozwoli zaoszczędzić dane, które mogłyby naruszać ograniczenia więzów integralności w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7cd49-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="7cd49-110">Jeśli używasz DefaultValueSql (String) dla właściwości w modelu, jest to interfejs API relacyjnej bazy danych i nie będzie mieć żadnego efektu w przypadku uruchamiania w odniesieniu do pamięci.</span><span class="sxs-lookup"><span data-stu-id="7cd49-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="7cd49-111">[Współbieżność za pośrednictwem sygnatury czasowej/wersji wiersza](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` lub `IsRowVersion`) nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="7cd49-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="7cd49-112">Jeśli aktualizacja zostanie wykonana przy użyciu starego tokenu współbieżności, nie zostanie zgłoszony żaden [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .</span><span class="sxs-lookup"><span data-stu-id="7cd49-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="7cd49-113">W wielu celach testowych te różnice nie będą miały znaczenia.</span><span class="sxs-lookup"><span data-stu-id="7cd49-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="7cd49-114">Jeśli jednak chcesz przeprowadzić test względem elementu, który zachowuje się podobnie jak w przypadku prawdziwej relacyjnej bazy danych, rozważ użycie [trybu w pamięci programu SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="7cd49-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="7cd49-115">Przykładowy scenariusz testowania</span><span class="sxs-lookup"><span data-stu-id="7cd49-115">Example testing scenario</span></span>

<span data-ttu-id="7cd49-116">Należy wziąć pod uwagę następujące usługi, dzięki którym kod aplikacji może wykonywać pewne operacje związane ze blogami.</span><span class="sxs-lookup"><span data-stu-id="7cd49-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="7cd49-117">Wewnętrznie używa `DbContext`, które nawiązuje połączenie z bazą danych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7cd49-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="7cd49-118">Warto przystąpić do zamiany tego kontekstu w celu nawiązania połączenia z bazą danych inMemory, dzięki czemu możemy pisać wydajne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonywania wielu zadań, aby utworzyć test w podwójnej części kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7cd49-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="7cd49-119">Przygotuj swój kontekst</span><span class="sxs-lookup"><span data-stu-id="7cd49-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="7cd49-120">Unikaj konfigurowania dwóch dostawców baz danych</span><span class="sxs-lookup"><span data-stu-id="7cd49-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="7cd49-121">W testach zamierzasz zewnętrznie skonfigurować kontekst do korzystania z dostawcy inMemory.</span><span class="sxs-lookup"><span data-stu-id="7cd49-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="7cd49-122">W przypadku konfigurowania dostawcy bazy danych przez zastępowanie `OnConfiguring` w kontekście, należy dodać kod warunkowy, aby upewnić się, że tylko dostawca bazy danych nie został jeszcze skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="7cd49-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="7cd49-123">Jeśli używasz ASP.NET Core, ten kod nie powinien być potrzebny, ponieważ dostawca bazy danych jest już skonfigurowany poza kontekstem (w Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="7cd49-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="7cd49-124">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="7cd49-124">Add a constructor for testing</span></span>

<span data-ttu-id="7cd49-125">Najprostszym sposobem na włączenie testowania względem innej bazy danych jest zmodyfikowanie kontekstu, aby uwidocznić konstruktora, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7cd49-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="7cd49-126">`DbContextOptions<TContext>` informuje kontekst wszystkich jego ustawień, takich jak baza danych, z którą ma zostać nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="7cd49-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="7cd49-127">Jest to ten sam obiekt, który jest zbudowany przez uruchomienie metody onconfiguring w Twoim kontekście.</span><span class="sxs-lookup"><span data-stu-id="7cd49-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="7cd49-128">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="7cd49-128">Writing tests</span></span>

<span data-ttu-id="7cd49-129">Kluczem do testowania za pomocą tego dostawcy jest możliwość poinformowania kontekstu o konieczności użycia dostawcy inMemory i kontrolowania zakresu bazy danych znajdującej się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="7cd49-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="7cd49-130">Zwykle potrzebna jest czysta baza danych dla każdej metody testowej.</span><span class="sxs-lookup"><span data-stu-id="7cd49-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="7cd49-131">Oto przykład klasy testowej, która korzysta z bazy danych inMemory.</span><span class="sxs-lookup"><span data-stu-id="7cd49-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="7cd49-132">Każda metoda testowa określa unikatową nazwę bazy danych, co oznacza, że każda metoda ma własną bazę danych inMemory.</span><span class="sxs-lookup"><span data-stu-id="7cd49-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="7cd49-133">Aby użyć metody rozszerzenia `.UseInMemoryDatabase()`, odwołuje się do pakietu NuGet [Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="7cd49-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
