---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 199cc45e316e215b9b8e859700e4dc124de315b2
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58867986"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="8292d-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="8292d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8292d-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="8292d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="8292d-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="8292d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="8292d-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="8292d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="8292d-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="8292d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="8292d-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="8292d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="8292d-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="8292d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="8292d-109">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-110">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-110">Old behavior</span></span>**

<span data-ttu-id="8292d-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8292d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="8292d-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="8292d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="8292d-113">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-113">New behavior</span></span>**

<span data-ttu-id="8292d-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8292d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="8292d-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8292d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="8292d-116">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-116">Why</span></span>**

<span data-ttu-id="8292d-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="8292d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="8292d-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8292d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="8292d-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8292d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="8292d-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="8292d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="8292d-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="8292d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="8292d-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="8292d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="8292d-123">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-123">Mitigations</span></span>**

<span data-ttu-id="8292d-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="8292d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="8292d-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8292d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="8292d-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="8292d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="8292d-127">Ta zmiana została wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="8292d-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="8292d-128">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-128">Old behavior</span></span>**

<span data-ttu-id="8292d-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8292d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="8292d-130">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-130">New behavior</span></span>**

<span data-ttu-id="8292d-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="8292d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="8292d-132">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-132">Why</span></span>**

<span data-ttu-id="8292d-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="8292d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="8292d-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="8292d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="8292d-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="8292d-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="8292d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="8292d-137">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-137">Mitigations</span></span>**

<span data-ttu-id="8292d-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="8292d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="8292d-139">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="8292d-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="8292d-140">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="8292d-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="8292d-141">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-142">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-142">Old behavior</span></span>**

<span data-ttu-id="8292d-143">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="8292d-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="8292d-144">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-144">New behavior</span></span>**

<span data-ttu-id="8292d-145">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="8292d-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="8292d-146">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="8292d-147">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="8292d-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="8292d-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="8292d-149">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="8292d-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="8292d-150">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-150">Why</span></span>**

<span data-ttu-id="8292d-151">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody raw srting, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="8292d-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="8292d-152">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="8292d-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="8292d-153">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-153">Mitigations</span></span>**

<span data-ttu-id="8292d-154">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="8292d-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="8292d-155">Wykonanie zapytania jest rejestrowane na poziomie debugowania</span><span class="sxs-lookup"><span data-stu-id="8292d-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="8292d-156">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="8292d-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="8292d-157">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-158">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-158">Old behavior</span></span>**

<span data-ttu-id="8292d-159">Przed programem EF Core 3.0, wykonywanie zapytań i innych poleceniach, zarejestrowano w `Info` poziom.</span><span class="sxs-lookup"><span data-stu-id="8292d-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="8292d-160">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-160">New behavior</span></span>**

