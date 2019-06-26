---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394828"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="848b9-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="848b9-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="848b9-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="848b9-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="848b9-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="848b9-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="848b9-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="848b9-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="848b9-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="848b9-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="848b9-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="848b9-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="848b9-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="848b9-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="848b9-109">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-110">**Old behavior**</span></span>

<span data-ttu-id="848b9-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="848b9-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="848b9-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="848b9-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="848b9-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-113">**New behavior**</span></span>

<span data-ttu-id="848b9-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="848b9-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="848b9-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="848b9-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="848b9-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-116">**Why**</span></span>

<span data-ttu-id="848b9-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="848b9-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="848b9-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="848b9-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="848b9-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="848b9-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="848b9-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="848b9-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="848b9-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="848b9-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="848b9-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="848b9-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="848b9-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-123">**Mitigations**</span></span>

<span data-ttu-id="848b9-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="848b9-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="848b9-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="848b9-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="848b9-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="848b9-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="848b9-127">Ta zmiana zostanie wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="848b9-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="848b9-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-128">**Old behavior**</span></span>

<span data-ttu-id="848b9-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="848b9-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="848b9-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-130">**New behavior**</span></span>

<span data-ttu-id="848b9-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="848b9-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="848b9-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-132">**Why**</span></span>

<span data-ttu-id="848b9-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="848b9-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="848b9-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="848b9-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="848b9-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="848b9-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="848b9-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="848b9-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-137">**Mitigations**</span></span>

<span data-ttu-id="848b9-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="848b9-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="848b9-139">Narzędzie wiersza polecenia programu EF Core dotnet ef nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="848b9-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="848b9-140">Śledzenie problem #14016</span><span class="sxs-lookup"><span data-stu-id="848b9-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="848b9-141">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4 i odpowiednia wersja zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="848b9-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="848b9-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-142">**Old behavior**</span></span>

<span data-ttu-id="848b9-143">Przed 3.0 `dotnet ef` narzędzie została uwzględniona w zestawie SDK programu .NET Core i były łatwo dostępne do użycia z poziomu wiersza polecenia z dowolnego projektu bez konieczności wykonania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="848b9-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="848b9-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-144">**New behavior**</span></span>

<span data-ttu-id="848b9-145">Począwszy od 3.0, zestaw SDK platformy .NET nie obejmuje `dotnet ef` narzędzia, dzięki czemu przed jego użyciem trzeba jawnie zainstalować ją jako narzędzie lokalnych lub globalnych.</span><span class="sxs-lookup"><span data-stu-id="848b9-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="848b9-146">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-146">**Why**</span></span>

<span data-ttu-id="848b9-147">Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako regularne narzędzia wiersza polecenia platformy .NET w usłudze NuGet, spójne z faktu, że EF Core 3.0 również są zawsze jest rozpowszechniany jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="848b9-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="848b9-148">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-148">**Mitigations**</span></span>

<span data-ttu-id="848b9-149">Aby można było zarządzać migracji lub szkieletu `DbContext`, zainstaluj `dotnet-ef` przy użyciu `dotnet tool install` polecenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="848b9-150">Aby zainstalować go jako narzędzie globalne, możesz na przykład, wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="848b9-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="848b9-151">Możesz również uzyskać je lokalne narzędzie podczas przywracania zależności projektu, który deklaruje ją jako zależność narzędzia przy użyciu [plik manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="848b9-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="848b9-152">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="848b9-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="848b9-153">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="848b9-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="848b9-154">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-155">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-155">**Old behavior**</span></span>

<span data-ttu-id="848b9-156">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="848b9-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="848b9-157">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-157">**New behavior**</span></span>

<span data-ttu-id="848b9-158">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="848b9-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="848b9-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="848b9-160">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="848b9-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="848b9-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="848b9-162">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="848b9-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="848b9-163">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-163">**Why**</span></span>

<span data-ttu-id="848b9-164">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody nieprzetworzonego ciągu, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="848b9-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="848b9-165">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="848b9-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="848b9-166">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-166">**Mitigations**</span></span>

<span data-ttu-id="848b9-167">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="848b9-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="848b9-168">Metody FromSql można określić tylko na elementy główne zapytanie</span><span class="sxs-lookup"><span data-stu-id="848b9-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="848b9-169">Śledzenie problem #15704</span><span class="sxs-lookup"><span data-stu-id="848b9-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="848b9-170">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="848b9-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="848b9-171">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-171">**Old behavior**</span></span>

<span data-ttu-id="848b9-172">Przed programem EF Core 3.0 to `FromSql` metoda może być określona w dowolnym miejscu w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="848b9-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="848b9-173">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-173">**New behavior**</span></span>

