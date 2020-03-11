---
title: Zapisywanie asynchroniczne — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417643"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="d86c6-102">Zapisywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="d86c6-102">Asynchronous Saving</span></span>

<span data-ttu-id="d86c6-103">Zapisywanie asynchroniczne pozwala uniknąć blokowania wątku, podczas gdy zmiany są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d86c6-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="d86c6-104">Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta.</span><span class="sxs-lookup"><span data-stu-id="d86c6-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="d86c6-105">Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d86c6-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="d86c6-106">Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="d86c6-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="d86c6-107">EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d86c6-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="d86c6-108">Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="d86c6-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="d86c6-109">Jest to zazwyczaj realizowane przy użyciu słowa kluczowego `await` w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="d86c6-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="d86c6-110">Entity Framework Core zapewnia `DbContext.SaveChangesAsync()` jako alternatywę asynchroniczną dla `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="d86c6-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
