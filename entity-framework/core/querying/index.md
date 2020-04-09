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
# <a name="querying-data"></a><span data-ttu-id="ecb9e-102">Wykonanie zapytania o dane</span><span class="sxs-lookup"><span data-stu-id="ecb9e-102">Querying Data</span></span>

<span data-ttu-id="ecb9e-103">Entity Framework Core używa języka zintegrowane zapytanie (LINQ) do kwerendy danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="ecb9e-104">LINQ umożliwia używanie języka C# (lub wybranego języka .NET) do pisania silnie wpisanych zapytań.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="ecb9e-105">Używa pochodnego kontekstu i klasy jednostek do odwoływania się do obiektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="ecb9e-106">EF Core przekazuje reprezentację kwerendy LINQ do dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="ecb9e-107">Dostawcy bazy danych z kolei tłumaczą go na język kwerend specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="ecb9e-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="ecb9e-108">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="ecb9e-109">Poniższe fragmenty kodu pokazują kilka przykładów, jak osiągnąć typowe zadania za pomocą programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ecb9e-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="ecb9e-110">Ładowanie wszystkich danych</span><span class="sxs-lookup"><span data-stu-id="ecb9e-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="ecb9e-111">Ładowanie pojedynczej jednostki</span><span class="sxs-lookup"><span data-stu-id="ecb9e-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="ecb9e-112">Filtrowanie</span><span class="sxs-lookup"><span data-stu-id="ecb9e-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="ecb9e-113">Dalsze odczyty</span><span class="sxs-lookup"><span data-stu-id="ecb9e-113">Further readings</span></span>

- <span data-ttu-id="ecb9e-114">Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="ecb9e-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="ecb9e-115">Aby uzyskać bardziej szczegółowe informacje na temat sposobu przetwarzania kwerendy w ef core, zobacz [Jak działa kwerenda](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="ecb9e-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
