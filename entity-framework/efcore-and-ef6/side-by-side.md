---
title: EF6 i podstawowe EF - ich użycie w tej samej aplikacji
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054212"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="1b350-102">Przy użyciu EF Core i EF6 w tej samej aplikacji</span><span class="sxs-lookup"><span data-stu-id="1b350-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="1b350-103">Istnieje możliwość używać EF Core i EF6 w tej samej aplikacji .NET Framework lub biblioteka instalując oba pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="1b350-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="1b350-104">Niektóre typy o takich samych nazwach EF Core i EF6 i różnią się tylko według przestrzeni nazw, które mogą skomplikować, używając EF Core i EF6 w tym samym pliku kodu.</span><span class="sxs-lookup"><span data-stu-id="1b350-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="1b350-105">Niejednoznaczności można łatwo usunąć przy użyciu dyrektyw aliasu przestrzeni nazw, np.:</span><span class="sxs-lookup"><span data-stu-id="1b350-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="1b350-106">Są eksportowanie istniejącej aplikacji, która ma wiele EF modeli, można selektywnie portu niektóre z nich na rdzeń EF, i kontynuować używanie EF6 dla innych.</span><span class="sxs-lookup"><span data-stu-id="1b350-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
