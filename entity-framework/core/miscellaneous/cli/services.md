---
title: Usługi czasu projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811937"
---
# <a name="design-time-services"></a><span data-ttu-id="dc901-102">Usługi czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="dc901-102">Design-time services</span></span>

<span data-ttu-id="dc901-103">Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="dc901-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="dc901-104">Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dc901-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="dc901-105">Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.</span><span class="sxs-lookup"><span data-stu-id="dc901-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
