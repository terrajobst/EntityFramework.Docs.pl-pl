---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619262"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="a8520-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="a8520-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8520-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="a8520-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="a8520-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="a8520-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="a8520-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="a8520-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="a8520-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="a8520-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="a8520-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="a8520-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="a8520-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="a8520-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="a8520-109">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-110">**Old behavior**</span></span>

<span data-ttu-id="a8520-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a8520-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="a8520-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a8520-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="a8520-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-113">**New behavior**</span></span>

<span data-ttu-id="a8520-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a8520-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="a8520-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a8520-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="a8520-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-116">**Why**</span></span>

<span data-ttu-id="a8520-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="a8520-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="a8520-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a8520-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="a8520-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a8520-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="a8520-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="a8520-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="a8520-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="a8520-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="a8520-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="a8520-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="a8520-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-123">**Mitigations**</span></span>

<span data-ttu-id="a8520-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="a8520-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="a8520-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8520-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="a8520-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="a8520-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="a8520-127">Ta zmiana została wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="a8520-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="a8520-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-128">**Old behavior**</span></span>

<span data-ttu-id="a8520-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a8520-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="a8520-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-130">**New behavior**</span></span>

<span data-ttu-id="a8520-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="a8520-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="a8520-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-132">**Why**</span></span>

<span data-ttu-id="a8520-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="a8520-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="a8520-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="a8520-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="a8520-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="a8520-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="a8520-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="a8520-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-137">**Mitigations**</span></span>

<span data-ttu-id="a8520-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="a8520-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="a8520-139">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="a8520-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="a8520-140">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="a8520-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="a8520-141">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-142">**Old behavior**</span></span>

<span data-ttu-id="a8520-143">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="a8520-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="a8520-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-144">**New behavior**</span></span>

<span data-ttu-id="a8520-145">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a8520-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="a8520-146">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="a8520-147">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="a8520-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="a8520-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="a8520-149">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="a8520-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="a8520-150">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-150">**Why**</span></span>

<span data-ttu-id="a8520-151">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody raw srting, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="a8520-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="a8520-152">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="a8520-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="a8520-153">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-153">**Mitigations**</span></span>

<span data-ttu-id="a8520-154">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="a8520-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="a8520-155">Wykonanie zapytania jest rejestrowane na poziomie debugowania</span><span class="sxs-lookup"><span data-stu-id="a8520-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="a8520-156">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="a8520-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="a8520-157">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-158">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-158">**Old behavior**</span></span>

<span data-ttu-id="a8520-159">Przed programem EF Core 3.0, wykonywanie zapytań i innych poleceniach, zarejestrowano w `Info` poziom.</span><span class="sxs-lookup"><span data-stu-id="a8520-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="a8520-160">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-160">**New behavior**</span></span>

