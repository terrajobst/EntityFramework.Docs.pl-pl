---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 9112d8d235237e68232aac54453d584af0edb524
ms.sourcegitcommit: b188194a1901f4d086d05765cbc5c9b8c9dc5eed
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66829484"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="b3db3-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="b3db3-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3db3-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="b3db3-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="b3db3-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="b3db3-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="b3db3-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="b3db3-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="b3db3-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="b3db3-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="b3db3-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="b3db3-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="b3db3-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="b3db3-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="b3db3-109">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-110">**Old behavior**</span></span>

<span data-ttu-id="b3db3-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b3db3-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="b3db3-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="b3db3-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-113">**New behavior**</span></span>

<span data-ttu-id="b3db3-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b3db3-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="b3db3-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="b3db3-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="b3db3-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-116">**Why**</span></span>

<span data-ttu-id="b3db3-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="b3db3-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="b3db3-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="b3db3-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b3db3-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="b3db3-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="b3db3-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="b3db3-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="b3db3-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="b3db3-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-123">**Mitigations**</span></span>

<span data-ttu-id="b3db3-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="b3db3-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="b3db3-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3db3-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="b3db3-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="b3db3-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="b3db3-127">Ta zmiana zostanie wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="b3db3-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="b3db3-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-128">**Old behavior**</span></span>

<span data-ttu-id="b3db3-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3db3-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="b3db3-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-130">**New behavior**</span></span>

<span data-ttu-id="b3db3-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3db3-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="b3db3-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-132">**Why**</span></span>

<span data-ttu-id="b3db3-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="b3db3-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="b3db3-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="b3db3-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="b3db3-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="b3db3-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-137">**Mitigations**</span></span>

<span data-ttu-id="b3db3-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="b3db3-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="b3db3-139">Narzędzie wiersza polecenia programu EF Core dotnet ef nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="b3db3-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="b3db3-140">Śledzenie problem #14016</span><span class="sxs-lookup"><span data-stu-id="b3db3-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="b3db3-141">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4 i odpowiednia wersja zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="b3db3-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="b3db3-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-142">**Old behavior**</span></span>

<span data-ttu-id="b3db3-143">Przed 3.0 `dotnet ef` narzędzie została uwzględniona w zestawie SDK programu .NET Core i były łatwo dostępne do użycia z poziomu wiersza polecenia z dowolnego projektu bez konieczności wykonania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="b3db3-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="b3db3-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-144">**New behavior**</span></span>

<span data-ttu-id="b3db3-145">Począwszy od 3.0, zestaw SDK platformy .NET nie obejmuje `dotnet ef` narzędzia, dzięki czemu przed jego użyciem trzeba jawnie zainstalować ją jako narzędzie lokalnych lub globalnych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="b3db3-146">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-146">**Why**</span></span>

<span data-ttu-id="b3db3-147">Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako regularne narzędzia wiersza polecenia platformy .NET w usłudze NuGet, spójne z faktu, że EF Core 3.0 również są zawsze jest rozpowszechniany jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b3db3-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="b3db3-148">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-148">**Mitigations**</span></span>

