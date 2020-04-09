---
title: EF Core publikacje i planowanie
description: Aktualne wydania EF Core oraz szczegóły harmonogramu/planowania przyszłych wydań
author: ajcvickers
ms.date: 03/03/2020
uid: core/what-is-new/index
ms.openlocfilehash: 89687417685f291b44dcb250c96c5c9fa57da80f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634264"
---
# <a name="ef-core-releases-and-planning"></a>EF Core publikacje i planowanie

## <a name="stable-releases"></a>Stabilne wydania

| Release | Struktura docelowa | Obsługiwane do | Linki
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.3) | .NET Standard 2.0 | 3 grudnia 2022 (LTS) | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Data wygaśnięcia 3 marca 2020 r. | [Anons](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Przełomowe zmiany](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Data wygaśnięcia 23 grudnia 2019 r. | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 sierpnia 2021 (LTS) | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Wygasła 1 października 2018 r. | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Wygasła 27 czerwca 2019 r. | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Wygasła 27 czerwca 2019 r. | [Ogłoszenie](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Zobacz [obsługiwane platformy,](../platforms/index.md) aby uzyskać informacje na temat określonych platform obsługiwanych przez każdą wersję EF Core.

Zobacz [zasady pomocy technicznej platformy .NET,](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) aby uzyskać informacje na temat wygasania pomocy technicznej i wydań pomocy technicznej długoterminowej (LTS).

## <a name="guidance-on-updating-to-new-releases"></a>Wskazówki dotyczące aktualizacji do nowych wydań

* Obsługiwane wersje są załatane pod kątem zabezpieczeń i innych krytycznych błędów. Zawsze używaj najnowszej poprawki danej wersji. Na przykład dla EF Core 2.1 użyj 2.1.14.
* Główne aktualizacje wersji (na przykład z EF Core 2 do EF Core 3) często mają istotne zmiany. Podczas aktualizacji w głównych wersjach zaleca się dokładne testowanie. Użyj linków zmiany podziału powyżej, aby uzyskać wskazówki dotyczące radzenia sobie ze zmianami breaking.
* Aktualizacje wersji pomocniczej zazwyczaj nie zawierają zmian w przerwach. Jednak dokładne testowanie jest nadal zalecane, ponieważ nowe funkcje mogą wprowadzać regresji.

## <a name="release-planning-and-schedules"></a>Planowanie wersji i harmonogramy

Wydania EF Core są zgodne z [harmonogramem wysyłki .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

Wydania łat zwykle są wysyłane co miesiąc, ale mają długi czas realizacji.
Pracujemy nad tym, aby to poprawić.

Zobacz [proces planowania wersji, aby](release-planning.md) uzyskać więcej informacji na temat tego, jak decydujemy, co wysłać w każdej wersji.
Zazwyczaj nie robimy szczegółowego planowania dalszego niż następne główne lub niewielkie wydanie.

## <a name="ef-core-50"></a>EF Core 5.0

Kolejną planowaną stabilną wersją jest **EF Core 5.0**, zaplanowana na listopad 2020.

[Plan wysokiego poziomu dla EF Core 5.0](ef-core-5.0/plan.md) został utworzony w następstwie [procesu planowania udokumentowanych wersji](release-planning.md).

Twoja opinia na temat planowania jest ważna.
Najlepszym sposobem, aby wskazać znaczenie problemu jest głosowanie (thumbs-up) 👍dla tego problemu na GitHub.
Te dane zostaną następnie uwzględnione w procesie planowania następnej wersji.

### <a name="get-it-now"></a>Pobierz teraz!

Pakiety EF Core 5.0 są **już dostępne** jako

* [Codzienne kompilacje](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md)
  * Wszystkie najnowsze funkcje i poprawki błędów. Ogólnie bardzo stabilny; 57,000+ testy uruchamiane przeciwko każdej kompilacji.
* [Podglądy na NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
  * Pozostaje w tyle za dziennymi kompilacjami, ale są testowane do pracy z odpowiednimi ASP.NET podglądów Core i .NET Core.

Korzystanie z wersji zapoznawców lub codziennych kompilacji to świetny sposób na znalezienie problemów i przekazanie opinii tak wcześnie, jak to możliwe.
Im szybciej otrzymamy taką opinię, tym bardziej prawdopodobne, że będzie można ją zasuceniu przed następnym oficjalnym wydaniem.
