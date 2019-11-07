---
title: Usługi czasu projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655822"
---
# <a name="design-time-services"></a><span data-ttu-id="6089f-102">Usługi czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="6089f-102">Design-time services</span></span>

<span data-ttu-id="6089f-103">Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="6089f-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="6089f-104">Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6089f-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="6089f-105">Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.</span><span class="sxs-lookup"><span data-stu-id="6089f-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
