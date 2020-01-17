---
title: Planowanie dla Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125383"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="9747c-102">Planowanie dla Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="9747c-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="9747c-103">Zgodnie z opisem w [procesie planowania](../release-planning.md)dane wejściowe od uczestników projektu zostały zebrane w ramach wstępnego planu dla wydania EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="9747c-104">Ten plan jest nadal wykonywany w toku.</span><span class="sxs-lookup"><span data-stu-id="9747c-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="9747c-105">Nic tutaj jest zobowiązaniem.</span><span class="sxs-lookup"><span data-stu-id="9747c-105">Nothing here is a commitment.</span></span> <span data-ttu-id="9747c-106">Ten plan jest punktem początkowym, który będzie się rozwijać, gdy douczymy się więcej.</span><span class="sxs-lookup"><span data-stu-id="9747c-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="9747c-107">Niektóre elementy, które nie są obecnie planowane dla 5,0, mogą zostać pobrane.</span><span class="sxs-lookup"><span data-stu-id="9747c-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="9747c-108">Niektóre elementy aktualnie planowane dla 5,0 mogą zostać punted.</span><span class="sxs-lookup"><span data-stu-id="9747c-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="9747c-109">Numer wersji i Data wydania.</span><span class="sxs-lookup"><span data-stu-id="9747c-109">Version number and release date.</span></span>

