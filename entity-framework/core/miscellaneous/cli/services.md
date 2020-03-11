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
# <a name="design-time-services"></a>Usługi czasu projektowania

Niektóre usługi używane przez narzędzia są używane tylko w czasie projektowania. Te usługi są zarządzane oddzielnie od usług środowiska uruchomieniowego EF Core, aby uniemożliwić ich wdrożenie w aplikacji. Aby zastąpić jedną z tych usług (na przykład usługę do generowania plików migracji), Dodaj implementację `IDesignTimeServices` do projektu startowego.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