<span data-ttu-id="848b9-174">Począwszy od programu EF Core 3.0 to nowy `FromSqlRaw` i `FromSqlInterpolated` metody (które Zastąp `FromSql`) można określić tylko na katalogi główne zapytanie, czyli bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="848b9-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="848b9-175">Próba określenia ich w jakimkolwiek innym spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="848b9-176">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-176">**Why**</span></span>

<span data-ttu-id="848b9-177">Określanie `FromSql` dowolnym innym niż na `DbSet` nie dodano znaczenie lub wartość dodaną i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="848b9-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="848b9-178">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-178">**Mitigations**</span></span>

<span data-ttu-id="848b9-179">`FromSql` wywołania zostanie przeniesiona za bezpośrednio na `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="848b9-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="848b9-180">~~Wykonanie zapytania jest rejestrowane na poziomie debugowania~~ przywrócono</span><span class="sxs-lookup"><span data-stu-id="848b9-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="848b9-181">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="848b9-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="848b9-182">Ta zmiana zostanie wycofana w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="848b9-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="848b9-183">Możemy przywrócić tę zmianę, ponieważ nowej konfiguracji w programie EF Core 3.0 umożliwia poziom rejestrowania dla dowolnego zdarzenia, należy określić przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="848b9-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="848b9-184">Na przykład, aby przełączyć rejestrowania programu SQL Server, aby `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="848b9-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="848b9-185">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="848b9-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="848b9-186">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="848b9-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="848b9-187">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="848b9-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="848b9-188">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-188">**Old behavior**</span></span>

<span data-ttu-id="848b9-189">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="848b9-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="848b9-190">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="848b9-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="848b9-191">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-191">**New behavior**</span></span>

<span data-ttu-id="848b9-192">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="848b9-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="848b9-193">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-193">**Why**</span></span>

<span data-ttu-id="848b9-194">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="848b9-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="848b9-195">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-195">**Mitigations**</span></span>

<span data-ttu-id="848b9-196">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="848b9-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="848b9-197">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="848b9-197">This can be avoided by:</span></span>
* <span data-ttu-id="848b9-198">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="848b9-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="848b9-199">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="848b9-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="848b9-200">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="848b9-201">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="848b9-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="848b9-202">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="848b9-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="848b9-203">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="848b9-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="848b9-204">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-205">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-205">**Old behavior**</span></span>

<span data-ttu-id="848b9-206">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="848b9-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="848b9-207">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-207">**New behavior**</span></span>

<span data-ttu-id="848b9-208">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="848b9-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="848b9-209">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="848b9-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="848b9-210">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="848b9-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="848b9-211">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-211">**Why**</span></span>

<span data-ttu-id="848b9-212">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="848b9-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="848b9-213">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-213">**Mitigations**</span></span>

<span data-ttu-id="848b9-214">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="848b9-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="848b9-215">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="848b9-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="848b9-216">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="848b9-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="848b9-217">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="848b9-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="848b9-218">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="848b9-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="848b9-219">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="848b9-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="848b9-220">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-221">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-221">**Old behavior**</span></span>

<span data-ttu-id="848b9-222">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="848b9-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="848b9-223">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-223">**New behavior**</span></span>

<span data-ttu-id="848b9-224">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="848b9-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="848b9-225">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="848b9-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="848b9-226">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-226">**Why**</span></span>

<span data-ttu-id="848b9-227">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="848b9-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="848b9-228">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-228">**Mitigations**</span></span>

<span data-ttu-id="848b9-229">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="848b9-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="848b9-230">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="848b9-231">DeleteBehavior.Restrict ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="848b9-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="848b9-232">Śledzenie problem #12661</span><span class="sxs-lookup"><span data-stu-id="848b9-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="848b9-233">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 5.</span><span class="sxs-lookup"><span data-stu-id="848b9-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="848b9-234">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-234">**Old behavior**</span></span>

<span data-ttu-id="848b9-235">Przed 3.0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych o `Restrict` semantykę, ale także zmienione wewnętrznego naprawy w taki sposób,-oczywiste.</span><span class="sxs-lookup"><span data-stu-id="848b9-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="848b9-236">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-236">**New behavior**</span></span>

<span data-ttu-id="848b9-237">Począwszy od 3.0 `DeleteBehavior.Restrict` gwarantuje, że klucze obce są tworzone za pomocą `Restrict` semantyki — czyli nie kaskady; throw na naruszenie ograniczenia — bez również wpływ na wewnętrzny naprawy EF.</span><span class="sxs-lookup"><span data-stu-id="848b9-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="848b9-238">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-238">**Why**</span></span>

<span data-ttu-id="848b9-239">Ta zmiana została wprowadzona w celu usprawnienie obsługi przy użyciu `DeleteBehavior` w sposób intuicyjny, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="848b9-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="848b9-240">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-240">**Mitigations**</span></span>

