---
title: Wyszukiwanie danych — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417688"
---
# <a name="querying-data"></a>Wykonanie zapytania o dane

Entity Framework Core używa języka zintegrowane zapytanie (LINQ) do kwerendy danych z bazy danych. LINQ umożliwia używanie języka C# (lub wybranego języka .NET) do pisania silnie wpisanych zapytań. Używa pochodnego kontekstu i klasy jednostek do odwoływania się do obiektów bazy danych. EF Core przekazuje reprezentację kwerendy LINQ do dostawcy bazy danych. Dostawcy bazy danych z kolei tłumaczą go na język kwerend specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).

> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.

Poniższe fragmenty kodu pokazują kilka przykładów, jak osiągnąć typowe zadania za pomocą programu Entity Framework Core.

## <a name="loading-all-data"></a>Ładowanie wszystkich danych

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Ładowanie pojedynczej jednostki

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtrowanie

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Dalsze odczyty

- Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Aby uzyskać bardziej szczegółowe informacje na temat sposobu przetwarzania kwerendy w ef core, zobacz [Jak działa kwerenda](xref:core/querying/how-query-works).
