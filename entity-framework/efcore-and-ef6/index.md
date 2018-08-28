---
title: Porównanie programów EF Core i EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997127"
---
# <a name="compare-ef-core--ef6"></a>Porównanie programów EF Core i EF6

Istnieją dwie różne wersje programu Entity Framework, platformy Entity Framework Core i Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) jest danych i przetestowanej technologii dostępu do wielu lat pracy nad funkcjami i stabilizacją. Je po raz pierwszy w 2008 roku jako część .NET Framework 3.5 SP1 i Visual Studio 2008 z dodatkiem SP1. Począwszy od wersji EF4.1 jest dostarczana jako [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/) — obecnie jedną z najbardziej popularnych pakietów w witrynie NuGet.org.

Programy EF6 w dalszym ciągu być obsługiwane produktu i będą nadal widzieć poprawki błędów i drobne ulepszenia przez pewien czas na osiągnięcie.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) to lekki, rozszerzalne i dla wielu platform wersją programu Entity Framework. EF Core wprowadzono wiele ulepszeń i nowych funkcji w porównaniu z platformy EF6. W tym samym czasie EF Core jest nowy kod podstawowy i nie jako dojrzała jako EF6.

EF Core zapewnia środowisko programistyczne z programu EF6 i najczęściej APIs najwyższego poziomu pozostają bez zmian, dzięki współdziałaniu bardzo podobnie do osób, które były używane platformy EF6 programu EF Core. W tym samym czasie programu EF Core jest wbudowana w całkowicie nowy zestaw podstawowych składników. Oznacza to, że programu EF Core z programu EF6 nie dziedziczy automatycznie wszystkich funkcji. Podczas gdy niektóre z tych funkcji będzie wyświetlany w przyszłych wersjach, inne rzadziej używane funkcje nie będzie zaimplementowana w wersji EF Core.

Core nowe, rozszerzone i uproszczone ma także umożliwiło nam dodać niektóre funkcje do programu EF Core, który nie będzie zaimplementowana w EF6 (np. klucze alternatywne, aktualizacje usługi batch i oceny mieszane/bazy danych klienta w zapytaniach LINQ).
