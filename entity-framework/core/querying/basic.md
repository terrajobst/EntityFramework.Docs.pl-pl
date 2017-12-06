---
title: "Podstawowe zapytania — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a>Zapytania podstawowego

Dowiedz się, jak załadować jednostek z bazy danych przy użyciu języka integracji zapytania (LINQ).

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="101-linq-samples"></a>101 przykłady interfejsów LINQ

Ta strona zawiera kilka przykładów w celu osiągnięcia typowe zadania związane z programu Entity Framework Core. Aby rozbudowany zestaw przykładów przedstawiający, co jest możliwe za pomocą LINQ, zobacz [101 przykłady interfejsów LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Ładowania wszystkich danych

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Ładowanie pojedynczej jednostki

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
