---
title: Asynchroniczne zapytania — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417754"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="d97f9-102">Zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="d97f9-102">Asynchronous Queries</span></span>

<span data-ttu-id="d97f9-103">Zapytania asynchroniczne zapobiegają zablokowaniu wątku w czasie wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d97f9-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="d97f9-104">Zapytania asynchroniczne są ważne, aby zachować interfejs użytkownika odpowiadający w aplikacjach z szeroką obsługą klienta.</span><span class="sxs-lookup"><span data-stu-id="d97f9-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="d97f9-105">Mogą również zwiększyć przepływność w aplikacjach sieci Web, gdy zwalniają wątek, aby obsłużyć inne żądania w aplikacjach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d97f9-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="d97f9-106">Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="d97f9-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="d97f9-107">EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d97f9-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="d97f9-108">Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="d97f9-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="d97f9-109">Jest to zazwyczaj realizowane przy użyciu słowa kluczowego `await` w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="d97f9-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="d97f9-110">Entity Framework Core udostępnia zestaw asynchronicznych metod rozszerzających podobne do metod LINQ, które wykonują zapytanie i zwracają wyniki.</span><span class="sxs-lookup"><span data-stu-id="d97f9-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="d97f9-111">Przykładami mogą być `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="d97f9-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="d97f9-112">Nie ma żadnych asynchronicznych wersji niektórych operatorów LINQ, takich jak `Where(...)` lub `OrderBy(...)`, ponieważ te metody kompilują tylko drzewo wyrażeń LINQ i nie powodują wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d97f9-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d97f9-113">Metody rozszerzeń asynchronicznych EF Core są zdefiniowane w przestrzeni nazw `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="d97f9-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="d97f9-114">Ta przestrzeń nazw musi zostać zaimportowana, aby można było korzystać z metod.</span><span class="sxs-lookup"><span data-stu-id="d97f9-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
