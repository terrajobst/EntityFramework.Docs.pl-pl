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
# <a name="asynchronous-saving"></a>Zapisywanie asynchroniczne

Zapisywanie asynchroniczne pozwala uniknąć blokowania wątku, podczas gdy zmiany są zapisywane w bazie danych. Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta. Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych. Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu. Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji. Jest to zazwyczaj realizowane przy użyciu słowa kluczowego `await` w każdej operacji asynchronicznej.

Entity Framework Core zapewnia `DbContext.SaveChangesAsync()` jako alternatywę asynchroniczną dla `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
