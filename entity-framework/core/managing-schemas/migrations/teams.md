---
title: Migracje w środowiskach zespołów — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: e6a1b86761a201cbcae34cced7e64f11df37a420
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811981"
---
# <a name="migrations-in-team-environments"></a>Migracje w środowiskach zespołów

Podczas pracy z migracjami w środowiskach zespołu należy zwrócić szczególną uwagę na plik migawek modelu. Ten plik może poinformować użytkownika, jeśli migracja Twojego zespołu nie zostanie prawidłowo scalona z Twoimi lub jeśli trzeba rozwiązać konflikt przez ponowne utworzenie migracji przed udostępnieniem jej.

## <a name="merging"></a>Scalanie

Po scaleniu migracji od członków zespołu mogą wystąpić konflikty w pliku migawek modelu. Jeśli obie zmiany są niepowiązane, scalanie jest proste, a dwie migracje mogą współistnieć. Na przykład w konfiguracji typu jednostki klienta może wystąpić konflikt scalania, który wygląda następująco:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Ponieważ obie te właściwości muszą istnieć w modelu końcowym, Ukończ scalanie przez dodanie obu właściwości. W wielu przypadkach system kontroli wersji może automatycznie scalić takie zmiany.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

W takich przypadkach migracja i migracja Twojego zespołu są niezależne od siebie. Ponieważ jedna z nich może zostać zastosowana jako pierwsza, nie musisz wprowadzać żadnych dodatkowych zmian do migracji przed udostępnieniem jej zespołowi.

## <a name="resolving-conflicts"></a>Rozwiązywanie konfliktów

Czasami występuje konflikt w czasie scalania modelu migawek modelu. Na przykład ty i Twojemu członkowi zespołu można zmienić nazwę tej samej właściwości.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Jeśli wystąpi ten rodzaj konfliktu, rozwiąż go przez ponowne utworzenie migracji. Wykonaj następujące kroki:

1. Przerwij scalanie i wycofywanie do katalogu roboczego przed scaleniem
2. Usuń migrację (ale Zachowaj zmiany modelu)
3. Scalanie zmian Twojego zespołu w katalogu roboczym
4. Ponowne Dodawanie migracji

Po wykonaniu tej czynności te dwie migracje mogą być stosowane w odpowiedniej kolejności. Najpierw zostanie zastosowana migracja, zmiana nazwy kolumny na *alias*, a następnie migracja zmienia nazwę na *nazwa_użytkownika*.

Migracja może być bezpiecznie udostępniona w pozostałej części zespołu.
