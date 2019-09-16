---
title: Nowe funkcje w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005556"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="0bca3-102">Nowe funkcje zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="0bca3-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="0bca3-103">Poniższa lista zawiera najważniejsze nowe funkcje planowane dla EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="0bca3-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="0bca3-104">EF Core 3,0 to główna wersja i zawiera także liczne istotne [zmiany](xref:core/what-is-new/ef-core-3.0/breaking-changes), które są ULEPSZENIAMI interfejsu API, które mogą mieć negatywny wpływ na istniejące aplikacje.</span><span class="sxs-lookup"><span data-stu-id="0bca3-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="0bca3-105">Ulepszenia LINQ</span><span class="sxs-lookup"><span data-stu-id="0bca3-105">LINQ improvements</span></span> 

<span data-ttu-id="0bca3-106">LINQ pozwala pisać zapytania bazy danych bez opuszczania wybranego języka, korzystając z informacji o typie rozbudowanym do oferowania funkcji IntelliSense i sprawdzania typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="0bca3-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="0bca3-107">Jednak LINQ pozwala także pisać nieograniczoną liczbę skomplikowanych kwerend zawierających dowolne wyrażenia (wywołania metod lub operacje).</span><span class="sxs-lookup"><span data-stu-id="0bca3-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="0bca3-108">Obsługa wszystkich tych kombinacji zawsze było znaczącym wyzwaniem dla dostawców LINQ.</span><span class="sxs-lookup"><span data-stu-id="0bca3-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="0bca3-109">W EF Core 3,0 wprowadziliśmy nasze implementację LINQ, aby umożliwić tłumaczenie większej liczby wyrażeń na SQL, generowanie wydajnych zapytań w większej liczbie przypadków, aby zapobiec wykryciu niewydajnych zapytań i ułatwić stopniowe wprowadzanie nowych zapytań możliwości i wydajność improvementswithout istniejące aplikacje i dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="0bca3-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="0bca3-110">Ocena klienta</span><span class="sxs-lookup"><span data-stu-id="0bca3-110">Client evaluation</span></span>

<span data-ttu-id="0bca3-111">Główna zmiana projektu w EF Core 3,0 musi zrobić, w jaki sposób obsługuje wyrażenia LINQ, których nie można przetłumaczyć na SQL lub parametry:</span><span class="sxs-lookup"><span data-stu-id="0bca3-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="0bca3-112">W kilku pierwszych wersjach EF Core po prostu ustalić, jakie fragmenty zapytania można przetłumaczyć na SQL i wykonać pozostałe zapytanie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="0bca3-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="0bca3-113">Ten typ wykonywania po stronie klienta może być pożądany w niektórych sytuacjach, ale w wielu innych przypadkach może to spowodować niewydajne zapytania.</span><span class="sxs-lookup"><span data-stu-id="0bca3-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="0bca3-114">Na przykład jeśli EF Core 2,2 nie może przetłumaczyć predykatu `Where()` w wywołaniu, wykonał instrukcję SQL bez filtru, Odczytaj wszystkie wiersze z bazy danych, a następnie przefiltruje je w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0bca3-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="0bca3-115">To może być akceptowalne, jeśli baza danych zawiera niewielką liczbę wierszy, ale może powodować znaczne problemy z wydajnością lub nawet niepowodzenie aplikacji, jeśli baza danych zawiera dużą liczbę lub kilka wierszy.</span><span class="sxs-lookup"><span data-stu-id="0bca3-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="0bca3-116">W EF Core 3,0 ograniczamy ocenę klienta tylko w przypadku projekcji najwyższego poziomu (ostatnie wywołanie do `Select()`).</span><span class="sxs-lookup"><span data-stu-id="0bca3-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="0bca3-117">Gdy EF Core 3,0 wykrywa wyrażenia, których nie można przetłumaczyć w innym miejscu w zapytaniu, zgłasza wyjątek czasu wykonania.</span><span class="sxs-lookup"><span data-stu-id="0bca3-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="0bca3-118">Obsługa Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0bca3-118">Cosmos DB support</span></span> 

<span data-ttu-id="0bca3-119">Dostawca Cosmos DB dla EF Core umożliwia deweloperom znającym model programu EF programowanie, aby łatwo kierować Azure Cosmos DB jako bazę danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0bca3-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="0bca3-120">Celem jest, aby niektóre zalety Cosmos DB, takich jak dystrybucja globalna, "zawsze włączone" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="0bca3-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="0bca3-121">Dostawca umożliwi korzystanie z większości funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w programie Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0bca3-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="0bca3-122">C#Obsługa 8,0</span><span class="sxs-lookup"><span data-stu-id="0bca3-122">C# 8.0 support</span></span>

