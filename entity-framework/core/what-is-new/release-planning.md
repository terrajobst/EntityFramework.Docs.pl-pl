---
title: Planowanie wydania EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417337"
---
# <a name="release-planning-process"></a>Proces planowania wersji

Często otrzymujemy pytania dotyczące sposobu, w jaki wybieramy konkretne funkcje, aby przejść do konkretnej wersji.
Ten dokument przedstawia proces, którego używamy.
Proces ten stale ewoluuje, gdy znajdujemy lepsze sposoby planowania, ale ogólne pomysły pozostają takie same.

## <a name="different-kinds-of-releases"></a>Różne rodzaje wydań

Różne rodzaje wydań zawierają różne rodzaje zmian.
To z kolei oznacza, że planowanie wersji jest inny dla różnych rodzajów wydania.

### <a name="patch-releases"></a>Wydania poprawek

Patch zwalnia zmienić tylko "patch" część wersji.
Na przykład EF Core 3.1. **1** to wersja, która wprowadza poprawki problemów znalezionych w EF Core 3.1. **0**.

Wydania poprawek mają na celu naprawienie krytycznych błędów klientów.
Oznacza to, że wersje poprawek nie zawierają nowych funkcji.
Zmiany interfejsu API nie są dozwolone w wydaniach poprawek, z wyjątkiem szczególnych okoliczności.

Pasek zmiany w wydaniu poprawki jest bardzo wysoki.
To dlatego, że ważne jest, aby wydania poprawek nie wprowadzały nowych błędów.
Dlatego proces decyzyjny kładzie nacisk na wysoką wartość i niskie ryzyko.

Jesteśmy bardziej skłonni do poprawki problem, jeśli:
  * Ma to wpływ na wielu klientów
  * Jest to regresja z poprzedniego wydania
  * Awaria powoduje uszkodzenie danych

Jesteśmy mniej skłonni do poprawki problemu, jeśli:
  * Istnieją rozsądne obejścia
  * Poprawka ma wysokie ryzyko złamania czegoś innego
  * Błąd znajduje się w narożnej obudowie

Ta poprzeczka stopniowo wzrasta przez cały okres istnienia [wydania wsparcia długoterminowego (LTS).](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) Dzieje się tak dlatego, że wydania LTS podkreślają stabilność.

Ostateczna decyzja o tym, czy załatać problem, jest podejmowana przez dyrektorów platformy .NET w firmie Microsoft.

### <a name="minor-releases"></a>Drobne wydania

Wersje pomocnicze zmieniają tylko "pomocniczą" część wersji.
Na przykład EF Core 3. **1**.0 to wersja, która poprawia ef core 3. **0,0.**

Drobne wydania:
* Mają na celu poprawę jakości i cech poprzedniej wersji
* Zazwyczaj zawierają poprawki błędów i nowe funkcje
* Nie należy uwzględniać zamierzonych zmian w łamaniu
* Mieć kilka podglądów wersji wstępnej wypchnięty do NuGet

### <a name="major-releases"></a>Główne wydania

Główne wydania zmieniają numer wersji "major" EF.
Na przykład EF Core **3**.0.0 jest główną wersją, która sprawia, że duży krok naprzód za ef core 2.2.x.

Główne wydania:
* Mają na celu poprawę jakości i cech poprzedniej wersji
* Zazwyczaj zawierają poprawki błędów i nowe funkcje
  * Niektóre z nowych funkcji mogą być fundamentalnymi zmianami w sposobie działania EF Core
* Zazwyczaj obejmują zamierzone zmiany włamywania
  * Przełomowe zmiany są niezbędną częścią ewoluującego EF Core, gdy uczymy się
  * Jednak bardzo dokładnie myślimy o dokonywaniu jakichkolwiek przełomowych zmian ze względu na potencjalny wpływ na klienta. Być może byliśmy zbyt agresywni w obliczu przełomowych zmian w przeszłości. W przyszłości będziemy dążyć do zminimalizowania zmian, które przerywają aplikacje i zmniejszenia zmian, które przerywają dostawców baz danych i rozszerzenia.
* Kilka wersji zapoznawców w wersji wstępnej jest wypchniętych do programu NuGet

## <a name="planning-for-majorminor-releases"></a>Planowanie dużych/mniejszych wydań

### <a name="github-issue-tracking"></a>Śledzenie problemów z githubem

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) jest źródłem prawdy dla wszystkich ef core planowania.

Problemy z GitHub mają:

