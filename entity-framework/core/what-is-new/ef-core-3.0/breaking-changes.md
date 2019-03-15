---
title: Powodująca niezgodność zmiany w EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 748db8a71a04a2d696ef21a03319906b9fc776be
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829229"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="8481f-102">Istotne zmiany zawarte w programie EF Core 3.0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="8481f-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8481f-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie i czasu, mimo że firma Microsoft podejmie próbę tej strony na bieżąco, jego mogą nie odzwierciedlać najnowszych plany na wszystkich.</span><span class="sxs-lookup"><span data-stu-id="8481f-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="8481f-104">Następujące zmiany interfejsu API i zachowanie mogą potencjalnie przerwanie aplikacje opracowane dla platformy EF Core 2.2.x po uaktualnieniu ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="8481f-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="8481f-105">Zmiany, oczekujemy, że mają wpływ tylko na dostawcy baz danych, które są udokumentowane w obszarze [zmian dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="8481f-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="8481f-106">Podział w nowych funkcji wprowadzonych w jednym 3.0 w wersji zapoznawczej do innej wersji 3.0 w wersji zapoznawczej nie są opisane tutaj.</span><span class="sxs-lookup"><span data-stu-id="8481f-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="8481f-107">Zapytania LINQ nie są obliczane na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="8481f-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="8481f-108">[Śledzenie 14935 # problem](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="8481f-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="8481f-109">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-110">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-110">**Old behavior**</span></span>

<span data-ttu-id="8481f-111">Przed 3.0 po programu EF Core nie można przekonwertować na wyrażenie, które było częścią zapytania SQL lub parametru, jego automatycznie oceniane wyrażenie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8481f-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="8481f-112">Domyślnie klient obliczanie wyrażeń potencjalnie kosztownym wyzwalane tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="8481f-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="8481f-113">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-113">**New behavior**</span></span>

<span data-ttu-id="8481f-114">Począwszy od 3.0 programu EF Core zezwala tylko wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołań w zapytaniu) ma zostać obliczone na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8481f-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="8481f-115">Jeśli nie można przekonwertować wyrażenia w dowolnej części zapytania SQL lub parametr, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8481f-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="8481f-116">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-116">**Why**</span></span>

<span data-ttu-id="8481f-117">Automatyczne klienta zapytań pozwala ona wiele zapytań do wykonania, nawet wtedy, gdy nie można przetłumaczyć ważne elementy.</span><span class="sxs-lookup"><span data-stu-id="8481f-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="8481f-118">To zachowanie może powodować nieoczekiwane i potencjalnie szkodliwych zachowanie, które mogą stać się widoczne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8481f-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="8481f-119">Na przykład warunku w `Where()` wywołanie, którego nie można przetłumaczyć może spowodować, że wszystkie wiersze z tabeli z serwera bazy danych i filtrów, które mają być stosowane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8481f-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="8481f-120">Ta sytuacja może łatwo przejść niewykryte, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale twardych trafień, gdy aplikacja przejdzie do środowiska produkcyjnego, gdzie tabeli może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="8481f-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="8481f-121">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="8481f-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="8481f-122">Poza tym oceny klienta może prowadzić do problemów, w których usprawnienie translacji zapytania dla określonych wyrażeń spowodowane niezamierzone przełomowych zmian między wersjami.</span><span class="sxs-lookup"><span data-stu-id="8481f-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="8481f-123">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-123">**Mitigations**</span></span>

<span data-ttu-id="8481f-124">Jeśli zapytanie nie może być w pełni przetłumaczona, a następnie zastąp kwerendy w postaci, które mogą być tłumaczone lub użyj `AsEnumerable()`, `ToList()`, lub podobne do jawnie możesz przenosić dane do klienta której następnie można dalsze przetworzona za pomocą LINQ do obiektów.</span><span class="sxs-lookup"><span data-stu-id="8481f-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="8481f-125">Entity Framework Core nie jest już częścią udostępnionej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8481f-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="8481f-126">Anonse problem śledzenia #325</span><span class="sxs-lookup"><span data-stu-id="8481f-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="8481f-127">Ta zmiana została wprowadzona w programie ASP.NET Core 3.0 w wersji zapoznawczej 1.</span><span class="sxs-lookup"><span data-stu-id="8481f-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="8481f-128">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-128">**Old behavior**</span></span>

<span data-ttu-id="8481f-129">Przed platformy ASP.NET Core 3.0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, może on zawierać programu EF Core i niektóre z programem EF Core dostawcy danych, takich jak dostawcy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8481f-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="8481f-130">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-130">**New behavior**</span></span>

<span data-ttu-id="8481f-131">Począwszy od 3.0, udostępnionej platformy ASP.NET Core nie zawiera programu EF Core lub żadnego dostawcy danych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="8481f-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="8481f-132">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-132">**Why**</span></span>

<span data-ttu-id="8481f-133">Przed wprowadzeniem tej zmiany pobierania programu EF Core wymaga różnych kroków w zależności od tego, czy aplikacja docelowej platformy ASP.NET Core i SQL Server, lub nie.</span><span class="sxs-lookup"><span data-stu-id="8481f-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="8481f-134">Ponadto uaktualnienie platformy ASP.NET Core wymuszone uaktualnienie programu EF Core i dostawcy programu SQL Server, który nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="8481f-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="8481f-135">Dzięki tej zmianie środowiska pobierania programu EF Core jest taka sama we wszystkich dostawców, obsługiwane implementacje platformy .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="8481f-136">Deweloperzy również teraz można kontrolować, dokładnie tak, po uaktualnieniu programu EF Core i programem EF Core dostawcy danych.</span><span class="sxs-lookup"><span data-stu-id="8481f-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="8481f-137">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-137">**Mitigations**</span></span>

