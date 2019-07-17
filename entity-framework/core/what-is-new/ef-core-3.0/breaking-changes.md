---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286493"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="33dc6-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="33dc6-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33dc6-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="33dc6-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="33dc6-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="33dc6-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="33dc6-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="33dc6-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="33dc6-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="33dc6-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="33dc6-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="33dc6-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="33dc6-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="33dc6-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="33dc6-109">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-110">**Old behavior**</span></span>

<span data-ttu-id="33dc6-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="33dc6-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="33dc6-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="33dc6-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-113">**New behavior**</span></span>

<span data-ttu-id="33dc6-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="33dc6-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="33dc6-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="33dc6-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="33dc6-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-116">**Why**</span></span>

<span data-ttu-id="33dc6-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="33dc6-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="33dc6-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="33dc6-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="33dc6-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="33dc6-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="33dc6-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="33dc6-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="33dc6-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="33dc6-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-123">**Mitigations**</span></span>

<span data-ttu-id="33dc6-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="33dc6-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="33dc6-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33dc6-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="33dc6-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="33dc6-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="33dc6-127">Ta zmiana zostanie wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="33dc6-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="33dc6-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-128">**Old behavior**</span></span>

<span data-ttu-id="33dc6-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="33dc6-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="33dc6-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-130">**New behavior**</span></span>

<span data-ttu-id="33dc6-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="33dc6-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-132">**Why**</span></span>

<span data-ttu-id="33dc6-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="33dc6-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="33dc6-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="33dc6-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="33dc6-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="33dc6-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-137">**Mitigations**</span></span>

<span data-ttu-id="33dc6-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="33dc6-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="33dc6-139">Narzędzie wiersza polecenia programu EF Core dotnet ef nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="33dc6-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="33dc6-140">Śledzenie problem #14016</span><span class="sxs-lookup"><span data-stu-id="33dc6-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="33dc6-141">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4 i odpowiednia wersja zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="33dc6-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="33dc6-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-142">**Old behavior**</span></span>

<span data-ttu-id="33dc6-143">Przed 3.0 `dotnet ef` narzędzie została uwzględniona w zestawie SDK programu .NET Core i były łatwo dostępne do użycia z poziomu wiersza polecenia z dowolnego projektu bez konieczności wykonania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="33dc6-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="33dc6-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-144">**New behavior**</span></span>

<span data-ttu-id="33dc6-145">Począwszy od 3.0, zestaw SDK platformy .NET nie obejmuje `dotnet ef` narzędzia, dzięki czemu przed jego użyciem trzeba jawnie zainstalować ją jako narzędzie lokalnych lub globalnych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="33dc6-146">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-146">**Why**</span></span>

<span data-ttu-id="33dc6-147">Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako regularne narzędzia wiersza polecenia platformy .NET w usłudze NuGet, spójne z faktu, że EF Core 3.0 również są zawsze jest rozpowszechniany jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="33dc6-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="33dc6-148">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-148">**Mitigations**</span></span>

<span data-ttu-id="33dc6-149">Aby można było zarządzać migracji lub szkieletu `DbContext`, zainstaluj `dotnet-ef` jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="33dc6-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="33dc6-150">Możesz również uzyskać je lokalne narzędzie podczas przywracania zależności projektu, który deklaruje ją jako zależność narzędzia przy użyciu [plik manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="33dc6-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="33dc6-151">Zmieniono FromSql ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="33dc6-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="33dc6-152">Śledzenie problem #10996</span><span class="sxs-lookup"><span data-stu-id="33dc6-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="33dc6-153">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-154">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-154">**Old behavior**</span></span>

<span data-ttu-id="33dc6-155">Przed programem EF Core 3.0 to te metody mają nazwy były przeciążone do pracy z normalnym ciągu lub ciąg, który powinien być interpolowane SQL i parametry.</span><span class="sxs-lookup"><span data-stu-id="33dc6-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="33dc6-156">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-156">**New behavior**</span></span>

<span data-ttu-id="33dc6-157">Począwszy od programu EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane oddzielnie z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="33dc6-158">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="33dc6-159">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i `ExecuteSqlInterpolatedAsync` można utworzyć zapytanie parametryczne, gdzie parametry są przekazywane jako część ciągu interpolowanego zapytania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="33dc6-160">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="33dc6-161">Należy pamiętać, że oba powyższe zapytania generuje taki sam sparametryzowanych SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="33dc6-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="33dc6-162">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-162">**Why**</span></span>

<span data-ttu-id="33dc6-163">Przeciążenia metody, takie jak to znacznie ułatwiają przypadkowo wywołania metody nieprzetworzonego ciągu, gdy celem było wywołanie metody ciągu interpolowanego i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="33dc6-164">Może to spowodować zapytania nie są parametryzowane, podczas gdy powinny być.</span><span class="sxs-lookup"><span data-stu-id="33dc6-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="33dc6-165">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-165">**Mitigations**</span></span>

<span data-ttu-id="33dc6-166">Przełącz, aby używać nowej nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="33dc6-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="33dc6-167">Metody FromSql można określić tylko na elementy główne zapytanie</span><span class="sxs-lookup"><span data-stu-id="33dc6-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="33dc6-168">Śledzenie problem #15704</span><span class="sxs-lookup"><span data-stu-id="33dc6-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="33dc6-169">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="33dc6-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33dc6-170">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-170">**Old behavior**</span></span>

<span data-ttu-id="33dc6-171">Przed programem EF Core 3.0 to `FromSql` metoda może być określona w dowolnym miejscu w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="33dc6-172">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-172">**New behavior**</span></span>

<span data-ttu-id="33dc6-173">Począwszy od programu EF Core 3.0 to nowy `FromSqlRaw` i `FromSqlInterpolated` metody (które Zastąp `FromSql`) można określić tylko na katalogi główne zapytanie, czyli bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="33dc6-174">Próba określenia ich w jakimkolwiek innym spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="33dc6-175">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-175">**Why**</span></span>

<span data-ttu-id="33dc6-176">Określanie `FromSql` dowolnym innym niż na `DbSet` nie dodano znaczenie lub wartość dodaną i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="33dc6-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="33dc6-177">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-177">**Mitigations**</span></span>

