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
# <a name="design-time-services"></a>Usługi czasu projektowania

Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania. Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji. Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
