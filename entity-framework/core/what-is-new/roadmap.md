---
title: Entity Framework Core plan
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: fd9086c9911cdb0890117d44c2787780aad9a7cb
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821364"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core plan

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.

## <a name="last-release-ef-core-21"></a>Ostatnie wydania: programu EF Core 2.1

Stabilna wersja programu EF Core 2.1 został wydany 30 maja 2018 r. Można znaleźć więcej informacji na temat tej wersji w [What's new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

## <a name="future-releases"></a>Przyszłe wersje

### <a name="ef-core-22"></a>EF Core 2.2

To wydanie będzie zawierać wiele poprawek błędów i stosunkowo niewielką liczbą nowych funkcji. Szczegółowe informacje na temat tej wersji znajdują się w [ogłoszenie harmonogram działania dla platformy EF Core 2.2](https://github.com/aspnet/Announcements/issues/308). 

### <a name="ef-core-30"></a>EF Core 3.0

Chociaż nie możemy ukończyć [wersji procesu planowania](#release-planning-process) na kolejne wydanie po 2.2 aktualnie planujemy zapewnienie głównej wersji algined przy użyciu platformy .NET Core 3.0 i ASP.NET 3.0. 

Możesz użyć [tego zapytania w naszym narzędzie do śledzenia problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) aby zobaczyć elementy robocze tenatively przypisane do tego przyszłej wersji.

## <a name="schedule"></a>Harmonogram

Harmonogram dla platformy EF Core jest zsynchronizowany z [harmonogram platformy .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) i [harmonogram platformy ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Zaległości

Używamy [punkt kontrolny zaległości](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) w nasze narzędzia do śledzenia błędów do utrzymania szczegółowa lista funkcji i problemów. Klienci mogą komentarz i głosowanie w górę, te.

Firma Microsoft często pozostaw otwarte, że firma Microsoft spodziewać firma Microsoft będzie działać na w pewnym momencie lub inną od społeczności można rozwiązania, ale nie oznacza celem ich rozwiązania w określonym przedziale czasu do czasu firma Microsoft przypisać je do określonego punktu kontrolnego w ramach naszych problemów [wersji procesu planowania](#release-planning-process).

Jeśli firma nie chce kiedykolwiek wdrożyć funkcję, firma Microsoft prawdopodobnie zostanie zamknięty problem. Problem, który możemy zamknięte można sposób w dowolnym momencie, jeśli możemy uzyskać nowe informacje o nim.

Wszystkie inaczej mówiąc, nie mamy wystarczającej ilości informacji o przyszłość, aby można było Załóżmy, że tej funkcji X zostaną rozwiązane przez czas/release Y. Tak jak wszystkie projekty oprogramowania priorytetów, harmonogramy wersji i dostępnych zasobów można zmienić w dowolnym momencie.

## <a name="release-planning-process"></a>Proces planowania wydania

Uzyskujemy często zadawane pytania dotyczące sposobu Wybierzmy określonych funkcji, aby przejść do określonej wersji. Naszych planach na pewno nie przekłada się automatycznie w planach wydania. Obecność funkcją EF6 również nie automatycznie oznacza, że ta funkcja musi zostać wdrożone w programie EF Core.

Trudno poniżej szczegółowo cały proces, który możemy wykonać, aby zaplanować wydanie, częściowo, ponieważ zawiera mnóstwo określone funkcje, możliwości i priorytety i częściowo, ponieważ sam proces jest zwykle ewoluuje z każdym wydaniem. Jednak jest stosunkowo łatwa do podsumowania często zadawanych pytań, którą spróbujemy odpowiedzieć przy podejmowaniu decyzji co do pracy po kliknięciu przycisku Dalej:

1. **Deweloperzy liczbę naszym zdaniem będzie używać tej funkcji i jak dużo lepiej wprowadzi na ich/korzystanie z aplikacji?** Firma Microsoft agregacji opinii z wielu źródeł do tego — komentarze i głosów problemów jest jednym z tych źródeł.

2. **Co to są osób rozwiązania można użyć, jeśli firma Microsoft nie jeszcze zaimplementować tę funkcję?** Na przykład wielu programistów mogą Mapuj tabelę sprzężenia w celu obejścia Brak natywnej obsługi wiele do wielu. Oczywiście deweloperzy nie wszystkie można to zrobić, można wiele — a jest to czynnik, który zlicza.

3. **Wdrażanie tej funkcji ewolucji architektury programu EF Core tak, aby przemieszczał się nam przybliżyć do wykonania innych funkcji?** Dążymy do Preferuj funkcje, które działają jako bloków konstrukcyjnych dla innych funkcji — na przykład, dzielenie tabeli, która została wykonana dla typów będących własnością pomaga nam idą w kierunku TPT pomocy technicznej.

4. **Funkcja punkt rozszerzeń?** Zwykle aby preferował punkty rozszerzeń, ponieważ umożliwiają one programistom łatwiej utworzenie punktu zaczepienia w ich własnych zachowania i otrzymujesz niektóre z brakującej funkcjonalności w ten sposób. Planujemy wykonanie niektórych z jako rozpoczęcia pracy z opóźnieniem ładowania.

5. **Co to jest współdziałanie funkcji w połączeniu z innymi produktami?** Firma Microsoft zwykle preferować funkcje, które umożliwiają programu EF Core można używać z innymi produktami lub znacznie poprawić środowisko korzystania innych produktów, takich jak .NET Core najnowszą wersję programu Visual Studio, Microsoft Azure, itp.

6. **Jakie są możliwości osób, które są dostępne w funkcji oraz jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu platformy EF i nawet naszej społeczności mają różne poziomy doświadczenie w różnych obszarach i mamy odpowiednio zaplanować. Nawet wtedy, gdy chcemy mieć "cały zespół na pokładzie" pracy w określonych funkcji, takich jak tłumaczenia GroupBy lub wiele do wielu, byłoby niepraktyczne.

Jak wspomniano wcześniej, ten proces ewoluuje w każdej wersji, a w przyszłości prosimy o poświęcenie można dodać więcej możliwości dla członków społeczności deweloperów ułatwia udostępnianie danych wejściowych w planach wydania, na przykład, ułatwiając przejrzeć proponowane wersje robocze funkcji wersji plan i sam.
