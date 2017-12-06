---
title: "EF Core i EF6 — które jest odpowiednie dla Ciebie"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core i EF6: które z nich jest odpowiedni

Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Ponieważ EF Core jest nowym produktem i nadal brakuje niektórych funkcji O/RM o krytycznym znaczeniu, EF6 będą nadal wybór najbardziej odpowiedniej dla wielu aplikacji.

**Są to typy aplikacji, których firma Microsoft zaleca, przy użyciu EF Core dla:**

* Nowe aplikacje, które nie wymagają funkcji, które nie zostały jeszcze zaimplementowane w EF Core. Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.

* Aplikacji przeznaczonych dla platformy .NET Core, takich jak aplikacje systemu Windows platformy Uniwersalnej i ASP.NET Core. Te aplikacje nie można użyć EF6, ponieważ wymaga programu .NET Framework (np. .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

Z powodu zmiany podstawowych rdzeni EF nie zaleca się próby przenieść aplikację EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany. Jeśli chcesz przejść do EF Core, aby korzystać z nowych funkcji, a następnie upewnij się, że są znane swoje ograniczenia, przed rozpoczęciem. Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.

**Należy wyświetlić przejście od EF6 na rdzeń EF jako portu, a nie uaktualnienie.** Zobacz [eksportowanie z EF6 do EF Core](porting/index.md) Aby uzyskać więcej informacji.