<span data-ttu-id="0bca3-123">EF Core 3,0 wykorzystuje niektóre nowe funkcje w C# 8,0:</span><span class="sxs-lookup"><span data-stu-id="0bca3-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="0bca3-124">Strumienie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="0bca3-124">Asynchronous streams</span></span>

<span data-ttu-id="0bca3-125">Asynchroniczne wyniki zapytania są teraz udostępniane przy użyciu nowego interfejsu `IAsyncEnumerable<T>` standardowego i mogą być używane przy `await foreach`użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="0bca3-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="0bca3-126">Typy referencyjne dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="0bca3-126">Nullable reference types</span></span> 

<span data-ttu-id="0bca3-127">Gdy ta nowa funkcja jest włączona w kodzie, EF Core może przyczynić się do wartości null właściwości typów refrence (typów pierwotnych, takich jak ciąg lub właściwości nawigacji) w celu określenia wartości null kolumn i relacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0bca3-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="0bca3-128">Przejmowanie</span><span class="sxs-lookup"><span data-stu-id="0bca3-128">Interception</span></span>

<span data-ttu-id="0bca3-129">Nowy interfejs API przechwytywania w EF Core 3,0 umożliwia programowo przestrzeganie i modyfikowanie wyniku operacji bazy danych niskiego poziomu, które występują w ramach normalnej operacji EF Core, takich jak otwieranie połączeń, transakcje initating i wykonywanie poleceń.</span><span class="sxs-lookup"><span data-stu-id="0bca3-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="0bca3-130">Odtwarzanie widoków bazy danych</span><span class="sxs-lookup"><span data-stu-id="0bca3-130">Reverse engineering of database views</span></span>

<span data-ttu-id="0bca3-131">Typy jednostek bez kluczy (wcześniej znanych jako [typy zapytań](xref:core/modeling/query-types)) reprezentują dane, które można odczytać z bazy danych, ale nie można ich zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="0bca3-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="0bca3-132">Ta cecha sprawia, że są one doskonałą zaletą mapowania widoków bazy danych w większości scenariuszy, dzięki czemu tworzymy typy jednostek bez kluczy podczas odtwarzania widoków bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0bca3-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="0bca3-133">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="0bca3-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="0bca3-134">Począwszy od EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zamapowana na tę samą tabelę, będzie możliwe dodanie `Order` bez `OrderDetails` i wszystkich `OrderDetails` właściwości, z wyjątkiem tego, że klucz podstawowy zostanie zmapowany na kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="0bca3-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="0bca3-135">Podczas wykonywania zapytania, EF Core zostanie ustawiona `OrderDetails` na `null` , Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="0bca3-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="0bca3-136">Dr 6,3 na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="0bca3-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="0bca3-137">Firma Microsoft zdaje sobie sprawę, że w wielu istniejących aplikacjach są używane poprzednie wersje EF, a ich przenoszenie do EF Core tylko w celu wykorzystania platformy .NET Core może czasami wymagać znacznego wysiłku.</span><span class="sxs-lookup"><span data-stu-id="0bca3-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="0bca3-138">Z tego powodu została włączona wersja newewst programu EF 6 do uruchamiania na platformie .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="0bca3-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="0bca3-139">Istnieją pewne ograniczenia, na przykład:</span><span class="sxs-lookup"><span data-stu-id="0bca3-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="0bca3-140">Nowi dostawcy muszą współpracować z platformą .NET Core</span><span class="sxs-lookup"><span data-stu-id="0bca3-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="0bca3-141">Obsługa przestrzenna z SQL Server nie będzie włączona</span><span class="sxs-lookup"><span data-stu-id="0bca3-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="0bca3-142">Funkcje odroczone</span><span class="sxs-lookup"><span data-stu-id="0bca3-142">Postponed features</span></span>

<span data-ttu-id="0bca3-143">Niektóre funkcje początkowo planowane dla EF Core 3,0 zostały odroczone do przyszłych wersji:</span><span class="sxs-lookup"><span data-stu-id="0bca3-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="0bca3-144">Możliwość ingore części modelu w migracjach, śledzonych przez [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="0bca3-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="0bca3-145">Jednostki zbioru właściwości, śledzone przez dwa oddzielne problemy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) informacji o jednostkach typu udostępnionego i [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o obsłudze mapowania właściwości indeksowanych.</span><span class="sxs-lookup"><span data-stu-id="0bca3-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
