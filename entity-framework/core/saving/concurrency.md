---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993115"
---
# <a name="handling-concurrency-conflicts"></a>Obsługa konfliktów współbieżności

> [!NOTE]
> Ta strona dokumentów, jak współbieżność działa w programie EF Core i sposób obsługi konfliktów współbieżności w aplikacji. Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) Aby uzyskać szczegółowe informacje na temat konfigurowania tokeny współbieżności w modelu.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) użyty w tym artykule można zobaczyć w witrynie GitHub.

_Baza danych współbieżności_ odnosi się do sytuacji, w których wiele procesów lub użytkownikom dostęp lub zmienić te same dane w bazie danych, w tym samym czasie. _Kontrola współbieżności_ odwołuje się do określonych mechanizmów używane w celu zapewnienia spójności danych w obecności równoczesnych zmian.

Implementuje programu EF Core _mechanizmu kontroli optymistycznej współbieżności_, co oznacza, że umożliwi ona wiele procesów lub użytkownicy wprowadzać zmiany, niezależnie od siebie bez konieczności synchronizacji lub blokowania. W sytuacji idealnej te zmiany nie kolidują ze sobą i w związku z tym będzie można pomyślnie. W najgorszym przypadku wielkości liter dwa lub więcej procesów podejmie próbę zmiany powodujące konflikt, i tylko jeden z nich ma być pomyślnie wykonane.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak działa Kontrola współbieżności w programie EF Core

Właściwości skonfigurowane tokeny współbieżności są używane do wdrożenia mechanizmu kontroli optymistycznej współbieżności: zawsze, gdy operacji update lub delete jest wykonywane podczas `SaveChanges`, wartość tokenu współbieżności w bazie danych jest porównywana oryginał wartość odczytu przez platformę EF Core.

- Jeśli wartości są zgodne, można wykonać operacji.
- Jeśli wartości nie są zgodne, EF Core założono, że inny użytkownik wykonał operacji powodującej konflikt i przerywa bieżącej transakcji.

Sytuacja, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącej operacji jest znany jako _konfliktów współbieżności_.

Dostawcy baz danych są odpowiedzialni za wdrażanie porównanie wartości tokenu współbieżności.

W relacyjnych baz danych programu EF Core obejmuje sprawdzenia wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcji. Po wykonaniu instrukcji, programem EF Core odczytuje liczbę wierszy, które miały wpływ.

Jeśli wpływają żadne wiersze nie został wykryty konflikt współbieżności i zgłasza wyjątek programu EF Core `DbUpdateConcurrencyException`.

Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` za tokenem współbieżności. Następnie żadnych operacji aktualizacji w osoba będzie zawierać wyboru współbieżność w `WHERE` klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Rozwiązywanie konfliktów współbieżności

Kontynuując poprzedni przykład gdy jeden użytkownik próbuje zapisać pewne zmiany `Person`, ale już inny użytkownik zmienił `LastName`, a następnie zostanie zgłoszony wyjątek.

W tym momencie aplikacja może po prostu informować użytkownika, że aktualizacja zakończyła się niepowodzeniem z powodu zmian powodujących konflikt i przejść. Jednak może być pożądane, aby monitować użytkownika, upewnij się, że ten rekord reprezentuje nadal tę samą osobę rzeczywiste i spróbuj ponownie wykonać operację.

Ten proces jest przykładem _Rozwiązywanie konfliktów współbieżności_.

Rozwiązywanie konfliktów współbieżności obejmuje scalanie oczekujące zmiany z bieżącej `DbContext` z wartościami w bazie danych. Scalony jakie wartości będą się różnić w zależności od aplikacji i zaleceniami dane wejściowe użytkownika.

**Istnieją trzy rodzaje wartości może pomóc rozwiązać konflikt współbieżności:**

* **Bieżące wartości** są wartościami, które aplikacja próby zapisu w bazie danych.

* **Oryginalne wartości** są wartościami, które zostały pierwotnie pobrany z bazy danych, zanim zmiany zostały wprowadzone.

* **Bazy danych wartości** wartości przechowywane w bazie danych.

Obsługa konfliktów współbieżności ogólne podejście jest:

1. CATCH `DbUpdateConcurrencyException` podczas `SaveChanges`.
2. Użyj `DbUpdateConcurrencyException.Entries` Aby przygotować nowy zestaw zmian dla obiektów, których to dotyczy.
3. Odśwież oryginalne wartości parametru tokenu współbieżności do bieżącej wartości w bazie danych.
4. Ponów próbę wykonania procesu, dopóki nie występują żadne konflikty.

W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności. Brak `// TODO:` komentarz w lokalizacji, gdzie obejmują określonej logiki aplikacji, aby wybrać wartość do zapisania.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
