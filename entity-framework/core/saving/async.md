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
# <a name="asynchronous-saving"></a>Zapisywanie asynchroniczne

Zapisywanie asynchroniczne pozwala uniknąć blokuje wątku podczas zmiany są zapisywane w bazie danych. Może to być przydatne w celu uniknięcia zamrażanie interfejs użytkownika aplikacji klienckiej grubości. Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym Wątek można zwolnić do obsługi innych żądań podczas operacji bazy danych. Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Podstawowe EF nie obsługuje wielu operacji równoległych uruchomione dla tego samego wystąpienia kontekstu. Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji. Jest to zazwyczaj wykonywane za pomocą `await` — słowo kluczowe w każdej operacji asynchronicznej.

Udostępnia Entity Framework Core `DbContext.SaveChangesAsync()` asynchroniczne zamiast `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
