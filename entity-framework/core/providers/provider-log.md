---
title: Dziennik zmian wpływających na dostawcy — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998365"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="abf6b-102">Zmiany wpływające na dostawcy</span><span class="sxs-lookup"><span data-stu-id="abf6b-102">Provider-impacting changes</span></span>

<span data-ttu-id="abf6b-103">Ta strona zawiera linki do przeprowadzanych na repozytorium programu EF Core, które mogą wymagać autorzy innych dostawców bazy danych, aby reagować żądań ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="abf6b-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="abf6b-104">Jest do udostępniania punktu wyjściowego dla autorów istniejącego dostawcy baz danych innych firm, podczas aktualizowania ich dostawcy do nowej wersji.</span><span class="sxs-lookup"><span data-stu-id="abf6b-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="abf6b-105">Rozpoczynamy ten dziennik zmian z 2.1 do wersji 2.2.</span><span class="sxs-lookup"><span data-stu-id="abf6b-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="abf6b-106">Przed 2.1 użyliśmy [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiety na nasze problemy i żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="abf6b-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="abf6b-107">2.1---> 2.2</span><span class="sxs-lookup"><span data-stu-id="abf6b-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="abf6b-108">Zmiany tylko do testów</span><span class="sxs-lookup"><span data-stu-id="abf6b-108">Test-only changes</span></span>

* <span data-ttu-id="abf6b-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Zezwalaj na możliwe do dostosowania ograniczników SQL w testach</span><span class="sxs-lookup"><span data-stu-id="abf6b-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="abf6b-110">Testowanie zmiany, które umożliwiają nieścisłym porównania punktu zmiennoprzecinkowego w BuiltInDataTypesTestBase</span><span class="sxs-lookup"><span data-stu-id="abf6b-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="abf6b-111">Zmiany testów, które umożliwiają zapytania testy, aby być ponownie używane, za pomocą różnych ograniczników SQL</span><span class="sxs-lookup"><span data-stu-id="abf6b-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="abf6b-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Dodaj testy DbFunction testów specyfikacji relacyjnych</span><span class="sxs-lookup"><span data-stu-id="abf6b-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="abf6b-113">Taki sposób, że te testy mogą być uruchamiane względem wszystkich dostawców bazy danych</span><span class="sxs-lookup"><span data-stu-id="abf6b-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="abf6b-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Czyszczenie testowego przełączania do Async</span><span class="sxs-lookup"><span data-stu-id="abf6b-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="abf6b-115">Usuń `Wait` wywołań, niepotrzebne async i zmienić nazwy niektórych metod testowych</span><span class="sxs-lookup"><span data-stu-id="abf6b-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="abf6b-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Ujednolicenie infrastrukturę testowania rejestrowania</span><span class="sxs-lookup"><span data-stu-id="abf6b-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="abf6b-117">Dodano `CreateListLoggerFactory` i usunąć niektóre poprzedniego infrastruktury rejestrowania, który będzie wymagać dostawców przy użyciu tych testów, reakcji</span><span class="sxs-lookup"><span data-stu-id="abf6b-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="abf6b-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 — Uruchamianie więcej testów zapytania synchronicznie i asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="abf6b-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="abf6b-119">Nazwy testów i uwzględniając uległ zmianie, co będzie wymagać dostawców przy użyciu tych testów, reakcji</span><span class="sxs-lookup"><span data-stu-id="abf6b-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="abf6b-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Zmiana nazwy tego modelu ComplexNavigations</span><span class="sxs-lookup"><span data-stu-id="abf6b-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="abf6b-121">Za pomocą tych testów dostawców może być konieczne reagowanie</span><span class="sxs-lookup"><span data-stu-id="abf6b-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="abf6b-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Zwracania kontekstu danej puli zamiast usuwania w testy funkcjonalne</span><span class="sxs-lookup"><span data-stu-id="abf6b-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="abf6b-123">Ta zmiana obejmuje niektóre refaktoryzacji testów, które mogą wymagać dostawców reagować</span><span class="sxs-lookup"><span data-stu-id="abf6b-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="abf6b-124">Test i produktu zmian w kodzie</span><span class="sxs-lookup"><span data-stu-id="abf6b-124">Test and product code changes</span></span>

* <span data-ttu-id="abf6b-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Skonsolidować RelationalTypeMapping.Clone metody</span><span class="sxs-lookup"><span data-stu-id="abf6b-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="abf6b-126">Zmiany w 2.1 RelationalTypeMapping dozwolone dla uproszczenia w klasach pochodnych.</span><span class="sxs-lookup"><span data-stu-id="abf6b-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="abf6b-127">Firma Microsoft uważa, nie zostało to istotne do dostawców, ale dostawców korzystać z zalet tej zmiany w ich typ pochodny mapowania klas.</span><span class="sxs-lookup"><span data-stu-id="abf6b-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="abf6b-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Oznakowane lub nazwanego zapytania</span><span class="sxs-lookup"><span data-stu-id="abf6b-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="abf6b-129">Dodaje infrastrukturę na potrzeby znakowania zapytań LINQ i posiadanie tych tagów, są wyświetlane jako komentarze w SQL.</span><span class="sxs-lookup"><span data-stu-id="abf6b-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="abf6b-130">Może to wymagać dostawców reagowania generowanie kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="abf6b-130">This may require providers to react in SQL generation.</span></span>
