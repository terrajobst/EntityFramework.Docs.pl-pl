---
title: Zapytania asynchroniczne — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993568"
---
# <a name="asynchronous-queries"></a>Zapytania asynchroniczne

Zapytania asynchroniczne uniknąć blokowania wątku podczas wykonywania zapytania w bazie danych. Może to być przydatne w celu uniknięcia zawiesza się interfejs użytkownika aplikacji klienckiej grubości. Operacje asynchroniczne także może zwiększyć przepływność w aplikacji sieci web, w którym wątku można zwolnić, do obsługi innych żądań podczas kończenia operacji bazy danych. Aby uzyskać więcej informacji, zobacz [asynchronicznego programowania w języku C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych, które są uruchamiane na tym samym wystąpieniu kontekstu. Należy zawsze poczekaj na zakończenie operacji przed rozpoczęciem następnej operacji. Zazwyczaj jest to wykonywane przy użyciu `await` słowo kluczowe w każdej operacji asynchronicznej.

Entity Framework Core udostępnia zestaw metod asynchronicznego rozszerzenia, które mogą być używane jako alternatywę dla metody LINQ, które powodują zapytanie do wykonania i zwracania wyników. Przykłady obejmują `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`itp. Brak wersji asynchronicznej operatorów LINQ takich jak `Where(...)`, `OrderBy(...)`, itp., ponieważ te metody tylko zbudowania drzewa wyrażeń LINQ i nie powodują zapytanie do wykonania w bazie danych.

> [!IMPORTANT]  
> Metody rozszerzenia programu EF Core asynchroniczne są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw. Ta przestrzeń nazw, należy zaimportować dla metod, które mają być dostępne.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
