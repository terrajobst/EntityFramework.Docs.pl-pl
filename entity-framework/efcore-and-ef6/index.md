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
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="494e2-102">Porównanie programów EF Core i EF6</span><span class="sxs-lookup"><span data-stu-id="494e2-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="494e2-103">Istnieją dwie różne wersje programu Entity Framework, platformy Entity Framework Core i Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="494e2-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="494e2-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="494e2-104">Entity Framework 6</span></span>

<span data-ttu-id="494e2-105">Entity Framework 6 (EF6) jest danych i przetestowanej technologii dostępu do wielu lat pracy nad funkcjami i stabilizacją.</span><span class="sxs-lookup"><span data-stu-id="494e2-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="494e2-106">Je po raz pierwszy w 2008 roku jako część .NET Framework 3.5 SP1 i Visual Studio 2008 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="494e2-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="494e2-107">Począwszy od wersji EF4.1 jest dostarczana jako [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/) — obecnie jedną z najbardziej popularnych pakietów w witrynie NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="494e2-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="494e2-108">Programy EF6 w dalszym ciągu być obsługiwane produktu i będą nadal widzieć poprawki błędów i drobne ulepszenia przez pewien czas na osiągnięcie.</span><span class="sxs-lookup"><span data-stu-id="494e2-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="494e2-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="494e2-109">Entity Framework Core</span></span>

<span data-ttu-id="494e2-110">Entity Framework Core (EF Core) to lekki, rozszerzalne i dla wielu platform wersją programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="494e2-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="494e2-111">EF Core wprowadzono wiele ulepszeń i nowych funkcji w porównaniu z platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="494e2-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="494e2-112">W tym samym czasie EF Core jest nowy kod podstawowy i nie jako dojrzała jako EF6.</span><span class="sxs-lookup"><span data-stu-id="494e2-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="494e2-113">EF Core zapewnia środowisko programistyczne z programu EF6 i najczęściej APIs najwyższego poziomu pozostają bez zmian, dzięki współdziałaniu bardzo podobnie do osób, które były używane platformy EF6 programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="494e2-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="494e2-114">W tym samym czasie programu EF Core jest wbudowana w całkowicie nowy zestaw podstawowych składników.</span><span class="sxs-lookup"><span data-stu-id="494e2-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="494e2-115">Oznacza to, że programu EF Core z programu EF6 nie dziedziczy automatycznie wszystkich funkcji.</span><span class="sxs-lookup"><span data-stu-id="494e2-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="494e2-116">Podczas gdy niektóre z tych funkcji będzie wyświetlany w przyszłych wersjach, inne rzadziej używane funkcje nie będzie zaimplementowana w wersji EF Core.</span><span class="sxs-lookup"><span data-stu-id="494e2-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="494e2-117">Core nowe, rozszerzone i uproszczone ma także umożliwiło nam dodać niektóre funkcje do programu EF Core, który nie będzie zaimplementowana w EF6 (np. klucze alternatywne, aktualizacje usługi batch i oceny mieszane/bazy danych klienta w zapytaniach LINQ).</span><span class="sxs-lookup"><span data-stu-id="494e2-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
