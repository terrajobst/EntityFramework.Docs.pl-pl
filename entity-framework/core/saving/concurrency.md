---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417591"
---
# <a name="handling-concurrency-conflicts"></a>Obsługa konfliktów współbieżności

> [!NOTE]
> Ta strona dokumentuje, jak współbieżność działa w EF Core i jak obsługiwać konflikty współbieżności w aplikacji. Zobacz [tokeny współbieżności,](xref:core/modeling/concurrency) aby uzyskać szczegółowe informacje na temat konfigurowania tokenów współbieżności w modelu.

> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) przykład artykułu na GitHub.

_Współbieżność bazy danych_ odnosi się do sytuacji, w których wiele procesów lub użytkowników dostęp lub zmienić te same dane w bazie danych w tym samym czasie. _Kontrola współbieżności_ odnosi się do określonych mechanizmów używanych w celu zapewnienia spójności danych w obecności równoczesnych zmian.

EF Core implementuje _kontrolę współbieżności optymistyczne,_ co oznacza, że pozwoli wielu procesów lub użytkowników wprowadzać zmiany niezależnie bez narzutu synchronizacji lub blokowania. W idealnej sytuacji zmiany te nie będą kolidować ze sobą i dlatego będą w stanie odnieść sukces. W najgorszym przypadku dwa lub więcej procesów będzie próbował wprowadzić zmiany powodujące konflikty i tylko jeden z nich powinien zakończyć się pomyślnie.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak działa kontrola współbieżności w EF Core

Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania kontroli współbieżności optymistyczne: `SaveChanges`za każdym razem, gdy operacja aktualizacji lub usuwania jest wykonywana podczas , wartość tokenu współbieżności w bazie danych jest porównywana z oryginalną wartością odczytywaną przez EF Core.

- Jeśli wartości są zgodne, operacja może zakończyć.
- Jeśli wartości nie są zgodne, EF Core zakłada, że inny użytkownik wykonał operację powodującą konflikt i przerywa bieżącą transakcję.

Sytuacja, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją jest znany jako _konflikt współbieżności_.

Dostawcy bazy danych są odpowiedzialni za implementację porównania wartości tokenów współbieżności.

W relacyjnych bazach danych EF Core zawiera sprawdzenie wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcji. Po wykonaniu instrukcji EF Core odczytuje liczbę wierszy, których dotyczy problem.

Jeśli nie dotyczy to żadnych wierszy, zostanie wykryty konflikt współbieżności, a ef core zostanie rzucony. `DbUpdateConcurrencyException`

Na przykład możemy chcieć `LastName` skonfigurować jako `Person` token współbieżności. Następnie każda operacja aktualizacji na osobę będzie zawierać `WHERE` sprawdzenie współbieżności w klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Rozwiązywanie konfliktów współbieżności

Kontynuując poprzedni przykład, jeśli jeden użytkownik próbuje zapisać `Person`niektóre zmiany w , `LastName`ale inny użytkownik już zmienił , a następnie zostanie zgłoszony wyjątek.

W tym momencie aplikacja może po prostu poinformować użytkownika, że aktualizacja nie powiodła się z powodu sprzecznych zmian i przejść dalej. Ale może być pożądane, aby poprosić użytkownika, aby upewnić się, że ten rekord nadal reprezentuje tę samą rzeczywistą osobę i ponowić próbę wykonania operacji.

Ten proces jest przykładem _rozwiązania konfliktu współbieżności_.

Rozwiązywanie konfliktu współbieżności polega na scalaniu oczekujących `DbContext` zmian z bieżącego z wartościami w bazie danych. Jakie wartości zostaną scalone różnią się w zależności od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.

**Dostępne są trzy zestawy wartości ułatwiające rozwiązanie konfliktu współbieżności:**

- **Bieżące wartości** są wartościami, które aplikacja próbowała zapisać w bazie danych.
- **Wartości oryginalne** są wartościami, które zostały pierwotnie pobrane z bazy danych, przed dokonaniem jakichkolwiek zmian.
- **Wartości bazy danych** są wartościami aktualnie przechowywanymi w bazie danych.

Ogólne podejście do obsługi konfliktów współbieżności jest:

1. Złapać `DbUpdateConcurrencyException` `SaveChanges`podczas .
2. Służy `DbUpdateConcurrencyException.Entries` do przygotowania nowego zestawu zmian dla jednostek, których dotyczy problem.
3. Odśwież oryginalne wartości tokenu współbieżności, aby odzwierciedlić bieżące wartości w bazie danych.
4. Ponów próbę wykonania procesu, dopóki nie wystąpią żadne konflikty.

W poniższym `Person.FirstName` przykładzie i `Person.LastName` są skonfigurowane jako tokeny współbieżności. Istnieje `// TODO:` komentarz w lokalizacji, w której można dołączyć logikę specyficzne dla aplikacji, aby wybrać wartość do zapisania.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
