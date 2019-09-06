---
title: Co nowego — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271445"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="7de8d-102">Co nowego w programie EF6</span><span class="sxs-lookup"><span data-stu-id="7de8d-102">What's New in EF6</span></span>

<span data-ttu-id="7de8d-103">Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.</span><span class="sxs-lookup"><span data-stu-id="7de8d-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="7de8d-104">Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="7de8d-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="7de8d-105">Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="7de8d-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="7de8d-106">Ta strona zawiera dokumenty dotyczące funkcji uwzględnionych w każdej nowej wersji.</span><span class="sxs-lookup"><span data-stu-id="7de8d-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="7de8d-107">Najnowsze wersje</span><span class="sxs-lookup"><span data-stu-id="7de8d-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="7de8d-108">Aktualizacja narzędzi EF w programie Visual Studio 2017 15,7</span><span class="sxs-lookup"><span data-stu-id="7de8d-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="7de8d-109">W maju 2018 firma Microsoft udostępniła zaktualizowaną wersję narzędzi EF w ramach programu Visual Studio 2017 15,7.</span><span class="sxs-lookup"><span data-stu-id="7de8d-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="7de8d-110">Obejmuje to ulepszenia niektórych typowych punktów:</span><span class="sxs-lookup"><span data-stu-id="7de8d-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="7de8d-111">Poprawki dla kilku błędów ułatwień dostępu do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="7de8d-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="7de8d-112">Obejście SQL Server regresji wydajności podczas generowania modeli z istniejących baz danych [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="7de8d-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="7de8d-113">Obsługa aktualizowania modeli dla większych modeli w SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="7de8d-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="7de8d-114">Innym ulepszeniem tej nowej wersji narzędzi EF jest zainstalowanie środowiska uruchomieniowego EF 6,2 podczas tworzenia modelu w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="7de8d-114">Another improvement in this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="7de8d-115">W starszych wersjach programu Visual Studio można używać środowiska uruchomieniowego EF 6,2 (a także dowolnej starszej wersji EF) przez zainstalowanie odpowiedniej wersji pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="7de8d-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="7de8d-116">Środowisko uruchomieniowe Dr 6,2</span><span class="sxs-lookup"><span data-stu-id="7de8d-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="7de8d-117">Środowisko uruchomieniowe EF 6,2 zostało wydane do NuGet w październiku z 2017.</span><span class="sxs-lookup"><span data-stu-id="7de8d-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="7de8d-118">Dziękujemy za dążenie do wysiłków naszej społeczności uczestników oprogramowania typu open source, EF 6,2 zawiera wiele [poprawek błędów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) i [ulepszeń produktów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="7de8d-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="7de8d-119">Poniżej przedstawiono krótką listę najważniejszych zmian wpływających na środowisko uruchomieniowe EF 6,2:</span><span class="sxs-lookup"><span data-stu-id="7de8d-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="7de8d-120">Skrócenie czasu uruchamiania przez załadowanie pierwszego modelu kodu z trwałej pamięci podręcznej [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="7de8d-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="7de8d-121">Interfejs API Fluent do definiowania indeksów [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="7de8d-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="7de8d-122">Dbfunctions. like () w celu umożliwienia pisania zapytań LINQ, które są tłumaczone na takie jak w programie SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="7de8d-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="7de8d-123">Program migruje. exe obsługuje teraz- [#240](https://github.com/aspnet/EntityFramework6/issues/240) opcji skryptu</span><span class="sxs-lookup"><span data-stu-id="7de8d-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="7de8d-124">EF6 może teraz korzystać z wartości kluczy generowanych przez sekwencję w SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="7de8d-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="7de8d-125">Aktualizowanie listy błędów przejściowych dla strategii wykonywania SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="7de8d-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="7de8d-126">Usterki Ponawianie próby wykonania zapytań lub poleceń SQL kończy się niepowodzeniem z błędem "SqlParameter jest już zawarty w innym SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="7de8d-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="7de8d-127">Usterki Obliczanie programu DBQuery. ToString () często trwa w debugerze [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="7de8d-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="7de8d-128">Przyszłe wersje</span><span class="sxs-lookup"><span data-stu-id="7de8d-128">Future Releases</span></span>

<span data-ttu-id="7de8d-129">Aby uzyskać informacje na temat przyszłej wersji programu EF6, zapoznaj się z naszym [planem](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="7de8d-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="7de8d-130">Wcześniejsze wersje</span><span class="sxs-lookup"><span data-stu-id="7de8d-130">Past Releases</span></span>

<span data-ttu-id="7de8d-131">Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="7de8d-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