<span data-ttu-id="b3db3-149">Aby można było zarządzać migracji lub szkieletu `DbContext`, zainstaluj `dotnet-ef` przy użyciu `dotnet tool install` polecenia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="b3db3-150">Aby zainstalować go jako narzędzie globalne, możesz na przykład, wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b3db3-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="b3db3-151">Możesz również uzyskać je lokalne narzędzie podczas przywracania zależności projektu, który deklaruje ją jako zależność narzędzia przy użyciu [plik manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="b3db3-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="b3db3-152">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="b3db3-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="b3db3-153">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="b3db3-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="b3db3-154">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-155">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-155">**Old behavior**</span></span>

<span data-ttu-id="b3db3-156">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="b3db3-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="b3db3-157">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-157">**New behavior**</span></span>

<span data-ttu-id="b3db3-158">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="b3db3-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="b3db3-160">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="b3db3-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="b3db3-162">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="b3db3-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="b3db3-163">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-163">**Why**</span></span>

<span data-ttu-id="b3db3-164">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody nieprzetworzonego ciągu, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="b3db3-165">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="b3db3-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="b3db3-166">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-166">**Mitigations**</span></span>

<span data-ttu-id="b3db3-167">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="b3db3-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="b3db3-168">Metody FromSql można określić tylko na elementy główne zapytanie</span><span class="sxs-lookup"><span data-stu-id="b3db3-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="b3db3-169">Śledzenie problem #15704</span><span class="sxs-lookup"><span data-stu-id="b3db3-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="b3db3-170">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="b3db3-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b3db3-171">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-171">**Old behavior**</span></span>

<span data-ttu-id="b3db3-172">Przed programem EF Core 3.0 to `FromSql` metoda może być określona w dowolnym miejscu w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="b3db3-173">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-173">**New behavior**</span></span>

<span data-ttu-id="b3db3-174">Począwszy od programu EF Core 3.0 to nowy `FromSqlRaw` i `FromSqlInterpolated` metody (które Zastąp `FromSql`) można określić tylko na katalogi główne zapytanie, czyli bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="b3db3-175">Próba określenia ich w jakimkolwiek innym spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="b3db3-176">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-176">**Why**</span></span>

<span data-ttu-id="b3db3-177">Określanie `FromSql` dowolnym innym niż na `DbSet` nie dodano znaczenie lub wartość dodaną i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="b3db3-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="b3db3-178">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-178">**Mitigations**</span></span>

<span data-ttu-id="b3db3-179">`FromSql` wywołania zostanie przeniesiona za bezpośrednio na `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="b3db3-180">Wykonanie zapytania jest rejestrowane na poziomie debugowania</span><span class="sxs-lookup"><span data-stu-id="b3db3-180">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="b3db3-181">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="b3db3-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="b3db3-182">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-182">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-183">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-183">**Old behavior**</span></span>

<span data-ttu-id="b3db3-184">Przed programem EF Core 3.0, wykonywanie zapytań i innych poleceniach, zarejestrowano w `Info` poziom.</span><span class="sxs-lookup"><span data-stu-id="b3db3-184">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="b3db3-185">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-185">**New behavior**</span></span>

<span data-ttu-id="b3db3-186">Począwszy od programu EF Core 3.0 rejestrowanie wykonywania polecenia/SQL jest w `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="b3db3-186">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="b3db3-187">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-187">**Why**</span></span>

<span data-ttu-id="b3db3-188">Ta zmiana została wprowadzona redukcji szumu w `Info` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="b3db3-188">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="b3db3-189">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-189">**Mitigations**</span></span>

<span data-ttu-id="b3db3-190">To zdarzenie logowania jest definiowany przez `RelationalEventId.CommandExecuting` zdarzenia o identyfikatorze 20100.</span><span class="sxs-lookup"><span data-stu-id="b3db3-190">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="b3db3-191">Do logowania SQL w `Info` poziomu ponownie, jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-191">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="b3db3-192">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-192">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="b3db3-193">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="b3db3-193">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="b3db3-194">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="b3db3-194">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="b3db3-195">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="b3db3-195">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3db3-196">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-196">**Old behavior**</span></span>

<span data-ttu-id="b3db3-197">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-197">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="b3db3-198">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-198">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="b3db3-199">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-199">**New behavior**</span></span>

<span data-ttu-id="b3db3-200">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="b3db3-200">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="b3db3-201">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-201">**Why**</span></span>

<span data-ttu-id="b3db3-202">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-202">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="b3db3-203">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-203">**Mitigations**</span></span>

<span data-ttu-id="b3db3-204">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-204">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="b3db3-205">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="b3db3-205">This can be avoided by:</span></span>
* <span data-ttu-id="b3db3-206">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-206">Not using store-generated keys.</span></span>
* <span data-ttu-id="b3db3-207">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-207">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="b3db3-208">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-208">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="b3db3-209">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="b3db3-209">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="b3db3-210">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="b3db3-210">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="b3db3-211">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="b3db3-211">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="b3db3-212">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-212">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-213">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-213">**Old behavior**</span></span>

<span data-ttu-id="b3db3-214">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b3db3-214">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="b3db3-215">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-215">**New behavior**</span></span>

<span data-ttu-id="b3db3-216">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-216">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="b3db3-217">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b3db3-217">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="b3db3-218">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="b3db3-218">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="b3db3-219">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-219">**Why**</span></span>

<span data-ttu-id="b3db3-220">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-220">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="b3db3-221">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-221">**Mitigations**</span></span>

<span data-ttu-id="b3db3-222">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="b3db3-222">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="b3db3-223">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-223">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="b3db3-224">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="b3db3-224">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="b3db3-225">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="b3db3-225">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="b3db3-226">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="b3db3-226">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="b3db3-227">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="b3db3-227">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="b3db3-228">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-228">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-229">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-229">**Old behavior**</span></span>

<span data-ttu-id="b3db3-230">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="b3db3-230">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="b3db3-231">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-231">**New behavior**</span></span>

<span data-ttu-id="b3db3-232">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-232">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="b3db3-233">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="b3db3-233">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="b3db3-234">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-234">**Why**</span></span>

<span data-ttu-id="b3db3-235">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b3db3-235">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="b3db3-236">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-236">**Mitigations**</span></span>

<span data-ttu-id="b3db3-237">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-237">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="b3db3-238">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-238">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="b3db3-239">DeleteBehavior.Restrict ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="b3db3-239">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="b3db3-240">Śledzenie problem #12661</span><span class="sxs-lookup"><span data-stu-id="b3db3-240">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="b3db3-241">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 5.</span><span class="sxs-lookup"><span data-stu-id="b3db3-241">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="b3db3-242">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-242">**Old behavior**</span></span>

<span data-ttu-id="b3db3-243">Przed 3.0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych o `Restrict` semantykę, ale także zmienione wewnętrznego naprawy w taki sposób,-oczywiste.</span><span class="sxs-lookup"><span data-stu-id="b3db3-243">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="b3db3-244">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-244">**New behavior**</span></span>

<span data-ttu-id="b3db3-245">Począwszy od 3.0 `DeleteBehavior.Restrict` gwarantuje, że klucze obce są tworzone za pomocą `Restrict` semantyki — czyli nie kaskady; throw na naruszenie ograniczenia — bez również wpływ na wewnętrzny naprawy EF.</span><span class="sxs-lookup"><span data-stu-id="b3db3-245">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="b3db3-246">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-246">**Why**</span></span>

<span data-ttu-id="b3db3-247">Ta zmiana została wprowadzona w celu usprawnienie obsługi przy użyciu `DeleteBehavior` w sposób intuicyjny, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-247">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="b3db3-248">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-248">**Mitigations**</span></span>

<span data-ttu-id="b3db3-249">Poprzednie zachowanie można przywrócić za pomocą `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-249">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="b3db3-250">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="b3db3-250">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="b3db3-251">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="b3db3-251">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="b3db3-252">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-252">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-253">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-253">**Old behavior**</span></span>

<span data-ttu-id="b3db3-254">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="b3db3-254">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="b3db3-255">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="b3db3-255">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="b3db3-256">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-256">**New behavior**</span></span>

<span data-ttu-id="b3db3-257">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-257">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="b3db3-258">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="b3db3-258">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="b3db3-259">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-259">**Why**</span></span>

<span data-ttu-id="b3db3-260">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="b3db3-260">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="b3db3-261">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-261">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="b3db3-262">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-262">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="b3db3-263">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-263">**Mitigations**</span></span>

<span data-ttu-id="b3db3-264">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="b3db3-264">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="b3db3-265">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-265">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="b3db3-266">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-266">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="b3db3-267">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="b3db3-267">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="b3db3-268">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="b3db3-268">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="b3db3-269">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="b3db3-269">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="b3db3-270">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="b3db3-270">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="b3db3-271">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-271">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-272">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-272">**Old behavior**</span></span>

<span data-ttu-id="b3db3-273">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-273">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="b3db3-274">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-274">**New behavior**</span></span>

<span data-ttu-id="b3db3-275">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-275">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="b3db3-276">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-276">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="b3db3-277">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="b3db3-277">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="b3db3-278">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-278">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="b3db3-279">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-279">For example:</span></span>

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

<span data-ttu-id="b3db3-280">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="b3db3-280">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="b3db3-281">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-281">**Why**</span></span>

<span data-ttu-id="b3db3-282">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-282">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="b3db3-283">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-283">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="b3db3-284">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-284">**Mitigations**</span></span>

<span data-ttu-id="b3db3-285">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-285">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="b3db3-286">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="b3db3-286">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="b3db3-287">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="b3db3-287">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="b3db3-288">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-288">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-289">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-289">**Old behavior**</span></span>

<span data-ttu-id="b3db3-290">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="b3db3-290">Consider the following model:</span></span>
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
<span data-ttu-id="b3db3-291">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="b3db3-292">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-292">**New behavior**</span></span>

<span data-ttu-id="b3db3-293">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="b3db3-293">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="b3db3-294">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-294">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="b3db3-295">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-295">**Mitigations**</span></span>

<span data-ttu-id="b3db3-296">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-296">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="b3db3-297">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-297">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="b3db3-298">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="b3db3-298">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="b3db3-299">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="b3db3-299">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="b3db3-300">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-300">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-301">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-301">**Old behavior**</span></span>

<span data-ttu-id="b3db3-302">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="b3db3-302">Consider the following model:</span></span>
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
<span data-ttu-id="b3db3-303">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b3db3-303">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="b3db3-304">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-304">**New behavior**</span></span>

<span data-ttu-id="b3db3-305">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-305">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="b3db3-306">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-306">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="b3db3-307">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-307">**Why**</span></span>

<span data-ttu-id="b3db3-308">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b3db3-308">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b3db3-309">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-309">**Mitigations**</span></span>

<span data-ttu-id="b3db3-310">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="b3db3-310">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="b3db3-311">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="b3db3-311">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="b3db3-312">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="b3db3-312">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="b3db3-313">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="b3db3-313">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="b3db3-314">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-314">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-315">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-315">**Old behavior**</span></span>

<span data-ttu-id="b3db3-316">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="b3db3-316">Consider the following model:</span></span>
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

<span data-ttu-id="b3db3-317">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-317">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="b3db3-318">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-318">**New behavior**</span></span>

<span data-ttu-id="b3db3-319">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-319">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="b3db3-320">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-320">**Why**</span></span>

<span data-ttu-id="b3db3-321">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="b3db3-321">The old behavoir was unexpected.</span></span>

<span data-ttu-id="b3db3-322">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-322">**Mitigations**</span></span>

<span data-ttu-id="b3db3-323">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="b3db3-323">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="b3db3-324">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="b3db3-324">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="b3db3-325">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="b3db3-325">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="b3db3-326">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-326">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-327">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-327">**Old behavior**</span></span>

<span data-ttu-id="b3db3-328">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="b3db3-328">Consider the following model:</span></span>
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
<span data-ttu-id="b3db3-329">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="b3db3-329">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="b3db3-330">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="b3db3-330">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="b3db3-331">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-331">**New behavior**</span></span>

<span data-ttu-id="b3db3-332">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="b3db3-332">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="b3db3-333">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="b3db3-333">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="b3db3-334">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-334">For example:</span></span>

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

<span data-ttu-id="b3db3-335">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-335">**Why**</span></span>

<span data-ttu-id="b3db3-336">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-336">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="b3db3-337">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-337">**Mitigations**</span></span>

<span data-ttu-id="b3db3-338">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-338">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="b3db3-339">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="b3db3-339">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="b3db3-340">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="b3db3-340">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="b3db3-341">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-341">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-342">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-342">**Old behavior**</span></span>

<span data-ttu-id="b3db3-343">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="b3db3-343">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="b3db3-344">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-344">**New behavior**</span></span>

<span data-ttu-id="b3db3-345">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-345">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="b3db3-346">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-346">**Why**</span></span>

<span data-ttu-id="b3db3-347">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-347">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="b3db3-348">Nowe zachowanie dopasowuje również EF6.</span><span class="sxs-lookup"><span data-stu-id="b3db3-348">The new behavior also matches EF6.</span></span>

<span data-ttu-id="b3db3-349">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-349">**Mitigations**</span></span>

<span data-ttu-id="b3db3-350">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="b3db3-350">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="b3db3-351">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="b3db3-351">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="b3db3-352">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="b3db3-352">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="b3db3-353">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-353">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-354">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-354">**Old behavior**</span></span>

<span data-ttu-id="b3db3-355">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b3db3-355">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="b3db3-356">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-356">**New behavior**</span></span>

<span data-ttu-id="b3db3-357">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b3db3-357">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="b3db3-358">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="b3db3-358">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="b3db3-359">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-359">**Why**</span></span>

<span data-ttu-id="b3db3-360">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b3db3-360">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="b3db3-361">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-361">**Mitigations**</span></span>

<span data-ttu-id="b3db3-362">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-362">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="b3db3-363">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-363">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="b3db3-364">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="b3db3-364">Backing fields are used by default</span></span>

[<span data-ttu-id="b3db3-365">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="b3db3-365">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="b3db3-366">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="b3db3-366">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3db3-367">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-367">**Old behavior**</span></span>

<span data-ttu-id="b3db3-368">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-368">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="b3db3-369">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="b3db3-369">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="b3db3-370">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-370">**New behavior**</span></span>

<span data-ttu-id="b3db3-371">Zaczynając od programu EF Core 3.0 to, jeśli znane jest pole zapasowe dla właściwości, następnie programu EF Core będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-371">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="b3db3-372">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="b3db3-372">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="b3db3-373">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-373">**Why**</span></span>

<span data-ttu-id="b3db3-374">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="b3db3-374">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="b3db3-375">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-375">**Mitigations**</span></span>

<span data-ttu-id="b3db3-376">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-376">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="b3db3-377">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-377">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="b3db3-378">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="b3db3-378">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="b3db3-379">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="b3db3-379">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="b3db3-380">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-380">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-381">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-381">**Old behavior**</span></span>

<span data-ttu-id="b3db3-382">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="b3db3-382">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="b3db3-383">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="b3db3-383">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="b3db3-384">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-384">**New behavior**</span></span>

<span data-ttu-id="b3db3-385">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="b3db3-385">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="b3db3-386">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-386">**Why**</span></span>

<span data-ttu-id="b3db3-387">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="b3db3-387">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="b3db3-388">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-388">**Mitigations**</span></span>

<span data-ttu-id="b3db3-389">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="b3db3-389">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="b3db3-390">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="b3db3-390">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="b3db3-391">Właściwość tylko do pola nazwy powinny pasować do nazwy pola</span><span class="sxs-lookup"><span data-stu-id="b3db3-391">Field-only property names should match the field name</span></span>

<span data-ttu-id="b3db3-392">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-392">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-393">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-393">**Old behavior**</span></span>

<span data-ttu-id="b3db3-394">Przed programem EF Core 3.0 to właściwość może być określony przez wartość ciągu i jeśli żadnej właściwości o tej nazwie został znaleziony na typ CLR programu EF Core będzie spróbuj dopasować go do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-394">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="b3db3-395">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-395">**New behavior**</span></span>

<span data-ttu-id="b3db3-396">Począwszy od programu EF Core 3.0 to właściwość tylko do pola musi być zgodny nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="b3db3-396">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="b3db3-397">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-397">**Why**</span></span>

<span data-ttu-id="b3db3-398">Ta zmiana została wprowadzona w celu uniknięcia przy użyciu tego samego pola na dwie właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości tylko do pól takie same jak właściwości zamapowanych do właściwości aparatu CLR.</span><span class="sxs-lookup"><span data-stu-id="b3db3-398">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="b3db3-399">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-399">**Mitigations**</span></span>

<span data-ttu-id="b3db3-400">Właściwości tylko do pola musi mieć nazwę taka sama jak pola, które są mapowane.</span><span class="sxs-lookup"><span data-stu-id="b3db3-400">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="b3db3-401">W nowszej wersji zapoznawczej programu EF Core 3.0 planujemy ponownie włączyć konfigurowanie jawnie nazwę pola, która jest inna niż nazwa właściwości:</span><span class="sxs-lookup"><span data-stu-id="b3db3-401">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="b3db3-402">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="b3db3-402">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="b3db3-403">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="b3db3-403">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="b3db3-404">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-404">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-405">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-405">**Old behavior**</span></span>

<span data-ttu-id="b3db3-406">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b3db3-406">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="b3db3-407">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-407">**New behavior**</span></span>

<span data-ttu-id="b3db3-408">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="b3db3-408">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="b3db3-409">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-409">**Why**</span></span>

<span data-ttu-id="b3db3-410">EF Core 3.0 nie wymaga, aby te usługi są w kontenerze DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-410">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="b3db3-411">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3db3-411">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="b3db3-412">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-412">**Mitigations**</span></span>

<span data-ttu-id="b3db3-413">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b3db3-413">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="b3db3-414">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="b3db3-414">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="b3db3-415">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="b3db3-415">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b3db3-416">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-416">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-417">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-417">**Old behavior**</span></span>

<span data-ttu-id="b3db3-418">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="b3db3-418">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="b3db3-419">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="b3db3-419">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="b3db3-420">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-420">**New behavior**</span></span>

<span data-ttu-id="b3db3-421">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-421">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="b3db3-422">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-422">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="b3db3-423">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="b3db3-423">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="b3db3-424">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="b3db3-424">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="b3db3-425">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-425">**Why**</span></span>

<span data-ttu-id="b3db3-426">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-426">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="b3db3-427">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-427">**Mitigations**</span></span>

<span data-ttu-id="b3db3-428">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3db3-428">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="b3db3-429">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="b3db3-429">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="b3db3-430">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="b3db3-430">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="b3db3-431">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-431">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-432">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-432">**Old behavior**</span></span>

<span data-ttu-id="b3db3-433">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="b3db3-433">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="b3db3-434">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-434">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="b3db3-435">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-435">**New behavior**</span></span>

<span data-ttu-id="b3db3-436">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="b3db3-436">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="b3db3-437">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-437">**Why**</span></span>

<span data-ttu-id="b3db3-438">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="b3db3-438">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="b3db3-439">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-439">**Mitigations**</span></span>

<span data-ttu-id="b3db3-440">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="b3db3-440">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="b3db3-441">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="b3db3-441">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="b3db3-442">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="b3db3-442">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="b3db3-443">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="b3db3-443">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="b3db3-444">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="b3db3-444">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="b3db3-445">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-445">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-446">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-446">**Old behavior**</span></span>

<span data-ttu-id="b3db3-447">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-447">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="b3db3-448">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-448">**New behavior**</span></span>

<span data-ttu-id="b3db3-449">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="b3db3-449">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="b3db3-450">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-450">**Why**</span></span>

<span data-ttu-id="b3db3-451">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="b3db3-451">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="b3db3-452">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-452">**Mitigations**</span></span>

<span data-ttu-id="b3db3-453">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3db3-453">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="b3db3-454">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="b3db3-454">This isn't common.</span></span>
<span data-ttu-id="b3db3-455">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="b3db3-455">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="b3db3-456">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-456">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="b3db3-457">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension została scalona z</span><span class="sxs-lookup"><span data-stu-id="b3db3-457">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="b3db3-458">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="b3db3-458">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b3db3-459">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-459">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-460">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-460">**Old behavior**</span></span>

<span data-ttu-id="b3db3-461">`IDbContextOptionsExtensionWithDebugInfo` przedłużono dodatkowy opcjonalny interfejs z `IDbContextOptionsExtension` Aby uniknąć konieczności istotne zmiany w interfejsie podczas cyklu wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="b3db3-461">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="b3db3-462">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-462">**New behavior**</span></span>

<span data-ttu-id="b3db3-463">Interfejsy są teraz scalone razem `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-463">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="b3db3-464">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-464">**Why**</span></span>

<span data-ttu-id="b3db3-465">Ta zmiana została wprowadzona, ponieważ interfejsy są koncepcyjnie jeden.</span><span class="sxs-lookup"><span data-stu-id="b3db3-465">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="b3db3-466">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-466">**Mitigations**</span></span>

<span data-ttu-id="b3db3-467">Wszelkie implementacje `IDbContextOptionsExtension` musi zostać zaktualizowany do obsługi nowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b3db3-467">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="b3db3-468">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="b3db3-468">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="b3db3-469">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="b3db3-469">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="b3db3-470">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-470">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-471">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-471">**Old behavior**</span></span>

<span data-ttu-id="b3db3-472">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="b3db3-472">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="b3db3-473">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="b3db3-473">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="b3db3-474">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="b3db3-474">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="b3db3-475">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-475">**New behavior**</span></span>

<span data-ttu-id="b3db3-476">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="b3db3-476">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="b3db3-477">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="b3db3-477">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="b3db3-478">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="b3db3-478">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="b3db3-479">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="b3db3-479">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="b3db3-480">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-480">**Why**</span></span>

<span data-ttu-id="b3db3-481">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-481">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="b3db3-482">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-482">**Mitigations**</span></span>

<span data-ttu-id="b3db3-483">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="b3db3-483">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="b3db3-484">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="b3db3-484">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="b3db3-485">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="b3db3-485">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="b3db3-486">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-486">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-487">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-487">**Old behavior**</span></span>

<span data-ttu-id="b3db3-488">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="b3db3-488">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="b3db3-489">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-489">**New behavior**</span></span>

<span data-ttu-id="b3db3-490">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="b3db3-490">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="b3db3-491">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-491">**Why**</span></span>

<span data-ttu-id="b3db3-492">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="b3db3-492">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="b3db3-493">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-493">**Mitigations**</span></span>

<span data-ttu-id="b3db3-494">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-494">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="b3db3-495">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-495">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="b3db3-496">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-496">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="b3db3-497">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="b3db3-497">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="b3db3-498">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="b3db3-498">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="b3db3-499">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-499">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-500">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-500">**Old behavior**</span></span>

<span data-ttu-id="b3db3-501">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została interpretowany w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="b3db3-501">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="b3db3-502">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-502">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="b3db3-503">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="b3db3-503">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="b3db3-504">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-504">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="b3db3-505">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-505">**New behavior**</span></span>

<span data-ttu-id="b3db3-506">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="b3db3-506">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="b3db3-507">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-507">**Why**</span></span>

<span data-ttu-id="b3db3-508">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="b3db3-508">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="b3db3-509">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-509">**Mitigations**</span></span>

<span data-ttu-id="b3db3-510">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-510">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="b3db3-511">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="b3db3-511">This is not common.</span></span>
<span data-ttu-id="b3db3-512">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-512">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="b3db3-513">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-513">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="b3db3-514">Typ zwracany dla kilku metod asynchronicznych została zmieniona z zadaniem do ValueTask</span><span class="sxs-lookup"><span data-stu-id="b3db3-514">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="b3db3-515">Śledzenie problem #15184</span><span class="sxs-lookup"><span data-stu-id="b3db3-515">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="b3db3-516">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-516">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-517">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-517">**Old behavior**</span></span>

<span data-ttu-id="b3db3-518">Następujące metody async wcześniej zwrócony `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="b3db3-518">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="b3db3-519">`ValueGenerator.NextValueAsync()` (i wyprowadzanie klas)</span><span class="sxs-lookup"><span data-stu-id="b3db3-519">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="b3db3-520">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-520">**New behavior**</span></span>

<span data-ttu-id="b3db3-521">Wymienione wyżej metod teraz zwracają `ValueTask<T>` w taki sam `T` tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="b3db3-521">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="b3db3-522">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-522">**Why**</span></span>

<span data-ttu-id="b3db3-523">Ta zmiana zmniejsza liczbę alokacji sterty naliczany podczas wywoływania metody te poprawę ogólnej wydajności.</span><span class="sxs-lookup"><span data-stu-id="b3db3-523">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="b3db3-524">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-524">**Mitigations**</span></span>

<span data-ttu-id="b3db3-525">Tylko po prostu oczekujące na powyższym interfejsów API muszą zostać zrekompilowane — aplikacje bez zmian źródła są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="b3db3-525">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="b3db3-526">Bardziej złożonych zastosowaniach (np. przekazywanie zwracanego `Task` do `Task.WhenAny()`) zwykle wymagają, aby zwrócony `ValueTask<T>` można przekonwertować na `Task<T>` przez wywołanie metody `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="b3db3-526">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="b3db3-527">Należy pamiętać, że to neguje zmniejszenia alokacji, które ta zmiana powoduje.</span><span class="sxs-lookup"><span data-stu-id="b3db3-527">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="b3db3-528">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="b3db3-528">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="b3db3-529">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="b3db3-529">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="b3db3-530">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="b3db3-530">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3db3-531">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-531">**Old behavior**</span></span>

<span data-ttu-id="b3db3-532">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b3db3-532">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="b3db3-533">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-533">**New behavior**</span></span>

<span data-ttu-id="b3db3-534">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b3db3-534">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="b3db3-535">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-535">**Why**</span></span>

<span data-ttu-id="b3db3-536">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-536">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="b3db3-537">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-537">**Mitigations**</span></span>

<span data-ttu-id="b3db3-538">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="b3db3-538">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="b3db3-539">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-539">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="b3db3-540">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="b3db3-540">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="b3db3-541">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="b3db3-541">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="b3db3-542">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-542">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-543">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-543">**Old behavior**</span></span>

<span data-ttu-id="b3db3-544">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-544">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="b3db3-545">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-545">**New behavior**</span></span>

<span data-ttu-id="b3db3-546">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-546">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="b3db3-547">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-547">**Why**</span></span>

<span data-ttu-id="b3db3-548">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b3db3-548">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="b3db3-549">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="b3db3-549">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="b3db3-550">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-550">**Mitigations**</span></span>

<span data-ttu-id="b3db3-551">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="b3db3-551">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="b3db3-552">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="b3db3-552">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="b3db3-553">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="b3db3-553">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="b3db3-554">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-554">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-555">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-555">**Old behavior**</span></span>

<span data-ttu-id="b3db3-556">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-556">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="b3db3-557">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-557">**New behavior**</span></span>

<span data-ttu-id="b3db3-558">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-558">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="b3db3-559">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-559">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="b3db3-560">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-560">**Why**</span></span>

<span data-ttu-id="b3db3-561">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Include` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b3db3-561">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="b3db3-562">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-562">**Mitigations**</span></span>

<span data-ttu-id="b3db3-563">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-563">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="b3db3-564">Zmiany metadanych interfejsu API</span><span class="sxs-lookup"><span data-stu-id="b3db3-564">Metadata API changes</span></span>

[<span data-ttu-id="b3db3-565">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="b3db3-565">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b3db3-566">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-566">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-567">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-567">**New behavior**</span></span>

<span data-ttu-id="b3db3-568">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b3db3-568">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="b3db3-569">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-569">**Why**</span></span>

<span data-ttu-id="b3db3-570">Ta zmiana ułatwia implementację interfejsów wyżej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-570">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="b3db3-571">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-571">**Mitigations**</span></span>

<span data-ttu-id="b3db3-572">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-572">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="b3db3-573">Zmiany interfejsu API metadane właściwe dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="b3db3-573">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="b3db3-574">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="b3db3-574">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b3db3-575">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="b3db3-575">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b3db3-576">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-576">**New behavior**</span></span>

<span data-ttu-id="b3db3-577">Metody rozszerzające właściwe dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="b3db3-577">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="b3db3-578">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-578">**Why**</span></span>

<span data-ttu-id="b3db3-579">Ta zmiana ułatwia implementację metody rozszerzenia wyżej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-579">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="b3db3-580">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-580">**Mitigations**</span></span>

<span data-ttu-id="b3db3-581">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-581">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="b3db3-582">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="b3db3-582">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="b3db3-583">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="b3db3-583">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="b3db3-584">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="b3db3-584">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3db3-585">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-585">**Old behavior**</span></span>

<span data-ttu-id="b3db3-586">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3db3-586">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b3db3-587">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-587">**New behavior**</span></span>

<span data-ttu-id="b3db3-588">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3db3-588">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b3db3-589">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-589">**Why**</span></span>

<span data-ttu-id="b3db3-590">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="b3db3-590">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="b3db3-591">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-591">**Mitigations**</span></span>

<span data-ttu-id="b3db3-592">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3db3-592">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="b3db3-593">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="b3db3-593">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="b3db3-594">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b3db3-594">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="b3db3-595">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-595">**Old behavior**</span></span>

<span data-ttu-id="b3db3-596">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-596">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="b3db3-597">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-597">**New behavior**</span></span>

<span data-ttu-id="b3db3-598">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-598">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="b3db3-599">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-599">**Why**</span></span>

<span data-ttu-id="b3db3-600">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="b3db3-600">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="b3db3-601">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-601">**Mitigations**</span></span>

<span data-ttu-id="b3db3-602">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="b3db3-602">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b3db3-603">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="b3db3-603">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b3db3-604">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="b3db3-604">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="b3db3-605">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-605">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-606">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-606">**Old behavior**</span></span>

<span data-ttu-id="b3db3-607">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3db3-607">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="b3db3-608">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-608">**New behavior**</span></span>

<span data-ttu-id="b3db3-609">Wartości identyfikatora GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="b3db3-609">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="b3db3-610">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-610">**Why**</span></span>

<span data-ttu-id="b3db3-611">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="b3db3-611">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="b3db3-612">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="b3db3-612">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b3db3-613">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-613">**Mitigations**</span></span>

<span data-ttu-id="b3db3-614">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-614">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="b3db3-615">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-615">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="b3db3-616">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="b3db3-616">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b3db3-617">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="b3db3-617">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b3db3-618">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="b3db3-618">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="b3db3-619">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-619">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-620">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-620">**Old behavior**</span></span>

<span data-ttu-id="b3db3-621">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3db3-621">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="b3db3-622">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="b3db3-622">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="b3db3-623">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-623">**New behavior**</span></span>

<span data-ttu-id="b3db3-624">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="b3db3-624">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="b3db3-625">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-625">**Why**</span></span>

<span data-ttu-id="b3db3-626">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="b3db3-626">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b3db3-627">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-627">**Mitigations**</span></span>

<span data-ttu-id="b3db3-628">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3db3-628">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="b3db3-629">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="b3db3-629">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="b3db3-630">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="b3db3-630">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="b3db3-631">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="b3db3-631">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="b3db3-632">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="b3db3-632">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="b3db3-633">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-633">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-634">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-634">**Old behavior**</span></span>

<span data-ttu-id="b3db3-635">Migracja identyfikatorów przypadkowo zostały wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="b3db3-635">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="b3db3-636">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-636">**New behavior**</span></span>

<span data-ttu-id="b3db3-637">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="b3db3-637">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="b3db3-638">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-638">**Why**</span></span>

<span data-ttu-id="b3db3-639">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="b3db3-639">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="b3db3-640">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="b3db3-640">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="b3db3-641">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-641">**Mitigations**</span></span>

<span data-ttu-id="b3db3-642">Ta zmiana dotyczy wszystkich osób korzystających z innych kalendarz gregoriański — gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="b3db3-642">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="b3db3-643">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-643">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="b3db3-644">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="b3db3-644">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="b3db3-645">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="b3db3-645">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="b3db3-646">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="b3db3-646">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="b3db3-647">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="b3db3-647">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="b3db3-648">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-648">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-649">**Change**</span><span class="sxs-lookup"><span data-stu-id="b3db3-649">**Change**</span></span>

<span data-ttu-id="b3db3-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="b3db3-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="b3db3-651">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-651">**Why**</span></span>

<span data-ttu-id="b3db3-652">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="b3db3-652">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="b3db3-653">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-653">**Mitigations**</span></span>

<span data-ttu-id="b3db3-654">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-654">Use the new name.</span></span> <span data-ttu-id="b3db3-655">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="b3db3-655">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="b3db3-656">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="b3db3-656">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="b3db3-657">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="b3db3-657">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="b3db3-658">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b3db3-658">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3db3-659">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-659">**Old behavior**</span></span>

<span data-ttu-id="b3db3-660">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="b3db3-660">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="b3db3-661">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-661">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="b3db3-662">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="b3db3-662">**New behavior**</span></span>

<span data-ttu-id="b3db3-663">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "Nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="b3db3-663">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="b3db3-664">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b3db3-664">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="b3db3-665">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="b3db3-665">**Why**</span></span>

<span data-ttu-id="b3db3-666">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego ograniczenia, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="b3db3-666">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="b3db3-667">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="b3db3-667">**Mitigations**</span></span>

<span data-ttu-id="b3db3-668">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="b3db3-668">Use the new name.</span></span>