<span data-ttu-id="848b9-241">Poprzednie zachowanie można przywrócić za pomocą `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="848b9-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="848b9-242">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="848b9-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="848b9-243">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="848b9-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="848b9-244">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-245">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-245">**Old behavior**</span></span>

<span data-ttu-id="848b9-246">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="848b9-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="848b9-247">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="848b9-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="848b9-248">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-248">**New behavior**</span></span>

<span data-ttu-id="848b9-249">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="848b9-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="848b9-250">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="848b9-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="848b9-251">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-251">**Why**</span></span>

<span data-ttu-id="848b9-252">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="848b9-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="848b9-253">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="848b9-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="848b9-254">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="848b9-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="848b9-255">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-255">**Mitigations**</span></span>

<span data-ttu-id="848b9-256">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="848b9-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="848b9-257">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="848b9-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="848b9-258">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="848b9-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="848b9-259">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="848b9-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="848b9-260">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="848b9-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="848b9-261">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="848b9-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="848b9-262">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="848b9-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="848b9-263">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-264">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-264">**Old behavior**</span></span>

<span data-ttu-id="848b9-265">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="848b9-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="848b9-266">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-266">**New behavior**</span></span>

<span data-ttu-id="848b9-267">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="848b9-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="848b9-268">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="848b9-269">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="848b9-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="848b9-270">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="848b9-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="848b9-271">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-271">For example:</span></span>

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

<span data-ttu-id="848b9-272">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="848b9-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="848b9-273">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-273">**Why**</span></span>

<span data-ttu-id="848b9-274">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="848b9-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="848b9-275">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="848b9-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="848b9-276">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-276">**Mitigations**</span></span>

<span data-ttu-id="848b9-277">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="848b9-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="848b9-278">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="848b9-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="848b9-279">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="848b9-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="848b9-280">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-281">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-281">**Old behavior**</span></span>

<span data-ttu-id="848b9-282">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="848b9-282">Consider the following model:</span></span>
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
<span data-ttu-id="848b9-283">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="848b9-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="848b9-284">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-284">**New behavior**</span></span>

<span data-ttu-id="848b9-285">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="848b9-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="848b9-286">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="848b9-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="848b9-287">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-287">**Mitigations**</span></span>

<span data-ttu-id="848b9-288">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="848b9-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="848b9-289">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="848b9-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="848b9-290">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="848b9-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="848b9-291">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="848b9-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="848b9-292">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-293">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-293">**Old behavior**</span></span>

<span data-ttu-id="848b9-294">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="848b9-294">Consider the following model:</span></span>
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
<span data-ttu-id="848b9-295">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="848b9-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="848b9-296">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-296">**New behavior**</span></span>

<span data-ttu-id="848b9-297">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="848b9-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="848b9-298">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="848b9-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="848b9-299">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-299">**Why**</span></span>

<span data-ttu-id="848b9-300">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="848b9-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="848b9-301">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-301">**Mitigations**</span></span>

<span data-ttu-id="848b9-302">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="848b9-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="848b9-303">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="848b9-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="848b9-304">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="848b9-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="848b9-305">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="848b9-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="848b9-306">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-307">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-307">**Old behavior**</span></span>

<span data-ttu-id="848b9-308">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="848b9-308">Consider the following model:</span></span>
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

<span data-ttu-id="848b9-309">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="848b9-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="848b9-310">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-310">**New behavior**</span></span>

<span data-ttu-id="848b9-311">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="848b9-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="848b9-312">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-312">**Why**</span></span>

<span data-ttu-id="848b9-313">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="848b9-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="848b9-314">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-314">**Mitigations**</span></span>

<span data-ttu-id="848b9-315">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="848b9-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="848b9-316">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="848b9-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="848b9-317">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="848b9-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="848b9-318">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-319">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-319">**Old behavior**</span></span>

<span data-ttu-id="848b9-320">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="848b9-320">Consider the following model:</span></span>
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
<span data-ttu-id="848b9-321">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="848b9-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="848b9-322">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="848b9-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="848b9-323">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-323">**New behavior**</span></span>

<span data-ttu-id="848b9-324">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="848b9-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="848b9-325">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="848b9-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="848b9-326">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-326">For example:</span></span>

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

<span data-ttu-id="848b9-327">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-327">**Why**</span></span>

<span data-ttu-id="848b9-328">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="848b9-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="848b9-329">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-329">**Mitigations**</span></span>

<span data-ttu-id="848b9-330">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="848b9-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="848b9-331">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="848b9-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="848b9-332">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="848b9-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="848b9-333">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-334">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-334">**Old behavior**</span></span>

<span data-ttu-id="848b9-335">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="848b9-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="848b9-336">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-336">**New behavior**</span></span>

<span data-ttu-id="848b9-337">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="848b9-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="848b9-338">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-338">**Why**</span></span>

<span data-ttu-id="848b9-339">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="848b9-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="848b9-340">Nowe zachowanie dopasowuje również EF6.</span><span class="sxs-lookup"><span data-stu-id="848b9-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="848b9-341">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-341">**Mitigations**</span></span>

<span data-ttu-id="848b9-342">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="848b9-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="848b9-343">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="848b9-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="848b9-344">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="848b9-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="848b9-345">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-346">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-346">**Old behavior**</span></span>

<span data-ttu-id="848b9-347">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="848b9-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="848b9-348">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-348">**New behavior**</span></span>

<span data-ttu-id="848b9-349">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="848b9-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="848b9-350">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="848b9-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="848b9-351">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-351">**Why**</span></span>

<span data-ttu-id="848b9-352">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="848b9-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="848b9-353">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-353">**Mitigations**</span></span>

<span data-ttu-id="848b9-354">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="848b9-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="848b9-355">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="848b9-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="848b9-356">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="848b9-356">Backing fields are used by default</span></span>

[<span data-ttu-id="848b9-357">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="848b9-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="848b9-358">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="848b9-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="848b9-359">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-359">**Old behavior**</span></span>

<span data-ttu-id="848b9-360">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="848b9-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="848b9-361">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="848b9-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="848b9-362">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-362">**New behavior**</span></span>

<span data-ttu-id="848b9-363">Zaczynając od programu EF Core 3.0 to, jeśli znane jest pole zapasowe dla właściwości, następnie programu EF Core będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="848b9-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="848b9-364">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="848b9-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="848b9-365">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-365">**Why**</span></span>

<span data-ttu-id="848b9-366">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="848b9-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="848b9-367">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-367">**Mitigations**</span></span>

<span data-ttu-id="848b9-368">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="848b9-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="848b9-369">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="848b9-370">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="848b9-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="848b9-371">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="848b9-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="848b9-372">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-373">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-373">**Old behavior**</span></span>

<span data-ttu-id="848b9-374">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="848b9-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="848b9-375">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="848b9-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="848b9-376">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-376">**New behavior**</span></span>

<span data-ttu-id="848b9-377">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="848b9-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="848b9-378">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-378">**Why**</span></span>

<span data-ttu-id="848b9-379">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="848b9-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="848b9-380">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-380">**Mitigations**</span></span>

<span data-ttu-id="848b9-381">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="848b9-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="848b9-382">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="848b9-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="848b9-383">Właściwość tylko do pola nazwy powinny pasować do nazwy pola</span><span class="sxs-lookup"><span data-stu-id="848b9-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="848b9-384">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-385">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-385">**Old behavior**</span></span>

<span data-ttu-id="848b9-386">Przed programem EF Core 3.0 to właściwość może być określony przez wartość ciągu i jeśli żadnej właściwości o tej nazwie został znaleziony na typ CLR programu EF Core będzie spróbuj dopasować go do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="848b9-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="848b9-387">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-387">**New behavior**</span></span>

<span data-ttu-id="848b9-388">Począwszy od programu EF Core 3.0 to właściwość tylko do pola musi być zgodny nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="848b9-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="848b9-389">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-389">**Why**</span></span>

<span data-ttu-id="848b9-390">Ta zmiana została wprowadzona w celu uniknięcia przy użyciu tego samego pola na dwie właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości tylko do pól takie same jak właściwości zamapowanych do właściwości aparatu CLR.</span><span class="sxs-lookup"><span data-stu-id="848b9-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="848b9-391">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-391">**Mitigations**</span></span>

<span data-ttu-id="848b9-392">Właściwości tylko do pola musi mieć nazwę taka sama jak pola, które są mapowane.</span><span class="sxs-lookup"><span data-stu-id="848b9-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="848b9-393">W nowszej wersji zapoznawczej programu EF Core 3.0 planujemy ponownie włączyć konfigurowanie jawnie nazwę pola, która jest inna niż nazwa właściwości:</span><span class="sxs-lookup"><span data-stu-id="848b9-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="848b9-394">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="848b9-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="848b9-395">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="848b9-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="848b9-396">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-397">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-397">**Old behavior**</span></span>

<span data-ttu-id="848b9-398">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="848b9-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="848b9-399">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-399">**New behavior**</span></span>

<span data-ttu-id="848b9-400">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="848b9-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="848b9-401">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-401">**Why**</span></span>

<span data-ttu-id="848b9-402">EF Core 3.0 nie wymaga, aby te usługi są w kontenerze DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="848b9-403">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="848b9-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="848b9-404">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-404">**Mitigations**</span></span>

<span data-ttu-id="848b9-405">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="848b9-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="848b9-406">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="848b9-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="848b9-407">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="848b9-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="848b9-408">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-409">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-409">**Old behavior**</span></span>

<span data-ttu-id="848b9-410">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="848b9-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="848b9-411">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="848b9-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="848b9-412">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-412">**New behavior**</span></span>

<span data-ttu-id="848b9-413">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="848b9-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="848b9-414">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="848b9-415">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="848b9-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="848b9-416">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="848b9-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="848b9-417">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-417">**Why**</span></span>

<span data-ttu-id="848b9-418">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="848b9-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="848b9-419">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-419">**Mitigations**</span></span>

<span data-ttu-id="848b9-420">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="848b9-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="848b9-421">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="848b9-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="848b9-422">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="848b9-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="848b9-423">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-424">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-424">**Old behavior**</span></span>

<span data-ttu-id="848b9-425">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="848b9-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="848b9-426">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="848b9-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="848b9-427">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-427">**New behavior**</span></span>

<span data-ttu-id="848b9-428">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="848b9-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="848b9-429">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-429">**Why**</span></span>

<span data-ttu-id="848b9-430">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="848b9-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="848b9-431">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-431">**Mitigations**</span></span>

<span data-ttu-id="848b9-432">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="848b9-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="848b9-433">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="848b9-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="848b9-434">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="848b9-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="848b9-435">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="848b9-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="848b9-436">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="848b9-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="848b9-437">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-438">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-438">**Old behavior**</span></span>

<span data-ttu-id="848b9-439">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="848b9-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="848b9-440">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-440">**New behavior**</span></span>

<span data-ttu-id="848b9-441">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="848b9-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="848b9-442">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-442">**Why**</span></span>

<span data-ttu-id="848b9-443">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="848b9-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="848b9-444">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-444">**Mitigations**</span></span>

<span data-ttu-id="848b9-445">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="848b9-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="848b9-446">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="848b9-446">This isn't common.</span></span>
<span data-ttu-id="848b9-447">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="848b9-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="848b9-448">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="848b9-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="848b9-449">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="848b9-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="848b9-450">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="848b9-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="848b9-451">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-452">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-452">**Old behavior**</span></span>

<span data-ttu-id="848b9-453">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="848b9-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="848b9-454">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="848b9-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="848b9-455">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="848b9-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="848b9-456">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-456">**New behavior**</span></span>

<span data-ttu-id="848b9-457">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="848b9-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="848b9-458">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="848b9-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="848b9-459">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="848b9-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="848b9-460">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="848b9-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="848b9-461">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-461">**Why**</span></span>

<span data-ttu-id="848b9-462">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="848b9-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="848b9-463">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-463">**Mitigations**</span></span>

<span data-ttu-id="848b9-464">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="848b9-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="848b9-465">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="848b9-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="848b9-466">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="848b9-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="848b9-467">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-468">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-468">**Old behavior**</span></span>

<span data-ttu-id="848b9-469">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="848b9-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="848b9-470">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-470">**New behavior**</span></span>

<span data-ttu-id="848b9-471">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="848b9-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="848b9-472">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-472">**Why**</span></span>

<span data-ttu-id="848b9-473">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="848b9-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="848b9-474">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-474">**Mitigations**</span></span>

<span data-ttu-id="848b9-475">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="848b9-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="848b9-476">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="848b9-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="848b9-477">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="848b9-478">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="848b9-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="848b9-479">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="848b9-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="848b9-480">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-481">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-481">**Old behavior**</span></span>

<span data-ttu-id="848b9-482">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została interpretowany w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="848b9-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="848b9-483">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="848b9-484">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="848b9-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="848b9-485">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="848b9-486">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-486">**New behavior**</span></span>

<span data-ttu-id="848b9-487">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="848b9-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="848b9-488">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-488">**Why**</span></span>

<span data-ttu-id="848b9-489">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="848b9-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="848b9-490">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-490">**Mitigations**</span></span>

<span data-ttu-id="848b9-491">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="848b9-492">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="848b9-492">This is not common.</span></span>
<span data-ttu-id="848b9-493">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="848b9-494">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="848b9-495">Typ zwracany dla kilku metod asynchronicznych została zmieniona z zadaniem do ValueTask</span><span class="sxs-lookup"><span data-stu-id="848b9-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="848b9-496">Śledzenie problem #15184</span><span class="sxs-lookup"><span data-stu-id="848b9-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="848b9-497">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-498">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-498">**Old behavior**</span></span>

<span data-ttu-id="848b9-499">Następujące metody async wcześniej zwrócony `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="848b9-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="848b9-500">`ValueGenerator.NextValueAsync()` (i wyprowadzanie klas)</span><span class="sxs-lookup"><span data-stu-id="848b9-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="848b9-501">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-501">**New behavior**</span></span>

