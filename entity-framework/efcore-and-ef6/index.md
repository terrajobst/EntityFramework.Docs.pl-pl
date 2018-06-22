---
title: Porównanie programów EF Core i EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002759"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

Istnieją dwie różne wersje programu Entity Framework, Entity Framework Core i Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) to technologia dostępu próby sklonowania i przetestowany danych z wielu lat funkcji i stabilizacji. On opublikowany po raz pierwszy w 2008 jako część programu .NET Framework 3.5 z dodatkiem SP1 i programu Visual Studio 2008 z dodatkiem SP1. Począwszy od wersji EF4.1 została wysłana jako [pakietu EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) — obecnie jedną z najbardziej popularnych pakiety na NuGet.org.

EF6 jest nadal obsługiwane produktu i będzie wyświetlana poprawek i drobne ulepszenia przez pewien czas do trybu.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) to lekkie, rozszerzalne i wieloplatformowych wersji programu Entity Framework. Podstawowe EF wprowadzono wiele ulepszeń i nowe funkcje w porównaniu z EF6. W tym samym czasie EF Core jest kod podstawowy, a nie jako dojrzałe jako EF6.

Podstawowe EF temu środowisko dewelopera z EF6 i większość najwyższego poziomu interfejsów API pozostają takie same, EF rdzenie będą możesz bardzo dostosowanym do pracowników, którzy użyli EF6. W tym samym czasie EF Core jest wbudowany w zupełnie nowy zestaw składników podstawowych. Oznacza to, że EF Core automatycznie nie dziedziczy z EF6 wszystkich funkcji. Gdy niektóre funkcje te będą widoczne w przyszłych wersjach, innych rzadziej używanych funkcji nie będzie zaimplementowana EF Core.

Nowe, rozszerzalne i lightweight rdzeni również zezwolił nam dodać niektóre funkcje do Core EF, który nie będzie zaimplementowana w EF6 (takie jak klucze alternatywne, aktualizacje wsadowe i oceny mieszanych/bazy danych klienta w zapytaniach LINQ).
