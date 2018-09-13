---
title: What's New - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490301"
---
# <a name="whats-new-in-ef6"></a>What's New in EF6

Zdecydowanie zaleca się, że umożliwia najnowszą wersję oficjalną programu Entity Framework upewnij się, że możesz korzystać z najnowszych funkcji i najwyższą trwałość.
Jednak firma Microsoft należy pamiętać, że może być konieczne użycie poprzedniej wersji lub czy może chcesz poeksperymentować z nowych ulepszeń w najnowszej wersji wstępnej.
Aby zainstalować określonych wersji EF, zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md).

Ta strona zawiera dokumentację funkcje, które znajdują się w kolejnych wersjach.

## <a name="recent-releases"></a>Najnowsze wersje

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>EF aktualizacji narzędzi programu Visual Studio 2017 wersji 15.7

W maju 2018 r. wydaliśmy zaktualizowaną wersję narzędzia EF jako część programu Visual Studio 2017 w wersji 15.7.
Zawiera usprawnienia w zakresie niektóre typowe słabe punkty:

- Poprawki dla kilku błędów dostępności interfejsu użytkownika
- Obejście regresji wydajności programu SQL Server podczas generowania modeli z istniejących baz danych [#4](https://github.com/aspnet/entityframework6/issues/4)
- Obsługa aktualizacji modeli większych modeli w programie SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Innym sposobem na usprawnienie w tym tej nowej wersji zestawu narzędzi platformy EF jest, że instaluje środowiskiem uruchomieniowym EF 6.2, podczas tworzenia modelu w nowym projekcie. Ze starszymi wersjami programu Visual Studio jest możliwość użycia środowiska uruchomieniowego EF 6.2 (a także żadnych poprzednich wersji EF) przez zainstalowanie odpowiedniej wersji pakietu NuGet.

### <a name="ef-62-runtime"></a>EF 6.2 środowiska uruchomieniowego

Środowiskiem uruchomieniowym EF 6.2 został wydany do narzędzia NuGet w października 2017 r.
Dziękuję w Wielkiej część wysiłek naszej społeczności uczestników projektów typu open source, EF 6.2 zawiera wiele [poprawki usterek](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) i [ulepszenia produktu](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Poniżej przedstawiono krótki opis listy najważniejszych zmian wpływających na środowiskiem uruchomieniowym EF 6.2:

- Zmniejsz czas uruchamiania, ładując Zakończono pierwszą modele kodu z trwała pamięć podręczna [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Interfejs Fluent API, aby zdefiniować indeksy [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() umożliwia zapisywanie zapytań LINQ, które dadzą podobne w języku SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe obsługuje teraz — opcja skryptu [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 teraz pracować z wartości klucza, wygenerowane za pomocą sekwencji w programie SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizuj listę błędów przejściowych strategii wykonywania usługi Azure SQL [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Usterka: Ponowieniem próby wykonania zapytania lub polecenia SQL nie powiodło się z "parametr SqlParameter jest już zawarty w innym SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Błąd: Obliczanie DbQuery.ToString() często upłynie limit czasu w debugerze [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Przyszłe wersje

Informacje na temat przyszłych wersjach programu EF6 można znaleźć w naszej [plan](roadmap.md).

## <a name="past-releases"></a>Poprzednich wersjach

[Poprzednich wersjach](past-releases.md) strona zawiera archiwum wszystkich wcześniejszych wersji programu EF i główne funkcje, które zostały wprowadzone w każdej wersji.
