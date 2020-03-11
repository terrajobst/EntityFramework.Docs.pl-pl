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
# <a name="querying-data"></a><span data-ttu-id="14575-102">Wykonanie zapytania o dane</span><span class="sxs-lookup"><span data-stu-id="14575-102">Querying Data</span></span>

<span data-ttu-id="14575-103">Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14575-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="14575-104">LINQ umożliwia pisanie kwerend silnie wpisanych przy użyciu C# (lub języka .NET).</span><span class="sxs-lookup"><span data-stu-id="14575-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="14575-105">Używa ona kontekstów pochodnych i klas jednostek do odwoływania się do obiektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14575-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="14575-106">EF Core przekazuje reprezentację zapytania LINQ do dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14575-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="14575-107">Dostawcy bazy danych z kolei przekładają ją na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="14575-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="14575-108">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="14575-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="14575-109">Poniższe fragmenty kodu zawierają kilka przykładów, w których można wykonać typowe zadania z Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="14575-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="14575-110">Ładowanie wszystkich danych</span><span class="sxs-lookup"><span data-stu-id="14575-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="14575-111">Ładowanie pojedynczej jednostki</span><span class="sxs-lookup"><span data-stu-id="14575-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="14575-112">Filtrowanie</span><span class="sxs-lookup"><span data-stu-id="14575-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="14575-113">Dalsze odczyty</span><span class="sxs-lookup"><span data-stu-id="14575-113">Further readings</span></span>

- <span data-ttu-id="14575-114">Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="14575-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="14575-115">Aby uzyskać bardziej szczegółowe informacje na temat sposobu przetwarzania zapytania w EF Core, zobacz [jak działa zapytanie](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="14575-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
