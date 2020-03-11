---
title: Planowanie dla Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: c5b7300c61c2f668b6f9393ae51bf9ebddf330a7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417877"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="ee9b5-102">Planowanie dla Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="ee9b5-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="ee9b5-103">Zgodnie z opisem w [procesie planowania](../release-planning.md)dane wejściowe od uczestników projektu zostały zebrane w ramach wstępnego planu dla wydania EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ee9b5-104">Ten plan jest nadal wykonywany w toku.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="ee9b5-105">Nic tutaj jest zobowiązaniem.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-105">Nothing here is a commitment.</span></span> <span data-ttu-id="ee9b5-106">Ten plan jest punktem początkowym, który będzie się rozwijać, gdy douczymy się więcej.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="ee9b5-107">Niektóre elementy, które nie są obecnie planowane dla 5,0, mogą zostać pobrane.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="ee9b5-108">Niektóre elementy aktualnie planowane dla 5,0 mogą zostać punted.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="ee9b5-109">Numer wersji i Data wydania.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-109">Version number and release date.</span></span>

<span data-ttu-id="ee9b5-110">EF Core 5,0 jest obecnie zaplanowane do wydania w ramach [programu .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="ee9b5-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="ee9b5-111">Wybrano wersję "5,0" do dopasowania z platformą .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="ee9b5-112">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="ee9b5-112">Supported platforms</span></span>

<span data-ttu-id="ee9b5-113">EF Core 5,0 jest planowane do uruchamiania na dowolnej platformie .NET 5,0 w oparciu o [zbieżność tych platform w programie .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="ee9b5-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="ee9b5-114">Co to oznacza .NET Standard i rzeczywiste użycie TFM nadal będzie możliwe do ustalenia.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="ee9b5-115">EF Core 5,0 nie będzie działać na .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="ee9b5-116">Zmiany powodujące niezgodność</span><span class="sxs-lookup"><span data-stu-id="ee9b5-116">Breaking changes</span></span>

<span data-ttu-id="ee9b5-117">EF Core 5,0 będzie zawierać pewne istotne zmiany, ale będą znacznie mniej surowe niż w przypadku EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="ee9b5-118">Naszym celem jest umożliwienie nieprzerwanego aktualizowania większości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="ee9b5-119">Oczekuje się, że istnieją istotne zmiany dotyczące dostawców baz danych, szczególnie w przypadku pomocy technicznej TPT.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="ee9b5-120">Jednak oczekujemy, że aktualizacja dostawcy na 5,0 będzie mniejsza niż wymagana do zaktualizowania dla 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="ee9b5-121">Motywy</span><span class="sxs-lookup"><span data-stu-id="ee9b5-121">Themes</span></span>

<span data-ttu-id="ee9b5-122">Wyodrębnimy kilka głównych obszarów lub motywów, które będą stanowić podstawę dużych inwestycji w EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="ee9b5-123">Właściwości nawigacji wiele-do-wielu (a. k. a "pomijanie nawigacji")</span><span class="sxs-lookup"><span data-stu-id="ee9b5-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="ee9b5-124">Potencjalni deweloperzy: @smitpatel i @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="ee9b5-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="ee9b5-125">Śledzone przez [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="ee9b5-126">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-126">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-127">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-127">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-128">Funkcja wiele-do-wielu jest [najbardziej żądaną funkcją](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 głosów) w zaległości usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="ee9b5-129">Obsługa relacji wiele-do-wielu w całości jest śledzona jako [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="ee9b5-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="ee9b5-130">Można to podzielić na trzy główne obszary:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="ee9b5-131">Pomiń właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-131">Skip navigation properties.</span></span> <span data-ttu-id="ee9b5-132">Umożliwiają one korzystanie z modelu na potrzeby zapytań itp. bez odwołania do podstawowej jednostki tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="ee9b5-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="ee9b5-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="ee9b5-134">Typy jednostek zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-134">Property-bag entity types.</span></span> <span data-ttu-id="ee9b5-135">Umożliwiają one użycie standardowego typu CLR (np. `Dictionary`) w przypadku wystąpień jednostek, w taki sposób, że jawny typ CLR nie jest wymagany dla każdego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="ee9b5-136">(Rozciągnij dla 5,0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="ee9b5-137">Cukier pozwala na łatwą konfigurację relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="ee9b5-138">(Rozciągnij dla 5,0).</span><span class="sxs-lookup"><span data-stu-id="ee9b5-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="ee9b5-139">Uważamy, że najbardziej znaczący blok dla tych, które chcą uzyskać pomoc techniczną wiele-do-wielu, nie ma możliwości używania "naturalnych" relacji bez odwołujących się do tabeli sprzężenia w logice biznesowej, takiej jak zapytania.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="ee9b5-140">Typ jednostki połączonej tabeli może nadal istnieć, ale nie powinien się on znajdować w sposób logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="ee9b5-141">To dlatego, że wybrano opcję pominięcia właściwości nawigacji dla 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="ee9b5-142">W tym momencie inne części typu wiele-do-wielu są realizowane jako cel ambitny dla EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="ee9b5-143">Oznacza to, że nie są one obecnie w planie dla 5,0, ale jeśli chcesz, dobrze mamy nadzieję, że powrócisz do nich.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="ee9b5-144">Mapowanie dziedziczenia według typu tabeli (TPT)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="ee9b5-145">Lider deweloperów: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="ee9b5-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="ee9b5-146">Śledzone przez [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="ee9b5-147">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="ee9b5-147">T-shirt size: XL</span></span>

<span data-ttu-id="ee9b5-148">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="ee9b5-148">Status: Not started</span></span>

<span data-ttu-id="ee9b5-149">Wykonujemy TPT, ponieważ jest to zarówno wysoce żądana funkcja (~ 254 głosów; trzecia ogólna), ponieważ wymaga ona pewnych zmian niskiego poziomu, które są odpowiednie dla ogólnego charakteru planu .NET 5.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="ee9b5-150">Oczekujemy, że spowoduje to powstanie istotnych zmian dla dostawców baz danych, chociaż powinny one być znacznie mniej surowe niż zmiany wymagane przez 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="ee9b5-151">Filtr obejmujący</span><span class="sxs-lookup"><span data-stu-id="ee9b5-151">Filtered Include</span></span>

<span data-ttu-id="ee9b5-152">Lider deweloperów: @maumar</span><span class="sxs-lookup"><span data-stu-id="ee9b5-152">Lead developer: @maumar</span></span>

<span data-ttu-id="ee9b5-153">Śledzone przez [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="ee9b5-154">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="ee9b5-154">T-shirt size: M</span></span>

<span data-ttu-id="ee9b5-155">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="ee9b5-155">Status: Not started</span></span>

<span data-ttu-id="ee9b5-156">Filtrowanie include to wysoce żądana funkcja (~ 317 głosów; druga ogólna), która nie jest ogromną ilością pracy, i że firma Microsoft uważa, że nie będzie można zablokować lub ułatwić wielu scenariuszy, które obecnie wymagają filtrów na poziomie modelu lub bardziej złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="ee9b5-157">Racjonalizacja ToTable, ToQuery, ToView, Z tabel itp.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="ee9b5-158">Potencjalni deweloperzy: @maumar i @smitpatel</span><span class="sxs-lookup"><span data-stu-id="ee9b5-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="ee9b5-159">Śledzone przez [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="ee9b5-160">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-160">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-161">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="ee9b5-161">Status: Not started</span></span>

<span data-ttu-id="ee9b5-162">Wprowadziliśmy postępy we wcześniejszych wersjach na potrzeby obsługi nieprzetworzonych, typów i niezwiązanych z programem SQL.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="ee9b5-163">Istnieją jednak zarówno luki, jak i niespójności w sposób, w jaki wszystko działa razem jako całość.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="ee9b5-164">Celem 5,0 jest rozwiązanie tego problemu i utworzenie dobrego środowiska do definiowania, migrowania i korzystania z różnych typów jednostek oraz skojarzonych z nimi zapytań i artefaktów baz danych.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="ee9b5-165">Może to również dotyczyć aktualizacji skompilowanego interfejsu API zapytań.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="ee9b5-166">Należy zauważyć, że ten element może spowodować pewne zmiany na poziomie aplikacji, ponieważ niektóre funkcje obecnie są zbyt ograniczane, dzięki czemu mogą szybko prowadzić do Pits awarii.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="ee9b5-167">Prawdopodobnie zakończy się blokowanie niektórych z tych funkcji wraz ze wskazówkami dotyczącymi tego, co należy zrobić.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="ee9b5-168">Ogólne ulepszenia zapytań</span><span class="sxs-lookup"><span data-stu-id="ee9b5-168">General query enhancements</span></span>

<span data-ttu-id="ee9b5-169">Potencjalni deweloperzy: @smitpatel i @maumar</span><span class="sxs-lookup"><span data-stu-id="ee9b5-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="ee9b5-170">Śledzone przez [problemy oznaczone `area-query`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ee9b5-171">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="ee9b5-171">T-shirt size: XL</span></span>

<span data-ttu-id="ee9b5-172">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-172">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-173">Kod tłumaczenia zapytania został rozbudowany na EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="ee9b5-174">W związku z tym kod zapytania jest zazwyczaj bardziej niezawodny.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="ee9b5-175">W przypadku 5,0 nie planuje się wprowadzania istotnych zmian zapytania poza tymi, które są potrzebne do obsługi TPT i pomijania właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="ee9b5-176">Jednak nadal istnieje znacząca konieczność naprawienia długu technicznego pozostałego w porównaniu z 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="ee9b5-177">Planuje również Rozwiązywanie problemów z wieloma usterkami i zaimplementowanie małych ulepszeń w celu dalszej poprawy ogólnego środowiska zapytań.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="ee9b5-178">Migracje i środowisko wdrażania</span><span class="sxs-lookup"><span data-stu-id="ee9b5-178">Migrations and deployment experience</span></span>

<span data-ttu-id="ee9b5-179">Potencjalni deweloperzy: @bricelam</span><span class="sxs-lookup"><span data-stu-id="ee9b5-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="ee9b5-180">Śledzone przez [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="ee9b5-181">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-181">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-182">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-182">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-183">Obecnie wielu deweloperów migruje swoje bazy danych podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="ee9b5-184">Jest to proste, ale nie jest zalecane, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="ee9b5-185">Wiele wątków/procesów/serwerów może próbować przeprowadzić migrację bazy danych współbieżnie</span><span class="sxs-lookup"><span data-stu-id="ee9b5-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="ee9b5-186">Aplikacje mogą próbować uzyskać dostęp do niespójnego stanu, gdy jest to wykonywane</span><span class="sxs-lookup"><span data-stu-id="ee9b5-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="ee9b5-187">Zwykle uprawnienia bazy danych do modyfikacji schematu nie należy przyznawać do wykonania aplikacji</span><span class="sxs-lookup"><span data-stu-id="ee9b5-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="ee9b5-188">Jeśli coś się nie stanie, trudno wrócić do stanu czystego</span><span class="sxs-lookup"><span data-stu-id="ee9b5-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="ee9b5-189">Chcemy zapewnić lepsze środowisko w tym miejscu, które pozwala na łatwe Migrowanie bazy danych w czasie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="ee9b5-190">Powinno to być:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-190">This should:</span></span>

* <span data-ttu-id="ee9b5-191">Pracuj w systemach Linux, Mac i Windows</span><span class="sxs-lookup"><span data-stu-id="ee9b5-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="ee9b5-192">Być dobrym doświadczeniem w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="ee9b5-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="ee9b5-193">Scenariusze pomocy technicznej z kontenerami</span><span class="sxs-lookup"><span data-stu-id="ee9b5-193">Support scenarios with containers</span></span>
* <span data-ttu-id="ee9b5-194">Pracuj z powszechnie używanymi rzeczywistymi narzędziami/przepływami wdrażania</span><span class="sxs-lookup"><span data-stu-id="ee9b5-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="ee9b5-195">Integruj do co najmniej programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee9b5-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="ee9b5-196">Może to być wiele małych ulepszeń w EF Core (na przykład lepszych migracji przy użyciu oprogramowania SQLite), a także wskazówki i średniookresowe współpracę z innymi zespołami w celu ulepszania kompleksowych środowisk, które wykraczają poza prawie Dr.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="ee9b5-197">Środowisko EF Core platform</span><span class="sxs-lookup"><span data-stu-id="ee9b5-197">EF Core platforms experience</span></span> 

<span data-ttu-id="ee9b5-198">Potencjalni deweloperzy: @roji i @bricelam</span><span class="sxs-lookup"><span data-stu-id="ee9b5-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="ee9b5-199">Śledzone przez [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="ee9b5-200">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-200">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-201">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="ee9b5-201">Status: Not started</span></span>

<span data-ttu-id="ee9b5-202">Mamy dobre wskazówki dotyczące używania EF Core w tradycyjnych aplikacjach internetowych opartych na standardzie MVC.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="ee9b5-203">Brak wskazówek dotyczących innych platform i modeli aplikacji albo są one nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="ee9b5-204">W przypadku EF Core 5,0 planujemy zbadać, ulepszyć i udokumentować środowisko korzystania z EF Core z:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="ee9b5-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="ee9b5-205">Blazor</span></span>
* <span data-ttu-id="ee9b5-206">Środowisko Xamarin, w tym używanie scenariusza AOT/konsolidatora</span><span class="sxs-lookup"><span data-stu-id="ee9b5-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="ee9b5-207">WinForms/WPF/WinUI i prawdopodobnie inne U.I.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="ee9b5-208">platform</span><span class="sxs-lookup"><span data-stu-id="ee9b5-208">frameworks</span></span>

<span data-ttu-id="ee9b5-209">Jest to prawdopodobnie wiele małych ulepszeń w EF Core, wraz ze wskazówkami i dłuższymi współpracy z innymi zespołami, aby ulepszyć kompleksowe środowiska, które wykraczają poza prawie Dr.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="ee9b5-210">Określone obszary, które planujemy sprawdzić, to:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="ee9b5-211">Wdrożenie, w tym środowisko do korzystania z narzędzi EF, takich jak migracja</span><span class="sxs-lookup"><span data-stu-id="ee9b5-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="ee9b5-212">Modele aplikacji, w tym Xamarin i Blazor, a prawdopodobnie inne</span><span class="sxs-lookup"><span data-stu-id="ee9b5-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="ee9b5-213">Środowiska oprogramowania SQLite, w tym środowisko przestrzenne i ponowne kompilacje tabel</span><span class="sxs-lookup"><span data-stu-id="ee9b5-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="ee9b5-214">AOT i łącza — środowisko</span><span class="sxs-lookup"><span data-stu-id="ee9b5-214">AOT and linking experiences</span></span>
* <span data-ttu-id="ee9b5-215">Integracja diagnostyki, w tym liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="ee9b5-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="ee9b5-216">Wydajność</span><span class="sxs-lookup"><span data-stu-id="ee9b5-216">Performance</span></span>

<span data-ttu-id="ee9b5-217">Lider deweloperów: @roji</span><span class="sxs-lookup"><span data-stu-id="ee9b5-217">Lead developer: @roji</span></span>

<span data-ttu-id="ee9b5-218">Śledzone przez [problemy oznaczone `area-perf`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ee9b5-219">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-219">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-220">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-220">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-221">W przypadku EF Core planujemy udoskonalić nasz pakiet testów wydajnościowych i wprowadzić ukierunkowane ulepszenia wydajności środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="ee9b5-222">Ponadto planujemy ukończenie nowego interfejsu API tworzenia wsadowego ADO.NET, który został utworzony w ramach cyklu wydania 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="ee9b5-223">Ponadto w warstwie ADO.NET planujemy dodatkowe ulepszenia wydajności dla dostawcy Npgsql.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="ee9b5-224">W ramach tej pracy zaplanowano również dodanie ADO.NET/EF podstawowych liczników wydajności i innych elementów diagnostycznych odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="ee9b5-225">Dokumentacja architektury/współautora</span><span class="sxs-lookup"><span data-stu-id="ee9b5-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="ee9b5-226">Dokument potencjalnego klienta: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ee9b5-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="ee9b5-227">Śledzone przez [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="ee9b5-228">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-228">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-229">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="ee9b5-229">Status: Not started</span></span>

<span data-ttu-id="ee9b5-230">Tutaj warto ułatwić zrozumienie, co się dzieje w wewnętrznych EF Core.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="ee9b5-231">Może to być przydatne dla każdej osoby korzystającej z EF Core, ale podstawową motywacją jest ułatwienie użytkownikom zewnętrznym:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="ee9b5-232">Współtworzenie kodu EF Core</span><span class="sxs-lookup"><span data-stu-id="ee9b5-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="ee9b5-233">Tworzenie dostawców baz danych</span><span class="sxs-lookup"><span data-stu-id="ee9b5-233">Create database providers</span></span>
* <span data-ttu-id="ee9b5-234">Kompiluj inne rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="ee9b5-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="ee9b5-235">Dokumentacja Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="ee9b5-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="ee9b5-236">Dokument potencjalnego klienta: @bricelam</span><span class="sxs-lookup"><span data-stu-id="ee9b5-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="ee9b5-237">Śledzone przez [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="ee9b5-238">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="ee9b5-238">T-shirt size: M</span></span>

<span data-ttu-id="ee9b5-239">Stan: ukończono.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-239">Status: Completed.</span></span> <span data-ttu-id="ee9b5-240">Nowa dokumentacja znajduje się [na żywo w witrynie Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="ee9b5-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="ee9b5-241">Zespół EF również jest właścicielem dostawcy ADO.NET Microsoft. Data. sqlite.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="ee9b5-242">Planujemy w pełni udokumentować tego dostawcę w wersji 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="ee9b5-243">Ogólna dokumentacja</span><span class="sxs-lookup"><span data-stu-id="ee9b5-243">General documentation</span></span>

<span data-ttu-id="ee9b5-244">Dokument potencjalnego klienta: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ee9b5-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="ee9b5-245">Śledzone przez [problemy w repozytorium docs w obszarze kontrolnym 5,0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ee9b5-246">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-246">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-247">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-247">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-248">Już trwa proces aktualizacji dokumentacji dla wersji 3,0 i 3,1.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="ee9b5-249">Pracujemy również nad:</span><span class="sxs-lookup"><span data-stu-id="ee9b5-249">We are also working on:</span></span>
  * <span data-ttu-id="ee9b5-250">Remontowanie dokumentów wprowadzających wprowadzenie, aby zwiększyć ich podejście/łatwiejsze w obserwowanie</span><span class="sxs-lookup"><span data-stu-id="ee9b5-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="ee9b5-251">Reorganizacja dokumentów, aby ułatwić znajdowanie i Dodawanie odsyłaczy</span><span class="sxs-lookup"><span data-stu-id="ee9b5-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="ee9b5-252">Dodawanie dalszych szczegółów i wyjaśnień do istniejących dokumentów</span><span class="sxs-lookup"><span data-stu-id="ee9b5-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="ee9b5-253">Aktualizowanie przykładów i Dodawanie kolejnych przykładów</span><span class="sxs-lookup"><span data-stu-id="ee9b5-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="ee9b5-254">Naprawianie usterek</span><span class="sxs-lookup"><span data-stu-id="ee9b5-254">Fixing bugs</span></span>

<span data-ttu-id="ee9b5-255">Śledzone przez [problemy oznaczone `type-bug`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="ee9b5-256">Deweloperzy: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ee9b5-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="ee9b5-257">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-257">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-258">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-258">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-259">W momencie pisania mamy 135 błędów zaklasyfikowany w wersji 5,0 (z 62 już Naprawiono), ale istnieje duże nakładanie się na sekcję _Ogólne ulepszenia zapytania_ .</span><span class="sxs-lookup"><span data-stu-id="ee9b5-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="ee9b5-260">Szybkość odbierania (problemy, które kończą się w trakcie pracy w kontrolce), dotyczyła 23 problemów miesięcznie w trakcie wydania 3,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="ee9b5-261">Nie wszystkie z nich będą musiały zostać naprawione w 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="ee9b5-262">Jako przybliżony szacunek planujemy rozwiązać dodatkowe 150 problemów w przedziale czasowym 5,0.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="ee9b5-263">Małe ulepszenia</span><span class="sxs-lookup"><span data-stu-id="ee9b5-263">Small enhancements</span></span>

<span data-ttu-id="ee9b5-264">Śledzone przez [problemy oznaczone `type-enhancement`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="ee9b5-265">Deweloperzy: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ee9b5-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="ee9b5-266">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="ee9b5-266">T-shirt size: L</span></span>

<span data-ttu-id="ee9b5-267">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="ee9b5-267">Status: In-progress</span></span>

<span data-ttu-id="ee9b5-268">Oprócz większych funkcji opisanych powyżej, firma Microsoft oferuje również wiele mniejszych ulepszeń zaplanowanych na 5,0, aby naprawić "kawałki papieru".</span><span class="sxs-lookup"><span data-stu-id="ee9b5-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="ee9b5-269">Należy zauważyć, że wiele z tych ulepszeń jest również objętych bardziej ogólnymi motywami opisanymi powyżej.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="ee9b5-270">Poniżej — wiersz</span><span class="sxs-lookup"><span data-stu-id="ee9b5-270">Below-the-line</span></span>

<span data-ttu-id="ee9b5-271">Śledzone przez [problemy oznaczone `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="ee9b5-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="ee9b5-272">Są to poprawki i udoskonalenia błędów, które **nie** są obecnie planowane dla wydania 5,0, ale będziemy przebiegać zgodnie z przeznaczeniem do rozciągnięcia w zależności od postępu wykonywanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="ee9b5-273">Ponadto zawsze należy wziąć pod uwagę [najczęstsze problemy związane](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) z planowaniem.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="ee9b5-274">Wszystkie te problemy w wersji są zawsze bolesnym, ale potrzebujemy realistycznego planu dla zasobów, które mamy.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="ee9b5-275">Opinia</span><span class="sxs-lookup"><span data-stu-id="ee9b5-275">Feedback</span></span>

<span data-ttu-id="ee9b5-276">Twoja opinia na temat planowania jest ważna.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-276">Your feedback on planning is important.</span></span> <span data-ttu-id="ee9b5-277">Najlepszym sposobem na wskazanie znaczenia problemu jest zagłosowanie (kciuki) dla tego problemu w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="ee9b5-278">Te dane zostaną następnie [przetworzone do procesu planowania](../release-planning.md) dla kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="ee9b5-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
