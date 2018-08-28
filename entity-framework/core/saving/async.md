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
# <a name="asynchronous-saving"></a>Zapisywanie asynchroniczne

Zapisywanie asynchroniczne unika blokuje wątku, a zmiany są zapisywane w bazie danych. Może to być przydatne w celu uniknięcia zawiesza się interfejs użytkownika aplikacji klienckiej grubości. Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym wątku można zwolnić, do obsługi innych żądań podczas kończenia operacji bazy danych. Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych, które są uruchamiane na tym samym wystąpieniu kontekstu. Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji. Zazwyczaj jest to wykonywane przy użyciu `await` słowo kluczowe w każdej operacji asynchronicznej.

Udostępnia platformy Entity Framework Core `DbContext.SaveChangesAsync()` asynchronicznego zamiast `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
