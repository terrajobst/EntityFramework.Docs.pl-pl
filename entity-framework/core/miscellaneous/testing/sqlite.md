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
# <a name="testing-with-sqlite"></a><span data-ttu-id="c07cf-102">Testowanie za pomocą SQLite</span><span class="sxs-lookup"><span data-stu-id="c07cf-102">Testing with SQLite</span></span>

<span data-ttu-id="c07cf-103">Bazy danych SQLite ma trybie w pamięci, która pozwala na potrzeby pisania testów przeciwko relacyjnej bazy danych bez konieczności operacji bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="c07cf-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="c07cf-104">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="c07cf-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="c07cf-105">Przykładowy scenariusz testowania</span><span class="sxs-lookup"><span data-stu-id="c07cf-105">Example testing scenario</span></span>

<span data-ttu-id="c07cf-106">Należy wziąć pod uwagę następujące usługa, która umożliwia wykonywanie operacji związanych z blogów kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c07cf-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="c07cf-107">Używa wewnętrznie `DbContext` łączący się z bazą danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c07cf-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="c07cf-108">Należałoby wymiany tego kontekstu, aby połączyć się z bazą danych SQLite w pamięci, możemy napisać efektywne testy dla tej usługi bez konieczności modyfikowania kodu lub wykonać dużo pracy, aby utworzyć test podwójnego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="c07cf-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="c07cf-109">Przygotuj kontekstu</span><span class="sxs-lookup"><span data-stu-id="c07cf-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="c07cf-110">Należy unikać konfigurowania dwóch dostawców bazy danych</span><span class="sxs-lookup"><span data-stu-id="c07cf-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="c07cf-111">W testach ma zewnętrznie skonfigurować kontekst do użycia dostawcy InMemory.</span><span class="sxs-lookup"><span data-stu-id="c07cf-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="c07cf-112">Jeśli konfigurujesz dostawcy bazy danych przez zastąpienie `OnConfiguring` w kontekstu, następnie należy dodać niektórych kod warunkowy, aby upewnić się, można tylko skonfigurować dostawcy bazy danych, gdy nie została już skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="c07cf-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="c07cf-113">Jeśli używasz platformy ASP.NET Core, następnie nie należy tego kodu od dostawcy bazy danych skonfigurowano poza kontekstem (w pliku Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="c07cf-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="c07cf-114">Dodaj Konstruktor do testowania</span><span class="sxs-lookup"><span data-stu-id="c07cf-114">Add a constructor for testing</span></span>

<span data-ttu-id="c07cf-115">Najprostszym sposobem, aby umożliwić testowanie z inną bazą danych jest zmodyfikowanie kontekst ujawniać Konstruktor, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c07cf-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="c07cf-116">`DbContextOptions<TContext>` informuje kontekstu, wszystkie jego ustawienia, takie jak której bazy danych, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="c07cf-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="c07cf-117">Jest to ten sam obiekt, który jest wbudowana, uruchamiając metoda OnConfiguring w kontekście usługi.</span><span class="sxs-lookup"><span data-stu-id="c07cf-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="c07cf-118">Pisanie testów</span><span class="sxs-lookup"><span data-stu-id="c07cf-118">Writing tests</span></span>

<span data-ttu-id="c07cf-119">Kluczem do testowania przy użyciu tego dostawcy jest możliwość opowiadania kontekstu do używania bazy danych SQLite i kontrolowanie zakresu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="c07cf-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="c07cf-120">Zakres bazy danych jest kontrolowana przez otwierające i zamykające połączenia.</span><span class="sxs-lookup"><span data-stu-id="c07cf-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="c07cf-121">Baza danych jest ograniczony do czasu trwania, że połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="c07cf-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="c07cf-122">Zazwyczaj chcesz czyste bazy danych dla każdej metody testowej.</span><span class="sxs-lookup"><span data-stu-id="c07cf-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
