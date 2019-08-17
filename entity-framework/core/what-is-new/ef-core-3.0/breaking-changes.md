---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565314"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="64279-102">Istotne zmiany zawarte w EF Core 3,0 (obecnie w wersji zapoznawczej)</span><span class="sxs-lookup"><span data-stu-id="64279-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64279-103">Należy pamiętać, że zestawy funkcji i harmonogramy przyszłych wersji zawsze mogą ulec zmianie, a mimo to, że firma Microsoft podejmie próbę zapewnienia aktualności tej strony, może nie odzwierciedlać naszych najnowszych planów przez cały czas.</span><span class="sxs-lookup"><span data-stu-id="64279-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="64279-104">Poniższe zmiany dotyczące interfejsu API i zachowania mogą spowodować przerwanie aplikacji utworzonych dla EF Core 2.2. x podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="64279-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="64279-105">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="64279-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="64279-106">Przerwy w nowych funkcjach wprowadzonych z jednej wersji zapoznawczej 3,0 do innej wersji zapoznawczej 3,0 nie są tutaj udokumentowane.</span><span class="sxs-lookup"><span data-stu-id="64279-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="64279-107">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="64279-107">Summary</span></span>

| <span data-ttu-id="64279-108">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="64279-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="64279-109">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="64279-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="64279-110">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="64279-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="64279-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="64279-111">High</span></span>       |
| [<span data-ttu-id="64279-112">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="64279-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="64279-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="64279-113">High</span></span>      |
| [<span data-ttu-id="64279-114">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="64279-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="64279-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="64279-115">High</span></span>      |
| [<span data-ttu-id="64279-116">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="64279-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="64279-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="64279-117">High</span></span>      |
| [<span data-ttu-id="64279-118">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="64279-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="64279-119">Wysoka</span><span class="sxs-lookup"><span data-stu-id="64279-119">High</span></span>      |
| [<span data-ttu-id="64279-120">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="64279-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="64279-121">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-121">Medium</span></span>      |
| [<span data-ttu-id="64279-122">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="64279-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="64279-123">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-123">Medium</span></span>      |
| [<span data-ttu-id="64279-124">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="64279-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="64279-125">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-125">Medium</span></span>      |
| [<span data-ttu-id="64279-126">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="64279-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="64279-127">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-127">Medium</span></span>      |
| [<span data-ttu-id="64279-128">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="64279-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="64279-129">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-129">Medium</span></span>      |
| [<span data-ttu-id="64279-130">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="64279-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="64279-131">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-131">Medium</span></span>      |
| [<span data-ttu-id="64279-132">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="64279-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="64279-133">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-133">Medium</span></span>      |
| [<span data-ttu-id="64279-134">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="64279-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="64279-135">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-135">Medium</span></span>      |
| [<span data-ttu-id="64279-136">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="64279-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="64279-137">Średni</span><span class="sxs-lookup"><span data-stu-id="64279-137">Medium</span></span>      |
| [<span data-ttu-id="64279-138">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="64279-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="64279-139">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-139">Low</span></span>      |
| [<span data-ttu-id="64279-140">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="64279-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="64279-141">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-141">Low</span></span>      |
| [<span data-ttu-id="64279-142">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="64279-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="64279-143">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-143">Low</span></span>      |
| [<span data-ttu-id="64279-144">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="64279-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="64279-145">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-145">Low</span></span>      |
| [<span data-ttu-id="64279-146">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="64279-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="64279-147">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-147">Low</span></span>      |
| [<span data-ttu-id="64279-148">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="64279-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="64279-149">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-149">Low</span></span>      |
| [<span data-ttu-id="64279-150">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="64279-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="64279-151">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-151">Low</span></span>      |
| [<span data-ttu-id="64279-152">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="64279-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="64279-153">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-153">Low</span></span>      |
| [<span data-ttu-id="64279-154">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="64279-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="64279-155">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-155">Low</span></span>      |
| [<span data-ttu-id="64279-156">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="64279-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="64279-157">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-157">Low</span></span>      |
| [<span data-ttu-id="64279-158">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="64279-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="64279-159">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-159">Low</span></span>      |
| [<span data-ttu-id="64279-160">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="64279-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="64279-161">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-161">Low</span></span>      |
| [<span data-ttu-id="64279-162">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="64279-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="64279-163">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-163">Low</span></span>      |
| [<span data-ttu-id="64279-164">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="64279-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="64279-165">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-165">Low</span></span>      |
| [<span data-ttu-id="64279-166">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="64279-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="64279-167">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-167">Low</span></span>      |
| [<span data-ttu-id="64279-168">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="64279-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="64279-169">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-169">Low</span></span>      |
| [<span data-ttu-id="64279-170">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="64279-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="64279-171">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-171">Low</span></span>      |
| [<span data-ttu-id="64279-172">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="64279-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="64279-173">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-173">Low</span></span>      |
| [<span data-ttu-id="64279-174">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="64279-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="64279-175">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-175">Low</span></span>      |
| [<span data-ttu-id="64279-176">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="64279-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="64279-177">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-177">Low</span></span>      |
| [<span data-ttu-id="64279-178">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="64279-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="64279-179">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-179">Low</span></span>      |
| [<span data-ttu-id="64279-180">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="64279-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="64279-181">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-181">Low</span></span>      |
| [<span data-ttu-id="64279-182">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="64279-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="64279-183">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-183">Low</span></span>      |
| [<span data-ttu-id="64279-184">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="64279-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="64279-185">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-185">Low</span></span>      |
| [<span data-ttu-id="64279-186">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="64279-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="64279-187">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-187">Low</span></span>      |
| [<span data-ttu-id="64279-188">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="64279-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="64279-189">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-189">Low</span></span>      |
| [<span data-ttu-id="64279-190">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="64279-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="64279-191">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-191">Low</span></span>      |
| [<span data-ttu-id="64279-192">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="64279-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="64279-193">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-193">Low</span></span>      |
| [<span data-ttu-id="64279-194">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="64279-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="64279-195">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-195">Low</span></span>      |
| [<span data-ttu-id="64279-196">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="64279-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="64279-197">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-197">Low</span></span>      |
| [<span data-ttu-id="64279-198">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="64279-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="64279-199">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-199">Low</span></span>      |
| [<span data-ttu-id="64279-200">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="64279-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="64279-201">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-201">Low</span></span>      |
| [<span data-ttu-id="64279-202">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="64279-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="64279-203">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-203">Low</span></span>      |
| [<span data-ttu-id="64279-204">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="64279-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="64279-205">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-205">Low</span></span>      |
| [<span data-ttu-id="64279-206">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="64279-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="64279-207">Małe</span><span class="sxs-lookup"><span data-stu-id="64279-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="64279-208">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="64279-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="64279-209">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="64279-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="64279-210">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-211">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-211">**Old behavior**</span></span>

<span data-ttu-id="64279-212">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="64279-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="64279-213">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="64279-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="64279-214">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-214">**New behavior**</span></span>

<span data-ttu-id="64279-215">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu ( `Select()` ostatnie wywołanie zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="64279-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="64279-216">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="64279-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="64279-217">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-217">**Why**</span></span>

<span data-ttu-id="64279-218">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="64279-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="64279-219">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="64279-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="64279-220">Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych, oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="64279-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="64279-221">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="64279-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="64279-222">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="64279-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="64279-223">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="64279-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="64279-224">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-224">**Mitigations**</span></span>

<span data-ttu-id="64279-225">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć lub użyć `AsEnumerable()`, `ToList()`lub podobnie jak jawnie przenieść dane z powrotem do klienta, na którym można następnie przetworzyć je za pomocą LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="64279-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="64279-226">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="64279-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="64279-227">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="64279-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="64279-228">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-229">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-229">**Old behavior**</span></span>

<span data-ttu-id="64279-230">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="64279-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="64279-231">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-231">**New behavior**</span></span>

<span data-ttu-id="64279-232">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="64279-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="64279-233">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="64279-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="64279-234">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-234">**Why**</span></span>

<span data-ttu-id="64279-235">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="64279-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="64279-236">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-236">**Mitigations**</span></span>

<span data-ttu-id="64279-237">Rozważ przeniesienie do nowoczesnej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="64279-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="64279-238">Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="64279-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="64279-239">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="64279-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="64279-240">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="64279-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="64279-241">Ta zmiana została wprowadzona w ASP.NET Core 3,0 — wersja zapoznawcza 1.</span><span class="sxs-lookup"><span data-stu-id="64279-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="64279-242">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-242">**Old behavior**</span></span>

<span data-ttu-id="64279-243">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, będzie ona obejmować EF Core i niektórych dostawców danych EF Core, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64279-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="64279-244">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-244">**New behavior**</span></span>

<span data-ttu-id="64279-245">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="64279-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="64279-246">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-246">**Why**</span></span>

<span data-ttu-id="64279-247">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="64279-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="64279-248">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="64279-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="64279-249">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64279-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="64279-250">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="64279-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="64279-251">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-251">**Mitigations**</span></span>

<span data-ttu-id="64279-252">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="64279-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="64279-253">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="64279-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="64279-254">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="64279-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="64279-255">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4 i odpowiadająca jej wersja zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="64279-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="64279-256">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-256">**Old behavior**</span></span>

<span data-ttu-id="64279-257">Przed 3,0 `dotnet ef` narzędzie zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="64279-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="64279-258">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-258">**New behavior**</span></span>

<span data-ttu-id="64279-259">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera `dotnet ef` narzędzia, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="64279-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="64279-260">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-260">**Why**</span></span>

<span data-ttu-id="64279-261">Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="64279-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="64279-262">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-262">**Mitigations**</span></span>

<span data-ttu-id="64279-263">Aby móc zarządzać migracjami lub szkieletem a `DbContext`, zainstaluj `dotnet-ef` program jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="64279-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="64279-264">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="64279-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="64279-265">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="64279-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="64279-266">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="64279-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="64279-267">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-268">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-268">**Old behavior**</span></span>

<span data-ttu-id="64279-269">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="64279-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="64279-270">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-270">**New behavior**</span></span>

<span data-ttu-id="64279-271">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="64279-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="64279-272">Przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="64279-273">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i`ExecuteSqlInterpolatedAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przenoszone w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="64279-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="64279-274">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="64279-275">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="64279-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="64279-276">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-276">**Why**</span></span>

<span data-ttu-id="64279-277">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="64279-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="64279-278">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="64279-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="64279-279">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-279">**Mitigations**</span></span>

<span data-ttu-id="64279-280">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="64279-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="64279-281">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="64279-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="64279-282">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="64279-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="64279-283">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="64279-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="64279-284">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-284">**Old behavior**</span></span>

<span data-ttu-id="64279-285">Przed EF Core 3,0, `FromSql` Metoda może być określona w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="64279-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="64279-286">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-286">**New behavior**</span></span>

<span data-ttu-id="64279-287">Począwszy od EF Core 3,0, `FromSqlRaw` nowe i `FromSqlInterpolated` metody (zastępujące `FromSql`) można określić tylko dla katalogów głównych `DbSet<>`zapytań, tj. bezpośrednio na.</span><span class="sxs-lookup"><span data-stu-id="64279-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="64279-288">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="64279-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="64279-289">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-289">**Why**</span></span>

<span data-ttu-id="64279-290">Określenie `FromSql` dowolnego miejsca, w którym `DbSet` nie ma żadnego dodanego znaczenia ani dodanej wartości, może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="64279-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="64279-291">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-291">**Mitigations**</span></span>

<span data-ttu-id="64279-292">`FromSql`wywołania należy przenieść, aby znajdowały się bezpośrednio w, `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="64279-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="64279-293">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="64279-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="64279-294">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="64279-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="64279-295">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="64279-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="64279-296">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-296">**Old behavior**</span></span>

<span data-ttu-id="64279-297">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="64279-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="64279-298">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="64279-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="64279-299">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="64279-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="64279-300">zwróci to samo `Category` wystąpienie dla każdego `Product` , które jest skojarzone z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="64279-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="64279-301">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-301">**New behavior**</span></span>

<span data-ttu-id="64279-302">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="64279-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="64279-303">Na przykład zapytanie powyżej zwróci teraz nowe `Category` wystąpienie dla każdego z nich `Product` , nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="64279-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="64279-304">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-304">**Why**</span></span>

<span data-ttu-id="64279-305">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="64279-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="64279-306">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="64279-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="64279-307">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="64279-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="64279-308">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-308">**Mitigations**</span></span>

<span data-ttu-id="64279-309">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="64279-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="64279-310">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="64279-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="64279-311">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="64279-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="64279-312">Ta zmiana jest przywracana w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-313">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="64279-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="64279-314">Na przykład, aby przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="64279-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="64279-315">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="64279-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="64279-316">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="64279-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="64279-317">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="64279-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="64279-318">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-318">**Old behavior**</span></span>

<span data-ttu-id="64279-319">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="64279-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="64279-320">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="64279-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="64279-321">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-321">**New behavior**</span></span>

<span data-ttu-id="64279-322">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="64279-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="64279-323">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-323">**Why**</span></span>

<span data-ttu-id="64279-324">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienie, jest przenoszona do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="64279-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="64279-325">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-325">**Mitigations**</span></span>

<span data-ttu-id="64279-326">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i `Added` należą do jednostek w stanie.</span><span class="sxs-lookup"><span data-stu-id="64279-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="64279-327">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="64279-327">This can be avoided by:</span></span>
* <span data-ttu-id="64279-328">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="64279-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="64279-329">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="64279-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="64279-330">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="64279-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="64279-331">Na przykład zwróci `context.Entry(blog).Property(e => e.Id).CurrentValue` wartość tymczasową nawet wtedy, gdy `blog.Id` sama nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="64279-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="64279-332">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="64279-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="64279-333">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="64279-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="64279-334">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-335">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-335">**Old behavior**</span></span>

<span data-ttu-id="64279-336">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` zostałaby śledzona `Added` w stanie i wstawiona jako nowy wiersz, gdy `SaveChanges` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="64279-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="64279-337">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-337">**New behavior**</span></span>

<span data-ttu-id="64279-338">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w `Modified` stanie.</span><span class="sxs-lookup"><span data-stu-id="64279-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="64279-339">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po `SaveChanges` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="64279-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="64279-340">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona tak `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="64279-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="64279-341">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-341">**Why**</span></span>

<span data-ttu-id="64279-342">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="64279-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="64279-343">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-343">**Mitigations**</span></span>

<span data-ttu-id="64279-344">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="64279-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="64279-345">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="64279-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="64279-346">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="64279-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="64279-347">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="64279-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="64279-348">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="64279-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="64279-349">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="64279-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="64279-350">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-351">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-351">**Old behavior**</span></span>

<span data-ttu-id="64279-352">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="64279-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="64279-353">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-353">**New behavior**</span></span>

<span data-ttu-id="64279-354">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="64279-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="64279-355">Na przykład wywołanie `context.Remove()` usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane zależności są również ustawione na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="64279-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="64279-356">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-356">**Why**</span></span>

<span data-ttu-id="64279-357">Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="64279-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="64279-358">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-358">**Mitigations**</span></span>

<span data-ttu-id="64279-359">Poprzednie zachowanie można przywrócić za pomocą ustawień na stronie `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="64279-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="64279-360">Przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="64279-361">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="64279-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="64279-362">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="64279-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="64279-363">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 5.</span><span class="sxs-lookup"><span data-stu-id="64279-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="64279-364">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-364">**Old behavior**</span></span>

<span data-ttu-id="64279-365">Przed 3,0, `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z `Restrict` semantyką, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.</span><span class="sxs-lookup"><span data-stu-id="64279-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="64279-366">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-366">**New behavior**</span></span>

<span data-ttu-id="64279-367">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone `Restrict` z semantyką--to nie, bez kaskad; throw przy naruszeniu ograniczenia — bez również wpływu na wewnętrzną naprawę EF.</span><span class="sxs-lookup"><span data-stu-id="64279-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="64279-368">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-368">**Why**</span></span>

<span data-ttu-id="64279-369">Ta zmiana została wprowadzona w celu usprawnienia korzystania `DeleteBehavior` z programu w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="64279-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="64279-370">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-370">**Mitigations**</span></span>

<span data-ttu-id="64279-371">Poprzednie zachowanie można przywrócić za pomocą polecenia `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="64279-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="64279-372">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="64279-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="64279-373">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="64279-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="64279-374">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-375">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-375">**Old behavior**</span></span>

<span data-ttu-id="64279-376">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/query-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="64279-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="64279-377">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="64279-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="64279-378">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-378">**New behavior**</span></span>

<span data-ttu-id="64279-379">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="64279-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="64279-380">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="64279-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="64279-381">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-381">**Why**</span></span>

<span data-ttu-id="64279-382">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="64279-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="64279-383">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="64279-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="64279-384">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="64279-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="64279-385">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-385">**Mitigations**</span></span>

<span data-ttu-id="64279-386">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="64279-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="64279-387">**`ModelBuilder.Query<>()`** — Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="64279-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="64279-388">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="64279-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="64279-389">**`DbQuery<>`** — Zamiast `DbSet<>` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="64279-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="64279-390">**`DbContext.Query<>()`** — Zamiast `DbContext.Set<>()` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="64279-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="64279-391">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="64279-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="64279-392">[Śledzenie](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
problemów ze śledzeniem #12444[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
śledzenia problemów[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="64279-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="64279-393">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-394">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-394">**Old behavior**</span></span>

<span data-ttu-id="64279-395">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po `OwnsOne` wywołaniu lub. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="64279-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="64279-396">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-396">**New behavior**</span></span>

<span data-ttu-id="64279-397">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="64279-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="64279-398">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="64279-399">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem `WithOwner()` po podobnym sposobie, jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="64279-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="64279-400">Mimo że konfiguracja dla samego samego typu jest nadal łańcuchem `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="64279-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="64279-401">Przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-401">For example:</span></span>

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

<span data-ttu-id="64279-402">Dodatkowo wywoływanie `Entity()`,, `Set()` lub z obiektem docelowym typu, `HasOne()`spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="64279-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="64279-403">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-403">**Why**</span></span>

<span data-ttu-id="64279-404">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="64279-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="64279-405">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, `HasForeignKey`takich jak.</span><span class="sxs-lookup"><span data-stu-id="64279-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="64279-406">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-406">**Mitigations**</span></span>

<span data-ttu-id="64279-407">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="64279-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="64279-408">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="64279-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="64279-409">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="64279-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="64279-410">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-411">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-411">**Old behavior**</span></span>

<span data-ttu-id="64279-412">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="64279-412">Consider the following model:</span></span>
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
<span data-ttu-id="64279-413">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli `OrderDetails` , wystąpienie jest zawsze wymagane podczas dodawania nowego `Order`elementu.</span><span class="sxs-lookup"><span data-stu-id="64279-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="64279-414">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-414">**New behavior**</span></span>

<span data-ttu-id="64279-415">Począwszy od 3,0, EF Core umożliwia dodanie `Order` , `OrderDetails` bez `OrderDetails` i mapuje wszystkie właściwości, z wyjątkiem klucza podstawowego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="64279-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="64279-416">Podczas wykonywania zapytania o zestawy `OrderDetails` EF Core `null` do, jeśli którekolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="64279-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="64279-417">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-417">**Mitigations**</span></span>

<span data-ttu-id="64279-418">Jeśli model ma zależeć do udostępniania tabeli dla wszystkich opcjonalnych kolumn, ale Nawigacja wskazująca, że nie jest oczekiwana `null` , aplikacja powinna zostać zmodyfikowana, aby obsługiwać przypadki, gdy Nawigacja `null`jest.</span><span class="sxs-lookup"><span data-stu-id="64279-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="64279-419">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć`null` przypisaną do niej inną wartość.</span><span class="sxs-lookup"><span data-stu-id="64279-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="64279-420">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="64279-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="64279-421">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="64279-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="64279-422">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-423">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-423">**Old behavior**</span></span>

<span data-ttu-id="64279-424">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="64279-424">Consider the following model:</span></span>
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
<span data-ttu-id="64279-425">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="64279-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="64279-426">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-426">**New behavior**</span></span>

<span data-ttu-id="64279-427">Począwszy od 3,0, EF Core propaguje nową `Version` wartość do `Order` , jeśli jest właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="64279-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="64279-428">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="64279-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="64279-429">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-429">**Why**</span></span>

<span data-ttu-id="64279-430">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="64279-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="64279-431">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-431">**Mitigations**</span></span>

<span data-ttu-id="64279-432">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="64279-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="64279-433">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="64279-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="64279-434">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="64279-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="64279-435">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="64279-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="64279-436">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-437">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-437">**Old behavior**</span></span>

<span data-ttu-id="64279-438">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="64279-438">Consider the following model:</span></span>
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

<span data-ttu-id="64279-439">Przed EF Core 3,0 `ShippingAddress` Właściwość zostałaby zamapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="64279-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="64279-440">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-440">**New behavior**</span></span>

<span data-ttu-id="64279-441">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="64279-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="64279-442">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-442">**Why**</span></span>

<span data-ttu-id="64279-443">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="64279-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="64279-444">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-444">**Mitigations**</span></span>

<span data-ttu-id="64279-445">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="64279-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="64279-446">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="64279-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="64279-447">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="64279-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="64279-448">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-449">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-449">**Old behavior**</span></span>

<span data-ttu-id="64279-450">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="64279-450">Consider the following model:</span></span>
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
<span data-ttu-id="64279-451">Przed EF Core 3,0, `CustomerId` właściwość będzie używana dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="64279-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="64279-452">Jeśli `Order` jednak jest typem będącym własnością, to `CustomerId` również klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="64279-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="64279-453">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-453">**New behavior**</span></span>

<span data-ttu-id="64279-454">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="64279-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="64279-455">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="64279-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="64279-456">Przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-456">For example:</span></span>

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

<span data-ttu-id="64279-457">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-457">**Why**</span></span>

<span data-ttu-id="64279-458">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="64279-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="64279-459">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-459">**Mitigations**</span></span>

<span data-ttu-id="64279-460">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="64279-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="64279-461">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="64279-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="64279-462">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="64279-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="64279-463">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-464">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-464">**Old behavior**</span></span>

<span data-ttu-id="64279-465">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="64279-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="64279-466">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-466">**New behavior**</span></span>

<span data-ttu-id="64279-467">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="64279-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="64279-468">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-468">**Why**</span></span>

<span data-ttu-id="64279-469">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="64279-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="64279-470">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="64279-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="64279-471">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-471">**Mitigations**</span></span>

<span data-ttu-id="64279-472">Jeśli połączenie musi pozostać otwarte jawne wywołanie w celu `OpenConnection()` upewnienia się, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="64279-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="64279-473">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="64279-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="64279-474">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="64279-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="64279-475">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-476">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-476">**Old behavior**</span></span>

<span data-ttu-id="64279-477">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="64279-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="64279-478">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-478">**New behavior**</span></span>

<span data-ttu-id="64279-479">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="64279-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="64279-480">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="64279-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="64279-481">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-481">**Why**</span></span>

<span data-ttu-id="64279-482">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="64279-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="64279-483">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-483">**Mitigations**</span></span>

<span data-ttu-id="64279-484">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="64279-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="64279-485">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="64279-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="64279-486">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="64279-486">Backing fields are used by default</span></span>

[<span data-ttu-id="64279-487">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="64279-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="64279-488">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="64279-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="64279-489">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-489">**Old behavior**</span></span>

<span data-ttu-id="64279-490">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="64279-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="64279-491">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="64279-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="64279-492">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-492">**New behavior**</span></span>

<span data-ttu-id="64279-493">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="64279-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="64279-494">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="64279-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="64279-495">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-495">**Why**</span></span>

<span data-ttu-id="64279-496">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="64279-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="64279-497">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-497">**Mitigations**</span></span>

<span data-ttu-id="64279-498">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu `ModelBuilder`dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="64279-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="64279-499">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="64279-500">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="64279-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="64279-501">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="64279-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="64279-502">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-503">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-503">**Old behavior**</span></span>

<span data-ttu-id="64279-504">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="64279-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="64279-505">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="64279-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="64279-506">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-506">**New behavior**</span></span>

<span data-ttu-id="64279-507">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="64279-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="64279-508">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-508">**Why**</span></span>

<span data-ttu-id="64279-509">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="64279-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="64279-510">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-510">**Mitigations**</span></span>

<span data-ttu-id="64279-511">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="64279-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="64279-512">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="64279-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="64279-513">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="64279-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="64279-514">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-515">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-515">**Old behavior**</span></span>

<span data-ttu-id="64279-516">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie CLR, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="64279-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="64279-517">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-517">**New behavior**</span></span>

<span data-ttu-id="64279-518">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="64279-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="64279-519">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-519">**Why**</span></span>

<span data-ttu-id="64279-520">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="64279-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="64279-521">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-521">**Mitigations**</span></span>

<span data-ttu-id="64279-522">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="64279-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="64279-523">W nowszej wersji zapoznawczej EF Core 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która różni się od nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="64279-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="64279-524">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="64279-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="64279-525">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="64279-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="64279-526">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-527">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-527">**Old behavior**</span></span>

<span data-ttu-id="64279-528">Przed EF Core 3,0 `AddDbContext` , wywoływanie `AddDbContextPool` lub zarejestrowanie usługi rejestrowania i pamięci podręcznej w usłudze D. I przez wywołania funkcji addlogging i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache). [](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging)</span><span class="sxs-lookup"><span data-stu-id="64279-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="64279-529">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-529">**New behavior**</span></span>

<span data-ttu-id="64279-530">Począwszy od EF Core 3,0 `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="64279-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="64279-531">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-531">**Why**</span></span>

<span data-ttu-id="64279-532">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64279-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="64279-533">Jeśli `ILoggerFactory` jednak program jest zarejestrowany w kontenerze di aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="64279-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="64279-534">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-534">**Mitigations**</span></span>

<span data-ttu-id="64279-535">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji addlogging lub [](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="64279-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="64279-536">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="64279-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="64279-537">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="64279-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="64279-538">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-539">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-539">**Old behavior**</span></span>

<span data-ttu-id="64279-540">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="64279-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="64279-541">W ten sposób zapewniono, że stan ujawniony na `EntityEntry` bieżąco.</span><span class="sxs-lookup"><span data-stu-id="64279-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="64279-542">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-542">**New behavior**</span></span>

<span data-ttu-id="64279-543">Począwszy od EF Core 3,0, wywołanie `DbContext.Entry` będzie teraz podejmować próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="64279-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="64279-544">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64279-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="64279-545">Należy pamiętać, `ChangeTracker.AutoDetectChangesEnabled` że jeśli jest `false` ustawiona na, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="64279-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="64279-546">Inne metody, które powodują wykrycie zmiany — na `ChangeTracker.Entries` przykład `SaveChanges`i--nadal powodują pełne `DetectChanges` wszystkie śledzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="64279-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="64279-547">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-547">**Why**</span></span>

<span data-ttu-id="64279-548">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania `context.Entry`z programu.</span><span class="sxs-lookup"><span data-stu-id="64279-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="64279-549">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-549">**Mitigations**</span></span>

<span data-ttu-id="64279-550">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed `Entry` wywołaniem, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="64279-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="64279-551">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="64279-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="64279-552">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="64279-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="64279-553">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-554">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-554">**Old behavior**</span></span>

<span data-ttu-id="64279-555">Przed EF Core 3,0 `string` i `byte[]` właściwości klucza mogą być używane bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="64279-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="64279-556">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, Zserializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="64279-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="64279-557">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-557">**New behavior**</span></span>

<span data-ttu-id="64279-558">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="64279-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="64279-559">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-559">**Why**</span></span>

<span data-ttu-id="64279-560">Ta zmiana została wprowadzona, ponieważ zazwyczaj wartości `string` generowane / `byte[]` przez klienta nie są przydatne, a domyślne zachowanie spowodowało trudne powody dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="64279-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="64279-561">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-561">**Mitigations**</span></span>

<span data-ttu-id="64279-562">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="64279-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="64279-563">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="64279-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="64279-564">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="64279-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="64279-565">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="64279-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="64279-566">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="64279-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="64279-567">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-568">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-568">**Old behavior**</span></span>

<span data-ttu-id="64279-569">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="64279-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="64279-570">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-570">**New behavior**</span></span>

<span data-ttu-id="64279-571">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="64279-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="64279-572">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-572">**Why**</span></span>

<span data-ttu-id="64279-573">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z `DbContext` wystąpieniem, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="64279-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="64279-574">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-574">**Mitigations**</span></span>

<span data-ttu-id="64279-575">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="64279-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="64279-576">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="64279-576">This isn't common.</span></span>
<span data-ttu-id="64279-577">W takich przypadkach większość elementów będzie nadal działać, ale wszystkie pojedyncze usługi, w `ILoggerFactory` zależności od tego, muszą zostać zmienione w celu `ILoggerFactory` uzyskania w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="64279-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="64279-578">Jeśli używasz `ILoggerFactory` takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak można lepiej zrozumieć, jak nie należy ponownie rozbić w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="64279-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="64279-579">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="64279-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="64279-580">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="64279-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="64279-581">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-582">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-582">**Old behavior**</span></span>

<span data-ttu-id="64279-583">Przed EF Core 3,0, gdy zostanie `DbContext` usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="64279-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="64279-584">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="64279-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="64279-585">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="64279-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="64279-586">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-586">**New behavior**</span></span>

<span data-ttu-id="64279-587">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="64279-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="64279-588">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="64279-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="64279-589">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="64279-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="64279-590">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="64279-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="64279-591">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-591">**Why**</span></span>

<span data-ttu-id="64279-592">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie przy próbie załadowania `DbContext` opóźnionego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="64279-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="64279-593">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-593">**Mitigations**</span></span>

<span data-ttu-id="64279-594">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="64279-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="64279-595">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="64279-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="64279-596">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="64279-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="64279-597">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-598">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-598">**Old behavior**</span></span>

<span data-ttu-id="64279-599">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="64279-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="64279-600">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-600">**New behavior**</span></span>

<span data-ttu-id="64279-601">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="64279-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="64279-602">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-602">**Why**</span></span>

<span data-ttu-id="64279-603">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="64279-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="64279-604">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-604">**Mitigations**</span></span>

<span data-ttu-id="64279-605">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="64279-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="64279-606">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację w `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64279-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="64279-607">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="64279-608">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="64279-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="64279-609">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="64279-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="64279-610">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-611">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-611">**Old behavior**</span></span>

<span data-ttu-id="64279-612">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="64279-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="64279-613">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="64279-614">Kod wygląda tak `Samurai` , jakby odnosił się do innego typu jednostki `Entrance` przy użyciu właściwości nawigacji, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="64279-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="64279-615">W rzeczywistości ten kod próbuje utworzyć relację z typem obiektu o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="64279-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="64279-616">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-616">**New behavior**</span></span>

<span data-ttu-id="64279-617">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="64279-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="64279-618">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-618">**Why**</span></span>

<span data-ttu-id="64279-619">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="64279-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="64279-620">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-620">**Mitigations**</span></span>

<span data-ttu-id="64279-621">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="64279-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="64279-622">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="64279-622">This is not common.</span></span>
<span data-ttu-id="64279-623">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="64279-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="64279-624">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="64279-625">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="64279-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="64279-626">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="64279-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="64279-627">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-628">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-628">**Old behavior**</span></span>

<span data-ttu-id="64279-629">Następujące metody asynchroniczne zwróciły `Task<T>`wcześniej:</span><span class="sxs-lookup"><span data-stu-id="64279-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="64279-630">`ValueGenerator.NextValueAsync()`(i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="64279-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="64279-631">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-631">**New behavior**</span></span>

<span data-ttu-id="64279-632">Powyższe metody teraz zwracają wartość `ValueTask<T>` powyżej `T` , tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="64279-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="64279-633">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-633">**Why**</span></span>

<span data-ttu-id="64279-634">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="64279-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="64279-635">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-635">**Mitigations**</span></span>

<span data-ttu-id="64279-636">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="64279-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="64279-637">Bardziej złożone użycie (np. przekazanie zwróconej `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` przekonwertowanie na a `Task<T>` przez wywołanie `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="64279-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="64279-638">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="64279-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="64279-639">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="64279-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="64279-640">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="64279-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="64279-641">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="64279-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="64279-642">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-642">**Old behavior**</span></span>

<span data-ttu-id="64279-643">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="64279-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="64279-644">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-644">**New behavior**</span></span>

<span data-ttu-id="64279-645">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="64279-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="64279-646">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-646">**Why**</span></span>

<span data-ttu-id="64279-647">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="64279-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="64279-648">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-648">**Mitigations**</span></span>

<span data-ttu-id="64279-649">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="64279-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="64279-650">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="64279-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="64279-651">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="64279-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="64279-652">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="64279-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="64279-653">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-654">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-654">**Old behavior**</span></span>

<span data-ttu-id="64279-655">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="64279-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="64279-656">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-656">**New behavior**</span></span>

<span data-ttu-id="64279-657">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, wywołana `ToTable()` dla typu pochodnego będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="64279-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="64279-658">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-658">**Why**</span></span>

<span data-ttu-id="64279-659">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="64279-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="64279-660">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="64279-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="64279-661">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-661">**Mitigations**</span></span>

<span data-ttu-id="64279-662">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="64279-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="64279-663">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="64279-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="64279-664">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="64279-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="64279-665">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-666">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-666">**Old behavior**</span></span>

<span data-ttu-id="64279-667">Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnia sposób konfigurowania kolumn używanych w programie. `INCLUDE`</span><span class="sxs-lookup"><span data-stu-id="64279-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="64279-668">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-668">**New behavior**</span></span>

<span data-ttu-id="64279-669">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="64279-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="64279-670">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="64279-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="64279-671">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-671">**Why**</span></span>

<span data-ttu-id="64279-672">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="64279-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="64279-673">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-673">**Mitigations**</span></span>

<span data-ttu-id="64279-674">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="64279-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="64279-675">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="64279-675">Metadata API changes</span></span>

[<span data-ttu-id="64279-676">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="64279-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="64279-677">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-678">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-678">**New behavior**</span></span>

<span data-ttu-id="64279-679">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="64279-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="64279-680">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-680">**Why**</span></span>

<span data-ttu-id="64279-681">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="64279-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="64279-682">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-682">**Mitigations**</span></span>

<span data-ttu-id="64279-683">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="64279-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="64279-684">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="64279-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="64279-685">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="64279-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="64279-686">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="64279-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="64279-687">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-687">**New behavior**</span></span>

<span data-ttu-id="64279-688">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="64279-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="64279-689">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-689">**Why**</span></span>

<span data-ttu-id="64279-690">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="64279-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="64279-691">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-691">**Mitigations**</span></span>

<span data-ttu-id="64279-692">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="64279-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="64279-693">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="64279-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="64279-694">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="64279-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="64279-695">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="64279-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="64279-696">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-696">**Old behavior**</span></span>

<span data-ttu-id="64279-697">Przed EF Core 3,0, EF Core wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z systemem SQLite.</span><span class="sxs-lookup"><span data-stu-id="64279-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="64279-698">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-698">**New behavior**</span></span>

<span data-ttu-id="64279-699">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="64279-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="64279-700">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-700">**Why**</span></span>

<span data-ttu-id="64279-701">Ta zmiana została wprowadzona, ponieważ domyślnie `SQLitePCLRaw.bundle_e_sqlite3` używa EF Core, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="64279-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="64279-702">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-702">**Mitigations**</span></span>

<span data-ttu-id="64279-703">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="64279-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="64279-704">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="64279-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="64279-705">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="64279-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="64279-706">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-706">**Old behavior**</span></span>

<span data-ttu-id="64279-707">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="64279-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="64279-708">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-708">**New behavior**</span></span>

<span data-ttu-id="64279-709">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="64279-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="64279-710">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-710">**Why**</span></span>

<span data-ttu-id="64279-711">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="64279-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="64279-712">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-712">**Mitigations**</span></span>

<span data-ttu-id="64279-713">Aby korzystać z natywnej wersji programu SQLite w systemie `Microsoft.Data.Sqlite` iOS, należy skonfigurować `SQLitePCLRaw` do korzystania z innego pakietu.</span><span class="sxs-lookup"><span data-stu-id="64279-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="64279-714">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="64279-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="64279-715">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="64279-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="64279-716">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-717">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-717">**Old behavior**</span></span>

<span data-ttu-id="64279-718">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="64279-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="64279-719">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-719">**New behavior**</span></span>

<span data-ttu-id="64279-720">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="64279-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="64279-721">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-721">**Why**</span></span>

<span data-ttu-id="64279-722">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="64279-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="64279-723">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="64279-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="64279-724">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-724">**Mitigations**</span></span>

<span data-ttu-id="64279-725">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="64279-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="64279-726">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="64279-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="64279-727">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="64279-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="64279-728">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="64279-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="64279-729">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="64279-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="64279-730">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-731">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-731">**Old behavior**</span></span>

<span data-ttu-id="64279-732">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="64279-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="64279-733">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="64279-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="64279-734">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-734">**New behavior**</span></span>

<span data-ttu-id="64279-735">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="64279-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="64279-736">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-736">**Why**</span></span>

<span data-ttu-id="64279-737">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="64279-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="64279-738">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-738">**Mitigations**</span></span>

<span data-ttu-id="64279-739">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="64279-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="64279-740">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="64279-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="64279-741">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="64279-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="64279-742">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="64279-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="64279-743">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="64279-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="64279-744">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-745">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-745">**Old behavior**</span></span>

<span data-ttu-id="64279-746">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="64279-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="64279-747">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-747">**New behavior**</span></span>

<span data-ttu-id="64279-748">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="64279-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="64279-749">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-749">**Why**</span></span>

<span data-ttu-id="64279-750">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="64279-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="64279-751">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="64279-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="64279-752">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-752">**Mitigations**</span></span>

<span data-ttu-id="64279-753">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="64279-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="64279-754">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="64279-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="64279-755">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="64279-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="64279-756">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="64279-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="64279-757">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="64279-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="64279-758">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="64279-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="64279-759">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="64279-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="64279-760">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-760">**Old behavior**</span></span>

<span data-ttu-id="64279-761">Przed EF Core 3,0 `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="64279-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="64279-762">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-762">**New behavior**</span></span>

<span data-ttu-id="64279-763">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64279-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="64279-764">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-764">**Why**</span></span>

<span data-ttu-id="64279-765">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="64279-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="64279-766">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-766">**Mitigations**</span></span>

<span data-ttu-id="64279-767">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="64279-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="64279-768">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze śledzeniem ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="64279-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="64279-769">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="64279-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="64279-770">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="64279-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="64279-771">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="64279-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="64279-772">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-773">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-773">**Old behavior**</span></span>

<span data-ttu-id="64279-774">`IDbContextOptionsExtension`zawiera metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="64279-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="64279-775">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-775">**New behavior**</span></span>

<span data-ttu-id="64279-776">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="64279-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="64279-777">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-777">**Why**</span></span>

<span data-ttu-id="64279-778">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="64279-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="64279-779">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="64279-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="64279-780">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-780">**Mitigations**</span></span>

<span data-ttu-id="64279-781">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="64279-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="64279-782">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="64279-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="64279-783">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="64279-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="64279-784">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="64279-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="64279-785">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-786">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="64279-786">**Change**</span></span>

<span data-ttu-id="64279-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="64279-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="64279-788">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-788">**Why**</span></span>

<span data-ttu-id="64279-789">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="64279-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="64279-790">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-790">**Mitigations**</span></span>

<span data-ttu-id="64279-791">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="64279-791">Use the new name.</span></span> <span data-ttu-id="64279-792">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="64279-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="64279-793">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="64279-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="64279-794">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="64279-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="64279-795">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-796">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-796">**Old behavior**</span></span>

<span data-ttu-id="64279-797">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="64279-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="64279-798">Przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="64279-799">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-799">**New behavior**</span></span>

<span data-ttu-id="64279-800">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="64279-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="64279-801">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="64279-802">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-802">**Why**</span></span>

<span data-ttu-id="64279-803">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="64279-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="64279-804">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-804">**Mitigations**</span></span>

<span data-ttu-id="64279-805">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="64279-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="64279-806">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="64279-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="64279-807">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="64279-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="64279-808">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-809">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-809">**Old behavior**</span></span>

<span data-ttu-id="64279-810">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="64279-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="64279-811">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-811">**New behavior**</span></span>

<span data-ttu-id="64279-812">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="64279-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="64279-813">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-813">**Why**</span></span>

<span data-ttu-id="64279-814">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="64279-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="64279-815">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="64279-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="64279-816">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-816">**Mitigations**</span></span>

<span data-ttu-id="64279-817">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="64279-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="64279-818">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="64279-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="64279-819">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="64279-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="64279-820">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="64279-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="64279-821">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-821">**Old behavior**</span></span>

<span data-ttu-id="64279-822">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="64279-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="64279-823">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-823">**New behavior**</span></span>

<span data-ttu-id="64279-824">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="64279-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="64279-825">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="64279-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="64279-826">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-826">**Why**</span></span>

<span data-ttu-id="64279-827">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="64279-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="64279-828">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="64279-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="64279-829">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="64279-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="64279-830">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-830">**Mitigations**</span></span>

<span data-ttu-id="64279-831">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="64279-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="64279-832">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="64279-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="64279-833">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="64279-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="64279-834">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="64279-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="64279-835">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-836">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-836">**Old behavior**</span></span>

<span data-ttu-id="64279-837">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="64279-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="64279-838">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-838">**New behavior**</span></span>

<span data-ttu-id="64279-839">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="64279-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="64279-840">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-840">**Why**</span></span>

<span data-ttu-id="64279-841">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="64279-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="64279-842">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="64279-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="64279-843">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-843">**Mitigations**</span></span>

<span data-ttu-id="64279-844">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="64279-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="64279-845">Szczegółowe informacje można znaleźć w informacjach o [wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="64279-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="64279-846">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="64279-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="64279-847">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="64279-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="64279-848">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="64279-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="64279-849">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-849">**Old behavior**</span></span>

<span data-ttu-id="64279-850">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="64279-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="64279-851">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-851">**New behavior**</span></span>

<span data-ttu-id="64279-852">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="64279-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="64279-853">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-853">**Why**</span></span>

<span data-ttu-id="64279-854">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="64279-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="64279-855">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-855">**Mitigations**</span></span>

<span data-ttu-id="64279-856">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="64279-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="64279-857">Szczegółowe informacje można znaleźć w informacjach o [wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="64279-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="64279-858">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="64279-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="64279-859">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="64279-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="64279-860">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="64279-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="64279-861">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-861">**Old behavior**</span></span>

<span data-ttu-id="64279-862">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="64279-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="64279-863">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-863">For example:</span></span>

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

<span data-ttu-id="64279-864">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="64279-864">**New behavior**</span></span>

<span data-ttu-id="64279-865">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="64279-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="64279-866">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="64279-866">**Why**</span></span>

<span data-ttu-id="64279-867">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="64279-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="64279-868">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="64279-868">**Mitigations**</span></span>

<span data-ttu-id="64279-869">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="64279-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="64279-870">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="64279-870">For example:</span></span>

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
