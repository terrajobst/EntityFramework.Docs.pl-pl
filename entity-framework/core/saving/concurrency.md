---
title: "Obsługa konfliktom współbieżności - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Obsługa konfliktom współbieżności

> [!NOTE]
> Ta strona dokumentów działania współbieżności EF Core i sposób obsługi konfliktom współbieżności w aplikacji. Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) szczegółowe informacje na temat konfigurowania tokeny współbieżności w modelu.

> [!TIP]
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) w witrynie GitHub.

_Baza danych współbieżności_ odwołuje się do sytuacji, w których wiele procesów lub użytkowników dostęp lub zmienić tych samych danych w bazie danych, w tym samym czasie. _Formant współbieżności_ odwołuje się do określonych mechanizmy służące do zapewnienia spójności danych w obecności równoczesnych zmian.

Implementuje EF Core _optymistyczne sterowanie współbieżnością_, co oznacza, że umożliwi programowi wiele procesów lub użytkownicy wprowadzać zmiany niezależnie bez ponoszenia dodatkowych nakładów synchronizacji lub blokowania. W sytuacji idealnej tych zmian nie kolidują ze sobą i w związku z tym będzie można pomyślnie. W scenariusz dwie lub więcej procesów podejmie próbę zmiany powodujące konflikt, i tylko jeden z nich ma być pomyślnie wykonane.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak działa formant współbieżności w EF Core

Właściwości skonfigurowany jako tokeny współbieżności są używane do implementowania optymistyczne sterowanie współbieżnością: zawsze, gdy jest wykonywana operacja aktualizacji lub usuwania podczas `SaveChanges`, wartość tokenu współbieżności bazy danych zostaną porównane z oryginalną wartość odczytywane przez EF Core.

- Jeśli wartości są zgodne, można wykonać operacji.
- Jeśli wartości nie są zgodne, EF Core założono, że inny użytkownik wykonał operacji powodującej konflikt i przerwanie bieżącej transakcji.

Sytuacji, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją nosi nazwę _konflikt współbieżności_.

Dostawcy bazy danych są zobowiązani do wykonania porównania wartości tokenu współbieżności.

W relacyjnych baz danych EF Core obejmuje sprawdzenia wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcje. Po wykonaniu instrukcji, EF Core odczytuje liczbę wierszy, które zostały zainfekowane.

Jeśli dotyczy żadnych wierszy, został wykryty konflikt współbieżności i zgłasza EF Core `DbUpdateConcurrencyException`.

Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` być tokenem współbieżności. Następnie żadnej operacji update na osoba będzie zawierać wyboru współbieżność w `WHERE` klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Rozwiązywanie konfliktów współbieżności

Kontynuowanie poprzedni przykład, jeśli jeden użytkownik próbuje zapisać pewnych zmian `Person`, ale już inny użytkownik zmienił `LastName` zostanie wygenerowany wyjątek.

W tym momencie aplikacja może po prostu poinformować użytkownika, że aktualizacja nie zakończyła się niepowodzeniem z powodu zmian powodujących konflikt i przenieść na. Jednak może być pożądane, aby monitować użytkownika, upewnij się, że ten rekord nadal reprezentuje sama osoba rzeczywiste i spróbuj ponownie wykonać operację.

Ten proces jest przykładem _Rozwiązywanie konfliktu współbieżności_.

Rozwiązywanie konfliktów współbieżności obejmuje scalanie oczekujących zmian z bieżącego `DbContext` z wartościami w bazie danych. Jakie wartości scalony różni się zależnie od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.

**Istnieją trzy zestawy wartości może pomóc rozwiązać konflikt współbieżności:**

* **Bieżące wartości** wartości próby zapisu w bazie danych aplikacji.

* **Oryginalne wartości** to wartości, które zostały pierwotnie pobrany z bazy danych, aby zmiany zostały wprowadzone.

* **Baza danych wartości** wartości przechowywane w bazie danych.

Ogólne podejście do obsługi konfliktom współbieżności jest:

1. CATCH `DbUpdateConcurrencyException` podczas `SaveChanges`.
2. Użyj `DbUpdateConcurrencyException.Entries` Aby przygotować nowy zestaw zmian do odpowiednich jednostek.
3. Odśwież oryginalnych wartości tokenu współbieżności do bieżącej wartości w bazie danych.
4. Ponów próbę wykonania procesu, dopóki nie występują żadne konflikty.

W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności. Brak `// TODO:` — komentarz w lokalizacji, gdzie obejmują określonego logikę aplikacji, aby wybrać wartość do zapisania.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
