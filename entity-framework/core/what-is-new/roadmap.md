---
title: Entity Framework Core plan
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 937bfff4cbc3ec0b2235a78cb2f1f246697128d5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929787"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core plan

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.

## <a name="ef-core-30"></a>EF Core 3.0

Przy użyciu programu EF Core 2.2 spory naszym głównym celem jest teraz EF Core 3.0.
Zobacz [What's new in EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index) uzyskać informacji na temat planowanych [nowe funkcje](xref:core/what-is-new/ef-core-3.0/features) i zamierzone [istotne zmiany w](xref:core/what-is-new/ef-core-3.0/breaking-changes) zawartych w tej wersji.

## <a name="schedule"></a>Harmonogram

Planowanie wydania dla platformy EF Core są zsynchronizowane z [harmonogram wersji platformy .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

## <a name="backlog"></a>Zaległości

[Punkt kontrolny zaległości](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) w naszym problem śledzenia zawiera problemy, które albo oczekujemy, że do pracy w przyszłości lub uważamy, że ktoś od społeczności można analizować.
Klienci mogą przesyłać komentarze i głosów na temat tych problemów — Zapraszamy.
Współautorzy chcą pracować na dowolnym z tych problemów są zachęcani do najpierw Rozpocznij dyskusję na temat sposobu ich podejście.

Nigdy nie ma gwarancji, że będziesz trwają prace nad dowolnym danej funkcji w określonej wersji programu EF Core.
Tak jak wszystkie projekty oprogramowania priorytetów, harmonogramy wersji i dostępnych zasobów można zmienić w dowolnym momencie.
Ale jeśli Chcieliśmy rozwiązać problem w określonym przedziale czasu, firma Microsoft będzie przypisać go do punktu kontrolnego wersji, a nie punktu kontrolnego zaległości.
Firma Microsoft regularnie przenoszenie problemów punkty kontrolne wydania i zaległości w ramach naszych [wersji procesu planowania](#release-planning-process).

Firma Microsoft będzie prawdopodobnie Zamknij problem, jeśli firma Microsoft nie jest planowane kiedykolwiek rozwiązać problem.
Ale możemy ponowne rozpatrzenie problem, który możemy wcześniej zamknięte, jeśli możemy uzyskać nowe informacje o nim.

## <a name="release-planning-process"></a>Proces planowania wydania

Uzyskujemy często zadawane pytania dotyczące sposobu Wybierzmy określonych funkcji, aby przejść do określonej wersji.
Naszych planach na pewno nie przekłada się automatycznie w planach wydania.
Obecność funkcją EF6 również nie automatycznie oznacza, że ta funkcja musi zostać wdrożone w programie EF Core.

Trudno szczegółowo cały proces wykonamy planowanie wydania.
Część informacji jest Omawiając określone funkcje, możliwości i priorytety, a sam proces ewoluuje się również z każdym wydaniem.
Jednak firma Microsoft Podsumowując często zadawanych pytań, którą spróbujemy odpowiedzieć przy podejmowaniu decyzji co do pracy po kliknięciu przycisku Dalej:

1. **Deweloperzy liczbę naszym zdaniem będzie używać tej funkcji i ile lepiej będzie ona aplikacji lub środowiska?** Aby odpowiedzieć na to pytanie, firma Microsoft zbieranie opinii z wielu źródeł, komentarze i głosów problemów jest jednym z tych źródeł.

2. **Co to są osób rozwiązania można użyć, jeśli firma Microsoft nie jeszcze zaimplementować tę funkcję?** Na przykład wielu deweloperów można mapować tabelę sprzężenia w celu obejścia Brak natywnej obsługi wiele do wielu. Oczywiście nie wszystkim deweloperom chcesz to zrobić, ale można wiele i który jest liczona jako czynnika podczas podjęcie decyzji.

3. **Wdrażanie tej funkcji ewolucji architektury programu EF Core tak, aby przemieszczał się nam przybliżyć do wykonania innych funkcji?** Firma Microsoft zwykle preferować funkcje, które działają jako bloków konstrukcyjnych dla innych funkcji. Na przykład właściwość zbiór jednostek, ułatwisz nam idą w kierunku obsługi wiele do wielu i konstruktory jednostki włączone obsłudze powolne ładowanie.

4. **Funkcja punkt rozszerzeń?** Zwykle aby preferował punkty rozszerzeń za pośrednictwem funkcji normalne, ponieważ umożliwiają one programistom podłączyć ich zachowania, a także kompensuje wszelkie brakujące funkcje.

5. **Co to jest współdziałanie funkcji w połączeniu z innymi produktami?** Będziemy preferować funkcje, które włącza lub znacznie poprawić środowisko przy użyciu programu EF Core przy użyciu innych produktów, takich jak .NET Core najnowszej wersji programu Visual Studio, Microsoft Azure i tak dalej.

6. **Co to są umiejętności osób, które są dostępne w funkcji oraz jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu platformy EF i naszej społeczności ma różne poziomy doświadczenia w innych obszarach, więc musimy odpowiednio zaplanować. Nawet wtedy, gdy chcemy mieć "cały zespół na pokładzie" pracy w określonych funkcji, takich jak tłumaczenia GroupBy lub wiele do wielu, byłoby niepraktyczne.

Jak wspomniano wcześniej, ten proces ewoluuje w każdej wersji.
W przyszłości zostanie podjęta próba dodać więcej możliwości dla członków społeczności Podaj dane wejściowe w naszych planach wydania.
Na przykład chcemy ułatwić przeglądanie projektów projektu funkcji i samego planu wersji.
