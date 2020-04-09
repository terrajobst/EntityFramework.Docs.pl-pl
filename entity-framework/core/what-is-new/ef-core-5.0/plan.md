---
title: Plan dla core 5.0 struktury podmiotu
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136216"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="04d7d-102">Plan dla core 5.0 struktury podmiotu</span><span class="sxs-lookup"><span data-stu-id="04d7d-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="04d7d-103">Jak opisano w [procesie planowania,](../release-planning.md)zebraliśmy informacje od zainteresowanych stron na wstępny plan wydania EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="04d7d-104">Plan ten jest nadal w toku.</span><span class="sxs-lookup"><span data-stu-id="04d7d-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="04d7d-105">Nic tu nie jest zobowiązaniem.</span><span class="sxs-lookup"><span data-stu-id="04d7d-105">Nothing here is a commitment.</span></span> <span data-ttu-id="04d7d-106">Ten plan jest punktem wyjścia, który będzie ewoluował, gdy dowiemy się więcej.</span><span class="sxs-lookup"><span data-stu-id="04d7d-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="04d7d-107">Niektóre rzeczy, które nie są obecnie planowane na 5.0, mogą zostać wciągnięte.</span><span class="sxs-lookup"><span data-stu-id="04d7d-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="04d7d-108">Niektóre rzeczy obecnie planowane na 5.0 może dostać punted obecnie.</span><span class="sxs-lookup"><span data-stu-id="04d7d-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="04d7d-109">Numer wersji i data wydania.</span><span class="sxs-lookup"><span data-stu-id="04d7d-109">Version number and release date.</span></span>

