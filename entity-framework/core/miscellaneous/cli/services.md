---
title: Usługi czasu projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416729"
---
# <a name="design-time-services"></a><span data-ttu-id="b2069-102">Usługi czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="b2069-102">Design-time services</span></span>

<span data-ttu-id="b2069-103">Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="b2069-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="b2069-104">Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2069-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="b2069-105">Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.</span><span class="sxs-lookup"><span data-stu-id="b2069-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
