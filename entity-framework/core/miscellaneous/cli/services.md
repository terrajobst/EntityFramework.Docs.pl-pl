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
# <a name="design-time-services"></a>Usługi czasu projektowania

Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania. Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji. Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