<span data-ttu-id="8292d-161">Począwszy od programu EF Core 3.0 rejestrowanie wykonywania polecenia/SQL jest w `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="8292d-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="8292d-162">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-162">Why</span></span>**

<span data-ttu-id="8292d-163">Ta zmiana została wprowadzona redukcji szumu w `Info` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="8292d-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="8292d-164">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-164">Mitigations</span></span>**

<span data-ttu-id="8292d-165">To zdarzenie logowania jest definiowany przez `RelationalEventId.CommandExecuting` zdarzenia o identyfikatorze 20100.</span><span class="sxs-lookup"><span data-stu-id="8292d-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="8292d-166">Do logowania SQL w `Info` poziomu ponownie, jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="8292d-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="8292d-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="8292d-168">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="8292d-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="8292d-169">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="8292d-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="8292d-170">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8292d-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="8292d-171">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-171">Old behavior</span></span>**

<span data-ttu-id="8292d-172">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8292d-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="8292d-173">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="8292d-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="8292d-174">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-174">New behavior</span></span>**

<span data-ttu-id="8292d-175">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="8292d-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="8292d-176">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-176">Why</span></span>**

<span data-ttu-id="8292d-177">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8292d-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="8292d-178">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-178">Mitigations</span></span>**

<span data-ttu-id="8292d-179">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="8292d-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="8292d-180">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="8292d-180">This can be avoided by:</span></span>
* <span data-ttu-id="8292d-181">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="8292d-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="8292d-182">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="8292d-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="8292d-183">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="8292d-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="8292d-184">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="8292d-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="8292d-185">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="8292d-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="8292d-186">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="8292d-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="8292d-187">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-188">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-188">Old behavior</span></span>**

<span data-ttu-id="8292d-189">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8292d-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="8292d-190">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-190">New behavior</span></span>**

<span data-ttu-id="8292d-191">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="8292d-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="8292d-192">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8292d-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="8292d-193">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="8292d-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="8292d-194">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-194">Why</span></span>**

<span data-ttu-id="8292d-195">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="8292d-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="8292d-196">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-196">Mitigations</span></span>**

<span data-ttu-id="8292d-197">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="8292d-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="8292d-198">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="8292d-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="8292d-199">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8292d-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="8292d-200">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="8292d-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="8292d-201">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="8292d-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="8292d-202">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="8292d-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="8292d-203">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-204">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-204">Old behavior</span></span>**

<span data-ttu-id="8292d-205">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="8292d-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="8292d-206">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-206">New behavior</span></span>**

<span data-ttu-id="8292d-207">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="8292d-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="8292d-208">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="8292d-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="8292d-209">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-209">Why</span></span>**

<span data-ttu-id="8292d-210">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8292d-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="8292d-211">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-211">Mitigations</span></span>**

<span data-ttu-id="8292d-212">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="8292d-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="8292d-213">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="8292d-214">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="8292d-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="8292d-215">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="8292d-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="8292d-216">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-217">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-217">Old behavior</span></span>**

<span data-ttu-id="8292d-218">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="8292d-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="8292d-219">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="8292d-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="8292d-220">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-220">New behavior</span></span>**

<span data-ttu-id="8292d-221">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8292d-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="8292d-222">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="8292d-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="8292d-223">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-223">Why</span></span>**

<span data-ttu-id="8292d-224">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="8292d-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="8292d-225">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="8292d-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="8292d-226">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="8292d-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="8292d-227">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-227">Mitigations</span></span>**

<span data-ttu-id="8292d-228">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="8292d-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="8292d-229">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="8292d-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="8292d-230">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="8292d-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="8292d-231">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="8292d-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="8292d-232">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="8292d-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="8292d-233">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="8292d-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="8292d-234">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="8292d-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="8292d-235">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-236">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-236">Old behavior</span></span>**

<span data-ttu-id="8292d-237">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="8292d-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="8292d-238">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-238">New behavior</span></span>**

<span data-ttu-id="8292d-239">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="8292d-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="8292d-240">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="8292d-241">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="8292d-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="8292d-242">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="8292d-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="8292d-243">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-243">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="8292d-244">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8292d-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="8292d-245">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-245">Why</span></span>**

<span data-ttu-id="8292d-246">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="8292d-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="8292d-247">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="8292d-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="8292d-248">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-248">Mitigations</span></span>**

<span data-ttu-id="8292d-249">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8292d-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="8292d-250">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="8292d-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="8292d-251">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="8292d-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="8292d-252">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-253">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-253">Old behavior</span></span>**

<span data-ttu-id="8292d-254">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="8292d-254">Consider the following model:</span></span>
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
<span data-ttu-id="8292d-255">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="8292d-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="8292d-256">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-256">New behavior</span></span>**

<span data-ttu-id="8292d-257">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="8292d-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="8292d-258">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="8292d-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="8292d-259">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-259">Mitigations</span></span>**

<span data-ttu-id="8292d-260">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="8292d-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="8292d-261">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="8292d-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="8292d-262">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="8292d-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="8292d-263">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="8292d-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="8292d-264">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-265">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-265">Old behavior</span></span>**

<span data-ttu-id="8292d-266">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="8292d-266">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="8292d-267">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8292d-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="8292d-268">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-268">New behavior</span></span>**

<span data-ttu-id="8292d-269">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="8292d-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="8292d-270">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="8292d-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="8292d-271">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-271">Why</span></span>**

<span data-ttu-id="8292d-272">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8292d-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="8292d-273">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-273">Mitigations</span></span>**

<span data-ttu-id="8292d-274">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8292d-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="8292d-275">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="8292d-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="8292d-276">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="8292d-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="8292d-277">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="8292d-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="8292d-278">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-279">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-279">Old behavior</span></span>**

<span data-ttu-id="8292d-280">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="8292d-280">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="8292d-281">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="8292d-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="8292d-282">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-282">New behavior</span></span>**

<span data-ttu-id="8292d-283">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="8292d-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="8292d-284">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-284">Why</span></span>**

<span data-ttu-id="8292d-285">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="8292d-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="8292d-286">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-286">Mitigations</span></span>**

<span data-ttu-id="8292d-287">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="8292d-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="8292d-288">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="8292d-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="8292d-289">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="8292d-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="8292d-290">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-291">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-291">Old behavior</span></span>**

<span data-ttu-id="8292d-292">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="8292d-292">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="8292d-293">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="8292d-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="8292d-294">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="8292d-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="8292d-295">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-295">New behavior</span></span>**

<span data-ttu-id="8292d-296">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="8292d-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="8292d-297">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="8292d-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="8292d-298">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-298">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**<span data-ttu-id="8292d-299">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-299">Why</span></span>**

<span data-ttu-id="8292d-300">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="8292d-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="8292d-301">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-301">Mitigations</span></span>**

<span data-ttu-id="8292d-302">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="8292d-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="8292d-303">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="8292d-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="8292d-304">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="8292d-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="8292d-305">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-306">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-306">Old behavior</span></span>**

<span data-ttu-id="8292d-307">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="8292d-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**<span data-ttu-id="8292d-308">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-308">New behavior</span></span>**

<span data-ttu-id="8292d-309">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="8292d-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="8292d-310">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-310">Why</span></span>**

<span data-ttu-id="8292d-311">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="8292d-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="8292d-312">Nowe alose zachowanie dopasowuje EF6.</span><span class="sxs-lookup"><span data-stu-id="8292d-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="8292d-313">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-313">Mitigations</span></span>**

<span data-ttu-id="8292d-314">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="8292d-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="8292d-315">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="8292d-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="8292d-316">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="8292d-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="8292d-317">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-318">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-318">Old behavior</span></span>**

<span data-ttu-id="8292d-319">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8292d-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="8292d-320">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-320">New behavior</span></span>**

<span data-ttu-id="8292d-321">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8292d-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="8292d-322">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="8292d-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="8292d-323">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-323">Why</span></span>**

<span data-ttu-id="8292d-324">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8292d-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="8292d-325">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-325">Mitigations</span></span>**

<span data-ttu-id="8292d-326">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="8292d-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="8292d-327">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="8292d-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="8292d-328">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="8292d-328">Backing fields are used by default</span></span>

[<span data-ttu-id="8292d-329">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="8292d-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="8292d-330">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8292d-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="8292d-331">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-331">Old behavior</span></span>**

<span data-ttu-id="8292d-332">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="8292d-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="8292d-333">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="8292d-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="8292d-334">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-334">New behavior</span></span>**

<span data-ttu-id="8292d-335">Począwszy od programu EF Core 3.0, jeśli pole zapasowe dla właściwości jest znany, następnie będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="8292d-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="8292d-336">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="8292d-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="8292d-337">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-337">Why</span></span>**

<span data-ttu-id="8292d-338">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="8292d-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="8292d-339">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-339">Mitigations</span></span>**

<span data-ttu-id="8292d-340">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości w interfejsie API fluent element modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="8292d-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="8292d-341">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="8292d-342">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="8292d-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="8292d-343">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="8292d-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="8292d-344">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-345">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-345">Old behavior</span></span>**

<span data-ttu-id="8292d-346">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="8292d-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="8292d-347">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="8292d-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="8292d-348">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-348">New behavior</span></span>**

<span data-ttu-id="8292d-349">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8292d-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="8292d-350">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-350">Why</span></span>**

<span data-ttu-id="8292d-351">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="8292d-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="8292d-352">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-352">Mitigations</span></span>**

<span data-ttu-id="8292d-353">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="8292d-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="8292d-354">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8292d-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="8292d-355">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="8292d-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="8292d-356">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="8292d-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="8292d-357">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-358">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-358">Old behavior</span></span>**

<span data-ttu-id="8292d-359">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8292d-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="8292d-360">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-360">New behavior</span></span>**

<span data-ttu-id="8292d-361">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="8292d-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="8292d-362">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-362">Why</span></span>**

<span data-ttu-id="8292d-363">EF Core 3.0 nie wymaga, że te usługi są w cotainer DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="8292d-364">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="8292d-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="8292d-365">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-365">Mitigations</span></span>**

<span data-ttu-id="8292d-366">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8292d-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="8292d-367">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="8292d-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="8292d-368">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="8292d-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8292d-369">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-370">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-370">Old behavior</span></span>**

<span data-ttu-id="8292d-371">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="8292d-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="8292d-372">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="8292d-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="8292d-373">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-373">New behavior</span></span>**

<span data-ttu-id="8292d-374">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="8292d-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="8292d-375">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="8292d-376">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="8292d-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="8292d-377">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="8292d-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="8292d-378">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-378">Why</span></span>**

<span data-ttu-id="8292d-379">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="8292d-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="8292d-380">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-380">Mitigations</span></span>**

<span data-ttu-id="8292d-381">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="8292d-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="8292d-382">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="8292d-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="8292d-383">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="8292d-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="8292d-384">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-385">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-385">Old behavior</span></span>**

<span data-ttu-id="8292d-386">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="8292d-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="8292d-387">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="8292d-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="8292d-388">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-388">New behavior</span></span>**

<span data-ttu-id="8292d-389">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="8292d-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="8292d-390">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-390">Why</span></span>**

<span data-ttu-id="8292d-391">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="8292d-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="8292d-392">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-392">Mitigations</span></span>**

<span data-ttu-id="8292d-393">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="8292d-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="8292d-394">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8292d-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="8292d-395">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="8292d-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="8292d-396">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="8292d-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="8292d-397">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="8292d-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="8292d-398">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-399">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-399">Old behavior</span></span>**

<span data-ttu-id="8292d-400">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8292d-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="8292d-401">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-401">New behavior</span></span>**

<span data-ttu-id="8292d-402">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="8292d-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="8292d-403">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-403">Why</span></span>**

<span data-ttu-id="8292d-404">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="8292d-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="8292d-405">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-405">Mitigations</span></span>**

<span data-ttu-id="8292d-406">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="8292d-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="8292d-407">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="8292d-407">This isn't common.</span></span>
<span data-ttu-id="8292d-408">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="8292d-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="8292d-409">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8292d-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="8292d-410">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension została scalona z</span><span class="sxs-lookup"><span data-stu-id="8292d-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="8292d-411">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="8292d-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8292d-412">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-413">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="8292d-414">przedłużono dodatkowy opcjonalny interfejs z `IDbContextOptionsExtension` Aby uniknąć konieczności istotne zmiany w interfejsie podczas cyklu wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="8292d-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="8292d-415">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-415">New behavior</span></span>**

<span data-ttu-id="8292d-416">Interfejsy są teraz scalone razem `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="8292d-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="8292d-417">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-417">Why</span></span>**