<span data-ttu-id="848b9-502">Wymienione wyżej metod teraz zwracają `ValueTask<T>` w taki sam `T` tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="848b9-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="848b9-503">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-503">**Why**</span></span>

<span data-ttu-id="848b9-504">Ta zmiana zmniejsza liczbę alokacji sterty naliczany podczas wywoływania metody te poprawę ogólnej wydajności.</span><span class="sxs-lookup"><span data-stu-id="848b9-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="848b9-505">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-505">**Mitigations**</span></span>

<span data-ttu-id="848b9-506">Tylko po prostu oczekujące na powyższym interfejsów API muszą zostać zrekompilowane — aplikacje bez zmian źródła są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="848b9-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="848b9-507">Bardziej złożonych zastosowaniach (np. przekazywanie zwracanego `Task` do `Task.WhenAny()`) zwykle wymagają, aby zwrócony `ValueTask<T>` można przekonwertować na `Task<T>` przez wywołanie metody `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="848b9-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="848b9-508">Należy pamiętać, że to neguje zmniejszenia alokacji, które ta zmiana powoduje.</span><span class="sxs-lookup"><span data-stu-id="848b9-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="848b9-509">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="848b9-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="848b9-510">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="848b9-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="848b9-511">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="848b9-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="848b9-512">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-512">**Old behavior**</span></span>

