---
title: Co nowego — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271445"
---
# <a name="whats-new-in-ef6"></a>Co nowego w programie EF6

Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.
Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.
Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).

Ta strona zawiera dokumenty dotyczące funkcji uwzględnionych w każdej nowej wersji.

## <a name="recent-releases"></a>Najnowsze wersje

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aktualizacja narzędzi EF w programie Visual Studio 2017 15,7

W maju 2018 firma Microsoft udostępniła zaktualizowaną wersję narzędzi EF w ramach programu Visual Studio 2017 15,7.
Obejmuje to ulepszenia niektórych typowych punktów:

- Poprawki dla kilku błędów ułatwień dostępu do interfejsu użytkownika
- Obejście SQL Server regresji wydajności podczas generowania modeli z istniejących baz danych [#4](https://github.com/aspnet/entityframework6/issues/4)
- Obsługa aktualizowania modeli dla większych modeli w SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Innym ulepszeniem tej nowej wersji narzędzi EF jest zainstalowanie środowiska uruchomieniowego EF 6,2 podczas tworzenia modelu w nowym projekcie. W starszych wersjach programu Visual Studio można używać środowiska uruchomieniowego EF 6,2 (a także dowolnej starszej wersji EF) przez zainstalowanie odpowiedniej wersji pakietu NuGet.

### <a name="ef-62-runtime"></a>Środowisko uruchomieniowe Dr 6,2

Środowisko uruchomieniowe EF 6,2 zostało wydane do NuGet w październiku z 2017.
Dziękujemy za dążenie do wysiłków naszej społeczności uczestników oprogramowania typu open source, EF 6,2 zawiera wiele [poprawek błędów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) i [ulepszeń produktów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Poniżej przedstawiono krótką listę najważniejszych zmian wpływających na środowisko uruchomieniowe EF 6,2:

- Skrócenie czasu uruchamiania przez załadowanie pierwszego modelu kodu z trwałej pamięci podręcznej [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Interfejs API Fluent do definiowania indeksów [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- Dbfunctions. like () w celu umożliwienia pisania zapytań LINQ, które są tłumaczone na takie jak w programie SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Program migruje. exe obsługuje teraz- [#240](https://github.com/aspnet/EntityFramework6/issues/240) opcji skryptu
- EF6 może teraz korzystać z wartości kluczy generowanych przez sekwencję w SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizowanie listy błędów przejściowych dla strategii wykonywania SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Usterki Ponawianie próby wykonania zapytań lub poleceń SQL kończy się niepowodzeniem z błędem "SqlParameter jest już zawarty w innym SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Usterki Obliczanie programu DBQuery. ToString () często trwa w debugerze [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Przyszłe wersje

Aby uzyskać informacje na temat przyszłej wersji programu EF6, zapoznaj się z naszym [planem](roadmap.md).

## <a name="past-releases"></a>Wcześniejsze wersje

Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.
