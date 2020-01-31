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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Używanie EF Core i EF6 w tej samej aplikacji

Możliwe jest używanie EF Core i EF6 w tej samej aplikacji lub bibliotece przez zainstalowanie obu pakietów NuGet.

Niektóre typy mają takie same nazwy w EF Core i EF6 i różnią się tylko przestrzenią nazw, co może spowodować skomplikowanie przy użyciu obu EF Core i EF6 w tym samym pliku kodu. Niejednoznaczność można łatwo usunąć przy użyciu dyrektyw aliasu przestrzeni nazw. Na przykład:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

W przypadku przenoszenia istniejącej aplikacji, która ma wiele modeli EF, można selektywnie przenieść niektóre z nich do EF Core i nadal używać EF6 dla innych osób.