<span data-ttu-id="8292d-418">Ta zmiana została wprowadzona, ponieważ interfejsy są koncepcyjnie jeden.</span><span class="sxs-lookup"><span data-stu-id="8292d-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="8292d-419">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-419">Mitigations</span></span>**

<span data-ttu-id="8292d-420">Wszelkie implementacje `IDbContextOptionsExtension` musi zostać zaktualizowany do obsługi nowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="8292d-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="8292d-421">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="8292d-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="8292d-422">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="8292d-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="8292d-423">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-424">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-424">Old behavior</span></span>**

<span data-ttu-id="8292d-425">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="8292d-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="8292d-426">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="8292d-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="8292d-427">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="8292d-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="8292d-428">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-428">New behavior</span></span>**

<span data-ttu-id="8292d-429">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="8292d-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="8292d-430">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="8292d-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="8292d-431">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="8292d-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="8292d-432">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="8292d-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="8292d-433">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-433">Why</span></span>**

<span data-ttu-id="8292d-434">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8292d-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="8292d-435">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-435">Mitigations</span></span>**

<span data-ttu-id="8292d-436">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8292d-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="8292d-437">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="8292d-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="8292d-438">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="8292d-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="8292d-439">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-440">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-440">Old behavior</span></span>**

<span data-ttu-id="8292d-441">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="8292d-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="8292d-442">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-442">New behavior</span></span>**

