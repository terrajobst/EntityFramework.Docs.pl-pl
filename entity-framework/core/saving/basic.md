---
title: Zapis podstawowy — ef core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417636"
---
# <a name="basic-save"></a>Zapisywanie podstawowe

Dowiedz się, jak dodawać, modyfikować i usuwać dane przy użyciu klas kontekstu i encji.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) przykład artykułu na GitHub.

## <a name="adding-data"></a>Dodawanie danych

Użyj *DbSet.Add* metody, aby dodać nowe wystąpienia klas jednostek. Dane zostaną wstawione do bazy danych podczas wywoływania *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody Dodaj, Dołącz i Aktualizuj wszystkie działają na pełnym wykresie jednostek przekazanych do nich, zgodnie z opisem w sekcji [Dane pokrewne.](related-data.md) Alternatywnie EntityEntry.State właściwość może służyć do ustawiania stanu tylko jednej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizowanie danych

EF automatycznie wykryje zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst. Obejmuje to jednostki, które można załadować/kwerendy z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.

Wystarczy zmodyfikować wartości przypisane do właściwości, a następnie *wywołać SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Usuwanie danych

Użyj *DbSet.Remove* metody, aby usunąć wystąpienia klas jednostek.

Jeśli jednostka już istnieje w bazie danych, zostanie usunięta podczas *savechanges*. Jeśli jednostka nie została jeszcze zapisana w bazie danych (oznacza to, że jest śledzona jako dodana), zostanie ona usunięta z kontekstu i nie będzie już wstawiana po *wywołaniu SaveChanges.*

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Wiele operacji w jednym savechanges

Można połączyć wiele operacji Dodaj/Aktualizuj/Usuń w jedno wywołanie *SaveChanges*.

> [!NOTE]  
> Dla większości dostawców bazy danych *SaveChanges* jest transakcyjnych. Oznacza to, że wszystkie operacje zakończy się powodzeniem lub niepowodzeniem, a operacje nigdy nie zostaną częściowo zastosowane.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
