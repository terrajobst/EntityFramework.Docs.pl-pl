---
title: "Detached.EntityFramework — narzędzia i rozszerzenia — EF Core"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: DE1C3FEA-F618-4C11-932F-7C392A1B2C94
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-detached
ms.openlocfilehash: 93937eee07a9e1a0b94bd92140faec7d1753332f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="efdetachedentityframework-extension"></a><span data-ttu-id="a903f-102">Rozszerzenie EFDetached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="a903f-102">EFDetached.EntityFramework Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="a903f-103">To rozszerzenie nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a903f-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a903f-104">Rozważając rozszerzenia innych firm, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="a903f-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="a903f-105">Załadowanie i zapisanie wykresy całego odłączyć jednostek (jednostka z ich obiektów podrzędnych i list).</span><span class="sxs-lookup"><span data-stu-id="a903f-105">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="a903f-106">Inspirowana przez [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="a903f-106">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="a903f-107">Ma to również dodać niektóre dodatki plug-in do simplificate powtarzających się zadań, takich jak przeprowadzanie inspekcji i podział na strony.</span><span class="sxs-lookup"><span data-stu-id="a903f-107">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

<span data-ttu-id="a903f-108">Następujące zasoby zawierają informacje pomocne podczas wprowadzenie Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="a903f-108">The following resource will help you get started with Detached.EntityFramework</span></span>
* [<span data-ttu-id="a903f-109">Repozytorium Detached.EntityFramework GitHub</span><span class="sxs-lookup"><span data-stu-id="a903f-109">Detached.EntityFramework GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)
