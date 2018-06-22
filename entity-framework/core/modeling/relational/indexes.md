---
title: Indeksy — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29679002"
---
# <a name="indexes"></a><span data-ttu-id="bd3fd-102">Indeksy</span><span class="sxs-lookup"><span data-stu-id="bd3fd-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="bd3fd-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="bd3fd-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="bd3fd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="bd3fd-105">Mapuje indeksu relacyjnej bazy danych do tego samego pojęcia jako indeks w podstawowe programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="bd3fd-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="bd3fd-106">Conventions</span></span>

<span data-ttu-id="bd3fd-107">Konwencja, indeksy są nazywane `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="bd3fd-108">Złożone indeksów `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bd3fd-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="bd3fd-109">Data Annotations</span></span>

<span data-ttu-id="bd3fd-110">Indeksy nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="bd3fd-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="bd3fd-111">Fluent API</span></span>

<span data-ttu-id="bd3fd-112">Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="bd3fd-113">Można również określić filtr.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="bd3fd-114">Przy użyciu dostawcy programu SQL Server EF dodaje "IS NOT NULL" Filtruj wszystkie kolumny wartości null, które są częścią unikatowego indeksu.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="bd3fd-115">Aby zastąpić tę Konwencję, możesz podać `null` wartość.</span><span class="sxs-lookup"><span data-stu-id="bd3fd-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
