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
# <a name="asynchronous-saving"></a>Zapisywanie asynchroniczne

Zapisywanie asynchroniczne pozwala uniknąć blokowania wątku, podczas gdy zmiany są zapisywane w bazie danych. Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta. Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych. Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu. Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji. Zwykle jest to wykonywane przy użyciu `await` słowa kluczowego dla każdej operacji asynchronicznej.

Entity Framework Core stanowi `DbContext.SaveChangesAsync()` alternatywę asynchroniczną dla `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
