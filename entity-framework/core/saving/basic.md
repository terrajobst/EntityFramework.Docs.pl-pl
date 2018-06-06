---
title: Zapisz podstawowy — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: 35bf14af43289ad6308a49482d3f45a7a8be9067
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754394"
---
# <a name="basic-save"></a>Zapisz podstawowy

Informacje o sposobie dodawania, modyfikowania i usuwania danych przy użyciu klas kontekstu i jednostek.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) w witrynie GitHub.

## <a name="adding-data"></a>Dodawanie danych

Użyj *DbSet.Add* metody w celu dodania nowych wystąpień klas jednostek. Dane zostanie wstawiony w bazie danych podczas wywoływania *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody Add, Dołącz i aktualizacji wszystkie działają na wykresie pełne jednostek przekazany do nich, zgodnie z opisem w [dane dotyczące](related-data.md) sekcji. Alternatywnie EntityEntry.State właściwości można ustawić stanu pojedynczej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizowanie danych

EF automatycznie wykrywa zmiany wprowadzone do istniejącego obiektu, które są śledzone przez kontekst. W tym jednostki, które możesz obciążenia/zapytania z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.

Po prostu zmodyfikuj wartości dla właściwości, a następnie wywołać *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Usuwanie danych

Użyj *DbSet.Remove* metodę, aby usunąć wystąpienia klas jednostek.

Jeśli ta jednostka już istnieje w bazie danych, zostaną usunięte podczas *SaveChanges*. Jeśli jednostka nie został jeszcze zapisany w bazie danych (np. jego śledzenia dodany), a następnie zostaną usunięte z kontekstu i nie będzie już wstawiony, kiedy *SaveChanges* jest wywoływana.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Wiele operacji w jednej metody SaveChanges

Można połączyć wiele operacji dodawania/aktualizacji/usuwania w jednym wywołaniu *SaveChanges*.

> [!NOTE]  
> W przypadku dostawców większości bazy danych *SaveChanges* jest transakcyjna. Oznacza to, wszystkie operacje będą powodzenie lub Niepowodzenie i operacje będą nigdy nie lewej częściowo stosowane.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
