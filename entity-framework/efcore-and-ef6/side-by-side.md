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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Przy użyciu programu EF Core i EF6 w tej samej aplikacji

Istnieje możliwość użycia programu EF Core i EF6 w tej samej aplikacji .NET Framework lub biblioteki, instalując oba pakiety NuGet.

Niektóre typy takich samych nazwach w programu EF Core i EF6 i różnią się jedynie przestrzeni nazw, które mogą skomplikować przy użyciu programu EF Core i EF6, w tym samym pliku kodu. Niejednoznaczności można łatwo usunąć za pomocą dyrektywy aliasu przestrzeni nazw. Na przykład:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Jeśli są Przenoszenie istniejących aplikacji, która ma wiele modeli EF, można selektywnie portu niektóre z nich do programu EF Core i kontynuować korzystanie z platformy EF6 dla innych użytkowników.
