---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0b36571dfe9e462be3aa818b72b5a38b9573410c
ms.sourcegitcommit: 1e44721cd0903b08781b78eb398d2a9b13a46db9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2019
ms.locfileid: "66815654"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="ac4fa-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="ac4fa-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ac4fa-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ac4fa-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="ac4fa-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="ac4fa-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="ac4fa-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="ac4fa-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="ac4fa-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="ac4fa-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="ac4fa-109">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-110">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="ac4fa-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="ac4fa-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-113">**New behavior**</span></span>

<span data-ttu-id="ac4fa-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="ac4fa-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="ac4fa-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-116">**Why**</span></span>

<span data-ttu-id="ac4fa-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="ac4fa-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="ac4fa-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="ac4fa-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="ac4fa-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="ac4fa-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="ac4fa-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-123">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="ac4fa-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac4fa-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="ac4fa-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="ac4fa-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="ac4fa-127">Ta zmiana zostanie wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="ac4fa-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-128">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="ac4fa-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-130">**New behavior**</span></span>

<span data-ttu-id="ac4fa-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="ac4fa-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-132">**Why**</span></span>

<span data-ttu-id="ac4fa-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="ac4fa-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="ac4fa-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="ac4fa-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="ac4fa-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-137">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="ac4fa-139">Narzędzie wiersza polecenia programu EF Core dotnet ef nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ac4fa-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="ac4fa-140">Śledzenie problem #14016</span><span class="sxs-lookup"><span data-stu-id="ac4fa-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="ac4fa-141">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4 i odpowiednia wersja zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="ac4fa-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-142">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-143">Przed 3.0 `dotnet ef` narzędzie została uwzględniona w zestawie SDK programu .NET Core i były łatwo dostępne do użycia z poziomu wiersza polecenia z dowolnego projektu bez konieczności wykonania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="ac4fa-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-144">**New behavior**</span></span>

<span data-ttu-id="ac4fa-145">Począwszy od 3.0, zestaw SDK platformy .NET nie obejmuje `dotnet ef` narzędzia, dzięki czemu przed jego użyciem trzeba jawnie zainstalować ją jako narzędzie lokalnych lub globalnych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="ac4fa-146">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-146">**Why**</span></span>