<span data-ttu-id="04d7d-110">EF Core 5.0 jest obecnie planowane do wydania w [tym samym czasie co .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="04d7d-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="04d7d-111">Wersja "5.0" została wybrana tak, aby wyrównać z .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="04d7d-112">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="04d7d-112">Supported platforms</span></span>

<span data-ttu-id="04d7d-113">EF Core 5.0 ma działać na dowolnej platformie .NET 5.0 w oparciu o [zbieżność tych platform z platformą .NET Core.](https://devblogs.microsoft.com/dotnet/introducing-net-5/)</span><span class="sxs-lookup"><span data-stu-id="04d7d-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="04d7d-114">Co to oznacza w zakresie .NET Standard i rzeczywiste TFM używane jest nadal TBD.</span><span class="sxs-lookup"><span data-stu-id="04d7d-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="04d7d-115">EF Core 5.0 nie będzie działać w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="04d7d-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="04d7d-116">Fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="04d7d-116">Breaking changes</span></span>

<span data-ttu-id="04d7d-117">EF Core 5.0 będzie zawierał pewne przełomowe zmiany, ale będą one znacznie mniej dotkliwe niż miało to miejsce w przypadku EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="04d7d-118">Naszym celem jest umożliwienie większości aplikacji aktualizacji bez przerywania.</span><span class="sxs-lookup"><span data-stu-id="04d7d-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="04d7d-119">Oczekuje się, że będą pewne przełomowe zmiany dla dostawców baz danych, zwłaszcza wokół obsługi TPT.</span><span class="sxs-lookup"><span data-stu-id="04d7d-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="04d7d-120">Oczekujemy jednak, że praca aktualizacji dostawcy dla 5.0 będzie mniejsza niż była wymagana do aktualizacji dla 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="04d7d-121">Motywy</span><span class="sxs-lookup"><span data-stu-id="04d7d-121">Themes</span></span>

<span data-ttu-id="04d7d-122">Wydobyliśmy kilka głównych obszarów lub tematów, które będą stanowić podstawę dla dużych inwestycji w EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="04d7d-123">Wiele do wielu właściwości nawigacji (aka "pomiń nawigacji")</span><span class="sxs-lookup"><span data-stu-id="04d7d-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="04d7d-124">Główna @smitpatel programistka: i@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="04d7d-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="04d7d-125">Śledzone przez [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="04d7d-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="04d7d-126">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-126">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-127">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-127">Status: In-progress</span></span>

<span data-ttu-id="04d7d-128">Wiele do wielu jest [najbardziej żądaną funkcją](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 głosów) na zaległości GitHub.</span><span class="sxs-lookup"><span data-stu-id="04d7d-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="04d7d-129">Obsługa relacji wiele do wielu w całości jest śledzona jako [#10508.](https://github.com/aspnet/EntityFrameworkCore/issues/10508)</span><span class="sxs-lookup"><span data-stu-id="04d7d-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="04d7d-130">Można to podzielić na trzy główne obszary:</span><span class="sxs-lookup"><span data-stu-id="04d7d-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="04d7d-131">Pomiń właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="04d7d-131">Skip navigation properties.</span></span> <span data-ttu-id="04d7d-132">Umożliwiają one model do użycia dla zapytań, itp.</span><span class="sxs-lookup"><span data-stu-id="04d7d-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="04d7d-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="04d7d-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="04d7d-134">Typy jednostek worka właściwości.</span><span class="sxs-lookup"><span data-stu-id="04d7d-134">Property-bag entity types.</span></span> <span data-ttu-id="04d7d-135">Umożliwiają one standardowy typ CLR `Dictionary`(np. ) do użycia dla wystąpień jednostki, tak że jawny typ CLR nie jest potrzebny dla każdego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="04d7d-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="04d7d-136">(Rozciągnij na 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="04d7d-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="04d7d-137">Cukier do łatwej konfiguracji relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="04d7d-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="04d7d-138">(Rozciągnij na 5.0.)</span><span class="sxs-lookup"><span data-stu-id="04d7d-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="04d7d-139">Uważamy, że najbardziej znaczący bloker dla tych, którzy chcą wiele do wielu wsparcia nie jest w stanie korzystać z "naturalnych" relacji, bez odwoływania się do tabeli sprzężenia, w logice biznesowej, takich jak zapytania.</span><span class="sxs-lookup"><span data-stu-id="04d7d-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="04d7d-140">Typ jednostki tabeli sprzężenia może nadal istnieć, ale nie powinien przeszkadzać logice biznesowej.</span><span class="sxs-lookup"><span data-stu-id="04d7d-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="04d7d-141">Dlatego zdecydowaliśmy się zająć właściwości nawigacji pominąć dla 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="04d7d-142">W tej chwili inne części wielu do wielu są realizowane jako cel stretch dla EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="04d7d-143">Oznacza to, że nie są one obecnie w planie 5.0, ale jeśli wszystko pójdzie dobrze, mamy nadzieję, że je wciągniemy.</span><span class="sxs-lookup"><span data-stu-id="04d7d-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="04d7d-144">Mapowanie dziedziczenia tabela na typ (TPT)</span><span class="sxs-lookup"><span data-stu-id="04d7d-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="04d7d-145">Główny programista:@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="04d7d-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="04d7d-146">Śledzone przez [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="04d7d-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="04d7d-147">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="04d7d-147">T-shirt size: XL</span></span>

<span data-ttu-id="04d7d-148">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-148">Status: In-progress</span></span>

<span data-ttu-id="04d7d-149">Robimy TPT, ponieważ jest to zarówno bardzo pożądana funkcja (~ 254 głosów; 3 ogólnie), a ponieważ wymaga pewnych zmian niskiego poziomu, które uważamy za odpowiednie dla fundamentalnego charakteru ogólnego planu .NET 5.</span><span class="sxs-lookup"><span data-stu-id="04d7d-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="04d7d-150">Oczekujemy, że spowoduje to wprowadzenie zmian w przypadku dostawców baz danych, chociaż powinny one być znacznie mniej dotkliwe niż zmiany wymagane dla 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="04d7d-151">Filtrowane uwzględnić</span><span class="sxs-lookup"><span data-stu-id="04d7d-151">Filtered Include</span></span>

<span data-ttu-id="04d7d-152">Główny programista:@maumar</span><span class="sxs-lookup"><span data-stu-id="04d7d-152">Lead developer: @maumar</span></span>

<span data-ttu-id="04d7d-153">Śledzone przez [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="04d7d-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="04d7d-154">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="04d7d-154">T-shirt size: M</span></span>

<span data-ttu-id="04d7d-155">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-155">Status: In-progress</span></span>

<span data-ttu-id="04d7d-156">Filtrowane include to bardzo pożądana funkcja (~317 głosów; druga ogólna), która nie jest ogromną ilością pracy i która, jak sądzimy, odblokuje lub ułatwi wiele scenariuszy, które obecnie wymagają filtrów na poziomie modelu lub bardziej złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="04d7d-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="04d7d-157">Racjonalizuj ToTable, ToQuery, ToView, FromSql itp.</span><span class="sxs-lookup"><span data-stu-id="04d7d-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="04d7d-158">Główna @maumar programistka: i@smitpatel</span><span class="sxs-lookup"><span data-stu-id="04d7d-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="04d7d-159">Śledzone przez [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="04d7d-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="04d7d-160">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-160">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-161">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-161">Status: In-progress</span></span>

<span data-ttu-id="04d7d-162">Poczyniliśmy postępy w poprzednich wersjach w kierunku obsługi nieprzetworzonego SQL, typów bezkluzywnych i powiązanych obszarów.</span><span class="sxs-lookup"><span data-stu-id="04d7d-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="04d7d-163">Istnieją jednak zarówno luki, jak i niespójności w sposobie, w jaki wszystko działa razem jako całość.</span><span class="sxs-lookup"><span data-stu-id="04d7d-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="04d7d-164">Celem dla 5.0 jest naprawienie tych i stworzenie dobrego środowiska do definiowania, migracji i używania różnych typów jednostek i skojarzonych z nimi zapytań i artefaktów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="04d7d-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="04d7d-165">Może to również obejmować aktualizacje interfejsu API skompilowanych kwerend.</span><span class="sxs-lookup"><span data-stu-id="04d7d-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="04d7d-166">Należy zauważyć, że ten element może spowodować niektóre zmiany podziału na poziomie aplikacji, ponieważ niektóre funkcje, które obecnie posiadamy, są zbyt permisywne, dzięki czemu mogą szybko prowadzić ludzi do dołków awarii.</span><span class="sxs-lookup"><span data-stu-id="04d7d-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="04d7d-167">Prawdopodobnie w końcu blokujemy niektóre z tych funkcji wraz ze wskazówkami, co zamiast tego zrobić.</span><span class="sxs-lookup"><span data-stu-id="04d7d-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="04d7d-168">Ogólne ulepszenia zapytań</span><span class="sxs-lookup"><span data-stu-id="04d7d-168">General query enhancements</span></span>

<span data-ttu-id="04d7d-169">Główna @smitpatel programistka: i@maumar</span><span class="sxs-lookup"><span data-stu-id="04d7d-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="04d7d-170">Śledzone przez [problemy oznaczone `area-query` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="04d7d-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="04d7d-171">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="04d7d-171">T-shirt size: XL</span></span>

<span data-ttu-id="04d7d-172">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-172">Status: In-progress</span></span>

<span data-ttu-id="04d7d-173">Kod tłumaczenia zapytania został szczegółowo przepisany dla EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="04d7d-174">Kod kwerendy jest ogólnie w znacznie bardziej niezawodnym stanie z tego powodu.</span><span class="sxs-lookup"><span data-stu-id="04d7d-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="04d7d-175">W przypadku 5.0 nie planujemy wprowadzania istotnych zmian zapytań, poza tymi, które są potrzebne do obsługi TPT i pomijania właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="04d7d-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="04d7d-176">Jednak nadal jest wiele pracy potrzebne do ustalenia niektórych długów technicznych pozostałych z 3.0 remont.</span><span class="sxs-lookup"><span data-stu-id="04d7d-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="04d7d-177">Planujemy również naprawić wiele błędów i zaimplementować małe ulepszenia, aby jeszcze bardziej poprawić ogólne środowisko kwerendy.</span><span class="sxs-lookup"><span data-stu-id="04d7d-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="04d7d-178">Migracje i środowisko wdrażania</span><span class="sxs-lookup"><span data-stu-id="04d7d-178">Migrations and deployment experience</span></span>

<span data-ttu-id="04d7d-179">Główna programistka:@bricelam</span><span class="sxs-lookup"><span data-stu-id="04d7d-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="04d7d-180">Śledzone przez [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="04d7d-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="04d7d-181">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-181">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-182">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-182">Status: In-progress</span></span>

<span data-ttu-id="04d7d-183">Obecnie wielu deweloperów migruje swoje bazy danych w czasie uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04d7d-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="04d7d-184">Jest to łatwe, ale nie jest zalecane, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="04d7d-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="04d7d-185">Wiele wątków/procesów/serwerów może próbować migrować bazę danych jednocześnie</span><span class="sxs-lookup"><span data-stu-id="04d7d-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="04d7d-186">Aplikacje mogą próbować uzyskać dostęp do niespójnego stanu, gdy tak się dzieje</span><span class="sxs-lookup"><span data-stu-id="04d7d-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="04d7d-187">Zazwyczaj uprawnienia bazy danych do modyfikowania schematu nie powinny być przyznawane do wykonywania aplikacji</span><span class="sxs-lookup"><span data-stu-id="04d7d-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="04d7d-188">Trudno jest powrócić do czystego stanu, jeśli coś pójdzie nie tak</span><span class="sxs-lookup"><span data-stu-id="04d7d-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="04d7d-189">Chcemy zapewnić lepsze środowisko w tym miejscu, co pozwala na łatwy sposób migracji bazy danych w czasie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="04d7d-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="04d7d-190">Powinno to:</span><span class="sxs-lookup"><span data-stu-id="04d7d-190">This should:</span></span>

* <span data-ttu-id="04d7d-191">Praca na systemach Linux, Mac i Windows</span><span class="sxs-lookup"><span data-stu-id="04d7d-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="04d7d-192">Bądź dobrym doświadczeniem w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="04d7d-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="04d7d-193">Scenariusze pomocy technicznej z kontenerami</span><span class="sxs-lookup"><span data-stu-id="04d7d-193">Support scenarios with containers</span></span>
* <span data-ttu-id="04d7d-194">Praca z powszechnie używanymi rzeczywistymi narzędziami/przepływami wdrażania</span><span class="sxs-lookup"><span data-stu-id="04d7d-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="04d7d-195">Integracja z co najmniej programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04d7d-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="04d7d-196">Rezultatem może być wiele drobnych ulepszeń w EF Core (na przykład lepsze migracje na SQLite), wraz z wytycznymi i długoterminową współpracą z innymi zespołami w celu poprawy kompleksowych doświadczeń, które wykraczają poza tylko EF.</span><span class="sxs-lookup"><span data-stu-id="04d7d-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="04d7d-197">Środowisko platform EF Core</span><span class="sxs-lookup"><span data-stu-id="04d7d-197">EF Core platforms experience</span></span> 

<span data-ttu-id="04d7d-198">Główna @roji programistka: i@bricelam</span><span class="sxs-lookup"><span data-stu-id="04d7d-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="04d7d-199">Śledzone przez [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="04d7d-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="04d7d-200">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-200">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-201">Stan: Nie rozpoczęto</span><span class="sxs-lookup"><span data-stu-id="04d7d-201">Status: Not started</span></span>

<span data-ttu-id="04d7d-202">Mamy dobre wskazówki dotyczące korzystania z EF Core w tradycyjnych aplikacjach internetowych podobnych do MVC.</span><span class="sxs-lookup"><span data-stu-id="04d7d-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="04d7d-203">Brakuje wskazówki dla innych platform i modeli aplikacji lub są nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="04d7d-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="04d7d-204">W przypadku EF Core 5.0 planujemy zbadać, poprawić i udokumentować doświadczenie korzystania z EF Core z:</span><span class="sxs-lookup"><span data-stu-id="04d7d-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="04d7d-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="04d7d-205">Blazor</span></span>
* <span data-ttu-id="04d7d-206">Xamarin, w tym za pomocą AOT / linker historia</span><span class="sxs-lookup"><span data-stu-id="04d7d-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="04d7d-207">WinForms / WPF / WinUI i ewentualnie inne U.I.</span><span class="sxs-lookup"><span data-stu-id="04d7d-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="04d7d-208">Ram</span><span class="sxs-lookup"><span data-stu-id="04d7d-208">frameworks</span></span>

<span data-ttu-id="04d7d-209">Prawdopodobnie będzie to wiele niewielkich ulepszeń ef core, wraz z wytycznymi i długoterminową współpracą z innymi zespołami w celu poprawy kompleksowych doświadczeń, które wykraczają poza tylko EF.</span><span class="sxs-lookup"><span data-stu-id="04d7d-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="04d7d-210">Konkretne obszary, które planujemy przyjrzeć się to:</span><span class="sxs-lookup"><span data-stu-id="04d7d-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="04d7d-211">Wdrażanie, w tym środowisko korzystania z narzędzi EF, takich jak migracje</span><span class="sxs-lookup"><span data-stu-id="04d7d-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="04d7d-212">Modele aplikacji, w tym Xamarin i Blazor, i prawdopodobnie inne</span><span class="sxs-lookup"><span data-stu-id="04d7d-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="04d7d-213">Środowiska SQLite, w tym doświadczenie przestrzenne i przebudowa tabeli</span><span class="sxs-lookup"><span data-stu-id="04d7d-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="04d7d-214">AOT i łączenie doświadczeń</span><span class="sxs-lookup"><span data-stu-id="04d7d-214">AOT and linking experiences</span></span>
* <span data-ttu-id="04d7d-215">Integracja diagnostyki, w tym liczniki perf</span><span class="sxs-lookup"><span data-stu-id="04d7d-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="04d7d-216">Wydajność</span><span class="sxs-lookup"><span data-stu-id="04d7d-216">Performance</span></span>

<span data-ttu-id="04d7d-217">Główny programista:@roji</span><span class="sxs-lookup"><span data-stu-id="04d7d-217">Lead developer: @roji</span></span>

<span data-ttu-id="04d7d-218">Śledzone przez [problemy oznaczone `area-perf` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="04d7d-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="04d7d-219">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-219">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-220">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-220">Status: In-progress</span></span>

<span data-ttu-id="04d7d-221">W przypadku ef core planujemy ulepszyć nasz zestaw testów porównawczych wydajności i wprowadzić ukierunkowane ulepszenia wydajności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="04d7d-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="04d7d-222">Ponadto planujemy zakończyć nowy interfejs API przetwarzania wsadowego ADO.NET, który został prototypowany podczas cyklu wersji 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="04d7d-223">Również w warstwie ADO.NET planujemy dodatkowe ulepszenia wydajności dla dostawcy Npgsql.</span><span class="sxs-lookup"><span data-stu-id="04d7d-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="04d7d-224">W ramach tych prac planujemy również dodać ADO.NET/EF liczniki wydajności Core i inne diagnostyki, stosownie do przypadku.</span><span class="sxs-lookup"><span data-stu-id="04d7d-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="04d7d-225">Dokumentacja architektoniczna/autora</span><span class="sxs-lookup"><span data-stu-id="04d7d-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="04d7d-226">Dokumentator wiodący:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="04d7d-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="04d7d-227">Śledzone przez [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="04d7d-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="04d7d-228">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-228">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-229">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-229">Status: In-progress</span></span>

<span data-ttu-id="04d7d-230">Chodzi o to, aby ułatwić zrozumienie, co dzieje się w wewnętrznych EF Core.</span><span class="sxs-lookup"><span data-stu-id="04d7d-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="04d7d-231">Może to być przydatne dla każdego, kto korzysta z EF Core, ale główną motywacją jest ułatwienie osobom zewnętrznym:</span><span class="sxs-lookup"><span data-stu-id="04d7d-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="04d7d-232">Współtworzenie kodu EF Core</span><span class="sxs-lookup"><span data-stu-id="04d7d-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="04d7d-233">Tworzenie dostawców baz danych</span><span class="sxs-lookup"><span data-stu-id="04d7d-233">Create database providers</span></span>
* <span data-ttu-id="04d7d-234">Tworzenie innych rozszerzeń</span><span class="sxs-lookup"><span data-stu-id="04d7d-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="04d7d-235">Dokumentacja microsoft.data.sqlite</span><span class="sxs-lookup"><span data-stu-id="04d7d-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="04d7d-236">Dokumentator wiodący:@bricelam</span><span class="sxs-lookup"><span data-stu-id="04d7d-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="04d7d-237">Śledzone przez [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="04d7d-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="04d7d-238">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="04d7d-238">T-shirt size: M</span></span>

<span data-ttu-id="04d7d-239">Stan: Ukończono.</span><span class="sxs-lookup"><span data-stu-id="04d7d-239">Status: Completed.</span></span> <span data-ttu-id="04d7d-240">Nowa dokumentacja jest [dostępna w witrynie dokumentów firmy Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="04d7d-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="04d7d-241">Zespół EF jest również właścicielem dostawcy ADO.NET microsoft.Data.Sqlite.</span><span class="sxs-lookup"><span data-stu-id="04d7d-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="04d7d-242">Planujemy w pełni udokumentować tego dostawcę w ramach wersji 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="04d7d-243">Dokumentacja ogólna</span><span class="sxs-lookup"><span data-stu-id="04d7d-243">General documentation</span></span>

<span data-ttu-id="04d7d-244">Dokumentator wiodący:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="04d7d-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="04d7d-245">Śledzone przez [problemy w repo dokumentów w 5.0 kamień milowy](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="04d7d-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="04d7d-246">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-246">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-247">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-247">Status: In-progress</span></span>

<span data-ttu-id="04d7d-248">Jesteśmy już w trakcie aktualizacji dokumentacji dla wersji 3.0 i 3.1.</span><span class="sxs-lookup"><span data-stu-id="04d7d-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="04d7d-249">Pracujemy również nad:</span><span class="sxs-lookup"><span data-stu-id="04d7d-249">We are also working on:</span></span>
  * <span data-ttu-id="04d7d-250">Przegląd dokumentów wprowadzenie, aby uczynić je bardziej przystępne / łatwiejsze do naśladowania</span><span class="sxs-lookup"><span data-stu-id="04d7d-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="04d7d-251">Reorganizacja dokumentów w celu ułatwienia znajdowania i dodawania odsyłaczy</span><span class="sxs-lookup"><span data-stu-id="04d7d-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="04d7d-252">Dodawanie więcej szczegółów i wyjaśnień do istniejących dokumentów</span><span class="sxs-lookup"><span data-stu-id="04d7d-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="04d7d-253">Aktualizowanie próbek i dodawanie kolejnych przykładów</span><span class="sxs-lookup"><span data-stu-id="04d7d-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="04d7d-254">Naprawianie błędów</span><span class="sxs-lookup"><span data-stu-id="04d7d-254">Fixing bugs</span></span>

<span data-ttu-id="04d7d-255">Śledzone przez [problemy oznaczone `type-bug` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="04d7d-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="04d7d-256">@rojiDeweloperzy: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="04d7d-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="04d7d-257">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-257">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-258">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-258">Status: In-progress</span></span>

<span data-ttu-id="04d7d-259">W momencie pisania mamy 135 błędów triaged do naprawienia w wersji 5.0 (z 62 już ustalone), ale istnieje znaczne nakładanie się z _ogólne ulepszenia zapytania_ powyżej.</span><span class="sxs-lookup"><span data-stu-id="04d7d-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="04d7d-260">Przychodzące stawki (problemy, które kończą się jako praca w kamieniu milowym) było około 23 problemów miesięcznie w trakcie wydania 3.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="04d7d-261">Nie wszystkie z nich trzeba będzie naprawić w 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="04d7d-262">W przybliżeniu planujemy rozwiązać dodatkowe 150 problemów w przedziale czasowym 5.0.</span><span class="sxs-lookup"><span data-stu-id="04d7d-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="04d7d-263">Małe ulepszenia</span><span class="sxs-lookup"><span data-stu-id="04d7d-263">Small enhancements</span></span>

<span data-ttu-id="04d7d-264">Śledzone przez [problemy oznaczone `type-enhancement` w 5.0 kamień milowy](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="04d7d-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="04d7d-265">@rojiDeweloperzy: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="04d7d-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="04d7d-266">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="04d7d-266">T-shirt size: L</span></span>

<span data-ttu-id="04d7d-267">Stan: W toku</span><span class="sxs-lookup"><span data-stu-id="04d7d-267">Status: In-progress</span></span>

<span data-ttu-id="04d7d-268">Oprócz większych funkcji opisanych powyżej, mamy również wiele mniejszych ulepszeń zaplanowanych na 5.0, aby naprawić "cięcia papieru".</span><span class="sxs-lookup"><span data-stu-id="04d7d-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="04d7d-269">Należy zauważyć, że wiele z tych ulepszeń są również objęte bardziej ogólne tematy opisane powyżej.</span><span class="sxs-lookup"><span data-stu-id="04d7d-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="04d7d-270">Poniżej linii</span><span class="sxs-lookup"><span data-stu-id="04d7d-270">Below-the-line</span></span>

<span data-ttu-id="04d7d-271">Śledzone przez [problemy oznaczone `consider-for-next-release` ](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="04d7d-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="04d7d-272">Są to poprawki błędów i ulepszenia, które **nie** są obecnie zaplanowane dla wersji 5.0, ale będziemy patrzeć na jako cele rozciągania w zależności od postępów poczynionych w powyższej pracy.</span><span class="sxs-lookup"><span data-stu-id="04d7d-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="04d7d-273">Ponadto podczas planowania zawsze bierzemy pod uwagę [najczęściej wybierane kwestie.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="04d7d-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="04d7d-274">Odcięcie którejkolwiek z tych kwestii z uwolnienia jest zawsze bolesne, ale potrzebujemy realistycznego planu dla zasobów, które mamy.</span><span class="sxs-lookup"><span data-stu-id="04d7d-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="04d7d-275">Opinia</span><span class="sxs-lookup"><span data-stu-id="04d7d-275">Feedback</span></span>

<span data-ttu-id="04d7d-276">Twoja opinia na temat planowania jest ważna.</span><span class="sxs-lookup"><span data-stu-id="04d7d-276">Your feedback on planning is important.</span></span> <span data-ttu-id="04d7d-277">Najlepszym sposobem, aby wskazać znaczenie problemu jest głosowanie (thumbs-up) dla tego problemu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="04d7d-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="04d7d-278">Te dane zostaną następnie uwzględnione w [procesie planowania](../release-planning.md) następnej wersji.</span><span class="sxs-lookup"><span data-stu-id="04d7d-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