<span data-ttu-id="8292d-443">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8292d-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="8292d-444">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-444">Why</span></span>**

<span data-ttu-id="8292d-445">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="8292d-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="8292d-446">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-446">Mitigations</span></span>**

<span data-ttu-id="8292d-447">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="8292d-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="8292d-448">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8292d-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="8292d-449">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="8292d-450">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="8292d-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="8292d-451">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="8292d-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="8292d-452">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-453">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-453">Old behavior</span></span>**

<span data-ttu-id="8292d-454">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została zinterpretowana w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="8292d-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="8292d-455">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="8292d-456">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="8292d-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="8292d-457">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="8292d-458">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-458">New behavior</span></span>**

<span data-ttu-id="8292d-459">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="8292d-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="8292d-460">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-460">Why</span></span>**

<span data-ttu-id="8292d-461">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="8292d-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="8292d-462">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-462">Mitigations</span></span>**

<span data-ttu-id="8292d-463">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="8292d-464">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="8292d-464">This is not common.</span></span>
<span data-ttu-id="8292d-465">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="8292d-466">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="8292d-467">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="8292d-467">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="8292d-468">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="8292d-468">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="8292d-469">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8292d-469">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="8292d-470">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-470">Old behavior</span></span>**

<span data-ttu-id="8292d-471">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8292d-471">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="8292d-472">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-472">New behavior</span></span>**