<span data-ttu-id="8481f-138">Aby użyć programu EF Core w aplikacji ASP.NET Core 3.0 lub dowolnej innej obsługiwanej aplikacji, należy jawnie dodać odwołania do pakietu do dostawcy bazy danych programu EF Core, używanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="8481f-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="8481f-139">Wykonanie zapytania jest rejestrowane na poziomie debugowania</span><span class="sxs-lookup"><span data-stu-id="8481f-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="8481f-140">Śledzenie problem #14523</span><span class="sxs-lookup"><span data-stu-id="8481f-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="8481f-141">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-142">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-142">**Old behavior**</span></span>

<span data-ttu-id="8481f-143">Przed programem EF Core 3.0, wykonywanie zapytań i innych poleceniach, zarejestrowano w `Info` poziom.</span><span class="sxs-lookup"><span data-stu-id="8481f-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="8481f-144">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-144">**New behavior**</span></span>

<span data-ttu-id="8481f-145">Począwszy od programu EF Core 3.0 rejestrowanie wykonywania polecenia/SQL jest w `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="8481f-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="8481f-146">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-146">**Why**</span></span>

<span data-ttu-id="8481f-147">Ta zmiana została wprowadzona redukcji szumu w `Info` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="8481f-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="8481f-148">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-148">**Mitigations**</span></span>

<span data-ttu-id="8481f-149">To zdarzenie logowania jest definiowany przez `RelationalEventId.CommandExecuting` zdarzenia o identyfikatorze 20100.</span><span class="sxs-lookup"><span data-stu-id="8481f-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="8481f-150">Do logowania SQL w `Info` poziomu ponownie, jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="8481f-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="8481f-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="8481f-152">Wartości klucza tymczasowego nie są już ustawione na wystąpień jednostek</span><span class="sxs-lookup"><span data-stu-id="8481f-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="8481f-153">Śledzenie problem #12378</span><span class="sxs-lookup"><span data-stu-id="8481f-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="8481f-154">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8481f-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8481f-155">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-155">**Old behavior**</span></span>

<span data-ttu-id="8481f-156">Przed programem EF Core 3.0 to tymczasowe wartości zostały przypisane do wszystkich właściwości klucza, które później będzie miała wartość rzeczywistego generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8481f-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="8481f-157">Zazwyczaj te wartości tymczasowej były dużych liczb ujemnych.</span><span class="sxs-lookup"><span data-stu-id="8481f-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="8481f-158">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-158">**New behavior**</span></span>

<span data-ttu-id="8481f-159">Począwszy od 3.0 tymczasowej wartości klucza są przechowywane jako część informacji śledzenia jednostki programu EF Core i pozostawia właściwość klucza, sama bez zmian.</span><span class="sxs-lookup"><span data-stu-id="8481f-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="8481f-160">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-160">**Why**</span></span>

<span data-ttu-id="8481f-161">Ta zmiana została wprowadzona, aby zapobiec wartości klucza tymczasowego błędnie trwałe, gdy jednostka, która ma zostać wcześniej śledzone przez niektóre `DbContext` wystąpienia zostanie przeniesiony do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8481f-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="8481f-162">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-162">**Mitigations**</span></span>

<span data-ttu-id="8481f-163">Aplikacje, które przypisać wartości klucza podstawowego na klucze obce do formularza skojarzenia między jednostkami może zależeć od starego zachowania, jeśli są generowane przez magazyn kluczy podstawowych, a należą do jednostki w `Added` stanu.</span><span class="sxs-lookup"><span data-stu-id="8481f-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="8481f-164">Można tego uniknąć, za pomocą:</span><span class="sxs-lookup"><span data-stu-id="8481f-164">This can be avoided by:</span></span>
* <span data-ttu-id="8481f-165">Nie używa generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="8481f-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="8481f-166">Ustawianie właściwości nawigacji do utworzenia relacji zamiast ustawiać wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="8481f-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="8481f-167">Uzyskać rzeczywiste wartości klucza tymczasowego z jednostki informacje ze śledzenia.</span><span class="sxs-lookup"><span data-stu-id="8481f-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="8481f-168">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartości tymczasowej, nawet jeśli `blog.Id` sam nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="8481f-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="8481f-169">Metody DetectChanges honoruje generowane przez Magazyn wartości klucza</span><span class="sxs-lookup"><span data-stu-id="8481f-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="8481f-170">Śledzenie problem #14616</span><span class="sxs-lookup"><span data-stu-id="8481f-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="8481f-171">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-172">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-172">**Old behavior**</span></span>

<span data-ttu-id="8481f-173">Przed programem EF Core 3.0 to jednostki nieśledzone znalezione przez `DetectChanges` będą śledzone w `Added` stanu i wstawiony jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8481f-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="8481f-174">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-174">**New behavior**</span></span>

<span data-ttu-id="8481f-175">Począwszy od programu EF Core 3.0, jeśli korzysta z jednostki generowane wartości klucza i niektóre wartości klucza jest ustawiona, a następnie jednostki będą śledzone w `Modified` stanu.</span><span class="sxs-lookup"><span data-stu-id="8481f-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="8481f-176">Oznacza to, że przyjęto, że wiersz dla tej jednostki istnieje i będzie aktualizowane, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8481f-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="8481f-177">Jeśli nie ustawiono wartości klucza, lub jeśli typ jednostki nie jest używana wygenerowanych kluczy, a następnie nową jednostkę, będą nadal być śledzone jako `Added` tak jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="8481f-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="8481f-178">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-178">**Why**</span></span>

<span data-ttu-id="8481f-179">Ta zmiana została wprowadzona do umożliwiają łatwiejsze i bardziej spójną do pracy z wykresami odłączonych jednostek podczas korzystania z generowane przez magazyn kluczy.</span><span class="sxs-lookup"><span data-stu-id="8481f-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="8481f-180">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-180">**Mitigations**</span></span>

<span data-ttu-id="8481f-181">Ta zmiana może przerwać aplikacji, jeśli typ jednostki jest skonfigurowany do użycia wygenerowanych kluczy, ale wartości klucza są jawnie ustawione dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="8481f-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="8481f-182">Obejście polega na jawnego konfigurowania właściwości klucza nie Użyj generowane wartości.</span><span class="sxs-lookup"><span data-stu-id="8481f-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="8481f-183">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8481f-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="8481f-184">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="8481f-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="8481f-185">Usuwanie kaskadowe teraz realizowane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="8481f-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="8481f-186">Śledzenie problem #10114</span><span class="sxs-lookup"><span data-stu-id="8481f-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="8481f-187">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-188">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-188">**Old behavior**</span></span>

<span data-ttu-id="8481f-189">Przed 3.0 obejmuje ją Sekwencja akcji programu EF Core stosowane (Usuwanie jednostki zależne, gdy wymagane podmiot zabezpieczeń został usunięty lub gdy relacji z podmiotem zabezpieczeń wymagane jest oddzielone) nie nastąpiły aż SaveChanges została wywołana.</span><span class="sxs-lookup"><span data-stu-id="8481f-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="8481f-190">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-190">**New behavior**</span></span>

<span data-ttu-id="8481f-191">Począwszy od 3.0 programu EF Core dotyczy obejmuje ją Sekwencja akcji zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="8481f-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="8481f-192">Na przykład, wywołanie `context.Remove()` konieczne usunięcie jednostki głównej będzie wynik we wszystkich śledzone związane z wymaganych zależności również ustawiona na wartość `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="8481f-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="8481f-193">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-193">**Why**</span></span>