* Stan
  * [Otwarte](https://github.com/dotnet/efcore/issues) problemy nie zostały rozwiązane.
  * [Rozwiązano](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) zamknięte problemy.
    * Wszystkie problemy, które zostały [naprawione,](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed)są oznaczone zamkniętym stałym . Problem oznaczony tagiem zamkniętej stałej został rozwiązany i scalony, ale może nie został wydany.
    * Inne `closed-` etykiety wskazują inne przyczyny zamknięcia problemu. Na przykład duplikaty są oznaczone zduplikowanymi.
* Typ
  * [Błędy](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) reprezentują błędy.
  * [Ulepszenia](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) reprezentują nowe funkcje lub lepsze funkcje w istniejących funkcjach.
* Kamień milowy
  * Zespół rozważa [problemy bez kamienia milowego.](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) Decyzja o tym, co zrobić z tą kwestią, nie została jeszcze podjęta lub rozważana jest zmiana decyzji.
  * [Problemy w punktach kontrolnych zaległości](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) są elementami, które zespół EF rozważy pracę w przyszłej wersji
    * Problemy w zaległości mogą być [oznaczone consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) wskazując, że ten element pracy jest wysoko na liście dla następnej wersji.
  * Otwarte problemy w wersji punktowej są elementy, które zespół planuje pracować w tej wersji. Na przykład [są to kwestie, nad którymi planujemy pracować dla EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Zamknięte problemy w wersji punktowej są problemy, które zostały zakończone dla tej wersji. Należy zauważyć, że wersja może nie została jeszcze wydana. Na przykład [są to problemy zakończone dla EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Głosów!
  * Głosowanie jest najlepszym sposobem, aby wskazać, że problem jest dla Ciebie ważny.
  * Aby zagłosować, wystarczy dodać "kciuk 👍 w górę" do kwestii. Na przykład [są to najważniejsze kwestie](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Proszę również komentarz opisujący konkretne powody, które są potrzebne do tej funkcji, jeśli uważasz, że to wartość dodana. Komentowanie "+1" lub podobne nie zwiększa wartości.

### <a name="the-planning-process"></a>Proces planowania

Proces planowania jest bardziej zaangażowany niż tylko przy najbardziej żądanych funkcji z zaległości.
Dzieje się tak dlatego, że zbieramy informacje zwrotne od wielu zainteresowanych stron na wiele sposobów.
Następnie kształtujemy zwolnienie na podstawie:

* Dane wejściowe od klientów
* Wkład innych zainteresowanych stron
* Kierunek strategiczny
* Dostępne zasoby
* Harmonogram

Oto niektóre z zadumy pytań:

1. **Ilu programistów uważamy, że będzie korzystać z tej funkcji i o ile lepiej będzie ich aplikacji lub doświadczenia?** Aby odpowiedzieć na to pytanie, zbieramy opinie z wielu źródeł — komentarze i głosy na tematy są jednym z tych źródeł. Szczególne kontakty z ważnymi klientami to kolejna.

2. **Jakie rozwiązania mogą korzystać użytkownicy, jeśli nie zaimplementujemy jeszcze tej funkcji?** Na przykład wielu deweloperów można mapować tabeli sprzężenia do pracy wokół braku macierzystej obsługi wielu do wielu. Oczywiście nie wszyscy deweloperzy chcą to zrobić, ale wielu może, a to liczy się jako czynnik w naszej decyzji.

3. **Czy wdrożenie tej funkcji ewoluuje architekturę EF Core w taki sposób, że przybliża nas do wdrażania innych funkcji?** Mamy tendencję do faworyzowania funkcji, które działają jako elementy składowe dla innych funkcji. Na przykład jednostki worka właściwości może pomóc nam przejść do wielu do wielu obsługi i konstruktorów jednostek włączone nasze wsparcie ładowania leniwy.

4. **Czy obiekt jest punktem rozszerzalności?** Mamy tendencję do faworyzowania punktów rozszerzalności nad normalnymi funkcjami, ponieważ umożliwiają one deweloperom hakowanie własnych zachowań i kompensowanie brakujących funkcji.

5. **Jaka jest synergia tej funkcji w połączeniu z innymi produktami?** Preferujemy funkcje, które umożliwiają lub znacznie poprawiają środowisko korzystania z ef core z innymi produktami, takimi jak .NET Core, najnowsza wersja programu Visual Studio, Microsoft Azure i tak dalej.

6. **Jakie są umiejętności osób dostępnych do pracy nad funkcją i jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu EF i nasi współpracownicy społeczności mają różne poziomy doświadczenia w różnych dziedzinach, więc musimy odpowiednio zaplanować. Nawet gdybyśmy chcieli mieć "wszystkie ręce na pokładzie", aby pracować nad konkretną funkcją, taką jak tłumaczenia GroupBy lub wiele do wielu, nie byłoby to praktyczne.