<span data-ttu-id="848b9-513">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="848b9-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="848b9-514">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-514">**New behavior**</span></span>

<span data-ttu-id="848b9-515">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="848b9-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="848b9-516">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-516">**Why**</span></span>

<span data-ttu-id="848b9-517">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="848b9-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="848b9-518">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-518">**Mitigations**</span></span>

<span data-ttu-id="848b9-519">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="848b9-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="848b9-520">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="848b9-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="848b9-521">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="848b9-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="848b9-522">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="848b9-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="848b9-523">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-524">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-524">**Old behavior**</span></span>

<span data-ttu-id="848b9-525">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="848b9-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="848b9-526">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-526">**New behavior**</span></span>

<span data-ttu-id="848b9-527">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="848b9-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="848b9-528">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-528">**Why**</span></span>

<span data-ttu-id="848b9-529">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="848b9-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="848b9-530">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="848b9-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="848b9-531">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-531">**Mitigations**</span></span>

<span data-ttu-id="848b9-532">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="848b9-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="848b9-533">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="848b9-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="848b9-534">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="848b9-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="848b9-535">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-536">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-536">**Old behavior**</span></span>

<span data-ttu-id="848b9-537">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="848b9-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="848b9-538">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-538">**New behavior**</span></span>

<span data-ttu-id="848b9-539">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="848b9-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="848b9-540">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="848b9-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="848b9-541">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-541">**Why**</span></span>

