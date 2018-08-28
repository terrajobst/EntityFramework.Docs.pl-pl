---
title: Zapisywanie podstawowe - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 23e0e4611f642d59048fca5a808d0782b22caa1e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994804"
---
# <a name="basic-save"></a>Zapisywanie podstawowe

Dowiedz się, jak dodawanie, modyfikowanie i usuwanie danych przy użyciu klas jednostek i kontekstu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="adding-data"></a>Dodawanie danych

Użyj *DbSet.Add* metodę, aby dodać nowe wystąpienia klas jednostek. Dane zostanie wstawiony w bazie danych podczas wywoływania *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody Add, Dołącz i aktualizacji, wszystkie robocze na pełny wykres jednostek jest przekazywany do nich, zgodnie z opisem w [powiązanych danych](related-data.md) sekcji. Alternatywnie właściwość EntityEntry.State może służyć do ustawiania stanu pojedynczej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizowanie danych

EF automatycznie wykrywa zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst. Obejmuje to jednostki, które możesz obciążenia/zapytań z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.

Po prostu zmodyfikuj wartości przypisane do właściwości, a następnie wywołać *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Usuwanie danych

Użyj *DbSet.Remove* metody, można usunąć wystąpień klas jednostek.

Jeśli ta jednostka już istnieje w bazie danych, zostaną usunięte podczas *SaveChanges*. Jeśli jednostka nie został jeszcze zapisany w bazie danych (czyli jego śledzenia dodawania) zostaną usunięte z kontekstu, a nie będzie już wstawiany gdy *SaveChanges* jest wywoływana.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Wiele operacji w jednym SaveChanges

Można połączyć wiele operacji Dodawanie/aktualizowanie/usuwanie na jedno wywołanie *SaveChanges*.

> [!NOTE]  
> W przypadku większości dostawców bazy danych *SaveChanges* jest transakcyjna. Oznacza to wszystkie operacje będą sukcesem lub niepowodzeniem, a operacje nigdy nie lewej częściowo zastosowanych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
