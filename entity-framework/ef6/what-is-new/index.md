---
title: What's New - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490301"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="3ab70-102">What's New in EF6</span><span class="sxs-lookup"><span data-stu-id="3ab70-102">What's New in EF6</span></span>

<span data-ttu-id="3ab70-103">Zdecydowanie zaleca się, że umożliwia najnowszą wersję oficjalną programu Entity Framework upewnij się, że możesz korzystać z najnowszych funkcji i najwyższą trwałość.</span><span class="sxs-lookup"><span data-stu-id="3ab70-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="3ab70-104">Jednak firma Microsoft należy pamiętać, że może być konieczne użycie poprzedniej wersji lub czy może chcesz poeksperymentować z nowych ulepszeń w najnowszej wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="3ab70-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="3ab70-105">Aby zainstalować określonych wersji EF, zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="3ab70-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="3ab70-106">Ta strona zawiera dokumentację funkcje, które znajdują się w kolejnych wersjach.</span><span class="sxs-lookup"><span data-stu-id="3ab70-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="3ab70-107">Najnowsze wersje</span><span class="sxs-lookup"><span data-stu-id="3ab70-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="3ab70-108">EF aktualizacji narzędzi programu Visual Studio 2017 wersji 15.7</span><span class="sxs-lookup"><span data-stu-id="3ab70-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="3ab70-109">W maju 2018 r. wydaliśmy zaktualizowaną wersję narzędzia EF jako część programu Visual Studio 2017 w wersji 15.7.</span><span class="sxs-lookup"><span data-stu-id="3ab70-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="3ab70-110">Zawiera usprawnienia w zakresie niektóre typowe słabe punkty:</span><span class="sxs-lookup"><span data-stu-id="3ab70-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="3ab70-111">Poprawki dla kilku błędów dostępności interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="3ab70-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="3ab70-112">Obejście regresji wydajności programu SQL Server podczas generowania modeli z istniejących baz danych [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="3ab70-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="3ab70-113">Obsługa aktualizacji modeli większych modeli w programie SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="3ab70-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="3ab70-114">Innym sposobem na usprawnienie w tym tej nowej wersji zestawu narzędzi platformy EF jest, że instaluje środowiskiem uruchomieniowym EF 6.2, podczas tworzenia modelu w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="3ab70-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="3ab70-115">Ze starszymi wersjami programu Visual Studio jest możliwość użycia środowiska uruchomieniowego EF 6.2 (a także żadnych poprzednich wersji EF) przez zainstalowanie odpowiedniej wersji pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ab70-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="3ab70-116">EF 6.2 środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="3ab70-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="3ab70-117">Środowiskiem uruchomieniowym EF 6.2 został wydany do narzędzia NuGet w października 2017 r.</span><span class="sxs-lookup"><span data-stu-id="3ab70-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="3ab70-118">Dziękuję w Wielkiej część wysiłek naszej społeczności uczestników projektów typu open source, EF 6.2 zawiera wiele [poprawki usterek](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) i [ulepszenia produktu](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="3ab70-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="3ab70-119">Poniżej przedstawiono krótki opis listy najważniejszych zmian wpływających na środowiskiem uruchomieniowym EF 6.2:</span><span class="sxs-lookup"><span data-stu-id="3ab70-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="3ab70-120">Zmniejsz czas uruchamiania, ładując Zakończono pierwszą modele kodu z trwała pamięć podręczna [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="3ab70-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="3ab70-121">Interfejs Fluent API, aby zdefiniować indeksy [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="3ab70-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="3ab70-122">DbFunctions.Like() umożliwia zapisywanie zapytań LINQ, które dadzą podobne w języku SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="3ab70-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="3ab70-123">Migrate.exe obsługuje teraz — opcja skryptu [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="3ab70-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="3ab70-124">EF6 teraz pracować z wartości klucza, wygenerowane za pomocą sekwencji w programie SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="3ab70-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="3ab70-125">Aktualizuj listę błędów przejściowych strategii wykonywania usługi Azure SQL [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="3ab70-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="3ab70-126">Usterka: Ponowieniem próby wykonania zapytania lub polecenia SQL nie powiodło się z "parametr SqlParameter jest już zawarty w innym SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="3ab70-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="3ab70-127">Błąd: Obliczanie DbQuery.ToString() często upłynie limit czasu w debugerze [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="3ab70-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="3ab70-128">Przyszłe wersje</span><span class="sxs-lookup"><span data-stu-id="3ab70-128">Future Releases</span></span>

<span data-ttu-id="3ab70-129">Informacje na temat przyszłych wersjach programu EF6 można znaleźć w naszej [plan](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="3ab70-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="3ab70-130">Poprzednich wersjach</span><span class="sxs-lookup"><span data-stu-id="3ab70-130">Past Releases</span></span>

<span data-ttu-id="3ab70-131">[Poprzednich wersjach](past-releases.md) strona zawiera archiwum wszystkich wcześniejszych wersji programu EF i główne funkcje, które zostały wprowadzone w każdej wersji.</span><span class="sxs-lookup"><span data-stu-id="3ab70-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
