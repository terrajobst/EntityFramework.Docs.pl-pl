---
title: EF6 i EF Core — używanie ich w tej samej aplikacji
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419645"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Korzystanie z EF Core i EF6 w tej samej aplikacji

Istnieje możliwość użycia EF Core i EF6 w tej samej aplikacji lub bibliotece, instalując oba pakiety NuGet.

Niektóre typy mają takie same nazwy w EF Core i EF6 i różnią się tylko przez obszar nazw, co może skomplikować przy użyciu EF Core i EF6 w tym samym pliku kodu. Niejednoznaczność można łatwo usunąć za pomocą dyrektyw aliasów obszaru nazw. Przykład:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Jeśli przenosisz istniejącą aplikację, która ma wiele modeli EF, można wybrać selektywnie przenieść niektóre z nich do EF Core i kontynuować korzystanie z EF6 dla innych.
