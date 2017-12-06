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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="99282-102">EF Core i EF6: które z nich jest odpowiedni</span><span class="sxs-lookup"><span data-stu-id="99282-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="99282-103">Poniższe informacje pomogą wybór między Entity Framework Core i Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="99282-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="99282-104">Wskazówki dotyczące nowych aplikacji</span><span class="sxs-lookup"><span data-stu-id="99282-104">Guidance for new applications</span></span>

<span data-ttu-id="99282-105">Ponieważ EF Core jest nowym produktem i nadal brakuje niektórych funkcji O/RM o krytycznym znaczeniu, EF6 będą nadal wybór najbardziej odpowiedniej dla wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99282-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="99282-106">**Są to typy aplikacji, których firma Microsoft zaleca, przy użyciu EF Core dla:**</span><span class="sxs-lookup"><span data-stu-id="99282-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="99282-107">Nowe aplikacje, które nie wymagają funkcji, które nie zostały jeszcze zaimplementowane w EF Core.</span><span class="sxs-lookup"><span data-stu-id="99282-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="99282-108">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99282-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="99282-109">Aplikacji przeznaczonych dla platformy .NET Core, takich jak aplikacje systemu Windows platformy Uniwersalnej i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99282-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="99282-110">Te aplikacje nie można użyć EF6, ponieważ wymaga programu .NET Framework (np. .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="99282-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="99282-111">Wskazówki dotyczące istniejących aplikacji EF6</span><span class="sxs-lookup"><span data-stu-id="99282-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="99282-112">Z powodu zmiany podstawowych rdzeni EF nie zaleca się próby przenieść aplikację EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="99282-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="99282-113">Jeśli chcesz przejść do EF Core, aby korzystać z nowych funkcji, a następnie upewnij się, że są znane swoje ograniczenia, przed rozpoczęciem.</span><span class="sxs-lookup"><span data-stu-id="99282-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="99282-114">Przegląd [porównanie funkcji](features.md) aby zobaczyć, jeśli podstawowe EF może być dobrym wyborem dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99282-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="99282-115">**Należy wyświetlić przejście od EF6 na rdzeń EF jako portu, a nie uaktualnienie.**</span><span class="sxs-lookup"><span data-stu-id="99282-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="99282-116">Zobacz [eksportowanie z EF6 do EF Core](porting/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="99282-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