<span data-ttu-id="9747c-110">EF Core 5,0 jest obecnie zaplanowane do wydania w ramach [programu .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="9747c-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="9747c-111">Wybrano wersję "5,0" do dopasowania z platformą .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="9747c-112">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="9747c-112">Supported platforms</span></span>

<span data-ttu-id="9747c-113">EF Core 5,0 jest planowane do uruchamiania na dowolnej platformie .NET 5,0 w oparciu o [zbieżność tych platform w programie .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="9747c-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="9747c-114">Co to oznacza .NET Standard i rzeczywiste użycie TFM nadal będzie możliwe do ustalenia.</span><span class="sxs-lookup"><span data-stu-id="9747c-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="9747c-115">EF Core 5,0 nie będzie działać na .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9747c-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="9747c-116">Fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="9747c-116">Breaking changes</span></span>

<span data-ttu-id="9747c-117">EF Core 5,0 będzie zawierać pewne istotne zmiany, ale będą znacznie mniej surowe niż w przypadku EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="9747c-118">Naszym celem jest umożliwienie nieprzerwanego aktualizowania większości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9747c-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="9747c-119">Oczekuje się, że istnieją istotne zmiany dotyczące dostawców baz danych, szczególnie w przypadku pomocy technicznej TPT.</span><span class="sxs-lookup"><span data-stu-id="9747c-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="9747c-120">Jednak oczekujemy, że aktualizacja dostawcy na 5,0 będzie mniejsza niż wymagana do zaktualizowania dla 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="9747c-121">Motywy</span><span class="sxs-lookup"><span data-stu-id="9747c-121">Themes</span></span>

<span data-ttu-id="9747c-122">Wyodrębnimy kilka głównych obszarów lub motywów, które będą stanowić podstawę dużych inwestycji w EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="9747c-123">Właściwości nawigacji wiele-do-wielu (a. k. a "pomijanie nawigacji")</span><span class="sxs-lookup"><span data-stu-id="9747c-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="9747c-124">Potencjalni deweloperzy: @smitpatel i @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="9747c-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="9747c-125">Śledzone przez [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="9747c-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="9747c-126">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-126">T-shirt size: L</span></span>

<span data-ttu-id="9747c-127">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-127">Status: In-progress</span></span>

<span data-ttu-id="9747c-128">Funkcja wiele-do-wielu jest najbardziej żądaną funkcją (~ 407 głosów) w zaległości usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="9747c-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="9747c-129">Obsługa relacji wiele-do-wielu można podzielić na trzy główne obszary:</span><span class="sxs-lookup"><span data-stu-id="9747c-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="9747c-130">Pomiń właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9747c-130">Skip navigation properties.</span></span> <span data-ttu-id="9747c-131">Umożliwiają one korzystanie z modelu na potrzeby zapytań itp. bez odwołania do podstawowej jednostki tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="9747c-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="9747c-132">Typy jednostek zbioru właściwości.</span><span class="sxs-lookup"><span data-stu-id="9747c-132">Property-bag entity types.</span></span> <span data-ttu-id="9747c-133">Umożliwiają one użycie standardowego typu CLR (np. `Dictionary`) w przypadku wystąpień jednostek, w taki sposób, że jawny typ CLR nie jest wymagany dla każdego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="9747c-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="9747c-134">Cukier pozwala na łatwą konfigurację relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="9747c-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="9747c-135">Uważamy, że najbardziej znaczący blok dla tych, które chcą uzyskać pomoc techniczną wiele-do-wielu, nie ma możliwości używania "naturalnych" relacji bez odwołujących się do tabeli sprzężenia w logice biznesowej, takiej jak zapytania.</span><span class="sxs-lookup"><span data-stu-id="9747c-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="9747c-136">Typ jednostki połączonej tabeli może nadal istnieć, ale nie powinien się on znajdować w sposób logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="9747c-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="9747c-137">To dlatego, że wybrano opcję pominięcia właściwości nawigacji dla 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="9747c-138">W tym momencie inne części typu wiele-do-wielu są realizowane jako cel ambitny dla EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="9747c-139">Oznacza to, że nie są one obecnie w planie dla 5,0, ale jeśli chcesz, dobrze mamy nadzieję, że powrócisz do nich.</span><span class="sxs-lookup"><span data-stu-id="9747c-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="9747c-140">Mapowanie dziedziczenia według typu tabeli (TPT)</span><span class="sxs-lookup"><span data-stu-id="9747c-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="9747c-141">Lider deweloperów: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="9747c-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="9747c-142">Śledzone przez [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="9747c-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="9747c-143">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="9747c-143">T-shirt size: XL</span></span>

<span data-ttu-id="9747c-144">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9747c-144">Status: Not started</span></span>

<span data-ttu-id="9747c-145">Wykonujemy TPT, ponieważ jest to zarówno wysoce żądana funkcja (~ 254 głosów; trzecia ogólna), ponieważ wymaga ona pewnych zmian niskiego poziomu, które są odpowiednie dla ogólnego charakteru planu .NET 5.</span><span class="sxs-lookup"><span data-stu-id="9747c-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="9747c-146">Oczekujemy, że spowoduje to powstanie istotnych zmian dla dostawców baz danych, chociaż powinny one być znacznie mniej surowe niż zmiany wymagane przez 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="9747c-147">Filtr obejmujący</span><span class="sxs-lookup"><span data-stu-id="9747c-147">Filtered Include</span></span>

<span data-ttu-id="9747c-148">Lider deweloperów: @maumar</span><span class="sxs-lookup"><span data-stu-id="9747c-148">Lead developer: @maumar</span></span>

<span data-ttu-id="9747c-149">Śledzone przez [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="9747c-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="9747c-150">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="9747c-150">T-shirt size: M</span></span>

<span data-ttu-id="9747c-151">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9747c-151">Status: Not started</span></span>

<span data-ttu-id="9747c-152">Filtrowanie include to wysoce żądana funkcja (~ 317 głosów; druga ogólna), która nie jest ogromną ilością pracy, i że firma Microsoft uważa, że nie będzie można zablokować lub ułatwić wielu scenariuszy, które obecnie wymagają filtrów na poziomie modelu lub bardziej złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="9747c-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="9747c-153">Racjonalizacja ToTable, ToQuery, ToView, Z tabel itp.</span><span class="sxs-lookup"><span data-stu-id="9747c-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="9747c-154">Potencjalni deweloperzy: @maumar i @smitpatel</span><span class="sxs-lookup"><span data-stu-id="9747c-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="9747c-155">Śledzone przez [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="9747c-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="9747c-156">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-156">T-shirt size: L</span></span>

<span data-ttu-id="9747c-157">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9747c-157">Status: Not started</span></span>

<span data-ttu-id="9747c-158">Wprowadziliśmy postępy we wcześniejszych wersjach na potrzeby obsługi nieprzetworzonych, typów i niezwiązanych z programem SQL.</span><span class="sxs-lookup"><span data-stu-id="9747c-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="9747c-159">Istnieją jednak zarówno luki, jak i niespójności w sposób, w jaki wszystko działa razem jako całość.</span><span class="sxs-lookup"><span data-stu-id="9747c-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="9747c-160">Celem 5,0 jest rozwiązanie tego problemu i utworzenie dobrego środowiska do definiowania, migrowania i korzystania z różnych typów jednostek oraz skojarzonych z nimi zapytań i artefaktów baz danych.</span><span class="sxs-lookup"><span data-stu-id="9747c-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="9747c-161">Może to również dotyczyć aktualizacji skompilowanego interfejsu API zapytań.</span><span class="sxs-lookup"><span data-stu-id="9747c-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="9747c-162">Należy zauważyć, że ten element może spowodować pewne zmiany na poziomie aplikacji, ponieważ niektóre funkcje obecnie są zbyt ograniczane, dzięki czemu mogą szybko prowadzić do Pits awarii.</span><span class="sxs-lookup"><span data-stu-id="9747c-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="9747c-163">Prawdopodobnie zakończy się blokowanie niektórych z tych funkcji wraz ze wskazówkami dotyczącymi tego, co należy zrobić.</span><span class="sxs-lookup"><span data-stu-id="9747c-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="9747c-164">Ogólne ulepszenia zapytań</span><span class="sxs-lookup"><span data-stu-id="9747c-164">General query enhancements</span></span>

<span data-ttu-id="9747c-165">Potencjalni deweloperzy: @smitpatel i @maumar</span><span class="sxs-lookup"><span data-stu-id="9747c-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="9747c-166">Śledzone przez [problemy oznaczone `area-query`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="9747c-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="9747c-167">Rozmiar koszulki: XL</span><span class="sxs-lookup"><span data-stu-id="9747c-167">T-shirt size: XL</span></span>

<span data-ttu-id="9747c-168">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-168">Status: In-progress</span></span>

<span data-ttu-id="9747c-169">Kod tłumaczenia zapytania został rozbudowany na EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="9747c-170">W związku z tym kod zapytania jest zazwyczaj bardziej niezawodny.</span><span class="sxs-lookup"><span data-stu-id="9747c-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="9747c-171">W przypadku 5,0 nie planuje się wprowadzania istotnych zmian zapytania poza tymi, które są potrzebne do obsługi TPT i pomijania właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9747c-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="9747c-172">Jednak nadal istnieje znacząca konieczność naprawienia długu technicznego pozostałego w porównaniu z 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="9747c-173">Planuje również Rozwiązywanie problemów z wieloma usterkami i zaimplementowanie małych ulepszeń w celu dalszej poprawy ogólnego środowiska zapytań.</span><span class="sxs-lookup"><span data-stu-id="9747c-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="9747c-174">Migracje i środowisko wdrażania</span><span class="sxs-lookup"><span data-stu-id="9747c-174">Migrations and deployment experience</span></span>

<span data-ttu-id="9747c-175">Potencjalni deweloperzy: @bricelam</span><span class="sxs-lookup"><span data-stu-id="9747c-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="9747c-176">Śledzone przez [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="9747c-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="9747c-177">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-177">T-shirt size: L</span></span>

<span data-ttu-id="9747c-178">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-178">Status: In-progress</span></span>

<span data-ttu-id="9747c-179">Obecnie wielu deweloperów migruje swoje bazy danych podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9747c-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="9747c-180">Jest to proste, ale nie jest zalecane, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="9747c-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="9747c-181">Wiele wątków/procesów/serwerów może próbować przeprowadzić migrację bazy danych współbieżnie</span><span class="sxs-lookup"><span data-stu-id="9747c-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="9747c-182">Aplikacje mogą próbować uzyskać dostęp do niespójnego stanu, gdy jest to wykonywane</span><span class="sxs-lookup"><span data-stu-id="9747c-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="9747c-183">Zwykle uprawnienia bazy danych do modyfikacji schematu nie należy przyznawać do wykonania aplikacji</span><span class="sxs-lookup"><span data-stu-id="9747c-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="9747c-184">Jego twarda próba powrotu do stanu czystego, jeśli coś się nie udaje</span><span class="sxs-lookup"><span data-stu-id="9747c-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="9747c-185">Chcemy zapewnić lepsze środowisko w tym miejscu, które pozwala na łatwe Migrowanie bazy danych w czasie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="9747c-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="9747c-186">Powinno to być:</span><span class="sxs-lookup"><span data-stu-id="9747c-186">This should:</span></span>

* <span data-ttu-id="9747c-187">Pracuj w systemach Linux, Mac i Windows</span><span class="sxs-lookup"><span data-stu-id="9747c-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="9747c-188">Być dobrym doświadczeniem w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="9747c-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="9747c-189">Scenariusze pomocy technicznej z kontenerami</span><span class="sxs-lookup"><span data-stu-id="9747c-189">Support scenarios with containers</span></span>
* <span data-ttu-id="9747c-190">Pracuj z powszechnie używanymi rzeczywistymi narzędziami/przepływami wdrażania</span><span class="sxs-lookup"><span data-stu-id="9747c-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="9747c-191">Integruj do co najmniej programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9747c-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="9747c-192">Może to być wiele małych ulepszeń w EF Core (na przykład lepszych migracji przy użyciu oprogramowania SQLite), a także wskazówki i średniookresowe współpracę z innymi zespołami w celu ulepszania kompleksowych środowisk, które wykraczają poza prawie Dr.</span><span class="sxs-lookup"><span data-stu-id="9747c-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="9747c-193">Środowisko EF Core platform</span><span class="sxs-lookup"><span data-stu-id="9747c-193">EF Core platforms experience</span></span> 

<span data-ttu-id="9747c-194">Potencjalni deweloperzy: @roji i @bricelam</span><span class="sxs-lookup"><span data-stu-id="9747c-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="9747c-195">Śledzone przez [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="9747c-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="9747c-196">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-196">T-shirt size: L</span></span>

<span data-ttu-id="9747c-197">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9747c-197">Status: Not started</span></span>

<span data-ttu-id="9747c-198">Mamy dobre wskazówki dotyczące używania EF Core w tradycyjnych aplikacjach internetowych opartych na standardzie MVC.</span><span class="sxs-lookup"><span data-stu-id="9747c-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="9747c-199">Brak wskazówek dotyczących innych platform i modeli aplikacji albo są one nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="9747c-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="9747c-200">W przypadku EF Core 5,0 planujemy zbadać, ulepszyć i udokumentować środowisko korzystania z EF Core z:</span><span class="sxs-lookup"><span data-stu-id="9747c-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="9747c-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="9747c-201">Blazor</span></span>
* <span data-ttu-id="9747c-202">Środowisko Xamarin, w tym używanie scenariusza AOT/konsolidatora</span><span class="sxs-lookup"><span data-stu-id="9747c-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="9747c-203">WinForms/WPF/WinUI i prawdopodobnie inne U.I.</span><span class="sxs-lookup"><span data-stu-id="9747c-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="9747c-204">platform</span><span class="sxs-lookup"><span data-stu-id="9747c-204">frameworks</span></span>

<span data-ttu-id="9747c-205">Jest to prawdopodobnie wiele małych ulepszeń w EF Core, wraz ze wskazówkami i dłuższymi współpracy z innymi zespołami, aby ulepszyć kompleksowe środowiska, które wykraczają poza prawie Dr.</span><span class="sxs-lookup"><span data-stu-id="9747c-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="9747c-206">Określone obszary, które planujemy sprawdzić, to:</span><span class="sxs-lookup"><span data-stu-id="9747c-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="9747c-207">Wdrożenie, w tym środowisko do korzystania z narzędzi EF, takich jak migracja</span><span class="sxs-lookup"><span data-stu-id="9747c-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="9747c-208">Modele aplikacji, w tym Xamarin i Blazor, a prawdopodobnie inne</span><span class="sxs-lookup"><span data-stu-id="9747c-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="9747c-209">Środowiska oprogramowania SQLite, w tym środowisko przestrzenne i ponowne kompilacje tabel</span><span class="sxs-lookup"><span data-stu-id="9747c-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="9747c-210">AOT i łącza — środowisko</span><span class="sxs-lookup"><span data-stu-id="9747c-210">AOT and linking experiences</span></span>
* <span data-ttu-id="9747c-211">Integracja diagnostyki, w tym liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="9747c-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="9747c-212">Wydajność</span><span class="sxs-lookup"><span data-stu-id="9747c-212">Performance</span></span>

<span data-ttu-id="9747c-213">Lider deweloperów: @roji</span><span class="sxs-lookup"><span data-stu-id="9747c-213">Lead developer: @roji</span></span>

<span data-ttu-id="9747c-214">Śledzone przez [problemy oznaczone `area-perf`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="9747c-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="9747c-215">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-215">T-shirt size: L</span></span>

<span data-ttu-id="9747c-216">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-216">Status: In-progress</span></span>

<span data-ttu-id="9747c-217">W EF Core planujemy udoskonalić nasz pakiet testów wydajności i wprowadzić ukierunkowane ulepszenia wydajności środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="9747c-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="9747c-218">Ponadto planujemy ukończenie nowego interfejsu API tworzenia wsadowego ADO.NET, który został utworzony w ramach cyklu wydania 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="9747c-219">Ponadto w warstwie ADO.NET planujemy dodatkowe ulepszenia wydajności dla dostawcy Npgsql.</span><span class="sxs-lookup"><span data-stu-id="9747c-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="9747c-220">W ramach tej pracy zaplanowano również dodanie ADO.NET/EF podstawowych liczników wydajności i innych elementów diagnostycznych odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="9747c-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="9747c-221">Dokumentacja architektury/współautora</span><span class="sxs-lookup"><span data-stu-id="9747c-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="9747c-222">Dokument potencjalnego klienta: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="9747c-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="9747c-223">Śledzone przez [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="9747c-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="9747c-224">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-224">T-shirt size: L</span></span>

<span data-ttu-id="9747c-225">Stan: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9747c-225">Status: Not started</span></span>

<span data-ttu-id="9747c-226">Tutaj warto ułatwić zrozumienie, co się dzieje w wewnętrznych EF Core.</span><span class="sxs-lookup"><span data-stu-id="9747c-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="9747c-227">Może to być przydatne dla każdej osoby korzystającej z EF Core, ale podstawową motywacją jest ułatwienie użytkownikom zewnętrznym:</span><span class="sxs-lookup"><span data-stu-id="9747c-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="9747c-228">Współtworzenie kodu EF Core</span><span class="sxs-lookup"><span data-stu-id="9747c-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="9747c-229">Tworzenie dostawców baz danych</span><span class="sxs-lookup"><span data-stu-id="9747c-229">Create database providers</span></span>
* <span data-ttu-id="9747c-230">Kompiluj inne rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="9747c-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="9747c-231">Dokumentacja Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="9747c-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="9747c-232">Dokument potencjalnego klienta: @bricelam</span><span class="sxs-lookup"><span data-stu-id="9747c-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="9747c-233">Śledzone przez [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="9747c-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="9747c-234">Rozmiar koszulki: M</span><span class="sxs-lookup"><span data-stu-id="9747c-234">T-shirt size: M</span></span>

<span data-ttu-id="9747c-235">Stan: ukończono.</span><span class="sxs-lookup"><span data-stu-id="9747c-235">Status: Completed.</span></span> <span data-ttu-id="9747c-236">Nowa dokumentacja znajduje się [na żywo w witrynie Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="9747c-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="9747c-237">Zespół EF również jest właścicielem dostawcy ADO.NET Microsoft. Data. sqlite.</span><span class="sxs-lookup"><span data-stu-id="9747c-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="9747c-238">Planujemy w pełni udokumentować tego dostawcę w wersji 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="9747c-239">Ogólna dokumentacja</span><span class="sxs-lookup"><span data-stu-id="9747c-239">General documentation</span></span>

<span data-ttu-id="9747c-240">Dokument potencjalnego klienta: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="9747c-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="9747c-241">Śledzone przez [problemy w repozytorium docs w obszarze kontrolnym 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="9747c-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="9747c-242">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-242">T-shirt size: L</span></span>

<span data-ttu-id="9747c-243">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-243">Status: In-progress</span></span>

<span data-ttu-id="9747c-244">Już trwa proces aktualizacji dokumentacji dla wersji 3,0 i 3,1.</span><span class="sxs-lookup"><span data-stu-id="9747c-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="9747c-245">Pracujemy również nad:</span><span class="sxs-lookup"><span data-stu-id="9747c-245">We are also working on:</span></span>
  * <span data-ttu-id="9747c-246">Remontowanie dokumentów wprowadzających wprowadzenie, aby zwiększyć ich podejście/łatwiejsze w obserwowanie</span><span class="sxs-lookup"><span data-stu-id="9747c-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="9747c-247">Reorganizacja dokumentów, aby ułatwić znajdowanie i Dodawanie odsyłaczy</span><span class="sxs-lookup"><span data-stu-id="9747c-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="9747c-248">Dodawanie dalszych szczegółów i wyjaśnień do istniejących dokumentów</span><span class="sxs-lookup"><span data-stu-id="9747c-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="9747c-249">Aktualizowanie przykładów i Dodawanie kolejnych przykładów</span><span class="sxs-lookup"><span data-stu-id="9747c-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="9747c-250">Naprawianie usterek</span><span class="sxs-lookup"><span data-stu-id="9747c-250">Fixing bugs</span></span>

<span data-ttu-id="9747c-251">Śledzone przez [problemy oznaczone `type-bug`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="9747c-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="9747c-252">Deweloperzy: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="9747c-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="9747c-253">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-253">T-shirt size: L</span></span>

<span data-ttu-id="9747c-254">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-254">Status: In-progress</span></span>

<span data-ttu-id="9747c-255">W momencie pisania mamy 135 błędów zaklasyfikowany w wersji 5,0 (z 62 już Naprawiono), ale istnieje duże nakładanie się na sekcję _Ogólne ulepszenia zapytania_ .</span><span class="sxs-lookup"><span data-stu-id="9747c-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="9747c-256">Szybkość odbierania (problemy, które kończą się w trakcie pracy w kontrolce), dotyczyła 23 problemów miesięcznie w trakcie wydania 3,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="9747c-257">Nie wszystkie z nich będą musiały zostać naprawione w 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="9747c-258">Jako przybliżony szacunek planujemy rozwiązać dodatkowe 150 problemów w przedziale czasowym 5,0.</span><span class="sxs-lookup"><span data-stu-id="9747c-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="9747c-259">Małe ulepszenia</span><span class="sxs-lookup"><span data-stu-id="9747c-259">Small enhancements</span></span>

<span data-ttu-id="9747c-260">Śledzone przez [problemy oznaczone `type-enhancement`m w obszarze kontrolnym 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="9747c-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="9747c-261">Deweloperzy: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="9747c-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="9747c-262">Rozmiar koszulki: L</span><span class="sxs-lookup"><span data-stu-id="9747c-262">T-shirt size: L</span></span>

<span data-ttu-id="9747c-263">Stan: w toku</span><span class="sxs-lookup"><span data-stu-id="9747c-263">Status: In-progress</span></span>

<span data-ttu-id="9747c-264">Oprócz większych funkcji opisanych powyżej, firma Microsoft oferuje również wiele mniejszych ulepszeń zaplanowanych na 5,0, aby naprawić "kawałki papieru".</span><span class="sxs-lookup"><span data-stu-id="9747c-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="9747c-265">Należy zauważyć, że wiele z tych ulepszeń jest również objętych bardziej ogólnymi motywami opisanymi powyżej.</span><span class="sxs-lookup"><span data-stu-id="9747c-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="9747c-266">Poniżej — wiersz</span><span class="sxs-lookup"><span data-stu-id="9747c-266">Below-the-line</span></span>

<span data-ttu-id="9747c-267">Śledzone przez [problemy oznaczone `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="9747c-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="9747c-268">Są to poprawki i udoskonalenia błędów, które **nie** są obecnie planowane dla wydania 5,0, ale będziemy przebiegać zgodnie z przeznaczeniem do rozciągnięcia w zależności od postępu wykonywanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="9747c-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="9747c-269">Ponadto zawsze należy wziąć pod uwagę [najczęstsze problemy związane](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) z planowaniem.</span><span class="sxs-lookup"><span data-stu-id="9747c-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="9747c-270">Wszystkie te problemy w wersji są zawsze bolesnym, ale potrzebujemy realistycznego planu dla zasobów, które mamy.</span><span class="sxs-lookup"><span data-stu-id="9747c-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="9747c-271">Opinie</span><span class="sxs-lookup"><span data-stu-id="9747c-271">Feedback</span></span>

<span data-ttu-id="9747c-272">Twoja opinia na temat planowania jest ważna.</span><span class="sxs-lookup"><span data-stu-id="9747c-272">Your feedback on planning is important.</span></span> <span data-ttu-id="9747c-273">Najlepszym sposobem na wskazanie znaczenia problemu jest zagłosowanie (kciuki) dla tego problemu w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="9747c-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="9747c-274">Te dane zostaną następnie [przetworzone do procesu planowania](../release-planning.md) dla kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="9747c-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
