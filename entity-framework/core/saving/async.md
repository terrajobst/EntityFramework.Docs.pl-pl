---
title: Oszczędzanie asynchroniczne - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417643"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="1b29e-102">Zapisywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="1b29e-102">Asynchronous Saving</span></span>

<span data-ttu-id="1b29e-103">Asynchroniczne zapisywanie pozwala uniknąć blokowania wątku, gdy zmiany są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1b29e-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="1b29e-104">Może to być przydatne, aby uniknąć zamrożenia interfejsu użytkownika aplikacji grubego klienta.</span><span class="sxs-lookup"><span data-stu-id="1b29e-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="1b29e-105">Operacje asynchroniczne można również zwiększyć przepływność w aplikacji sieci web, gdzie wątek może zostać zwolniona do obsługi innych żądań podczas wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1b29e-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="1b29e-106">Aby uzyskać więcej informacji, zobacz [Programowanie asynchroniczne w języku C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="1b29e-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="1b29e-107">EF Core nie obsługuje wielu równoległych operacji uruchamianych w tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1b29e-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="1b29e-108">Należy zawsze czekać na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b29e-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="1b29e-109">Zazwyczaj odbywa się to `await` przy użyciu słowa kluczowego w każdej operacji asynchroniiowej.</span><span class="sxs-lookup"><span data-stu-id="1b29e-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="1b29e-110">Entity Framework `DbContext.SaveChangesAsync()` Core zapewnia asynchroniiową alternatywę `DbContext.SaveChanges()`dla .</span><span class="sxs-lookup"><span data-stu-id="1b29e-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
