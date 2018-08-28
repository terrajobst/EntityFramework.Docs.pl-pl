---
title: Zapytania asynchroniczne — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993568"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="90bfa-102">Zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="90bfa-102">Asynchronous Queries</span></span>

<span data-ttu-id="90bfa-103">Zapytania asynchroniczne uniknąć blokowania wątku podczas wykonywania zapytania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="90bfa-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="90bfa-104">Może to być przydatne w celu uniknięcia zawiesza się interfejs użytkownika aplikacji klienckiej grubości.</span><span class="sxs-lookup"><span data-stu-id="90bfa-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="90bfa-105">Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym wątku można zwolnić, do obsługi innych żądań podczas kończenia operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90bfa-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="90bfa-106">Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="90bfa-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="90bfa-107">EF Core nie obsługuje wielu operacji równoległych, które są uruchamiane na tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="90bfa-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="90bfa-108">Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="90bfa-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="90bfa-109">Zazwyczaj jest to wykonywane przy użyciu `await` słowo kluczowe w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="90bfa-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="90bfa-110">Entity Framework Core udostępnia zestaw metod asynchronicznego rozszerzenia, które mogą być używane jako alternatywę dla metody LINQ, które powodują zapytanie do wykonania i zwracania wyników.</span><span class="sxs-lookup"><span data-stu-id="90bfa-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="90bfa-111">Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`itp. Brak wersji asynchronicznej operatorów LINQ takich jak `Where(...)`, `OrderBy(...)`, itp., ponieważ te metody tylko zbudowania drzewa wyrażeń LINQ i nie powodują zapytanie do wykonania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="90bfa-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="90bfa-112">Metody rozszerzenia programu EF Core asynchroniczne są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="90bfa-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="90bfa-113">Ta przestrzeń nazw, należy zaimportować dla metod, które mają być dostępne.</span><span class="sxs-lookup"><span data-stu-id="90bfa-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
