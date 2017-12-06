---
title: "Porównaj EF Core & EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>Porównaj EF Core & EF6

Istnieją dwie wersje programu Entity Framework, Entity Framework Core oraz Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) to technologia dostępu próby sklonowania i przetestowany danych z wielu lat funkcji i stabilizacji. On opublikowany po raz pierwszy w 2008 jako część programu .NET Framework 3.5 z dodatkiem SP1 i programu Visual Studio 2008 z dodatkiem SP1. Począwszy od wersji EF4.1 została wysłana jako [pakietu EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) — obecnie najpopularniejsze pakietu na NuGet.org.

EF6 jest nadal obsługiwane produktu i będzie wyświetlana poprawek i drobne ulepszenia przez pewien czas do trybu.

## <a name="entity-framework-core"></a>Program Entity Framework Core

Entity Framework Core (EF Core) to lekkie, rozszerzalne i wieloplatformowych wersji programu Entity Framework. Podstawowe EF wprowadzono wiele ulepszeń i nowe funkcje w porównaniu z EF6. W tym samym czasie EF Core jest kod podstawowy, a nie jako dojrzałe jako EF6.

Podstawowe EF temu środowisko dewelopera z EF6 i większość najwyższego poziomu interfejsów API pozostają takie same, EF rdzenie będą możesz bardzo dostosowanym do pracowników, którzy użyli EF6. W tym samym czasie EF Core jest wbudowany w zupełnie nowy zestaw składników podstawowych. Oznacza to, że EF Core automatycznie nie dziedziczy z EF6 wszystkich funkcji. Niektóre z tych funkcji będą widoczne w przyszłych wersjach (na przykład opóźnionego ładowania i połączenia odporności), inne rzadko używane się, że nie będzie zaimplementowana funkcji EF główną.

Nowe, rozszerzalne i lightweight rdzeni również zezwolił nam dodać niektóre funkcje do Core EF, który nie będzie zaimplementowana w EF6 (takie jak klucze alternatywne i oceny mieszanych/bazy danych klienta w zapytaniach LINQ).
