---
title: Plan dla core 5.0 struktury podmiotu
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136216"
---
# <a name="plan-for-entity-framework-core-50"></a>Plan dla core 5.0 struktury podmiotu

Jak opisano w [procesie planowania,](../release-planning.md)zebraliśmy informacje od zainteresowanych stron na wstępny plan wydania EF Core 5.0.

> [!IMPORTANT] 
> Plan ten jest nadal w toku. Nic tu nie jest zobowiązaniem. Ten plan jest punktem wyjścia, który będzie ewoluował, gdy dowiemy się więcej. Niektóre rzeczy, które nie są obecnie planowane na 5.0, mogą zostać wciągnięte. Niektóre rzeczy obecnie planowane na 5.0 może dostać punted obecnie.

### <a name="version-number-and-release-date"></a>Numer wersji i data wydania.

EF Core 5.0 jest obecnie planowane do wydania w [tym samym czasie co .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Wersja "5.0" została wybrana tak, aby wyrównać z .NET 5.0.

### <a name="supported-platforms"></a>Obsługiwane platformy

EF Core 5.0 ma działać na dowolnej platformie .NET 5.0 w oparciu o [zbieżność tych platform z platformą .NET Core.](https://devblogs.microsoft.com/dotnet/introducing-net-5/) Co to oznacza w zakresie .NET Standard i rzeczywiste TFM używane jest nadal TBD.

EF Core 5.0 nie będzie działać w programie .NET Framework.

### <a name="breaking-changes"></a>Fundamentalne zmiany

EF Core 5.0 będzie zawierał pewne przełomowe zmiany, ale będą one znacznie mniej dotkliwe niż miało to miejsce w przypadku EF Core 3.0. Naszym celem jest umożliwienie większości aplikacji aktualizacji bez przerywania.

Oczekuje się, że będą pewne przełomowe zmiany dla dostawców baz danych, zwłaszcza wokół obsługi TPT. Oczekujemy jednak, że praca aktualizacji dostawcy dla 5.0 będzie mniejsza niż była wymagana do aktualizacji dla 3.0.

## <a name="themes"></a>Motywy

Wydobyliśmy kilka głównych obszarów lub tematów, które będą stanowić podstawę dla dużych inwestycji w EF Core 5.0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Wiele do wielu właściwości nawigacji (aka "pomiń nawigacji")

Główna @smitpatel programistka: i@AndriySvyryd

Śledzone przez [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Rozmiar koszulki: L

Stan: W toku

Wiele do wielu jest [najbardziej żądaną funkcją](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 głosów) na zaległości GitHub.

Obsługa relacji wiele do wielu w całości jest śledzona jako [#10508.](https://github.com/aspnet/EntityFrameworkCore/issues/10508) Można to podzielić na trzy główne obszary:

* Pomiń właściwości nawigacji. Umożliwiają one model do użycia dla zapytań, itp. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Typy jednostek worka właściwości. Umożliwiają one standardowy typ CLR `Dictionary`(np. ) do użycia dla wystąpień jednostki, tak że jawny typ CLR nie jest potrzebny dla każdego typu jednostki. (Rozciągnij na 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Cukier do łatwej konfiguracji relacji wiele do wielu. (Rozciągnij na 5.0.)

Uważamy, że najbardziej znaczący bloker dla tych, którzy chcą wiele do wielu wsparcia nie jest w stanie korzystać z "naturalnych" relacji, bez odwoływania się do tabeli sprzężenia, w logice biznesowej, takich jak zapytania. Typ jednostki tabeli sprzężenia może nadal istnieć, ale nie powinien przeszkadzać logice biznesowej. Dlatego zdecydowaliśmy się zająć właściwości nawigacji pominąć dla 5.0.

W tej chwili inne części wielu do wielu są realizowane jako cel stretch dla EF Core 5.0. Oznacza to, że nie są one obecnie w planie 5.0, ale jeśli wszystko pójdzie dobrze, mamy nadzieję, że je wciągniemy.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapowanie dziedziczenia tabela na typ (TPT)

Główny programista:@AndriySvyryd

Śledzone przez [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Rozmiar koszulki: XL

Stan: W toku

Robimy TPT, ponieważ jest to zarówno bardzo pożądana funkcja (~ 254 głosów; 3 ogólnie), a ponieważ wymaga pewnych zmian niskiego poziomu, które uważamy za odpowiednie dla fundamentalnego charakteru ogólnego planu .NET 5. Oczekujemy, że spowoduje to wprowadzenie zmian w przypadku dostawców baz danych, chociaż powinny one być znacznie mniej dotkliwe niż zmiany wymagane dla 3.0.

## <a name="filtered-include"></a>Filtrowane uwzględnić

Główny programista:@maumar

Śledzone przez [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Rozmiar koszulki: M

Stan: W toku

Filtrowane include to bardzo pożądana funkcja (~317 głosów; druga ogólna), która nie jest ogromną ilością pracy i która, jak sądzimy, odblokuje lub ułatwi wiele scenariuszy, które obecnie wymagają filtrów na poziomie modelu lub bardziej złożonych zapytań.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racjonalizuj ToTable, ToQuery, ToView, FromSql itp.

Główna @maumar programistka: i@smitpatel

Śledzone przez [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Rozmiar koszulki: L

Stan: W toku

Poczyniliśmy postępy w poprzednich wersjach w kierunku obsługi nieprzetworzonego SQL, typów bezkluzywnych i powiązanych obszarów. Istnieją jednak zarówno luki, jak i niespójności w sposobie, w jaki wszystko działa razem jako całość. Celem dla 5.0 jest naprawienie tych i stworzenie dobrego środowiska do definiowania, migracji i używania różnych typów jednostek i skojarzonych z nimi zapytań i artefaktów bazy danych. Może to również obejmować aktualizacje interfejsu API skompilowanych kwerend.

Należy zauważyć, że ten element może spowodować niektóre zmiany podziału na poziomie aplikacji, ponieważ niektóre funkcje, które obecnie posiadamy, są zbyt permisywne, dzięki czemu mogą szybko prowadzić ludzi do dołków awarii. Prawdopodobnie w końcu blokujemy niektóre z tych funkcji wraz ze wskazówkami, co zamiast tego zrobić.

## <a name="general-query-enhancements"></a>Ogólne ulepszenia zapytań

Główna @smitpatel programistka: i@maumar

Śledzone przez [problemy oznaczone `area-query` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Rozmiar koszulki: XL

Stan: W toku

Kod tłumaczenia zapytania został szczegółowo przepisany dla EF Core 3.0. Kod kwerendy jest ogólnie w znacznie bardziej niezawodnym stanie z tego powodu. W przypadku 5.0 nie planujemy wprowadzania istotnych zmian zapytań, poza tymi, które są potrzebne do obsługi TPT i pomijania właściwości nawigacji. Jednak nadal jest wiele pracy potrzebne do ustalenia niektórych długów technicznych pozostałych z 3.0 remont. Planujemy również naprawić wiele błędów i zaimplementować małe ulepszenia, aby jeszcze bardziej poprawić ogólne środowisko kwerendy.

## <a name="migrations-and-deployment-experience"></a>Migracje i środowisko wdrażania

Główna programistka:@bricelam

Śledzone przez [#19587](https://github.com/dotnet/efcore/issues/19587)

Rozmiar koszulki: L

Stan: W toku

Obecnie wielu deweloperów migruje swoje bazy danych w czasie uruchamiania aplikacji. Jest to łatwe, ale nie jest zalecane, ponieważ:

* Wiele wątków/procesów/serwerów może próbować migrować bazę danych jednocześnie
* Aplikacje mogą próbować uzyskać dostęp do niespójnego stanu, gdy tak się dzieje
* Zazwyczaj uprawnienia bazy danych do modyfikowania schematu nie powinny być przyznawane do wykonywania aplikacji
* Trudno jest powrócić do czystego stanu, jeśli coś pójdzie nie tak

Chcemy zapewnić lepsze środowisko w tym miejscu, co pozwala na łatwy sposób migracji bazy danych w czasie wdrażania. Powinno to:

* Praca na systemach Linux, Mac i Windows
* Bądź dobrym doświadczeniem w wierszu polecenia
* Scenariusze pomocy technicznej z kontenerami
* Praca z powszechnie używanymi rzeczywistymi narzędziami/przepływami wdrażania
* Integracja z co najmniej programem Visual Studio

Rezultatem może być wiele drobnych ulepszeń w EF Core (na przykład lepsze migracje na SQLite), wraz z wytycznymi i długoterminową współpracą z innymi zespołami w celu poprawy kompleksowych doświadczeń, które wykraczają poza tylko EF.

## <a name="ef-core-platforms-experience"></a>Środowisko platform EF Core 

Główna @roji programistka: i@bricelam

Śledzone przez [#19588](https://github.com/dotnet/efcore/issues/19588)

Rozmiar koszulki: L

Stan: Nie rozpoczęto

Mamy dobre wskazówki dotyczące korzystania z EF Core w tradycyjnych aplikacjach internetowych podobnych do MVC. Brakuje wskazówki dla innych platform i modeli aplikacji lub są nieaktualne. W przypadku EF Core 5.0 planujemy zbadać, poprawić i udokumentować doświadczenie korzystania z EF Core z:

* Blazor
* Xamarin, w tym za pomocą AOT / linker historia
* WinForms / WPF / WinUI i ewentualnie inne U.I. Ram

Prawdopodobnie będzie to wiele niewielkich ulepszeń ef core, wraz z wytycznymi i długoterminową współpracą z innymi zespołami w celu poprawy kompleksowych doświadczeń, które wykraczają poza tylko EF.

Konkretne obszary, które planujemy przyjrzeć się to:

* Wdrażanie, w tym środowisko korzystania z narzędzi EF, takich jak migracje
* Modele aplikacji, w tym Xamarin i Blazor, i prawdopodobnie inne
* Środowiska SQLite, w tym doświadczenie przestrzenne i przebudowa tabeli
* AOT i łączenie doświadczeń
* Integracja diagnostyki, w tym liczniki perf

## <a name="performance"></a>Wydajność

Główny programista:@roji

Śledzone przez [problemy oznaczone `area-perf` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Rozmiar koszulki: L

Stan: W toku

W przypadku ef core planujemy ulepszyć nasz zestaw testów porównawczych wydajności i wprowadzić ukierunkowane ulepszenia wydajności w czasie wykonywania. Ponadto planujemy zakończyć nowy interfejs API przetwarzania wsadowego ADO.NET, który został prototypowany podczas cyklu wersji 3.0. Również w warstwie ADO.NET planujemy dodatkowe ulepszenia wydajności dla dostawcy Npgsql.

W ramach tych prac planujemy również dodać ADO.NET/EF liczniki wydajności Core i inne diagnostyki, stosownie do przypadku.

## <a name="architecturalcontributor-documentation"></a>Dokumentacja architektoniczna/autora

Dokumentator wiodący:@ajcvickers

Śledzone przez [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Rozmiar koszulki: L

Stan: W toku

Chodzi o to, aby ułatwić zrozumienie, co dzieje się w wewnętrznych EF Core. Może to być przydatne dla każdego, kto korzysta z EF Core, ale główną motywacją jest ułatwienie osobom zewnętrznym:

* Współtworzenie kodu EF Core
* Tworzenie dostawców baz danych
* Tworzenie innych rozszerzeń

## <a name="microsoftdatasqlite-documentation"></a>Dokumentacja microsoft.data.sqlite

Dokumentator wiodący:@bricelam

Śledzone przez [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

Rozmiar koszulki: M

Stan: Ukończono. Nowa dokumentacja jest [dostępna w witrynie dokumentów firmy Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

Zespół EF jest również właścicielem dostawcy ADO.NET microsoft.Data.Sqlite. Planujemy w pełni udokumentować tego dostawcę w ramach wersji 5.0.

## <a name="general-documentation"></a>Dokumentacja ogólna

Dokumentator wiodący:@ajcvickers

Śledzone przez [problemy w repo dokumentów w 5.0 kamień milowy](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Rozmiar koszulki: L

Stan: W toku

Jesteśmy już w trakcie aktualizacji dokumentacji dla wersji 3.0 i 3.1. Pracujemy również nad:
  * Przegląd dokumentów wprowadzenie, aby uczynić je bardziej przystępne / łatwiejsze do naśladowania
  * Reorganizacja dokumentów w celu ułatwienia znajdowania i dodawania odsyłaczy
  * Dodawanie więcej szczegółów i wyjaśnień do istniejących dokumentów
  * Aktualizowanie próbek i dodawanie kolejnych przykładów

## <a name="fixing-bugs"></a>Naprawianie błędów

Śledzone przez [problemy oznaczone `type-bug` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

@rojiDeweloperzy: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers

Rozmiar koszulki: L

Stan: W toku

W momencie pisania mamy 135 błędów triaged do naprawienia w wersji 5.0 (z 62 już ustalone), ale istnieje znaczne nakładanie się z _ogólne ulepszenia zapytania_ powyżej.

Przychodzące stawki (problemy, które kończą się jako praca w kamieniu milowym) było około 23 problemów miesięcznie w trakcie wydania 3.0. Nie wszystkie z nich trzeba będzie naprawić w 5.0. W przybliżeniu planujemy rozwiązać dodatkowe 150 problemów w przedziale czasowym 5.0.

## <a name="small-enhancements"></a>Małe ulepszenia

Śledzone przez [problemy oznaczone `type-enhancement` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

@rojiDeweloperzy: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers

Rozmiar koszulki: L

Stan: W toku

Oprócz większych funkcji opisanych powyżej, mamy również wiele mniejszych ulepszeń zaplanowanych na 5.0, aby naprawić "cięcia papieru". Należy zauważyć, że wiele z tych ulepszeń są również objęte bardziej ogólne tematy opisane powyżej.

## <a name="below-the-line"></a>Poniżej linii

Śledzone przez [problemy oznaczone `consider-for-next-release` ](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Są to poprawki błędów i ulepszenia, które **nie** są obecnie zaplanowane dla wersji 5.0, ale będziemy patrzeć na jako cele rozciągania w zależności od postępów poczynionych w powyższej pracy.

Ponadto podczas planowania zawsze bierzemy pod uwagę [najczęściej wybierane kwestie.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) Odcięcie którejkolwiek z tych kwestii z uwolnienia jest zawsze bolesne, ale potrzebujemy realistycznego planu dla zasobów, które mamy.

## <a name="feedback"></a>Opinia

Twoja opinia na temat planowania jest ważna. Najlepszym sposobem, aby wskazać znaczenie problemu jest głosowanie (thumbs-up) dla tego problemu na GitHub. Te dane zostaną następnie uwzględnione w [procesie planowania](../release-planning.md) następnej wersji.
