---
title: Migracja w środowiskach zespołu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054725"
---
<a name="migrations-in-team-environments"></a>Migracja w środowiskach zespołu
===============================
Podczas pracy z migracji w środowiskach zespołu, należy zwrócić uwagę dodatkowe migawki pliku modelu. Ten plik można stwierdzić, jeśli migracja z partnerem scala prawidłowo z Twoimi zmianami z Jeśli chcesz rozwiązać konflikt, ponownie tworząc migracji przed udostępnieniem go.

<a name="merging"></a>Scalanie
-------
Podczas migracji scalania z członków zespołu, mogą wystąpić konflikty w pliku modelu migawki. W przypadku niepowiązanych zmiany scalania jest proste i dwa migracje mogą współistnieć. Na przykład może zostać konfliktu scalania w konfiguracji typu jednostki klienta, który wygląda następująco:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Ponieważ obu tych właściwości muszą istnieć w ostatnim modelu scalania, dodając obu właściwości. W wielu przypadkach system kontroli wersji może automatycznie scalić zmian.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

W takich przypadkach migracji i migracji z partnerem są od siebie niezależne. Ponieważ każdej z nich może być zastosowana jako pierwsza, nie trzeba wprowadzać żadnych zmian dodatkowe migracji przed udostępnieniem go z zespołem.

<a name="resolving-conflicts"></a>Rozwiązywanie konfliktów
-------------------
Czasami wystąpią true konflikt podczas scalania modelu migawki modelu. Na przykład możesz i z partnerem każdego zmieniona tej samej właściwości.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Jeśli wystąpią tego rodzaju konflikt rozwiązanie go ponownie tworząc migracji. Wykonaj następujące kroki:

1. Przerwij scalania i wycofania do katalogu roboczego przed scaleniem
2. Usuń migracji (ale zachować zmiany modelu)
3. Scal zmiany z partnerem w katalogu roboczego
4. Dodaj ponownie migracji

Po wykonaniu tej czynności dwa migracje mogą być stosowane w odpowiedniej kolejności. Ich migracji zostanie zastosowana jako pierwsza, zmiana nazwy kolumny, która ma *Alias*, następnie migracji zmienia jego nazwę, aby *Username*.

Migracji można bezpiecznie udostępniony reszty zespołu.
