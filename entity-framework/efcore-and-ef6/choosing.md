---
title: EF Core i EF6 — które z nich jest odpowiednie dla Ciebie
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993836"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="8ce80-102">EF Core i EF6: który z nich jest odpowiedni dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="8ce80-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="8ce80-103">Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ce80-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="8ce80-104">Wskazówki dotyczące nowych aplikacji</span><span class="sxs-lookup"><span data-stu-id="8ce80-104">Guidance for new applications</span></span>

<span data-ttu-id="8ce80-105">Należy rozważyć użycie programu EF Core dla nowych aplikacji, jeśli chcesz wykorzystać wszystkie możliwości programu EF Core i aplikacja nie wymaga żadnych funkcji, które nie są jeszcze zaimplementowane w programie EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ce80-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="8ce80-106">Programy EF6 wymaga programu .NET Framework 4.0 (lub nowszym) i jest obsługiwane tylko dla Windows (oznacza to, że EF6 nie działa obecnie na platformie .NET Core i nie jest obsługiwana w innych systemach operacyjnych), ale nadal jest wygodną wybranego dla nowych aplikacji, jak długo te ograniczenia są dopuszczalne i aplikacja nie wymaga nowych funkcji w programie EF Core, które nie są dostępne dla platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="8ce80-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="8ce80-107">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli programu EF Core może być dobrym wyborem dla twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ce80-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="8ce80-108">Wskazówki dotyczące istniejących aplikacji EF6</span><span class="sxs-lookup"><span data-stu-id="8ce80-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="8ce80-109">Ze względu na fundamentalne zmiany w programie EF Core nie jest zalecane Próba przeniesienia aplikacji EF6 do programu EF Core, chyba że istnieje istotny powód, aby wprowadzić zmianę.</span><span class="sxs-lookup"><span data-stu-id="8ce80-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="8ce80-110">Jeśli chcesz przejść do programu EF Core, aby skorzystać z nowych funkcji, a następnie upewnij się, że możesz świadomość jej ograniczenia przed rozpoczęciem.</span><span class="sxs-lookup"><span data-stu-id="8ce80-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="8ce80-111">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli programu EF Core może być dobrym wyborem dla twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ce80-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="8ce80-112">**Należy wyświetlić przenoszenie z programu EF6 do programu EF Core jako portu, a nie uaktualnienie.**</span><span class="sxs-lookup"><span data-stu-id="8ce80-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="8ce80-113">Zobacz [przenoszenie z programu EF6 do programu EF Core](porting/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8ce80-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
