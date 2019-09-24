---
title: Zapisywanie podstawowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 6f72458504a9dbe99038af7cfd23b6991258f6b8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197779"
---
# <a name="basic-save"></a>Zapisywanie podstawowe

Dowiedz się, jak dodawać, modyfikować i usuwać dane przy użyciu własnych klas kontekstu i jednostek.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="adding-data"></a>Dodawanie danych

Użyj metody *nieogólnymi. Add* , aby dodać nowe wystąpienia klas jednostek. Dane zostaną wstawione do bazy danych po wywołaniu *metody SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody dodawania, dołączania i aktualizowania działają na pełnym wykresie obiektów, do których przekazały, zgodnie z opisem w sekcji [powiązane dane](related-data.md) . Alternatywnie Właściwość EntityEntry. State może służyć do ustawiania stanu tylko pojedynczej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizowanie danych

EF automatycznie wykrywa zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst. Obejmuje to jednostki, które zostały załadowane z bazy danych, oraz obiekty, które zostały wcześniej dodane i zapisane w bazie danych.

Po prostu Zmodyfikuj wartości przypisane do właściwości, a następnie Wywołaj *metody SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Usuwanie danych

Użyj metody *nieogólnymi. Remove* , aby usunąć wystąpienia klas jednostek.

Jeśli jednostka już istnieje w bazie danych, zostanie usunięta podczas *metody SaveChanges*. Jeśli obiekt nie został jeszcze zapisany w bazie danych (jest śledzony jako dodany), zostanie usunięty z kontekstu i nie będzie już wstawiany, gdy *metody SaveChanges* jest wywoływana.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Wiele operacji w jednym metody SaveChanges

Można połączyć wiele operacji dodawania/aktualizowania/usuwania w jedno wywołanie do *metody SaveChanges*.

> [!NOTE]  
> W przypadku większości dostawców baz danych *metody SaveChanges* jest transakcyjna. Oznacza to, że wszystkie operacje zakończą się powodzeniem lub niepowodzeniem, a operacje nigdy nie zostaną zastosowane częściowo.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