<span data-ttu-id="848b9-542">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Include` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="848b9-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="848b9-543">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-543">**Mitigations**</span></span>

<span data-ttu-id="848b9-544">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="848b9-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="848b9-545">Zmiany metadanych interfejsu API</span><span class="sxs-lookup"><span data-stu-id="848b9-545">Metadata API changes</span></span>

[<span data-ttu-id="848b9-546">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="848b9-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="848b9-547">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-548">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-548">**New behavior**</span></span>

<span data-ttu-id="848b9-549">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="848b9-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="848b9-550">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-550">**Why**</span></span>

<span data-ttu-id="848b9-551">Ta zmiana ułatwia implementację interfejsów wyżej.</span><span class="sxs-lookup"><span data-stu-id="848b9-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="848b9-552">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-552">**Mitigations**</span></span>

<span data-ttu-id="848b9-553">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="848b9-554">Zmiany interfejsu API metadane właściwe dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="848b9-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="848b9-555">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="848b9-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="848b9-556">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="848b9-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="848b9-557">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-557">**New behavior**</span></span>

<span data-ttu-id="848b9-558">Metody rozszerzające właściwe dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="848b9-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="848b9-559">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-559">**Why**</span></span>

<span data-ttu-id="848b9-560">Ta zmiana ułatwia implementację metody rozszerzenia wyżej.</span><span class="sxs-lookup"><span data-stu-id="848b9-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="848b9-561">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-561">**Mitigations**</span></span>

<span data-ttu-id="848b9-562">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="848b9-563">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="848b9-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="848b9-564">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="848b9-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="848b9-565">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="848b9-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="848b9-566">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-566">**Old behavior**</span></span>

<span data-ttu-id="848b9-567">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="848b9-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="848b9-568">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-568">**New behavior**</span></span>

<span data-ttu-id="848b9-569">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="848b9-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="848b9-570">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-570">**Why**</span></span>

<span data-ttu-id="848b9-571">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="848b9-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="848b9-572">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-572">**Mitigations**</span></span>

<span data-ttu-id="848b9-573">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="848b9-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="848b9-574">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="848b9-575">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="848b9-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="848b9-576">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-576">**Old behavior**</span></span>

<span data-ttu-id="848b9-577">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="848b9-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="848b9-578">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-578">**New behavior**</span></span>

<span data-ttu-id="848b9-579">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="848b9-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="848b9-580">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-580">**Why**</span></span>

<span data-ttu-id="848b9-581">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="848b9-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="848b9-582">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-582">**Mitigations**</span></span>

<span data-ttu-id="848b9-583">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="848b9-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="848b9-584">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="848b9-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="848b9-585">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="848b9-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="848b9-586">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-587">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-587">**Old behavior**</span></span>

<span data-ttu-id="848b9-588">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="848b9-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="848b9-589">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-589">**New behavior**</span></span>

<span data-ttu-id="848b9-590">Wartości identyfikatora GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="848b9-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="848b9-591">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-591">**Why**</span></span>

<span data-ttu-id="848b9-592">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="848b9-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="848b9-593">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="848b9-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="848b9-594">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-594">**Mitigations**</span></span>

<span data-ttu-id="848b9-595">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="848b9-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="848b9-596">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="848b9-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="848b9-597">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="848b9-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="848b9-598">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="848b9-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="848b9-599">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="848b9-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="848b9-600">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-601">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-601">**Old behavior**</span></span>

<span data-ttu-id="848b9-602">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="848b9-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="848b9-603">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="848b9-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="848b9-604">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-604">**New behavior**</span></span>

<span data-ttu-id="848b9-605">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="848b9-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="848b9-606">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-606">**Why**</span></span>

<span data-ttu-id="848b9-607">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="848b9-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="848b9-608">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-608">**Mitigations**</span></span>

<span data-ttu-id="848b9-609">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="848b9-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="848b9-610">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="848b9-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="848b9-611">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="848b9-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="848b9-612">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="848b9-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="848b9-613">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="848b9-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="848b9-614">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-615">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-615">**Old behavior**</span></span>

<span data-ttu-id="848b9-616">Migracja identyfikatorów przypadkowo zostały wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="848b9-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="848b9-617">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-617">**New behavior**</span></span>

<span data-ttu-id="848b9-618">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="848b9-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="848b9-619">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-619">**Why**</span></span>

<span data-ttu-id="848b9-620">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="848b9-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="848b9-621">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="848b9-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="848b9-622">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-622">**Mitigations**</span></span>

<span data-ttu-id="848b9-623">Ta zmiana dotyczy wszystkich osób korzystających z innych kalendarz gregoriański — gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="848b9-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="848b9-624">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="848b9-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="848b9-625">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="848b9-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="848b9-626">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="848b9-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="848b9-627">Metadane informacji rozszerzenia została usunięta z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="848b9-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="848b9-628">Śledzenie problem #16119</span><span class="sxs-lookup"><span data-stu-id="848b9-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="848b9-629">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="848b9-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="848b9-630">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-630">**Old behavior**</span></span>

<span data-ttu-id="848b9-631">`IDbContextOptionsExtension` zawiera metody dostarczania metadanych dotyczących rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="848b9-632">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-632">**New behavior**</span></span>

<span data-ttu-id="848b9-633">Te metody zostały przeniesione na nowy `DbContextOptionsExtensionInfo` abstrakcyjna klasa bazowa, która jest zwracana z nową `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="848b9-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="848b9-634">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-634">**Why**</span></span>

