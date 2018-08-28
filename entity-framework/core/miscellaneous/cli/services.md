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
<a name="design-time-services"></a>Usługi w czasie projektowania
====================
Niektóre usługi używana przez narzędzia są używane tylko w czasie projektowania. Te usługi są zarządzane oddzielnie od usługi czasu wykonywania programu EF Core, aby uniemożliwić im wdrażana razem z aplikacją. Aby zastąpić jedną z tych usług (na przykład usługa ma generować pliki migracji), Dodaj implementację `IDesignTimeServices` na Twój projekt startowy.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
