---
title: Asynchroniczne zapytania — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921696"
---
# <a name="asynchronous-queries"></a>Zapytania asynchroniczne

Zapytania asynchroniczne zapobiegają zablokowaniu wątku w czasie wykonywania zapytania w bazie danych. Może to być przydatne, aby uniknąć zamarzania interfejsu użytkownika aplikacji z szeroką obsługą klienta. Operacje asynchroniczne mogą również zwiększyć przepływność w aplikacji sieci Web, gdzie wątek może zostać zwolniony do obsługi innych żądań podczas kończenia operacji bazy danych. Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu. Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji. Zwykle jest to wykonywane przy użyciu `await` słowa kluczowego dla każdej operacji asynchronicznej.

Entity Framework Core udostępnia zestaw asynchronicznych metod rozszerzających, które mogą być używane jako alternatywa dla metod LINQ, które powodują wykonanie zapytania i zwracanych wyników. Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, itd. Nie istnieją asynchroniczne wersje operatorów LINQ, takie jak `Where(...)`, `OrderBy(...)`itp., ponieważ te metody kompilują tylko drzewo wyrażeń LINQ i nie powodują wykonywania zapytania w bazie danych.

> [!IMPORTANT]  
> Metody rozszerzenia asynchronicznego EF Core są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw. Ta przestrzeń nazw musi zostać zaimportowana, aby można było korzystać z metod.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