<span data-ttu-id="33dc6-178">`FromSql` wywołania zostanie przeniesiona za bezpośrednio na `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="33dc6-179">~~Wykonanie zapytania jest rejestrowane na poziomie debugowania~~ przywrócono</span><span class="sxs-lookup"><span data-stu-id="33dc6-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="33dc6-180">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="33dc6-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="33dc6-181">Ta zmiana zostanie wycofana w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="33dc6-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33dc6-182">Możemy przywrócić tę zmianę, ponieważ nowej konfiguracji w programie EF Core 3.0 umożliwia poziom rejestrowania dla dowolnego zdarzenia, należy określić przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="33dc6-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="33dc6-183">Na przykład, aby przełączyć rejestrowania programu SQL Server, aby `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="33dc6-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="33dc6-184">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="33dc6-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="33dc6-185">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="33dc6-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="33dc6-186">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33dc6-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33dc6-187">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-187">**Old behavior**</span></span>

<span data-ttu-id="33dc6-188">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="33dc6-189">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="33dc6-190">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-190">**New behavior**</span></span>

<span data-ttu-id="33dc6-191">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="33dc6-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="33dc6-192">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-192">**Why**</span></span>

<span data-ttu-id="33dc6-193">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="33dc6-194">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-194">**Mitigations**</span></span>

<span data-ttu-id="33dc6-195">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="33dc6-196">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="33dc6-196">This can be avoided by:</span></span>
* <span data-ttu-id="33dc6-197">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="33dc6-198">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="33dc6-199">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="33dc6-200">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="33dc6-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="33dc6-201">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="33dc6-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="33dc6-202">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="33dc6-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="33dc6-203">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-204">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-204">**Old behavior**</span></span>

<span data-ttu-id="33dc6-205">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="33dc6-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="33dc6-206">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-206">**New behavior**</span></span>

<span data-ttu-id="33dc6-207">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="33dc6-208">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="33dc6-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="33dc6-209">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="33dc6-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="33dc6-210">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-210">**Why**</span></span>

<span data-ttu-id="33dc6-211">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="33dc6-212">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-212">**Mitigations**</span></span>

<span data-ttu-id="33dc6-213">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="33dc6-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="33dc6-214">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="33dc6-215">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="33dc6-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="33dc6-216">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="33dc6-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="33dc6-217">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="33dc6-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="33dc6-218">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="33dc6-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="33dc6-219">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-220">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-220">**Old behavior**</span></span>

<span data-ttu-id="33dc6-221">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="33dc6-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="33dc6-222">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-222">**New behavior**</span></span>

<span data-ttu-id="33dc6-223">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="33dc6-224">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="33dc6-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="33dc6-225">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-225">**Why**</span></span>

<span data-ttu-id="33dc6-226">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="33dc6-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="33dc6-227">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-227">**Mitigations**</span></span>

<span data-ttu-id="33dc6-228">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="33dc6-229">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="33dc6-230">DeleteBehavior.Restrict ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="33dc6-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="33dc6-231">Śledzenie problem #12661</span><span class="sxs-lookup"><span data-stu-id="33dc6-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="33dc6-232">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 5.</span><span class="sxs-lookup"><span data-stu-id="33dc6-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="33dc6-233">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-233">**Old behavior**</span></span>

<span data-ttu-id="33dc6-234">Przed 3.0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych o `Restrict` semantykę, ale także zmienione wewnętrznego naprawy w taki sposób,-oczywiste.</span><span class="sxs-lookup"><span data-stu-id="33dc6-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="33dc6-235">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-235">**New behavior**</span></span>

<span data-ttu-id="33dc6-236">Począwszy od 3.0 `DeleteBehavior.Restrict` gwarantuje, że klucze obce są tworzone za pomocą `Restrict` semantyki — czyli nie kaskady; throw na naruszenie ograniczenia — bez również wpływ na wewnętrzny naprawy EF.</span><span class="sxs-lookup"><span data-stu-id="33dc6-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="33dc6-237">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-237">**Why**</span></span>

<span data-ttu-id="33dc6-238">Ta zmiana została wprowadzona w celu usprawnienie obsługi przy użyciu `DeleteBehavior` w sposób intuicyjny, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="33dc6-239">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-239">**Mitigations**</span></span>

