---
title: Usługi czasu projektowania — podstawowe EF
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054608"
---
<a name="design-time-services"></a>Usługi w czasie projektowania
====================
Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania. Te usługi są zarządzane oddzielnie od usługi czasu wykonywania EF Core, aby zapobiec wdrażany z aplikacją. Aby zastąpić jedną z tych usług (na przykład usługa generuje pliki migracji), Dodaj implementację `IDesignTimeServices` na Twój projekt startowy.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
