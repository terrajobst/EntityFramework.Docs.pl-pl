---
title: Zapisywanie asynchroniczne — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197823"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="448f8-102">Zapisywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="448f8-102">Asynchronous Saving</span></span>

<span data-ttu-id="448f8-103">Zapisywanie asynchroniczne pozwala uniknąć blokowania wątku, podczas gdy zmiany są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="448f8-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="448f8-104">Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta.</span><span class="sxs-lookup"><span data-stu-id="448f8-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="448f8-105">Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="448f8-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="448f8-106">Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="448f8-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="448f8-107">EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="448f8-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="448f8-108">Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji.</span><span class="sxs-lookup"><span data-stu-id="448f8-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="448f8-109">Zwykle jest to wykonywane przy użyciu `await` słowa kluczowego dla każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="448f8-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="448f8-110">Entity Framework Core stanowi `DbContext.SaveChangesAsync()` alternatywę asynchroniczną dla `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="448f8-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
