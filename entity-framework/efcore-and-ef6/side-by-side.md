---
title: Programy EF6 i EF Core — ich użycie w ramach jednej aplikacji
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995712"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="e10a0-102">Przy użyciu programu EF Core i EF6 w tej samej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e10a0-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="e10a0-103">Istnieje możliwość użycia programu EF Core i EF6 w tej samej aplikacji .NET Framework lub biblioteki, instalując oba pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="e10a0-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="e10a0-104">Niektóre typy takich samych nazwach w programu EF Core i EF6 i różnią się jedynie przestrzeni nazw, które mogą skomplikować przy użyciu programu EF Core i EF6, w tym samym pliku kodu.</span><span class="sxs-lookup"><span data-stu-id="e10a0-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="e10a0-105">Niejednoznaczności można łatwo usunąć za pomocą dyrektywy aliasu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e10a0-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="e10a0-106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e10a0-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="e10a0-107">Jeśli są Przenoszenie istniejących aplikacji, która ma wiele modeli EF, można selektywnie portu niektóre z nich do programu EF Core i kontynuować korzystanie z platformy EF6 dla innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e10a0-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
