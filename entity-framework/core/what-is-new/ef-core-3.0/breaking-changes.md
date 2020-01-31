---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888112"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="35018-102">Istotne zmiany zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="35018-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="35018-103">Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="35018-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="35018-104">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="35018-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="35018-105">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="35018-105">Summary</span></span>

| <span data-ttu-id="35018-106">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="35018-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="35018-107">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="35018-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="35018-108">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="35018-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="35018-109">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-109">High</span></span>       |
| [<span data-ttu-id="35018-110">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="35018-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="35018-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-111">High</span></span>      |
| [<span data-ttu-id="35018-112">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="35018-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="35018-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-113">High</span></span>      |
| [<span data-ttu-id="35018-114">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="35018-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="35018-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-115">High</span></span>      |
| [<span data-ttu-id="35018-116">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="35018-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="35018-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-117">High</span></span>      |
| [<span data-ttu-id="35018-118">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="35018-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="35018-119">Wysoka</span><span class="sxs-lookup"><span data-stu-id="35018-119">High</span></span>      |
| [<span data-ttu-id="35018-120">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="35018-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="35018-121">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-121">Medium</span></span>      |
| [<span data-ttu-id="35018-122">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="35018-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="35018-123">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-123">Medium</span></span>      |
| [<span data-ttu-id="35018-124">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="35018-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="35018-125">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-125">Medium</span></span>      |
| [<span data-ttu-id="35018-126">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="35018-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="35018-127">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-127">Medium</span></span>      |
| [<span data-ttu-id="35018-128">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="35018-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="35018-129">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-129">Medium</span></span>      |
| [<span data-ttu-id="35018-130">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="35018-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="35018-131">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-131">Medium</span></span>      |
| [<span data-ttu-id="35018-132">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="35018-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="35018-133">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-133">Medium</span></span>      |
| [<span data-ttu-id="35018-134">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="35018-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="35018-135">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-135">Medium</span></span>      |
| [<span data-ttu-id="35018-136">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="35018-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="35018-137">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-137">Medium</span></span>      |
| [<span data-ttu-id="35018-138">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="35018-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="35018-139">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-139">Medium</span></span>      |
| [<span data-ttu-id="35018-140">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="35018-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="35018-141">Średnia</span><span class="sxs-lookup"><span data-stu-id="35018-141">Medium</span></span>      |
| [<span data-ttu-id="35018-142">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="35018-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="35018-143">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-143">Low</span></span>      |
| [<span data-ttu-id="35018-144">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="35018-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="35018-145">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-145">Low</span></span>      |
| [<span data-ttu-id="35018-146">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="35018-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="35018-147">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-147">Low</span></span>      |
| [<span data-ttu-id="35018-148">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="35018-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="35018-149">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-149">Low</span></span>      |
| [<span data-ttu-id="35018-150">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="35018-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="35018-151">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-151">Low</span></span>      |
| [<span data-ttu-id="35018-152">Nie można wykonać zapytania dotyczącego jednostek będących własnością, bez właściciela przy użyciu zapytania śledzenia</span><span class="sxs-lookup"><span data-stu-id="35018-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="35018-153">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-153">Low</span></span>      |
| [<span data-ttu-id="35018-154">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="35018-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="35018-155">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-155">Low</span></span>      |
| [<span data-ttu-id="35018-156">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="35018-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="35018-157">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-157">Low</span></span>      |
| [<span data-ttu-id="35018-158">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="35018-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="35018-159">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-159">Low</span></span>      |
| [<span data-ttu-id="35018-160">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="35018-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="35018-161">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-161">Low</span></span>      |
| [<span data-ttu-id="35018-162">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="35018-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="35018-163">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-163">Low</span></span>      |
| [<span data-ttu-id="35018-164">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="35018-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="35018-165">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-165">Low</span></span>      |
| [<span data-ttu-id="35018-166">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="35018-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="35018-167">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-167">Low</span></span>      |
| [<span data-ttu-id="35018-168">AddEntityFramework \* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="35018-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="35018-169">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-169">Low</span></span>      |
| [<span data-ttu-id="35018-170">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="35018-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="35018-171">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-171">Low</span></span>      |
| [<span data-ttu-id="35018-172">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="35018-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="35018-173">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-173">Low</span></span>      |
| [<span data-ttu-id="35018-174">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="35018-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="35018-175">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-175">Low</span></span>      |
| [<span data-ttu-id="35018-176">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="35018-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="35018-177">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-177">Low</span></span>      |
| [<span data-ttu-id="35018-178">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="35018-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="35018-179">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-179">Low</span></span>      |
| [<span data-ttu-id="35018-180">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="35018-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="35018-181">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-181">Low</span></span>      |
| [<span data-ttu-id="35018-182">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="35018-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="35018-183">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-183">Low</span></span>      |
| [<span data-ttu-id="35018-184">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="35018-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="35018-185">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-185">Low</span></span>      |
| [<span data-ttu-id="35018-186">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="35018-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="35018-187">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-187">Low</span></span>      |
| [<span data-ttu-id="35018-188">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="35018-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="35018-189">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-189">Low</span></span>      |
| [<span data-ttu-id="35018-190">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="35018-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="35018-191">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-191">Low</span></span>      |
| [<span data-ttu-id="35018-192">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="35018-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="35018-193">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-193">Low</span></span>      |
| [<span data-ttu-id="35018-194">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="35018-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="35018-195">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-195">Low</span></span>      |
| [<span data-ttu-id="35018-196">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="35018-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="35018-197">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-197">Low</span></span>      |
| [<span data-ttu-id="35018-198">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="35018-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="35018-199">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-199">Low</span></span>      |
| [<span data-ttu-id="35018-200">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="35018-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="35018-201">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-201">Low</span></span>      |
| [<span data-ttu-id="35018-202">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="35018-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="35018-203">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-203">Low</span></span>      |
| [<span data-ttu-id="35018-204">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="35018-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="35018-205">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-205">Low</span></span>      |
| [<span data-ttu-id="35018-206">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="35018-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="35018-207">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-207">Low</span></span>      |
| [<span data-ttu-id="35018-208">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="35018-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="35018-209">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-209">Low</span></span>      |
| [<span data-ttu-id="35018-210">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="35018-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="35018-211">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-211">Low</span></span>      |
| [<span data-ttu-id="35018-212">Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="35018-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="35018-213">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-213">Low</span></span>      |
| [<span data-ttu-id="35018-214">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="35018-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="35018-215">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-215">Low</span></span>      |
| [<span data-ttu-id="35018-216">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="35018-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="35018-217">Niski</span><span class="sxs-lookup"><span data-stu-id="35018-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="35018-218">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="35018-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="35018-219">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[zobacz również problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="35018-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="35018-220">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-220">**Old behavior**</span></span>

<span data-ttu-id="35018-221">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="35018-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="35018-222">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="35018-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="35018-223">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-223">**New behavior**</span></span>

<span data-ttu-id="35018-224">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołaniu zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="35018-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="35018-225">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="35018-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="35018-226">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-226">**Why**</span></span>

<span data-ttu-id="35018-227">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="35018-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="35018-228">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="35018-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="35018-229">Na przykład warunek w wywołaniu `Where()`, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="35018-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="35018-230">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="35018-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="35018-231">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="35018-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="35018-232">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="35018-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="35018-233">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-233">**Mitigations**</span></span>

<span data-ttu-id="35018-234">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć, lub użyć `AsEnumerable()`, `ToList()`lub podobnego do jawnego przywrócenia danych z powrotem do klienta, na którym można następnie przetworzyć je przy użyciu LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="35018-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="35018-235">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="35018-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="35018-236">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="35018-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="35018-237">Ponownie EF Core 3,1 cele .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="35018-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="35018-238">Spowoduje to przywrócenie obsługi .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="35018-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="35018-239">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-239">**Old behavior**</span></span>

<span data-ttu-id="35018-240">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="35018-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="35018-241">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-241">**New behavior**</span></span>

<span data-ttu-id="35018-242">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="35018-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="35018-243">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="35018-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="35018-244">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-244">**Why**</span></span>

<span data-ttu-id="35018-245">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="35018-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="35018-246">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-246">**Mitigations**</span></span>

<span data-ttu-id="35018-247">Użyj EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="35018-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="35018-248">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="35018-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="35018-249">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="35018-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="35018-250">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-250">**Old behavior**</span></span>

<span data-ttu-id="35018-251">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`będzie ona obejmować EF Core i niektórych spośród EF Core dostawców danych, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="35018-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="35018-252">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-252">**New behavior**</span></span>

<span data-ttu-id="35018-253">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="35018-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="35018-254">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-254">**Why**</span></span>

<span data-ttu-id="35018-255">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="35018-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="35018-256">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="35018-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="35018-257">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35018-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="35018-258">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="35018-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="35018-259">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-259">**Mitigations**</span></span>

<span data-ttu-id="35018-260">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="35018-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="35018-261">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="35018-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="35018-262">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="35018-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="35018-263">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-263">**Old behavior**</span></span>

<span data-ttu-id="35018-264">Przed 3,0 Narzędzie `dotnet ef` zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="35018-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="35018-265">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-265">**New behavior**</span></span>

<span data-ttu-id="35018-266">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera narzędzia `dotnet ef`, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="35018-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="35018-267">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-267">**Why**</span></span>

<span data-ttu-id="35018-268">Ta zmiana umożliwia nam dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="35018-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="35018-269">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-269">**Mitigations**</span></span>

<span data-ttu-id="35018-270">Aby móc zarządzać migracjami lub szkieletem `DbContext`, zainstaluj `dotnet-ef` jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="35018-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="35018-271">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="35018-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="35018-272">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="35018-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="35018-273">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="35018-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="35018-274">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-274">**Old behavior**</span></span>

<span data-ttu-id="35018-275">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="35018-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="35018-276">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-276">**New behavior**</span></span>

<span data-ttu-id="35018-277">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`i `ExecuteSqlRawAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="35018-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="35018-278">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="35018-279">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`i `ExecuteSqlInterpolatedAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="35018-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="35018-280">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="35018-281">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="35018-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="35018-282">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-282">**Why**</span></span>

<span data-ttu-id="35018-283">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="35018-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="35018-284">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="35018-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="35018-285">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-285">**Mitigations**</span></span>

<span data-ttu-id="35018-286">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="35018-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="35018-287">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="35018-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="35018-288">Śledzenie problemu #15392</span><span class="sxs-lookup"><span data-stu-id="35018-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="35018-289">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-289">**Old behavior**</span></span>

<span data-ttu-id="35018-290">Przed EF Core 3,0, Metoda Z tabel podjęła próbę wykrycia, czy może być złożona pozostała wartość SQL.</span><span class="sxs-lookup"><span data-stu-id="35018-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="35018-291">Klient przeprowadził ocenę, gdy nie udało się utworzyć kodu SQL, podobnie jak procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="35018-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="35018-292">Następujące zapytanie działało przez uruchomienie procedury składowanej na serwerze i wykonanie FirstOrDefault po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="35018-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="35018-293">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-293">**New behavior**</span></span>

<span data-ttu-id="35018-294">Począwszy od EF Core 3,0, EF Core nie będzie próbować przeanalizować bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="35018-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="35018-295">Dlatego w przypadku redagowania po FromSqlRaw/FromSqlInterpolated, EF Core nastąpi złożenie zapytania podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="35018-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="35018-296">Dlatego jeśli używasz procedury składowanej z kompozycją, zostanie pobrany wyjątek dla nieprawidłowej składni SQL.</span><span class="sxs-lookup"><span data-stu-id="35018-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="35018-297">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-297">**Why**</span></span>

<span data-ttu-id="35018-298">EF Core 3,0 nie obsługuje automatycznej oceny klienta, ponieważ została ona podatna na błędy, jak wyjaśniono [tutaj](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="35018-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="35018-299">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-299">**Mitigation**</span></span>

<span data-ttu-id="35018-300">Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można jej składować, więc możesz dodać __AsEnumerable/AsAsyncEnumerable__ bezpośrednio po wywołaniu metody z tabel, aby uniknąć jakiegokolwiek złożenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="35018-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="35018-301">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="35018-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="35018-302">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="35018-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="35018-303">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-303">**Old behavior**</span></span>

<span data-ttu-id="35018-304">Przed EF Core 3,0 można określić metodę `FromSql` w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="35018-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="35018-305">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-305">**New behavior**</span></span>

<span data-ttu-id="35018-306">Począwszy od EF Core 3,0, nowe metody `FromSqlRaw` i `FromSqlInterpolated` (które zastępują `FromSql`) można określić tylko dla katalogów głównych zapytań, tj. bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="35018-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="35018-307">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="35018-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="35018-308">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-308">**Why**</span></span>

<span data-ttu-id="35018-309">Określanie `FromSql` w dowolnym miejscu niż na `DbSet` nie miało znaczenia ani dodanej wartości i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="35018-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="35018-310">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-310">**Mitigations**</span></span>

<span data-ttu-id="35018-311">wywołania `FromSql` powinny zostać przeniesione bezpośrednio na `DbSet`, do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="35018-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="35018-312">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="35018-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="35018-313">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="35018-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="35018-314">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-314">**Old behavior**</span></span>

<span data-ttu-id="35018-315">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="35018-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="35018-316">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="35018-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="35018-317">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="35018-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="35018-318">zwróci to samo wystąpienie `Category` dla każdej `Product` skojarzonej z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="35018-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="35018-319">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-319">**New behavior**</span></span>

<span data-ttu-id="35018-320">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="35018-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="35018-321">Na przykład zapytanie powyżej zwróci teraz nowe wystąpienie `Category` dla każdej `Product`, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="35018-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="35018-322">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-322">**Why**</span></span>

<span data-ttu-id="35018-323">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="35018-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="35018-324">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="35018-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="35018-325">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="35018-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="35018-326">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-326">**Mitigations**</span></span>

<span data-ttu-id="35018-327">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="35018-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="35018-328">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="35018-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="35018-329">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="35018-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="35018-330">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="35018-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="35018-331">Aby na przykład przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="35018-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="35018-332">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="35018-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="35018-333">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="35018-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="35018-334">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-334">**Old behavior**</span></span>

<span data-ttu-id="35018-335">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="35018-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="35018-336">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="35018-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="35018-337">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-337">**New behavior**</span></span>

<span data-ttu-id="35018-338">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="35018-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="35018-339">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-339">**Why**</span></span>

<span data-ttu-id="35018-340">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości klucza tymczasowego, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienia, zostanie przeniesiona do innego wystąpienia `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="35018-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="35018-341">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-341">**Mitigations**</span></span>

<span data-ttu-id="35018-342">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w stanie `Added`.</span><span class="sxs-lookup"><span data-stu-id="35018-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="35018-343">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="35018-343">This can be avoided by:</span></span>
* <span data-ttu-id="35018-344">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="35018-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="35018-345">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="35018-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="35018-346">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="35018-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="35018-347">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową, nawet jeśli nie został ustawiony `blog.Id` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="35018-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="35018-348">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="35018-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="35018-349">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="35018-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="35018-350">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-350">**Old behavior**</span></span>

<span data-ttu-id="35018-351">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` byłaby śledzona w `Added` stanie i wstawiona jako nowy wiersz po wywołaniu `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="35018-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="35018-352">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-352">**New behavior**</span></span>

<span data-ttu-id="35018-353">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w stanie `Modified`.</span><span class="sxs-lookup"><span data-stu-id="35018-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="35018-354">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po wywołaniu `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="35018-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="35018-355">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona jako `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="35018-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="35018-356">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-356">**Why**</span></span>

<span data-ttu-id="35018-357">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="35018-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="35018-358">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-358">**Mitigations**</span></span>

<span data-ttu-id="35018-359">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="35018-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="35018-360">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="35018-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="35018-361">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="35018-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="35018-362">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="35018-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="35018-363">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="35018-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="35018-364">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="35018-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="35018-365">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-365">**Old behavior**</span></span>

<span data-ttu-id="35018-366">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="35018-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="35018-367">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-367">**New behavior**</span></span>

<span data-ttu-id="35018-368">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="35018-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="35018-369">Na przykład wywołanie `context.Remove()` w celu usunięcia podmiotu zabezpieczeń spowoduje również, że wszystkie śledzone powiązane wymagane elementy zależne są ustawiane na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="35018-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="35018-370">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-370">**Why**</span></span>

<span data-ttu-id="35018-371">Ta zmiana została wprowadzona w celu poprawy środowiska dla scenariuszy powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ wywołaniem `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="35018-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="35018-372">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-372">**Mitigations**</span></span>

<span data-ttu-id="35018-373">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="35018-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="35018-374">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="35018-375">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="35018-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="35018-376">Śledzenie problemu #18022</span><span class="sxs-lookup"><span data-stu-id="35018-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="35018-377">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-377">**Old behavior**</span></span>

<span data-ttu-id="35018-378">Przed 3,0 eagerly załadowanie nawigacji kolekcji za pośrednictwem operatorów `Include` spowodowało generowanie wielu zapytań w relacyjnej bazie danych, po jednej dla każdego powiązanego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="35018-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="35018-379">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-379">**New behavior**</span></span>

<span data-ttu-id="35018-380">Począwszy od 3,0, EF Core generuje pojedyncze zapytanie z sprzężeniami w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="35018-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="35018-381">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-381">**Why**</span></span>

<span data-ttu-id="35018-382">Wygenerowanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ powodowało wiele problemów, w tym negatywną wydajność, ponieważ wiele danych jest niezbędnych, a błędy spójność danych, ponieważ każde zapytanie może obserwować inny stan bazy danych.</span><span class="sxs-lookup"><span data-stu-id="35018-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="35018-383">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-383">**Mitigations**</span></span>

<span data-ttu-id="35018-384">Chociaż technicznie nie jest to istotna zmiana, może ona mieć znaczny wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę `Include` operatora na nawigowaniu po kolekcji.</span><span class="sxs-lookup"><span data-stu-id="35018-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="35018-385">[Zobacz ten komentarz](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , aby uzyskać więcej informacji i przepisać zapytania w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="35018-386">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="35018-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="35018-387">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="35018-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="35018-388">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-388">**Old behavior**</span></span>

<span data-ttu-id="35018-389">Przed 3,0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych z semantyką `Restrict`, ale również zmieniono wewnętrzną korektę w niewidoczny sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="35018-390">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-390">**New behavior**</span></span>

<span data-ttu-id="35018-391">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone z semantyką `Restrict`, czyli bez kaskad; Zgłoś naruszenie ograniczenia — bez również wpływu na wewnętrzną korektę EF.</span><span class="sxs-lookup"><span data-stu-id="35018-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="35018-392">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-392">**Why**</span></span>

<span data-ttu-id="35018-393">Ta zmiana została wprowadzona w celu poprawy środowiska korzystania z `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="35018-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="35018-394">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-394">**Mitigations**</span></span>

<span data-ttu-id="35018-395">Poprzednie zachowanie można przywrócić przy użyciu `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="35018-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="35018-396">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="35018-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="35018-397">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="35018-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="35018-398">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-398">**Old behavior**</span></span>

<span data-ttu-id="35018-399">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="35018-400">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="35018-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="35018-401">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-401">**New behavior**</span></span>

<span data-ttu-id="35018-402">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="35018-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="35018-403">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="35018-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="35018-404">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-404">**Why**</span></span>

<span data-ttu-id="35018-405">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="35018-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="35018-406">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="35018-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="35018-407">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="35018-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="35018-408">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-408">**Mitigations**</span></span>

<span data-ttu-id="35018-409">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="35018-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="35018-410">należy wywołać `ModelBuilder.Entity<>().HasNoKey()` **`ModelBuilder.Query<>()`** , aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="35018-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="35018-411">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="35018-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="35018-412">należy użyć `DbSet<>` **`DbQuery<>`** .</span><span class="sxs-lookup"><span data-stu-id="35018-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="35018-413">należy użyć `DbContext.Set<>()` **`DbContext.Query<>()`** .</span><span class="sxs-lookup"><span data-stu-id="35018-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="35018-414">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="35018-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="35018-415">Problem ze śledzeniem [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
śledzenia [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148) problem ze [śledzeniem
#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="35018-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="35018-416">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-416">**Old behavior**</span></span>

<span data-ttu-id="35018-417">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po wywołaniu `OwnsOne` lub `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="35018-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="35018-418">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-418">**New behavior**</span></span>

<span data-ttu-id="35018-419">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="35018-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="35018-420">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="35018-421">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem po `WithOwner()` podobnie jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="35018-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="35018-422">Mimo że konfiguracja dla samego typu, nadal będzie łańcucha po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="35018-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="35018-423">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-423">For example:</span></span>

```csharp
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

<span data-ttu-id="35018-424">Dodatkowo wywoływanie `Entity()`, `HasOne()`lub `Set()` z elementem docelowym typu będącego własnością spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="35018-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="35018-425">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-425">**Why**</span></span>

<span data-ttu-id="35018-426">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="35018-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="35018-427">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="35018-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="35018-428">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-428">**Mitigations**</span></span>

<span data-ttu-id="35018-429">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="35018-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="35018-430">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="35018-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="35018-431">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="35018-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="35018-432">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-432">**Old behavior**</span></span>

<span data-ttu-id="35018-433">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="35018-433">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="35018-434">Przed EF Core 3,0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zamapowane do tej samej tabeli, wystąpienie `OrderDetails` było zawsze wymagane podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="35018-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="35018-435">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-435">**New behavior**</span></span>

<span data-ttu-id="35018-436">Począwszy od 3,0, EF Core umożliwia dodanie `Order` bez `OrderDetails` i mapuje wszystkie właściwości `OrderDetails` z wyjątkiem klucza podstawowego na kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="35018-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="35018-437">Podczas wykonywania zapytania o EF Core zestawy `OrderDetails` do `null`, jeśli żadna z jej wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="35018-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="35018-438">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-438">**Mitigations**</span></span>

<span data-ttu-id="35018-439">Jeśli Twój model ma zależne od siebie wszystkie opcjonalne kolumny, ale nawigacja nie powinna być `null`, należy ją zmodyfikować, aby obsługiwać przypadki, gdy nawigacja jest `null`.</span><span class="sxs-lookup"><span data-stu-id="35018-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="35018-440">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć przypisaną wartość inną niż`null`.</span><span class="sxs-lookup"><span data-stu-id="35018-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="35018-441">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="35018-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="35018-442">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="35018-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="35018-443">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-443">**Old behavior**</span></span>

<span data-ttu-id="35018-444">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="35018-444">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="35018-445">Przed EF Core 3,0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zamapowane do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji wartości `Version` na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="35018-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="35018-446">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-446">**New behavior**</span></span>

<span data-ttu-id="35018-447">Począwszy od 3,0, EF Core propaguje nową wartość `Version` do `Order`, jeśli należy ona do `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="35018-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="35018-448">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="35018-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="35018-449">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-449">**Why**</span></span>

<span data-ttu-id="35018-450">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="35018-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="35018-451">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-451">**Mitigations**</span></span>

<span data-ttu-id="35018-452">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="35018-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="35018-453">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="35018-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="35018-454">Nie można wykonać zapytania dotyczącego jednostek będących własnością, bez właściciela przy użyciu zapytania śledzenia</span><span class="sxs-lookup"><span data-stu-id="35018-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="35018-455">Śledzenie problemu #18876</span><span class="sxs-lookup"><span data-stu-id="35018-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="35018-456">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-456">**Old behavior**</span></span>

<span data-ttu-id="35018-457">Przed EF Core 3,0 jednostki będące własnością mogą być badane jako każda inna nawigacja.</span><span class="sxs-lookup"><span data-stu-id="35018-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="35018-458">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-458">**New behavior**</span></span>

<span data-ttu-id="35018-459">Począwszy od 3,0, EF Core zostanie zgłoszony, jeśli zapytanie śledzenia projektuje jednostkę będącą własnością, bez właściciela.</span><span class="sxs-lookup"><span data-stu-id="35018-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="35018-460">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-460">**Why**</span></span>

<span data-ttu-id="35018-461">Elementy należące do użytkownika nie mogą być manipulowane bez właściciela, więc w większości przypadków, w których zapytania są w ten sposób, są błędem.</span><span class="sxs-lookup"><span data-stu-id="35018-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="35018-462">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-462">**Mitigations**</span></span>

<span data-ttu-id="35018-463">Jeśli element będący własnością ma być śledzony do zmodyfikowania w dowolny sposób, właściciel powinien zostać uwzględniony w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="35018-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="35018-464">W przeciwnym razie Dodaj wywołanie `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="35018-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="35018-465">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="35018-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="35018-466">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="35018-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="35018-467">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-467">**Old behavior**</span></span>

<span data-ttu-id="35018-468">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="35018-468">Consider the following model:</span></span>
```csharp
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

<span data-ttu-id="35018-469">Przed EF Core 3,0 Właściwość `ShippingAddress` zostanie zmapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="35018-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="35018-470">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-470">**New behavior**</span></span>

<span data-ttu-id="35018-471">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="35018-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="35018-472">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-472">**Why**</span></span>

<span data-ttu-id="35018-473">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="35018-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="35018-474">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-474">**Mitigations**</span></span>

<span data-ttu-id="35018-475">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="35018-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="35018-476">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="35018-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="35018-477">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="35018-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="35018-478">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-478">**Old behavior**</span></span>

<span data-ttu-id="35018-479">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="35018-479">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="35018-480">Przed EF Core 3,0 Właściwość `CustomerId` byłaby użyta dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="35018-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="35018-481">Jeśli jednak `Order` jest typem będącym własnością, to również `CustomerId` klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="35018-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="35018-482">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-482">**New behavior**</span></span>

<span data-ttu-id="35018-483">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="35018-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="35018-484">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="35018-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="35018-485">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-485">For example:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="35018-486">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-486">**Why**</span></span>

<span data-ttu-id="35018-487">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="35018-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="35018-488">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-488">**Mitigations**</span></span>

<span data-ttu-id="35018-489">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="35018-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="35018-490">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="35018-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="35018-491">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="35018-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="35018-492">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-492">**Old behavior**</span></span>

<span data-ttu-id="35018-493">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="35018-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
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

<span data-ttu-id="35018-494">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-494">**New behavior**</span></span>

<span data-ttu-id="35018-495">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="35018-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="35018-496">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-496">**Why**</span></span>

<span data-ttu-id="35018-497">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="35018-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="35018-498">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="35018-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="35018-499">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-499">**Mitigations**</span></span>

<span data-ttu-id="35018-500">Jeśli połączenie musi pozostać otwarte jawne wywołanie do `OpenConnection()`, zapewni, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="35018-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="35018-501">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="35018-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="35018-502">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="35018-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="35018-503">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-503">**Old behavior**</span></span>

<span data-ttu-id="35018-504">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35018-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="35018-505">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-505">**New behavior**</span></span>

<span data-ttu-id="35018-506">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35018-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="35018-507">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="35018-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="35018-508">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-508">**Why**</span></span>

<span data-ttu-id="35018-509">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35018-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="35018-510">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-510">**Mitigations**</span></span>

<span data-ttu-id="35018-511">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35018-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="35018-512">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="35018-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="35018-513">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="35018-513">Backing fields are used by default</span></span>

[<span data-ttu-id="35018-514">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="35018-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="35018-515">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-515">**Old behavior**</span></span>

<span data-ttu-id="35018-516">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="35018-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="35018-517">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="35018-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="35018-518">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-518">**New behavior**</span></span>

<span data-ttu-id="35018-519">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="35018-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="35018-520">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="35018-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="35018-521">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-521">**Why**</span></span>

<span data-ttu-id="35018-522">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="35018-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="35018-523">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-523">**Mitigations**</span></span>

<span data-ttu-id="35018-524">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu dostępu do właściwości w `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="35018-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="35018-525">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="35018-526">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="35018-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="35018-527">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="35018-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="35018-528">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-528">**Old behavior**</span></span>

<span data-ttu-id="35018-529">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="35018-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="35018-530">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="35018-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="35018-531">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-531">**New behavior**</span></span>

<span data-ttu-id="35018-532">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="35018-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="35018-533">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-533">**Why**</span></span>

<span data-ttu-id="35018-534">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="35018-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="35018-535">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-535">**Mitigations**</span></span>

<span data-ttu-id="35018-536">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="35018-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="35018-537">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="35018-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="35018-538">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="35018-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="35018-539">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-539">**Old behavior**</span></span>

<span data-ttu-id="35018-540">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="35018-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="35018-541">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-541">**New behavior**</span></span>

<span data-ttu-id="35018-542">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="35018-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="35018-543">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-543">**Why**</span></span>

<span data-ttu-id="35018-544">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="35018-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="35018-545">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-545">**Mitigations**</span></span>

<span data-ttu-id="35018-546">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="35018-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="35018-547">W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="35018-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="35018-548">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="35018-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="35018-549">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="35018-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="35018-550">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-550">**Old behavior**</span></span>

<span data-ttu-id="35018-551">Przed EF Core 3,0, wywoływanie `AddDbContext` lub `AddDbContextPool` spowoduje również zarejestrowanie usługi rejestrowania i buforowania pamięci przy użyciu funkcji DI poprzez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="35018-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="35018-552">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-552">**New behavior**</span></span>

<span data-ttu-id="35018-553">Począwszy od EF Core 3,0, `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="35018-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="35018-554">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-554">**Why**</span></span>

<span data-ttu-id="35018-555">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35018-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="35018-556">Jeśli jednak `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="35018-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="35018-557">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-557">**Mitigations**</span></span>

<span data-ttu-id="35018-558">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="35018-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="35018-559">AddEntityFramework \* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="35018-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="35018-560">Śledzenie problemu #12905</span><span class="sxs-lookup"><span data-stu-id="35018-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="35018-561">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-561">**Old behavior**</span></span>

<span data-ttu-id="35018-562">Przed EF Core 3,0, wywoływanie metod `AddEntityFramework*` również rejestruje usługi pamięci podręcznej z systemem DI bez limitu rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="35018-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="35018-563">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-563">**New behavior**</span></span>

<span data-ttu-id="35018-564">Począwszy od EF Core 3,0, `AddEntityFramework*` zarejestruje usługę IMemoryCache z limitem rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="35018-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="35018-565">Jeśli jakiekolwiek inne usługi dodane w późniejszym czasie zależą od IMemoryCache, mogą szybko dotrzeć do domyślnego limitu powodującego wyjątki lub obniżoną wydajność.</span><span class="sxs-lookup"><span data-stu-id="35018-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="35018-566">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-566">**Why**</span></span>

<span data-ttu-id="35018-567">Użycie IMemoryCache bez ograniczenia może spowodować niekontrolowane użycie pamięci, jeśli wystąpi błąd w logice buforowania zapytań lub zapytania są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="35018-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="35018-568">Ustawienie domyślnego ograniczenia ogranicza potencjalny atak w systemie DoS.</span><span class="sxs-lookup"><span data-stu-id="35018-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="35018-569">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-569">**Mitigations**</span></span>

<span data-ttu-id="35018-570">W większości przypadków wywoływanie `AddEntityFramework*` nie jest konieczne w przypadku wywołania `AddDbContext` lub `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="35018-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="35018-571">W związku z tym najlepszym rozwiązaniem jest usunięcie wywołania `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="35018-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="35018-572">Jeśli aplikacja wymaga tych usług, należy zarejestrować implementację IMemoryCache jawnie przy użyciu funkcji DI Container z góry [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="35018-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="35018-573">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="35018-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="35018-574">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="35018-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="35018-575">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-575">**Old behavior**</span></span>

<span data-ttu-id="35018-576">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="35018-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="35018-577">Zapewnia to aktualność stanu ujawnionego w `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="35018-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="35018-578">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-578">**New behavior**</span></span>

<span data-ttu-id="35018-579">Począwszy od EF Core 3,0, wywoływanie `DbContext.Entry` podejmie teraz próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="35018-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="35018-580">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35018-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="35018-581">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false`, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="35018-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="35018-582">Inne metody, które powodują wykrycie zmiany — na przykład `ChangeTracker.Entries` i `SaveChanges`--nadal powodują pełne `DetectChanges` wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="35018-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="35018-583">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-583">**Why**</span></span>

<span data-ttu-id="35018-584">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="35018-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="35018-585">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-585">**Mitigations**</span></span>

<span data-ttu-id="35018-586">Wywołaj `ChangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry`, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="35018-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="35018-587">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="35018-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="35018-588">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="35018-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="35018-589">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-589">**Old behavior**</span></span>

<span data-ttu-id="35018-590">Przed EF Core 3,0 można użyć właściwości klucza `string` i `byte[]` bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="35018-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="35018-591">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, serializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="35018-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="35018-592">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-592">**New behavior**</span></span>

<span data-ttu-id="35018-593">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="35018-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="35018-594">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-594">**Why**</span></span>

<span data-ttu-id="35018-595">Ta zmiana została wprowadzona, ponieważ wygenerowane przez klienta `string`/wartości `byte[]` zwykle nie są przydatne, a zachowanie domyślne spowodowało trudne przyczyny dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="35018-596">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-596">**Mitigations**</span></span>

<span data-ttu-id="35018-597">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="35018-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="35018-598">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="35018-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="35018-599">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="35018-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="35018-600">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="35018-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="35018-601">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="35018-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="35018-602">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-602">**Old behavior**</span></span>

<span data-ttu-id="35018-603">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="35018-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="35018-604">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-604">**New behavior**</span></span>

<span data-ttu-id="35018-605">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="35018-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="35018-606">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-606">**Why**</span></span>

<span data-ttu-id="35018-607">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z wystąpieniem `DbContext`, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="35018-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="35018-608">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-608">**Mitigations**</span></span>

<span data-ttu-id="35018-609">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="35018-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="35018-610">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="35018-610">This isn't common.</span></span>
<span data-ttu-id="35018-611">W takich przypadkach większość elementów będzie nadal działać, ale każda usługa singleton, która była zależna od `ILoggerFactory` będzie musiała zostać zmieniona w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="35018-612">Jeśli używasz takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak korzystasz z `ILoggerFactory`, dzięki czemu możemy lepiej zrozumieć, jak nie należy ponownie rozbić tego w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="35018-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="35018-613">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="35018-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="35018-614">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="35018-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="35018-615">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-615">**Old behavior**</span></span>

<span data-ttu-id="35018-616">Przed EF Core 3,0, gdy `DbContext` został usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="35018-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="35018-617">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="35018-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="35018-618">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="35018-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="35018-619">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-619">**New behavior**</span></span>

<span data-ttu-id="35018-620">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="35018-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="35018-621">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="35018-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="35018-622">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="35018-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="35018-623">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="35018-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="35018-624">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-624">**Why**</span></span>

<span data-ttu-id="35018-625">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie podczas próby załadowania opóźnionego na usunięte wystąpienie `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="35018-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="35018-626">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-626">**Mitigations**</span></span>

<span data-ttu-id="35018-627">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="35018-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="35018-628">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="35018-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="35018-629">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="35018-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="35018-630">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-630">**Old behavior**</span></span>

<span data-ttu-id="35018-631">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="35018-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="35018-632">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-632">**New behavior**</span></span>

<span data-ttu-id="35018-633">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="35018-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="35018-634">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-634">**Why**</span></span>

<span data-ttu-id="35018-635">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="35018-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="35018-636">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-636">**Mitigations**</span></span>

<span data-ttu-id="35018-637">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="35018-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="35018-638">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="35018-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="35018-639">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="35018-640">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="35018-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="35018-641">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="35018-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="35018-642">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-642">**Old behavior**</span></span>

<span data-ttu-id="35018-643">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="35018-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="35018-644">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="35018-645">Kod wygląda tak, jak odnosi się `Samurai` do innego typu jednostki przy użyciu właściwości nawigacji `Entrance`, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="35018-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="35018-646">W rzeczywistości ten kod próbuje utworzyć relację z typem jednostki o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="35018-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="35018-647">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-647">**New behavior**</span></span>

<span data-ttu-id="35018-648">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="35018-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="35018-649">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-649">**Why**</span></span>

<span data-ttu-id="35018-650">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="35018-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="35018-651">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-651">**Mitigations**</span></span>

<span data-ttu-id="35018-652">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="35018-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="35018-653">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-653">This is not common.</span></span>
<span data-ttu-id="35018-654">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="35018-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="35018-655">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="35018-656">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="35018-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="35018-657">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="35018-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="35018-658">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-658">**Old behavior**</span></span>

<span data-ttu-id="35018-659">Następujące metody asynchroniczne wcześniej zwracały `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="35018-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="35018-660">`ValueGenerator.NextValueAsync()` (i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="35018-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="35018-661">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-661">**New behavior**</span></span>

<span data-ttu-id="35018-662">Powyższe metody teraz zwracają `ValueTask<T>` w tym samym `T` jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="35018-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="35018-663">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-663">**Why**</span></span>

<span data-ttu-id="35018-664">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="35018-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="35018-665">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-665">**Mitigations**</span></span>

<span data-ttu-id="35018-666">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="35018-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="35018-667">Bardziej złożone użycie (np. przekazywanie zwróconych `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` być konwertowane na `Task<T>` przez wywołanie `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="35018-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="35018-668">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="35018-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="35018-669">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="35018-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="35018-670">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="35018-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="35018-671">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-671">**Old behavior**</span></span>

<span data-ttu-id="35018-672">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="35018-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="35018-673">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-673">**New behavior**</span></span>

<span data-ttu-id="35018-674">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="35018-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="35018-675">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-675">**Why**</span></span>

<span data-ttu-id="35018-676">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="35018-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="35018-677">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-677">**Mitigations**</span></span>

<span data-ttu-id="35018-678">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="35018-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="35018-679">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="35018-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="35018-680">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="35018-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="35018-681">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="35018-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="35018-682">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-682">**Old behavior**</span></span>

<span data-ttu-id="35018-683">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdzie jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="35018-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="35018-684">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-684">**New behavior**</span></span>

<span data-ttu-id="35018-685">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, `ToTable()` wywołana dla typu pochodnego spowoduje teraz zgłoszenie wyjątku, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="35018-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="35018-686">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-686">**Why**</span></span>

<span data-ttu-id="35018-687">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="35018-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="35018-688">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="35018-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="35018-689">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-689">**Mitigations**</span></span>

<span data-ttu-id="35018-690">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="35018-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="35018-691">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="35018-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="35018-692">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="35018-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="35018-693">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-693">**Old behavior**</span></span>

<span data-ttu-id="35018-694">Przed EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnić sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="35018-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="35018-695">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-695">**New behavior**</span></span>

<span data-ttu-id="35018-696">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="35018-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="35018-697">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="35018-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="35018-698">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-698">**Why**</span></span>

<span data-ttu-id="35018-699">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów z `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="35018-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="35018-700">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-700">**Mitigations**</span></span>

<span data-ttu-id="35018-701">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="35018-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="35018-702">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="35018-702">Metadata API changes</span></span>

[<span data-ttu-id="35018-703">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="35018-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="35018-704">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-704">**New behavior**</span></span>

<span data-ttu-id="35018-705">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="35018-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="35018-706">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-706">**Why**</span></span>

<span data-ttu-id="35018-707">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="35018-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="35018-708">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-708">**Mitigations**</span></span>

<span data-ttu-id="35018-709">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="35018-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="35018-710">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="35018-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="35018-711">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="35018-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="35018-712">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-712">**New behavior**</span></span>

<span data-ttu-id="35018-713">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="35018-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="35018-714">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-714">**Why**</span></span>

<span data-ttu-id="35018-715">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="35018-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="35018-716">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-716">**Mitigations**</span></span>

<span data-ttu-id="35018-717">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="35018-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="35018-718">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="35018-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="35018-719">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="35018-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="35018-720">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-720">**Old behavior**</span></span>

<span data-ttu-id="35018-721">Przed EF Core 3,0, EF Core wyśle `PRAGMA foreign_keys = 1` w przypadku otwarcia połączenia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="35018-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="35018-722">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-722">**New behavior**</span></span>

<span data-ttu-id="35018-723">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` po otwarciu połączenia z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="35018-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="35018-724">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-724">**Why**</span></span>

<span data-ttu-id="35018-725">Ta zmiana została wprowadzona, ponieważ EF Core domyślnie używa `SQLitePCLRaw.bundle_e_sqlite3`, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="35018-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="35018-726">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-726">**Mitigations**</span></span>

<span data-ttu-id="35018-727">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="35018-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="35018-728">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="35018-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="35018-729">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="35018-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="35018-730">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-730">**Old behavior**</span></span>

<span data-ttu-id="35018-731">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="35018-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="35018-732">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-732">**New behavior**</span></span>

<span data-ttu-id="35018-733">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="35018-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="35018-734">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-734">**Why**</span></span>

<span data-ttu-id="35018-735">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="35018-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="35018-736">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-736">**Mitigations**</span></span>

<span data-ttu-id="35018-737">Aby użyć natywnej wersji oprogramowania SQLite w systemie iOS, skonfiguruj `Microsoft.Data.Sqlite` tak, aby korzystała z innego pakietu `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="35018-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="35018-738">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="35018-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="35018-739">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="35018-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="35018-740">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-740">**Old behavior**</span></span>

<span data-ttu-id="35018-741">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="35018-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="35018-742">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-742">**New behavior**</span></span>

<span data-ttu-id="35018-743">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="35018-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="35018-744">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-744">**Why**</span></span>

<span data-ttu-id="35018-745">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="35018-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="35018-746">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="35018-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="35018-747">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-747">**Mitigations**</span></span>

<span data-ttu-id="35018-748">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="35018-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="35018-749">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="35018-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="35018-750">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="35018-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="35018-751">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="35018-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="35018-752">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="35018-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="35018-753">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-753">**Old behavior**</span></span>

<span data-ttu-id="35018-754">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="35018-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="35018-755">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="35018-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="35018-756">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-756">**New behavior**</span></span>

<span data-ttu-id="35018-757">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="35018-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="35018-758">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-758">**Why**</span></span>

<span data-ttu-id="35018-759">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="35018-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="35018-760">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-760">**Mitigations**</span></span>

<span data-ttu-id="35018-761">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="35018-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="35018-762">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="35018-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="35018-763">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="35018-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="35018-764">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="35018-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="35018-765">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="35018-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="35018-766">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-766">**Old behavior**</span></span>

<span data-ttu-id="35018-767">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="35018-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="35018-768">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-768">**New behavior**</span></span>

<span data-ttu-id="35018-769">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="35018-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="35018-770">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-770">**Why**</span></span>

<span data-ttu-id="35018-771">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="35018-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="35018-772">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="35018-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="35018-773">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-773">**Mitigations**</span></span>

<span data-ttu-id="35018-774">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="35018-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="35018-775">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="35018-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="35018-776">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="35018-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="35018-777">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="35018-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="35018-778">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="35018-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="35018-779">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="35018-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="35018-780">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-780">**Old behavior**</span></span>

<span data-ttu-id="35018-781">Przed EF Core 3,0, `UseRowNumberForPaging` może być użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="35018-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="35018-782">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-782">**New behavior**</span></span>

<span data-ttu-id="35018-783">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="35018-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="35018-784">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-784">**Why**</span></span>

<span data-ttu-id="35018-785">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="35018-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="35018-786">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-786">**Mitigations**</span></span>

<span data-ttu-id="35018-787">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="35018-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="35018-788">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="35018-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="35018-789">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="35018-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="35018-790">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="35018-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="35018-791">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="35018-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="35018-792">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-792">**Old behavior**</span></span>

<span data-ttu-id="35018-793">`IDbContextOptionsExtension` zawierały metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="35018-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="35018-794">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-794">**New behavior**</span></span>

<span data-ttu-id="35018-795">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej właściwości `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="35018-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="35018-796">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-796">**Why**</span></span>

<span data-ttu-id="35018-797">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="35018-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="35018-798">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="35018-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="35018-799">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-799">**Mitigations**</span></span>

<span data-ttu-id="35018-800">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="35018-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="35018-801">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="35018-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="35018-802">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="35018-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="35018-803">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="35018-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="35018-804">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="35018-804">**Change**</span></span>

<span data-ttu-id="35018-805">Zmieniono nazwę `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="35018-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="35018-806">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-806">**Why**</span></span>

<span data-ttu-id="35018-807">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="35018-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="35018-808">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-808">**Mitigations**</span></span>

<span data-ttu-id="35018-809">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="35018-809">Use the new name.</span></span> <span data-ttu-id="35018-810">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="35018-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="35018-811">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="35018-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="35018-812">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="35018-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="35018-813">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-813">**Old behavior**</span></span>

<span data-ttu-id="35018-814">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="35018-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="35018-815">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="35018-816">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-816">**New behavior**</span></span>

<span data-ttu-id="35018-817">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="35018-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="35018-818">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="35018-819">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-819">**Why**</span></span>

<span data-ttu-id="35018-820">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="35018-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="35018-821">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-821">**Mitigations**</span></span>

<span data-ttu-id="35018-822">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="35018-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="35018-823">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="35018-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="35018-824">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="35018-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="35018-825">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-825">**Old behavior**</span></span>

<span data-ttu-id="35018-826">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="35018-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="35018-827">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-827">**New behavior**</span></span>

<span data-ttu-id="35018-828">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="35018-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="35018-829">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-829">**Why**</span></span>

<span data-ttu-id="35018-830">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="35018-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="35018-831">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="35018-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="35018-832">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-832">**Mitigations**</span></span>

<span data-ttu-id="35018-833">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="35018-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="35018-834">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="35018-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="35018-835">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="35018-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="35018-836">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-836">**Old behavior**</span></span>

<span data-ttu-id="35018-837">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="35018-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="35018-838">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-838">**New behavior**</span></span>

<span data-ttu-id="35018-839">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="35018-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="35018-840">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwoływać się do jej zestawu.</span><span class="sxs-lookup"><span data-stu-id="35018-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="35018-841">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-841">**Why**</span></span>

<span data-ttu-id="35018-842">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="35018-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="35018-843">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="35018-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="35018-844">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="35018-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="35018-845">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-845">**Mitigations**</span></span>

<span data-ttu-id="35018-846">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie w czasie projektowania EF Core, następnie możesz zaktualizować metadane elementu PackageReference w projekcie.</span><span class="sxs-lookup"><span data-stu-id="35018-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="35018-847">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="35018-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="35018-848">Takie jawne odwołanie należy dodać do każdego projektu, w którym są potrzebne typy z pakietu.</span><span class="sxs-lookup"><span data-stu-id="35018-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="35018-849">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="35018-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="35018-850">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="35018-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="35018-851">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-851">**Old behavior**</span></span>

<span data-ttu-id="35018-852">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="35018-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="35018-853">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-853">**New behavior**</span></span>

<span data-ttu-id="35018-854">Zaktualizowaliśmy pakiet, który jest zależny od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="35018-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="35018-855">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-855">**Why**</span></span>

<span data-ttu-id="35018-856">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="35018-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="35018-857">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="35018-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="35018-858">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-858">**Mitigations**</span></span>

<span data-ttu-id="35018-859">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="35018-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="35018-860">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="35018-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="35018-861">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="35018-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="35018-862">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="35018-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="35018-863">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-863">**Old behavior**</span></span>

<span data-ttu-id="35018-864">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="35018-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="35018-865">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-865">**New behavior**</span></span>

<span data-ttu-id="35018-866">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="35018-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="35018-867">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-867">**Why**</span></span>

<span data-ttu-id="35018-868">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="35018-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="35018-869">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-869">**Mitigations**</span></span>

<span data-ttu-id="35018-870">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="35018-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="35018-871">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="35018-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="35018-872">Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="35018-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="35018-873">Śledzenie problemu #15636</span><span class="sxs-lookup"><span data-stu-id="35018-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="35018-874">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-874">**Old behavior**</span></span>

<span data-ttu-id="35018-875">Microsoft. EntityFrameworkCore. SqlServer poprzednio zależała od typu System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="35018-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="35018-876">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-876">**New behavior**</span></span>

<span data-ttu-id="35018-877">Zaktualizowaliśmy pakiet, który jest zależny od firmy Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="35018-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="35018-878">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-878">**Why**</span></span>

<span data-ttu-id="35018-879">Microsoft. Data. SqlClient to sterownik dostępu do danych sztandarowe, który umożliwia SQL Server przechodzenie do przodu, a system. Data. SqlClient nie jest już fokusem rozwoju.</span><span class="sxs-lookup"><span data-stu-id="35018-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="35018-880">Niektóre ważne funkcje, takie jak Always Encrypted, są dostępne tylko w firmie Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="35018-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="35018-881">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-881">**Mitigations**</span></span>

<span data-ttu-id="35018-882">Jeśli kod przyjmuje bezpośrednią zależność od elementu System. Data. SqlClient, należy zmienić go na odwołanie Microsoft. Data. SqlClient zamiast tego. ponieważ dwa pakiety obsługują bardzo wysoki poziom zgodności interfejsów API, powinno to być tylko proste zmiany pakietów i przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="35018-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="35018-883">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="35018-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="35018-884">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="35018-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="35018-885">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-885">**Old behavior**</span></span>

<span data-ttu-id="35018-886">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="35018-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="35018-887">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-887">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="35018-888">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-888">**New behavior**</span></span>

<span data-ttu-id="35018-889">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="35018-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="35018-890">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-890">**Why**</span></span>

<span data-ttu-id="35018-891">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="35018-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="35018-892">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-892">**Mitigations**</span></span>

<span data-ttu-id="35018-893">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="35018-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="35018-894">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35018-894">For example:</span></span>

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="35018-895">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="35018-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="35018-896">Śledzenie problemu #12757</span><span class="sxs-lookup"><span data-stu-id="35018-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="35018-897">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-897">**Old behavior**</span></span>

<span data-ttu-id="35018-898">Funkcja dbzostała skonfigurowana ze schematem jako pusty ciąg był traktowany jak wbudowana funkcja bez schematu.</span><span class="sxs-lookup"><span data-stu-id="35018-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="35018-899">Na przykład poniższy kod mapuje `DatePart` funkcję CLR, aby `DATEPART` funkcję wbudowaną na serwerze SqlServer.</span><span class="sxs-lookup"><span data-stu-id="35018-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="35018-900">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="35018-900">**New behavior**</span></span>

<span data-ttu-id="35018-901">Wszystkie mapowania funkcji dbfunction są uznawane za mapowane do funkcji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="35018-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="35018-902">W związku z tym wartość pustego ciągu spowodowałaby umieszczenie funkcji wewnątrz domyślnego schematu modelu.</span><span class="sxs-lookup"><span data-stu-id="35018-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="35018-903">Może to być schemat skonfigurowany jawnie za pośrednictwem interfejsu API Fluent `modelBuilder.HasDefaultSchema()` lub `dbo` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="35018-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="35018-904">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="35018-904">**Why**</span></span>

<span data-ttu-id="35018-905">Poprzednio pusty schemat był sposobem traktowania tej funkcji, ale ta logika jest stosowana tylko dla SqlServer, w której wbudowane funkcje nie należą do żadnego schematu.</span><span class="sxs-lookup"><span data-stu-id="35018-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="35018-906">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="35018-906">**Mitigations**</span></span>

<span data-ttu-id="35018-907">Skonfiguruj ręcznie tłumaczenie funkcji dbfunction, aby zamapować ją na wbudowaną funkcję.</span><span class="sxs-lookup"><span data-stu-id="35018-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
