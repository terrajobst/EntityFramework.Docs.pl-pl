---
title: Wykonywanie zapytania na danych - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054215"
---
# <a name="querying-data"></a><span data-ttu-id="50bcf-102">Wykonywanie zapytania na danych</span><span class="sxs-lookup"><span data-stu-id="50bcf-102">Querying Data</span></span>

<span data-ttu-id="50bcf-103">Entity Framework Core używa języka integracji zapytania (LINQ) wykonać zapytania o dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="50bcf-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="50bcf-104">LINQ umożliwia przy użyciu języka C# (lub z języka .NET) do zapisania jednoznacznie zapytań opartych na klas pochodnych kontekstu i jednostki.</span><span class="sxs-lookup"><span data-stu-id="50bcf-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="50bcf-105">Reprezentacja zapytania LINQ są przekazywane do dostawcy bazy danych, aby być przetłumaczony na język kwerendy specyficzne dla bazy danych (np. SQL relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="50bcf-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="50bcf-106">Aby uzyskać bardziej szczegółowe informacje dotyczące sposobu przetwarzania zapytania, zobacz [jak działa zapytanie](overview.md).</span><span class="sxs-lookup"><span data-stu-id="50bcf-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
