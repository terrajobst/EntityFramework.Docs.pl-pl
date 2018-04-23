---
title: Entity Framework Core plan
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 6c10e64a4fa3bf26dc0da64bb9e102c8b76d3a6e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core plan

> [!IMPORTANT]
> Zauważ, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, mimo że firma Microsoft podejmie próbę aktualności tej strony, może ona nie odzwierciedlać najnowszych plany na wszystkich razy.

Drugi preview EF Core 2.1 został wydany 2018 kwietnia. Teraz można znaleźć więcej informacji na temat tej wersji w [What's new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Firma Microsoft planuje Zwolnij dodatkowe wersji zapoznawczych platformy EF Core co miesiąc 2.1 i ostateczną wersją na drugi kwartał kalendarzowy 2018.

Firma Microsoft nie została ukończona [wersji, planowania](#release-planning-process) w następnej wersji po 2.1.

## <a name="schedule"></a>Harmonogram

Harmonogram EF Core jest zsynchronizowana z [harmonogram .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) i [harmonogram platformy ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Zaległości

Używamy [punkt kontrolny zaległości](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) w naszych śledzącym problem, aby zachować szczegółową listę problemów i funkcje. Klienci mogą komentarzy i głosowanie w górę, te.

Firma Microsoft często pozostaw otwarte, że firma Microsoft spodziewać firma Microsoft będzie działać na w pewnym momencie lub inną od społeczności można rozwiązania, ale nie oznacza celem ich rozwiązania w określonym przedziale czasu do czasu firma Microsoft przypisać je do określonego punktu kontrolnego w ramach naszych problemów [wersji procesu planowania](#release-planning-process).

Jeśli firma Microsoft nie planuje kiedykolwiek wdrożenie funkcją, firma Microsoft zamknie prawdopodobnie problem. Problem, który możemy zamknąć może zostać ponownie rozważone w późniejszym czasie, jeśli uzyskany nowe informacje.

Wszystkie inaczej mówiąc, firma Microsoft nie ma wystarczającej ilości informacji o przyszłej działalności, aby można było wskazywać, że ta funkcja X zostaną rozwiązane przez czas i wersji Y. Tak jak wszystkie projekty oprogramowania priorytetów, harmonogramy wersji i dostępnych zasobów można zmienić w dowolnym momencie.

## <a name="release-planning-process"></a>Proces planowania zlecenia

Uzyskujemy często zadawane pytania dotyczące sposobu wybieramy opcję określonych funkcji, aby przejść do określonej wersji. Nasze zaległości na pewno nie automatycznie przekłada się na planów wprowadzenia produktu. Obecność funkcją EF6 również nie automatycznie oznacza funkcji musi być implementowane w EF Core.

Trudno tutaj szczegółowo cały proces, który możemy wykonać, aby zaplanować zlecenia, częściowo, ponieważ zawiera ona wiele określonych funkcji, możliwości i priorytety i częściowo, ponieważ sam proces zwykle rozwoju w każdej wersji. Istnieje stosunkowo łatwa do podsumowania często zadawane pytania, którą spróbujemy odpowiedzieć przy podejmowaniu decyzji co do pracy w następnej kolejności:

1. **Ile deweloperzy naszym zdaniem użyje funkcji i ile lepiej spowoduje na ich/korzystanie z aplikacji?** Firma Microsoft agregować opinii z wielu źródeł do tego — komentarze i głosów problemów jest jednym z tych źródeł.

2. **Co to są osób obejścia można użyć, jeśli firma Microsoft nie jeszcze zaimplementować tę funkcję?** Na przykład wielu deweloperów mogą mapować tabelę sprzężenia w celu obejścia braku macierzystą obsługę wiele do wielu. Oczywiście nie wszystkich deweloperów można to zrobić, ale można wiele i jest to czynnik, które zlicza.

3. **Wdrożenie tej funkcji rozwijać architektura EF Core tak, aby przenosi nam bliżej do wykonania innych funkcji?** Firma Microsoft często preferować funkcje, które działają jako bloków konstrukcyjnych dla innych funkcji — na przykład dzielenia tabeli, która została wykonana dla typów należących do pomaga nam przejścia do TPT pomocy technicznej.

4. **Funkcja rozszerzalność punktu?** Firma Microsoft zwykle preferować punkty rozszerzeń, ponieważ umożliwiają deweloperom co ułatwia utworzenie punktu zaczepienia w ich własnych zachowania i pobrać niektórych funkcji brakuje w ten sposób. Firma Microsoft planowane jest część jako rozpoczęcia pracy opóźnionego ładowania.

5. **Co to jest współdziałania funkcję w połączeniu z innymi produktami?** Firma Microsoft zwykle preferować funkcje, które umożliwiają Core EF ma być używany z innymi produktami lub w znacznym stopniu poprawić środowisko korzystania innych produktów, takich jak .NET Core najnowszej wersji programu Visual Studio, Microsoft Azure itp.

6. **Jakie są możliwości osób, które są dostępne w funkcji i jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu EF i nawet naszej społeczności mają różne poziomy obsługi w przypadku różnych obszarów i konieczne jest odpowiednio zaplanować. Nawet wtedy, gdy trzeba mieć "wszystkie wskazówki talii" pracy na określonych funkcji, takich jak tłumaczeń GroupBy lub wiele do wielu, byłoby praktyczne.

Jak wspomniano wcześniej, ten proces rozwoju środowisko w każdej wersji, a w przyszłości chcielibyśmy dodać więcej możliwości dla członków społeczności deweloperów Podaj dane wejściowe do planów wprowadzenia produktu, np. przez co ułatwia przeglądanie projektów proponowanych funkcji i z sam plan wersji.
