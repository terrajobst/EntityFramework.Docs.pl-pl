---
title: Zapisywanie asynchroniczne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="ee4e1-102">Zapisywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="ee4e1-102">Asynchronous Saving</span></span>

<span data-ttu-id="ee4e1-103">Zapisywanie asynchroniczne pozwala uniknąć blokuje wątku podczas zmiany są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="ee4e1-104">Może to być przydatne w celu uniknięcia zamrażanie interfejs użytkownika aplikacji klienckiej grubości.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="ee4e1-105">Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym Wątek można zwolnić do obsługi innych żądań podczas operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="ee4e1-106">Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="ee4e1-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="ee4e1-107">Podstawowe EF nie obsługuje wielu operacji równoległych uruchomione dla tego samego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="ee4e1-108">Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="ee4e1-109">Jest to zazwyczaj wykonywane za pomocą `await` — słowo kluczowe w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="ee4e1-110">Udostępnia Entity Framework Core `DbContext.SaveChangesAsync()` asynchroniczne zamiast `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="ee4e1-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
