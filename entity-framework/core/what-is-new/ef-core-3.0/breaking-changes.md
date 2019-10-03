---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813618"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="d87a4-102">Istotne zmiany zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="d87a4-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="d87a4-103">Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="d87a4-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="d87a4-104">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="d87a4-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="d87a4-105">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d87a4-105">Summary</span></span>

| <span data-ttu-id="d87a4-106">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="d87a4-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="d87a4-107">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="d87a4-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="d87a4-108">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="d87a4-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="d87a4-109">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-109">High</span></span>       |
| [<span data-ttu-id="d87a4-110">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="d87a4-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="d87a4-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-111">High</span></span>      |
| [<span data-ttu-id="d87a4-112">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="d87a4-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="d87a4-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-113">High</span></span>      |
| [<span data-ttu-id="d87a4-114">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="d87a4-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="d87a4-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-115">High</span></span>      |
| [<span data-ttu-id="d87a4-116">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="d87a4-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="d87a4-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-117">High</span></span>      |
| [<span data-ttu-id="d87a4-118">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="d87a4-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="d87a4-119">Wysoka</span><span class="sxs-lookup"><span data-stu-id="d87a4-119">High</span></span>      |
| [<span data-ttu-id="d87a4-120">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="d87a4-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="d87a4-121">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-121">Medium</span></span>      |
| [<span data-ttu-id="d87a4-122">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="d87a4-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="d87a4-123">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-123">Medium</span></span>      |
| [<span data-ttu-id="d87a4-124">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="d87a4-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="d87a4-125">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-125">Medium</span></span>      |
| [<span data-ttu-id="d87a4-126">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="d87a4-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="d87a4-127">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-127">Medium</span></span>      |
| [<span data-ttu-id="d87a4-128">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="d87a4-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="d87a4-129">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-129">Medium</span></span>      |
| [<span data-ttu-id="d87a4-130">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="d87a4-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="d87a4-131">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-131">Medium</span></span>      |
| [<span data-ttu-id="d87a4-132">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="d87a4-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="d87a4-133">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-133">Medium</span></span>      |
| [<span data-ttu-id="d87a4-134">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="d87a4-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="d87a4-135">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-135">Medium</span></span>      |
| [<span data-ttu-id="d87a4-136">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="d87a4-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="d87a4-137">Średni</span><span class="sxs-lookup"><span data-stu-id="d87a4-137">Medium</span></span>      |
| [<span data-ttu-id="d87a4-138">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="d87a4-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="d87a4-139">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-139">Low</span></span>      |
| [<span data-ttu-id="d87a4-140">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="d87a4-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="d87a4-141">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-141">Low</span></span>      |
| [<span data-ttu-id="d87a4-142">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="d87a4-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="d87a4-143">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-143">Low</span></span>      |
| [<span data-ttu-id="d87a4-144">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="d87a4-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="d87a4-145">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-145">Low</span></span>      |
| [<span data-ttu-id="d87a4-146">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="d87a4-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="d87a4-147">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-147">Low</span></span>      |
| [<span data-ttu-id="d87a4-148">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="d87a4-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="d87a4-149">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-149">Low</span></span>      |
| [<span data-ttu-id="d87a4-150">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="d87a4-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="d87a4-151">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-151">Low</span></span>      |
| [<span data-ttu-id="d87a4-152">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="d87a4-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="d87a4-153">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-153">Low</span></span>      |
| [<span data-ttu-id="d87a4-154">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="d87a4-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="d87a4-155">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-155">Low</span></span>      |
| [<span data-ttu-id="d87a4-156">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="d87a4-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="d87a4-157">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-157">Low</span></span>      |
| [<span data-ttu-id="d87a4-158">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="d87a4-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="d87a4-159">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-159">Low</span></span>      |
| [<span data-ttu-id="d87a4-160">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="d87a4-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="d87a4-161">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-161">Low</span></span>      |
| [<span data-ttu-id="d87a4-162">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="d87a4-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="d87a4-163">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-163">Low</span></span>      |
| [<span data-ttu-id="d87a4-164">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="d87a4-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="d87a4-165">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-165">Low</span></span>      |
| [<span data-ttu-id="d87a4-166">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="d87a4-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="d87a4-167">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-167">Low</span></span>      |
| [<span data-ttu-id="d87a4-168">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="d87a4-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="d87a4-169">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-169">Low</span></span>      |
| [<span data-ttu-id="d87a4-170">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="d87a4-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="d87a4-171">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-171">Low</span></span>      |
| [<span data-ttu-id="d87a4-172">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="d87a4-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="d87a4-173">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-173">Low</span></span>      |
| [<span data-ttu-id="d87a4-174">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="d87a4-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="d87a4-175">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-175">Low</span></span>      |
| [<span data-ttu-id="d87a4-176">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="d87a4-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="d87a4-177">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-177">Low</span></span>      |
| [<span data-ttu-id="d87a4-178">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="d87a4-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="d87a4-179">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-179">Low</span></span>      |
| [<span data-ttu-id="d87a4-180">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="d87a4-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="d87a4-181">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-181">Low</span></span>      |
| [<span data-ttu-id="d87a4-182">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="d87a4-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="d87a4-183">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-183">Low</span></span>      |
| [<span data-ttu-id="d87a4-184">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="d87a4-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="d87a4-185">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-185">Low</span></span>      |
| [<span data-ttu-id="d87a4-186">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="d87a4-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="d87a4-187">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-187">Low</span></span>      |
| [<span data-ttu-id="d87a4-188">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="d87a4-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="d87a4-189">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-189">Low</span></span>      |
| [<span data-ttu-id="d87a4-190">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="d87a4-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="d87a4-191">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-191">Low</span></span>      |
| [<span data-ttu-id="d87a4-192">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="d87a4-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="d87a4-193">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-193">Low</span></span>      |
| [<span data-ttu-id="d87a4-194">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="d87a4-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="d87a4-195">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-195">Low</span></span>      |
| [<span data-ttu-id="d87a4-196">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="d87a4-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="d87a4-197">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-197">Low</span></span>      |
| [<span data-ttu-id="d87a4-198">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="d87a4-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="d87a4-199">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-199">Low</span></span>      |
| [<span data-ttu-id="d87a4-200">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d87a4-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="d87a4-201">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-201">Low</span></span>      |
| [<span data-ttu-id="d87a4-202">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d87a4-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="d87a4-203">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-203">Low</span></span>      |
| [<span data-ttu-id="d87a4-204">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="d87a4-205">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-205">Low</span></span>      |
| [<span data-ttu-id="d87a4-206">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="d87a4-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="d87a4-207">Małe</span><span class="sxs-lookup"><span data-stu-id="d87a4-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="d87a4-208">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="d87a4-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="d87a4-209">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Zobacz też problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="d87a4-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="d87a4-210">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-210">**Old behavior**</span></span>

<span data-ttu-id="d87a4-211">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="d87a4-212">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="d87a4-213">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-213">**New behavior**</span></span>

<span data-ttu-id="d87a4-214">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu ( `Select()` ostatnie wywołanie zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="d87a4-215">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d87a4-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="d87a4-216">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-216">**Why**</span></span>

<span data-ttu-id="d87a4-217">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="d87a4-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="d87a4-218">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d87a4-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="d87a4-219">Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych, oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="d87a4-220">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="d87a4-221">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="d87a4-222">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="d87a4-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="d87a4-223">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-223">**Mitigations**</span></span>

<span data-ttu-id="d87a4-224">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć lub użyć `AsEnumerable()`, `ToList()`lub podobnie jak jawnie przenieść dane z powrotem do klienta, na którym można następnie przetworzyć je za pomocą LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="d87a4-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="d87a4-225">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="d87a4-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="d87a4-226">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="d87a4-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="d87a4-227">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-227">**Old behavior**</span></span>

<span data-ttu-id="d87a4-228">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d87a4-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="d87a4-229">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-229">**New behavior**</span></span>

<span data-ttu-id="d87a4-230">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="d87a4-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="d87a4-231">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d87a4-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="d87a4-232">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-232">**Why**</span></span>

<span data-ttu-id="d87a4-233">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="d87a4-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="d87a4-234">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-234">**Mitigations**</span></span>

<span data-ttu-id="d87a4-235">Rozważ przeniesienie do nowoczesnej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d87a4-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="d87a4-236">Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d87a4-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="d87a4-237">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="d87a4-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="d87a4-238">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="d87a4-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="d87a4-239">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-239">**Old behavior**</span></span>

<span data-ttu-id="d87a4-240">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`, będzie ona obejmować EF Core i niektórych dostawców danych EF Core, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d87a4-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="d87a4-241">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-241">**New behavior**</span></span>

<span data-ttu-id="d87a4-242">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="d87a4-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="d87a4-243">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-243">**Why**</span></span>

<span data-ttu-id="d87a4-244">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="d87a4-245">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="d87a4-246">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="d87a4-247">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="d87a4-248">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-248">**Mitigations**</span></span>

<span data-ttu-id="d87a4-249">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="d87a4-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="d87a4-250">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="d87a4-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="d87a4-251">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="d87a4-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="d87a4-252">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-252">**Old behavior**</span></span>

<span data-ttu-id="d87a4-253">Przed 3,0 `dotnet ef` narzędzie zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="d87a4-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="d87a4-254">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-254">**New behavior**</span></span>

<span data-ttu-id="d87a4-255">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera `dotnet ef` narzędzia, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="d87a4-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="d87a4-256">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-256">**Why**</span></span>

<span data-ttu-id="d87a4-257">Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="d87a4-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="d87a4-258">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-258">**Mitigations**</span></span>

<span data-ttu-id="d87a4-259">Aby móc zarządzać migracjami lub szkieletem a `DbContext`, zainstaluj `dotnet-ef` program jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="d87a4-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="d87a4-260">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="d87a4-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="d87a4-261">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="d87a4-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="d87a4-262">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="d87a4-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="d87a4-263">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-263">**Old behavior**</span></span>

<span data-ttu-id="d87a4-264">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="d87a4-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="d87a4-265">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-265">**New behavior**</span></span>

<span data-ttu-id="d87a4-266">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`, i `ExecuteSqlRawAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="d87a4-267">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="d87a4-268">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`, i`ExecuteSqlInterpolatedAsync` , aby utworzyć zapytanie parametryczne, gdzie parametry są przenoszone w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="d87a4-269">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="d87a4-270">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="d87a4-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="d87a4-271">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-271">**Why**</span></span>

<span data-ttu-id="d87a4-272">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="d87a4-273">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="d87a4-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="d87a4-274">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-274">**Mitigations**</span></span>

<span data-ttu-id="d87a4-275">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="d87a4-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="d87a4-276">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="d87a4-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="d87a4-277">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="d87a4-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="d87a4-278">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-278">**Old behavior**</span></span>

<span data-ttu-id="d87a4-279">Przed EF Core 3,0, `FromSql` Metoda może być określona w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="d87a4-280">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-280">**New behavior**</span></span>

<span data-ttu-id="d87a4-281">Począwszy od EF Core 3,0, `FromSqlRaw` nowe i `FromSqlInterpolated` metody (zastępujące `FromSql`) można określić tylko dla katalogów głównych `DbSet<>`zapytań, tj. bezpośrednio na.</span><span class="sxs-lookup"><span data-stu-id="d87a4-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="d87a4-282">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="d87a4-283">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-283">**Why**</span></span>

<span data-ttu-id="d87a4-284">Określenie `FromSql` dowolnego miejsca, w którym `DbSet` nie ma żadnego dodanego znaczenia ani dodanej wartości, może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="d87a4-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="d87a4-285">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-285">**Mitigations**</span></span>

<span data-ttu-id="d87a4-286">`FromSql`wywołania należy przenieść, aby znajdowały się bezpośrednio w, `DbSet` do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="d87a4-287">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="d87a4-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="d87a4-288">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="d87a4-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="d87a4-289">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-289">**Old behavior**</span></span>

<span data-ttu-id="d87a4-290">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="d87a4-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="d87a4-291">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="d87a4-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="d87a4-292">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="d87a4-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="d87a4-293">zwróci to samo `Category` wystąpienie dla każdego `Product` , które jest skojarzone z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="d87a4-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="d87a4-294">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-294">**New behavior**</span></span>

<span data-ttu-id="d87a4-295">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="d87a4-296">Na przykład zapytanie powyżej zwróci teraz nowe `Category` wystąpienie dla każdego z nich `Product` , nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="d87a4-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="d87a4-297">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-297">**Why**</span></span>

<span data-ttu-id="d87a4-298">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="d87a4-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="d87a4-299">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="d87a4-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="d87a4-300">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="d87a4-301">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-301">**Mitigations**</span></span>

<span data-ttu-id="d87a4-302">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="d87a4-303">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="d87a4-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="d87a4-304">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="d87a4-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="d87a4-305">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="d87a4-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="d87a4-306">Na przykład, aby przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="d87a4-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="d87a4-307">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="d87a4-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="d87a4-308">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="d87a4-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="d87a4-309">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-309">**Old behavior**</span></span>

<span data-ttu-id="d87a4-310">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="d87a4-311">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="d87a4-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="d87a4-312">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-312">**New behavior**</span></span>

<span data-ttu-id="d87a4-313">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d87a4-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="d87a4-314">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-314">**Why**</span></span>

<span data-ttu-id="d87a4-315">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienie, jest przenoszona do innego `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="d87a4-316">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-316">**Mitigations**</span></span>

<span data-ttu-id="d87a4-317">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i `Added` należą do jednostek w stanie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="d87a4-318">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="d87a4-318">This can be avoided by:</span></span>
* <span data-ttu-id="d87a4-319">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="d87a4-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="d87a4-320">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="d87a4-321">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="d87a4-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="d87a4-322">Na przykład zwróci `context.Entry(blog).Property(e => e.Id).CurrentValue` wartość tymczasową nawet wtedy, gdy `blog.Id` sama nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="d87a4-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="d87a4-323">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="d87a4-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="d87a4-324">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="d87a4-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="d87a4-325">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-325">**Old behavior**</span></span>

<span data-ttu-id="d87a4-326">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` zostałaby śledzona `Added` w stanie i wstawiona jako nowy wiersz, gdy `SaveChanges` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="d87a4-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="d87a4-327">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-327">**New behavior**</span></span>

<span data-ttu-id="d87a4-328">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w `Modified` stanie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="d87a4-329">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po `SaveChanges` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="d87a4-330">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona tak `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="d87a4-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="d87a4-331">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-331">**Why**</span></span>

<span data-ttu-id="d87a4-332">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="d87a4-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="d87a4-333">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-333">**Mitigations**</span></span>

<span data-ttu-id="d87a4-334">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="d87a4-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="d87a4-335">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="d87a4-336">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d87a4-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="d87a4-337">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="d87a4-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="d87a4-338">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="d87a4-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="d87a4-339">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="d87a4-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="d87a4-340">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-340">**Old behavior**</span></span>

<span data-ttu-id="d87a4-341">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="d87a4-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="d87a4-342">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-342">**New behavior**</span></span>

<span data-ttu-id="d87a4-343">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="d87a4-344">Na przykład wywołanie `context.Remove()` usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane zależności są również ustawione na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="d87a4-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="d87a4-345">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-345">**Why**</span></span>

<span data-ttu-id="d87a4-346">Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="d87a4-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="d87a4-347">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-347">**Mitigations**</span></span>

<span data-ttu-id="d87a4-348">Poprzednie zachowanie można przywrócić za pomocą ustawień na stronie `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="d87a4-349">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="d87a4-350">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="d87a4-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="d87a4-351">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="d87a4-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="d87a4-352">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-352">**Old behavior**</span></span>

<span data-ttu-id="d87a4-353">Przed 3,0, `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z `Restrict` semantyką, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="d87a4-354">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-354">**New behavior**</span></span>

<span data-ttu-id="d87a4-355">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone `Restrict` z semantyką--to nie, bez kaskad; throw przy naruszeniu ograniczenia — bez również wpływu na wewnętrzną naprawę EF.</span><span class="sxs-lookup"><span data-stu-id="d87a4-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="d87a4-356">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-356">**Why**</span></span>

<span data-ttu-id="d87a4-357">Ta zmiana została wprowadzona w celu usprawnienia korzystania `DeleteBehavior` z programu w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="d87a4-358">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-358">**Mitigations**</span></span>

<span data-ttu-id="d87a4-359">Poprzednie zachowanie można przywrócić za pomocą polecenia `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="d87a4-360">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="d87a4-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="d87a4-361">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="d87a4-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="d87a4-362">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-362">**Old behavior**</span></span>

<span data-ttu-id="d87a4-363">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="d87a4-364">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="d87a4-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="d87a4-365">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-365">**New behavior**</span></span>

<span data-ttu-id="d87a4-366">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="d87a4-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="d87a4-367">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="d87a4-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="d87a4-368">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-368">**Why**</span></span>

<span data-ttu-id="d87a4-369">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="d87a4-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="d87a4-370">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="d87a4-371">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="d87a4-372">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-372">**Mitigations**</span></span>

<span data-ttu-id="d87a4-373">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="d87a4-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="d87a4-374">**`ModelBuilder.Query<>()`** — Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="d87a4-375">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="d87a4-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="d87a4-376">**`DbQuery<>`** — Zamiast `DbSet<>` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="d87a4-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="d87a4-377">**`DbContext.Query<>()`** — Zamiast `DbContext.Set<>()` tego należy używać.</span><span class="sxs-lookup"><span data-stu-id="d87a4-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="d87a4-378">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="d87a4-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="d87a4-379">[Śledzenie](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
problemów ze śledzeniem #12444[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
śledzenia problemów[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="d87a4-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="d87a4-380">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-380">**Old behavior**</span></span>

<span data-ttu-id="d87a4-381">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po `OwnsOne` wywołaniu lub. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="d87a4-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="d87a4-382">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-382">**New behavior**</span></span>

<span data-ttu-id="d87a4-383">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="d87a4-384">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="d87a4-385">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem `WithOwner()` po podobnym sposobie, jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="d87a4-386">Mimo że konfiguracja dla samego samego typu jest nadal łańcuchem `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="d87a4-387">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-387">For example:</span></span>

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

<span data-ttu-id="d87a4-388">Dodatkowo wywoływanie `Entity()`,, `Set()` lub z obiektem docelowym typu, `HasOne()`spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="d87a4-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="d87a4-389">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-389">**Why**</span></span>

<span data-ttu-id="d87a4-390">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="d87a4-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="d87a4-391">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, `HasForeignKey`takich jak.</span><span class="sxs-lookup"><span data-stu-id="d87a4-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="d87a4-392">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-392">**Mitigations**</span></span>

<span data-ttu-id="d87a4-393">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="d87a4-394">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="d87a4-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="d87a4-395">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="d87a4-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="d87a4-396">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-396">**Old behavior**</span></span>

<span data-ttu-id="d87a4-397">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="d87a4-397">Consider the following model:</span></span>
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
<span data-ttu-id="d87a4-398">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli `OrderDetails` , wystąpienie jest zawsze wymagane podczas dodawania nowego `Order`elementu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="d87a4-399">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-399">**New behavior**</span></span>

<span data-ttu-id="d87a4-400">Począwszy od 3,0, EF Core umożliwia dodanie `Order` , `OrderDetails` bez `OrderDetails` i mapuje wszystkie właściwości, z wyjątkiem klucza podstawowego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="d87a4-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="d87a4-401">Podczas wykonywania zapytania o zestawy `OrderDetails` EF Core `null` do, jeśli którekolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="d87a4-402">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-402">**Mitigations**</span></span>

<span data-ttu-id="d87a4-403">Jeśli model ma zależeć do udostępniania tabeli dla wszystkich opcjonalnych kolumn, ale Nawigacja wskazująca, że nie jest oczekiwana `null` , aplikacja powinna zostać zmodyfikowana, aby obsługiwać przypadki, gdy Nawigacja `null`jest.</span><span class="sxs-lookup"><span data-stu-id="d87a4-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="d87a4-404">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć`null` przypisaną do niej inną wartość.</span><span class="sxs-lookup"><span data-stu-id="d87a4-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="d87a4-405">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="d87a4-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="d87a4-406">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="d87a4-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="d87a4-407">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-407">**Old behavior**</span></span>

<span data-ttu-id="d87a4-408">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="d87a4-408">Consider the following model:</span></span>
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
<span data-ttu-id="d87a4-409">Przed EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zmapowana do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d87a4-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="d87a4-410">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-410">**New behavior**</span></span>

<span data-ttu-id="d87a4-411">Począwszy od 3,0, EF Core propaguje nową `Version` wartość do `Order` , jeśli jest właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="d87a4-412">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="d87a4-413">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-413">**Why**</span></span>

<span data-ttu-id="d87a4-414">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="d87a4-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="d87a4-415">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-415">**Mitigations**</span></span>

<span data-ttu-id="d87a4-416">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d87a4-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="d87a4-417">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="d87a4-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="d87a4-418">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="d87a4-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="d87a4-419">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="d87a4-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="d87a4-420">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-420">**Old behavior**</span></span>

<span data-ttu-id="d87a4-421">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="d87a4-421">Consider the following model:</span></span>
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

<span data-ttu-id="d87a4-422">Przed EF Core 3,0 `ShippingAddress` Właściwość zostałaby zamapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="d87a4-423">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-423">**New behavior**</span></span>

<span data-ttu-id="d87a4-424">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="d87a4-425">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-425">**Why**</span></span>

<span data-ttu-id="d87a4-426">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="d87a4-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="d87a4-427">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-427">**Mitigations**</span></span>

<span data-ttu-id="d87a4-428">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="d87a4-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="d87a4-429">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="d87a4-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="d87a4-430">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="d87a4-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="d87a4-431">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-431">**Old behavior**</span></span>

<span data-ttu-id="d87a4-432">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="d87a4-432">Consider the following model:</span></span>
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
<span data-ttu-id="d87a4-433">Przed EF Core 3,0, `CustomerId` właściwość będzie używana dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="d87a4-434">Jeśli `Order` jednak jest typem będącym własnością, to `CustomerId` również klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="d87a4-435">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-435">**New behavior**</span></span>

<span data-ttu-id="d87a4-436">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d87a4-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="d87a4-437">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="d87a4-438">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-438">For example:</span></span>

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

<span data-ttu-id="d87a4-439">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-439">**Why**</span></span>

<span data-ttu-id="d87a4-440">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="d87a4-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="d87a4-441">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-441">**Mitigations**</span></span>

<span data-ttu-id="d87a4-442">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="d87a4-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="d87a4-443">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="d87a4-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="d87a4-444">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="d87a4-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="d87a4-445">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-445">**Old behavior**</span></span>

<span data-ttu-id="d87a4-446">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="d87a4-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="d87a4-447">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-447">**New behavior**</span></span>

<span data-ttu-id="d87a4-448">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="d87a4-449">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-449">**Why**</span></span>

<span data-ttu-id="d87a4-450">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="d87a4-451">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="d87a4-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="d87a4-452">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-452">**Mitigations**</span></span>

<span data-ttu-id="d87a4-453">Jeśli połączenie musi pozostać otwarte jawne wywołanie w celu `OpenConnection()` upewnienia się, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="d87a4-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="d87a4-454">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="d87a4-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="d87a4-455">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="d87a4-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="d87a4-456">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-456">**Old behavior**</span></span>

<span data-ttu-id="d87a4-457">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d87a4-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="d87a4-458">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-458">**New behavior**</span></span>

<span data-ttu-id="d87a4-459">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d87a4-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="d87a4-460">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="d87a4-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="d87a4-461">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-461">**Why**</span></span>

<span data-ttu-id="d87a4-462">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d87a4-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="d87a4-463">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-463">**Mitigations**</span></span>

<span data-ttu-id="d87a4-464">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d87a4-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="d87a4-465">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="d87a4-466">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="d87a4-466">Backing fields are used by default</span></span>

[<span data-ttu-id="d87a4-467">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="d87a4-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="d87a4-468">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-468">**Old behavior**</span></span>

<span data-ttu-id="d87a4-469">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="d87a4-470">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="d87a4-471">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-471">**New behavior**</span></span>

<span data-ttu-id="d87a4-472">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="d87a4-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="d87a4-473">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="d87a4-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="d87a4-474">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-474">**Why**</span></span>

<span data-ttu-id="d87a4-475">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="d87a4-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="d87a4-476">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-476">**Mitigations**</span></span>

<span data-ttu-id="d87a4-477">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu `ModelBuilder`dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="d87a4-478">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="d87a4-479">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="d87a4-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="d87a4-480">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="d87a4-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="d87a4-481">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-481">**Old behavior**</span></span>

<span data-ttu-id="d87a4-482">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="d87a4-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="d87a4-483">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="d87a4-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="d87a4-484">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-484">**New behavior**</span></span>

<span data-ttu-id="d87a4-485">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d87a4-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="d87a4-486">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-486">**Why**</span></span>

<span data-ttu-id="d87a4-487">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="d87a4-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="d87a4-488">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-488">**Mitigations**</span></span>

<span data-ttu-id="d87a4-489">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="d87a4-490">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d87a4-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="d87a4-491">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="d87a4-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="d87a4-492">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-492">**Old behavior**</span></span>

<span data-ttu-id="d87a4-493">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="d87a4-494">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-494">**New behavior**</span></span>

<span data-ttu-id="d87a4-495">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="d87a4-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="d87a4-496">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-496">**Why**</span></span>

<span data-ttu-id="d87a4-497">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="d87a4-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="d87a4-498">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-498">**Mitigations**</span></span>

<span data-ttu-id="d87a4-499">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="d87a4-500">W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="d87a4-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="d87a4-501">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="d87a4-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="d87a4-502">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="d87a4-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="d87a4-503">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-503">**Old behavior**</span></span>

<span data-ttu-id="d87a4-504">Przed `AddDbContext` EF Core 3,0, wywoływanie `AddDbContextPool` lub zarejestrowanie usługi rejestrowania i pamięci podręcznej w usłudze D. I przez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d87a4-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="d87a4-505">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-505">**New behavior**</span></span>

<span data-ttu-id="d87a4-506">Począwszy od EF Core 3,0 `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="d87a4-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="d87a4-507">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-507">**Why**</span></span>

<span data-ttu-id="d87a4-508">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="d87a4-509">Jeśli `ILoggerFactory` jednak program jest zarejestrowany w kontenerze di aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="d87a4-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="d87a4-510">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-510">**Mitigations**</span></span>

<span data-ttu-id="d87a4-511">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d87a4-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="d87a4-512">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="d87a4-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="d87a4-513">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="d87a4-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="d87a4-514">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-514">**Old behavior**</span></span>

<span data-ttu-id="d87a4-515">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d87a4-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="d87a4-516">W ten sposób zapewniono, że stan ujawniony na `EntityEntry` bieżąco.</span><span class="sxs-lookup"><span data-stu-id="d87a4-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="d87a4-517">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-517">**New behavior**</span></span>

<span data-ttu-id="d87a4-518">Począwszy od EF Core 3,0, wywołanie `DbContext.Entry` będzie teraz podejmować próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="d87a4-519">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="d87a4-520">Należy pamiętać, `ChangeTracker.AutoDetectChangesEnabled` że jeśli jest `false` ustawiona na, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="d87a4-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="d87a4-521">Inne metody, które powodują wykrycie zmiany — na `ChangeTracker.Entries` przykład `SaveChanges`i--nadal powodują pełne `DetectChanges` wszystkie śledzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="d87a4-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="d87a4-522">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-522">**Why**</span></span>

<span data-ttu-id="d87a4-523">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania `context.Entry`z programu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="d87a4-524">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-524">**Mitigations**</span></span>

<span data-ttu-id="d87a4-525">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed `Entry` wywołaniem, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="d87a4-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="d87a4-526">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="d87a4-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="d87a4-527">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="d87a4-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="d87a4-528">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-528">**Old behavior**</span></span>

<span data-ttu-id="d87a4-529">Przed EF Core 3,0 `string` i `byte[]` właściwości klucza mogą być używane bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="d87a4-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="d87a4-530">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, Zserializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="d87a4-531">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-531">**New behavior**</span></span>

<span data-ttu-id="d87a4-532">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="d87a4-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="d87a4-533">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-533">**Why**</span></span>

<span data-ttu-id="d87a4-534">Ta zmiana została wprowadzona, ponieważ zazwyczaj wartości `string` generowane / `byte[]` przez klienta nie są przydatne, a domyślne zachowanie spowodowało trudne powody dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="d87a4-535">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-535">**Mitigations**</span></span>

<span data-ttu-id="d87a4-536">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="d87a4-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="d87a4-537">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d87a4-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="d87a4-538">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="d87a4-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="d87a4-539">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="d87a4-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="d87a4-540">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="d87a4-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="d87a4-541">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-541">**Old behavior**</span></span>

<span data-ttu-id="d87a4-542">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="d87a4-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="d87a4-543">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-543">**New behavior**</span></span>

<span data-ttu-id="d87a4-544">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="d87a4-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="d87a4-545">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-545">**Why**</span></span>

<span data-ttu-id="d87a4-546">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z `DbContext` wystąpieniem, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="d87a4-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="d87a4-547">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-547">**Mitigations**</span></span>

<span data-ttu-id="d87a4-548">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="d87a4-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="d87a4-549">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="d87a4-549">This isn't common.</span></span>
<span data-ttu-id="d87a4-550">W takich przypadkach większość elementów będzie nadal działać, ale wszystkie pojedyncze usługi, w `ILoggerFactory` zależności od tego, muszą zostać zmienione w celu `ILoggerFactory` uzyskania w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="d87a4-551">Jeśli używasz `ILoggerFactory` takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak można lepiej zrozumieć, jak nie należy ponownie rozbić w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="d87a4-552">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="d87a4-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="d87a4-553">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="d87a4-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="d87a4-554">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-554">**Old behavior**</span></span>

<span data-ttu-id="d87a4-555">Przed EF Core 3,0, gdy zostanie `DbContext` usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="d87a4-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="d87a4-556">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="d87a4-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="d87a4-557">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="d87a4-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="d87a4-558">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-558">**New behavior**</span></span>

<span data-ttu-id="d87a4-559">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="d87a4-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="d87a4-560">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="d87a4-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="d87a4-561">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="d87a4-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="d87a4-562">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="d87a4-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="d87a4-563">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-563">**Why**</span></span>

<span data-ttu-id="d87a4-564">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie przy próbie załadowania `DbContext` opóźnionego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="d87a4-565">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-565">**Mitigations**</span></span>

<span data-ttu-id="d87a4-566">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="d87a4-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="d87a4-567">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="d87a4-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="d87a4-568">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="d87a4-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="d87a4-569">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-569">**Old behavior**</span></span>

<span data-ttu-id="d87a4-570">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="d87a4-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="d87a4-571">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-571">**New behavior**</span></span>

<span data-ttu-id="d87a4-572">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d87a4-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="d87a4-573">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-573">**Why**</span></span>

<span data-ttu-id="d87a4-574">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="d87a4-575">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-575">**Mitigations**</span></span>

<span data-ttu-id="d87a4-576">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="d87a4-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="d87a4-577">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację w `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="d87a4-578">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="d87a4-579">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="d87a4-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="d87a4-580">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="d87a4-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="d87a4-581">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-581">**Old behavior**</span></span>

<span data-ttu-id="d87a4-582">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="d87a4-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="d87a4-583">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="d87a4-584">Kod wygląda tak `Samurai` , jakby odnosił się do innego typu jednostki `Entrance` przy użyciu właściwości nawigacji, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="d87a4-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="d87a4-585">W rzeczywistości ten kod próbuje utworzyć relację z typem obiektu o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="d87a4-586">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-586">**New behavior**</span></span>

<span data-ttu-id="d87a4-587">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="d87a4-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="d87a4-588">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-588">**Why**</span></span>

<span data-ttu-id="d87a4-589">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="d87a4-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="d87a4-590">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-590">**Mitigations**</span></span>

<span data-ttu-id="d87a4-591">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="d87a4-592">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-592">This is not common.</span></span>
<span data-ttu-id="d87a4-593">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="d87a4-594">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="d87a4-595">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="d87a4-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="d87a4-596">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="d87a4-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="d87a4-597">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-597">**Old behavior**</span></span>

<span data-ttu-id="d87a4-598">Następujące metody asynchroniczne zwróciły `Task<T>`wcześniej:</span><span class="sxs-lookup"><span data-stu-id="d87a4-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="d87a4-599">`ValueGenerator.NextValueAsync()`(i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="d87a4-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="d87a4-600">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-600">**New behavior**</span></span>

<span data-ttu-id="d87a4-601">Powyższe metody teraz zwracają wartość `ValueTask<T>` powyżej `T` , tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="d87a4-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="d87a4-602">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-602">**Why**</span></span>

<span data-ttu-id="d87a4-603">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="d87a4-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="d87a4-604">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-604">**Mitigations**</span></span>

<span data-ttu-id="d87a4-605">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="d87a4-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="d87a4-606">Bardziej złożone użycie (np. przekazanie zwróconej `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` przekonwertowanie na a `Task<T>` przez wywołanie `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="d87a4-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="d87a4-607">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="d87a4-608">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="d87a4-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="d87a4-609">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="d87a4-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="d87a4-610">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-610">**Old behavior**</span></span>

<span data-ttu-id="d87a4-611">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d87a4-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="d87a4-612">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-612">**New behavior**</span></span>

<span data-ttu-id="d87a4-613">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d87a4-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="d87a4-614">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-614">**Why**</span></span>

<span data-ttu-id="d87a4-615">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="d87a4-616">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-616">**Mitigations**</span></span>

<span data-ttu-id="d87a4-617">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="d87a4-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="d87a4-618">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="d87a4-619">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="d87a4-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="d87a4-620">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="d87a4-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="d87a4-621">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-621">**Old behavior**</span></span>

<span data-ttu-id="d87a4-622">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="d87a4-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="d87a4-623">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-623">**New behavior**</span></span>

<span data-ttu-id="d87a4-624">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, wywołana `ToTable()` dla typu pochodnego będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="d87a4-625">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-625">**Why**</span></span>

<span data-ttu-id="d87a4-626">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="d87a4-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="d87a4-627">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="d87a4-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="d87a4-628">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-628">**Mitigations**</span></span>

<span data-ttu-id="d87a4-629">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="d87a4-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="d87a4-630">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="d87a4-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="d87a4-631">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="d87a4-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="d87a4-632">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-632">**Old behavior**</span></span>

<span data-ttu-id="d87a4-633">Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnia sposób konfigurowania kolumn używanych w programie. `INCLUDE`</span><span class="sxs-lookup"><span data-stu-id="d87a4-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="d87a4-634">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-634">**New behavior**</span></span>

<span data-ttu-id="d87a4-635">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="d87a4-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="d87a4-636">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="d87a4-637">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-637">**Why**</span></span>

<span data-ttu-id="d87a4-638">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="d87a4-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="d87a4-639">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-639">**Mitigations**</span></span>

<span data-ttu-id="d87a4-640">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="d87a4-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="d87a4-641">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="d87a4-641">Metadata API changes</span></span>

[<span data-ttu-id="d87a4-642">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="d87a4-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="d87a4-643">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-643">**New behavior**</span></span>

<span data-ttu-id="d87a4-644">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="d87a4-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="d87a4-645">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-645">**Why**</span></span>

<span data-ttu-id="d87a4-646">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="d87a4-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="d87a4-647">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-647">**Mitigations**</span></span>

<span data-ttu-id="d87a4-648">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="d87a4-649">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="d87a4-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="d87a4-650">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="d87a4-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="d87a4-651">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-651">**New behavior**</span></span>

<span data-ttu-id="d87a4-652">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="d87a4-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="d87a4-653">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-653">**Why**</span></span>

<span data-ttu-id="d87a4-654">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="d87a4-655">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-655">**Mitigations**</span></span>

<span data-ttu-id="d87a4-656">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="d87a4-657">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="d87a4-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="d87a4-658">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="d87a4-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="d87a4-659">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-659">**Old behavior**</span></span>

<span data-ttu-id="d87a4-660">Przed EF Core 3,0, EF Core wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z systemem SQLite.</span><span class="sxs-lookup"><span data-stu-id="d87a4-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d87a4-661">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-661">**New behavior**</span></span>

<span data-ttu-id="d87a4-662">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` , gdy zostanie otwarte połączenie z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="d87a4-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d87a4-663">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-663">**Why**</span></span>

<span data-ttu-id="d87a4-664">Ta zmiana została wprowadzona, ponieważ domyślnie `SQLitePCLRaw.bundle_e_sqlite3` używa EF Core, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="d87a4-665">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-665">**Mitigations**</span></span>

<span data-ttu-id="d87a4-666">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="d87a4-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="d87a4-667">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="d87a4-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="d87a4-668">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="d87a4-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="d87a4-669">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-669">**Old behavior**</span></span>

<span data-ttu-id="d87a4-670">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="d87a4-671">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-671">**New behavior**</span></span>

<span data-ttu-id="d87a4-672">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="d87a4-673">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-673">**Why**</span></span>

<span data-ttu-id="d87a4-674">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="d87a4-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="d87a4-675">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-675">**Mitigations**</span></span>

<span data-ttu-id="d87a4-676">Aby korzystać z natywnej wersji programu SQLite w systemie `Microsoft.Data.Sqlite` iOS, należy skonfigurować `SQLitePCLRaw` do korzystania z innego pakietu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d87a4-677">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="d87a4-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d87a4-678">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="d87a4-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="d87a4-679">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-679">**Old behavior**</span></span>

<span data-ttu-id="d87a4-680">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="d87a4-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="d87a4-681">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-681">**New behavior**</span></span>

<span data-ttu-id="d87a4-682">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="d87a4-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="d87a4-683">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-683">**Why**</span></span>

<span data-ttu-id="d87a4-684">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="d87a4-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="d87a4-685">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="d87a4-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d87a4-686">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-686">**Mitigations**</span></span>

<span data-ttu-id="d87a4-687">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="d87a4-688">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="d87a4-689">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="d87a4-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d87a4-690">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="d87a4-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d87a4-691">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="d87a4-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="d87a4-692">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-692">**Old behavior**</span></span>

<span data-ttu-id="d87a4-693">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="d87a4-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="d87a4-694">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="d87a4-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="d87a4-695">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-695">**New behavior**</span></span>

<span data-ttu-id="d87a4-696">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="d87a4-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="d87a4-697">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-697">**Why**</span></span>

<span data-ttu-id="d87a4-698">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="d87a4-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d87a4-699">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-699">**Mitigations**</span></span>

<span data-ttu-id="d87a4-700">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="d87a4-701">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="d87a4-702">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="d87a4-703">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="d87a4-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="d87a4-704">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="d87a4-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="d87a4-705">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-705">**Old behavior**</span></span>

<span data-ttu-id="d87a4-706">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="d87a4-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="d87a4-707">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-707">**New behavior**</span></span>

<span data-ttu-id="d87a4-708">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="d87a4-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="d87a4-709">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-709">**Why**</span></span>

<span data-ttu-id="d87a4-710">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="d87a4-711">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="d87a4-712">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-712">**Mitigations**</span></span>

<span data-ttu-id="d87a4-713">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="d87a4-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="d87a4-714">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="d87a4-715">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="d87a4-716">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="d87a4-717">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="d87a4-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="d87a4-718">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="d87a4-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="d87a4-719">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-719">**Old behavior**</span></span>

<span data-ttu-id="d87a4-720">Przed EF Core 3,0 `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="d87a4-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="d87a4-721">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-721">**New behavior**</span></span>

<span data-ttu-id="d87a4-722">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d87a4-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="d87a4-723">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-723">**Why**</span></span>

<span data-ttu-id="d87a4-724">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="d87a4-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="d87a4-725">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-725">**Mitigations**</span></span>

<span data-ttu-id="d87a4-726">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="d87a4-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="d87a4-727">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="d87a4-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="d87a4-728">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="d87a4-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="d87a4-729">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="d87a4-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="d87a4-730">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="d87a4-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="d87a4-731">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-731">**Old behavior**</span></span>

<span data-ttu-id="d87a4-732">`IDbContextOptionsExtension`zawiera metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="d87a4-733">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-733">**New behavior**</span></span>

<span data-ttu-id="d87a4-734">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="d87a4-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="d87a4-735">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-735">**Why**</span></span>

<span data-ttu-id="d87a4-736">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="d87a4-737">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="d87a4-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="d87a4-738">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-738">**Mitigations**</span></span>

<span data-ttu-id="d87a4-739">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="d87a4-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="d87a4-740">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="d87a4-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="d87a4-741">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="d87a4-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="d87a4-742">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="d87a4-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="d87a4-743">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="d87a4-743">**Change**</span></span>

<span data-ttu-id="d87a4-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="d87a4-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="d87a4-745">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-745">**Why**</span></span>

<span data-ttu-id="d87a4-746">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="d87a4-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="d87a4-747">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-747">**Mitigations**</span></span>

<span data-ttu-id="d87a4-748">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-748">Use the new name.</span></span> <span data-ttu-id="d87a4-749">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="d87a4-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="d87a4-750">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="d87a4-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="d87a4-751">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="d87a4-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="d87a4-752">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-752">**Old behavior**</span></span>

<span data-ttu-id="d87a4-753">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="d87a4-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="d87a4-754">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="d87a4-755">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-755">**New behavior**</span></span>

<span data-ttu-id="d87a4-756">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="d87a4-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="d87a4-757">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="d87a4-758">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-758">**Why**</span></span>

<span data-ttu-id="d87a4-759">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="d87a4-760">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-760">**Mitigations**</span></span>

<span data-ttu-id="d87a4-761">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="d87a4-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="d87a4-762">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="d87a4-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="d87a4-763">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="d87a4-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="d87a4-764">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-764">**Old behavior**</span></span>

<span data-ttu-id="d87a4-765">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="d87a4-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="d87a4-766">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-766">**New behavior**</span></span>

<span data-ttu-id="d87a4-767">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="d87a4-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="d87a4-768">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-768">**Why**</span></span>

<span data-ttu-id="d87a4-769">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="d87a4-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="d87a4-770">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="d87a4-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="d87a4-771">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-771">**Mitigations**</span></span>

<span data-ttu-id="d87a4-772">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="d87a4-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="d87a4-773">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="d87a4-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="d87a4-774">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="d87a4-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="d87a4-775">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-775">**Old behavior**</span></span>

<span data-ttu-id="d87a4-776">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="d87a4-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="d87a4-777">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-777">**New behavior**</span></span>

<span data-ttu-id="d87a4-778">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="d87a4-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="d87a4-779">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="d87a4-780">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-780">**Why**</span></span>

<span data-ttu-id="d87a4-781">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="d87a4-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="d87a4-782">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="d87a4-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="d87a4-783">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="d87a4-784">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-784">**Mitigations**</span></span>

<span data-ttu-id="d87a4-785">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="d87a4-786">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="d87a4-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="d87a4-787">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d87a4-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="d87a4-788">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="d87a4-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="d87a4-789">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-789">**Old behavior**</span></span>

<span data-ttu-id="d87a4-790">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="d87a4-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="d87a4-791">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-791">**New behavior**</span></span>

<span data-ttu-id="d87a4-792">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="d87a4-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="d87a4-793">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-793">**Why**</span></span>

<span data-ttu-id="d87a4-794">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="d87a4-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="d87a4-795">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="d87a4-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="d87a4-796">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-796">**Mitigations**</span></span>

<span data-ttu-id="d87a4-797">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="d87a4-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="d87a4-798">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="d87a4-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="d87a4-799">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d87a4-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="d87a4-800">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="d87a4-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="d87a4-801">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-801">**Old behavior**</span></span>

<span data-ttu-id="d87a4-802">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="d87a4-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="d87a4-803">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-803">**New behavior**</span></span>

<span data-ttu-id="d87a4-804">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="d87a4-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="d87a4-805">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-805">**Why**</span></span>

<span data-ttu-id="d87a4-806">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d87a4-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="d87a4-807">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-807">**Mitigations**</span></span>

<span data-ttu-id="d87a4-808">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="d87a4-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="d87a4-809">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="d87a4-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="d87a4-810">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="d87a4-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="d87a4-811">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="d87a4-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="d87a4-812">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-812">**Old behavior**</span></span>

<span data-ttu-id="d87a4-813">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="d87a4-814">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-814">For example:</span></span>

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

<span data-ttu-id="d87a4-815">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-815">**New behavior**</span></span>

<span data-ttu-id="d87a4-816">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="d87a4-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="d87a4-817">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-817">**Why**</span></span>

<span data-ttu-id="d87a4-818">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="d87a4-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="d87a4-819">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-819">**Mitigations**</span></span>

<span data-ttu-id="d87a4-820">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="d87a4-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="d87a4-821">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d87a4-821">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="d87a4-822">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="d87a4-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="d87a4-823">Śledzenie problemu #12757</span><span class="sxs-lookup"><span data-stu-id="d87a4-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="d87a4-824">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-824">**Old behavior**</span></span>

<span data-ttu-id="d87a4-825">Funkcja dbzostała skonfigurowana ze schematem jako pusty ciąg był traktowany jak wbudowana funkcja bez schematu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="d87a4-826">Na przykład poniższy kod mapuje `DatePart` funkcję CLR na `DATEPART` wbudowaną funkcję na serwerze SqlServer.</span><span class="sxs-lookup"><span data-stu-id="d87a4-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="d87a4-827">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="d87a4-827">**New behavior**</span></span>

<span data-ttu-id="d87a4-828">Wszystkie mapowania funkcji dbfunction są uznawane za mapowane do funkcji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d87a4-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="d87a4-829">W związku z tym wartość pustego ciągu spowodowałaby umieszczenie funkcji wewnątrz domyślnego schematu modelu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="d87a4-830">Może to być schemat skonfigurowany jawnie za pośrednictwem interfejsu `modelBuilder.HasDefaultSchema()` API `dbo` Fluent lub w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="d87a4-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="d87a4-831">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="d87a4-831">**Why**</span></span>

<span data-ttu-id="d87a4-832">Poprzednio pusty schemat był sposobem traktowania tej funkcji, ale ta logika jest stosowana tylko dla SqlServer, w której wbudowane funkcje nie należą do żadnego schematu.</span><span class="sxs-lookup"><span data-stu-id="d87a4-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="d87a4-833">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="d87a4-833">**Mitigations**</span></span>

<span data-ttu-id="d87a4-834">Skonfiguruj ręcznie tłumaczenie funkcji dbfunction, aby zamapować ją na wbudowaną funkcję.</span><span class="sxs-lookup"><span data-stu-id="d87a4-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
