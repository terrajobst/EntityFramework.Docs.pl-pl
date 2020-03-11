---
title: Wykonywanie zapytania dotyczącego danych — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417688"
---
# <a name="querying-data"></a>Wykonanie zapytania o dane

Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych. LINQ umożliwia pisanie kwerend silnie wpisanych przy użyciu C# (lub języka .NET). Używa ona kontekstów pochodnych i klas jednostek do odwoływania się do obiektów bazy danych. EF Core przekazuje reprezentację zapytania LINQ do dostawcy bazy danych. Dostawcy bazy danych z kolei przekładają ją na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).

> [!TIP]
> [Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.

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