<span data-ttu-id="8292d-473">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8292d-473">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="8292d-474">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-474">Why</span></span>**

<span data-ttu-id="8292d-475">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8292d-475">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="8292d-476">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-476">Mitigations</span></span>**

<span data-ttu-id="8292d-477">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="8292d-477">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="8292d-478">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="8292d-478">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="8292d-479">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="8292d-479">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="8292d-480">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="8292d-480">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="8292d-481">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-481">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-482">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-482">Old behavior</span></span>**

<span data-ttu-id="8292d-483">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="8292d-483">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="8292d-484">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-484">New behavior</span></span>**

<span data-ttu-id="8292d-485">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8292d-485">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="8292d-486">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-486">Why</span></span>**

<span data-ttu-id="8292d-487">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8292d-487">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="8292d-488">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="8292d-488">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="8292d-489">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-489">Mitigations</span></span>**

<span data-ttu-id="8292d-490">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="8292d-490">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="8292d-491">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="8292d-491">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="8292d-492">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="8292d-492">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="8292d-493">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-493">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-494">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-494">Old behavior</span></span>**

<span data-ttu-id="8292d-495">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="8292d-495">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="8292d-496">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-496">New behavior</span></span>**

<span data-ttu-id="8292d-497">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="8292d-497">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="8292d-498">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="8292d-498">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="8292d-499">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-499">Why</span></span>**

