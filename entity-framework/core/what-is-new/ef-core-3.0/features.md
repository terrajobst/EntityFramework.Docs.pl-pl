---
title: Nowe funkcje programu EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58867960"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="ec191-102">Nowych funkcji dostępnych w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="ec191-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec191-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="ec191-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ec191-104">Poniższa lista zawiera główne nowe funkcje usunięte EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="ec191-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="ec191-105">Większość tych funkcji nie są uwzględnione w bieżącej wersji zapoznawczej, ale będą dostępne w miarę wprowadzania postępów w wersji RTM.</span><span class="sxs-lookup"><span data-stu-id="ec191-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="ec191-106">Przyczyną jest to, że na początku wersji koncentrujemy się na implementowaniu planowane [istotne zmiany w](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="ec191-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="ec191-107">Wiele z tych przełomowe zmiany dotyczą poprawy do programu EF Core własnych.</span><span class="sxs-lookup"><span data-stu-id="ec191-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="ec191-108">Wiele innych są wymagane, aby odblokować dalsze ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="ec191-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="ec191-109">Aby uzyskać pełną listę poprawek usterek i ulepszeń, widać [tego zapytania w naszym narzędzie do śledzenia problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="ec191-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="ec191-110">Ulepszenia zapytań LINQ</span><span class="sxs-lookup"><span data-stu-id="ec191-110">LINQ improvements</span></span> 

[<span data-ttu-id="ec191-111">Śledzenie problem #12795</span><span class="sxs-lookup"><span data-stu-id="ec191-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="ec191-112">Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ec191-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="ec191-113">LINQ umożliwia tworzenie zapytań bazy danych bez opuszczania usługi z wybranego języka, wykorzystując zaawansowane wpisz informacje funkcji IntelliSense i sprawdzanie typów w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ec191-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="ec191-114">Ale LINQ można również napisać nieograniczonej liczby zapytań skomplikowane, a który zawsze było ogromnym wyzwaniom dla dostawców LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec191-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="ec191-115">W pierwszym kilka wersji programu EF Core możemy rozwiązać, w części za ustalenie, jakie części zapytania mogą być tłumaczone do bazy danych SQL, a następnie, umożliwiając pozostałe kwerenda do wykonania w pamięci na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ec191-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="ec191-116">Wykonanie tego po stronie klienta może być pożądana w niektórych sytuacjach, ale w innych przypadkach może to skutkować nieefektywne zapytań, które nie mogą być określane, dopóki aplikacja jest wdrażana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="ec191-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="ec191-117">W programie EF Core 3.0 planujemy wprowadzić głęboki zmiany sposobu działania naszej implementacji LINQ i jak możemy przetestować.</span><span class="sxs-lookup"><span data-stu-id="ec191-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="ec191-118">Cele są aby działał on bardziej niezawodnie (na przykład, aby uniknąć przerywanie zapytania w wersjach poprawki), aby włączyć Translacja wyrażeń więcej poprawnie w bazie SQL, można wygenerować wydajne zapytania w większości przypadków i uniemożliwić nieefektywne zapytania przechodząc niewykryte.</span><span class="sxs-lookup"><span data-stu-id="ec191-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="ec191-119">Obsługa usługi cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ec191-119">Cosmos DB support</span></span> 

[<span data-ttu-id="ec191-120">Śledzenie problem #8443</span><span class="sxs-lookup"><span data-stu-id="ec191-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="ec191-121">Ta funkcja jest dołączona w bieżącej wersji zapoznawczej, ale nie została jeszcze ukończona.</span><span class="sxs-lookup"><span data-stu-id="ec191-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="ec191-122">Pracujemy nad dostawcę usługi Cosmos DB dla platformy EF Core, aby umożliwić deweloperom zapoznać się z modelem programistyczne EF łatwo Kieruj usługi Azure Cosmos DB jako bazę danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec191-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="ec191-123">Celem jest zapewnienie niektóre korzyści wynikające z usługi Cosmos DB, takie jak dystrybucji globalnej, "zawsze włączone" dostępność, elastyczną skalowalność i małego opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ec191-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="ec191-124">Dostawca umożliwi większość funkcji EF Core, takie jak automatyczna zmiana, śledzenie, LINQ i konwersji wartości, przy użyciu interfejsu SQL API w usłudze Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ec191-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="ec191-125">Rozpoczęliśmy ten nakład pracy przed programem EF Core 2.2, i [Wprowadziliśmy pewne wersji dostawcy dostępne w wersji zapoznawczej](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="ec191-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="ec191-126">Nowy plan jest, aby kontynuować, tworzenie dostawcy wraz z programem EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="ec191-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="ec191-127">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="ec191-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="ec191-128">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="ec191-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="ec191-129">Ta funkcja zostaną wprowadzone w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ec191-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ec191-130">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="ec191-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="ec191-131">Począwszy od programu EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, będzie można dodać `Order` bez `OrderDetails` i wszystkie `OrderDetails` będą zamapowane właściwości, z wyjątkiem klucza podstawowego kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="ec191-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties except the primary key will be mapped to nullable columns.</span></span>
<span data-ttu-id="ec191-132">Podczas wykonywania zapytań programu EF Core zostanie ustawiony `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="ec191-132">When querying EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="ec191-133">C#Obsługa 8.0</span><span class="sxs-lookup"><span data-stu-id="ec191-133">C# 8.0 support</span></span>

<span data-ttu-id="ec191-134">[Śledzenie problem #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[śledzenia problemu #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="ec191-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="ec191-135">Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ec191-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="ec191-136">Chcemy, aby klienci mogą korzystać z niektórych [nowe funkcje zostaną dodane w C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) strumieni asynchronicznych, takich jak (w tym `await foreach`) i typy referencyjne dopuszczającego wartość null podczas korzystania z programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec191-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="ec191-137">Odtwarzanie widoki bazy danych</span><span class="sxs-lookup"><span data-stu-id="ec191-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="ec191-138">Śledzenie problem #1679</span><span class="sxs-lookup"><span data-stu-id="ec191-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="ec191-139">Ta funkcja nie jest zawarty w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ec191-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="ec191-140">[Typy zapytań](xref:core/modeling/query-types), wprowadzone w programie EF Core 2.1 i uznawane za typów jednostek, bez kluczy w EF Core 3.0 to reprezentują dane, które mogą być odczytywane z bazy danych, ale nie można zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="ec191-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="ec191-141">Cecha ta sprawia, że ich doskonale sprawdzą się w przypadku widoków bazy danych w większości przypadków, więc planujemy do zautomatyzowania tworzenia typów jednostek, bez kluczy podczas odtwarzania widoki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ec191-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="ec191-142">Jednostki zbioru właściwości</span><span class="sxs-lookup"><span data-stu-id="ec191-142">Property bag entities</span></span>

<span data-ttu-id="ec191-143">[Śledzenie problem #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) i [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="ec191-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="ec191-144">Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ec191-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="ec191-145">Ta funkcja jest zapewnienie jednostek, które przechowują dane w właściwości indeksowanych zamiast regularnego właściwości, a także o będzie mógł korzystać z wystąpień klasy .NET (potencjalnie coś tak proste, jak `Dictionary<string, object>`) do reprezentowania typów różnych jednostek w tym samym modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec191-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="ec191-146">Ta funkcja jest kamień przechodzenia krok po kroku do obsługi relacji wiele do wielu, bez jednostki sprzężenia ([wystawiać #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), które jest jednym z najbardziej pożądanych ulepszenia dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec191-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="ec191-147">EF 6.3 na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="ec191-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="ec191-148">Śledzenie EF6 problem #271</span><span class="sxs-lookup"><span data-stu-id="ec191-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="ec191-149">Rozpoczęto pracę na temat tej funkcji, ale nie jest zawarty w bieżącej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ec191-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="ec191-150">Rozumiemy, że wiele istniejących aplikacji używać starszych wersji programu EF i że przenoszenia ich do programu EF Core tylko po to, aby móc korzystać z platformy .NET Core może czasem wymagać znaczących nakładów pracy.</span><span class="sxs-lookup"><span data-stu-id="ec191-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="ec191-151">Z tego powodu firma Microsoft będzie można dostosowanie Następna wersja programu EF 6 do uruchamiania na .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="ec191-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="ec191-152">Robimy to w celu ułatwienia przenoszenia istniejących aplikacji przy minimalnych zmianach.</span><span class="sxs-lookup"><span data-stu-id="ec191-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="ec191-153">Czy powstaną pewne ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="ec191-153">There are going to be some limitations.</span></span> <span data-ttu-id="ec191-154">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ec191-154">For example:</span></span>
- <span data-ttu-id="ec191-155">Będzie wymagać nowego dostawcy do pracy z innymi bazami danych oprócz uwzględnione Obsługa programu SQL Server na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="ec191-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="ec191-156">Nie można włączyć obsługi przestrzennych z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="ec191-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="ec191-157">Należy zauważyć, że nie istnieją żadne nowe funkcje, w tym momencie zaplanowała programów EF 6.</span><span class="sxs-lookup"><span data-stu-id="ec191-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