<span data-ttu-id="848b9-635">W wersji 2.0, 3.0 Musieliśmy dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="848b9-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="848b9-636">Przerwanie ich do nowego abstrakcyjna klasa bazowa ułatwi zapewnienie te rodzaje zmian bez przerywania istniejące rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="848b9-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="848b9-637">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-637">**Mitigations**</span></span>

<span data-ttu-id="848b9-638">Aktualizowanie rozszerzeń do wykonania nowego wzorca.</span><span class="sxs-lookup"><span data-stu-id="848b9-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="848b9-639">Przykłady znajdują się w wielu implementacjach `IDbContextOptionsExtension` kod źródłowy dla różnych rodzajów rozszerzeń w programie EF Core.</span><span class="sxs-lookup"><span data-stu-id="848b9-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="848b9-640">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="848b9-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="848b9-641">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="848b9-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="848b9-642">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-643">**Change**</span><span class="sxs-lookup"><span data-stu-id="848b9-643">**Change**</span></span>

<span data-ttu-id="848b9-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="848b9-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="848b9-645">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-645">**Why**</span></span>

<span data-ttu-id="848b9-646">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="848b9-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="848b9-647">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-647">**Mitigations**</span></span>

<span data-ttu-id="848b9-648">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="848b9-648">Use the new name.</span></span> <span data-ttu-id="848b9-649">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="848b9-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="848b9-650">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="848b9-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="848b9-651">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="848b9-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="848b9-652">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-653">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-653">**Old behavior**</span></span>

