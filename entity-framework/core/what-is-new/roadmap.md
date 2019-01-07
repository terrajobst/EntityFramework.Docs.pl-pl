---
title: Entity Framework Core plan
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 7eba9e1a8e145ef407f844ff3a3ab3069495b7ae
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058737"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core plan

> [!IMPORTANT]
> Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.

### <a name="ef-core-30"></a>EF Core 3.0

Przy użyciu programu EF Core 2.2 spory naszym głównym celem jest teraz EF Core 3.0, które mają zostać wyrównane przy użyciu platformy .NET Core 3.0 i zwalnia ASP.NET 3.0.

Firma Microsoft nie wykonano żadnych nowych funkcji, więc [EF Core 3.0 w wersji zapoznawczej 1 pakiety opublikowane w galerii NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) grudnia 2018 r. tylko zawierać [poprawki, drobne ulepszenia i zmiany, które wprowadziliśmy w Przygotowanie do pracy 3.0](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

W rzeczywistości nadal trzeba dostosować naszych [wersji, planowania](#release-planning-process) 3.0, aby upewnić się, mamy właściwy zestaw funkcji, które można wykonać w wyznaczonym czasie.
Firma Microsoft udostępni więcej informacji, uzyskujemy bardziej przejrzysty, ale poniżej przedstawiono pewne ogólne motywy i funkcje, firma Microsoft itend pracować nad:

- **Ulepszenia zapytań LINQ ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ umożliwia tworzenie zapytań bazy danych bez opuszczania usługi z wybranego języka, wykorzystując zaawansowane wpisz informacje funkcji IntelliSense i sprawdzanie typów w czasie kompilacji.
  Ale LINQ można również napisać nieograniczonej liczby zapytań skomplikowane, a który zawsze było ogromnym wyzwaniom dla dostawców LINQ.
  W pierwszym kilka wersji programu EF Core możemy rozwiązać, w części za ustalenie, jakie części zapytania mogą być tłumaczone do bazy danych SQL, a następnie, umożliwiając pozostałe kwerenda do wykonania w pamięci na komputerze klienckim.
  Wykonanie tego po stronie klienta może być pożądana w niektórych sytuacjach, ale w innych przypadkach może to skutkować nieefektywne zapytań, które nie mogą być określane, dopóki aplikacja jest wdrażana w środowisku produkcyjnym.
  W programie EF Core 3.0 planujemy wprowadzić głęboki zmiany sposobu działania naszej implementacji LINQ i jak możemy przetestować.
  Cele są aby działał on bardziej niezawodnie (na przykład, aby uniknąć przerywanie zapytania w wersjach poprawki), aby można było tłumaczenie więcej wyrażeń poprawnie w bazie SQL, można wygenerować wydajne zapytania w większości przypadków i uniemożliwić nieefektywne zapytania przechodząc niewykryte.

- **Usługa cosmos DB obsługuje ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Pracujemy nad dostawcę usługi Cosmos DB dla platformy EF Core, aby umożliwić deweloperom zapoznać się z modelem programistyczne EF łatwo Kieruj usługi Azure Cosmos DB jako bazę danych aplikacji.
  Celem jest zapewnienie niektóre korzyści wynikające z usługi Cosmos DB, takie jak dystrybucji globalnej, "zawsze włączone" dostępność, elastyczną skalowalność i małego opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.
  Dostawca umożliwi większość funkcji EF Core, takie jak automatyczna zmiana, śledzenie, LINQ i konwersji wartości, przy użyciu interfejsu SQL API w usłudze Cosmos DB. Rozpoczęliśmy ten nakład pracy przed programem EF Core 2.2, i [Wprowadziliśmy pewne wersji dostawcy dostępne w wersji zapoznawczej](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Nowy plan jest, aby kontynuować, tworzenie dostawcy wraz z programem EF Core 3.0.   

- **C#Obsługa 8.0 ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Chcemy, aby klienci mogą korzystać z niektórych [nowe funkcje zostaną dodane w C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) strumieni asynchronicznych, takich jak (w tym await dla każdego) i typy referencyjne dopuszczającego wartość null podczas korzystania z programu EF Core.

- **Odwróć inżynierów widoki bazy danych do typów zapytań ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: W programie EF Core 2.1 Dodaliśmy obsługę typów zapytań, które mogą reprezentować dane, które mogą być odczytywane z bazy danych, ale nie można zaktualizować.
  Typy zapytań są widoki doskonałe rozwiązanie dla mapowania bazy danych, więc w programie EF Core 3.0, prosimy o poświęcenie do zautomatyzowania tworzenia typów zapytań na widoki baz danych.

- **Właściwość zbiór jednostek ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) i [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Ta funkcja jest zapewnienie jednostek, które przechowują dane w właściwości indeksowanych zamiast regularnego właściwości, a także o będzie mógł korzystać z wystąpień klasy .NET (potencjalnie coś tak proste, jak `Dictionary<string, object>`) do reprezentowania typów różnych jednostek w tym samym modelu platformy EF Core.
  Ta funkcja jest kamień przechodzenia krok po kroku do obsługi relacji wiele do wielu, bez jednostki sprzężenia, który jest jednym z najbardziej pożądanych ulepszenia dla platformy EF Core.

- **EF 6.3 na platformie .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: Rozumiemy, że wiele istniejących aplikacji używać starszych wersji programu EF i że przenoszenia ich do programu EF Core tylko po to, aby móc korzystać z platformy .NET Core może czasem wymagać znaczących nakładów pracy.
  Z tego powodu firma Microsoft będzie można dostosowanie Następna wersja programu EF 6 do uruchamiania na .NET Core 3.0.
  Robimy to w celu ułatwienia przenoszenia istniejących aplikacji przy minimalnych zmianach.
  Czy powstaną pewne ograniczenia (na przykład będzie wymagać nowego dostawcy, obsługa przestrzennych z programem SQL Server nie jest włączony) i są planowane do programów EF 6 żadne nowe funkcje.

W międzyczasie możesz użyć [tego zapytania w naszym narzędzie do śledzenia problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) aby zobaczyć elementy robocze wstępnie przypisane do wersji 3.0.

## <a name="schedule"></a>Harmonogram

Harmonogram dla platformy EF Core jest zsynchronizowany z [harmonogram platformy .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) i [harmonogram platformy ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Zaległości

[Punkt kontrolny zaległości](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) w naszym problem śledzenia zawiera problemy, oczekujemy, że działasz kiedyś lub uważamy, że ktoś od społeczności można analizować.
Klienci mogą przesyłać komentarze i głosów na temat tych problemów — Zapraszamy.
Współautorzy chcą pracować na dowolnym z tych problemów są zachęcani do najpierw Rozpocznij dyskusję na temat sposobu ich podejście.

Nigdy nie jest gwarancją, jaką pracujemy nad tym, na dowolnej danej funkcji w określonej wersji programu EF Core.
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

1. **Deweloperzy liczbę naszym zdaniem będzie używać tej funkcji i jak dużo lepiej wprowadzi na ich/korzystanie z aplikacji?** Odpowiedzi na to, firma Microsoft zbieranie opinii z wielu źródeł, komentarze i głosów problemów jest jednym z tych źródeł.

2. **Co to są osób rozwiązania można użyć, jeśli firma Microsoft nie jeszcze zaimplementować tę funkcję?** Na przykład wielu deweloperów można mapować tabelę sprzężenia w celu obejścia Brak natywnej obsługi wiele do wielu. Oczywiście nie wszystkim deweloperom chcesz to zrobić, ale można wiele i który jest liczona jako czynnika podczas podjęcie decyzji.

3. **Wdrażanie tej funkcji ewolucji architektury programu EF Core tak, aby przemieszczał się nam przybliżyć do wykonania innych funkcji?** Firma Microsoft zwykle preferować funkcje, które działają jako bloków konstrukcyjnych dla innych funkcji. Na przykład właściwość zbiór jednostek, ułatwisz nam idą w kierunku obsługi wiele do wielu i konstruktory jednostki włączono obsługę ładowania z opóźnieniem. 

4. **Funkcja punkt rozszerzeń?** Zwykle aby preferował punkty rozszerzeń za pośrednictwem funkcji normalne, ponieważ umożliwiają one programistom podłączyć ich zachowania, a także kompensuje wszelkie brakujące funkcje. 

5. **Co to jest współdziałanie funkcji w połączeniu z innymi produktami?** Będziemy preferować funkcje, które włącza lub znacznie poprawić środowisko przy użyciu programu EF Core przy użyciu innych produktów, takich jak .NET Core najnowszą wersję programu Visual Studio, Microsoft Azure, itp.

6. **Co to są umiejętności osób, które są dostępne w funkcji oraz jak najlepiej wykorzystać te zasoby?** Każdy członek zespołu platformy EF i naszej społeczności ma różne poziomy doświadczenia w innych obszarach, więc musimy odpowiednio zaplanować. Nawet wtedy, gdy chcemy mieć "cały zespół na pokładzie" pracy w określonych funkcji, takich jak tłumaczenia GroupBy lub wiele do wielu, byłoby niepraktyczne.

Jak wspomniano wcześniej, ten proces ewoluuje w każdej wersji.
W przyszłości zostanie podjęta próba dodać więcej możliwości dla członków społeczności Podaj dane wejściowe w naszych planach wydania.
Na przykład chcemy ułatwić przeglądanie projektów projektu funkcji i samego planu wersji.
