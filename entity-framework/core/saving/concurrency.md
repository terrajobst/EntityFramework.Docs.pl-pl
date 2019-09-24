---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 4d6ff24e58caa0b228e9c1e4313beda78d1025fc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197832"
---
# <a name="handling-concurrency-conflicts"></a>Obsługa konfliktów współbieżności

> [!NOTE]
> Ta strona dokumentuje sposób działania współbieżności w EF Core oraz sposób obsługi konfliktów współbieżności w aplikacji. Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) , aby uzyskać szczegółowe informacje na temat konfigurowania tokenów współbieżności w modelu.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) użyty w tym artykule można zobaczyć w witrynie GitHub.

_Współbieżność bazy danych_ odnosi się do sytuacji, w których wiele procesów lub użytkowników uzyskuje dostęp lub zmienia te same dane w bazie danych w tym samym czasie. _Kontrola współbieżności_ odnosi się do określonych mechanizmów używanych do zapewnienia spójności danych w obecności współbieżnych zmian.

EF Core implementuje _optymistyczną kontrolę współbieżności_, co oznacza, że umożliwia wiele procesów lub użytkownikom wprowadzanie zmian niezależnie od obciążenia synchronizacji lub blokowania. W przypadku idealnej sytuacji te zmiany nie będą zakłócać działania i w związku z tym będą mogły się powieść. W scenariuszu najgorszego przypadku co najmniej dwa procesy będą próbować wprowadzać zmiany w konflikcie, a tylko jeden z nich powinien pomyślnie.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak działa kontrola współbieżności w EF Core

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistycznej kontroli współbieżności: za każdym razem, gdy `SaveChanges`operacja aktualizowania lub usuwania jest wykonywana w trakcie, wartość tokenu współbieżności w bazie danych jest porównywana z oryginalną wartość odczytana przez EF Core.

- Jeśli wartości są takie same, operacja może zostać ukończona.
- Jeśli wartości nie są zgodne, EF Core zakłada, że inny użytkownik wykonał operację powodującą konflikt i przerywa bieżącą transakcję.

Sytuacja, w której inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją, jest znana jako _konflikt współbieżności_.

Dostawcy baz danych są odpowiedzialni za implementację porównania wartości tokenów współbieżności.

W relacyjnych bazach danych EF Core obejmuje sprawdzenie wartości tokenu współbieżności w `WHERE` klauzuli `UPDATE` instrukcji or `DELETE` . Po wykonaniu instrukcji EF Core odczytuje liczbę wierszy, których to dotyczy.

Jeśli nie wpłynie to na żadne wiersze, zostanie wykryte konflikt współbieżności, a `DbUpdateConcurrencyException`EF Core zgłasza.

Na przykład możemy chcieć skonfigurować `LastName` `Person` jako token współbieżności. Następnie każda operacja aktualizacji dla osoby będzie obejmować kontrolę współbieżności w `WHERE` klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Rozwiązywanie konfliktów współbieżności

Kontynuując poprzedni przykład, jeśli jeden użytkownik próbuje zapisać pewne zmiany w `Person`, ale inny użytkownik już `LastName`zmienił, zostanie zgłoszony wyjątek.

W tym momencie aplikacja może po prostu poinformować użytkownika o tym, że aktualizacja nie powiodła się z powodu sprzecznych zmian i przejścia. Może jednak być wskazane monitowanie użytkownika o zapewnienie, że ten rekord nadal reprezentuje tę samą osobę rzeczywistą i spróbuje ponownie wykonać operację.

Ten proces jest przykładem _rozwiązania konfliktu współbieżności_.

Rozwiązanie konfliktu współbieżności polega na scaleniu oczekujących zmian z bieżącego `DbContext` z wartościami w bazie danych. Jakie wartości są scalane, różnią się w zależności od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.

**Dostępne są trzy zestawy wartości, które ułatwiają rozwiązanie konfliktu współbieżności:**

* **Bieżące wartości** to wartości, które aplikacja podjęła próbę zapisu w bazie danych.

* **Pierwotne wartości** to wartości, które zostały pierwotnie pobrane z bazy danych, przed wprowadzeniem jakichkolwiek zmian.

* **Wartości bazy danych** są obecnie przechowywane w bazie danych.

Ogólnym podejściem do obsłużenia konfliktów współbieżności jest:

1. Catch `DbUpdateConcurrencyException` w `SaveChanges`trakcie.
2. Służy `DbUpdateConcurrencyException.Entries` do przygotowywania nowego zestawu zmian dla odnośnych jednostek.
3. Odśwież oryginalne wartości tokenu współbieżności, aby odzwierciedlić bieżące wartości w bazie danych.
4. Ponów próbę wykonania procesu, dopóki nie wystąpią żadne konflikty.

W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności. `// TODO:` Istnieje komentarz w lokalizacji, w której dołączysz logikę specyficzną dla aplikacji w celu wybrania wartości do zapisania.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
