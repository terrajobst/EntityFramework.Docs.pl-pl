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
# <a name="testing-with-inmemory"></a><span data-ttu-id="8670c-102">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="8670c-102">Testing with InMemory</span></span>

<span data-ttu-id="8670c-103">Dostawca InMemory jest przydatne, gdy chcesz przetestować składników za pomocą coś, co przybliża z bazą danych rzeczywistych, bez konieczności operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8670c-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="8670c-104">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="8670c-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="8670c-105">InMemory nie jest relacyjnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="8670c-105">InMemory is not a relational database</span></span>

<span data-ttu-id="8670c-106">Dostawcy baz danych programu EF Core nie trzeba być relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="8670c-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="8670c-107">InMemory została zaprojektowana jako bazy danych ogólnego przeznaczenia do testowania i nie jest przeznaczony do naśladowania relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8670c-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="8670c-108">Niektóre przykłady to między innymi:</span><span class="sxs-lookup"><span data-stu-id="8670c-108">Some examples of this include:</span></span>

* <span data-ttu-id="8670c-109">InMemory pozwoli na zapisywanie danych, który mógłby naruszyć ograniczenia integralności referencyjnej w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8670c-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="8670c-110">Jeśli używasz DefaultValueSql(string) właściwości w modelu jest interfejs API relacyjnej bazy danych i nie odniesie skutku przy uruchamianiu InMemory.</span><span class="sxs-lookup"><span data-stu-id="8670c-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="8670c-111">[Współbieżność za pośrednictwem wiersza i znacznik czasu: wersja](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` lub `IsRowVersion`) nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="8670c-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="8670c-112">Nie [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) zostanie zgłoszony, jeśli aktualizacja jest wykonywane przy użyciu starego tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8670c-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="8670c-113">Różnice te nie będą znaczenia, dla wielu celów testowych.</span><span class="sxs-lookup"><span data-stu-id="8670c-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="8670c-114">Jednak jeśli chcesz przetestować względem elementu, który zachowuje się bardziej jak true relacyjnej bazy danych, rozważ użycie [trybie w pamięci z bazy danych SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="8670c-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="8670c-115">Przykładowy scenariusz testowania</span><span class="sxs-lookup"><span data-stu-id="8670c-115">Example testing scenario</span></span>

<span data-ttu-id="8670c-116">Należy wziąć pod uwagę następujące usługa, która umożliwia wykonywanie operacji związanych z blogów kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8670c-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="8670c-117">Używa wewnętrznie `DbContext` łączący się z bazą danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8670c-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="8670c-118">Należałoby wymiany z tym kontekstem do nawiązania połączenia z bazą InMemory tak, aby firma Microsoft można zapisać efektywne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonać dużo pracy, aby utworzyć test podwójnego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="8670c-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="8670c-119">Przygotuj kontekstu</span><span class="sxs-lookup"><span data-stu-id="8670c-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="8670c-120">Należy unikać konfigurowania dwóch dostawców bazy danych</span><span class="sxs-lookup"><span data-stu-id="8670c-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="8670c-121">W testach ma zewnętrznie skonfigurować kontekst do użycia dostawcy InMemory.</span><span class="sxs-lookup"><span data-stu-id="8670c-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="8670c-122">Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać niektórych kod warunkowy, aby upewnić się, można tylko skonfigurować dostawcy bazy danych, gdy nie została już skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="8670c-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="8670c-123">Jeśli używasz platformy ASP.NET Core, następnie nie należy tego kodu od dostawcy bazy danych jest już skonfigurowana poza kontekstem (w pliku Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="8670c-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="8670c-124">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="8670c-124">Add a constructor for testing</span></span>

<span data-ttu-id="8670c-125">Najprostszym sposobem, aby umożliwić testowanie z inną bazą danych jest zmodyfikowanie kontekst ujawniać Konstruktor, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8670c-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="8670c-126">`DbContextOptions<TContext>` informuje kontekstu, wszystkie jego ustawienia, takie jak której bazy danych, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="8670c-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="8670c-127">Jest to ten sam obiekt, który jest wbudowana, uruchamiając metoda OnConfiguring w kontekście usługi.</span><span class="sxs-lookup"><span data-stu-id="8670c-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="8670c-128">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="8670c-128">Writing tests</span></span>

<span data-ttu-id="8670c-129">Kluczem do testowania przy użyciu tego dostawcy jest możliwość opowiadania kontekst do użycia dostawcy InMemory i kontrolowanie zakresu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8670c-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="8670c-130">Zazwyczaj chcesz czyste bazy danych dla każdej metody testowej.</span><span class="sxs-lookup"><span data-stu-id="8670c-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="8670c-131">Oto przykład klasy testowej, korzystającej z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8670c-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="8670c-132">Każdej metody testowej Określa unikatową nazwę bazy danych, co oznacza, że każda metoda charakteryzuje się własną bazę danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8670c-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="8670c-133">Aby użyć `.UseInMemoryDatabase()` metodę rozszerzenia, odwołanie do pakietu NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="8670c-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
