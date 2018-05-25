---
title: Zapytania asynchronicznego — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Zapytania asynchroniczne

Zapytania asynchronicznego należy unikać blokowania wątku, podczas wykonywania zapytania w bazie danych. Może to być przydatne w celu uniknięcia zamrażanie interfejs użytkownika aplikacji klienckiej grubości. Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym Wątek można zwolnić do obsługi innych żądań podczas operacji bazy danych. Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Podstawowe EF nie obsługuje wielu operacji równoległych uruchomione dla tego samego wystąpienia kontekstu. Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji. Jest to zazwyczaj wykonywane za pomocą `await` — słowo kluczowe w każdej operacji asynchronicznej.

Program Entity Framework Core udostępnia zestaw metod asynchronicznych rozszerzenia, które mogą być używane jako alternatywę dla metody LINQ, które powodują zapytania do wykonania i wyników zwróconych. Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`itp. Nie ma wersji async LINQ operatorów takich jak `Where(...)`, `OrderBy(...)`itp., ponieważ te metody tylko tworzenie drzewa wyrażenia LINQ i nie powodują zapytania do wykonania w bazie danych.

> [!IMPORTANT]  
> Metody rozszerzenia asynchroniczne EF Core są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw. Ta przestrzeń nazw muszą zostać zaimportowane dla metod, które mają być dostępne.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
