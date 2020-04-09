---
title: Zapytania asynchroniczne — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417754"
---
# <a name="asynchronous-queries"></a>Zapytania asynchroniczne

Kwerendy asynchroniczne uniknąć blokowania wątku, gdy kwerenda jest wykonywana w bazie danych. Zapytania asynchroniowe są ważne dla utrzymania elastycznego interfejsu użytkownika w aplikacjach grubych klientów. Mogą również zwiększyć przepływność w aplikacjach sieci web, gdzie zwalniają wątek do obsługi innych żądań w aplikacjach sieci web. Aby uzyskać więcej informacji, zobacz [Programowanie asynchroniczne w języku C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu równoległych operacji uruchamianych w tym samym wystąpieniu kontekstu. Należy zawsze czekać na zakończenie operacji przed rozpoczęciem następnej operacji. Zazwyczaj odbywa się to `await` przy użyciu słowa kluczowego w każdej operacji asynchronii.

Entity Framework Core zawiera zestaw metod rozszerzenia asynchronicznej podobne do metod LINQ, które wykonują kwerendę i zwracają wyniki. Przykładami `ToListAsync()`są `ToArrayAsync()` `SingleAsync()`, , . Nie ma żadnych asynchronicznej wersji `Where(...)` niektórych operatorów LINQ, takich jak lub `OrderBy(...)` ponieważ te metody tylko budować drzewa wyrażeń LINQ i nie powodują kwerendy do wykonania w bazie danych.

> [!IMPORTANT]  
> Metody rozszerzenia asynchronii EF `Microsoft.EntityFrameworkCore` Core są zdefiniowane w obszarze nazw. Ten obszar nazw musi zostać zaimportowany, aby metody były dostępne.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