<span data-ttu-id="8292d-500">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Includes` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8292d-500">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="8292d-501">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-501">Mitigations</span></span>**

<span data-ttu-id="8292d-502">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="8292d-502">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="8292d-503">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="8292d-503">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="8292d-504">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="8292d-504">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="8292d-505">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8292d-505">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="8292d-506">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-506">Old behavior</span></span>**

<span data-ttu-id="8292d-507">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8292d-507">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="8292d-508">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-508">New behavior</span></span>**

<span data-ttu-id="8292d-509">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8292d-509">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="8292d-510">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-510">Why</span></span>**

<span data-ttu-id="8292d-511">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="8292d-511">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="8292d-512">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-512">Mitigations</span></span>**

<span data-ttu-id="8292d-513">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="8292d-513">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="8292d-514">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="8292d-514">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="8292d-515">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="8292d-515">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="8292d-516">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-516">Old behavior</span></span>**

<span data-ttu-id="8292d-517">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="8292d-517">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="8292d-518">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-518">New behavior</span></span>**

<span data-ttu-id="8292d-519">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="8292d-519">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="8292d-520">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-520">Why</span></span>**

<span data-ttu-id="8292d-521">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="8292d-521">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="8292d-522">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-522">Mitigations</span></span>**

<span data-ttu-id="8292d-523">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="8292d-523">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8292d-524">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="8292d-524">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8292d-525">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="8292d-525">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="8292d-526">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-526">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-527">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-527">Old behavior</span></span>**

<span data-ttu-id="8292d-528">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8292d-528">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="8292d-529">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-529">New behavior</span></span>**

<span data-ttu-id="8292d-530">Wartości identyfikatora GUID są teraz sotred jako tekst.</span><span class="sxs-lookup"><span data-stu-id="8292d-530">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="8292d-531">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-531">Why</span></span>**

<span data-ttu-id="8292d-532">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="8292d-532">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="8292d-533">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="8292d-533">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="8292d-534">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-534">Mitigations</span></span>**

<span data-ttu-id="8292d-535">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8292d-535">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="8292d-536">W programie EF Core można także kontynuować, używając poprzednie zachowanie configuirng konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="8292d-536">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="8292d-537">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="8292d-537">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8292d-538">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="8292d-538">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8292d-539">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="8292d-539">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="8292d-540">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-541">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-541">Old behavior</span></span>**

<span data-ttu-id="8292d-542">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8292d-542">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="8292d-543">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="8292d-543">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="8292d-544">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-544">New behavior</span></span>**

<span data-ttu-id="8292d-545">Wartości char są teraz sotred jako tekst.</span><span class="sxs-lookup"><span data-stu-id="8292d-545">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="8292d-546">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-546">Why</span></span>**

<span data-ttu-id="8292d-547">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="8292d-547">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="8292d-548">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-548">Mitigations</span></span>**

<span data-ttu-id="8292d-549">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8292d-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="8292d-550">W programie EF Core można także kontynuować, używając poprzednie zachowanie configuirng konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="8292d-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="8292d-551">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="8292d-551">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="8292d-552">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="8292d-552">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="8292d-553">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="8292d-553">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="8292d-554">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-555">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-555">Old behavior</span></span>**

<span data-ttu-id="8292d-556">Migracja identyfikatorów były generowane przy użyciu kalendarza kultury currret inadvertantly.</span><span class="sxs-lookup"><span data-stu-id="8292d-556">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="8292d-557">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-557">New behavior</span></span>**

<span data-ttu-id="8292d-558">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="8292d-558">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="8292d-559">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-559">Why</span></span>**

<span data-ttu-id="8292d-560">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="8292d-560">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="8292d-561">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="8292d-561">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="8292d-562">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-562">Mitigations</span></span>**

<span data-ttu-id="8292d-563">Ta zmiana dotyczy wszystkich osób korzystających kalendarzowe innych niż gregoriański, gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="8292d-563">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="8292d-564">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="8292d-564">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="8292d-565">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="8292d-565">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="8292d-566">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="8292d-566">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="8292d-567">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="8292d-567">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="8292d-568">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="8292d-568">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="8292d-569">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-569">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-570">Zmiana</span><span class="sxs-lookup"><span data-stu-id="8292d-570">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="8292d-571">została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="8292d-571">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="8292d-572">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-572">Why</span></span>**

<span data-ttu-id="8292d-573">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="8292d-573">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="8292d-574">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-574">Mitigations</span></span>**

<span data-ttu-id="8292d-575">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="8292d-575">Use the new name.</span></span> <span data-ttu-id="8292d-576">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="8292d-576">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="8292d-577">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="8292d-577">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="8292d-578">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="8292d-578">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="8292d-579">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8292d-579">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="8292d-580">Stare zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-580">Old behavior</span></span>**

<span data-ttu-id="8292d-581">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="8292d-581">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="8292d-582">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-582">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="8292d-583">Nowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="8292d-583">New behavior</span></span>**

<span data-ttu-id="8292d-584">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "tego name".</span><span class="sxs-lookup"><span data-stu-id="8292d-584">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="8292d-585">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8292d-585">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="8292d-586">Dlaczego</span><span class="sxs-lookup"><span data-stu-id="8292d-586">Why</span></span>**

<span data-ttu-id="8292d-587">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego tego, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="8292d-587">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="8292d-588">Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="8292d-588">Mitigations</span></span>**

<span data-ttu-id="8292d-589">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="8292d-589">Use the new name.</span></span>