<span data-ttu-id="8481f-194">Ta zmiana została wprowadzona w celu usprawnienie obsługi wiązaniu danych i inspekcji scenariusze, w których ważne jest, aby zrozumieć, w których obiektach zostaną usunięte _przed_ `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8481f-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="8481f-195">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-195">**Mitigations**</span></span>

<span data-ttu-id="8481f-196">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="8481f-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="8481f-197">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="8481f-198">Typy zapytań i dalszych są skonsolidowane przy użyciu typów jednostek</span><span class="sxs-lookup"><span data-stu-id="8481f-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="8481f-199">Śledzenie problem #14194</span><span class="sxs-lookup"><span data-stu-id="8481f-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="8481f-200">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-201">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-201">**Old behavior**</span></span>

<span data-ttu-id="8481f-202">Przed programem EF Core 3.0 to [typy zapytań](xref:core/modeling/query-types) zostały oznacza wykonywać zapytania względem danych, która nie zdefiniowano klucza podstawowego w taki sposób, ze strukturą.</span><span class="sxs-lookup"><span data-stu-id="8481f-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="8481f-203">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek, bez kluczy (najprawdopodobniej w widoku, ale prawdopodobnie z tabeli) podczas regularnego jednostki typu podczas użyto klucz był dostępny (większe prawdopodobieństwo z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="8481f-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="8481f-204">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-204">**New behavior**</span></span>

<span data-ttu-id="8481f-205">Typ zapytania staje się teraz tylko typ jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8481f-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="8481f-206">Typy bez kluczy jednostki mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="8481f-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="8481f-207">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-207">**Why**</span></span>

<span data-ttu-id="8481f-208">Ta zmiana została wprowadzona do pomyłek wokół celem typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="8481f-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="8481f-209">W szczególności są typy jednostek bez kluczy i w związku z tym są założenia tylko do odczytu, ale nie powinny być używane tylko w przypadku, ponieważ typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="8481f-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="8481f-210">Podobnie często są mapowane do widoków, ale jest to tylko w przypadku, ponieważ często widoki nie określają kluczy.</span><span class="sxs-lookup"><span data-stu-id="8481f-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="8481f-211">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-211">**Mitigations**</span></span>

<span data-ttu-id="8481f-212">Następujące elementy interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="8481f-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="8481f-213">**`ModelBuilder.Query<>()`** — Zamiast tego `ModelBuilder.Entity<>().HasNoKey()` należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="8481f-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="8481f-214">To będzie nadal nie można skonfigurować zgodnie z Konwencją, aby uniknąć błędnej konfiguracji, gdy klucz podstawowy jest oczekiwany, ale nie jest zgodna z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="8481f-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="8481f-215">**`DbQuery<>`** — Zamiast tego `DbSet<>` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="8481f-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="8481f-216">**`DbContext.Query<>()`** — Zamiast tego `DbContext.Set<>()` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="8481f-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="8481f-217">Konfiguracja interfejsu API w przypadku relacji należących do typu została zmieniona</span><span class="sxs-lookup"><span data-stu-id="8481f-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="8481f-218">[Śledzenie problem #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[śledzenia problemu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[śledzenia problemu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="8481f-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="8481f-219">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-220">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-220">**Old behavior**</span></span>

<span data-ttu-id="8481f-221">Przed programem EF Core 3.0 to konfiguracja należących do relacji zostało wykonane bezpośrednio po `OwnsOne` lub `OwnsMany` wywołania.</span><span class="sxs-lookup"><span data-stu-id="8481f-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="8481f-222">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-222">**New behavior**</span></span>

<span data-ttu-id="8481f-223">Począwszy od programu EF Core 3.0 jest teraz wygodnego interfejsu API, aby skonfigurować właściwości nawigacji do właściciela, przy użyciu funkcji `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="8481f-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="8481f-224">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="8481f-225">Powinny teraz zezwalającym konfiguracji związanych z relacją między właściciela, jak i po `WithOwner()` podobnie do sposobu konfiguracji relacje.</span><span class="sxs-lookup"><span data-stu-id="8481f-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="8481f-226">Podczas konfiguracji dla samego typu należące do firmy będą nadal zezwalającym po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="8481f-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="8481f-227">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-227">For example:</span></span>

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

