---
title: Asynchroniczne zapytania — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417754"
---
# <a name="asynchronous-queries"></a>Zapytania asynchroniczne

Zapytania asynchroniczne zapobiegają zablokowaniu wątku w czasie wykonywania zapytania w bazie danych. Zapytania asynchroniczne są ważne, aby zachować interfejs użytkownika odpowiadający w aplikacjach z szeroką obsługą klienta. Mogą również zwiększyć przepływność w aplikacjach sieci Web, gdy zwalniają wątek, aby obsłużyć inne żądania w aplikacjach sieci Web. Aby uzyskać więcej informacji, zobacz [programowanie asynchroniczne C#w ](/dotnet/csharp/async).

> [!WARNING]  
> EF Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym wystąpieniu kontekstu. Przed rozpoczęciem następnej operacji zawsze należy czekać na ukończenie operacji. Jest to zazwyczaj realizowane przy użyciu słowa kluczowego `await` w każdej operacji asynchronicznej.

Entity Framework Core udostępnia zestaw asynchronicznych metod rozszerzających podobne do metod LINQ, które wykonują zapytanie i zwracają wyniki. Przykładami mogą być `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`. Nie ma żadnych asynchronicznych wersji niektórych operatorów LINQ, takich jak `Where(...)` lub `OrderBy(...)`, ponieważ te metody kompilują tylko drzewo wyrażeń LINQ i nie powodują wykonywania zapytania w bazie danych.

> [!IMPORTANT]  
> Metody rozszerzeń asynchronicznych EF Core są zdefiniowane w przestrzeni nazw `Microsoft.EntityFrameworkCore`. Ta przestrzeń nazw musi zostać zaimportowana, aby można było korzystać z metod.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