<span data-ttu-id="a8520-161">Począwszy od programu EF Core 3.0 rejestrowanie wykonywania polecenia/SQL jest w `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="a8520-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="a8520-162">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-162">**Why**</span></span>

<span data-ttu-id="a8520-163">Ta zmiana została wprowadzona redukcji szumu w `Info` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="a8520-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="a8520-164">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-164">**Mitigations**</span></span>

<span data-ttu-id="a8520-165">To zdarzenie logowania jest definiowany przez `RelationalEventId.CommandExecuting` zdarzenia o identyfikatorze 20100.</span><span class="sxs-lookup"><span data-stu-id="a8520-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="a8520-166">Do logowania SQL w `Info` poziomu ponownie, jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="a8520-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="a8520-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="a8520-168">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="a8520-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="a8520-169">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="a8520-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="a8520-170">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="a8520-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a8520-171">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-171">**Old behavior**</span></span>

<span data-ttu-id="a8520-172">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a8520-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="a8520-173">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="a8520-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="a8520-174">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-174">**New behavior**</span></span>

<span data-ttu-id="a8520-175">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="a8520-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="a8520-176">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-176">**Why**</span></span>

<span data-ttu-id="a8520-177">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a8520-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="a8520-178">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-178">**Mitigations**</span></span>

<span data-ttu-id="a8520-179">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="a8520-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="a8520-180">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="a8520-180">This can be avoided by:</span></span>
* <span data-ttu-id="a8520-181">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="a8520-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="a8520-182">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a8520-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="a8520-183">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="a8520-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="a8520-184">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="a8520-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="a8520-185">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="a8520-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="a8520-186">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="a8520-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="a8520-187">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-188">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-188">**Old behavior**</span></span>

<span data-ttu-id="a8520-189">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a8520-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="a8520-190">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-190">**New behavior**</span></span>

<span data-ttu-id="a8520-191">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="a8520-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="a8520-192">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a8520-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="a8520-193">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="a8520-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="a8520-194">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-194">**Why**</span></span>

<span data-ttu-id="a8520-195">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="a8520-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="a8520-196">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-196">**Mitigations**</span></span>

<span data-ttu-id="a8520-197">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a8520-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="a8520-198">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="a8520-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="a8520-199">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a8520-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="a8520-200">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="a8520-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="a8520-201">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="a8520-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="a8520-202">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="a8520-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="a8520-203">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-204">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-204">**Old behavior**</span></span>

<span data-ttu-id="a8520-205">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="a8520-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="a8520-206">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-206">**New behavior**</span></span>

<span data-ttu-id="a8520-207">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="a8520-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="a8520-208">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a8520-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="a8520-209">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-209">**Why**</span></span>

<span data-ttu-id="a8520-210">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a8520-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="a8520-211">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-211">**Mitigations**</span></span>

<span data-ttu-id="a8520-212">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="a8520-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="a8520-213">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="a8520-214">DeleteBehavior.Restrict ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="a8520-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="a8520-215">Śledzenie problem #12661</span><span class="sxs-lookup"><span data-stu-id="a8520-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="a8520-216">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 5.</span><span class="sxs-lookup"><span data-stu-id="a8520-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="a8520-217">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-217">**Old behavior**</span></span>

<span data-ttu-id="a8520-218">Przed 3.0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych o `Restrict` semantykę, ale także zmienione wewnętrznego naprawy w taki sposób,-oczywiste.</span><span class="sxs-lookup"><span data-stu-id="a8520-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="a8520-219">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-219">**New behavior**</span></span>

<span data-ttu-id="a8520-220">Począwszy od 3.0 `DeleteBehavior.Restrict` gwarantuje, że klucze obce są tworzone za pomocą `Restrict` semantyki — czyli nie kaskady; throw na naruszenie ograniczenia — bez również wpływ na wewnętrzny naprawy EF.</span><span class="sxs-lookup"><span data-stu-id="a8520-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="a8520-221">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-221">**Why**</span></span>

<span data-ttu-id="a8520-222">Ta zmiana została wprowadzona w celu usprawnienie obsługi przy użyciu `DeleteBehavior` w sposób intuicyjny, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="a8520-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="a8520-223">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-223">**Mitigations**</span></span>

<span data-ttu-id="a8520-224">Poprzednie zachowanie można przywrócić za pomocą `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="a8520-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="a8520-225">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="a8520-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="a8520-226">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="a8520-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="a8520-227">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-228">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-228">**Old behavior**</span></span>

<span data-ttu-id="a8520-229">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="a8520-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="a8520-230">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="a8520-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="a8520-231">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-231">**New behavior**</span></span>

<span data-ttu-id="a8520-232">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a8520-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="a8520-233">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="a8520-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="a8520-234">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-234">**Why**</span></span>

<span data-ttu-id="a8520-235">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="a8520-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="a8520-236">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="a8520-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="a8520-237">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="a8520-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="a8520-238">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-238">**Mitigations**</span></span>

