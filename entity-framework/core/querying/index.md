---
title: Wykonywanie zapytania dotyczącego danych — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181918"
---
# <a name="querying-data"></a>Wykonanie zapytania o dane

Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych. LINQ umożliwia pisanie kwerend silnie wpisanych przy użyciu C# (lub języka .NET). Używa ona kontekstów pochodnych i klas jednostek do odwoływania się do obiektów bazy danych. EF Core przekazuje reprezentację zapytania LINQ do dostawcy bazy danych. Dostawcy bazy danych z kolei przekładają ją na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

Poniższe fragmenty kodu zawierają kilka przykładów, w których można wykonać typowe zadania z Entity Framework Core.

## <a name="loading-all-data"></a>Ładowanie wszystkich danych

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Ładowanie pojedynczej jednostki

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtrowanie

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Dalsze odczyty

- Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Aby uzyskać bardziej szczegółowe informacje na temat sposobu przetwarzania zapytania w EF Core, zobacz [jak działa zapytanie](xref:core/querying/how-query-works).
