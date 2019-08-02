---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497528"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="42e27-102">Istotne zmiany zawarte w EF Core 3,0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="42e27-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42e27-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, a mimo to, że firma Microsoft podejmie próbę zapewnienia aktualności tej strony, może nie odzwierciedlać naszych najnowszych planów przez cały czas.</span><span class="sxs-lookup"><span data-stu-id="42e27-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="42e27-104">Poniższe zmiany dotyczące interfejsu API i zachowania mogą spowodować przerwanie aplikacji utworzonych dla EF Core 2.2. x podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="42e27-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="42e27-105">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="42e27-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="42e27-106">Przerwy w nowych funkcjach wprowadzonych z jednej wersji zapoznawczej 3,0 do innej wersji zapoznawczej 3,0 nie są tutaj udokumentowane.</span><span class="sxs-lookup"><span data-stu-id="42e27-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="42e27-107">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="42e27-107">Summary</span></span>

| <span data-ttu-id="42e27-108">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="42e27-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="42e27-109">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="42e27-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="42e27-110">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="42e27-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="42e27-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="42e27-111">High</span></span>       |
| [<span data-ttu-id="42e27-112">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="42e27-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="42e27-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="42e27-113">High</span></span>      |
| [<span data-ttu-id="42e27-114">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="42e27-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="42e27-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="42e27-115">High</span></span>      |
| [<span data-ttu-id="42e27-116">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="42e27-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="42e27-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="42e27-117">High</span></span>      |
| [<span data-ttu-id="42e27-118">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="42e27-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="42e27-119">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-119">Medium</span></span>      |
| [<span data-ttu-id="42e27-120">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="42e27-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="42e27-121">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-121">Medium</span></span>      |
| [<span data-ttu-id="42e27-122">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="42e27-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="42e27-123">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-123">Medium</span></span>      |
| [<span data-ttu-id="42e27-124">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="42e27-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="42e27-125">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-125">Medium</span></span>      |
| [<span data-ttu-id="42e27-126">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="42e27-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="42e27-127">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-127">Medium</span></span>      |
| [<span data-ttu-id="42e27-128">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="42e27-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="42e27-129">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-129">Medium</span></span>      |
| [<span data-ttu-id="42e27-130">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="42e27-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="42e27-131">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-131">Medium</span></span>      |
| [<span data-ttu-id="42e27-132">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="42e27-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="42e27-133">Średni</span><span class="sxs-lookup"><span data-stu-id="42e27-133">Medium</span></span>      |
| [<span data-ttu-id="42e27-134">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="42e27-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="42e27-135">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-135">Low</span></span>      |
| [<span data-ttu-id="42e27-136">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="42e27-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="42e27-137">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-137">Low</span></span>      |
| [<span data-ttu-id="42e27-138">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="42e27-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="42e27-139">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-139">Low</span></span>      |
| [<span data-ttu-id="42e27-140">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="42e27-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="42e27-141">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-141">Low</span></span>      |
| [<span data-ttu-id="42e27-142">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="42e27-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="42e27-143">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-143">Low</span></span>      |
| [<span data-ttu-id="42e27-144">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="42e27-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="42e27-145">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-145">Low</span></span>      |
| [<span data-ttu-id="42e27-146">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="42e27-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="42e27-147">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-147">Low</span></span>      |
| [<span data-ttu-id="42e27-148">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="42e27-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="42e27-149">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-149">Low</span></span>      |
| [<span data-ttu-id="42e27-150">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="42e27-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="42e27-151">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-151">Low</span></span>      |
| [<span data-ttu-id="42e27-152">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="42e27-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="42e27-153">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-153">Low</span></span>      |
| [<span data-ttu-id="42e27-154">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="42e27-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="42e27-155">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-155">Low</span></span>      |
| [<span data-ttu-id="42e27-156">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="42e27-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="42e27-157">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-157">Low</span></span>      |
| [<span data-ttu-id="42e27-158">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="42e27-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="42e27-159">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-159">Low</span></span>      |
| [<span data-ttu-id="42e27-160">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="42e27-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="42e27-161">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-161">Low</span></span>      |
| [<span data-ttu-id="42e27-162">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="42e27-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="42e27-163">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-163">Low</span></span>      |
| [<span data-ttu-id="42e27-164">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="42e27-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="42e27-165">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-165">Low</span></span>      |
| [<span data-ttu-id="42e27-166">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="42e27-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="42e27-167">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-167">Low</span></span>      |
| [<span data-ttu-id="42e27-168">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="42e27-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="42e27-169">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-169">Low</span></span>      |
| [<span data-ttu-id="42e27-170">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="42e27-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="42e27-171">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-171">Low</span></span>      |
| [<span data-ttu-id="42e27-172">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="42e27-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="42e27-173">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-173">Low</span></span>      |
| [<span data-ttu-id="42e27-174">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="42e27-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="42e27-175">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-175">Low</span></span>      |
| [<span data-ttu-id="42e27-176">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="42e27-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="42e27-177">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-177">Low</span></span>      |
| [<span data-ttu-id="42e27-178">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="42e27-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="42e27-179">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-179">Low</span></span>      |
| [<span data-ttu-id="42e27-180">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="42e27-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="42e27-181">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-181">Low</span></span>      |
| [<span data-ttu-id="42e27-182">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="42e27-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="42e27-183">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-183">Low</span></span>      |
| [<span data-ttu-id="42e27-184">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="42e27-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="42e27-185">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-185">Low</span></span>      |
| [<span data-ttu-id="42e27-186">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="42e27-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="42e27-187">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-187">Low</span></span>      |
| [<span data-ttu-id="42e27-188">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="42e27-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="42e27-189">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-189">Low</span></span>      |
| [<span data-ttu-id="42e27-190">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="42e27-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="42e27-191">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-191">Low</span></span>      |
| [<span data-ttu-id="42e27-192">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="42e27-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="42e27-193">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-193">Low</span></span>      |
| [<span data-ttu-id="42e27-194">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="42e27-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="42e27-195">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-195">Low</span></span>      |
| [<span data-ttu-id="42e27-196">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="42e27-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="42e27-197">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-197">Low</span></span>      |
| [<span data-ttu-id="42e27-198">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="42e27-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="42e27-199">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-199">Low</span></span>      |
| [<span data-ttu-id="42e27-200">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="42e27-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="42e27-201">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-201">Low</span></span>      |
| [<span data-ttu-id="42e27-202">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="42e27-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="42e27-203">Małe</span><span class="sxs-lookup"><span data-stu-id="42e27-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="42e27-204">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="42e27-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="42e27-205">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="42e27-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="42e27-206">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-207">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-207">**Old behavior**</span></span>

<span data-ttu-id="42e27-208">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="42e27-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="42e27-209">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="42e27-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="42e27-210">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-210">**New behavior**</span></span>

<span data-ttu-id="42e27-211">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu ( `Select()` ostatnie wywołanie zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="42e27-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="42e27-212">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="42e27-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="42e27-213">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-213">**Why**</span></span>

<span data-ttu-id="42e27-214">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="42e27-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="42e27-215">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="42e27-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="42e27-216">Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych, oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="42e27-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="42e27-217">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="42e27-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="42e27-218">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="42e27-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="42e27-219">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="42e27-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="42e27-220">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-220">**Mitigations**</span></span>

<span data-ttu-id="42e27-221">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć lub użyć `AsEnumerable()`, `ToList()`lub podobnie jak jawnie przenieść dane z powrotem do klienta, na którym można następnie przetworzyć je za pomocą LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="42e27-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="42e27-222">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="42e27-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="42e27-223">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="42e27-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="42e27-224">Ta zmiana została wprowadzona w ASP.NET Core 3,0 — wersja zapoznawcza 1.</span><span class="sxs-lookup"><span data-stu-id="42e27-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="42e27-225">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-225">**Old behavior**</span></span>

<span data-ttu-id="42e27-226">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, będzie ona obejmować EF Core i niektórych dostawców danych EF Core, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42e27-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="42e27-227">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-227">**New behavior**</span></span>

<span data-ttu-id="42e27-228">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="42e27-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="42e27-229">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-229">**Why**</span></span>

<span data-ttu-id="42e27-230">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="42e27-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="42e27-231">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="42e27-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="42e27-232">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="42e27-233">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="42e27-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="42e27-234">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-234">**Mitigations**</span></span>

<span data-ttu-id="42e27-235">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="42e27-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="42e27-236">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="42e27-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="42e27-237">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="42e27-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="42e27-238">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4 i odpowiadająca jej wersja zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="42e27-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="42e27-239">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-239">**Old behavior**</span></span>

<span data-ttu-id="42e27-240">Przed 3,0 `dotnet ef` narzędzie zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="42e27-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="42e27-241">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-241">**New behavior**</span></span>

<span data-ttu-id="42e27-242">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera `dotnet ef` narzędzia, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="42e27-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="42e27-243">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-243">**Why**</span></span>

<span data-ttu-id="42e27-244">Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="42e27-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="42e27-245">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-245">**Mitigations**</span></span>

<span data-ttu-id="42e27-246">Aby móc zarządzać migracjami lub szkieletem a `DbContext`, zainstaluj `dotnet-ef` program jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="42e27-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="42e27-247">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="42e27-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="42e27-248">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="42e27-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="42e27-249">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="42e27-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="42e27-250">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-251">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-251">**Old behavior**</span></span>

<span data-ttu-id="42e27-252">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="42e27-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="42e27-253">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-253">**New behavior**</span></span>

<span data-ttu-id="42e27-254">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="42e27-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="42e27-255">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="42e27-256">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i`ExecuteSqlInterpolatedAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przenoszone w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="42e27-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="42e27-257">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="42e27-258">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="42e27-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="42e27-259">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-259">**Why**</span></span>

<span data-ttu-id="42e27-260">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="42e27-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="42e27-261">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="42e27-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="42e27-262">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-262">**Mitigations**</span></span>

<span data-ttu-id="42e27-263">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="42e27-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="42e27-264">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="42e27-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="42e27-265">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="42e27-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="42e27-266">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="42e27-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="42e27-267">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-267">**Old behavior**</span></span>

<span data-ttu-id="42e27-268">Przed EF Core 3,0, `FromSql` Metoda może być określona w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="42e27-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="42e27-269">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-269">**New behavior**</span></span>

<span data-ttu-id="42e27-270">Począwszy od EF Core 3,0, `FromSqlRaw` nowe i `FromSqlInterpolated` metody (zastępujące `FromSql`) można określić tylko dla katalogów głównych `DbSet<>`zapytań, tj. bezpośrednio na.</span><span class="sxs-lookup"><span data-stu-id="42e27-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="42e27-271">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="42e27-272">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-272">**Why**</span></span>

<span data-ttu-id="42e27-273">Określenie `FromSql` dowolnego miejsca, w którym `DbSet` nie ma żadnego dodanego znaczenia ani dodanej wartości, może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="42e27-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="42e27-274">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-274">**Mitigations**</span></span>

<span data-ttu-id="42e27-275">`FromSql`wywołania należy przenieść, aby znajdowały się bezpośrednio w, `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="42e27-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="42e27-276">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="42e27-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="42e27-277">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="42e27-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="42e27-278">Ta zmiana jest przywracana w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="42e27-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="42e27-279">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="42e27-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="42e27-280">Na przykład, aby przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="42e27-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="42e27-281">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="42e27-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="42e27-282">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="42e27-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="42e27-283">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="42e27-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="42e27-284">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-284">**Old behavior**</span></span>

<span data-ttu-id="42e27-285">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="42e27-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="42e27-286">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="42e27-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="42e27-287">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-287">**New behavior**</span></span>

<span data-ttu-id="42e27-288">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="42e27-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="42e27-289">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-289">**Why**</span></span>

<span data-ttu-id="42e27-290">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienie, jest przenoszona do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="42e27-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="42e27-291">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-291">**Mitigations**</span></span>

<span data-ttu-id="42e27-292">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i `Added` należą do jednostek w stanie.</span><span class="sxs-lookup"><span data-stu-id="42e27-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="42e27-293">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="42e27-293">This can be avoided by:</span></span>
* <span data-ttu-id="42e27-294">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="42e27-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="42e27-295">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="42e27-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="42e27-296">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="42e27-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="42e27-297">Na przykład zwróci `context.Entry(blog).Property(e => e.Id).CurrentValue` wartość tymczasową nawet wtedy, gdy `blog.Id` sama nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="42e27-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="42e27-298">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="42e27-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="42e27-299">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="42e27-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="42e27-300">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-301">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-301">**Old behavior**</span></span>

<span data-ttu-id="42e27-302">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` zostałaby śledzona `Added` w stanie i wstawiona jako nowy wiersz, gdy `SaveChanges` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="42e27-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="42e27-303">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-303">**New behavior**</span></span>

<span data-ttu-id="42e27-304">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w `Modified` stanie.</span><span class="sxs-lookup"><span data-stu-id="42e27-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="42e27-305">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po `SaveChanges` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="42e27-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="42e27-306">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona tak `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="42e27-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="42e27-307">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-307">**Why**</span></span>

<span data-ttu-id="42e27-308">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="42e27-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="42e27-309">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-309">**Mitigations**</span></span>

<span data-ttu-id="42e27-310">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="42e27-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="42e27-311">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="42e27-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="42e27-312">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="42e27-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="42e27-313">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="42e27-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="42e27-314">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="42e27-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="42e27-315">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="42e27-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="42e27-316">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-317">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-317">**Old behavior**</span></span>

<span data-ttu-id="42e27-318">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="42e27-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="42e27-319">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-319">**New behavior**</span></span>

<span data-ttu-id="42e27-320">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="42e27-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="42e27-321">Na przykład wywołanie `context.Remove()` usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane zależności są również ustawione na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="42e27-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="42e27-322">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-322">**Why**</span></span>

<span data-ttu-id="42e27-323">Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="42e27-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="42e27-324">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-324">**Mitigations**</span></span>

<span data-ttu-id="42e27-325">Poprzednie zachowanie można przywrócić za pomocą ustawień na stronie `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="42e27-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="42e27-326">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="42e27-327">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="42e27-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="42e27-328">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="42e27-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="42e27-329">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 5.</span><span class="sxs-lookup"><span data-stu-id="42e27-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="42e27-330">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-330">**Old behavior**</span></span>

<span data-ttu-id="42e27-331">Przed 3,0, `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z `Restrict` semantyką, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.</span><span class="sxs-lookup"><span data-stu-id="42e27-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="42e27-332">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-332">**New behavior**</span></span>

<span data-ttu-id="42e27-333">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone `Restrict` z semantyką--to nie, bez kaskad; throw przy naruszeniu ograniczenia — bez również wpływu na wewnętrzną naprawę EF.</span><span class="sxs-lookup"><span data-stu-id="42e27-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="42e27-334">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-334">**Why**</span></span>

<span data-ttu-id="42e27-335">Ta zmiana została wprowadzona w celu usprawnienia korzystania `DeleteBehavior` z programu w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="42e27-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="42e27-336">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-336">**Mitigations**</span></span>

<span data-ttu-id="42e27-337">Poprzednie zachowanie można przywrócić za pomocą polecenia `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="42e27-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="42e27-338">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="42e27-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="42e27-339">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="42e27-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="42e27-340">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-341">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-341">**Old behavior**</span></span>

<span data-ttu-id="42e27-342">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/query-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="42e27-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="42e27-343">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="42e27-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="42e27-344">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-344">**New behavior**</span></span>

<span data-ttu-id="42e27-345">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="42e27-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="42e27-346">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="42e27-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="42e27-347">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-347">**Why**</span></span>

<span data-ttu-id="42e27-348">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="42e27-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="42e27-349">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="42e27-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="42e27-350">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="42e27-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="42e27-351">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-351">**Mitigations**</span></span>

<span data-ttu-id="42e27-352">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="42e27-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="42e27-353">**`ModelBuilder.Query<>()`** — Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="42e27-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="42e27-354">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="42e27-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="42e27-355">**`DbQuery<>`** — Zamiast `DbSet<>` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="42e27-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="42e27-356">**`DbContext.Query<>()`** — Zamiast `DbContext.Set<>()` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="42e27-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="42e27-357">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="42e27-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="42e27-358">[Śledzenie](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
problemów ze śledzeniem #12444[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
śledzenia problemów[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="42e27-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="42e27-359">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-360">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-360">**Old behavior**</span></span>

<span data-ttu-id="42e27-361">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po `OwnsOne` wywołaniu lub. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="42e27-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="42e27-362">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-362">**New behavior**</span></span>

<span data-ttu-id="42e27-363">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="42e27-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="42e27-364">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="42e27-365">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem `WithOwner()` po podobnym sposobie, jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="42e27-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="42e27-366">Mimo że konfiguracja dla samego samego typu jest nadal łańcuchem `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="42e27-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="42e27-367">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-367">For example:</span></span>

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

<span data-ttu-id="42e27-368">Dodatkowo wywoływanie `Entity()`,, `Set()` lub z obiektem docelowym typu, `HasOne()`spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="42e27-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="42e27-369">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-369">**Why**</span></span>

<span data-ttu-id="42e27-370">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="42e27-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="42e27-371">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, `HasForeignKey`takich jak.</span><span class="sxs-lookup"><span data-stu-id="42e27-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="42e27-372">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-372">**Mitigations**</span></span>

<span data-ttu-id="42e27-373">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="42e27-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="42e27-374">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="42e27-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="42e27-375">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="42e27-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="42e27-376">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-377">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-377">**Old behavior**</span></span>

<span data-ttu-id="42e27-378">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="42e27-378">Consider the following model:</span></span>
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
<span data-ttu-id="42e27-379">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli `OrderDetails` , wystąpienie jest zawsze wymagane podczas dodawania nowego `Order`elementu.</span><span class="sxs-lookup"><span data-stu-id="42e27-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="42e27-380">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-380">**New behavior**</span></span>

<span data-ttu-id="42e27-381">Począwszy od 3,0, EF Core umożliwia dodanie `Order` , `OrderDetails` bez `OrderDetails` i mapuje wszystkie właściwości, z wyjątkiem klucza podstawowego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="42e27-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="42e27-382">Podczas wykonywania zapytania o zestawy `OrderDetails` EF Core `null` do, jeśli którekolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="42e27-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="42e27-383">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-383">**Mitigations**</span></span>

<span data-ttu-id="42e27-384">Jeśli model ma zależeć do udostępniania tabeli dla wszystkich opcjonalnych kolumn, ale Nawigacja wskazująca, że nie jest oczekiwana `null` , aplikacja powinna zostać zmodyfikowana, aby obsługiwać przypadki, gdy Nawigacja `null`jest.</span><span class="sxs-lookup"><span data-stu-id="42e27-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="42e27-385">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć`null` przypisaną do niej inną wartość.</span><span class="sxs-lookup"><span data-stu-id="42e27-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="42e27-386">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="42e27-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="42e27-387">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="42e27-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="42e27-388">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-389">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-389">**Old behavior**</span></span>

<span data-ttu-id="42e27-390">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="42e27-390">Consider the following model:</span></span>
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
<span data-ttu-id="42e27-391">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="42e27-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="42e27-392">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-392">**New behavior**</span></span>

<span data-ttu-id="42e27-393">Począwszy od 3,0, EF Core propaguje nową `Version` wartość do `Order` , jeśli jest właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="42e27-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="42e27-394">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="42e27-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="42e27-395">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-395">**Why**</span></span>

<span data-ttu-id="42e27-396">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="42e27-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="42e27-397">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-397">**Mitigations**</span></span>

<span data-ttu-id="42e27-398">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="42e27-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="42e27-399">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="42e27-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="42e27-400">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="42e27-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="42e27-401">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="42e27-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="42e27-402">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-403">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-403">**Old behavior**</span></span>

<span data-ttu-id="42e27-404">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="42e27-404">Consider the following model:</span></span>
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

<span data-ttu-id="42e27-405">Przed EF Core 3,0 `ShippingAddress` Właściwość zostałaby zamapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="42e27-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="42e27-406">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-406">**New behavior**</span></span>

<span data-ttu-id="42e27-407">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="42e27-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="42e27-408">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-408">**Why**</span></span>

<span data-ttu-id="42e27-409">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="42e27-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="42e27-410">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-410">**Mitigations**</span></span>

<span data-ttu-id="42e27-411">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="42e27-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="42e27-412">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="42e27-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="42e27-413">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="42e27-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="42e27-414">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-415">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-415">**Old behavior**</span></span>

<span data-ttu-id="42e27-416">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="42e27-416">Consider the following model:</span></span>
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
<span data-ttu-id="42e27-417">Przed EF Core 3,0, `CustomerId` właściwość będzie używana dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="42e27-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="42e27-418">Jeśli `Order` jednak jest typem będącym własnością, to `CustomerId` również klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="42e27-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="42e27-419">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-419">**New behavior**</span></span>

<span data-ttu-id="42e27-420">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="42e27-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="42e27-421">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="42e27-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="42e27-422">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-422">For example:</span></span>

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

<span data-ttu-id="42e27-423">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-423">**Why**</span></span>

<span data-ttu-id="42e27-424">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="42e27-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="42e27-425">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-425">**Mitigations**</span></span>

<span data-ttu-id="42e27-426">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="42e27-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="42e27-427">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="42e27-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="42e27-428">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="42e27-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="42e27-429">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-430">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-430">**Old behavior**</span></span>

<span data-ttu-id="42e27-431">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="42e27-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="42e27-432">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-432">**New behavior**</span></span>

<span data-ttu-id="42e27-433">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="42e27-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="42e27-434">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-434">**Why**</span></span>

<span data-ttu-id="42e27-435">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="42e27-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="42e27-436">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="42e27-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="42e27-437">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-437">**Mitigations**</span></span>

<span data-ttu-id="42e27-438">Jeśli połączenie musi pozostać otwarte jawne wywołanie w celu `OpenConnection()` upewnienia się, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="42e27-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="42e27-439">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="42e27-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="42e27-440">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="42e27-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="42e27-441">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-442">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-442">**Old behavior**</span></span>

<span data-ttu-id="42e27-443">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="42e27-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="42e27-444">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-444">**New behavior**</span></span>

<span data-ttu-id="42e27-445">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="42e27-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="42e27-446">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="42e27-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="42e27-447">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-447">**Why**</span></span>

<span data-ttu-id="42e27-448">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="42e27-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="42e27-449">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-449">**Mitigations**</span></span>

<span data-ttu-id="42e27-450">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="42e27-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="42e27-451">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="42e27-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="42e27-452">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="42e27-452">Backing fields are used by default</span></span>

[<span data-ttu-id="42e27-453">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="42e27-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="42e27-454">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="42e27-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="42e27-455">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-455">**Old behavior**</span></span>

<span data-ttu-id="42e27-456">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="42e27-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="42e27-457">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="42e27-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="42e27-458">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-458">**New behavior**</span></span>

<span data-ttu-id="42e27-459">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="42e27-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="42e27-460">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="42e27-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="42e27-461">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-461">**Why**</span></span>

<span data-ttu-id="42e27-462">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="42e27-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="42e27-463">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-463">**Mitigations**</span></span>

<span data-ttu-id="42e27-464">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu `ModelBuilder`dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="42e27-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="42e27-465">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="42e27-466">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="42e27-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="42e27-467">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="42e27-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="42e27-468">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-469">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-469">**Old behavior**</span></span>

<span data-ttu-id="42e27-470">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="42e27-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="42e27-471">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="42e27-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="42e27-472">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-472">**New behavior**</span></span>

<span data-ttu-id="42e27-473">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="42e27-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="42e27-474">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-474">**Why**</span></span>

<span data-ttu-id="42e27-475">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="42e27-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="42e27-476">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-476">**Mitigations**</span></span>

<span data-ttu-id="42e27-477">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="42e27-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="42e27-478">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="42e27-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="42e27-479">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="42e27-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="42e27-480">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-481">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-481">**Old behavior**</span></span>

<span data-ttu-id="42e27-482">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie CLR, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="42e27-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="42e27-483">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-483">**New behavior**</span></span>

<span data-ttu-id="42e27-484">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="42e27-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="42e27-485">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-485">**Why**</span></span>

<span data-ttu-id="42e27-486">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="42e27-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="42e27-487">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-487">**Mitigations**</span></span>

<span data-ttu-id="42e27-488">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="42e27-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="42e27-489">W nowszej wersji zapoznawczej EF Core 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która różni się od nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="42e27-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="42e27-490">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="42e27-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="42e27-491">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="42e27-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="42e27-492">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-493">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-493">**Old behavior**</span></span>

<span data-ttu-id="42e27-494">Przed EF Core 3,0 `AddDbContext` , wywoływanie `AddDbContextPool` lub zarejestrowanie usługi rejestrowania i pamięci podręcznej w usłudze D. I przez wywołania funkcji addlogging i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache). [](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging)</span><span class="sxs-lookup"><span data-stu-id="42e27-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="42e27-495">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-495">**New behavior**</span></span>

<span data-ttu-id="42e27-496">Począwszy od EF Core 3,0 `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="42e27-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="42e27-497">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-497">**Why**</span></span>

<span data-ttu-id="42e27-498">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="42e27-499">Jeśli `ILoggerFactory` jednak program jest zarejestrowany w kontenerze di aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="42e27-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="42e27-500">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-500">**Mitigations**</span></span>

<span data-ttu-id="42e27-501">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji addlogging lub [](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="42e27-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="42e27-502">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="42e27-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="42e27-503">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="42e27-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="42e27-504">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-505">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-505">**Old behavior**</span></span>

<span data-ttu-id="42e27-506">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="42e27-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="42e27-507">W ten sposób zapewniono, że stan ujawniony na `EntityEntry` bieżąco.</span><span class="sxs-lookup"><span data-stu-id="42e27-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="42e27-508">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-508">**New behavior**</span></span>

<span data-ttu-id="42e27-509">Począwszy od EF Core 3,0, wywołanie `DbContext.Entry` będzie teraz podejmować próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="42e27-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="42e27-510">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="42e27-511">Należy pamiętać, `ChangeTracker.AutoDetectChangesEnabled` że jeśli jest `false` ustawiona na, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="42e27-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="42e27-512">Inne metody, które powodują wykrycie zmiany — na `ChangeTracker.Entries` przykład `SaveChanges`i--nadal powodują pełne `DetectChanges` wszystkie śledzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="42e27-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="42e27-513">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-513">**Why**</span></span>

<span data-ttu-id="42e27-514">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania `context.Entry`z programu.</span><span class="sxs-lookup"><span data-stu-id="42e27-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="42e27-515">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-515">**Mitigations**</span></span>

<span data-ttu-id="42e27-516">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed `Entry` wywołaniem, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="42e27-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="42e27-517">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="42e27-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="42e27-518">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="42e27-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="42e27-519">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-520">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-520">**Old behavior**</span></span>

<span data-ttu-id="42e27-521">Przed EF Core 3,0 `string` i `byte[]` właściwości klucza mogą być używane bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="42e27-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="42e27-522">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, Zserializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="42e27-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="42e27-523">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-523">**New behavior**</span></span>

<span data-ttu-id="42e27-524">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="42e27-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="42e27-525">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-525">**Why**</span></span>

<span data-ttu-id="42e27-526">Ta zmiana została wprowadzona, ponieważ zazwyczaj wartości `string` generowane / `byte[]` przez klienta nie są przydatne, a domyślne zachowanie spowodowało trudne powody dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="42e27-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="42e27-527">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-527">**Mitigations**</span></span>

<span data-ttu-id="42e27-528">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="42e27-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="42e27-529">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="42e27-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="42e27-530">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="42e27-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="42e27-531">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="42e27-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="42e27-532">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="42e27-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="42e27-533">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-534">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-534">**Old behavior**</span></span>

<span data-ttu-id="42e27-535">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="42e27-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="42e27-536">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-536">**New behavior**</span></span>

<span data-ttu-id="42e27-537">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="42e27-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="42e27-538">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-538">**Why**</span></span>

<span data-ttu-id="42e27-539">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z `DbContext` wystąpieniem, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="42e27-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="42e27-540">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-540">**Mitigations**</span></span>

<span data-ttu-id="42e27-541">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="42e27-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="42e27-542">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="42e27-542">This isn't common.</span></span>
<span data-ttu-id="42e27-543">W takich przypadkach większość elementów będzie nadal działać, ale wszystkie pojedyncze usługi, w `ILoggerFactory` zależności od tego, muszą zostać zmienione w celu `ILoggerFactory` uzyskania w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="42e27-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="42e27-544">Jeśli używasz `ILoggerFactory` takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak można lepiej zrozumieć, jak nie należy ponownie rozbić w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="42e27-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="42e27-545">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="42e27-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="42e27-546">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="42e27-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="42e27-547">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-548">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-548">**Old behavior**</span></span>

<span data-ttu-id="42e27-549">Przed EF Core 3,0, gdy zostanie `DbContext` usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="42e27-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="42e27-550">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="42e27-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="42e27-551">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="42e27-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="42e27-552">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-552">**New behavior**</span></span>

<span data-ttu-id="42e27-553">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="42e27-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="42e27-554">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="42e27-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="42e27-555">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="42e27-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="42e27-556">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="42e27-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="42e27-557">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-557">**Why**</span></span>

<span data-ttu-id="42e27-558">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie przy próbie załadowania `DbContext` opóźnionego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="42e27-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="42e27-559">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-559">**Mitigations**</span></span>

<span data-ttu-id="42e27-560">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="42e27-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="42e27-561">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="42e27-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="42e27-562">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="42e27-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="42e27-563">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-564">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-564">**Old behavior**</span></span>

<span data-ttu-id="42e27-565">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="42e27-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="42e27-566">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-566">**New behavior**</span></span>

<span data-ttu-id="42e27-567">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="42e27-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="42e27-568">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-568">**Why**</span></span>

<span data-ttu-id="42e27-569">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="42e27-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="42e27-570">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-570">**Mitigations**</span></span>

<span data-ttu-id="42e27-571">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="42e27-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="42e27-572">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację w `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42e27-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="42e27-573">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="42e27-574">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="42e27-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="42e27-575">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="42e27-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="42e27-576">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-577">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-577">**Old behavior**</span></span>

<span data-ttu-id="42e27-578">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="42e27-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="42e27-579">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="42e27-580">Kod wygląda tak `Samurai` , jakby odnosił się do innego typu jednostki `Entrance` przy użyciu właściwości nawigacji, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="42e27-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="42e27-581">W rzeczywistości ten kod próbuje utworzyć relację z typem obiektu o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="42e27-582">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-582">**New behavior**</span></span>

<span data-ttu-id="42e27-583">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="42e27-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="42e27-584">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-584">**Why**</span></span>

<span data-ttu-id="42e27-585">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="42e27-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="42e27-586">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-586">**Mitigations**</span></span>

<span data-ttu-id="42e27-587">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="42e27-588">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="42e27-588">This is not common.</span></span>
<span data-ttu-id="42e27-589">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="42e27-590">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="42e27-591">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="42e27-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="42e27-592">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="42e27-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="42e27-593">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-594">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-594">**Old behavior**</span></span>

<span data-ttu-id="42e27-595">Następujące metody asynchroniczne zwróciły `Task<T>`wcześniej:</span><span class="sxs-lookup"><span data-stu-id="42e27-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="42e27-596">`ValueGenerator.NextValueAsync()`(i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="42e27-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="42e27-597">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-597">**New behavior**</span></span>

<span data-ttu-id="42e27-598">Powyższe metody teraz zwracają wartość `ValueTask<T>` powyżej `T` , tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="42e27-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="42e27-599">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-599">**Why**</span></span>

<span data-ttu-id="42e27-600">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="42e27-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="42e27-601">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-601">**Mitigations**</span></span>

<span data-ttu-id="42e27-602">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="42e27-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="42e27-603">Bardziej złożone użycie (np. przekazanie zwróconej `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` przekonwertowanie na a `Task<T>` przez wywołanie `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="42e27-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="42e27-604">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="42e27-605">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="42e27-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="42e27-606">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="42e27-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="42e27-607">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="42e27-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="42e27-608">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-608">**Old behavior**</span></span>

<span data-ttu-id="42e27-609">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="42e27-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="42e27-610">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-610">**New behavior**</span></span>

<span data-ttu-id="42e27-611">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="42e27-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="42e27-612">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-612">**Why**</span></span>

<span data-ttu-id="42e27-613">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="42e27-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="42e27-614">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-614">**Mitigations**</span></span>

<span data-ttu-id="42e27-615">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="42e27-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="42e27-616">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="42e27-617">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="42e27-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="42e27-618">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="42e27-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="42e27-619">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-620">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-620">**Old behavior**</span></span>

<span data-ttu-id="42e27-621">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="42e27-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="42e27-622">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-622">**New behavior**</span></span>

<span data-ttu-id="42e27-623">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, wywołana `ToTable()` dla typu pochodnego będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="42e27-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="42e27-624">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-624">**Why**</span></span>

<span data-ttu-id="42e27-625">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="42e27-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="42e27-626">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="42e27-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="42e27-627">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-627">**Mitigations**</span></span>

<span data-ttu-id="42e27-628">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="42e27-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="42e27-629">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="42e27-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="42e27-630">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="42e27-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="42e27-631">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-632">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-632">**Old behavior**</span></span>

<span data-ttu-id="42e27-633">Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnia sposób konfigurowania kolumn używanych w programie. `INCLUDE`</span><span class="sxs-lookup"><span data-stu-id="42e27-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="42e27-634">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-634">**New behavior**</span></span>

<span data-ttu-id="42e27-635">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="42e27-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="42e27-636">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="42e27-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="42e27-637">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-637">**Why**</span></span>

<span data-ttu-id="42e27-638">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="42e27-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="42e27-639">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-639">**Mitigations**</span></span>

<span data-ttu-id="42e27-640">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="42e27-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="42e27-641">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="42e27-641">Metadata API changes</span></span>

[<span data-ttu-id="42e27-642">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="42e27-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="42e27-643">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-644">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-644">**New behavior**</span></span>

<span data-ttu-id="42e27-645">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="42e27-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="42e27-646">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-646">**Why**</span></span>

<span data-ttu-id="42e27-647">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="42e27-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="42e27-648">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-648">**Mitigations**</span></span>

<span data-ttu-id="42e27-649">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="42e27-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="42e27-650">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="42e27-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="42e27-651">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="42e27-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="42e27-652">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="42e27-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="42e27-653">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-653">**New behavior**</span></span>

<span data-ttu-id="42e27-654">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="42e27-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="42e27-655">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-655">**Why**</span></span>

<span data-ttu-id="42e27-656">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="42e27-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="42e27-657">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-657">**Mitigations**</span></span>

<span data-ttu-id="42e27-658">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="42e27-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="42e27-659">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="42e27-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="42e27-660">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="42e27-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="42e27-661">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="42e27-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="42e27-662">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-662">**Old behavior**</span></span>

<span data-ttu-id="42e27-663">Przed EF Core 3,0, EF Core wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z systemem SQLite.</span><span class="sxs-lookup"><span data-stu-id="42e27-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="42e27-664">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-664">**New behavior**</span></span>

<span data-ttu-id="42e27-665">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="42e27-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="42e27-666">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-666">**Why**</span></span>

<span data-ttu-id="42e27-667">Ta zmiana została wprowadzona, ponieważ domyślnie `SQLitePCLRaw.bundle_e_sqlite3` używa EF Core, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="42e27-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="42e27-668">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-668">**Mitigations**</span></span>

<span data-ttu-id="42e27-669">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="42e27-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="42e27-670">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="42e27-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="42e27-671">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="42e27-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="42e27-672">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-672">**Old behavior**</span></span>

<span data-ttu-id="42e27-673">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="42e27-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="42e27-674">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-674">**New behavior**</span></span>

<span data-ttu-id="42e27-675">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="42e27-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="42e27-676">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-676">**Why**</span></span>

<span data-ttu-id="42e27-677">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="42e27-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="42e27-678">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-678">**Mitigations**</span></span>

<span data-ttu-id="42e27-679">Aby korzystać z natywnej wersji programu SQLite w systemie `Microsoft.Data.Sqlite` iOS, należy skonfigurować `SQLitePCLRaw` do korzystania z innego pakietu.</span><span class="sxs-lookup"><span data-stu-id="42e27-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="42e27-680">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="42e27-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="42e27-681">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="42e27-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="42e27-682">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-683">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-683">**Old behavior**</span></span>

<span data-ttu-id="42e27-684">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="42e27-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="42e27-685">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-685">**New behavior**</span></span>

<span data-ttu-id="42e27-686">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="42e27-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="42e27-687">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-687">**Why**</span></span>

<span data-ttu-id="42e27-688">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="42e27-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="42e27-689">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="42e27-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="42e27-690">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-690">**Mitigations**</span></span>

<span data-ttu-id="42e27-691">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="42e27-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="42e27-692">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="42e27-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="42e27-693">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="42e27-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="42e27-694">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="42e27-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="42e27-695">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="42e27-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="42e27-696">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-697">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-697">**Old behavior**</span></span>

<span data-ttu-id="42e27-698">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="42e27-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="42e27-699">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="42e27-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="42e27-700">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-700">**New behavior**</span></span>

<span data-ttu-id="42e27-701">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="42e27-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="42e27-702">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-702">**Why**</span></span>

<span data-ttu-id="42e27-703">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="42e27-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="42e27-704">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-704">**Mitigations**</span></span>

<span data-ttu-id="42e27-705">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="42e27-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="42e27-706">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="42e27-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="42e27-707">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="42e27-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="42e27-708">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="42e27-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="42e27-709">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="42e27-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="42e27-710">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-711">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-711">**Old behavior**</span></span>

<span data-ttu-id="42e27-712">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="42e27-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="42e27-713">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-713">**New behavior**</span></span>

<span data-ttu-id="42e27-714">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="42e27-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="42e27-715">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-715">**Why**</span></span>

<span data-ttu-id="42e27-716">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="42e27-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="42e27-717">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="42e27-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="42e27-718">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-718">**Mitigations**</span></span>

<span data-ttu-id="42e27-719">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="42e27-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="42e27-720">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="42e27-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="42e27-721">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="42e27-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="42e27-722">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="42e27-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="42e27-723">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="42e27-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="42e27-724">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="42e27-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="42e27-725">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="42e27-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="42e27-726">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-726">**Old behavior**</span></span>

<span data-ttu-id="42e27-727">Przed EF Core 3,0 `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="42e27-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="42e27-728">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-728">**New behavior**</span></span>

<span data-ttu-id="42e27-729">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42e27-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="42e27-730">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-730">**Why**</span></span>

<span data-ttu-id="42e27-731">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="42e27-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="42e27-732">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-732">**Mitigations**</span></span>

<span data-ttu-id="42e27-733">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="42e27-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="42e27-734">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze śledzeniem ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="42e27-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="42e27-735">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="42e27-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="42e27-736">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="42e27-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="42e27-737">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="42e27-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="42e27-738">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="42e27-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="42e27-739">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-739">**Old behavior**</span></span>

<span data-ttu-id="42e27-740">`IDbContextOptionsExtension`zawiera metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="42e27-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="42e27-741">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-741">**New behavior**</span></span>

<span data-ttu-id="42e27-742">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="42e27-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="42e27-743">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-743">**Why**</span></span>

<span data-ttu-id="42e27-744">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="42e27-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="42e27-745">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="42e27-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="42e27-746">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-746">**Mitigations**</span></span>

<span data-ttu-id="42e27-747">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="42e27-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="42e27-748">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="42e27-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="42e27-749">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="42e27-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="42e27-750">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="42e27-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="42e27-751">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-752">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="42e27-752">**Change**</span></span>

<span data-ttu-id="42e27-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="42e27-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="42e27-754">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-754">**Why**</span></span>

<span data-ttu-id="42e27-755">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="42e27-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="42e27-756">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-756">**Mitigations**</span></span>

<span data-ttu-id="42e27-757">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="42e27-757">Use the new name.</span></span> <span data-ttu-id="42e27-758">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="42e27-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="42e27-759">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="42e27-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="42e27-760">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="42e27-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="42e27-761">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-762">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-762">**Old behavior**</span></span>

<span data-ttu-id="42e27-763">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="42e27-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="42e27-764">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="42e27-765">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-765">**New behavior**</span></span>

<span data-ttu-id="42e27-766">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="42e27-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="42e27-767">Przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="42e27-768">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-768">**Why**</span></span>

<span data-ttu-id="42e27-769">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="42e27-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="42e27-770">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-770">**Mitigations**</span></span>

<span data-ttu-id="42e27-771">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="42e27-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="42e27-772">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="42e27-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="42e27-773">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="42e27-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="42e27-774">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="42e27-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="42e27-775">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-775">**Old behavior**</span></span>

<span data-ttu-id="42e27-776">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="42e27-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="42e27-777">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-777">**New behavior**</span></span>

<span data-ttu-id="42e27-778">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="42e27-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="42e27-779">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-779">**Why**</span></span>

<span data-ttu-id="42e27-780">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="42e27-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="42e27-781">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="42e27-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="42e27-782">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-782">**Mitigations**</span></span>

<span data-ttu-id="42e27-783">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="42e27-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="42e27-784">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="42e27-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="42e27-785">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="42e27-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="42e27-786">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="42e27-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="42e27-787">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-787">**Old behavior**</span></span>

<span data-ttu-id="42e27-788">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="42e27-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="42e27-789">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-789">**New behavior**</span></span>

<span data-ttu-id="42e27-790">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="42e27-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="42e27-791">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="42e27-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="42e27-792">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-792">**Why**</span></span>

<span data-ttu-id="42e27-793">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="42e27-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="42e27-794">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="42e27-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="42e27-795">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="42e27-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="42e27-796">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-796">**Mitigations**</span></span>

<span data-ttu-id="42e27-797">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="42e27-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="42e27-798">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="42e27-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="42e27-799">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="42e27-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="42e27-800">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="42e27-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="42e27-801">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="42e27-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="42e27-802">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-802">**Old behavior**</span></span>

<span data-ttu-id="42e27-803">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="42e27-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="42e27-804">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-804">**New behavior**</span></span>

<span data-ttu-id="42e27-805">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="42e27-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="42e27-806">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-806">**Why**</span></span>

<span data-ttu-id="42e27-807">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="42e27-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="42e27-808">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="42e27-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="42e27-809">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-809">**Mitigations**</span></span>

<span data-ttu-id="42e27-810">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="42e27-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="42e27-811">Szczegółowe informacje można znaleźć w informacjach o [wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="42e27-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="42e27-812">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="42e27-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="42e27-813">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="42e27-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="42e27-814">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="42e27-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="42e27-815">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-815">**Old behavior**</span></span>

<span data-ttu-id="42e27-816">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="42e27-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="42e27-817">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-817">**New behavior**</span></span>

<span data-ttu-id="42e27-818">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="42e27-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="42e27-819">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-819">**Why**</span></span>

<span data-ttu-id="42e27-820">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="42e27-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="42e27-821">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-821">**Mitigations**</span></span>

<span data-ttu-id="42e27-822">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="42e27-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="42e27-823">Szczegółowe informacje można znaleźć w informacjach o [wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="42e27-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="42e27-824">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="42e27-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="42e27-825">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="42e27-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="42e27-826">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="42e27-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="42e27-827">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-827">**Old behavior**</span></span>

<span data-ttu-id="42e27-828">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="42e27-829">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-829">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="42e27-830">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="42e27-830">**New behavior**</span></span>

<span data-ttu-id="42e27-831">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="42e27-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="42e27-832">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="42e27-832">**Why**</span></span>

<span data-ttu-id="42e27-833">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="42e27-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="42e27-834">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="42e27-834">**Mitigations**</span></span>

<span data-ttu-id="42e27-835">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="42e27-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="42e27-836">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e27-836">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
