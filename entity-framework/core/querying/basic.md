---
title: Podstawowe zapytania — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: eceac81546b23157611edd530b8b71f71e970c1f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949065"
---
# <a name="basic-queries"></a>Podstawowe zapytania

Dowiedz się, jak można załadować jednostek z bazy danych przy użyciu Language Integrated Query (LINQ).

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="101-linq-samples"></a>101 przykładów LINQ

Ta strona zawiera kilka przykładów, aby osiągnąć typowych zadań przy użyciu platformy Entity Framework Core. Aby uzyskać obszerny zestaw przykładów pokazujący, co jest możliwe za pomocą LINQ, zobacz [101 przykładów LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Podczas ładowania wszystkie dane

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Trwa ładowanie pojedynczej jednostki

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Filtrowanie

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
