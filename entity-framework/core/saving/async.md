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
# <a name="asynchronous-saving"></a>Zapisywanie asynchroniczne

Asynchroniczne zapisywanie pozwala uniknąć blokowania wątku, gdy zmiany są zapisywane w bazie danych. Może to być przydatne, aby uniknąć zamrożenia interfejsu użytkownika aplikacji grubego klienta. Operacje asynchroniczne można również zwiększyć przepływność w aplikacji sieci web, gdzie wątek może zostać zwolniona do obsługi innych żądań podczas wykonywania operacji bazy danych. Aby uzyskać więcej informacji, zobacz [Programowanie asynchroniczne w języku C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu równoległych operacji uruchamianych w tym samym wystąpieniu kontekstu. Należy zawsze czekać na zakończenie operacji przed rozpoczęciem następnej operacji. Zazwyczaj odbywa się to `await` przy użyciu słowa kluczowego w każdej operacji asynchroniiowej.

Entity Framework `DbContext.SaveChangesAsync()` Core zapewnia asynchroniiową alternatywę `DbContext.SaveChanges()`dla .

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
