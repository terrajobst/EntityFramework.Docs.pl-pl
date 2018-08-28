---
title: Usługi czasu projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997534"
---
<a name="design-time-services"></a><span data-ttu-id="c171c-102">Usługi w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="c171c-102">Design-time services</span></span>
====================
<span data-ttu-id="c171c-103">Niektóre usługi używana przez narzędzia są używane tylko w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="c171c-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="c171c-104">Te usługi są zarządzane oddzielnie od usługi czasu wykonywania programu EF Core, aby uniemożliwić im wdrażana razem z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="c171c-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="c171c-105">Aby zastąpić jedną z tych usług (na przykład usługa ma generować pliki migracji), Dodaj implementację `IDesignTimeServices` na Twój projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="c171c-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
