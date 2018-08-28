---
title: Zapisywanie asynchroniczne — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997285"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="1c0aa-102">Zapisywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="1c0aa-102">Asynchronous Saving</span></span>

<span data-ttu-id="1c0aa-103">Zapisywanie asynchroniczne unika blokuje wątku, a zmiany są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="1c0aa-104">Może to być przydatne w celu uniknięcia zawiesza się interfejs użytkownika aplikacji klienckiej grubości.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="1c0aa-105">Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym wątku można zwolnić, do obsługi innych żądań podczas kończenia operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="1c0aa-106">Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="1c0aa-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="1c0aa-107">EF Core nie obsługuje wielu operacji równoległych, które są uruchamiane na tym samym wystąpieniu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="1c0aa-108">Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="1c0aa-109">Zazwyczaj jest to wykonywane przy użyciu `await` słowo kluczowe w każdej operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="1c0aa-110">Udostępnia platformy Entity Framework Core `DbContext.SaveChangesAsync()` asynchronicznego zamiast `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="1c0aa-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
