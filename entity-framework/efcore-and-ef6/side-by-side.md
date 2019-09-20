---
title: EF6 i EF Core — używanie ich w tej samej aplikacji
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149299"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Używanie EF Core i EF6 w tej samej aplikacji

Możliwe jest używanie EF Core i EF6 w tej samej aplikacji lub bibliotece przez zainstalowanie obu pakietów NuGet.

Niektóre typy mają takie same nazwy w EF Core i EF6 i różnią się tylko przestrzenią nazw, co może spowodować skomplikowanie przy użyciu obu EF Core i EF6 w tym samym pliku kodu. Niejednoznaczność można łatwo usunąć przy użyciu dyrektyw aliasu przestrzeni nazw. Na przykład:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

W przypadku przenoszenia istniejącej aplikacji, która ma wiele modeli EF, można selektywnie przenieść niektóre z nich do EF Core i nadal używać EF6 dla innych osób.
