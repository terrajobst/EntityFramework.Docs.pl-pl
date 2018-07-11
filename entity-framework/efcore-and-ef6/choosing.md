---
title: EF Core i EF6 — które z nich jest odpowiednie dla Ciebie
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949143"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core i EF6: który z nich jest odpowiedni dla Ciebie

Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Wskazówki dotyczące nowych aplikacji

Należy rozważyć użycie programu EF Core dla nowych aplikacji, jeśli chcesz wykorzystać wszystkie możliwości programu EF Core i aplikacja nie wymaga żadnych funkcji, które nie są jeszcze zaimplementowane w programie EF Core.

Programy EF6 wymaga programu .NET Framework 4.0 (lub nowszym) i jest obsługiwane tylko dla Windows (oznacza to, że EF6 nie działa obecnie na platformie .NET Core i nie jest obsługiwana w innych systemach operacyjnych), ale nadal jest wygodną wybranego dla nowych aplikacji, jak długo te ograniczenia są dopuszczalne i aplikacja nie wymaga nowych funkcji w programie EF Core, które nie są dostępne dla platformy EF6.

Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli programu EF Core może być dobrym wyborem dla twojej aplikacji.

## <a name="guidance-for-existing-ef6-applications"></a>Wskazówki dotyczące istniejących aplikacji EF6

Ze względu na fundamentalne zmiany w programie EF Core nie jest zalecane Próba przeniesienia aplikacji EF6 do programu EF Core, chyba że istnieje istotny powód, aby wprowadzić zmianę. Jeśli chcesz przejść do programu EF Core, aby skorzystać z nowych funkcji, a następnie upewnij się, że możesz świadomość jej ograniczenia przed rozpoczęciem. Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli programu EF Core może być dobrym wyborem dla twojej aplikacji.

**Należy wyświetlić przenoszenie z programu EF6 do programu EF Core jako portu, a nie uaktualnienie.** Zobacz [przenoszenie z programu EF6 do programu EF Core](porting/index.md) Aby uzyskać więcej informacji.
