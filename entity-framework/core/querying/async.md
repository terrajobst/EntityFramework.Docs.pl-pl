---
title: Zapytania asynchroniczne — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417754"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="311e2-102">Zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="311e2-102">Asynchronous Queries</span></span>

<span data-ttu-id="311e2-103">Kwerendy asynchroniczne uniknąć blokowania wątku, gdy kwerenda jest wykonywana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="311e2-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="311e2-104">Zapytania asynchroniowe są ważne dla utrzymania elastycznego interfejsu użytkownika w aplikacjach grubych klientów.</span><span class="sxs-lookup"><span data-stu-id="311e2-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="311e2-105">Mogą również zwiększyć przepływność w aplikacjach sieci web, gdzie zwalniają wątek do obsługi innych żądań w aplikacjach sieci web.</span><span class="sxs-lookup"><span data-stu-id="311e2-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="311e2-106">Aby uzyskać więcej informacji, zobacz [Programowanie asynchroniczne w języku C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="311e2-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="311e2-107">EF Core nie obsługuje wielu równoległych operacji uruchamianych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="311e2-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="311e2-108">Należy zawsze czekać na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="311e2-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="311e2-109">Zazwyczaj odbywa się to `await` przy użyciu słowa kluczowego w każdej operacji asynchronii.</span><span class="sxs-lookup"><span data-stu-id="311e2-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="311e2-110">Entity Framework Core zawiera zestaw metod rozszerzenia asynchronicznej podobne do metod LINQ, które wykonują kwerendę i zwracają wyniki.</span><span class="sxs-lookup"><span data-stu-id="311e2-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="311e2-111">Przykładami `ToListAsync()`są `ToArrayAsync()` `SingleAsync()`, , .</span><span class="sxs-lookup"><span data-stu-id="311e2-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="311e2-112">Nie ma żadnych asynchronicznej wersji `Where(...)` niektórych operatorów LINQ, takich jak lub `OrderBy(...)` ponieważ te metody tylko budować drzewa wyrażeń LINQ i nie powodują kwerendy do wykonania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="311e2-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="311e2-113">Metody rozszerzenia asynchronii EF `Microsoft.EntityFrameworkCore` Core są zdefiniowane w obszarze nazw.</span><span class="sxs-lookup"><span data-stu-id="311e2-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="311e2-114">Ten obszar nazw musi zostać zaimportowany, aby metody były dostępne.</span><span class="sxs-lookup"><span data-stu-id="311e2-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
