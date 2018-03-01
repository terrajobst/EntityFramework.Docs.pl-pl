---
title: "Dostawca bazy danych MyCat pomelo — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="91c9a-102">Dostawca pomelo MyCat EF podstawowej bazy danych</span><span class="sxs-lookup"><span data-stu-id="91c9a-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="91c9a-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z [MyCat](https://github.com/MyCATApache/Mycat-Server).</span><span class="sxs-lookup"><span data-stu-id="91c9a-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="91c9a-104">Dostawca jest przechowywany jako część [projektu Foundation Pomelo](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span><span class="sxs-lookup"><span data-stu-id="91c9a-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="91c9a-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="91c9a-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="91c9a-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="91c9a-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="91c9a-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="91c9a-107">Install</span></span>

<span data-ttu-id="91c9a-108">Pobierz i uruchom [najnowszej wersji z witryny projektu](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span><span class="sxs-lookup"><span data-stu-id="91c9a-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="91c9a-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="91c9a-109">Get Started</span></span>

<span data-ttu-id="91c9a-110">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="91c9a-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="91c9a-111">Kroki konfiguracji</span><span class="sxs-lookup"><span data-stu-id="91c9a-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="91c9a-112">Wprowadzenie wideo</span><span class="sxs-lookup"><span data-stu-id="91c9a-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="91c9a-113">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="91c9a-113">Supported Database Engines</span></span>

* <span data-ttu-id="91c9a-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="91c9a-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="91c9a-115">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="91c9a-115">Supported Platforms</span></span>

* <span data-ttu-id="91c9a-116">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="91c9a-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="91c9a-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="91c9a-117">.NET Core</span></span>
* <span data-ttu-id="91c9a-118">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="91c9a-118">Mono (4.2.0 onwards)</span></span>