<span data-ttu-id="8481f-228">Ponadto podczas wywoływania `Entity()`, `HasOne()`, lub `Set()` z typem należących do docelowego teraz spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8481f-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="8481f-229">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-229">**Why**</span></span>

<span data-ttu-id="8481f-230">Ta zmiana została wprowadzona do utworzenia jaśniejsze rozdzielenie między skonfigurowaniem należących do samego typu i _relacji_ należących do typu.</span><span class="sxs-lookup"><span data-stu-id="8481f-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="8481f-231">To z kolei usuwa niejednoznaczności i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="8481f-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="8481f-232">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-232">**Mitigations**</span></span>

<span data-ttu-id="8481f-233">Zmień konfigurację relacje typów należących do używania nowych powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8481f-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="8481f-234">Konwencja właściwość klucza obcego nie jest już zgodny takiej samej nazwie jak właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="8481f-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="8481f-235">Śledzenie problem #13274</span><span class="sxs-lookup"><span data-stu-id="8481f-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="8481f-236">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-237">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-237">**Old behavior**</span></span>

<span data-ttu-id="8481f-238">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="8481f-238">Consider the following model:</span></span>
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
<span data-ttu-id="8481f-239">Przed programem EF Core 3.0 to `CustomerId` właściwość będzie używana dla klucza obcego przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="8481f-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="8481f-240">Jednak jeśli `Order` jest typem należące do firmy, dzięki temu upewnisz się również `CustomerId` klucz podstawowy i to nie jest zazwyczaj oczekuje.</span><span class="sxs-lookup"><span data-stu-id="8481f-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="8481f-241">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-241">**New behavior**</span></span>

<span data-ttu-id="8481f-242">Począwszy od 3.0 programu EF Core nie próbował użyć właściwości kluczy obcych zgodnie z Konwencją, jeśli mają taką samą nazwę jak właściwość jednostki.</span><span class="sxs-lookup"><span data-stu-id="8481f-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="8481f-243">Nazwa typu jednostki połączona z nazwą właściwości jednostki, a nazwa nawigacji połączona z wzorców nazwy właściwości podmiotu zabezpieczeń nadal są dopasowywane.</span><span class="sxs-lookup"><span data-stu-id="8481f-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="8481f-244">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-244">For example:</span></span>

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

<span data-ttu-id="8481f-245">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-245">**Why**</span></span>

<span data-ttu-id="8481f-246">Ta zmiana została wprowadzona w celu uniknięcia błędnie Definiowanie właściwości klucza podstawowego w typie należące do firmy.</span><span class="sxs-lookup"><span data-stu-id="8481f-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="8481f-247">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-247">**Mitigations**</span></span>

<span data-ttu-id="8481f-248">Jeśli właściwość ma to być klucza obcego, a więc część klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="8481f-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="8481f-249">Każda właściwość używa generowania kluczy niezależnie od liczby całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="8481f-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="8481f-250">Śledzenie problem #6872</span><span class="sxs-lookup"><span data-stu-id="8481f-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="8481f-251">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-252">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-252">**Old behavior**</span></span>

<span data-ttu-id="8481f-253">Przed programem EF Core 3.0 to co generator wartości udostępnionego był używany dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8481f-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="8481f-254">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-254">**New behavior**</span></span>

<span data-ttu-id="8481f-255">Począwszy od programu EF Core 3.0 właściwości klucza każda liczba całkowita pobiera własnego generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8481f-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="8481f-256">Ponadto jeśli baza danych zostanie usunięty, generowania klucza jest resetowany dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="8481f-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="8481f-257">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-257">**Why**</span></span>

<span data-ttu-id="8481f-258">Ta zmiana została wprowadzona, aby wyrównać generowania klucza w pamięci do generowania kluczy rzeczywista baza danych i poprawić możliwość izolowania testów od siebie nawzajem, korzystając z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="8481f-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="8481f-259">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-259">**Mitigations**</span></span>

<span data-ttu-id="8481f-260">Może to spowodować awarię aplikacji, która jest polegania na określonych wartości klucza w pamięci do ustawienia.</span><span class="sxs-lookup"><span data-stu-id="8481f-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="8481f-261">Rozważ w zamian nie opierając się na konkretnych wartości klucza, lub aktualizacja odpowiadający nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="8481f-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="8481f-262">Domyślnie są używane pola zapasowego</span><span class="sxs-lookup"><span data-stu-id="8481f-262">Backing fields are used by default</span></span>

