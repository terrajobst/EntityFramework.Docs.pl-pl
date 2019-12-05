---
title: Indeksy (relacyjna baza danych) — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824578"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="36c12-102">Indeksy (relacyjna baza danych)</span><span class="sxs-lookup"><span data-stu-id="36c12-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="36c12-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="36c12-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="36c12-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="36c12-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="36c12-105">Indeks w relacyjnej bazie danych jest mapowany na takie same koncepcje jak indeks w rdzeńu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="36c12-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="36c12-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="36c12-106">Conventions</span></span>

<span data-ttu-id="36c12-107">Według Konwencji, indeksy mają nazwę `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="36c12-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="36c12-108">W przypadku indeksów złożonych `<property name>` być rozdzielaną podkreśleniem listę nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="36c12-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="36c12-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="36c12-109">Data Annotations</span></span>

<span data-ttu-id="36c12-110">Nie można skonfigurować indeksów przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="36c12-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="36c12-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="36c12-111">Fluent API</span></span>

<span data-ttu-id="36c12-112">Za pomocą interfejsu API Fluent można skonfigurować nazwę indeksu.</span><span class="sxs-lookup"><span data-stu-id="36c12-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="36c12-113">Można również określić filtr.</span><span class="sxs-lookup"><span data-stu-id="36c12-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="36c12-114">W przypadku używania dostawcy SQL Server Dr dodaje filtr `'IS NOT NULL'` dla wszystkich kolumn dopuszczających wartości null, które są częścią unikatowego indeksu.</span><span class="sxs-lookup"><span data-stu-id="36c12-114">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="36c12-115">Aby zastąpić tę Konwencję, możesz podać wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="36c12-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="36c12-116">Uwzględnij kolumny w indeksach SQL Server</span><span class="sxs-lookup"><span data-stu-id="36c12-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="36c12-117">Istnieje możliwość skonfigurowania [indeksów z uwzględnieniem kolumn](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , aby znacznie poprawić wydajność zapytań, gdy wszystkie kolumny w zapytaniu są uwzględniane w indeksie jako kolumna klucza lub niebędąca kolumną klucza.</span><span class="sxs-lookup"><span data-stu-id="36c12-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