<span data-ttu-id="a8520-239">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="a8520-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="a8520-240">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="a8520-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="a8520-241">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="a8520-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="a8520-242">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="a8520-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="a8520-243">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="a8520-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="a8520-244">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="a8520-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="a8520-245">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="a8520-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="a8520-246">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-247">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-247">**Old behavior**</span></span>

<span data-ttu-id="a8520-248">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="a8520-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="a8520-249">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-249">**New behavior**</span></span>

<span data-ttu-id="a8520-250">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="a8520-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="a8520-251">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="a8520-252">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="a8520-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="a8520-253">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="a8520-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="a8520-254">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-254">For example:</span></span>

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

<span data-ttu-id="a8520-255">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="a8520-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="a8520-256">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-256">**Why**</span></span>

<span data-ttu-id="a8520-257">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="a8520-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="a8520-258">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="a8520-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="a8520-259">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-259">**Mitigations**</span></span>

<span data-ttu-id="a8520-260">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a8520-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="a8520-261">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="a8520-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="a8520-262">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="a8520-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="a8520-263">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-264">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-264">**Old behavior**</span></span>

<span data-ttu-id="a8520-265">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="a8520-265">Consider the following model:</span></span>
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
<span data-ttu-id="a8520-266">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="a8520-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="a8520-267">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-267">**New behavior**</span></span>

<span data-ttu-id="a8520-268">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="a8520-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="a8520-269">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="a8520-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="a8520-270">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-270">**Mitigations**</span></span>

<span data-ttu-id="a8520-271">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="a8520-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="a8520-272">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="a8520-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="a8520-273">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="a8520-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="a8520-274">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="a8520-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="a8520-275">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-276">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-276">**Old behavior**</span></span>

<span data-ttu-id="a8520-277">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="a8520-277">Consider the following model:</span></span>
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
<span data-ttu-id="a8520-278">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="a8520-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="a8520-279">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-279">**New behavior**</span></span>

<span data-ttu-id="a8520-280">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="a8520-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="a8520-281">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="a8520-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="a8520-282">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-282">**Why**</span></span>

<span data-ttu-id="a8520-283">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a8520-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="a8520-284">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-284">**Mitigations**</span></span>

<span data-ttu-id="a8520-285">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="a8520-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="a8520-286">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="a8520-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="a8520-287">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="a8520-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="a8520-288">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="a8520-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="a8520-289">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-290">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-290">**Old behavior**</span></span>

<span data-ttu-id="a8520-291">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="a8520-291">Consider the following model:</span></span>
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

<span data-ttu-id="a8520-292">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a8520-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="a8520-293">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-293">**New behavior**</span></span>

<span data-ttu-id="a8520-294">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="a8520-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="a8520-295">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-295">**Why**</span></span>

<span data-ttu-id="a8520-296">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="a8520-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="a8520-297">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-297">**Mitigations**</span></span>

<span data-ttu-id="a8520-298">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="a8520-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="a8520-299">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="a8520-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="a8520-300">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="a8520-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="a8520-301">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-302">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-302">**Old behavior**</span></span>

<span data-ttu-id="a8520-303">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="a8520-303">Consider the following model:</span></span>
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
<span data-ttu-id="a8520-304">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="a8520-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="a8520-305">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="a8520-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="a8520-306">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-306">**New behavior**</span></span>

<span data-ttu-id="a8520-307">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="a8520-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="a8520-308">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="a8520-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="a8520-309">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-309">For example:</span></span>

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

<span data-ttu-id="a8520-310">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-310">**Why**</span></span>

<span data-ttu-id="a8520-311">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="a8520-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="a8520-312">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-312">**Mitigations**</span></span>

<span data-ttu-id="a8520-313">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="a8520-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="a8520-314">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="a8520-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="a8520-315">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="a8520-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="a8520-316">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-317">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-317">**Old behavior**</span></span>

<span data-ttu-id="a8520-318">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="a8520-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="a8520-319">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-319">**New behavior**</span></span>

