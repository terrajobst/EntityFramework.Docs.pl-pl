---
title: Co nowego — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2ca4915fca515b4bdbfeb77bc7b02f15ce1704b6
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197726"
---
# <a name="whats-new-in-ef-core"></a>Co nowego w EF Core

## <a name="recent-releases"></a>Najnowsze wersje

- **EF Core 3,0** (Najnowsza wersja stabilna) 
  - [Nowe funkcje](xref:core/what-is-new/ef-core-3.0/index) 
  - Istotne [zmiany](xref:core/what-is-new/ef-core-3.0/breaking-changes) , o których należy wiedzieć podczas uaktualniania
- [EF Core 2.2](xref:core/what-is-new/ef-core-2.2)
- [EF Core 2,1](xref:core/what-is-new/ef-core-2.1) (Najnowsza wersja długoterminowej pomocy technicznej)

## <a name="product-roadmap"></a>Plan produktu

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, a mimo to, że firma Microsoft podejmie próbę zapewnienia aktualności tej strony, może nie odzwierciedlać naszych najnowszych planów przez cały czas.

### <a name="future-releases"></a>Przyszłe wersje

- **EF Core 3,1**  
  - W obszarze aktywne programowanie
  - Będzie zawierać [niewielkie ulepszenia wydajności, jakości i stabilności](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.1.0+sort%3Areactions-desc)
  - Planowane jako wersja LTS (Long-Term Support)
  - Obecnie zaplanowano 2019 grudnia
- **EF Core "vNext"**   
  - Następna wersja główna EF Core do wysłania wraz z platformą .NET 5
  - Planowanie tej wersji nie zostało jeszcze rozpoczęte i nie ogłoszono żadnych funkcji.  

### <a name="schedule"></a>Harmonogram

Harmonogram wydania dla EF Core jest zsynchronizowany z [harmonogramem wersji programu .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="backlog"></a>Zaległości

[Punkt kontrolny zaległości](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) w naszym monitorze problemów zawiera problemy, które należy wykonać w witrynie Someday, lub Pomyśl, że ktoś ze społeczności może się zaradzić.
Klienci mogą przesłać Komentarze i głosy na te problemy.
Współautorzy, którzy chcą korzystać z dowolnego z tych problemów, zachęcamy do pierwszego rozpoczęcia dyskusji na temat sposobu podejścia do nich.

Nigdy nie gwarantujemy, że będziemy współdziałać nad daną funkcją w określonej wersji EF Core.
Podobnie jak w przypadku wszystkich projektów oprogramowania, priorytetów, harmonogramów wydań i dostępnych zasobów można zmienić w dowolnym momencie.
Jeśli jednak zamierzasz rozwiązać problem w określonym przedziale czasu, przypiszemy go do punktu kontrolnego zlecenia, a nie do punktu kontrolnego zaległości.
Rutynowe przenoszenie problemów między zaległościami i wersjami punktów kontrolnych jest częścią naszego [procesu planowania wydań](#release-planning-process).

Prawdopodobnie zamkniemy problem, jeśli nie planujemy go kiedykolwiek zająć.
Jednak możemy ponownie rozważyć problem, który został wcześniej zamknięty, jeśli otrzymamy nowe informacje na jego temat.

### <a name="release-planning-process"></a>Proces planowania zlecenia

Często otrzymujemy pytania dotyczące sposobu wybierania określonych funkcji, aby przejść do określonej wersji.
Nasze zaległości nie są automatycznie przekształcane na plany wydań.
Obecność funkcji w programie EF6 również nie oznacza, że funkcja musi zostać wdrożona w EF Core.

Trudno jest szczegółować cały proces, który obserwujemy, aby zaplanować wydanie.
Wiele z nich omawia konkretne funkcje, szanse sprzedaży i priorytety, a proces zmienia się również z każdą wersją.
Można jednak podsumowywać typowe pytania, z którymi staramy się odpowiedzieć przy podejmowaniu decyzji o tym, co należy zrobić dalej:

1. **Ilu deweloperów będziemy używać tej funkcji i ile lepiej przeprowadzi swoje aplikacje lub środowisko?** Aby odpowiedzieć na to pytanie, zbieramy Opinie z wielu źródeł — Komentarze i głosy dotyczące problemów są jednym z tych źródeł.

2. **Jakie obejścia mogą być używane w przypadku, gdy ta funkcja nie została jeszcze wdrożona?** Na przykład wielu deweloperów może zamapować tabelę sprzężenia, aby obejść brak natywnej obsługi wiele-do-wielu. Oczywiście nie wszyscy deweloperzy chcą tego robić, ale wiele z nich może, a także liczyć jako czynnik w naszej decyzji.

3. **Czy zaimplementowanie tej funkcji spowoduje rozwój architektury EF Core w taki sposób, aby przeniesieł nam bliższe wdrożenie innych funkcji?** Chcemy preferować funkcje, które pełnią rolę bloków konstrukcyjnych dla innych funkcji. Na przykład jednostki zbioru właściwości mogą pomóc nam w przeniesieniu do pomocy technicznej wiele-do-wielu, a konstruktory jednostek włączyli obsługę ładowania z opóźnieniem.

4. **Czy funkcja jest punktem rozszerzalności?** Chcemy preferować punkty rozszerzalności w porównaniu z normalnymi funkcjami, ponieważ umożliwiają deweloperom Podłączanie własnych zachowań i kompensowanie wszelkich brakujących funkcji.

5. **Jaka jest synergia funkcji, gdy jest używana w połączeniu z innymi produktami?** Firma Microsoft oferuje funkcje, które umożliwiają lub znacząco ulepszają środowisko korzystania z EF Core z innymi produktami, takimi jak .NET Core, Najnowsza wersja programu Visual Studio, Microsoft Azure i tak dalej.

6. **Jakie umiejętności mają pracownicy, którzy pracują nad funkcją i jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu EF i współautorzy społeczności mają różne poziomy doświadczenia w różnych obszarach, dlatego musimy odpowiednio zaplanować. Nawet jeśli chcemy mieć "wszystkie ręce na pokładzie", aby działać na określonej funkcji, takiej jak tłumaczenia GroupBy lub wiele-do-wielu, które nie były praktyczne.

Jak wspomniano wcześniej, proces zmienia się w każdej wersji.
W przyszłości będziemy próbować dodać więcej szans dla członków społeczności, aby zapewnić dane wejściowe w naszych planach wydania.
Na przykład chcielibyśmy ułatwić zapoznanie się z roboczymi projektami funkcji i samego planu wydania.