[<span data-ttu-id="8481f-263">Śledzenie problem #12430</span><span class="sxs-lookup"><span data-stu-id="8481f-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="8481f-264">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8481f-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8481f-265">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-265">**Old behavior**</span></span>

<span data-ttu-id="8481f-266">Przed 3.0 nawet wtedy, gdy pole zapasowe dla właściwości jest znane, programem EF Core będzie domyślnie nadal odczytu i zapisu wartości właściwości, za pomocą metody getter i setter właściwości.</span><span class="sxs-lookup"><span data-stu-id="8481f-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="8481f-267">Wyjątkiem od tej była wykonywania zapytania, gdzie pole zapasowe będzie miał ustawienie bezpośrednio Jeśli jest znany.</span><span class="sxs-lookup"><span data-stu-id="8481f-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="8481f-268">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-268">**New behavior**</span></span>

<span data-ttu-id="8481f-269">Począwszy od programu EF Core 3.0, jeśli pole zapasowe dla właściwości jest znany, następnie będzie zawsze Odczyt i zapis tej właściwości, za pomocą pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="8481f-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="8481f-270">Może to spowodować przerwanie aplikacji, jeżeli aplikacja powołuje się na zachowanie dodatkowego kodowane do metody pobierającą czy ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="8481f-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="8481f-271">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-271">**Why**</span></span>

<span data-ttu-id="8481f-272">Ta zmiana została wprowadzona aby zapobiec błędnie wyzwalanie logiki biznesowej EF Core domyślnie podczas wykonywania operacji bazy danych dotyczących jednostki.</span><span class="sxs-lookup"><span data-stu-id="8481f-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="8481f-273">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-273">**Mitigations**</span></span>

<span data-ttu-id="8481f-274">Zachowanie pre 3.0 można przywrócić za pomocą konfiguracji tryb dostępu do właściwości w interfejsie API fluent element modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="8481f-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="8481f-275">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="8481f-276">Throw, jeśli zostaną znalezione wiele pól zgodnych zapasowy</span><span class="sxs-lookup"><span data-stu-id="8481f-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="8481f-277">Śledzenie problem #12523</span><span class="sxs-lookup"><span data-stu-id="8481f-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="8481f-278">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-279">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-279">**Old behavior**</span></span>

<span data-ttu-id="8481f-280">Przed programem EF Core 3.0 Jeśli pasuje wiele pól reguły służące do znajdowania do pola pomocniczego, właściwości, a następnie będzie można wybrać jedno pole na podstawie niektórych kolejność pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="8481f-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="8481f-281">Może to spowodować nieprawidłowe pole ma być używany w przypadku niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="8481f-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="8481f-282">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-282">**New behavior**</span></span>

<span data-ttu-id="8481f-283">Zaczynając od programu EF Core 3.0 to, jeśli wiele pól są dopasowywane do tej samej właściwości, następnie jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8481f-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="8481f-284">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-284">**Why**</span></span>

<span data-ttu-id="8481f-285">Ta zmiana została wprowadzona w celu uniknięcia dyskretne przy użyciu jedno pole zamiast innego, gdy tylko jeden może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="8481f-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="8481f-286">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-286">**Mitigations**</span></span>

<span data-ttu-id="8481f-287">Właściwości z polami niejednoznaczne zapasowy musi mieć pole do użycia, jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="8481f-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="8481f-288">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8481f-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="8481f-289">AddDbContext/AddDbContextPool już wywołać AddLogging AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="8481f-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="8481f-290">Śledzenie problem #14756</span><span class="sxs-lookup"><span data-stu-id="8481f-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="8481f-291">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-292">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-292">**Old behavior**</span></span>

<span data-ttu-id="8481f-293">Przed programem EF Core 3.0, wywołanie `AddDbContext` lub `AddDbContextPool` będzie również zarejestrować rejestrowanie i buforowanie usług za pomocą D.I za pośrednictwem wywołania do pamięci [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8481f-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="8481f-294">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-294">**New behavior**</span></span>

<span data-ttu-id="8481f-295">Począwszy od programu EF Core 3.0 to `AddDbContext` i `AddDbContextPool` nie będą już rejestrowanie tych usług za pomocą iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="8481f-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="8481f-296">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-296">**Why**</span></span>

<span data-ttu-id="8481f-297">EF Core 3.0 nie wymaga, że te usługi są w cotainer DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="8481f-298">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie nadal będzie on używany przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="8481f-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="8481f-299">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-299">**Mitigations**</span></span>

<span data-ttu-id="8481f-300">Jeśli Twoja aplikacja potrzebuje tych usług, następnie zarejestruj je jawnie przy użyciu kontenera DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8481f-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="8481f-301">DbContext.Entry wykonuje obecnie lokalnego metody DetectChanges</span><span class="sxs-lookup"><span data-stu-id="8481f-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="8481f-302">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="8481f-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8481f-303">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-304">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-304">**Old behavior**</span></span>

<span data-ttu-id="8481f-305">Przed programem EF Core 3.0, wywołanie `DbContext.Entry` spowodowałoby zmiany zostało wykryte, dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="8481f-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="8481f-306">To zapewnić, że stan ujawnione w `EntityEntry` był aktualny.</span><span class="sxs-lookup"><span data-stu-id="8481f-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="8481f-307">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-307">**New behavior**</span></span>

<span data-ttu-id="8481f-308">Począwszy od programu EF Core 3.0, wywołanie `DbContext.Entry` zostanie teraz tylko próba wykrycia zmiany w danej jednostki, a wszystkie śledzone jednostki głównej odnoszących się do niego.</span><span class="sxs-lookup"><span data-stu-id="8481f-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="8481f-309">Oznacza to, że zmiany w innym miejscu może nie została wykryta przez wywołanie tej metody, który może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="8481f-310">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` ustawiono `false` , a następnie nawet wykryciem lokalne zmiany zostaną wyłączone.</span><span class="sxs-lookup"><span data-stu-id="8481f-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="8481f-311">Inne metody, które powodują wykrywania zmian — na przykład `ChangeTracker.Entries` i `SaveChanges`— nadal powodują pełne `DetectChanges` wszystkich śledzone jednostek.</span><span class="sxs-lookup"><span data-stu-id="8481f-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="8481f-312">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-312">**Why**</span></span>

<span data-ttu-id="8481f-313">Ta zmiana została wprowadzona w celu zwiększenia wydajności domyślnego korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="8481f-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="8481f-314">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-314">**Mitigations**</span></span>

<span data-ttu-id="8481f-315">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry` aby zapewnić zachowanie pre 3.0.</span><span class="sxs-lookup"><span data-stu-id="8481f-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="8481f-316">Parametry i bajtów klucze tablicy nie są generowane przez klientów domyślnie</span><span class="sxs-lookup"><span data-stu-id="8481f-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="8481f-317">Śledzenie problem #14617</span><span class="sxs-lookup"><span data-stu-id="8481f-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="8481f-318">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-319">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-319">**Old behavior**</span></span>

<span data-ttu-id="8481f-320">Przed programem EF Core 3.0 to `string` i `byte[]` właściwości klucza, można użyć bez jawne ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="8481f-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="8481f-321">W takim przypadku wartość klucza powinien zostać wygenerowany na komputerze klienckim jako identyfikatora GUID, serializować do bajtów `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="8481f-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="8481f-322">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-322">**New behavior**</span></span>

<span data-ttu-id="8481f-323">Począwszy od programu EF Core 3.0 wyjątek zostanie zgłoszony, wskazujący, że ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="8481f-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="8481f-324">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-324">**Why**</span></span>

<span data-ttu-id="8481f-325">Ta zmiana została wprowadzona ponieważ generowane przez klientów `string` / `byte[]` wartościami ogólnie nie są przydatne i domyślne zachowanie dotarło twardych przeglądanie informacji o wartości klucza wygenerowanego w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="8481f-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="8481f-326">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-326">**Mitigations**</span></span>

<span data-ttu-id="8481f-327">Zachowanie pre 3.0 można uzyskać przez jawne określenie, że właściwości klucza należy używać wartości wygenerowany, jeśli jest ustawiona żadna wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="8481f-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="8481f-328">Na przykład przy użyciu wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8481f-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="8481f-329">Lub przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="8481f-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="8481f-330">Element ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="8481f-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="8481f-331">Śledzenie problem #14698</span><span class="sxs-lookup"><span data-stu-id="8481f-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="8481f-332">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-333">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-333">**Old behavior**</span></span>

<span data-ttu-id="8481f-334">Przed programem EF Core 3.0 to `ILoggerFactory` został zarejestrowany jako usługa pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8481f-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="8481f-335">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-335">**New behavior**</span></span>

<span data-ttu-id="8481f-336">Począwszy od programu EF Core 3.0 to `ILoggerFactory` został zarejestrowany zgodnie z zakresem.</span><span class="sxs-lookup"><span data-stu-id="8481f-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="8481f-337">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-337">**Why**</span></span>

<span data-ttu-id="8481f-338">Ta zmiana została wprowadzona, aby umożliwić skojarzenie rejestratora o `DbContext` wystąpienia, co umożliwia inne funkcje i usuwa czasami patologicznych zachowanie, na przykład rozłożenie wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="8481f-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="8481f-339">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-339">**Mitigations**</span></span>

<span data-ttu-id="8481f-340">Ta zmiana nie powinna wpływu na kod aplikacji, chyba że jest rejestrowanie, a za pomocą niestandardowych usług dla dostawcy usługi wewnętrznej programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="8481f-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="8481f-341">Nie jest to typowe.</span><span class="sxs-lookup"><span data-stu-id="8481f-341">This isn't common.</span></span>
<span data-ttu-id="8481f-342">W takich przypadkach większości zadań będą nadal działają, ale dowolnej usługi singleton, który został w zależności od `ILoggerFactory` będzie musiał zostać zmienione w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="8481f-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="8481f-343">Jeśli napotkasz sytuacjach, w następujący sposób na Zgłoś problem w [narzędzie do śledzenia problemów EF Core w usłudze GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) aby dać nam znać, jak używasz `ILoggerFactory` taki sposób, że firma Microsoft można lepiej zrozumieć, jak się to w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8481f-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="8481f-344">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension została scalona z</span><span class="sxs-lookup"><span data-stu-id="8481f-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="8481f-345">Śledzenie problem #13552</span><span class="sxs-lookup"><span data-stu-id="8481f-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8481f-346">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-347">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-347">**Old behavior**</span></span>

<span data-ttu-id="8481f-348">`IDbContextOptionsExtensionWithDebugInfo` przedłużono dodatkowy opcjonalny interfejs z `IDbContextOptionsExtension` Aby uniknąć konieczności istotne zmiany w interfejsie podczas cyklu wersji 2.x.</span><span class="sxs-lookup"><span data-stu-id="8481f-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="8481f-349">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-349">**New behavior**</span></span>

<span data-ttu-id="8481f-350">Interfejsy są teraz scalone razem `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="8481f-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="8481f-351">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-351">**Why**</span></span>

<span data-ttu-id="8481f-352">Ta zmiana została wprowadzona, ponieważ interfejsy są koncepcyjnie jeden.</span><span class="sxs-lookup"><span data-stu-id="8481f-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="8481f-353">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-353">**Mitigations**</span></span>

<span data-ttu-id="8481f-354">Wszelkie implementacje `IDbContextOptionsExtension` musi zostać zaktualizowany do obsługi nowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="8481f-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="8481f-355">Serwery proxy ładowanych z opóźnieniem nie jest już założono, że właściwości nawigacji są w pełni załadowany</span><span class="sxs-lookup"><span data-stu-id="8481f-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="8481f-356">Śledzenie problem #12780</span><span class="sxs-lookup"><span data-stu-id="8481f-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="8481f-357">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-358">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-358">**Old behavior**</span></span>

<span data-ttu-id="8481f-359">Przed EF Core 3.0 to raz `DbContext` zlikwidowano nie było możliwości określenia, czy danej właściwości nawigacji jednostki uzyskany z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="8481f-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="8481f-360">Serwery proxy zamiast tego będzie założono, że nawigacji odwołanie jest ładowany, jeśli ma on wartość inną niż null i że nawigacji kolekcji jest ładowany, jeśli nie jest pusty.</span><span class="sxs-lookup"><span data-stu-id="8481f-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="8481f-361">W takich przypadkach próby ładowanych z opóźnieniem będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="8481f-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="8481f-362">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-362">**New behavior**</span></span>

<span data-ttu-id="8481f-363">Począwszy od programu EF Core 3.0, serwery proxy zachować informacje o informację określającą, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="8481f-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="8481f-364">Oznacza to, że próba dostępu do właściwości nawigacji, który jest ładowany po został usunięty kontekst zawsze będzie pusta, nawet wtedy, gdy załadowano Nawigacja jest pusty lub ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="8481f-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="8481f-365">Z drugiej strony próby uzyskania dostępu do właściwości nawigacji, który nie został załadowany, spowoduje zgłoszenie wyjątku, jeśli kontekst zostanie usunięty, nawet jeśli właściwość nawigacji jest pusta kolekcja.</span><span class="sxs-lookup"><span data-stu-id="8481f-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="8481f-366">W razie takiej sytuacji, oznacza to, próbę wykorzystania ładowanych z opóźnieniem w czasie nieprawidłowy kod aplikacji i aplikacji, należy zmienić należy tego robić.</span><span class="sxs-lookup"><span data-stu-id="8481f-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="8481f-367">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-367">**Why**</span></span>

<span data-ttu-id="8481f-368">Ta zmiana została wprowadzona się zachowanie spójnego i prawidłowe przy próbie z opóźnieniem obciążenia na usunięte `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8481f-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="8481f-369">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-369">**Mitigations**</span></span>

<span data-ttu-id="8481f-370">Zaktualizować kod aplikacji, aby nie podejmował prób powolne ładowanie kontekstu usunięte lub skonfigurować, aby była pusta, zgodnie z opisem w komunikat o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8481f-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="8481f-371">Nadmierne tworzenia dostawców usług wewnętrznego błędu teraz domyślnie</span><span class="sxs-lookup"><span data-stu-id="8481f-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="8481f-372">Śledzenie problem #10236</span><span class="sxs-lookup"><span data-stu-id="8481f-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="8481f-373">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-374">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-374">**Old behavior**</span></span>

<span data-ttu-id="8481f-375">Przed programem EF Core 3.0 to ostrzeżenie zostanie zarejestrowany dla aplikacji utworzenie patologicznych numeru wewnętrznego usługodawców.</span><span class="sxs-lookup"><span data-stu-id="8481f-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="8481f-376">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-376">**New behavior**</span></span>

<span data-ttu-id="8481f-377">Począwszy od programu EF Core 3.0 to ostrzeżenie została uznana za i zostanie zgłoszony błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8481f-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="8481f-378">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-378">**Why**</span></span>

<span data-ttu-id="8481f-379">Ta zmiana została wprowadzona w celu tworzenia lepszych kod aplikacji za pośrednictwem udostępnianie takim patologicznych bardziej jawny sposób.</span><span class="sxs-lookup"><span data-stu-id="8481f-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="8481f-380">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-380">**Mitigations**</span></span>

<span data-ttu-id="8481f-381">Najbardziej odpowiednią przyczynę akcji po wystąpieniu tego błędu jest poznać główną przyczynę i Zatrzymaj tworzenie tak wielu dostawców usługi wewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="8481f-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="8481f-382">Jednak błąd można przekonwertować ostrzeżenie (lub ignorowane) za pośrednictwem konfiguracji na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8481f-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="8481f-383">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="8481f-384">Nowe zachowanie HasOne/HasMany volat pojedynczy ciąg</span><span class="sxs-lookup"><span data-stu-id="8481f-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="8481f-385">Śledzenie problem #9171</span><span class="sxs-lookup"><span data-stu-id="8481f-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="8481f-386">Ta zmiana zostanie wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="8481f-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8481f-387">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-387">**Old behavior**</span></span>

<span data-ttu-id="8481f-388">Przed programem EF Core 3.0 to kod wywoływania `HasOne` lub `HasMany` przy użyciu jednego ciągu została zinterpretowana w sposób mylące.</span><span class="sxs-lookup"><span data-stu-id="8481f-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="8481f-389">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="8481f-390">Kod wygląda na to jest odnoszących się `Samuri` do niektóre inne jednostki typu przy użyciu `Entrance` właściwość nawigacji, która może być prywatny.</span><span class="sxs-lookup"><span data-stu-id="8481f-390">The code looks like it is relating `Samuri` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="8481f-391">W rzeczywistości, ten kod próbuje utworzyć relację pewnego typu jednostki o nazwie `Entrance` przy użyciu żadnej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="8481f-392">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-392">**New behavior**</span></span>

<span data-ttu-id="8481f-393">Począwszy od programu EF Core 3.0 powyższy kod teraz robi co wyglądało na to powinno mieć czynności przed.</span><span class="sxs-lookup"><span data-stu-id="8481f-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="8481f-394">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-394">**Why**</span></span>

<span data-ttu-id="8481f-395">Stare zachowanie było mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="8481f-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="8481f-396">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-396">**Mitigations**</span></span>

<span data-ttu-id="8481f-397">Spowoduje to przerwanie tylko aplikacji, które jawnego konfigurowania relacji za pomocą ciągów, nazwy typów oraz bez jawne określenie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="8481f-398">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="8481f-398">This is not common.</span></span>
<span data-ttu-id="8481f-399">Poprzednie zachowanie można uzyskać poprzez jawne przekazywanie `null` dla danej nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="8481f-400">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8481f-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="8481f-401">Adnotacja relacyjne: właściwość TypeMapping jest obecnie tylko właściwość TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="8481f-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="8481f-402">Śledzenie problem #9913</span><span class="sxs-lookup"><span data-stu-id="8481f-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="8481f-403">Ta zmiana została wprowadzona w programu EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="8481f-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8481f-404">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-404">**Old behavior**</span></span>

<span data-ttu-id="8481f-405">Nazwa adnotacji dla adnotacji mapowania typu ma wartość "Relacyjne: właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8481f-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="8481f-406">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-406">**New behavior**</span></span>

<span data-ttu-id="8481f-407">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "Właściwość TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8481f-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="8481f-408">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-408">**Why**</span></span>

<span data-ttu-id="8481f-409">Mapowanie typu teraz są używane dla dostawców więcej niż tylko relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8481f-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="8481f-410">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-410">**Mitigations**</span></span>

<span data-ttu-id="8481f-411">Spowoduje to przerwanie tylko aplikacje uzyskujące dostęp do mapowania typów bezpośrednio jako adnotacja nie jest wspólne.</span><span class="sxs-lookup"><span data-stu-id="8481f-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="8481f-412">Najbardziej odpowiednią akcję, aby rozwiązać problem polega na użyciu powierzchni interfejsu API do mapowania typu dostępu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="8481f-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="8481f-413">Zgłasza wyjątek, ToTable na typ pochodny</span><span class="sxs-lookup"><span data-stu-id="8481f-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="8481f-414">Śledzenie problem #11811</span><span class="sxs-lookup"><span data-stu-id="8481f-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="8481f-415">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-416">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-416">**Old behavior**</span></span>

<span data-ttu-id="8481f-417">Przed programem EF Core 3.0 to `ToTable()` wywoływana na pochodnego typu będzie zignorowany, ponieważ tylko strategii mapowania dziedziczenia został TPH, w którym jest on nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="8481f-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="8481f-418">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-418">**New behavior**</span></span>

<span data-ttu-id="8481f-419">Uruchamianie przy użyciu programu EF Core 3.0 i w ramach przygotowania do dodawania TPT i TPC pomocy technicznej w nowszej wersji, `ToTable()` o nazwie na typ pochodny będą teraz throw wyjątek, aby uniknąć zmian nieoczekiwane mapowanie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="8481f-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="8481f-420">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-420">**Why**</span></span>

<span data-ttu-id="8481f-421">Obecnie nie jest prawidłowy do mapowania typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="8481f-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="8481f-422">Ta zmiana eliminuje istotne w przyszłości, kiedy stanie się nieprawidłowy niczego robić.</span><span class="sxs-lookup"><span data-stu-id="8481f-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="8481f-423">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-423">**Mitigations**</span></span>

<span data-ttu-id="8481f-424">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="8481f-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="8481f-425">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="8481f-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="8481f-426">Śledzenie problem #12366</span><span class="sxs-lookup"><span data-stu-id="8481f-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="8481f-427">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-428">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-428">**Old behavior**</span></span>

<span data-ttu-id="8481f-429">Przed programem EF Core 3.0 to `ForSqlServerHasIndex().ForSqlServerInclude()` podany sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="8481f-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="8481f-430">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-430">**New behavior**</span></span>

<span data-ttu-id="8481f-431">Począwszy od programu EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwane na poziomie relacyjnych.</span><span class="sxs-lookup"><span data-stu-id="8481f-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="8481f-432">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="8481f-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="8481f-433">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-433">**Why**</span></span>

<span data-ttu-id="8481f-434">Ta zmiana została wprowadzona w celu skonsolidowania interfejsu API dla indeksów, z `Includes` w jednym miejscu dla wszystkich dostawców w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8481f-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="8481f-435">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-435">**Mitigations**</span></span>

<span data-ttu-id="8481f-436">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="8481f-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="8481f-437">EF Core nie jest już wysyła pragmy do wymuszania klucza Obcego SQLite</span><span class="sxs-lookup"><span data-stu-id="8481f-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="8481f-438">Śledzenie problem #12151</span><span class="sxs-lookup"><span data-stu-id="8481f-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="8481f-439">Ta zmiana została wprowadzona w programu EF Core 3.0 — w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="8481f-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8481f-440">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-440">**Old behavior**</span></span>

<span data-ttu-id="8481f-441">Przed programem EF Core 3.0 to prześle programu EF Core `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8481f-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8481f-442">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-442">**New behavior**</span></span>

<span data-ttu-id="8481f-443">Począwszy od programu EF Core 3.0 programu EF Core nie jest już wysyła `PRAGMA foreign_keys = 1` po otwarciu połączenia bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="8481f-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8481f-444">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-444">**Why**</span></span>

<span data-ttu-id="8481f-445">Ta zmiana została wprowadzona, ponieważ korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3` domyślnie, co z kolei oznacza, że wymuszania klucza Obcego jest włączone domyślnie i nie muszą być jawnie włączone każdorazowo, połączenie jest otwarte.</span><span class="sxs-lookup"><span data-stu-id="8481f-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="8481f-446">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-446">**Mitigations**</span></span>

<span data-ttu-id="8481f-447">Kluczy obcych są domyślnie włączone w SQLitePCLRaw.bundle_e_sqlite3, który jest używany domyślnie dla platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="8481f-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="8481f-448">W pozostałych przypadkach można włączyć kluczy obcych, określając `Foreign Keys=True` w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="8481f-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="8481f-449">Microsoft.EntityFrameworkCore.Sqlite zależy od teraz SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="8481f-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="8481f-450">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-450">**Old behavior**</span></span>

<span data-ttu-id="8481f-451">Przed programem EF Core 3.0, użyć programu EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="8481f-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="8481f-452">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="8481f-452">**New behavior**</span></span>

<span data-ttu-id="8481f-453">Począwszy od programu EF Core 3.0 korzysta z programu EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="8481f-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="8481f-454">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="8481f-454">**Why**</span></span>

<span data-ttu-id="8481f-455">Ta zmiana została wprowadzona, co powoduje wersję bazy danych SQLite w systemie iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="8481f-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="8481f-456">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="8481f-456">**Mitigations**</span></span>

<span data-ttu-id="8481f-457">Aby korzystać z natywnych wersji bazy danych SQLite w systemie iOS, należy skonfigurować `Microsoft.Data.Sqlite` zostać użyty inny `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="8481f-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