<span data-ttu-id="a8520-320">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="a8520-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="a8520-321">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-321">**Why**</span></span>

<span data-ttu-id="a8520-322">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="a8520-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="a8520-323">Nowe alose zachowanie dopasowuje EF6.</span><span class="sxs-lookup"><span data-stu-id="a8520-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="a8520-324">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-324">**Mitigations**</span></span>

<span data-ttu-id="a8520-325">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="a8520-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="a8520-326">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="a8520-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="a8520-327">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="a8520-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="a8520-328">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-329">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-329">**Old behavior**</span></span>

<span data-ttu-id="a8520-330">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a8520-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="a8520-331">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-331">**New behavior**</span></span>

<span data-ttu-id="a8520-332">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a8520-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="a8520-333">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="a8520-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="a8520-334">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-334">**Why**</span></span>

<span data-ttu-id="a8520-335">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a8520-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="a8520-336">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-336">**Mitigations**</span></span>

<span data-ttu-id="a8520-337">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="a8520-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="a8520-338">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="a8520-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="a8520-339">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="a8520-339">Backing fields are used by default</span></span>

[<span data-ttu-id="a8520-340">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="a8520-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="a8520-341">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="a8520-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a8520-342">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-342">**Old behavior**</span></span>

<span data-ttu-id="a8520-343">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="a8520-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="a8520-344">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="a8520-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="a8520-345">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-345">**New behavior**</span></span>

<span data-ttu-id="a8520-346">Począwszy od programu EF Core 3.0, jeśli pole zapasowe dla właściwości jest znany, następnie będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="a8520-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="a8520-347">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="a8520-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="a8520-348">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-348">**Why**</span></span>

<span data-ttu-id="a8520-349">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="a8520-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="a8520-350">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-350">**Mitigations**</span></span>

<span data-ttu-id="a8520-351">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości w interfejsie API fluent element modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="a8520-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="a8520-352">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="a8520-353">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="a8520-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="a8520-354">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="a8520-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="a8520-355">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-356">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-356">**Old behavior**</span></span>

<span data-ttu-id="a8520-357">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="a8520-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="a8520-358">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="a8520-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="a8520-359">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-359">**New behavior**</span></span>

<span data-ttu-id="a8520-360">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a8520-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="a8520-361">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-361">**Why**</span></span>

<span data-ttu-id="a8520-362">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="a8520-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="a8520-363">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-363">**Mitigations**</span></span>

<span data-ttu-id="a8520-364">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="a8520-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="a8520-365">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a8520-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="a8520-366">Właściwość tylko do pola nazwy powinny pasować do nazwy pola</span><span class="sxs-lookup"><span data-stu-id="a8520-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="a8520-367">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-368">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-368">**Old behavior**</span></span>

<span data-ttu-id="a8520-369">Przed programem EF Core 3.0 to właściwość może być określony przez wartość ciągu i jeśli żadnej właściwości o tej nazwie został znaleziony na typ CLR programu EF Core będzie spróbuj dopasować go do pola przy użyciu reguł Konwencję.</span><span class="sxs-lookup"><span data-stu-id="a8520-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="a8520-370">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-370">**New behavior**</span></span>

<span data-ttu-id="a8520-371">Począwszy od programu EF Core 3.0 to właściwość tylko do pola musi być zgodny nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="a8520-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="a8520-372">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-372">**Why**</span></span>

<span data-ttu-id="a8520-373">Ta zmiana została wprowadzona w celu uniknięcia przy użyciu tego samego pola na dwie właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości tylko do pól takie same jak właściwości zamapowanych do właściwości aparatu CLR.</span><span class="sxs-lookup"><span data-stu-id="a8520-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="a8520-374">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-374">**Mitigations**</span></span>

<span data-ttu-id="a8520-375">Właściwości tylko do pola musi mieć nazwę taka sama jak pola, które są mapowane.</span><span class="sxs-lookup"><span data-stu-id="a8520-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="a8520-376">W nowszej wersji zapoznawczej programu EF Core 3.0 planujemy ponownie włączyć konfigurowanie jawnie nazwę pola, która jest inna niż nazwa właściwości:</span><span class="sxs-lookup"><span data-stu-id="a8520-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="a8520-377">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="a8520-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="a8520-378">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="a8520-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="a8520-379">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-380">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-380">**Old behavior**</span></span>

<span data-ttu-id="a8520-381">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a8520-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="a8520-382">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-382">**New behavior**</span></span>

<span data-ttu-id="a8520-383">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="a8520-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="a8520-384">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-384">**Why**</span></span>

<span data-ttu-id="a8520-385">EF Core 3.0 nie wymaga, że te usługi są w cotainer DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="a8520-386">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="a8520-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="a8520-387">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-387">**Mitigations**</span></span>

<span data-ttu-id="a8520-388">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a8520-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="a8520-389">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="a8520-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="a8520-390">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="a8520-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a8520-391">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-392">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-392">**Old behavior**</span></span>

<span data-ttu-id="a8520-393">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="a8520-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="a8520-394">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="a8520-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="a8520-395">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-395">**New behavior**</span></span>

<span data-ttu-id="a8520-396">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="a8520-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="a8520-397">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="a8520-398">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="a8520-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="a8520-399">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="a8520-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="a8520-400">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-400">**Why**</span></span>

<span data-ttu-id="a8520-401">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="a8520-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="a8520-402">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-402">**Mitigations**</span></span>

<span data-ttu-id="a8520-403">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="a8520-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="a8520-404">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="a8520-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="a8520-405">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="a8520-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="a8520-406">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-407">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-407">**Old behavior**</span></span>

<span data-ttu-id="a8520-408">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="a8520-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="a8520-409">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="a8520-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="a8520-410">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-410">**New behavior**</span></span>

<span data-ttu-id="a8520-411">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="a8520-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="a8520-412">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-412">**Why**</span></span>

<span data-ttu-id="a8520-413">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="a8520-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="a8520-414">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-414">**Mitigations**</span></span>

<span data-ttu-id="a8520-415">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="a8520-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="a8520-416">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a8520-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="a8520-417">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="a8520-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="a8520-418">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="a8520-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="a8520-419">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="a8520-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="a8520-420">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-421">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-421">**Old behavior**</span></span>

<span data-ttu-id="a8520-422">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a8520-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="a8520-423">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-423">**New behavior**</span></span>

<span data-ttu-id="a8520-424">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="a8520-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="a8520-425">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-425">**Why**</span></span>

<span data-ttu-id="a8520-426">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="a8520-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="a8520-427">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-427">**Mitigations**</span></span>

<span data-ttu-id="a8520-428">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="a8520-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="a8520-429">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="a8520-429">This isn't common.</span></span>
<span data-ttu-id="a8520-430">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="a8520-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="a8520-431">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="a8520-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="a8520-432">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension została scalona z</span><span class="sxs-lookup"><span data-stu-id="a8520-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="a8520-433">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="a8520-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a8520-434">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-435">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-435">**Old behavior**</span></span>

<span data-ttu-id="a8520-436">`IDbContextOptionsExtensionWithDebugInfo` przedłużono dodatkowy opcjonalny interfejs z `IDbContextOptionsExtension` Aby uniknąć konieczności istotne zmiany w interfejsie podczas cyklu wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="a8520-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="a8520-437">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-437">**New behavior**</span></span>

<span data-ttu-id="a8520-438">Interfejsy są teraz scalone razem `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="a8520-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="a8520-439">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-439">**Why**</span></span>

<span data-ttu-id="a8520-440">Ta zmiana została wprowadzona, ponieważ interfejsy są koncepcyjnie jeden.</span><span class="sxs-lookup"><span data-stu-id="a8520-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="a8520-441">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-441">**Mitigations**</span></span>

<span data-ttu-id="a8520-442">Wszelkie implementacje `IDbContextOptionsExtension` musi zostać zaktualizowany do obsługi nowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="a8520-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="a8520-443">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="a8520-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="a8520-444">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="a8520-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="a8520-445">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-446">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-446">**Old behavior**</span></span>

<span data-ttu-id="a8520-447">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="a8520-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="a8520-448">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="a8520-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="a8520-449">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="a8520-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="a8520-450">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-450">**New behavior**</span></span>

<span data-ttu-id="a8520-451">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="a8520-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="a8520-452">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="a8520-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="a8520-453">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="a8520-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="a8520-454">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="a8520-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="a8520-455">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-455">**Why**</span></span>

<span data-ttu-id="a8520-456">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a8520-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="a8520-457">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-457">**Mitigations**</span></span>

<span data-ttu-id="a8520-458">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="a8520-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="a8520-459">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="a8520-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="a8520-460">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="a8520-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="a8520-461">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-462">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-462">**Old behavior**</span></span>

<span data-ttu-id="a8520-463">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="a8520-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="a8520-464">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-464">**New behavior**</span></span>

<span data-ttu-id="a8520-465">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a8520-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="a8520-466">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-466">**Why**</span></span>

<span data-ttu-id="a8520-467">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="a8520-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="a8520-468">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-468">**Mitigations**</span></span>

<span data-ttu-id="a8520-469">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="a8520-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="a8520-470">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8520-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="a8520-471">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="a8520-472">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="a8520-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="a8520-473">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="a8520-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="a8520-474">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-475">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-475">**Old behavior**</span></span>

<span data-ttu-id="a8520-476">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została zinterpretowana w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="a8520-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="a8520-477">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="a8520-478">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="a8520-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="a8520-479">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="a8520-480">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-480">**New behavior**</span></span>

<span data-ttu-id="a8520-481">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="a8520-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="a8520-482">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-482">**Why**</span></span>

<span data-ttu-id="a8520-483">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="a8520-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="a8520-484">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-484">**Mitigations**</span></span>

<span data-ttu-id="a8520-485">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="a8520-486">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="a8520-486">This is not common.</span></span>
<span data-ttu-id="a8520-487">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="a8520-488">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="a8520-489">Typ zwracany dla kilku metod asynchronicznych została zmieniona z zadaniem do ValueTask</span><span class="sxs-lookup"><span data-stu-id="a8520-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="a8520-490">Śledzenie problem #15184</span><span class="sxs-lookup"><span data-stu-id="a8520-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="a8520-491">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-492">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-492">**Old behavior**</span></span>

<span data-ttu-id="a8520-493">Następujące metody async wcześniej zwrócony `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="a8520-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="a8520-494">`ValueGenerator.NextValueAsync()` (i wyprowadzanie klas)</span><span class="sxs-lookup"><span data-stu-id="a8520-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="a8520-495">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-495">**New behavior**</span></span>

<span data-ttu-id="a8520-496">Wymienione wyżej metod teraz zwracają `ValueTask<T>` w taki sam `T` tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="a8520-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="a8520-497">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-497">**Why**</span></span>

<span data-ttu-id="a8520-498">Ta zmiana zmniejsza liczbę alokacji sterty naliczany podczas wywoływania metody te poprawę ogólnej wydajności.</span><span class="sxs-lookup"><span data-stu-id="a8520-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="a8520-499">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-499">**Mitigations**</span></span>

<span data-ttu-id="a8520-500">Tylko po prostu oczekujące na powyższym interfejsów API muszą zostać zrekompilowane — aplikacje bez zmian źródła są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="a8520-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="a8520-501">Bardziej złożonych zastosowaniach (np. przekazywanie zwracanego `Task` do `Task.WhenAny()`) zwykle wymagają, aby zwrócony `ValueTask<T>` można przekonwertować na `Task<T>` przez wywołanie metody `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="a8520-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="a8520-502">Należy pamiętać, że to neguje zmniejszenia alokacji, które ta zmiana powoduje.</span><span class="sxs-lookup"><span data-stu-id="a8520-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="a8520-503">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a8520-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="a8520-504">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="a8520-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="a8520-505">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="a8520-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a8520-506">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-506">**Old behavior**</span></span>

<span data-ttu-id="a8520-507">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a8520-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="a8520-508">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-508">**New behavior**</span></span>

<span data-ttu-id="a8520-509">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a8520-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="a8520-510">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-510">**Why**</span></span>

<span data-ttu-id="a8520-511">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8520-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="a8520-512">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-512">**Mitigations**</span></span>

<span data-ttu-id="a8520-513">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="a8520-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="a8520-514">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="a8520-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="a8520-515">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="a8520-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="a8520-516">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="a8520-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="a8520-517">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-518">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-518">**Old behavior**</span></span>

<span data-ttu-id="a8520-519">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="a8520-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="a8520-520">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-520">**New behavior**</span></span>

<span data-ttu-id="a8520-521">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="a8520-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="a8520-522">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-522">**Why**</span></span>

<span data-ttu-id="a8520-523">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a8520-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="a8520-524">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="a8520-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="a8520-525">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-525">**Mitigations**</span></span>

<span data-ttu-id="a8520-526">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="a8520-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="a8520-527">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="a8520-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="a8520-528">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="a8520-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="a8520-529">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-530">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-530">**Old behavior**</span></span>

<span data-ttu-id="a8520-531">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="a8520-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="a8520-532">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-532">**New behavior**</span></span>

<span data-ttu-id="a8520-533">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="a8520-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="a8520-534">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="a8520-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="a8520-535">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-535">**Why**</span></span>

<span data-ttu-id="a8520-536">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Include` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a8520-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="a8520-537">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-537">**Mitigations**</span></span>

<span data-ttu-id="a8520-538">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="a8520-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="a8520-539">Zmiany metadanych interfejsu API</span><span class="sxs-lookup"><span data-stu-id="a8520-539">Metadata API changes</span></span>

[<span data-ttu-id="a8520-540">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="a8520-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a8520-541">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-542">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-542">**New behavior**</span></span>

<span data-ttu-id="a8520-543">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="a8520-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="a8520-544">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-544">**Why**</span></span>

<span data-ttu-id="a8520-545">Ta zmiana ułatwia implementację interfejsów wyżej.</span><span class="sxs-lookup"><span data-stu-id="a8520-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="a8520-546">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-546">**Mitigations**</span></span>

<span data-ttu-id="a8520-547">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a8520-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="a8520-548">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="a8520-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="a8520-549">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="a8520-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="a8520-550">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="a8520-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a8520-551">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-551">**Old behavior**</span></span>

<span data-ttu-id="a8520-552">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a8520-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a8520-553">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-553">**New behavior**</span></span>

<span data-ttu-id="a8520-554">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a8520-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a8520-555">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-555">**Why**</span></span>

<span data-ttu-id="a8520-556">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="a8520-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="a8520-557">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-557">**Mitigations**</span></span>

<span data-ttu-id="a8520-558">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="a8520-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="a8520-559">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8520-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="a8520-560">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="a8520-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="a8520-561">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-561">**Old behavior**</span></span>

<span data-ttu-id="a8520-562">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="a8520-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="a8520-563">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-563">**New behavior**</span></span>

<span data-ttu-id="a8520-564">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="a8520-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="a8520-565">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-565">**Why**</span></span>

<span data-ttu-id="a8520-566">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="a8520-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="a8520-567">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-567">**Mitigations**</span></span>

<span data-ttu-id="a8520-568">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="a8520-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a8520-569">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="a8520-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a8520-570">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="a8520-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="a8520-571">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-572">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-572">**Old behavior**</span></span>

<span data-ttu-id="a8520-573">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a8520-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="a8520-574">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-574">**New behavior**</span></span>

<span data-ttu-id="a8520-575">Wartości identyfikatora GUID są teraz sotred jako tekst.</span><span class="sxs-lookup"><span data-stu-id="a8520-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="a8520-576">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-576">**Why**</span></span>

<span data-ttu-id="a8520-577">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="a8520-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="a8520-578">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="a8520-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a8520-579">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-579">**Mitigations**</span></span>

<span data-ttu-id="a8520-580">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a8520-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="a8520-581">W programie EF Core można także kontynuować, używając poprzednie zachowanie configuirng konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="a8520-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="a8520-582">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="a8520-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a8520-583">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="a8520-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a8520-584">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="a8520-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="a8520-585">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-586">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-586">**Old behavior**</span></span>

<span data-ttu-id="a8520-587">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a8520-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="a8520-588">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="a8520-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="a8520-589">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-589">**New behavior**</span></span>

<span data-ttu-id="a8520-590">Wartości char są teraz sotred jako tekst.</span><span class="sxs-lookup"><span data-stu-id="a8520-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="a8520-591">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-591">**Why**</span></span>

<span data-ttu-id="a8520-592">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="a8520-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a8520-593">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-593">**Mitigations**</span></span>

<span data-ttu-id="a8520-594">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a8520-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="a8520-595">W programie EF Core można także kontynuować, używając poprzednie zachowanie configuirng konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="a8520-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="a8520-596">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="a8520-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="a8520-597">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="a8520-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="a8520-598">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="a8520-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="a8520-599">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-600">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-600">**Old behavior**</span></span>

<span data-ttu-id="a8520-601">Migracja identyfikatorów były generowane przy użyciu kalendarza kultury currret inadvertantly.</span><span class="sxs-lookup"><span data-stu-id="a8520-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="a8520-602">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-602">**New behavior**</span></span>

<span data-ttu-id="a8520-603">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="a8520-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="a8520-604">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-604">**Why**</span></span>

<span data-ttu-id="a8520-605">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="a8520-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="a8520-606">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="a8520-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="a8520-607">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-607">**Mitigations**</span></span>

<span data-ttu-id="a8520-608">Ta zmiana dotyczy wszystkich osób korzystających kalendarzowe innych niż gregoriański, gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="a8520-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="a8520-609">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="a8520-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="a8520-610">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="a8520-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="a8520-611">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="a8520-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="a8520-612">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="a8520-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="a8520-613">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="a8520-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="a8520-614">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-615">**Change**</span><span class="sxs-lookup"><span data-stu-id="a8520-615">**Change**</span></span>

<span data-ttu-id="a8520-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="a8520-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="a8520-617">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-617">**Why**</span></span>

<span data-ttu-id="a8520-618">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="a8520-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="a8520-619">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-619">**Mitigations**</span></span>

<span data-ttu-id="a8520-620">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a8520-620">Use the new name.</span></span> <span data-ttu-id="a8520-621">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="a8520-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="a8520-622">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="a8520-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="a8520-623">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="a8520-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="a8520-624">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="a8520-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a8520-625">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-625">**Old behavior**</span></span>

<span data-ttu-id="a8520-626">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="a8520-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="a8520-627">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="a8520-628">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a8520-628">**New behavior**</span></span>

<span data-ttu-id="a8520-629">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "tego name".</span><span class="sxs-lookup"><span data-stu-id="a8520-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="a8520-630">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8520-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="a8520-631">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a8520-631">**Why**</span></span>

<span data-ttu-id="a8520-632">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego tego, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="a8520-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="a8520-633">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a8520-633">**Mitigations**</span></span>

<span data-ttu-id="a8520-634">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a8520-634">Use the new name.</span></span>
