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
# <a name="querying-data"></a><span data-ttu-id="528bb-102">Wykonanie zapytania o dane</span><span class="sxs-lookup"><span data-stu-id="528bb-102">Querying Data</span></span>

<span data-ttu-id="528bb-103">Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528bb-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="528bb-104">LINQ umożliwia pisanie kwerend silnie wpisanych przy użyciu C# (lub języka .NET).</span><span class="sxs-lookup"><span data-stu-id="528bb-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="528bb-105">Używa ona kontekstów pochodnych i klas jednostek do odwoływania się do obiektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528bb-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="528bb-106">EF Core przekazuje reprezentację zapytania LINQ do dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528bb-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="528bb-107">Dostawcy bazy danych z kolei przekładają ją na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="528bb-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="528bb-108">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="528bb-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="528bb-109">Poniższe fragmenty kodu zawierają kilka przykładów, w których można wykonać typowe zadania z Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="528bb-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="528bb-110">Ładowanie wszystkich danych</span><span class="sxs-lookup"><span data-stu-id="528bb-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="528bb-111">Ładowanie pojedynczej jednostki</span><span class="sxs-lookup"><span data-stu-id="528bb-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="528bb-112">Filtrowanie</span><span class="sxs-lookup"><span data-stu-id="528bb-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="528bb-113">Dalsze odczyty</span><span class="sxs-lookup"><span data-stu-id="528bb-113">Further readings</span></span>

- <span data-ttu-id="528bb-114">Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="528bb-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="528bb-115">Aby uzyskać bardziej szczegółowe informacje na temat sposobu przetwarzania zapytania w EF Core, zobacz [jak działa zapytanie](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="528bb-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
