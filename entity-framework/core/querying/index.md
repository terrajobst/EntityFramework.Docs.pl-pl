---
title: Wykonywanie zapytania o dane — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993537"
---
# <a name="querying-data"></a><span data-ttu-id="6b6fd-102">Wykonanie zapytania o dane</span><span class="sxs-lookup"><span data-stu-id="6b6fd-102">Querying Data</span></span>

<span data-ttu-id="6b6fd-103">Entity Framework Core używa Language Integrated Query (LINQ) do zapytania o dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b6fd-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="6b6fd-104">LINQ umożliwia przy użyciu języka C# (lub wybranym języku .NET) do pisania zapytań silnie typizowaną oparte na klasach pochodnych kontekstu i jednostki.</span><span class="sxs-lookup"><span data-stu-id="6b6fd-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="6b6fd-105">Reprezentacja zapytania LINQ są przekazywane do dostawcy bazy danych, aby być przetłumaczony na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="6b6fd-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="6b6fd-106">Aby uzyskać bardziej szczegółowe informacje dotyczące sposobu przetwarzania zapytania, zobacz [jak działa zapytanie](overview.md).</span><span class="sxs-lookup"><span data-stu-id="6b6fd-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
