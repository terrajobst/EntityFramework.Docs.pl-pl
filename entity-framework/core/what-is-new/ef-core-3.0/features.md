---
title: Nowe funkcje w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: a71aa01e81d9830d7b9e6cb01c200851100a15df
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628431"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="f3d62-102">Nowe funkcje zawarte w EF Core 3,0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="f3d62-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3d62-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, a mimo to, że firma Microsoft podejmie próbę zapewnienia aktualności tej strony, może nie odzwierciedlać naszych najnowszych planów przez cały czas.</span><span class="sxs-lookup"><span data-stu-id="f3d62-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="f3d62-104">Poniższa lista zawiera najważniejsze nowe funkcje planowane dla EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="f3d62-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="f3d62-105">Większość z tych funkcji nie jest uwzględnionych w bieżącej wersji zapoznawczej, ale staje się dostępna w miarę postępu w kierunku do wersji RTM.</span><span class="sxs-lookup"><span data-stu-id="f3d62-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="f3d62-106">Przyczyną jest to, że na początku wydania koncentrujemy się na wdrażaniu planowanych [zmian](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="f3d62-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="f3d62-107">Wiele z tych istotnych zmian jest ulepszonych do EF Core.</span><span class="sxs-lookup"><span data-stu-id="f3d62-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="f3d62-108">Wiele innych jest wymaganych do odblokowania dalszych ulepszeń.</span><span class="sxs-lookup"><span data-stu-id="f3d62-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="f3d62-109">Aby zapoznać się z pełną listą poprawek i ulepszeń błędów, można zobaczyć [to zapytanie w naszym monitorze problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="f3d62-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="f3d62-110">Ulepszenia LINQ</span><span class="sxs-lookup"><span data-stu-id="f3d62-110">LINQ improvements</span></span> 

[<span data-ttu-id="f3d62-111">Śledzenie problemu #12795</span><span class="sxs-lookup"><span data-stu-id="f3d62-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="f3d62-112">Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="f3d62-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="f3d62-113">LINQ pozwala pisać zapytania bazy danych bez opuszczania wybranego języka, korzystając z informacji o typie rozbudowanym do pobierania funkcji IntelliSense i sprawdzania typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f3d62-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="f3d62-114">Jednak LINQ pozwala także pisać nieograniczoną liczbę skomplikowanych zapytań, która zawsze była bardzo ogromnym wyzwaniem dla dostawców LINQ.</span><span class="sxs-lookup"><span data-stu-id="f3d62-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="f3d62-115">W kilku pierwszych wersjach EF Core firma Microsoft rozwiązała, że w części poprzez ustalenie, jakie fragmenty zapytania można przetłumaczyć na SQL, a następnie przez umożliwienie pozostałej części zapytania w pamięci na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="f3d62-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="f3d62-116">To wykonanie po stronie klienta może być pożądane w niektórych sytuacjach, ale w wielu innych przypadkach może to spowodować niewydajne zapytania, które mogą nie zostać zidentyfikowane do czasu wdrożenia aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f3d62-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="f3d62-117">W EF Core 3,0 planujemy przeprowadzenia głębokich zmian w sposobie działania naszej implementacji LINQ i sposobach ich przetestowania.</span><span class="sxs-lookup"><span data-stu-id="f3d62-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="f3d62-118">Cele są bardziej niezawodne (na przykład w celu uniknięcia przerywania zapytań w wersjach poprawek), aby umożliwić poprawne tłumaczenie większej liczby wyrażeń na SQL, generowanie wydajnych zapytań w większej liczbie przypadków i zapobieganie wykryciu niewydajnych zapytań.</span><span class="sxs-lookup"><span data-stu-id="f3d62-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="f3d62-119">Obsługa Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f3d62-119">Cosmos DB support</span></span> 

[<span data-ttu-id="f3d62-120">Śledzenie problemu #8443</span><span class="sxs-lookup"><span data-stu-id="f3d62-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="f3d62-121">Ta funkcja jest uwzględniona w bieżącej wersji zapoznawczej, ale nie została jeszcze ukończona.</span><span class="sxs-lookup"><span data-stu-id="f3d62-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="f3d62-122">Pracujemy nad dostawcą Cosmos DB na potrzeby EF Core, aby umożliwić deweloperom zaznajomionym z modelem programowania EF łatwe kierowanie Azure Cosmos DB jako bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3d62-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="f3d62-123">Celem jest, aby niektóre zalety Cosmos DB, takich jak dystrybucja globalna, "zawsze włączone" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f3d62-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="f3d62-124">Dostawca umożliwi korzystanie z większości funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w programie Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f3d62-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="f3d62-125">Rozpocząłmy ten nakład pracy przed EF Core 2,2 i wprowadziliśmy [dostępne wersje zapoznawcze dostawcy](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="f3d62-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="f3d62-126">Nowy plan polega na dalszym tworzeniu dostawcy wraz z EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="f3d62-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="f3d62-127">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="f3d62-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="f3d62-128">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="f3d62-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="f3d62-129">Ta funkcja zostanie wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="f3d62-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f3d62-130">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="f3d62-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="f3d62-131">Począwszy od EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zamapowana na tę samą tabelę, będzie możliwe dodanie `Order` bez `OrderDetails` i wszystkich `OrderDetails` właściwości, z wyjątkiem tego, że klucz podstawowy zostanie zmapowany na kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="f3d62-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="f3d62-132">Podczas wykonywania zapytania, EF Core zostanie ustawiona `OrderDetails` na `null` , Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="f3d62-132">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="f3d62-133">C#Obsługa 8,0</span><span class="sxs-lookup"><span data-stu-id="f3d62-133">C# 8.0 support</span></span>

<span data-ttu-id="f3d62-134">[](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
Problem ze śledzeniem #12047[śledzenia problemów #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="f3d62-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="f3d62-135">Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="f3d62-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="f3d62-136">Chcemy, aby nasi klienci korzystali z niektórych [nowych funkcji w C# 8,0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) takich jak strumienie asynchroniczne (w `await foreach`tym) i typy referencyjne dopuszczające wartość null podczas korzystania z EF Core.</span><span class="sxs-lookup"><span data-stu-id="f3d62-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="f3d62-137">Odtwarzanie widoków bazy danych</span><span class="sxs-lookup"><span data-stu-id="f3d62-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="f3d62-138">Śledzenie problemu #1679</span><span class="sxs-lookup"><span data-stu-id="f3d62-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="f3d62-139">Ta funkcja nie jest uwzględniona w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="f3d62-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="f3d62-140">[Typy zapytań](xref:core/modeling/query-types)wprowadzone w EF Core 2,1 i uznawane za typy jednostek bez kluczy w EF Core 3,0 reprezentują dane, które można odczytać z bazy danych, ale nie można ich zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="f3d62-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="f3d62-141">Ta cecha sprawia, że najlepiej pasuje do widoków bazy danych w większości scenariuszy, dlatego planujemy zautomatyzować tworzenie typów jednostek bez kluczy podczas odtwarzania widoków bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f3d62-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="f3d62-142">Jednostki zbioru właściwości</span><span class="sxs-lookup"><span data-stu-id="f3d62-142">Property bag entities</span></span>

<span data-ttu-id="f3d62-143">[Śledzenie problemów #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) i [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="f3d62-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="f3d62-144">Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="f3d62-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="f3d62-145">Ta funkcja służy do włączania jednostek, które przechowują dane we właściwościach indeksowanych zamiast zwykłych właściwości, a także z możliwością używania wystąpień tej samej klasy .NET (potencjalnie jako prostej `Dictionary<string, object>`jako) do reprezentowania różnych typów jednostek w tym samym modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="f3d62-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="f3d62-146">Ta funkcja to krok do obsługi relacji wiele-do-wielu bez jednostki sprzężenia ([#1368 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), która jest jednym z najbardziej żądanych ulepszeń EF Core.</span><span class="sxs-lookup"><span data-stu-id="f3d62-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="f3d62-147">Dr 6,3 na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3d62-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="f3d62-148">Śledzenie problemu EF6 # 271</span><span class="sxs-lookup"><span data-stu-id="f3d62-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="f3d62-149">Pracę nad tą funkcją uruchomiono, ale nie jest ona uwzględniona w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="f3d62-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="f3d62-150">Firma Microsoft zdaje sobie sprawę, że w wielu istniejących aplikacjach są używane poprzednie wersje EF, a ich przenoszenie do EF Core tylko w celu wykorzystania platformy .NET Core może czasami wymagać znacznego wysiłku.</span><span class="sxs-lookup"><span data-stu-id="f3d62-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="f3d62-151">Z tego powodu będziemy dostosowywać następną wersję EF 6 do uruchamiania na platformie .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="f3d62-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="f3d62-152">Robimy to w celu ułatwienia przenoszenia istniejących aplikacji przy minimalnych zmianach.</span><span class="sxs-lookup"><span data-stu-id="f3d62-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="f3d62-153">Istnieją pewne ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="f3d62-153">There are going to be some limitations.</span></span> <span data-ttu-id="f3d62-154">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f3d62-154">For example:</span></span>
- <span data-ttu-id="f3d62-155">Będzie wymagała od nowych dostawców pracy z innymi bazami danych, poza uwzględnioną SQL Server obsługą platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3d62-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="f3d62-156">Obsługa przestrzenna z SQL Server nie będzie włączona</span><span class="sxs-lookup"><span data-stu-id="f3d62-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="f3d62-157">Należy pamiętać, że na tym etapie nie są planowane żadne nowe funkcje programu EF 6.</span><span class="sxs-lookup"><span data-stu-id="f3d62-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
