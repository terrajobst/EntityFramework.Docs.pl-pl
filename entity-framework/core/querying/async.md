---
title: Zapytania asynchronicznego — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="1e606-102">Zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="1e606-102">Asynchronous Queries</span></span>

<span data-ttu-id="1e606-103">Zapytania asynchronicznego należy unikać blokowania wątku, podczas wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e606-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="1e606-104">Może to być przydatne w celu uniknięcia zamrażanie interfejs użytkownika aplikacji klienckiej grubości.</span><span class="sxs-lookup"><span data-stu-id="1e606-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="1e606-105">Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym Wątek można zwolnić do obsługi innych żądań podczas operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e606-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="1e606-106">Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="1e606-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="1e606-107">Podstawowe EF nie obsługuje wielu operacji równoległych uruchomione dla tego samego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1e606-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="1e606-108">Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="1e606-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="1e606-109">Jest to zazwyczaj wykonywane za pomocą `await` — słowo kluczowe w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="1e606-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="1e606-110">Program Entity Framework Core udostępnia zestaw metod asynchronicznych rozszerzenia, które mogą być używane jako alternatywę dla metody LINQ, które powodują zapytania do wykonania i wyników zwróconych.</span><span class="sxs-lookup"><span data-stu-id="1e606-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="1e606-111">Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`itp. Nie ma wersji async LINQ operatorów takich jak `Where(...)`, `OrderBy(...)`itp., ponieważ te metody tylko tworzenie drzewa wyrażenia LINQ i nie powodują zapytania do wykonania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e606-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="1e606-112">Metody rozszerzenia asynchroniczne EF Core są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="1e606-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="1e606-113">Ta przestrzeń nazw muszą zostać zaimportowane dla metod, które mają być dostępne.</span><span class="sxs-lookup"><span data-stu-id="1e606-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
