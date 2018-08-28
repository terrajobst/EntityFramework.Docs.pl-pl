---
title: Migracja w środowiskach zespołu — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997698"
---
<a name="migrations-in-team-environments"></a>Migracja w środowiskach zespołu
===============================
Podczas pracy z migracji w środowiskach zespołu, należy zwracać szczególną uwagę na migawki pliku modelu. Ten plik może określić, czy migracja z partnerem nie pozostawia żadnych śladów scala z Twoimi czy trzeba rozwiązać konflikt, ponownie tworząc migracji przed ich udostępnieniem.

<a name="merging"></a>Scalanie
-------
Podczas migracji scalania z członkami zespołu, może wystąpić konflikty w modelu migawki pliku. W przypadku niepowiązanych obie zmiany scalanie jest proste i dwie migracje mogą współistnieć. Na przykład może wystąpić konflikt scalania w konfiguracji typu jednostki Klient, który wygląda tak:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Ponieważ obu tych właściwości muszą istnieć w ostatnim modelu Ukończ scalanie, dodając obie te właściwości. W wielu przypadkach system kontroli wersji może automatycznie scalić takich zmian.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

W takich przypadkach migracji i migrację z partnerem są niezależne od siebie nawzajem. Ponieważ którąś z tych funkcji można najpierw zastosować, nie potrzebujesz dodatkowych zmian przed ich udostępnieniem ze swoim zespołem migracji.

<a name="resolving-conflicts"></a>Rozwiązywanie konfliktów
-------------------
Czasami wystąpią true konflikt podczas scalania model modelu migawki. Na przykład możesz i z partnerem każdego zmieniona tej samej właściwości.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Jeśli wystąpi tego rodzaju konflikt rozwiązać ten problem, ponownie utworzyć plan migracji. Wykonaj następujące kroki:

1. Przerwij, scalania i wycofania do katalogu roboczego przed scaleniem
2. Usuń plan migracji (ale zachować zmiany modelu)
3. Scal zmiany z partnerem w katalogu roboczym
4. Ponowne dodanie migracji

Po wykonaniu tego, dwie migracje mogą być stosowane w odpowiedniej kolejności. Ich migracji zostanie zastosowana jako pierwsza, zmiana nazwy kolumny, która ma *Alias*, po tej dacie migracji zmienia jego nazwę, aby *Username*.

Migracja może być bezpiecznie udostępniane reszta zespołu.
