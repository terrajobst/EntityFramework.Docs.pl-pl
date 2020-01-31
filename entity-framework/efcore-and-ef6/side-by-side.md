---
title: EF6 i EF Core — używanie ich w tej samej aplikacji
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888138"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="dbeaf-102">Używanie EF Core i EF6 w tej samej aplikacji</span><span class="sxs-lookup"><span data-stu-id="dbeaf-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="dbeaf-103">Możliwe jest używanie EF Core i EF6 w tej samej aplikacji lub bibliotece przez zainstalowanie obu pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="dbeaf-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="dbeaf-104">Niektóre typy mają takie same nazwy w EF Core i EF6 i różnią się tylko przestrzenią nazw, co może spowodować skomplikowanie przy użyciu obu EF Core i EF6 w tym samym pliku kodu.</span><span class="sxs-lookup"><span data-stu-id="dbeaf-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="dbeaf-105">Niejednoznaczność można łatwo usunąć przy użyciu dyrektyw aliasu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="dbeaf-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="dbeaf-106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dbeaf-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="dbeaf-107">W przypadku przenoszenia istniejącej aplikacji, która ma wiele modeli EF, można selektywnie przenieść niektóre z nich do EF Core i nadal używać EF6 dla innych osób.</span><span class="sxs-lookup"><span data-stu-id="dbeaf-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
