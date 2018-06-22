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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Przy użyciu EF Core i EF6 w tej samej aplikacji

Istnieje możliwość używać EF Core i EF6 w tej samej aplikacji .NET Framework lub biblioteka instalując oba pakiety NuGet. 

Niektóre typy o takich samych nazwach EF Core i EF6 i różnią się tylko według przestrzeni nazw, które mogą skomplikować, używając EF Core i EF6 w tym samym pliku kodu. Niejednoznaczności można łatwo usunąć przy użyciu dyrektyw aliasu przestrzeni nazw, np.:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Są eksportowanie istniejącej aplikacji, która ma wiele EF modeli, można selektywnie portu niektóre z nich na rdzeń EF, i kontynuować używanie EF6 dla innych.
