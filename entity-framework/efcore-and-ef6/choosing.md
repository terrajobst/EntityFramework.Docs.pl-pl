---
title: "EF Core i EF6 — które jest odpowiednie dla Ciebie"
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
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="50f34-102">EF Core i EF6: które z nich jest odpowiedni</span><span class="sxs-lookup"><span data-stu-id="50f34-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="50f34-103">Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="50f34-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="50f34-104">Wskazówki dotyczące nowych aplikacji</span><span class="sxs-lookup"><span data-stu-id="50f34-104">Guidance for new applications</span></span>

<span data-ttu-id="50f34-105">Należy rozważyć użycie EF Core dla nowych aplikacji, jeśli chcesz wykorzystać wszystkie możliwości EF podstawowych i aplikacja nie wymaga dowolne funkcje, które nie zostały jeszcze zaimplementowane w EF Core.</span><span class="sxs-lookup"><span data-stu-id="50f34-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="50f34-106">EF6 wymaga programu .NET Framework 4.0 (lub nowszy) i jest obsługiwany tylko w systemie Windows (np. jego nie działa w .NET Core i nie jest obsługiwany w innych systemach operacyjnych), ale jest nadal działało wybór dla nowych aplikacji, jak długo te ograniczenia są dopuszczalne i pplication nie wymaga nowych funkcji w EF podstawowych, które nie są dostępne do EF6.</span><span class="sxs-lookup"><span data-stu-id="50f34-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="50f34-107">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50f34-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="50f34-108">Wskazówki dotyczące istniejących aplikacji EF6</span><span class="sxs-lookup"><span data-stu-id="50f34-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="50f34-109">Z powodu zmiany podstawowych rdzeni EF nie zaleca się próby przenieść aplikację EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="50f34-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="50f34-110">Jeśli chcesz przejść do EF Core, aby korzystać z nowych funkcji, a następnie upewnij się, że są znane swoje ograniczenia, przed rozpoczęciem.</span><span class="sxs-lookup"><span data-stu-id="50f34-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="50f34-111">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="50f34-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="50f34-112">**Należy wyświetlić przejście od EF6 na rdzeń EF jako portu, a nie uaktualnienie.**</span><span class="sxs-lookup"><span data-stu-id="50f34-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="50f34-113">Zobacz [eksportowanie z EF6 do EF Core](porting/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="50f34-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
