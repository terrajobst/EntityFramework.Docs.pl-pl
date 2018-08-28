---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993220"
---
# <a name="indexes"></a><span data-ttu-id="17170-102">Indeksy</span><span class="sxs-lookup"><span data-stu-id="17170-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="17170-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="17170-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="17170-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="17170-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="17170-105">Indeks w relacyjnej bazie danych jest mapowany na tę samą koncepcję jako indeks w obszarach podstawowych programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="17170-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="17170-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="17170-106">Conventions</span></span>

<span data-ttu-id="17170-107">Zgodnie z Konwencją indeksy są nazywane `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="17170-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="17170-108">Dla indeksów złożonych `<property name>` staje się listę nazw właściwości oddzielonych znakiem podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="17170-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="17170-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="17170-109">Data Annotations</span></span>

<span data-ttu-id="17170-110">Indeksy nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="17170-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="17170-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="17170-111">Fluent API</span></span>

<span data-ttu-id="17170-112">Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="17170-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="17170-113">Można również określić filtr.</span><span class="sxs-lookup"><span data-stu-id="17170-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="17170-114">Po dodaniu przy użyciu dostawcy programu SQL Server EF 'IS NOT NULL' filtrować wszystkie kolumny dopuszczające wartość null, które są częścią unikatowego indeksu.</span><span class="sxs-lookup"><span data-stu-id="17170-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="17170-115">Aby zastąpić tę Konwencję, możesz podać `null` wartość.</span><span class="sxs-lookup"><span data-stu-id="17170-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