<span data-ttu-id="ac4fa-147">Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako regularne narzędzia wiersza polecenia platformy .NET w usłudze NuGet, spójne z faktu, że EF Core 3.0 również są zawsze jest rozpowszechniany jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="ac4fa-148">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-148">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-149">Aby można było zarządzać migracji lub szkieletu `DbContext`, zainstaluj `dotnet-ef` przy użyciu `dotnet tool install` polecenia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="ac4fa-150">Aby zainstalować go jako narzędzie globalne, możesz na przykład, wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="ac4fa-151">Możesz również uzyskać je lokalne narzędzie podczas przywracania zależności projektu, który deklaruje ją jako zależność narzędzia przy użyciu [plik manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="ac4fa-152">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="ac4fa-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="ac4fa-153">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="ac4fa-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="ac4fa-154">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-155">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-155">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-156">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="ac4fa-157">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-157">**New behavior**</span></span>

<span data-ttu-id="ac4fa-158">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="ac4fa-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="ac4fa-160">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="ac4fa-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="ac4fa-162">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="ac4fa-163">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-163">**Why**</span></span>

<span data-ttu-id="ac4fa-164">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody nieprzetworzonego ciągu, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="ac4fa-165">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="ac4fa-166">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-166">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-167">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="ac4fa-168">Wykonanie zapytania jest rejestrowane na poziomie debugowania</span><span class="sxs-lookup"><span data-stu-id="ac4fa-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="ac4fa-169">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="ac4fa-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="ac4fa-170">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-170">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-171">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-171">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-172">Przed programem EF Core 3.0, wykonywanie zapytań i innych poleceniach, zarejestrowano w `Info` poziom.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="ac4fa-173">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-173">**New behavior**</span></span>

<span data-ttu-id="ac4fa-174">Począwszy od programu EF Core 3.0 rejestrowanie wykonywania polecenia/SQL jest w `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="ac4fa-175">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-175">**Why**</span></span>

<span data-ttu-id="ac4fa-176">Ta zmiana została wprowadzona redukcji szumu w `Info` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="ac4fa-177">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-177">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-178">To zdarzenie logowania jest definiowany przez `RelationalEventId.CommandExecuting` zdarzenia o identyfikatorze 20100.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="ac4fa-179">Do logowania SQL w `Info` poziomu ponownie, jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="ac4fa-180">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="ac4fa-181">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="ac4fa-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="ac4fa-182">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="ac4fa-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="ac4fa-183">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-183">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ac4fa-184">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-184">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-185">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="ac4fa-186">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="ac4fa-187">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-187">**New behavior**</span></span>

<span data-ttu-id="ac4fa-188">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="ac4fa-189">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-189">**Why**</span></span>

<span data-ttu-id="ac4fa-190">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="ac4fa-191">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-191">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-192">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="ac4fa-193">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-193">This can be avoided by:</span></span>
* <span data-ttu-id="ac4fa-194">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="ac4fa-195">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="ac4fa-196">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="ac4fa-197">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="ac4fa-198">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="ac4fa-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="ac4fa-199">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="ac4fa-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="ac4fa-200">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-200">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-201">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-201">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-202">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="ac4fa-203">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-203">**New behavior**</span></span>

<span data-ttu-id="ac4fa-204">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="ac4fa-205">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="ac4fa-206">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="ac4fa-207">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-207">**Why**</span></span>

<span data-ttu-id="ac4fa-208">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="ac4fa-209">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-209">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-210">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="ac4fa-211">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="ac4fa-212">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="ac4fa-213">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="ac4fa-214">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="ac4fa-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="ac4fa-215">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="ac4fa-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="ac4fa-216">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-216">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-217">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-217">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-218">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="ac4fa-219">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-219">**New behavior**</span></span>

<span data-ttu-id="ac4fa-220">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="ac4fa-221">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="ac4fa-222">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-222">**Why**</span></span>

<span data-ttu-id="ac4fa-223">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="ac4fa-224">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-224">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-225">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="ac4fa-226">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="ac4fa-227">DeleteBehavior.Restrict ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="ac4fa-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="ac4fa-228">Śledzenie problem #12661</span><span class="sxs-lookup"><span data-stu-id="ac4fa-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="ac4fa-229">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 5.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-229">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="ac4fa-230">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-230">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-231">Przed 3.0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych o `Restrict` semantykę, ale także zmienione wewnętrznego naprawy w taki sposób,-oczywiste.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="ac4fa-232">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-232">**New behavior**</span></span>

<span data-ttu-id="ac4fa-233">Począwszy od 3.0 `DeleteBehavior.Restrict` gwarantuje, że klucze obce są tworzone za pomocą `Restrict` semantyki — czyli nie kaskady; throw na naruszenie ograniczenia — bez również wpływ na wewnętrzny naprawy EF.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="ac4fa-234">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-234">**Why**</span></span>

<span data-ttu-id="ac4fa-235">Ta zmiana została wprowadzona w celu usprawnienie obsługi przy użyciu `DeleteBehavior` w sposób intuicyjny, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="ac4fa-236">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-236">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-237">Poprzednie zachowanie można przywrócić za pomocą `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="ac4fa-238">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="ac4fa-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="ac4fa-239">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="ac4fa-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="ac4fa-240">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-240">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-241">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-241">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-242">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="ac4fa-243">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="ac4fa-244">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-244">**New behavior**</span></span>

<span data-ttu-id="ac4fa-245">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="ac4fa-246">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="ac4fa-247">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-247">**Why**</span></span>

<span data-ttu-id="ac4fa-248">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="ac4fa-249">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="ac4fa-250">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="ac4fa-251">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-251">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-252">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="ac4fa-253">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="ac4fa-254">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="ac4fa-255">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="ac4fa-256">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="ac4fa-257">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="ac4fa-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="ac4fa-258">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="ac4fa-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="ac4fa-259">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-259">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-260">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-260">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-261">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="ac4fa-262">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-262">**New behavior**</span></span>

<span data-ttu-id="ac4fa-263">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="ac4fa-264">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="ac4fa-265">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="ac4fa-266">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="ac4fa-267">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-267">For example:</span></span>

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

<span data-ttu-id="ac4fa-268">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="ac4fa-269">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-269">**Why**</span></span>

<span data-ttu-id="ac4fa-270">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="ac4fa-271">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="ac4fa-272">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-272">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-273">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="ac4fa-274">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="ac4fa-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="ac4fa-275">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="ac4fa-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="ac4fa-276">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-276">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-277">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-277">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-278">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-278">Consider the following model:</span></span>
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
<span data-ttu-id="ac4fa-279">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="ac4fa-280">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-280">**New behavior**</span></span>

<span data-ttu-id="ac4fa-281">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="ac4fa-282">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="ac4fa-283">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-283">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-284">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="ac4fa-285">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="ac4fa-286">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="ac4fa-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="ac4fa-287">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="ac4fa-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="ac4fa-288">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-288">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-289">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-289">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-290">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-290">Consider the following model:</span></span>
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
<span data-ttu-id="ac4fa-291">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="ac4fa-292">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-292">**New behavior**</span></span>

<span data-ttu-id="ac4fa-293">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="ac4fa-294">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="ac4fa-295">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-295">**Why**</span></span>

<span data-ttu-id="ac4fa-296">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="ac4fa-297">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-297">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-298">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="ac4fa-299">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="ac4fa-300">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="ac4fa-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="ac4fa-301">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="ac4fa-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="ac4fa-302">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-302">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-303">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-303">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-304">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-304">Consider the following model:</span></span>
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

<span data-ttu-id="ac4fa-305">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="ac4fa-306">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-306">**New behavior**</span></span>

<span data-ttu-id="ac4fa-307">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="ac4fa-308">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-308">**Why**</span></span>

<span data-ttu-id="ac4fa-309">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="ac4fa-310">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-310">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-311">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="ac4fa-312">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="ac4fa-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="ac4fa-313">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="ac4fa-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="ac4fa-314">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-314">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-315">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-315">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-316">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-316">Consider the following model:</span></span>
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
<span data-ttu-id="ac4fa-317">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="ac4fa-318">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="ac4fa-319">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-319">**New behavior**</span></span>

<span data-ttu-id="ac4fa-320">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="ac4fa-321">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="ac4fa-322">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-322">For example:</span></span>

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

<span data-ttu-id="ac4fa-323">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-323">**Why**</span></span>

<span data-ttu-id="ac4fa-324">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="ac4fa-325">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-325">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-326">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="ac4fa-327">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="ac4fa-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="ac4fa-328">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="ac4fa-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="ac4fa-329">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-329">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-330">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-330">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-331">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="ac4fa-332">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-332">**New behavior**</span></span>

<span data-ttu-id="ac4fa-333">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="ac4fa-334">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-334">**Why**</span></span>

<span data-ttu-id="ac4fa-335">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="ac4fa-336">Nowe zachowanie dopasowuje również EF6.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="ac4fa-337">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-337">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-338">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="ac4fa-339">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="ac4fa-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="ac4fa-340">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="ac4fa-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="ac4fa-341">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-341">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-342">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-342">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-343">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="ac4fa-344">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-344">**New behavior**</span></span>

<span data-ttu-id="ac4fa-345">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="ac4fa-346">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="ac4fa-347">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-347">**Why**</span></span>

<span data-ttu-id="ac4fa-348">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="ac4fa-349">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-349">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-350">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="ac4fa-351">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="ac4fa-352">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="ac4fa-352">Backing fields are used by default</span></span>

[<span data-ttu-id="ac4fa-353">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="ac4fa-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="ac4fa-354">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-354">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ac4fa-355">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-355">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-356">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="ac4fa-357">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="ac4fa-358">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-358">**New behavior**</span></span>

<span data-ttu-id="ac4fa-359">Zaczynając od programu EF Core 3.0 to, jeśli znane jest pole zapasowe dla właściwości, następnie programu EF Core będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="ac4fa-360">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="ac4fa-361">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-361">**Why**</span></span>

<span data-ttu-id="ac4fa-362">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="ac4fa-363">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-363">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-364">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-364">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="ac4fa-365">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="ac4fa-366">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="ac4fa-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="ac4fa-367">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="ac4fa-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="ac4fa-368">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-368">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-369">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-369">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-370">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="ac4fa-371">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="ac4fa-372">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-372">**New behavior**</span></span>

<span data-ttu-id="ac4fa-373">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="ac4fa-374">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-374">**Why**</span></span>

<span data-ttu-id="ac4fa-375">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="ac4fa-376">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-376">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-377">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="ac4fa-378">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="ac4fa-379">Właściwość tylko do pola nazwy powinny pasować do nazwy pola</span><span class="sxs-lookup"><span data-stu-id="ac4fa-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="ac4fa-380">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-380">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-381">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-381">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-382">Przed programem EF Core 3.0 to właściwość może być określony przez wartość ciągu i jeśli żadnej właściwości o tej nazwie został znaleziony na typ CLR programu EF Core będzie spróbuj dopasować go do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="ac4fa-383">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-383">**New behavior**</span></span>

<span data-ttu-id="ac4fa-384">Począwszy od programu EF Core 3.0 to właściwość tylko do pola musi być zgodny nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="ac4fa-385">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-385">**Why**</span></span>

<span data-ttu-id="ac4fa-386">Ta zmiana została wprowadzona w celu uniknięcia przy użyciu tego samego pola na dwie właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości tylko do pól takie same jak właściwości zamapowanych do właściwości aparatu CLR.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="ac4fa-387">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-387">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-388">Właściwości tylko do pola musi mieć nazwę taka sama jak pola, które są mapowane.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="ac4fa-389">W nowszej wersji zapoznawczej programu EF Core 3.0 planujemy ponownie włączyć konfigurowanie jawnie nazwę pola, która jest inna niż nazwa właściwości:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="ac4fa-390">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ac4fa-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="ac4fa-391">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="ac4fa-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="ac4fa-392">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-392">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-393">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-393">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-394">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="ac4fa-395">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-395">**New behavior**</span></span>

<span data-ttu-id="ac4fa-396">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="ac4fa-397">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-397">**Why**</span></span>

<span data-ttu-id="ac4fa-398">EF Core 3.0 nie wymaga, aby te usługi są w kontenerze DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-398">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="ac4fa-399">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="ac4fa-400">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-400">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-401">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="ac4fa-402">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="ac4fa-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="ac4fa-403">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="ac4fa-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ac4fa-404">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-404">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-405">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-405">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-406">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="ac4fa-407">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="ac4fa-408">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-408">**New behavior**</span></span>

<span data-ttu-id="ac4fa-409">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="ac4fa-410">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="ac4fa-411">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="ac4fa-412">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="ac4fa-413">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-413">**Why**</span></span>

<span data-ttu-id="ac4fa-414">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="ac4fa-415">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-415">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-416">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="ac4fa-417">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="ac4fa-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="ac4fa-418">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="ac4fa-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="ac4fa-419">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-419">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-420">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-420">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-421">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="ac4fa-422">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="ac4fa-423">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-423">**New behavior**</span></span>

<span data-ttu-id="ac4fa-424">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="ac4fa-425">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-425">**Why**</span></span>

<span data-ttu-id="ac4fa-426">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="ac4fa-427">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-427">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-428">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="ac4fa-429">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="ac4fa-430">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="ac4fa-431">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="ac4fa-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="ac4fa-432">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="ac4fa-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="ac4fa-433">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-433">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-434">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-434">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-435">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="ac4fa-436">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-436">**New behavior**</span></span>

<span data-ttu-id="ac4fa-437">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="ac4fa-438">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-438">**Why**</span></span>

<span data-ttu-id="ac4fa-439">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="ac4fa-440">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-440">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-441">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="ac4fa-442">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-442">This isn't common.</span></span>
<span data-ttu-id="ac4fa-443">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="ac4fa-444">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="ac4fa-445">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension została scalona z</span><span class="sxs-lookup"><span data-stu-id="ac4fa-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="ac4fa-446">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="ac4fa-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ac4fa-447">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-448">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-448">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-449">`IDbContextOptionsExtensionWithDebugInfo` przedłużono dodatkowy opcjonalny interfejs z `IDbContextOptionsExtension` Aby uniknąć konieczności istotne zmiany w interfejsie podczas cyklu wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="ac4fa-450">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-450">**New behavior**</span></span>

<span data-ttu-id="ac4fa-451">Interfejsy są teraz scalone razem `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="ac4fa-452">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-452">**Why**</span></span>

<span data-ttu-id="ac4fa-453">Ta zmiana została wprowadzona, ponieważ interfejsy są koncepcyjnie jeden.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="ac4fa-454">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-454">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-455">Wszelkie implementacje `IDbContextOptionsExtension` musi zostać zaktualizowany do obsługi nowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="ac4fa-456">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="ac4fa-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="ac4fa-457">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="ac4fa-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="ac4fa-458">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-458">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-459">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-459">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-460">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="ac4fa-461">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="ac4fa-462">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="ac4fa-463">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-463">**New behavior**</span></span>

<span data-ttu-id="ac4fa-464">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="ac4fa-465">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="ac4fa-466">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="ac4fa-467">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="ac4fa-468">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-468">**Why**</span></span>

<span data-ttu-id="ac4fa-469">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="ac4fa-470">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-470">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-471">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="ac4fa-472">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="ac4fa-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="ac4fa-473">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="ac4fa-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="ac4fa-474">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-474">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-475">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-475">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-476">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="ac4fa-477">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-477">**New behavior**</span></span>

<span data-ttu-id="ac4fa-478">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="ac4fa-479">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-479">**Why**</span></span>

<span data-ttu-id="ac4fa-480">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="ac4fa-481">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-481">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-482">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="ac4fa-483">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="ac4fa-484">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="ac4fa-485">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="ac4fa-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="ac4fa-486">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="ac4fa-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="ac4fa-487">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-487">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-488">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-488">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-489">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została interpretowany w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="ac4fa-490">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="ac4fa-491">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="ac4fa-492">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="ac4fa-493">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-493">**New behavior**</span></span>

<span data-ttu-id="ac4fa-494">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="ac4fa-495">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-495">**Why**</span></span>

<span data-ttu-id="ac4fa-496">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="ac4fa-497">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-497">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-498">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="ac4fa-499">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-499">This is not common.</span></span>
<span data-ttu-id="ac4fa-500">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="ac4fa-501">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="ac4fa-502">Typ zwracany dla kilku metod asynchronicznych została zmieniona z zadaniem do ValueTask</span><span class="sxs-lookup"><span data-stu-id="ac4fa-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="ac4fa-503">Śledzenie problem #15184</span><span class="sxs-lookup"><span data-stu-id="ac4fa-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="ac4fa-504">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-504">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-505">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-505">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-506">Następujące metody async wcześniej zwrócony `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="ac4fa-507">`ValueGenerator.NextValueAsync()` (i wyprowadzanie klas)</span><span class="sxs-lookup"><span data-stu-id="ac4fa-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="ac4fa-508">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-508">**New behavior**</span></span>

<span data-ttu-id="ac4fa-509">Wymienione wyżej metod teraz zwracają `ValueTask<T>` w taki sam `T` tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="ac4fa-510">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-510">**Why**</span></span>

<span data-ttu-id="ac4fa-511">Ta zmiana zmniejsza liczbę alokacji sterty naliczany podczas wywoływania metody te poprawę ogólnej wydajności.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="ac4fa-512">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-512">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-513">Tylko po prostu oczekujące na powyższym interfejsów API muszą zostać zrekompilowane — aplikacje bez zmian źródła są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="ac4fa-514">Bardziej złożonych zastosowaniach (np. przekazywanie zwracanego `Task` do `Task.WhenAny()`) zwykle wymagają, aby zwrócony `ValueTask<T>` można przekonwertować na `Task<T>` przez wywołanie metody `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="ac4fa-515">Należy pamiętać, że to neguje zmniejszenia alokacji, które ta zmiana powoduje.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="ac4fa-516">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="ac4fa-517">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="ac4fa-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="ac4fa-518">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-518">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ac4fa-519">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-519">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-520">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="ac4fa-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="ac4fa-521">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-521">**New behavior**</span></span>

<span data-ttu-id="ac4fa-522">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="ac4fa-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="ac4fa-523">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-523">**Why**</span></span>

<span data-ttu-id="ac4fa-524">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="ac4fa-525">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-525">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-526">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="ac4fa-527">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="ac4fa-528">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="ac4fa-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="ac4fa-529">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="ac4fa-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="ac4fa-530">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-530">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-531">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-531">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-532">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="ac4fa-533">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-533">**New behavior**</span></span>

<span data-ttu-id="ac4fa-534">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="ac4fa-535">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-535">**Why**</span></span>

<span data-ttu-id="ac4fa-536">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="ac4fa-537">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="ac4fa-538">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-538">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-539">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="ac4fa-540">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="ac4fa-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="ac4fa-541">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="ac4fa-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="ac4fa-542">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-542">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-543">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-543">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-544">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="ac4fa-545">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-545">**New behavior**</span></span>

<span data-ttu-id="ac4fa-546">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="ac4fa-547">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="ac4fa-548">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-548">**Why**</span></span>

<span data-ttu-id="ac4fa-549">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Include` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="ac4fa-550">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-550">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-551">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="ac4fa-552">Zmiany metadanych interfejsu API</span><span class="sxs-lookup"><span data-stu-id="ac4fa-552">Metadata API changes</span></span>

[<span data-ttu-id="ac4fa-553">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="ac4fa-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ac4fa-554">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-554">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-555">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-555">**New behavior**</span></span>

<span data-ttu-id="ac4fa-556">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="ac4fa-557">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-557">**Why**</span></span>

<span data-ttu-id="ac4fa-558">Ta zmiana ułatwia implementację interfejsów wyżej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="ac4fa-559">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-559">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-560">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-560">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="ac4fa-561">Zmiany interfejsu API metadane właściwe dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="ac4fa-561">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="ac4fa-562">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="ac4fa-562">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ac4fa-563">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-563">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ac4fa-564">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-564">**New behavior**</span></span>

<span data-ttu-id="ac4fa-565">Metody rozszerzające właściwe dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-565">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="ac4fa-566">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-566">**Why**</span></span>

<span data-ttu-id="ac4fa-567">Ta zmiana ułatwia implementację metody rozszerzenia wyżej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-567">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="ac4fa-568">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-568">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-569">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-569">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="ac4fa-570">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="ac4fa-570">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="ac4fa-571">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="ac4fa-571">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="ac4fa-572">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-572">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ac4fa-573">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-573">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-574">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-574">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ac4fa-575">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-575">**New behavior**</span></span>

<span data-ttu-id="ac4fa-576">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-576">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ac4fa-577">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-577">**Why**</span></span>

<span data-ttu-id="ac4fa-578">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-578">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="ac4fa-579">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-579">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-580">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-580">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="ac4fa-581">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-581">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="ac4fa-582">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="ac4fa-582">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="ac4fa-583">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-583">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-584">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-584">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="ac4fa-585">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-585">**New behavior**</span></span>

<span data-ttu-id="ac4fa-586">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-586">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="ac4fa-587">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-587">**Why**</span></span>

<span data-ttu-id="ac4fa-588">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-588">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="ac4fa-589">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-589">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-590">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-590">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ac4fa-591">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="ac4fa-591">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ac4fa-592">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="ac4fa-592">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="ac4fa-593">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-594">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-594">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-595">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-595">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="ac4fa-596">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-596">**New behavior**</span></span>

<span data-ttu-id="ac4fa-597">Wartości identyfikatora GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-597">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="ac4fa-598">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-598">**Why**</span></span>

<span data-ttu-id="ac4fa-599">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-599">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="ac4fa-600">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-600">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ac4fa-601">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-601">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-602">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-602">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="ac4fa-603">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-603">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="ac4fa-604">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-604">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ac4fa-605">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="ac4fa-605">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ac4fa-606">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="ac4fa-606">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="ac4fa-607">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-607">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-608">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-608">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-609">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-609">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="ac4fa-610">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-610">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="ac4fa-611">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-611">**New behavior**</span></span>

<span data-ttu-id="ac4fa-612">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-612">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="ac4fa-613">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-613">**Why**</span></span>

<span data-ttu-id="ac4fa-614">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-614">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ac4fa-615">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-615">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-616">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-616">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="ac4fa-617">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-617">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="ac4fa-618">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-618">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="ac4fa-619">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="ac4fa-619">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="ac4fa-620">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="ac4fa-620">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="ac4fa-621">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-621">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-622">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-622">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-623">Migracja identyfikatorów przypadkowo zostały wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-623">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="ac4fa-624">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-624">**New behavior**</span></span>

<span data-ttu-id="ac4fa-625">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-625">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="ac4fa-626">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-626">**Why**</span></span>

<span data-ttu-id="ac4fa-627">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-627">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="ac4fa-628">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-628">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="ac4fa-629">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-629">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-630">Ta zmiana dotyczy wszystkich osób korzystających z innych kalendarz gregoriański — gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-630">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="ac4fa-631">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-631">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="ac4fa-632">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-632">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="ac4fa-633">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-633">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="ac4fa-634">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-634">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="ac4fa-635">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="ac4fa-635">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="ac4fa-636">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-636">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-637">**Change**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-637">**Change**</span></span>

<span data-ttu-id="ac4fa-638">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-638">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="ac4fa-639">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-639">**Why**</span></span>

<span data-ttu-id="ac4fa-640">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-640">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="ac4fa-641">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-641">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-642">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-642">Use the new name.</span></span> <span data-ttu-id="ac4fa-643">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="ac4fa-643">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="ac4fa-644">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="ac4fa-644">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="ac4fa-645">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="ac4fa-645">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="ac4fa-646">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-646">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ac4fa-647">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-647">**Old behavior**</span></span>

<span data-ttu-id="ac4fa-648">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="ac4fa-648">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="ac4fa-649">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-649">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="ac4fa-650">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-650">**New behavior**</span></span>

<span data-ttu-id="ac4fa-651">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "Nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="ac4fa-651">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="ac4fa-652">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac4fa-652">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="ac4fa-653">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-653">**Why**</span></span>

<span data-ttu-id="ac4fa-654">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego ograniczenia, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-654">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="ac4fa-655">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ac4fa-655">**Mitigations**</span></span>

<span data-ttu-id="ac4fa-656">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="ac4fa-656">Use the new name.</span></span>
