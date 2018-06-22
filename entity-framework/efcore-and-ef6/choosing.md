---
title: EF Core i EF6 — które jest odpowiednie dla Ciebie
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002824"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core i EF6: które z nich jest odpowiedni

Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Należy rozważyć użycie EF Core dla nowych aplikacji, jeśli chcesz wykorzystać wszystkie możliwości EF podstawowych i aplikacja nie wymaga dowolne funkcje, które nie zostały jeszcze zaimplementowane w EF Core.

EF6 wymaga programu .NET Framework 4.0 (lub nowszy) i jest obsługiwany tylko w systemie Windows (np. jego nie działa w .NET Core i nie jest obsługiwany w innych systemach operacyjnych), ale jest nadal działało wybór dla nowych aplikacji, jak długo te ograniczenia są dopuszczalne i pplication nie wymaga nowych funkcji w EF podstawowych, które nie są dostępne do EF6.

Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

Z powodu zmiany podstawowych rdzeni EF nie zaleca się próby przenieść aplikację EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany. Jeśli chcesz przejść do EF Core, aby korzystać z nowych funkcji, a następnie upewnij się, że są znane swoje ograniczenia, przed rozpoczęciem. Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.

**Należy wyświetlić przejście od EF6 na rdzeń EF jako portu, a nie uaktualnienie.** Zobacz [eksportowanie z EF6 do EF Core](porting/index.md) Aby uzyskać więcej informacji.