<span data-ttu-id="33dc6-240">Poprzednie zachowanie można przywrócić za pomocą `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="33dc6-241">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="33dc6-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="33dc6-242">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="33dc6-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="33dc6-243">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-244">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-244">**Old behavior**</span></span>

<span data-ttu-id="33dc6-245">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="33dc6-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="33dc6-246">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="33dc6-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="33dc6-247">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-247">**New behavior**</span></span>

<span data-ttu-id="33dc6-248">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="33dc6-249">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="33dc6-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="33dc6-250">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-250">**Why**</span></span>

<span data-ttu-id="33dc6-251">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="33dc6-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="33dc6-252">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="33dc6-253">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="33dc6-254">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-254">**Mitigations**</span></span>

<span data-ttu-id="33dc6-255">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="33dc6-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="33dc6-256">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="33dc6-257">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="33dc6-258">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="33dc6-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="33dc6-259">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="33dc6-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="33dc6-260">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="33dc6-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="33dc6-261">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="33dc6-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="33dc6-262">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-263">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-263">**Old behavior**</span></span>

<span data-ttu-id="33dc6-264">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="33dc6-265">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-265">**New behavior**</span></span>

<span data-ttu-id="33dc6-266">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="33dc6-267">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="33dc6-268">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="33dc6-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="33dc6-269">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="33dc6-270">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-270">For example:</span></span>

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

<span data-ttu-id="33dc6-271">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="33dc6-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="33dc6-272">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-272">**Why**</span></span>

<span data-ttu-id="33dc6-273">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="33dc6-274">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="33dc6-275">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-275">**Mitigations**</span></span>

<span data-ttu-id="33dc6-276">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="33dc6-277">Udostępnianie w tabeli z jednostką jednostki zależne są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="33dc6-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="33dc6-278">Śledzenie problem #9005</span><span class="sxs-lookup"><span data-stu-id="33dc6-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="33dc6-279">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-280">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-280">**Old behavior**</span></span>

<span data-ttu-id="33dc6-281">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="33dc6-281">Consider the following model:</span></span>
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
<span data-ttu-id="33dc6-282">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, a następnie `OrderDetails` wystąpienia zawsze była wymagana podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="33dc6-283">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-283">**New behavior**</span></span>

<span data-ttu-id="33dc6-284">Począwszy od 3.0 programu EF Core umożliwia dodawanie `Order` bez `OrderDetails` i mapuje wszystkie `OrderDetails` właściwości, z wyjątkiem klucz podstawowy, aby kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="33dc6-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="33dc6-285">Podczas badania zestawów programu EF Core `OrderDetails` do `null` Jeśli dowolne wymagane właściwości nie ma wartość, lub jeśli go nie ma wymaganych właściwości oprócz klucza podstawowego, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="33dc6-286">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-286">**Mitigations**</span></span>

<span data-ttu-id="33dc6-287">Jeśli model zawiera tabelę udostępnianie zależne ze wszystkimi kolumnami opcjonalne, ale nawigacji wskazujących na niego nie powinny mieć `null` aplikacji powinno zostać zmodyfikowane w sposób obsługiwać przypadki, gdy jest nawigacji, a następnie `null`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="33dc6-288">Jeśli nie jest to możliwe wymagana właściwość powinna być dodana do typu jednostki lub powinien mieć co najmniej jedną właściwość innej niż`null` wartości przypisanej do niego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="33dc6-289">Wszystkie jednostki udostępnianie tabelę z kolumną tokenu współbieżności będą musieli mapować je do właściwości</span><span class="sxs-lookup"><span data-stu-id="33dc6-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="33dc6-290">Śledzenie problem #14154</span><span class="sxs-lookup"><span data-stu-id="33dc6-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="33dc6-291">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-292">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-292">**Old behavior**</span></span>

<span data-ttu-id="33dc6-293">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="33dc6-293">Consider the following model:</span></span>
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
<span data-ttu-id="33dc6-294">Przed programem EF Core 3.0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zmapowane do tej samej tabeli, po prostu zaktualizowanie `OrderDetails` nie może zaktualizować `Version` wartości na urządzeniu klienckim i kolejnej aktualizacji zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="33dc6-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="33dc6-295">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-295">**New behavior**</span></span>

<span data-ttu-id="33dc6-296">Począwszy od 3.0 programu EF Core propaguje nowy `Version` wartość `Order` Jeśli jest ona właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="33dc6-297">W przeciwnym razie jest zgłaszany wyjątek podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="33dc6-298">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-298">**Why**</span></span>

<span data-ttu-id="33dc6-299">Ta zmiana została wprowadzona w celu uniknięcia wartość tokenu współbieżności starych po zaktualizowaniu tylko jeden z jednostkami mapowany do tej samej tabeli.</span><span class="sxs-lookup"><span data-stu-id="33dc6-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="33dc6-300">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-300">**Mitigations**</span></span>

<span data-ttu-id="33dc6-301">Wszystkie jednostki udostępnianie tabeli muszą być uwzględniane właściwość, która jest mapowana na kolumnę tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="33dc6-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="33dc6-302">Może się zdarzyć, Utwórz jeden w stanie w tle:</span><span class="sxs-lookup"><span data-stu-id="33dc6-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="33dc6-303">Właściwości dziedziczone z niezamapowane typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="33dc6-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="33dc6-304">Śledzenie problem #13998</span><span class="sxs-lookup"><span data-stu-id="33dc6-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="33dc6-305">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-306">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-306">**Old behavior**</span></span>

<span data-ttu-id="33dc6-307">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="33dc6-307">Consider the following model:</span></span>
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

<span data-ttu-id="33dc6-308">Przed programem EF Core 3.0 to `ShippingAddress` właściwość zostanie on zamapowany do rozdzielania kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="33dc6-309">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-309">**New behavior**</span></span>

<span data-ttu-id="33dc6-310">Począwszy od 3.0 programu EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="33dc6-311">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-311">**Why**</span></span>

<span data-ttu-id="33dc6-312">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="33dc6-313">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-313">**Mitigations**</span></span>

<span data-ttu-id="33dc6-314">Właściwość może nadal jawnie mapowany do rozdzielania kolumn na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="33dc6-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="33dc6-315">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="33dc6-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="33dc6-316">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="33dc6-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="33dc6-317">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-318">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-318">**Old behavior**</span></span>

<span data-ttu-id="33dc6-319">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="33dc6-319">Consider the following model:</span></span>
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
<span data-ttu-id="33dc6-320">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="33dc6-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="33dc6-321">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="33dc6-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="33dc6-322">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-322">**New behavior**</span></span>

<span data-ttu-id="33dc6-323">Począwszy od 3.0 programu EF Core nie próbuje użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="33dc6-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="33dc6-324">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="33dc6-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="33dc6-325">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-325">For example:</span></span>

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

<span data-ttu-id="33dc6-326">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-326">**Why**</span></span>

<span data-ttu-id="33dc6-327">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="33dc6-328">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-328">**Mitigations**</span></span>

<span data-ttu-id="33dc6-329">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="33dc6-330">Połączenie z bazą danych jest teraz zamknięte niewykorzystane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="33dc6-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="33dc6-331">Śledzenie problem #14218</span><span class="sxs-lookup"><span data-stu-id="33dc6-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="33dc6-332">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-333">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-333">**Old behavior**</span></span>

<span data-ttu-id="33dc6-334">Przed programem EF Core 3.0, jeśli kontekst zostanie otwarte połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarty podczas bieżącej `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="33dc6-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="33dc6-335">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-335">**New behavior**</span></span>

<span data-ttu-id="33dc6-336">Począwszy od 3.0 programu EF Core zamyka połączenie zaraz po wykonaniu korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="33dc6-337">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-337">**Why**</span></span>

<span data-ttu-id="33dc6-338">Ta zmiana pozwala używać wielu kontekstach w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="33dc6-339">Nowe zachowanie dopasowuje również EF6.</span><span class="sxs-lookup"><span data-stu-id="33dc6-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="33dc6-340">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-340">**Mitigations**</span></span>

