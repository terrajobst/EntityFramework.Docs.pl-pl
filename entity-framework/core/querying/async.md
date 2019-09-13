---
title: Asynchroniczne zapytania — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921696"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="87582-102">Zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="87582-102">Asynchronous Queries</span></span>

<span data-ttu-id="87582-103">Zapytania asynchroniczne zapobiegają zablokowaniu wątku w czasie wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="87582-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="87582-104">Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta.</span><span class="sxs-lookup"><span data-stu-id="87582-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="87582-105">Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="87582-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="87582-106">Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="87582-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="87582-107">EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="87582-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="87582-108">Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="87582-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="87582-109">Zwykle jest to wykonywane przy użyciu `await` słowa kluczowego dla każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="87582-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="87582-110">Entity Framework Core udostępnia zestaw asynchronicznych metod rozszerzających, które mogą być używane jako alternatywa dla metod LINQ, które powodują wykonanie zapytania i zwracanych wyników.</span><span class="sxs-lookup"><span data-stu-id="87582-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="87582-111">Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, itd. Nie istnieją asynchroniczne wersje operatorów LINQ, takie jak `Where(...)`, `OrderBy(...)`itp., ponieważ te metody kompilują tylko drzewo wyrażeń LINQ i nie powodują wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="87582-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="87582-112">Metody rozszerzenia asynchronicznego EF Core są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="87582-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="87582-113">Ta przestrzeń nazw musi zostać zaimportowana, aby można było korzystać z metod.</span><span class="sxs-lookup"><span data-stu-id="87582-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