<span data-ttu-id="848b9-654">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="848b9-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="848b9-655">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="848b9-656">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-656">**New behavior**</span></span>

<span data-ttu-id="848b9-657">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "Nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="848b9-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="848b9-658">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="848b9-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="848b9-659">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-659">**Why**</span></span>

<span data-ttu-id="848b9-660">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego ograniczenia, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="848b9-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="848b9-661">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-661">**Mitigations**</span></span>

<span data-ttu-id="848b9-662">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="848b9-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="848b9-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync wprowadzono publiczne</span><span class="sxs-lookup"><span data-stu-id="848b9-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="848b9-664">Śledzenie problem #15997</span><span class="sxs-lookup"><span data-stu-id="848b9-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="848b9-665">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="848b9-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="848b9-666">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-666">**Old behavior**</span></span>

<span data-ttu-id="848b9-667">Przed programem EF Core 3.0 to chronione zostały tych metod.</span><span class="sxs-lookup"><span data-stu-id="848b9-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="848b9-668">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-668">**New behavior**</span></span>

<span data-ttu-id="848b9-669">Począwszy od programu EF Core 3.0 te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="848b9-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="848b9-670">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-670">**Why**</span></span>

<span data-ttu-id="848b9-671">Te metody są używane przez EF do ustalenia, czy bazy danych jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="848b9-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="848b9-672">Może to być również przydatne z poza EF podczas ustalania, czy należy zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="848b9-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="848b9-673">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-673">**Mitigations**</span></span>

<span data-ttu-id="848b9-674">Zmień dostępność elementu zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="848b9-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="848b9-675">Microsoft.EntityFrameworkCore.Design jest teraz pakiet DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="848b9-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="848b9-676">Śledzenie problem #11506</span><span class="sxs-lookup"><span data-stu-id="848b9-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="848b9-677">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="848b9-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="848b9-678">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-678">**Old behavior**</span></span>

<span data-ttu-id="848b9-679">Przed programem EF Core 3.0 to Microsoft.EntityFrameworkCore.Design był regularne pakietu NuGet zestawu, którego można odwoływać się projekty, które zależą od jej.</span><span class="sxs-lookup"><span data-stu-id="848b9-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="848b9-680">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-680">**New behavior**</span></span>

<span data-ttu-id="848b9-681">Począwszy od programu EF Core 3.0 to jest pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="848b9-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="848b9-682">Co oznacza, że zależność nie została przechodnio przepływu do innych projektów i nie będzie mógł, domyślnie odwoływać się do własnego zestawu.</span><span class="sxs-lookup"><span data-stu-id="848b9-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="848b9-683">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-683">**Why**</span></span>

<span data-ttu-id="848b9-684">Ten pakiet jest przeznaczona tylko do użycia w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="848b9-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="848b9-685">Wdrożone aplikacje nie powinny odwoływać się do niego.</span><span class="sxs-lookup"><span data-stu-id="848b9-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="848b9-686">Tworzenie pakietu DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="848b9-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="848b9-687">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-687">**Mitigations**</span></span>

<span data-ttu-id="848b9-688">Jeśli musisz odwoływać się do tego pakietu, aby zastąpić zachowanie czasu projektowania programu EF Core, możesz zaktualizować aktualizacji metadanych elementu PackageReference w projekcie.</span><span class="sxs-lookup"><span data-stu-id="848b9-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="848b9-689">Jeśli pakiet jest ono przywoływane przechodni, za pośrednictwem Microsoft.EntityFrameworkCore.Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadanych.</span><span class="sxs-lookup"><span data-stu-id="848b9-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="848b9-690">Zaktualizowany do wersji 2.0.0 SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="848b9-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="848b9-691">Śledzenie problem #14824</span><span class="sxs-lookup"><span data-stu-id="848b9-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="848b9-692">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="848b9-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="848b9-693">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-693">**Old behavior**</span></span>

<span data-ttu-id="848b9-694">Microsoft.EntityFrameworkCore.Sqlite wcześniej zależała od wersji 1.1.12 SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="848b9-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="848b9-695">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="848b9-695">**New behavior**</span></span>

<span data-ttu-id="848b9-696">Aktualizujemy już naszego pakietu są zależne od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="848b9-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="848b9-697">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="848b9-697">**Why**</span></span>

<span data-ttu-id="848b9-698">W wersji 2.0.0 SQLitePCL.raw jest przeznaczony dla .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="848b9-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="848b9-699">Wskazanych .NET 1.1 standardowe, wymagająca dużych zamknięcia przechodnie pakietów do pracy.</span><span class="sxs-lookup"><span data-stu-id="848b9-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="848b9-700">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="848b9-700">**Mitigations**</span></span>

<span data-ttu-id="848b9-701">Wersja SQLitePCL.raw 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="848b9-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="848b9-702">Zobacz [informacje o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="848b9-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