<span data-ttu-id="33dc6-341">Jeśli połączenie musi pozostać otwarte jawnym wywołaniem `OpenConnection()` zapewni, że programu EF Core nie go zamknąć przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="33dc6-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="33dc6-342">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="33dc6-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="33dc6-343">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="33dc6-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="33dc6-344">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-345">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-345">**Old behavior**</span></span>

<span data-ttu-id="33dc6-346">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="33dc6-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="33dc6-347">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-347">**New behavior**</span></span>

<span data-ttu-id="33dc6-348">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="33dc6-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="33dc6-349">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="33dc6-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="33dc6-350">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-350">**Why**</span></span>

<span data-ttu-id="33dc6-351">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="33dc6-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="33dc6-352">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-352">**Mitigations**</span></span>

<span data-ttu-id="33dc6-353">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="33dc6-354">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="33dc6-355">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="33dc6-355">Backing fields are used by default</span></span>

[<span data-ttu-id="33dc6-356">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="33dc6-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="33dc6-357">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33dc6-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33dc6-358">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-358">**Old behavior**</span></span>

<span data-ttu-id="33dc6-359">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="33dc6-360">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="33dc6-361">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-361">**New behavior**</span></span>

<span data-ttu-id="33dc6-362">Zaczynając od programu EF Core 3.0 to, jeśli znane jest pole zapasowe dla właściwości, następnie programu EF Core będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="33dc6-363">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="33dc6-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="33dc6-364">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-364">**Why**</span></span>

<span data-ttu-id="33dc6-365">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="33dc6-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="33dc6-366">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-366">**Mitigations**</span></span>

<span data-ttu-id="33dc6-367">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="33dc6-368">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="33dc6-369">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="33dc6-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="33dc6-370">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="33dc6-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="33dc6-371">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-372">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-372">**Old behavior**</span></span>

<span data-ttu-id="33dc6-373">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="33dc6-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="33dc6-374">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="33dc6-375">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-375">**New behavior**</span></span>

<span data-ttu-id="33dc6-376">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="33dc6-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="33dc6-377">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-377">**Why**</span></span>

<span data-ttu-id="33dc6-378">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="33dc6-379">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-379">**Mitigations**</span></span>

<span data-ttu-id="33dc6-380">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="33dc6-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="33dc6-381">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="33dc6-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="33dc6-382">Właściwość tylko do pola nazwy powinny pasować do nazwy pola</span><span class="sxs-lookup"><span data-stu-id="33dc6-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="33dc6-383">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-384">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-384">**Old behavior**</span></span>

<span data-ttu-id="33dc6-385">Przed programem EF Core 3.0 to właściwość może być określony przez wartość ciągu i jeśli żadnej właściwości o tej nazwie został znaleziony na typ CLR programu EF Core będzie spróbuj dopasować go do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="33dc6-386">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-386">**New behavior**</span></span>

<span data-ttu-id="33dc6-387">Począwszy od programu EF Core 3.0 to właściwość tylko do pola musi być zgodny nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="33dc6-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="33dc6-388">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-388">**Why**</span></span>

<span data-ttu-id="33dc6-389">Ta zmiana została wprowadzona w celu uniknięcia przy użyciu tego samego pola na dwie właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości tylko do pól takie same jak właściwości zamapowanych do właściwości aparatu CLR.</span><span class="sxs-lookup"><span data-stu-id="33dc6-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="33dc6-390">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-390">**Mitigations**</span></span>

<span data-ttu-id="33dc6-391">Właściwości tylko do pola musi mieć nazwę taka sama jak pola, które są mapowane.</span><span class="sxs-lookup"><span data-stu-id="33dc6-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="33dc6-392">W nowszej wersji zapoznawczej programu EF Core 3.0 planujemy ponownie włączyć konfigurowanie jawnie nazwę pola, która jest inna niż nazwa właściwości:</span><span class="sxs-lookup"><span data-stu-id="33dc6-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="33dc6-393">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="33dc6-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="33dc6-394">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="33dc6-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="33dc6-395">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-396">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-396">**Old behavior**</span></span>

<span data-ttu-id="33dc6-397">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="33dc6-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="33dc6-398">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-398">**New behavior**</span></span>

<span data-ttu-id="33dc6-399">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="33dc6-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="33dc6-400">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-400">**Why**</span></span>

<span data-ttu-id="33dc6-401">EF Core 3.0 nie wymaga, aby te usługi są w kontenerze DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="33dc6-402">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="33dc6-403">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-403">**Mitigations**</span></span>

<span data-ttu-id="33dc6-404">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="33dc6-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="33dc6-405">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="33dc6-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="33dc6-406">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="33dc6-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="33dc6-407">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-408">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-408">**Old behavior**</span></span>

<span data-ttu-id="33dc6-409">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="33dc6-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="33dc6-410">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="33dc6-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="33dc6-411">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-411">**New behavior**</span></span>

<span data-ttu-id="33dc6-412">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="33dc6-413">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="33dc6-414">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="33dc6-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="33dc6-415">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="33dc6-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="33dc6-416">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-416">**Why**</span></span>

<span data-ttu-id="33dc6-417">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="33dc6-418">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-418">**Mitigations**</span></span>

<span data-ttu-id="33dc6-419">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="33dc6-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="33dc6-420">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="33dc6-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="33dc6-421">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="33dc6-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="33dc6-422">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-423">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-423">**Old behavior**</span></span>

<span data-ttu-id="33dc6-424">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="33dc6-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="33dc6-425">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="33dc6-426">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-426">**New behavior**</span></span>

<span data-ttu-id="33dc6-427">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="33dc6-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="33dc6-428">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-428">**Why**</span></span>

<span data-ttu-id="33dc6-429">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="33dc6-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="33dc6-430">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-430">**Mitigations**</span></span>

<span data-ttu-id="33dc6-431">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="33dc6-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="33dc6-432">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="33dc6-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="33dc6-433">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="33dc6-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="33dc6-434">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="33dc6-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="33dc6-435">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="33dc6-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="33dc6-436">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-437">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-437">**Old behavior**</span></span>

<span data-ttu-id="33dc6-438">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="33dc6-439">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-439">**New behavior**</span></span>

<span data-ttu-id="33dc6-440">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="33dc6-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="33dc6-441">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-441">**Why**</span></span>

<span data-ttu-id="33dc6-442">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="33dc6-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="33dc6-443">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-443">**Mitigations**</span></span>

<span data-ttu-id="33dc6-444">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="33dc6-445">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="33dc6-445">This isn't common.</span></span>
<span data-ttu-id="33dc6-446">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="33dc6-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="33dc6-447">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="33dc6-448">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="33dc6-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="33dc6-449">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="33dc6-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="33dc6-450">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-451">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-451">**Old behavior**</span></span>

<span data-ttu-id="33dc6-452">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="33dc6-453">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="33dc6-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="33dc6-454">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="33dc6-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="33dc6-455">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-455">**New behavior**</span></span>

<span data-ttu-id="33dc6-456">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="33dc6-457">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="33dc6-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="33dc6-458">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="33dc6-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="33dc6-459">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="33dc6-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="33dc6-460">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-460">**Why**</span></span>

<span data-ttu-id="33dc6-461">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="33dc6-462">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-462">**Mitigations**</span></span>

<span data-ttu-id="33dc6-463">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="33dc6-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="33dc6-464">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="33dc6-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="33dc6-465">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="33dc6-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="33dc6-466">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-467">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-467">**Old behavior**</span></span>

<span data-ttu-id="33dc6-468">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="33dc6-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="33dc6-469">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-469">**New behavior**</span></span>

<span data-ttu-id="33dc6-470">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="33dc6-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="33dc6-471">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-471">**Why**</span></span>

<span data-ttu-id="33dc6-472">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="33dc6-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="33dc6-473">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-473">**Mitigations**</span></span>

<span data-ttu-id="33dc6-474">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="33dc6-475">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="33dc6-476">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="33dc6-477">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="33dc6-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="33dc6-478">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="33dc6-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="33dc6-479">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-480">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-480">**Old behavior**</span></span>

<span data-ttu-id="33dc6-481">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została interpretowany w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="33dc6-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="33dc6-482">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="33dc6-483">Kod wygląda na to jest odnoszących się `Samurai` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="33dc6-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="33dc6-484">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="33dc6-485">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-485">**New behavior**</span></span>

<span data-ttu-id="33dc6-486">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="33dc6-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="33dc6-487">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-487">**Why**</span></span>

<span data-ttu-id="33dc6-488">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="33dc6-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="33dc6-489">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-489">**Mitigations**</span></span>

<span data-ttu-id="33dc6-490">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="33dc6-491">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-491">This is not common.</span></span>
<span data-ttu-id="33dc6-492">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="33dc6-493">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="33dc6-494">Typ zwracany dla kilku metod asynchronicznych została zmieniona z zadaniem do ValueTask</span><span class="sxs-lookup"><span data-stu-id="33dc6-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="33dc6-495">Śledzenie problem #15184</span><span class="sxs-lookup"><span data-stu-id="33dc6-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="33dc6-496">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-497">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-497">**Old behavior**</span></span>

<span data-ttu-id="33dc6-498">Następujące metody async wcześniej zwrócony `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="33dc6-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="33dc6-499">`ValueGenerator.NextValueAsync()` (i wyprowadzanie klas)</span><span class="sxs-lookup"><span data-stu-id="33dc6-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="33dc6-500">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-500">**New behavior**</span></span>

<span data-ttu-id="33dc6-501">Wymienione wyżej metod teraz zwracają `ValueTask<T>` w taki sam `T` tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="33dc6-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="33dc6-502">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-502">**Why**</span></span>

<span data-ttu-id="33dc6-503">Ta zmiana zmniejsza liczbę alokacji sterty naliczany podczas wywoływania metody te poprawę ogólnej wydajności.</span><span class="sxs-lookup"><span data-stu-id="33dc6-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="33dc6-504">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-504">**Mitigations**</span></span>

<span data-ttu-id="33dc6-505">Tylko po prostu oczekujące na powyższym interfejsów API muszą zostać zrekompilowane — aplikacje bez zmian źródła są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="33dc6-506">Bardziej złożonych zastosowaniach (np. przekazywanie zwracanego `Task` do `Task.WhenAny()`) zwykle wymagają, aby zwrócony `ValueTask<T>` można przekonwertować na `Task<T>` przez wywołanie metody `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="33dc6-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="33dc6-507">Należy pamiętać, że to neguje zmniejszenia alokacji, które ta zmiana powoduje.</span><span class="sxs-lookup"><span data-stu-id="33dc6-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="33dc6-508">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="33dc6-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="33dc6-509">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="33dc6-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="33dc6-510">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33dc6-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33dc6-511">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-511">**Old behavior**</span></span>

<span data-ttu-id="33dc6-512">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="33dc6-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="33dc6-513">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-513">**New behavior**</span></span>

<span data-ttu-id="33dc6-514">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="33dc6-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="33dc6-515">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-515">**Why**</span></span>

<span data-ttu-id="33dc6-516">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="33dc6-517">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-517">**Mitigations**</span></span>

<span data-ttu-id="33dc6-518">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="33dc6-519">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="33dc6-520">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="33dc6-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="33dc6-521">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="33dc6-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="33dc6-522">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-523">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-523">**Old behavior**</span></span>

<span data-ttu-id="33dc6-524">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="33dc6-525">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-525">**New behavior**</span></span>

<span data-ttu-id="33dc6-526">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="33dc6-527">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-527">**Why**</span></span>

<span data-ttu-id="33dc6-528">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="33dc6-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="33dc6-529">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="33dc6-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="33dc6-530">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-530">**Mitigations**</span></span>

<span data-ttu-id="33dc6-531">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="33dc6-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="33dc6-532">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="33dc6-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="33dc6-533">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="33dc6-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="33dc6-534">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-535">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-535">**Old behavior**</span></span>

<span data-ttu-id="33dc6-536">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="33dc6-537">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-537">**New behavior**</span></span>

<span data-ttu-id="33dc6-538">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="33dc6-539">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="33dc6-540">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-540">**Why**</span></span>

<span data-ttu-id="33dc6-541">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Include` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="33dc6-542">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-542">**Mitigations**</span></span>

<span data-ttu-id="33dc6-543">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="33dc6-544">Zmiany metadanych interfejsu API</span><span class="sxs-lookup"><span data-stu-id="33dc6-544">Metadata API changes</span></span>

[<span data-ttu-id="33dc6-545">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="33dc6-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="33dc6-546">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-547">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-547">**New behavior**</span></span>

<span data-ttu-id="33dc6-548">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="33dc6-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="33dc6-549">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-549">**Why**</span></span>

<span data-ttu-id="33dc6-550">Ta zmiana ułatwia implementację interfejsów wyżej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="33dc6-551">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-551">**Mitigations**</span></span>

<span data-ttu-id="33dc6-552">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="33dc6-553">Zmiany interfejsu API metadane właściwe dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="33dc6-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="33dc6-554">Śledzenie problem #214</span><span class="sxs-lookup"><span data-stu-id="33dc6-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="33dc6-555">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="33dc6-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33dc6-556">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-556">**New behavior**</span></span>

<span data-ttu-id="33dc6-557">Metody rozszerzające właściwe dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="33dc6-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="33dc6-558">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-558">**Why**</span></span>

<span data-ttu-id="33dc6-559">Ta zmiana ułatwia implementację metody rozszerzenia wyżej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="33dc6-560">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-560">**Mitigations**</span></span>

<span data-ttu-id="33dc6-561">Korzystając z nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="33dc6-562">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="33dc6-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="33dc6-563">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="33dc6-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="33dc6-564">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="33dc6-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33dc6-565">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-565">**Old behavior**</span></span>

<span data-ttu-id="33dc6-566">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="33dc6-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="33dc6-567">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-567">**New behavior**</span></span>

<span data-ttu-id="33dc6-568">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="33dc6-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="33dc6-569">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-569">**Why**</span></span>

<span data-ttu-id="33dc6-570">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="33dc6-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="33dc6-571">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-571">**Mitigations**</span></span>

<span data-ttu-id="33dc6-572">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="33dc6-573">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="33dc6-574">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="33dc6-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="33dc6-575">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-575">**Old behavior**</span></span>

<span data-ttu-id="33dc6-576">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="33dc6-577">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-577">**New behavior**</span></span>

<span data-ttu-id="33dc6-578">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="33dc6-579">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-579">**Why**</span></span>

<span data-ttu-id="33dc6-580">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="33dc6-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="33dc6-581">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-581">**Mitigations**</span></span>

<span data-ttu-id="33dc6-582">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="33dc6-583">Wartości identyfikatora GUID są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="33dc6-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="33dc6-584">Śledzenie problem #15078</span><span class="sxs-lookup"><span data-stu-id="33dc6-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="33dc6-585">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-586">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-586">**Old behavior**</span></span>

<span data-ttu-id="33dc6-587">Identyfikator GUID wartości zostały wcześniej sored jako wartości obiektu BLOB dla bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="33dc6-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="33dc6-588">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-588">**New behavior**</span></span>

<span data-ttu-id="33dc6-589">Wartości identyfikatora GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="33dc6-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="33dc6-590">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-590">**Why**</span></span>

<span data-ttu-id="33dc6-591">Format binarny identyfikatorów GUID nie jest standardowym.</span><span class="sxs-lookup"><span data-stu-id="33dc6-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="33dc6-592">Przechowywanie wartości jako tekst sprawia, że baza danych większą zgodność z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="33dc6-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="33dc6-593">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-593">**Mitigations**</span></span>

<span data-ttu-id="33dc6-594">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="33dc6-595">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="33dc6-596">Microsoft.Data.Sqlite pozostaje możliwość odczytywania wartości identyfikatora Guid z kolumny obiektów BLOB i tekst; Jednakże ponieważ zmieniono domyślny format parametrów i stałych prawdopodobnie należy podjąć działania w przypadku większości scenariuszy obejmujących identyfikatorów GUID.</span><span class="sxs-lookup"><span data-stu-id="33dc6-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="33dc6-597">Wartości char są obecnie przechowywane jako tekst w SQLite</span><span class="sxs-lookup"><span data-stu-id="33dc6-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="33dc6-598">Śledzenie problem #15020</span><span class="sxs-lookup"><span data-stu-id="33dc6-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="33dc6-599">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-600">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-600">**Old behavior**</span></span>

<span data-ttu-id="33dc6-601">Wartości char zostały wcześniej sored jako wartości całkowite na bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="33dc6-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="33dc6-602">Na przykład wartości znakowej na wartość z *A* została zapisana jako wartość całkowitą 65.</span><span class="sxs-lookup"><span data-stu-id="33dc6-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="33dc6-603">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-603">**New behavior**</span></span>

<span data-ttu-id="33dc6-604">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="33dc6-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="33dc6-605">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-605">**Why**</span></span>

<span data-ttu-id="33dc6-606">Przechowywanie wartości jako tekst jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="33dc6-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="33dc6-607">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-607">**Mitigations**</span></span>

<span data-ttu-id="33dc6-608">Aby Migrowanie istniejących baz danych do nowego formatu, wykonywanie SQL, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="33dc6-609">W programie EF Core można także kontynuować przy użyciu poprzednie zachowanie przez skonfigurowanie konwertera wartości tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="33dc6-610">Microsoft.Data.Sqlite pozostaje też obecna mogą odczytywać wartości znakowych z kolumn tekst i liczby całkowitej, więc w niektórych scenariuszach może nie wymagają żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="33dc6-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="33dc6-611">Migracja identyfikatorów są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="33dc6-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="33dc6-612">Śledzenie problem #12978</span><span class="sxs-lookup"><span data-stu-id="33dc6-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="33dc6-613">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-614">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-614">**Old behavior**</span></span>

<span data-ttu-id="33dc6-615">Migracja identyfikatorów przypadkowo zostały wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="33dc6-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="33dc6-616">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-616">**New behavior**</span></span>

<span data-ttu-id="33dc6-617">Migracja identyfikatorów są teraz zawsze generowane przy użyciu niezmiennej kultury kalendarza (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="33dc6-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="33dc6-618">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-618">**Why**</span></span>

<span data-ttu-id="33dc6-619">Kolejność migracji jest ważne w przypadku aktualizowania bazy danych lub Rozwiązywanie konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="33dc6-620">Przy użyciu niezmiennej kalendarza pozwala uniknąć porządkowanie problemów mogących powodować od członków zespołu o kalendarzach różnych systemów.</span><span class="sxs-lookup"><span data-stu-id="33dc6-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="33dc6-621">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-621">**Mitigations**</span></span>

<span data-ttu-id="33dc6-622">Ta zmiana dotyczy wszystkich osób korzystających z innych kalendarz gregoriański — gdzie rok jest większa niż kalendarz gregoriański (np. tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="33dc6-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="33dc6-623">Migracji istniejących identyfikatorów będą musiały zostać zaktualizowane, tak, aby nowe migracje są uporządkowane od istniejących migracji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="33dc6-624">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="33dc6-625">Tabela migracji historii musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="33dc6-626">UseRowNumberForPaging został usunięty.</span><span class="sxs-lookup"><span data-stu-id="33dc6-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="33dc6-627">Śledzenie problem #16400</span><span class="sxs-lookup"><span data-stu-id="33dc6-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="33dc6-628">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 6.</span><span class="sxs-lookup"><span data-stu-id="33dc6-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33dc6-629">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-629">**Old behavior**</span></span>

<span data-ttu-id="33dc6-630">Przed programem EF Core 3.0 to `UseRowNumberForPaging` może służyć do generowania SQL dla stronicowania, który jest zgodny z programu SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="33dc6-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="33dc6-631">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-631">**New behavior**</span></span>

<span data-ttu-id="33dc6-632">Począwszy od programu EF Core 3.0 EF będą generowane tylko SQL stronicowanie, która jest zgodna tylko z nowszej wersji programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="33dc6-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="33dc6-633">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-633">**Why**</span></span>

<span data-ttu-id="33dc6-634">Wprowadzamy tej zmiany, ponieważ [programu SQL Server 2008 nie jest już obsługiwany produkt](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tę funkcję, aby pracować z zapytania zmian w programie EF Core 3.0 jest dużo pracy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="33dc6-635">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-635">**Mitigations**</span></span>

<span data-ttu-id="33dc6-636">Aktualizacja do nowszej wersji programu SQL Server lub za pomocą wyższym poziomie zgodności firma Microsoft zaleca, aby wygenerowanego SQL jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="33dc6-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="33dc6-637">Następnie po uwzględnieniu, jeśli nie można w tym celu należy [komentarza dotyczącego problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="33dc6-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="33dc6-638">Firma Microsoft może ponownie tej decyzji na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="33dc6-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="33dc6-639">Metadane informacji rozszerzenia została usunięta z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="33dc6-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="33dc6-640">Śledzenie problem #16119</span><span class="sxs-lookup"><span data-stu-id="33dc6-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="33dc6-641">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="33dc6-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33dc6-642">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-642">**Old behavior**</span></span>

<span data-ttu-id="33dc6-643">`IDbContextOptionsExtension` zawiera metody dostarczania metadanych dotyczących rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="33dc6-644">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-644">**New behavior**</span></span>

<span data-ttu-id="33dc6-645">Te metody zostały przeniesione na nowy `DbContextOptionsExtensionInfo` abstrakcyjna klasa bazowa, która jest zwracana z nową `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="33dc6-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="33dc6-646">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-646">**Why**</span></span>

<span data-ttu-id="33dc6-647">W wersji 2.0, 3.0 Musieliśmy dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="33dc6-648">Przerwanie ich do nowego abstrakcyjna klasa bazowa ułatwi zapewnienie te rodzaje zmian bez przerywania istniejące rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="33dc6-649">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-649">**Mitigations**</span></span>

<span data-ttu-id="33dc6-650">Aktualizowanie rozszerzeń do wykonania nowego wzorca.</span><span class="sxs-lookup"><span data-stu-id="33dc6-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="33dc6-651">Przykłady znajdują się w wielu implementacjach `IDbContextOptionsExtension` kod źródłowy dla różnych rodzajów rozszerzeń w programie EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="33dc6-652">LogQueryPossibleExceptionWithAggregateOperator została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="33dc6-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="33dc6-653">Śledzenie problem #10985</span><span class="sxs-lookup"><span data-stu-id="33dc6-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="33dc6-654">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-655">**Zmiany**</span><span class="sxs-lookup"><span data-stu-id="33dc6-655">**Change**</span></span>

<span data-ttu-id="33dc6-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="33dc6-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="33dc6-657">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-657">**Why**</span></span>

<span data-ttu-id="33dc6-658">Wyrównuje nazewnictwa to zdarzenie ostrzegawcze z innymi zdarzeniami ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="33dc6-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="33dc6-659">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-659">**Mitigations**</span></span>

<span data-ttu-id="33dc6-660">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-660">Use the new name.</span></span> <span data-ttu-id="33dc6-661">(Zwróć uwagę, czy nie zmieniono numer identyfikacyjny zdarzeń).</span><span class="sxs-lookup"><span data-stu-id="33dc6-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="33dc6-662">Wyjaśnienie interfejsu API dla nazw ograniczenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="33dc6-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="33dc6-663">Śledzenie problem #10730</span><span class="sxs-lookup"><span data-stu-id="33dc6-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="33dc6-664">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-665">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-665">**Old behavior**</span></span>

<span data-ttu-id="33dc6-666">Przed programem EF Core 3.0 to ograniczenie klucza obcego nazwy były nazywane po prostu "name".</span><span class="sxs-lookup"><span data-stu-id="33dc6-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="33dc6-667">Przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="33dc6-668">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-668">**New behavior**</span></span>

<span data-ttu-id="33dc6-669">Począwszy od programu EF Core 3.0, ograniczenie klucza obcego nazwy są teraz określane jako "Nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="33dc6-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="33dc6-670">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33dc6-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="33dc6-671">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-671">**Why**</span></span>

<span data-ttu-id="33dc6-672">Ta zmiana oferuje spójność nazw w tym obszarze, a także wyjaśnia, że jest to nazwa nazwy klucza obcego ograniczenia, a nie kolumny lub właściwość klucza obcego zdefiniowany na.</span><span class="sxs-lookup"><span data-stu-id="33dc6-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="33dc6-673">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-673">**Mitigations**</span></span>

<span data-ttu-id="33dc6-674">Użycie nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="33dc6-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync wprowadzono publiczne</span><span class="sxs-lookup"><span data-stu-id="33dc6-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="33dc6-676">Śledzenie problem #15997</span><span class="sxs-lookup"><span data-stu-id="33dc6-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="33dc6-677">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="33dc6-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33dc6-678">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-678">**Old behavior**</span></span>

<span data-ttu-id="33dc6-679">Przed programem EF Core 3.0 to chronione zostały tych metod.</span><span class="sxs-lookup"><span data-stu-id="33dc6-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="33dc6-680">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-680">**New behavior**</span></span>

<span data-ttu-id="33dc6-681">Począwszy od programu EF Core 3.0 te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="33dc6-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="33dc6-682">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-682">**Why**</span></span>

<span data-ttu-id="33dc6-683">Te metody są używane przez EF do ustalenia, czy bazy danych jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="33dc6-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="33dc6-684">Może to być również przydatne z poza EF podczas ustalania, czy należy zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="33dc6-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="33dc6-685">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-685">**Mitigations**</span></span>

<span data-ttu-id="33dc6-686">Zmień dostępność elementu zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="33dc6-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="33dc6-687">Microsoft.EntityFrameworkCore.Design jest teraz pakiet DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="33dc6-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="33dc6-688">Śledzenie problem #11506</span><span class="sxs-lookup"><span data-stu-id="33dc6-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="33dc6-689">Ta zmiana jest wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="33dc6-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33dc6-690">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-690">**Old behavior**</span></span>

<span data-ttu-id="33dc6-691">Przed programem EF Core 3.0 to Microsoft.EntityFrameworkCore.Design był regularne pakietu NuGet zestawu, którego można odwoływać się projekty, które zależą od jej.</span><span class="sxs-lookup"><span data-stu-id="33dc6-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="33dc6-692">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-692">**New behavior**</span></span>

<span data-ttu-id="33dc6-693">Począwszy od programu EF Core 3.0 to jest pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="33dc6-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="33dc6-694">Co oznacza, że zależność nie została przechodnio przepływu do innych projektów i nie będzie mógł, domyślnie odwoływać się do własnego zestawu.</span><span class="sxs-lookup"><span data-stu-id="33dc6-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="33dc6-695">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-695">**Why**</span></span>

<span data-ttu-id="33dc6-696">Ten pakiet jest przeznaczona tylko do użycia w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="33dc6-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="33dc6-697">Wdrożone aplikacje nie powinny odwoływać się do niego.</span><span class="sxs-lookup"><span data-stu-id="33dc6-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="33dc6-698">Tworzenie pakietu DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="33dc6-699">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-699">**Mitigations**</span></span>

<span data-ttu-id="33dc6-700">Jeśli musisz odwoływać się do tego pakietu, aby zastąpić zachowanie czasu projektowania programu EF Core, możesz zaktualizować aktualizacji metadanych elementu PackageReference w projekcie.</span><span class="sxs-lookup"><span data-stu-id="33dc6-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="33dc6-701">Jeśli pakiet jest ono przywoływane przechodni, za pośrednictwem Microsoft.EntityFrameworkCore.Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadanych.</span><span class="sxs-lookup"><span data-stu-id="33dc6-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="33dc6-702">Zaktualizowany do wersji 2.0.0 SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="33dc6-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="33dc6-703">Śledzenie problem #14824</span><span class="sxs-lookup"><span data-stu-id="33dc6-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="33dc6-704">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="33dc6-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33dc6-705">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-705">**Old behavior**</span></span>

<span data-ttu-id="33dc6-706">Microsoft.EntityFrameworkCore.Sqlite wcześniej zależała od wersji 1.1.12 SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="33dc6-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="33dc6-707">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-707">**New behavior**</span></span>

<span data-ttu-id="33dc6-708">Aktualizujemy już naszego pakietu są zależne od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="33dc6-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="33dc6-709">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-709">**Why**</span></span>

<span data-ttu-id="33dc6-710">W wersji 2.0.0 SQLitePCL.raw jest przeznaczony dla .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="33dc6-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="33dc6-711">Wskazanych .NET 1.1 standardowe, wymagająca dużych zamknięcia przechodnie pakietów do pracy.</span><span class="sxs-lookup"><span data-stu-id="33dc6-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="33dc6-712">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-712">**Mitigations**</span></span>

<span data-ttu-id="33dc6-713">Wersja SQLitePCL.raw 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="33dc6-714">Zobacz [informacje o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="33dc6-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="33dc6-715">Zaktualizowany do wersji 2.0.0 NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="33dc6-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="33dc6-716">Śledzenie problem #14825</span><span class="sxs-lookup"><span data-stu-id="33dc6-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="33dc6-717">Ta zmiana jest wprowadzona w programu EF Core 3.0-preview 7.</span><span class="sxs-lookup"><span data-stu-id="33dc6-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33dc6-718">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-718">**Old behavior**</span></span>

<span data-ttu-id="33dc6-719">Przestrzenne pakietów wcześniej zależała od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="33dc6-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="33dc6-720">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="33dc6-720">**New behavior**</span></span>

<span data-ttu-id="33dc6-721">Aktualizujemy już naszego pakietu są zależne od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="33dc6-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="33dc6-722">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="33dc6-722">**Why**</span></span>

<span data-ttu-id="33dc6-723">W wersji 2.0.0 NetTopologySuite ma na celu kilka użyteczność rozwiązywania problemów napotykanych przez użytkowników programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="33dc6-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="33dc6-724">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="33dc6-724">**Mitigations**</span></span>

<span data-ttu-id="33dc6-725">NetTopologySuite wersji 2.0.0 obejmuje pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="33dc6-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="33dc6-726">Zobacz [informacje o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="33dc6-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
