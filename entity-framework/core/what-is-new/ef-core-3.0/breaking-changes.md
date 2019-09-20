---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 1f63593631017a61c39ccab9216adbc4663700e7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148904"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="3b40b-102">Istotne zmiany zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="3b40b-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="3b40b-103">Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b40b-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="3b40b-104">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="3b40b-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="3b40b-105">Przerwy z jednej wersji zapoznawczej 3,0 do innej wersji zapoznawczej 3,0 nie są tutaj udokumentowane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="3b40b-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="3b40b-106">Summary</span></span>

| <span data-ttu-id="3b40b-107">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="3b40b-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="3b40b-108">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="3b40b-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="3b40b-109">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="3b40b-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="3b40b-110">Wysoka</span><span class="sxs-lookup"><span data-stu-id="3b40b-110">High</span></span>       |
| [<span data-ttu-id="3b40b-111">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="3b40b-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="3b40b-112">Wysoka</span><span class="sxs-lookup"><span data-stu-id="3b40b-112">High</span></span>      |
| [<span data-ttu-id="3b40b-113">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3b40b-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="3b40b-114">Wysoka</span><span class="sxs-lookup"><span data-stu-id="3b40b-114">High</span></span>      |
| [<span data-ttu-id="3b40b-115">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="3b40b-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="3b40b-116">Wysoka</span><span class="sxs-lookup"><span data-stu-id="3b40b-116">High</span></span>      |
| [<span data-ttu-id="3b40b-117">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="3b40b-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="3b40b-118">Wysoka</span><span class="sxs-lookup"><span data-stu-id="3b40b-118">High</span></span>      |
| [<span data-ttu-id="3b40b-119">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="3b40b-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="3b40b-120">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-120">Medium</span></span>      |
| [<span data-ttu-id="3b40b-121">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="3b40b-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="3b40b-122">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-122">Medium</span></span>      |
| [<span data-ttu-id="3b40b-123">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="3b40b-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="3b40b-124">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-124">Medium</span></span>      |
| [<span data-ttu-id="3b40b-125">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="3b40b-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="3b40b-126">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-126">Medium</span></span>      |
| [<span data-ttu-id="3b40b-127">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="3b40b-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="3b40b-128">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-128">Medium</span></span>      |
| [<span data-ttu-id="3b40b-129">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="3b40b-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="3b40b-130">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-130">Medium</span></span>      |
| [<span data-ttu-id="3b40b-131">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="3b40b-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="3b40b-132">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-132">Medium</span></span>      |
| [<span data-ttu-id="3b40b-133">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="3b40b-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="3b40b-134">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-134">Medium</span></span>      |
| [<span data-ttu-id="3b40b-135">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="3b40b-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="3b40b-136">Średni</span><span class="sxs-lookup"><span data-stu-id="3b40b-136">Medium</span></span>      |
| [<span data-ttu-id="3b40b-137">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="3b40b-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="3b40b-138">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-138">Low</span></span>      |
| [<span data-ttu-id="3b40b-139">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="3b40b-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="3b40b-140">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-140">Low</span></span>      |
| [<span data-ttu-id="3b40b-141">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="3b40b-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="3b40b-142">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-142">Low</span></span>      |
| [<span data-ttu-id="3b40b-143">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="3b40b-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="3b40b-144">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-144">Low</span></span>      |
| [<span data-ttu-id="3b40b-145">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="3b40b-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="3b40b-146">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-146">Low</span></span>      |
| [<span data-ttu-id="3b40b-147">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="3b40b-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="3b40b-148">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-148">Low</span></span>      |
| [<span data-ttu-id="3b40b-149">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="3b40b-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="3b40b-150">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-150">Low</span></span>      |
| [<span data-ttu-id="3b40b-151">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="3b40b-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="3b40b-152">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-152">Low</span></span>      |
| [<span data-ttu-id="3b40b-153">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3b40b-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="3b40b-154">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-154">Low</span></span>      |
| [<span data-ttu-id="3b40b-155">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="3b40b-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="3b40b-156">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-156">Low</span></span>      |
| [<span data-ttu-id="3b40b-157">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="3b40b-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="3b40b-158">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-158">Low</span></span>      |
| [<span data-ttu-id="3b40b-159">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="3b40b-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="3b40b-160">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-160">Low</span></span>      |
| [<span data-ttu-id="3b40b-161">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3b40b-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="3b40b-162">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-162">Low</span></span>      |
| [<span data-ttu-id="3b40b-163">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="3b40b-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="3b40b-164">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-164">Low</span></span>      |
| [<span data-ttu-id="3b40b-165">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="3b40b-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="3b40b-166">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-166">Low</span></span>      |
| [<span data-ttu-id="3b40b-167">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="3b40b-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="3b40b-168">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-168">Low</span></span>      |
| [<span data-ttu-id="3b40b-169">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="3b40b-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="3b40b-170">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-170">Low</span></span>      |
| [<span data-ttu-id="3b40b-171">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="3b40b-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="3b40b-172">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-172">Low</span></span>      |
| [<span data-ttu-id="3b40b-173">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="3b40b-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="3b40b-174">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-174">Low</span></span>      |
| [<span data-ttu-id="3b40b-175">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="3b40b-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="3b40b-176">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-176">Low</span></span>      |
| [<span data-ttu-id="3b40b-177">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3b40b-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="3b40b-178">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-178">Low</span></span>      |
| [<span data-ttu-id="3b40b-179">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="3b40b-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="3b40b-180">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-180">Low</span></span>      |
| [<span data-ttu-id="3b40b-181">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="3b40b-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="3b40b-182">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-182">Low</span></span>      |
| [<span data-ttu-id="3b40b-183">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3b40b-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="3b40b-184">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-184">Low</span></span>      |
| [<span data-ttu-id="3b40b-185">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="3b40b-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="3b40b-186">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-186">Low</span></span>      |
| [<span data-ttu-id="3b40b-187">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="3b40b-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="3b40b-188">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-188">Low</span></span>      |
| [<span data-ttu-id="3b40b-189">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="3b40b-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="3b40b-190">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-190">Low</span></span>      |
| [<span data-ttu-id="3b40b-191">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3b40b-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="3b40b-192">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-192">Low</span></span>      |
| [<span data-ttu-id="3b40b-193">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="3b40b-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="3b40b-194">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-194">Low</span></span>      |
| [<span data-ttu-id="3b40b-195">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="3b40b-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="3b40b-196">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-196">Low</span></span>      |
| [<span data-ttu-id="3b40b-197">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="3b40b-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="3b40b-198">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-198">Low</span></span>      |
| [<span data-ttu-id="3b40b-199">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3b40b-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="3b40b-200">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-200">Low</span></span>      |
| [<span data-ttu-id="3b40b-201">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b40b-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="3b40b-202">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-202">Low</span></span>      |
| [<span data-ttu-id="3b40b-203">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b40b-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="3b40b-204">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-204">Low</span></span>      |
| [<span data-ttu-id="3b40b-205">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="3b40b-206">Małe</span><span class="sxs-lookup"><span data-stu-id="3b40b-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="3b40b-207">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="3b40b-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="3b40b-208">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="3b40b-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="3b40b-209">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-210">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-210">**Old behavior**</span></span>

<span data-ttu-id="3b40b-211">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="3b40b-212">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="3b40b-213">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-213">**New behavior**</span></span>

<span data-ttu-id="3b40b-214">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu ( `Select()` ostatnie wywołanie zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="3b40b-215">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3b40b-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="3b40b-216">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-216">**Why**</span></span>

<span data-ttu-id="3b40b-217">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="3b40b-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="3b40b-218">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="3b40b-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="3b40b-219">Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych, oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="3b40b-220">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="3b40b-221">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="3b40b-222">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="3b40b-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="3b40b-223">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-223">**Mitigations**</span></span>

<span data-ttu-id="3b40b-224">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć lub użyć `AsEnumerable()`, `ToList()`lub podobnie jak jawnie przenieść dane z powrotem do klienta, na którym można następnie przetworzyć je za pomocą LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="3b40b-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="3b40b-225">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="3b40b-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="3b40b-226">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="3b40b-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="3b40b-227">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-228">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-228">**Old behavior**</span></span>

<span data-ttu-id="3b40b-229">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b40b-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="3b40b-230">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-230">**New behavior**</span></span>

<span data-ttu-id="3b40b-231">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="3b40b-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="3b40b-232">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b40b-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="3b40b-233">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-233">**Why**</span></span>

<span data-ttu-id="3b40b-234">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="3b40b-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="3b40b-235">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-235">**Mitigations**</span></span>

<span data-ttu-id="3b40b-236">Rozważ przeniesienie do nowoczesnej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3b40b-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="3b40b-237">Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b40b-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="3b40b-238">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="3b40b-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="3b40b-239">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="3b40b-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="3b40b-240">Ta zmiana została wprowadzona w ASP.NET Core 3,0 — wersja zapoznawcza 1.</span><span class="sxs-lookup"><span data-stu-id="3b40b-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="3b40b-241">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-241">**Old behavior**</span></span>

<span data-ttu-id="3b40b-242">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, będzie ona obejmować EF Core i niektórych dostawców danych EF Core, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b40b-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="3b40b-243">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-243">**New behavior**</span></span>

<span data-ttu-id="3b40b-244">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b40b-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="3b40b-245">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-245">**Why**</span></span>

<span data-ttu-id="3b40b-246">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="3b40b-247">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="3b40b-248">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="3b40b-249">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="3b40b-250">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-250">**Mitigations**</span></span>

<span data-ttu-id="3b40b-251">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3b40b-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="3b40b-252">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3b40b-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="3b40b-253">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="3b40b-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="3b40b-254">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4 i odpowiadająca jej wersja zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="3b40b-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="3b40b-255">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-255">**Old behavior**</span></span>

<span data-ttu-id="3b40b-256">Przed 3,0 `dotnet ef` narzędzie zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="3b40b-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="3b40b-257">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-257">**New behavior**</span></span>

<span data-ttu-id="3b40b-258">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera `dotnet ef` narzędzia, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="3b40b-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="3b40b-259">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-259">**Why**</span></span>

<span data-ttu-id="3b40b-260">Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b40b-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="3b40b-261">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-261">**Mitigations**</span></span>

<span data-ttu-id="3b40b-262">Aby móc zarządzać migracjami lub szkieletem a `DbContext`, zainstaluj `dotnet-ef` program jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="3b40b-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="3b40b-263">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="3b40b-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="3b40b-264">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="3b40b-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="3b40b-265">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="3b40b-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="3b40b-266">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-267">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-267">**Old behavior**</span></span>

<span data-ttu-id="3b40b-268">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="3b40b-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="3b40b-269">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-269">**New behavior**</span></span>

<span data-ttu-id="3b40b-270">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="3b40b-271">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="3b40b-272">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i`ExecuteSqlInterpolatedAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przenoszone w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="3b40b-273">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="3b40b-274">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="3b40b-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="3b40b-275">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-275">**Why**</span></span>

<span data-ttu-id="3b40b-276">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="3b40b-277">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="3b40b-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="3b40b-278">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-278">**Mitigations**</span></span>

<span data-ttu-id="3b40b-279">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="3b40b-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="3b40b-280">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="3b40b-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="3b40b-281">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="3b40b-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="3b40b-282">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="3b40b-283">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-283">**Old behavior**</span></span>

<span data-ttu-id="3b40b-284">Przed EF Core 3,0, `FromSql` Metoda może być określona w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="3b40b-285">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-285">**New behavior**</span></span>

<span data-ttu-id="3b40b-286">Począwszy od EF Core 3,0, `FromSqlRaw` nowe i `FromSqlInterpolated` metody (zastępujące `FromSql`) można określić tylko dla katalogów głównych `DbSet<>`zapytań, tj. bezpośrednio na.</span><span class="sxs-lookup"><span data-stu-id="3b40b-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="3b40b-287">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="3b40b-288">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-288">**Why**</span></span>

<span data-ttu-id="3b40b-289">Określenie `FromSql` dowolnego miejsca, w którym `DbSet` nie ma żadnego dodanego znaczenia ani dodanej wartości, może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="3b40b-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="3b40b-290">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-290">**Mitigations**</span></span>

<span data-ttu-id="3b40b-291">`FromSql`wywołania należy przenieść, aby znajdowały się bezpośrednio w, `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="3b40b-292">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="3b40b-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="3b40b-293">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="3b40b-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="3b40b-294">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="3b40b-295">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-295">**Old behavior**</span></span>

<span data-ttu-id="3b40b-296">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="3b40b-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="3b40b-297">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="3b40b-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="3b40b-298">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="3b40b-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="3b40b-299">zwróci to samo `Category` wystąpienie dla każdego `Product` , które jest skojarzone z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="3b40b-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="3b40b-300">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-300">**New behavior**</span></span>

<span data-ttu-id="3b40b-301">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="3b40b-302">Na przykład zapytanie powyżej zwróci teraz nowe `Category` wystąpienie dla każdego z nich `Product` , nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="3b40b-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="3b40b-303">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-303">**Why**</span></span>

<span data-ttu-id="3b40b-304">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="3b40b-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="3b40b-305">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="3b40b-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="3b40b-306">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="3b40b-307">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-307">**Mitigations**</span></span>

<span data-ttu-id="3b40b-308">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="3b40b-309">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="3b40b-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="3b40b-310">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="3b40b-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="3b40b-311">Ta zmiana jest przywracana w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-312">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3b40b-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="3b40b-313">Na przykład, aby przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="3b40b-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="3b40b-314">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="3b40b-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="3b40b-315">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="3b40b-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="3b40b-316">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="3b40b-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="3b40b-317">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-317">**Old behavior**</span></span>

<span data-ttu-id="3b40b-318">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="3b40b-319">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="3b40b-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="3b40b-320">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-320">**New behavior**</span></span>

<span data-ttu-id="3b40b-321">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="3b40b-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="3b40b-322">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-322">**Why**</span></span>

<span data-ttu-id="3b40b-323">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienie, jest przenoszona do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="3b40b-324">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-324">**Mitigations**</span></span>

<span data-ttu-id="3b40b-325">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i `Added` należą do jednostek w stanie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="3b40b-326">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="3b40b-326">This can be avoided by:</span></span>
* <span data-ttu-id="3b40b-327">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="3b40b-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="3b40b-328">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="3b40b-329">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="3b40b-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="3b40b-330">Na przykład zwróci `context.Entry(blog).Property(e => e.Id).CurrentValue` wartość tymczasową nawet wtedy, gdy `blog.Id` sama nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="3b40b-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="3b40b-331">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="3b40b-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="3b40b-332">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="3b40b-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="3b40b-333">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-334">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-334">**Old behavior**</span></span>

<span data-ttu-id="3b40b-335">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` zostałaby śledzona `Added` w stanie i wstawiona jako nowy wiersz, gdy `SaveChanges` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="3b40b-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="3b40b-336">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-336">**New behavior**</span></span>

<span data-ttu-id="3b40b-337">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w `Modified` stanie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="3b40b-338">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po `SaveChanges` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="3b40b-339">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona tak `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="3b40b-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="3b40b-340">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-340">**Why**</span></span>

<span data-ttu-id="3b40b-341">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="3b40b-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="3b40b-342">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-342">**Mitigations**</span></span>

<span data-ttu-id="3b40b-343">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="3b40b-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="3b40b-344">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="3b40b-345">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3b40b-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="3b40b-346">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="3b40b-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="3b40b-347">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="3b40b-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="3b40b-348">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="3b40b-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="3b40b-349">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-350">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-350">**Old behavior**</span></span>

<span data-ttu-id="3b40b-351">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3b40b-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="3b40b-352">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-352">**New behavior**</span></span>

<span data-ttu-id="3b40b-353">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="3b40b-354">Na przykład wywołanie `context.Remove()` usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane zależności są również ustawione na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="3b40b-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="3b40b-355">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-355">**Why**</span></span>

<span data-ttu-id="3b40b-356">Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="3b40b-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="3b40b-357">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-357">**Mitigations**</span></span>

<span data-ttu-id="3b40b-358">Poprzednie zachowanie można przywrócić za pomocą ustawień na stronie `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="3b40b-359">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="3b40b-360">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="3b40b-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="3b40b-361">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="3b40b-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="3b40b-362">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 5.</span><span class="sxs-lookup"><span data-stu-id="3b40b-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="3b40b-363">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-363">**Old behavior**</span></span>

<span data-ttu-id="3b40b-364">Przed 3,0, `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z `Restrict` semantyką, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.</span><span class="sxs-lookup"><span data-stu-id="3b40b-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="3b40b-365">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-365">**New behavior**</span></span>

<span data-ttu-id="3b40b-366">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone `Restrict` z semantyką--to nie, bez kaskad; throw przy naruszeniu ograniczenia — bez również wpływu na wewnętrzną naprawę EF.</span><span class="sxs-lookup"><span data-stu-id="3b40b-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="3b40b-367">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-367">**Why**</span></span>

<span data-ttu-id="3b40b-368">Ta zmiana została wprowadzona w celu usprawnienia korzystania `DeleteBehavior` z programu w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="3b40b-369">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-369">**Mitigations**</span></span>

<span data-ttu-id="3b40b-370">Poprzednie zachowanie można przywrócić za pomocą polecenia `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="3b40b-371">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="3b40b-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="3b40b-372">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="3b40b-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="3b40b-373">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-374">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-374">**Old behavior**</span></span>

<span data-ttu-id="3b40b-375">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="3b40b-375">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="3b40b-376">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="3b40b-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="3b40b-377">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-377">**New behavior**</span></span>

<span data-ttu-id="3b40b-378">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="3b40b-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="3b40b-379">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="3b40b-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="3b40b-380">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-380">**Why**</span></span>

<span data-ttu-id="3b40b-381">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="3b40b-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="3b40b-382">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="3b40b-383">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="3b40b-384">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-384">**Mitigations**</span></span>

<span data-ttu-id="3b40b-385">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="3b40b-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="3b40b-386">**`ModelBuilder.Query<>()`** — Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="3b40b-387">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="3b40b-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="3b40b-388">**`DbQuery<>`** — Zamiast `DbSet<>` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="3b40b-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="3b40b-389">**`DbContext.Query<>()`** — Zamiast `DbContext.Set<>()` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="3b40b-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="3b40b-390">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="3b40b-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="3b40b-391">[Śledzenie](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
problemów ze śledzeniem #12444[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
śledzenia problemów[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="3b40b-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="3b40b-392">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-393">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-393">**Old behavior**</span></span>

<span data-ttu-id="3b40b-394">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po `OwnsOne` wywołaniu lub. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="3b40b-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="3b40b-395">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-395">**New behavior**</span></span>

<span data-ttu-id="3b40b-396">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="3b40b-397">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="3b40b-398">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem `WithOwner()` po podobnym sposobie, jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="3b40b-399">Mimo że konfiguracja dla samego samego typu jest nadal łańcuchem `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="3b40b-400">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-400">For example:</span></span>

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

<span data-ttu-id="3b40b-401">Dodatkowo wywoływanie `Entity()`,, `Set()` lub z obiektem docelowym typu, `HasOne()`spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="3b40b-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="3b40b-402">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-402">**Why**</span></span>

<span data-ttu-id="3b40b-403">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="3b40b-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="3b40b-404">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, `HasForeignKey`takich jak.</span><span class="sxs-lookup"><span data-stu-id="3b40b-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="3b40b-405">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-405">**Mitigations**</span></span>

<span data-ttu-id="3b40b-406">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="3b40b-407">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="3b40b-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="3b40b-408">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="3b40b-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="3b40b-409">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-410">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-410">**Old behavior**</span></span>

<span data-ttu-id="3b40b-411">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="3b40b-411">Consider the following model:</span></span>
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
<span data-ttu-id="3b40b-412">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli `OrderDetails` , wystąpienie jest zawsze wymagane podczas dodawania nowego `Order`elementu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="3b40b-413">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-413">**New behavior**</span></span>

<span data-ttu-id="3b40b-414">Począwszy od 3,0, EF Core umożliwia dodanie `Order` , `OrderDetails` bez `OrderDetails` i mapuje wszystkie właściwości, z wyjątkiem klucza podstawowego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="3b40b-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="3b40b-415">Podczas wykonywania zapytania o zestawy `OrderDetails` EF Core `null` do, jeśli którekolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="3b40b-416">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-416">**Mitigations**</span></span>

<span data-ttu-id="3b40b-417">Jeśli model ma zależeć do udostępniania tabeli dla wszystkich opcjonalnych kolumn, ale Nawigacja wskazująca, że nie jest oczekiwana `null` , aplikacja powinna zostać zmodyfikowana, aby obsługiwać przypadki, gdy Nawigacja `null`jest.</span><span class="sxs-lookup"><span data-stu-id="3b40b-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="3b40b-418">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć`null` przypisaną do niej inną wartość.</span><span class="sxs-lookup"><span data-stu-id="3b40b-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="3b40b-419">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="3b40b-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="3b40b-420">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="3b40b-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="3b40b-421">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-422">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-422">**Old behavior**</span></span>

<span data-ttu-id="3b40b-423">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="3b40b-423">Consider the following model:</span></span>
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
<span data-ttu-id="3b40b-424">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="3b40b-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="3b40b-425">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-425">**New behavior**</span></span>

<span data-ttu-id="3b40b-426">Począwszy od 3,0, EF Core propaguje nową `Version` wartość do `Order` , jeśli jest właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="3b40b-427">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="3b40b-428">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-428">**Why**</span></span>

<span data-ttu-id="3b40b-429">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="3b40b-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="3b40b-430">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-430">**Mitigations**</span></span>

<span data-ttu-id="3b40b-431">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="3b40b-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="3b40b-432">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="3b40b-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="3b40b-433">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="3b40b-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="3b40b-434">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="3b40b-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="3b40b-435">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-436">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-436">**Old behavior**</span></span>

<span data-ttu-id="3b40b-437">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="3b40b-437">Consider the following model:</span></span>
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

<span data-ttu-id="3b40b-438">Przed EF Core 3,0 `ShippingAddress` Właściwość zostałaby zamapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="3b40b-439">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-439">**New behavior**</span></span>

<span data-ttu-id="3b40b-440">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="3b40b-441">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-441">**Why**</span></span>

<span data-ttu-id="3b40b-442">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="3b40b-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="3b40b-443">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-443">**Mitigations**</span></span>

<span data-ttu-id="3b40b-444">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="3b40b-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="3b40b-445">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="3b40b-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="3b40b-446">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="3b40b-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="3b40b-447">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-448">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-448">**Old behavior**</span></span>

<span data-ttu-id="3b40b-449">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="3b40b-449">Consider the following model:</span></span>
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
<span data-ttu-id="3b40b-450">Przed EF Core 3,0, `CustomerId` właściwość będzie używana dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="3b40b-451">Jeśli `Order` jednak jest typem będącym własnością, to `CustomerId` również klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="3b40b-452">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-452">**New behavior**</span></span>

<span data-ttu-id="3b40b-453">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3b40b-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="3b40b-454">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="3b40b-455">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-455">For example:</span></span>

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

<span data-ttu-id="3b40b-456">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-456">**Why**</span></span>

<span data-ttu-id="3b40b-457">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="3b40b-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="3b40b-458">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-458">**Mitigations**</span></span>

<span data-ttu-id="3b40b-459">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="3b40b-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="3b40b-460">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3b40b-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="3b40b-461">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="3b40b-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="3b40b-462">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-463">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-463">**Old behavior**</span></span>

<span data-ttu-id="3b40b-464">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="3b40b-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="3b40b-465">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-465">**New behavior**</span></span>

<span data-ttu-id="3b40b-466">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="3b40b-467">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-467">**Why**</span></span>

<span data-ttu-id="3b40b-468">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="3b40b-469">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="3b40b-470">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-470">**Mitigations**</span></span>

<span data-ttu-id="3b40b-471">Jeśli połączenie musi pozostać otwarte jawne wywołanie w celu `OpenConnection()` upewnienia się, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="3b40b-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="3b40b-472">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="3b40b-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="3b40b-473">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="3b40b-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="3b40b-474">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-475">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-475">**Old behavior**</span></span>

<span data-ttu-id="3b40b-476">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b40b-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="3b40b-477">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-477">**New behavior**</span></span>

<span data-ttu-id="3b40b-478">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b40b-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="3b40b-479">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="3b40b-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="3b40b-480">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-480">**Why**</span></span>

<span data-ttu-id="3b40b-481">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b40b-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="3b40b-482">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-482">**Mitigations**</span></span>

<span data-ttu-id="3b40b-483">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b40b-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="3b40b-484">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="3b40b-485">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="3b40b-485">Backing fields are used by default</span></span>

[<span data-ttu-id="3b40b-486">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="3b40b-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="3b40b-487">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="3b40b-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="3b40b-488">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-488">**Old behavior**</span></span>

<span data-ttu-id="3b40b-489">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="3b40b-490">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="3b40b-491">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-491">**New behavior**</span></span>

<span data-ttu-id="3b40b-492">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="3b40b-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="3b40b-493">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="3b40b-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="3b40b-494">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-494">**Why**</span></span>

<span data-ttu-id="3b40b-495">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="3b40b-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="3b40b-496">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-496">**Mitigations**</span></span>

<span data-ttu-id="3b40b-497">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu `ModelBuilder`dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="3b40b-498">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="3b40b-499">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="3b40b-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="3b40b-500">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="3b40b-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="3b40b-501">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-502">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-502">**Old behavior**</span></span>

<span data-ttu-id="3b40b-503">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="3b40b-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="3b40b-504">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="3b40b-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="3b40b-505">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-505">**New behavior**</span></span>

<span data-ttu-id="3b40b-506">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3b40b-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="3b40b-507">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-507">**Why**</span></span>

<span data-ttu-id="3b40b-508">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="3b40b-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="3b40b-509">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-509">**Mitigations**</span></span>

<span data-ttu-id="3b40b-510">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="3b40b-511">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3b40b-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="3b40b-512">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="3b40b-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="3b40b-513">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-514">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-514">**Old behavior**</span></span>

<span data-ttu-id="3b40b-515">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="3b40b-516">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-516">**New behavior**</span></span>

<span data-ttu-id="3b40b-517">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="3b40b-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="3b40b-518">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-518">**Why**</span></span>

<span data-ttu-id="3b40b-519">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="3b40b-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="3b40b-520">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-520">**Mitigations**</span></span>

<span data-ttu-id="3b40b-521">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="3b40b-522">W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="3b40b-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="3b40b-523">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3b40b-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="3b40b-524">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="3b40b-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="3b40b-525">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-526">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-526">**Old behavior**</span></span>

<span data-ttu-id="3b40b-527">Przed `AddDbContext` EF Core 3,0, wywoływanie `AddDbContextPool` lub zarejestrowanie usługi rejestrowania i pamięci podręcznej w usłudze D. I przez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3b40b-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="3b40b-528">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-528">**New behavior**</span></span>

<span data-ttu-id="3b40b-529">Począwszy od EF Core 3,0 `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="3b40b-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="3b40b-530">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-530">**Why**</span></span>

<span data-ttu-id="3b40b-531">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="3b40b-532">Jeśli `ILoggerFactory` jednak program jest zarejestrowany w kontenerze di aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b40b-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="3b40b-533">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-533">**Mitigations**</span></span>

<span data-ttu-id="3b40b-534">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3b40b-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="3b40b-535">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="3b40b-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="3b40b-536">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="3b40b-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="3b40b-537">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-538">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-538">**Old behavior**</span></span>

<span data-ttu-id="3b40b-539">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="3b40b-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="3b40b-540">W ten sposób zapewniono, że stan ujawniony na `EntityEntry` bieżąco.</span><span class="sxs-lookup"><span data-stu-id="3b40b-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="3b40b-541">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-541">**New behavior**</span></span>

<span data-ttu-id="3b40b-542">Począwszy od EF Core 3,0, wywołanie `DbContext.Entry` będzie teraz podejmować próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="3b40b-543">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="3b40b-544">Należy pamiętać, `ChangeTracker.AutoDetectChangesEnabled` że jeśli jest `false` ustawiona na, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="3b40b-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="3b40b-545">Inne metody, które powodują wykrycie zmiany — na `ChangeTracker.Entries` przykład `SaveChanges`i--nadal powodują pełne `DetectChanges` wszystkie śledzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="3b40b-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="3b40b-546">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-546">**Why**</span></span>

<span data-ttu-id="3b40b-547">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania `context.Entry`z programu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="3b40b-548">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-548">**Mitigations**</span></span>

<span data-ttu-id="3b40b-549">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed `Entry` wywołaniem, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="3b40b-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="3b40b-550">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="3b40b-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="3b40b-551">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="3b40b-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="3b40b-552">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-553">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-553">**Old behavior**</span></span>

<span data-ttu-id="3b40b-554">Przed EF Core 3,0 `string` i `byte[]` właściwości klucza mogą być używane bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="3b40b-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="3b40b-555">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, Zserializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="3b40b-556">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-556">**New behavior**</span></span>

<span data-ttu-id="3b40b-557">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="3b40b-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="3b40b-558">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-558">**Why**</span></span>

<span data-ttu-id="3b40b-559">Ta zmiana została wprowadzona, ponieważ zazwyczaj wartości `string` generowane / `byte[]` przez klienta nie są przydatne, a domyślne zachowanie spowodowało trudne powody dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="3b40b-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="3b40b-560">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-560">**Mitigations**</span></span>

<span data-ttu-id="3b40b-561">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="3b40b-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="3b40b-562">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3b40b-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="3b40b-563">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="3b40b-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="3b40b-564">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="3b40b-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="3b40b-565">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="3b40b-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="3b40b-566">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-567">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-567">**Old behavior**</span></span>

<span data-ttu-id="3b40b-568">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="3b40b-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="3b40b-569">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-569">**New behavior**</span></span>

<span data-ttu-id="3b40b-570">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="3b40b-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="3b40b-571">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-571">**Why**</span></span>

<span data-ttu-id="3b40b-572">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z `DbContext` wystąpieniem, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="3b40b-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="3b40b-573">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-573">**Mitigations**</span></span>

<span data-ttu-id="3b40b-574">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="3b40b-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="3b40b-575">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="3b40b-575">This isn't common.</span></span>
<span data-ttu-id="3b40b-576">W takich przypadkach większość elementów będzie nadal działać, ale wszystkie pojedyncze usługi, w `ILoggerFactory` zależności od tego, muszą zostać zmienione w celu `ILoggerFactory` uzyskania w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="3b40b-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="3b40b-577">Jeśli używasz `ILoggerFactory` takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak można lepiej zrozumieć, jak nie należy ponownie rozbić w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="3b40b-578">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="3b40b-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="3b40b-579">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="3b40b-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="3b40b-580">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-581">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-581">**Old behavior**</span></span>

<span data-ttu-id="3b40b-582">Przed EF Core 3,0, gdy zostanie `DbContext` usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="3b40b-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="3b40b-583">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="3b40b-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="3b40b-584">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="3b40b-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="3b40b-585">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-585">**New behavior**</span></span>

<span data-ttu-id="3b40b-586">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="3b40b-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="3b40b-587">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="3b40b-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="3b40b-588">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="3b40b-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="3b40b-589">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="3b40b-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="3b40b-590">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-590">**Why**</span></span>

<span data-ttu-id="3b40b-591">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie przy próbie załadowania `DbContext` opóźnionego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="3b40b-592">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-592">**Mitigations**</span></span>

<span data-ttu-id="3b40b-593">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="3b40b-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="3b40b-594">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="3b40b-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="3b40b-595">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="3b40b-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="3b40b-596">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-597">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-597">**Old behavior**</span></span>

<span data-ttu-id="3b40b-598">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="3b40b-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="3b40b-599">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-599">**New behavior**</span></span>

<span data-ttu-id="3b40b-600">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3b40b-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="3b40b-601">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-601">**Why**</span></span>

<span data-ttu-id="3b40b-602">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="3b40b-603">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-603">**Mitigations**</span></span>

<span data-ttu-id="3b40b-604">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="3b40b-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="3b40b-605">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację w `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="3b40b-606">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="3b40b-607">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="3b40b-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="3b40b-608">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="3b40b-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="3b40b-609">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-610">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-610">**Old behavior**</span></span>

<span data-ttu-id="3b40b-611">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="3b40b-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="3b40b-612">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="3b40b-613">Kod wygląda tak `Samurai` , jakby odnosił się do innego typu jednostki `Entrance` przy użyciu właściwości nawigacji, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="3b40b-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="3b40b-614">W rzeczywistości ten kod próbuje utworzyć relację z typem obiektu o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="3b40b-615">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-615">**New behavior**</span></span>

<span data-ttu-id="3b40b-616">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="3b40b-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="3b40b-617">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-617">**Why**</span></span>

<span data-ttu-id="3b40b-618">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="3b40b-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="3b40b-619">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-619">**Mitigations**</span></span>

<span data-ttu-id="3b40b-620">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="3b40b-621">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="3b40b-621">This is not common.</span></span>
<span data-ttu-id="3b40b-622">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="3b40b-623">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="3b40b-624">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="3b40b-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="3b40b-625">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="3b40b-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="3b40b-626">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-627">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-627">**Old behavior**</span></span>

<span data-ttu-id="3b40b-628">Następujące metody asynchroniczne zwróciły `Task<T>`wcześniej:</span><span class="sxs-lookup"><span data-stu-id="3b40b-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="3b40b-629">`ValueGenerator.NextValueAsync()`(i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="3b40b-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="3b40b-630">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-630">**New behavior**</span></span>

<span data-ttu-id="3b40b-631">Powyższe metody teraz zwracają wartość `ValueTask<T>` powyżej `T` , tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="3b40b-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="3b40b-632">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-632">**Why**</span></span>

<span data-ttu-id="3b40b-633">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="3b40b-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="3b40b-634">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-634">**Mitigations**</span></span>

<span data-ttu-id="3b40b-635">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="3b40b-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="3b40b-636">Bardziej złożone użycie (np. przekazanie zwróconej `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` przekonwertowanie na a `Task<T>` przez wywołanie `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="3b40b-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="3b40b-637">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="3b40b-638">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3b40b-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="3b40b-639">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="3b40b-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="3b40b-640">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 2.</span><span class="sxs-lookup"><span data-stu-id="3b40b-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="3b40b-641">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-641">**Old behavior**</span></span>

<span data-ttu-id="3b40b-642">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3b40b-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="3b40b-643">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-643">**New behavior**</span></span>

<span data-ttu-id="3b40b-644">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3b40b-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="3b40b-645">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-645">**Why**</span></span>

<span data-ttu-id="3b40b-646">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="3b40b-647">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-647">**Mitigations**</span></span>

<span data-ttu-id="3b40b-648">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="3b40b-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="3b40b-649">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="3b40b-650">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="3b40b-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="3b40b-651">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="3b40b-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="3b40b-652">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-653">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-653">**Old behavior**</span></span>

<span data-ttu-id="3b40b-654">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="3b40b-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="3b40b-655">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-655">**New behavior**</span></span>

<span data-ttu-id="3b40b-656">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, wywołana `ToTable()` dla typu pochodnego będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="3b40b-657">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-657">**Why**</span></span>

<span data-ttu-id="3b40b-658">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="3b40b-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="3b40b-659">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="3b40b-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="3b40b-660">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-660">**Mitigations**</span></span>

<span data-ttu-id="3b40b-661">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="3b40b-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="3b40b-662">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="3b40b-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="3b40b-663">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="3b40b-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="3b40b-664">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-665">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-665">**Old behavior**</span></span>

<span data-ttu-id="3b40b-666">Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnia sposób konfigurowania kolumn używanych w programie. `INCLUDE`</span><span class="sxs-lookup"><span data-stu-id="3b40b-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="3b40b-667">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-667">**New behavior**</span></span>

<span data-ttu-id="3b40b-668">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3b40b-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="3b40b-669">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="3b40b-670">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-670">**Why**</span></span>

<span data-ttu-id="3b40b-671">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="3b40b-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="3b40b-672">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-672">**Mitigations**</span></span>

<span data-ttu-id="3b40b-673">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="3b40b-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="3b40b-674">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="3b40b-674">Metadata API changes</span></span>

[<span data-ttu-id="3b40b-675">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="3b40b-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3b40b-676">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-677">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-677">**New behavior**</span></span>

<span data-ttu-id="3b40b-678">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="3b40b-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="3b40b-679">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-679">**Why**</span></span>

<span data-ttu-id="3b40b-680">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="3b40b-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="3b40b-681">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-681">**Mitigations**</span></span>

<span data-ttu-id="3b40b-682">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="3b40b-683">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="3b40b-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="3b40b-684">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="3b40b-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3b40b-685">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="3b40b-686">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-686">**New behavior**</span></span>

<span data-ttu-id="3b40b-687">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="3b40b-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="3b40b-688">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-688">**Why**</span></span>

<span data-ttu-id="3b40b-689">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="3b40b-690">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-690">**Mitigations**</span></span>

<span data-ttu-id="3b40b-691">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="3b40b-692">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="3b40b-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="3b40b-693">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="3b40b-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="3b40b-694">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 3.</span><span class="sxs-lookup"><span data-stu-id="3b40b-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="3b40b-695">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-695">**Old behavior**</span></span>

<span data-ttu-id="3b40b-696">Przed EF Core 3,0, EF Core wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z systemem SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b40b-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3b40b-697">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-697">**New behavior**</span></span>

<span data-ttu-id="3b40b-698">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b40b-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3b40b-699">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-699">**Why**</span></span>

<span data-ttu-id="3b40b-700">Ta zmiana została wprowadzona, ponieważ domyślnie `SQLitePCLRaw.bundle_e_sqlite3` używa EF Core, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="3b40b-701">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-701">**Mitigations**</span></span>

<span data-ttu-id="3b40b-702">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b40b-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="3b40b-703">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="3b40b-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="3b40b-704">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3b40b-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="3b40b-705">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-705">**Old behavior**</span></span>

<span data-ttu-id="3b40b-706">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="3b40b-707">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-707">**New behavior**</span></span>

<span data-ttu-id="3b40b-708">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="3b40b-709">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-709">**Why**</span></span>

<span data-ttu-id="3b40b-710">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="3b40b-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="3b40b-711">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-711">**Mitigations**</span></span>

<span data-ttu-id="3b40b-712">Aby korzystać z natywnej wersji programu SQLite w systemie `Microsoft.Data.Sqlite` iOS, należy skonfigurować `SQLitePCLRaw` do korzystania z innego pakietu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3b40b-713">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="3b40b-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3b40b-714">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="3b40b-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="3b40b-715">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-716">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-716">**Old behavior**</span></span>

<span data-ttu-id="3b40b-717">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b40b-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="3b40b-718">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-718">**New behavior**</span></span>

<span data-ttu-id="3b40b-719">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="3b40b-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="3b40b-720">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-720">**Why**</span></span>

<span data-ttu-id="3b40b-721">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="3b40b-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="3b40b-722">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="3b40b-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3b40b-723">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-723">**Mitigations**</span></span>

<span data-ttu-id="3b40b-724">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="3b40b-725">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="3b40b-726">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="3b40b-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3b40b-727">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="3b40b-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3b40b-728">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="3b40b-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="3b40b-729">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-730">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-730">**Old behavior**</span></span>

<span data-ttu-id="3b40b-731">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b40b-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="3b40b-732">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="3b40b-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="3b40b-733">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-733">**New behavior**</span></span>

<span data-ttu-id="3b40b-734">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="3b40b-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="3b40b-735">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-735">**Why**</span></span>

<span data-ttu-id="3b40b-736">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="3b40b-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3b40b-737">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-737">**Mitigations**</span></span>

<span data-ttu-id="3b40b-738">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="3b40b-739">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="3b40b-740">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="3b40b-741">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="3b40b-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="3b40b-742">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="3b40b-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="3b40b-743">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-744">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-744">**Old behavior**</span></span>

<span data-ttu-id="3b40b-745">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="3b40b-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="3b40b-746">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-746">**New behavior**</span></span>

<span data-ttu-id="3b40b-747">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="3b40b-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="3b40b-748">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-748">**Why**</span></span>

<span data-ttu-id="3b40b-749">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="3b40b-750">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="3b40b-751">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-751">**Mitigations**</span></span>

<span data-ttu-id="3b40b-752">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="3b40b-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="3b40b-753">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="3b40b-754">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="3b40b-755">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="3b40b-756">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="3b40b-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="3b40b-757">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="3b40b-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="3b40b-758">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="3b40b-759">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-759">**Old behavior**</span></span>

<span data-ttu-id="3b40b-760">Przed EF Core 3,0 `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="3b40b-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="3b40b-761">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-761">**New behavior**</span></span>

<span data-ttu-id="3b40b-762">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b40b-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="3b40b-763">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-763">**Why**</span></span>

<span data-ttu-id="3b40b-764">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="3b40b-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="3b40b-765">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-765">**Mitigations**</span></span>

<span data-ttu-id="3b40b-766">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="3b40b-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="3b40b-767">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="3b40b-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="3b40b-768">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="3b40b-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="3b40b-769">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3b40b-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="3b40b-770">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="3b40b-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="3b40b-771">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-772">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-772">**Old behavior**</span></span>

<span data-ttu-id="3b40b-773">`IDbContextOptionsExtension`zawiera metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="3b40b-774">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-774">**New behavior**</span></span>

<span data-ttu-id="3b40b-775">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b40b-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="3b40b-776">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-776">**Why**</span></span>

<span data-ttu-id="3b40b-777">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="3b40b-778">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="3b40b-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="3b40b-779">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-779">**Mitigations**</span></span>

<span data-ttu-id="3b40b-780">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="3b40b-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="3b40b-781">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="3b40b-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="3b40b-782">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="3b40b-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="3b40b-783">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="3b40b-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="3b40b-784">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-785">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="3b40b-785">**Change**</span></span>

<span data-ttu-id="3b40b-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="3b40b-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="3b40b-787">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-787">**Why**</span></span>

<span data-ttu-id="3b40b-788">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="3b40b-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="3b40b-789">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-789">**Mitigations**</span></span>

<span data-ttu-id="3b40b-790">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-790">Use the new name.</span></span> <span data-ttu-id="3b40b-791">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="3b40b-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="3b40b-792">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="3b40b-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="3b40b-793">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="3b40b-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="3b40b-794">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-795">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-795">**Old behavior**</span></span>

<span data-ttu-id="3b40b-796">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="3b40b-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="3b40b-797">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="3b40b-798">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-798">**New behavior**</span></span>

<span data-ttu-id="3b40b-799">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="3b40b-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="3b40b-800">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="3b40b-801">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-801">**Why**</span></span>

<span data-ttu-id="3b40b-802">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="3b40b-803">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-803">**Mitigations**</span></span>

<span data-ttu-id="3b40b-804">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="3b40b-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="3b40b-805">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="3b40b-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="3b40b-806">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="3b40b-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="3b40b-807">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-808">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-808">**Old behavior**</span></span>

<span data-ttu-id="3b40b-809">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="3b40b-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="3b40b-810">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-810">**New behavior**</span></span>

<span data-ttu-id="3b40b-811">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="3b40b-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="3b40b-812">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-812">**Why**</span></span>

<span data-ttu-id="3b40b-813">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="3b40b-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="3b40b-814">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="3b40b-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="3b40b-815">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-815">**Mitigations**</span></span>

<span data-ttu-id="3b40b-816">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="3b40b-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="3b40b-817">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3b40b-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="3b40b-818">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="3b40b-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="3b40b-819">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 4.</span><span class="sxs-lookup"><span data-stu-id="3b40b-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="3b40b-820">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-820">**Old behavior**</span></span>

<span data-ttu-id="3b40b-821">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="3b40b-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="3b40b-822">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-822">**New behavior**</span></span>

<span data-ttu-id="3b40b-823">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3b40b-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="3b40b-824">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="3b40b-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="3b40b-825">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-825">**Why**</span></span>

<span data-ttu-id="3b40b-826">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="3b40b-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="3b40b-827">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="3b40b-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="3b40b-828">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="3b40b-829">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-829">**Mitigations**</span></span>

<span data-ttu-id="3b40b-830">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="3b40b-831">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="3b40b-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="3b40b-832">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b40b-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="3b40b-833">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="3b40b-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="3b40b-834">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-835">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-835">**Old behavior**</span></span>

<span data-ttu-id="3b40b-836">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="3b40b-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="3b40b-837">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-837">**New behavior**</span></span>

<span data-ttu-id="3b40b-838">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b40b-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3b40b-839">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-839">**Why**</span></span>

<span data-ttu-id="3b40b-840">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="3b40b-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="3b40b-841">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="3b40b-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="3b40b-842">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-842">**Mitigations**</span></span>

<span data-ttu-id="3b40b-843">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="3b40b-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3b40b-844">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="3b40b-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="3b40b-845">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b40b-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="3b40b-846">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="3b40b-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="3b40b-847">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 7.</span><span class="sxs-lookup"><span data-stu-id="3b40b-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="3b40b-848">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-848">**Old behavior**</span></span>

<span data-ttu-id="3b40b-849">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="3b40b-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="3b40b-850">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-850">**New behavior**</span></span>

<span data-ttu-id="3b40b-851">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b40b-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3b40b-852">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-852">**Why**</span></span>

<span data-ttu-id="3b40b-853">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="3b40b-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="3b40b-854">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-854">**Mitigations**</span></span>

<span data-ttu-id="3b40b-855">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="3b40b-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3b40b-856">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="3b40b-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="3b40b-857">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="3b40b-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="3b40b-858">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="3b40b-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="3b40b-859">Ta zmiana została wprowadzona w EF Core 3,0 — wersja zapoznawcza 6.</span><span class="sxs-lookup"><span data-stu-id="3b40b-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="3b40b-860">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-860">**Old behavior**</span></span>

<span data-ttu-id="3b40b-861">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="3b40b-862">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-862">For example:</span></span>

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

<span data-ttu-id="3b40b-863">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="3b40b-863">**New behavior**</span></span>

<span data-ttu-id="3b40b-864">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="3b40b-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="3b40b-865">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="3b40b-865">**Why**</span></span>

<span data-ttu-id="3b40b-866">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="3b40b-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="3b40b-867">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="3b40b-867">**Mitigations**</span></span>

<span data-ttu-id="3b40b-868">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="3b40b-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="3b40b-869">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b40b-869">For example:</span></span>

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
