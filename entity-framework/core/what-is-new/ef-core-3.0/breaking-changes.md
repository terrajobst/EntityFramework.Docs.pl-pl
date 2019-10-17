---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 690c7828cfe5019f4e7ae904c92430fab4726cb9
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446017"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="4b7e5-102">Istotne zmiany zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="4b7e5-103">Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="4b7e5-104">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="4b7e5-105">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-105">Summary</span></span>

| <span data-ttu-id="4b7e5-106">**Zmiana podziału**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="4b7e5-107">**Notebook**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="4b7e5-108">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="4b7e5-109">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-109">High</span></span>       |
| [<span data-ttu-id="4b7e5-110">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="4b7e5-111">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-111">High</span></span>      |
| [<span data-ttu-id="4b7e5-112">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="4b7e5-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="4b7e5-113">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-113">High</span></span>      |
| [<span data-ttu-id="4b7e5-114">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="4b7e5-115">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-115">High</span></span>      |
| [<span data-ttu-id="4b7e5-116">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="4b7e5-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="4b7e5-117">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-117">High</span></span>      |
| [<span data-ttu-id="4b7e5-118">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="4b7e5-119">Wysokowydajn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-119">High</span></span>      |
| [<span data-ttu-id="4b7e5-120">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="4b7e5-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="4b7e5-121">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-121">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-122">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="4b7e5-123">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-123">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-124">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="4b7e5-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="4b7e5-125">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-125">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-126">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="4b7e5-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="4b7e5-127">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-127">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-128">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="4b7e5-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="4b7e5-129">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-129">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-130">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="4b7e5-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="4b7e5-131">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-131">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-132">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="4b7e5-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="4b7e5-133">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-133">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-134">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="4b7e5-135">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-135">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-136">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="4b7e5-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="4b7e5-137">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-137">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-138">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="4b7e5-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="4b7e5-139">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-139">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-140">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="4b7e5-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="4b7e5-141">Średniookresow</span><span class="sxs-lookup"><span data-stu-id="4b7e5-141">Medium</span></span>      |
| [<span data-ttu-id="4b7e5-142">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="4b7e5-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="4b7e5-143">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-143">Low</span></span>      |
| [<span data-ttu-id="4b7e5-144">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="4b7e5-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="4b7e5-145">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-145">Low</span></span>      |
| [<span data-ttu-id="4b7e5-146">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="4b7e5-147">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-147">Low</span></span>      |
| [<span data-ttu-id="4b7e5-148">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="4b7e5-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="4b7e5-149">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-149">Low</span></span>      |
| [<span data-ttu-id="4b7e5-150">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="4b7e5-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="4b7e5-151">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-151">Low</span></span>      |
| [<span data-ttu-id="4b7e5-152">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="4b7e5-153">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-153">Low</span></span>      |
| [<span data-ttu-id="4b7e5-154">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="4b7e5-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="4b7e5-155">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-155">Low</span></span>      |
| [<span data-ttu-id="4b7e5-156">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="4b7e5-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="4b7e5-157">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-157">Low</span></span>      |
| [<span data-ttu-id="4b7e5-158">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="4b7e5-159">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-159">Low</span></span>      |
| [<span data-ttu-id="4b7e5-160">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="4b7e5-161">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-161">Low</span></span>      |
| [<span data-ttu-id="4b7e5-162">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="4b7e5-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="4b7e5-163">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-163">Low</span></span>      |
| [<span data-ttu-id="4b7e5-164">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="4b7e5-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="4b7e5-165">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-165">Low</span></span>      |
| [<span data-ttu-id="4b7e5-166">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="4b7e5-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="4b7e5-167">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-167">Low</span></span>      |
| [<span data-ttu-id="4b7e5-168">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="4b7e5-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="4b7e5-169">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-169">Low</span></span>      |
| [<span data-ttu-id="4b7e5-170">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="4b7e5-171">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-171">Low</span></span>      |
| [<span data-ttu-id="4b7e5-172">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="4b7e5-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="4b7e5-173">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-173">Low</span></span>      |
| [<span data-ttu-id="4b7e5-174">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="4b7e5-175">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-175">Low</span></span>      |
| [<span data-ttu-id="4b7e5-176">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="4b7e5-177">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-177">Low</span></span>      |
| [<span data-ttu-id="4b7e5-178">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="4b7e5-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="4b7e5-179">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-179">Low</span></span>      |
| [<span data-ttu-id="4b7e5-180">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="4b7e5-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="4b7e5-181">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-181">Low</span></span>      |
| [<span data-ttu-id="4b7e5-182">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="4b7e5-183">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-183">Low</span></span>      |
| [<span data-ttu-id="4b7e5-184">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="4b7e5-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="4b7e5-185">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-185">Low</span></span>      |
| [<span data-ttu-id="4b7e5-186">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="4b7e5-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="4b7e5-187">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-187">Low</span></span>      |
| [<span data-ttu-id="4b7e5-188">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="4b7e5-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="4b7e5-189">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-189">Low</span></span>      |
| [<span data-ttu-id="4b7e5-190">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="4b7e5-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="4b7e5-191">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-191">Low</span></span>      |
| [<span data-ttu-id="4b7e5-192">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="4b7e5-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="4b7e5-193">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-193">Low</span></span>      |
| [<span data-ttu-id="4b7e5-194">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="4b7e5-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="4b7e5-195">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-195">Low</span></span>      |
| [<span data-ttu-id="4b7e5-196">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="4b7e5-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="4b7e5-197">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-197">Low</span></span>      |
| [<span data-ttu-id="4b7e5-198">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="4b7e5-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="4b7e5-199">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-199">Low</span></span>      |
| [<span data-ttu-id="4b7e5-200">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="4b7e5-201">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-201">Low</span></span>      |
| [<span data-ttu-id="4b7e5-202">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="4b7e5-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="4b7e5-203">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-203">Low</span></span>      |
| [<span data-ttu-id="4b7e5-204">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="4b7e5-205">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-205">Low</span></span>      |
| [<span data-ttu-id="4b7e5-206">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="4b7e5-207">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-207">Low</span></span>      |
| [<span data-ttu-id="4b7e5-208">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="4b7e5-209">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-209">Low</span></span>      |
| [<span data-ttu-id="4b7e5-210">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="4b7e5-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="4b7e5-211">Małą</span><span class="sxs-lookup"><span data-stu-id="4b7e5-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="4b7e5-212">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="4b7e5-213">[Problem ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)@no__t — 1[zobacz także problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="4b7e5-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="4b7e5-214">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-214">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-215">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="4b7e5-216">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="4b7e5-217">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-217">**New behavior**</span></span>

<span data-ttu-id="4b7e5-218">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołania w zapytaniu) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="4b7e5-219">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="4b7e5-220">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-220">**Why**</span></span>

<span data-ttu-id="4b7e5-221">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="4b7e5-222">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="4b7e5-223">Na przykład warunek w wywołaniu `Where()`, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="4b7e5-224">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="4b7e5-225">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="4b7e5-226">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="4b7e5-227">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-227">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-228">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć, lub użyć `AsEnumerable()`, `ToList()` lub podobnego do jawnego przywrócenia danych z powrotem do klienta, na którym będzie można go następnie przetworzyć przy użyciu LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="4b7e5-229">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="4b7e5-230">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="4b7e5-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="4b7e5-231">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-231">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-232">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="4b7e5-233">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-233">**New behavior**</span></span>

<span data-ttu-id="4b7e5-234">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="4b7e5-235">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="4b7e5-236">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-236">**Why**</span></span>

<span data-ttu-id="4b7e5-237">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="4b7e5-238">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-238">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-239">Rozważ przeniesienie do nowoczesnej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="4b7e5-240">Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="4b7e5-241">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="4b7e5-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="4b7e5-242">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="4b7e5-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="4b7e5-243">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-243">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-244">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All` będzie obejmował EF Core i niektórych spośród EF Core dostawców danych, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="4b7e5-245">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-245">**New behavior**</span></span>

<span data-ttu-id="4b7e5-246">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="4b7e5-247">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-247">**Why**</span></span>

<span data-ttu-id="4b7e5-248">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="4b7e5-249">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="4b7e5-250">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="4b7e5-251">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="4b7e5-252">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-252">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-253">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="4b7e5-254">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="4b7e5-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="4b7e5-255">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="4b7e5-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="4b7e5-256">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-256">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-257">Przed 3,0 Narzędzie `dotnet ef` zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="4b7e5-258">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-258">**New behavior**</span></span>

<span data-ttu-id="4b7e5-259">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera narzędzia `dotnet ef`, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="4b7e5-260">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-260">**Why**</span></span>

<span data-ttu-id="4b7e5-261">Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="4b7e5-262">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-262">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-263">Aby móc zarządzać migracjami lub szkieletem `DbContext`, zainstaluj `dotnet-ef` jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="4b7e5-264">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="4b7e5-265">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="4b7e5-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="4b7e5-266">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="4b7e5-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="4b7e5-267">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-267">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-268">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="4b7e5-269">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-269">**New behavior**</span></span>

<span data-ttu-id="4b7e5-270">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw` i `ExecuteSqlRawAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="4b7e5-271">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="4b7e5-272">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated` i `ExecuteSqlInterpolatedAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="4b7e5-273">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="4b7e5-274">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="4b7e5-275">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-275">**Why**</span></span>

<span data-ttu-id="4b7e5-276">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="4b7e5-277">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="4b7e5-278">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-278">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-279">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="4b7e5-280">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="4b7e5-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="4b7e5-281">Śledzenie problemu #15392</span><span class="sxs-lookup"><span data-stu-id="4b7e5-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="4b7e5-282">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-282">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-283">Przed EF Core 3,0, Metoda Z tabel podjęła próbę wykrycia, czy może być złożona pozostała wartość SQL.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="4b7e5-284">Klient przeprowadził ocenę, gdy nie udało się utworzyć kodu SQL, podobnie jak procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="4b7e5-285">Następujące zapytanie działało przez uruchomienie procedury składowanej na serwerze i wykonanie FirstOrDefault po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="4b7e5-286">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-286">**New behavior**</span></span>

<span data-ttu-id="4b7e5-287">Począwszy od EF Core 3,0, EF Core nie będzie próbować przeanalizować bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="4b7e5-288">Dlatego w przypadku redagowania po FromSqlRaw/FromSqlInterpolated, EF Core nastąpi złożenie zapytania podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="4b7e5-289">Dlatego jeśli używasz procedury składowanej z kompozycją, zostanie pobrany wyjątek dla nieprawidłowej składni SQL.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="4b7e5-290">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-290">**Why**</span></span>

<span data-ttu-id="4b7e5-291">EF Core 3,0 nie obsługuje automatycznej oceny klienta, ponieważ została ona podatna na błędy, jak wyjaśniono [tutaj](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="4b7e5-292">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-292">**Mitigation**</span></span>

<span data-ttu-id="4b7e5-293">Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można jej składować, więc możesz dodać __AsEnumerable/AsAsyncEnumerable__ bezpośrednio po wywołaniu metody z tabel, aby uniknąć jakiegokolwiek złożenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="4b7e5-294">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="4b7e5-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="4b7e5-295">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="4b7e5-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="4b7e5-296">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-296">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-297">Przed EF Core 3,0 można określić metodę `FromSql` w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="4b7e5-298">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-298">**New behavior**</span></span>

<span data-ttu-id="4b7e5-299">Począwszy od EF Core 3,0, nowe metody `FromSqlRaw` i `FromSqlInterpolated` (zastępujące `FromSql`) można określić tylko dla katalogów głównych zapytań, tj. bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="4b7e5-300">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="4b7e5-301">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-301">**Why**</span></span>

<span data-ttu-id="4b7e5-302">Określenie wartości `FromSql` w innym miejscu niż na `DbSet` nie miało znaczenia lub dodano wartość i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="4b7e5-303">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-303">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-304">wywołania `FromSql` powinny być przenoszone bezpośrednio do `DbSet`, do którego mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="4b7e5-305">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="4b7e5-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="4b7e5-306">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="4b7e5-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="4b7e5-307">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-307">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-308">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="4b7e5-309">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="4b7e5-310">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="4b7e5-311">zwróci to samo wystąpienie `Category` dla każdej `Product`, która jest skojarzona z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="4b7e5-312">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-312">**New behavior**</span></span>

<span data-ttu-id="4b7e5-313">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="4b7e5-314">Na przykład zapytanie powyżej zwróci nowe wystąpienie `Category` dla każdej `Product`, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="4b7e5-315">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-315">**Why**</span></span>

<span data-ttu-id="4b7e5-316">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="4b7e5-317">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="4b7e5-318">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="4b7e5-319">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-319">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-320">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="4b7e5-321">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="4b7e5-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="4b7e5-322">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="4b7e5-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="4b7e5-323">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="4b7e5-324">Aby na przykład przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="4b7e5-325">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="4b7e5-326">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="4b7e5-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="4b7e5-327">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-327">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-328">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="4b7e5-329">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="4b7e5-330">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-330">**New behavior**</span></span>

<span data-ttu-id="4b7e5-331">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="4b7e5-332">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-332">**Why**</span></span>

<span data-ttu-id="4b7e5-333">Ta zmiana została wprowadzona w celu zapobiegania błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre wystąpienie `DbContext` jest przenoszona do innego wystąpienia `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="4b7e5-334">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-334">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-335">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w stanie `Added`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="4b7e5-336">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-336">This can be avoided by:</span></span>
* <span data-ttu-id="4b7e5-337">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="4b7e5-338">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="4b7e5-339">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="4b7e5-340">Na przykład, `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową nawet wtedy, gdy nie ustawiono elementu `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="4b7e5-341">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="4b7e5-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="4b7e5-342">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="4b7e5-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="4b7e5-343">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-343">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-344">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` będzie śledzona w stanie `Added` i wstawiona jako nowy wiersz w przypadku wywołania `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="4b7e5-345">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-345">**New behavior**</span></span>

<span data-ttu-id="4b7e5-346">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i jest ustawiona określona wartość klucza, jednostka będzie śledzona w stanie `Modified`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="4b7e5-347">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany, gdy zostanie wywołane `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="4b7e5-348">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona jako `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="4b7e5-349">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-349">**Why**</span></span>

<span data-ttu-id="4b7e5-350">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="4b7e5-351">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-351">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-352">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="4b7e5-353">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="4b7e5-354">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="4b7e5-355">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="4b7e5-356">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="4b7e5-357">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="4b7e5-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="4b7e5-358">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-358">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-359">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="4b7e5-360">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-360">**New behavior**</span></span>

<span data-ttu-id="4b7e5-361">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="4b7e5-362">Na przykład wywołanie `context.Remove()` w celu usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane elementy zależne są również domyślnie ustawione na `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="4b7e5-363">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-363">**Why**</span></span>

<span data-ttu-id="4b7e5-364">Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ wywołaniem `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="4b7e5-365">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-365">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-366">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="4b7e5-367">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="4b7e5-368">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="4b7e5-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="4b7e5-369">Śledzenie problemu #18022</span><span class="sxs-lookup"><span data-stu-id="4b7e5-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="4b7e5-370">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-370">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-371">Przed 3,0 eagerly załadowanie nawigacji kolekcji za pośrednictwem operatorów `Include` spowodowało generowanie wielu zapytań w relacyjnej bazie danych, po jednej dla każdego powiązanego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="4b7e5-372">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-372">**New behavior**</span></span>

<span data-ttu-id="4b7e5-373">Począwszy od 3,0, EF Core generuje pojedyncze zapytanie z sprzężeniami w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="4b7e5-374">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-374">**Why**</span></span>

<span data-ttu-id="4b7e5-375">Wygenerowanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ powodowało wiele problemów, w tym negatywną wydajność, ponieważ wiele danych jest niezbędnych, a błędy spójność danych, ponieważ każde zapytanie może obserwować inny stan bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="4b7e5-376">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-376">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-377">Chociaż technicznie nie jest to zmiana, może ona mieć znaczny wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę operatora `Include` na nawigowaniu po kolekcji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="4b7e5-378">[Zobacz ten komentarz](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , aby uzyskać więcej informacji i przepisać zapytania w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="4b7e5-379">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="4b7e5-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="4b7e5-380">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="4b7e5-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="4b7e5-381">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-381">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-382">Przed 3,0 `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z semantyką `Restrict`, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="4b7e5-383">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-383">**New behavior**</span></span>

<span data-ttu-id="4b7e5-384">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone z semantyką `Restrict` — to oznacza, że nie ma żadnych kaskadowych; Zgłoś naruszenie ograniczenia — bez również wpływu na wewnętrzną korektę EF.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="4b7e5-385">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-385">**Why**</span></span>

<span data-ttu-id="4b7e5-386">Ta zmiana została wprowadzona w celu poprawy środowiska korzystania z `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="4b7e5-387">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-387">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-388">Poprzednie zachowanie można przywrócić przy użyciu `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="4b7e5-389">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="4b7e5-390">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="4b7e5-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="4b7e5-391">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-391">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-392">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="4b7e5-393">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="4b7e5-394">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-394">**New behavior**</span></span>

<span data-ttu-id="4b7e5-395">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="4b7e5-396">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="4b7e5-397">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-397">**Why**</span></span>

<span data-ttu-id="4b7e5-398">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="4b7e5-399">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="4b7e5-400">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="4b7e5-401">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-401">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-402">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="4b7e5-403">**`ModelBuilder.Query<>()`** — zamiast tego należy wywołać `ModelBuilder.Entity<>().HasNoKey()`, aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="4b7e5-404">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="4b7e5-405">należy używać **`DbQuery<>`** — zamiast tego należy użyć `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="4b7e5-406">należy używać **`DbContext.Query<>()`** — zamiast tego należy użyć `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="4b7e5-407">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="4b7e5-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="4b7e5-408">Problem ze śledzeniem [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)@no__t śledzenia problemów[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[problem ze śledzeniem #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="4b7e5-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="4b7e5-409">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-409">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-410">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po wywołaniu `OwnsOne` lub `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="4b7e5-411">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-411">**New behavior**</span></span>

<span data-ttu-id="4b7e5-412">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="4b7e5-413">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="4b7e5-414">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem po `WithOwner()` podobnie jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="4b7e5-415">Mimo że konfiguracja dla samego typu, nadal będzie łańcucha po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="4b7e5-416">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-416">For example:</span></span>

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

<span data-ttu-id="4b7e5-417">Dodatkowo wywołanie `Entity()`, `HasOne()` lub `Set()` z elementem docelowym typu będącego właścicielem spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="4b7e5-418">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-418">**Why**</span></span>

<span data-ttu-id="4b7e5-419">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="4b7e5-420">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="4b7e5-421">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-421">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-422">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="4b7e5-423">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="4b7e5-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="4b7e5-424">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="4b7e5-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="4b7e5-425">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-425">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-426">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-426">Consider the following model:</span></span>
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
<span data-ttu-id="4b7e5-427">Przed EF Core 3,0, jeśli `OrderDetails` należy do `Order` lub jawnie zamapowane do tej samej tabeli, wystąpienie `OrderDetails` było zawsze wymagane podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="4b7e5-428">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-428">**New behavior**</span></span>

<span data-ttu-id="4b7e5-429">Począwszy od 3,0, EF Core umożliwia dodanie `Order` bez `OrderDetails` i mapuje wszystkie właściwości `OrderDetails` z wyjątkiem klucza podstawowego na kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="4b7e5-430">Podczas wykonywania zapytania o EF Core ustawia `OrderDetails`, aby `null`, Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym, a wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="4b7e5-431">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-431">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-432">Jeśli Twój model ma zależne od siebie wszystkie opcjonalne kolumny, ale nawigacja nie powinna być `null`, należy zmodyfikować aplikację tak, aby obsługiwała przypadki, gdy nawigacja jest `null`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="4b7e5-433">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć przypisaną wartość inną niż `null`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="4b7e5-434">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="4b7e5-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="4b7e5-435">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="4b7e5-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="4b7e5-436">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-436">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-437">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-437">Consider the following model:</span></span>
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
<span data-ttu-id="4b7e5-438">Przed EF Core 3,0, jeśli `OrderDetails` należy do `Order` lub jawnie zamapowanych do tej samej tabeli, aktualizacja tylko `OrderDetails` nie będzie aktualizować `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="4b7e5-439">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-439">**New behavior**</span></span>

<span data-ttu-id="4b7e5-440">Począwszy od 3,0, EF Core propaguje nową wartość `Version` do `Order`, jeśli jest ona własnością `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="4b7e5-441">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="4b7e5-442">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-442">**Why**</span></span>

<span data-ttu-id="4b7e5-443">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="4b7e5-444">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-444">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-445">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="4b7e5-446">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="4b7e5-447">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="4b7e5-448">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="4b7e5-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="4b7e5-449">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-449">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-450">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-450">Consider the following model:</span></span>
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

<span data-ttu-id="4b7e5-451">Przed EF Core 3,0 Właściwość `ShippingAddress` będzie domyślnie mapowana na oddzielne kolumny dla `BulkOrder` i `Order`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="4b7e5-452">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-452">**New behavior**</span></span>

<span data-ttu-id="4b7e5-453">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="4b7e5-454">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-454">**Why**</span></span>

<span data-ttu-id="4b7e5-455">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="4b7e5-456">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-456">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-457">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="4b7e5-458">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="4b7e5-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="4b7e5-459">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="4b7e5-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="4b7e5-460">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-460">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-461">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-461">Consider the following model:</span></span>
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
<span data-ttu-id="4b7e5-462">Przed EF Core 3,0 Właściwość `CustomerId` byłaby użyta dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="4b7e5-463">Jeśli jednak wartość `Order` jest typem będącym własnością, spowoduje to również, że `CustomerId` klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="4b7e5-464">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-464">**New behavior**</span></span>

<span data-ttu-id="4b7e5-465">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="4b7e5-466">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="4b7e5-467">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-467">For example:</span></span>

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

<span data-ttu-id="4b7e5-468">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-468">**Why**</span></span>

<span data-ttu-id="4b7e5-469">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="4b7e5-470">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-470">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-471">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="4b7e5-472">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="4b7e5-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="4b7e5-473">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="4b7e5-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="4b7e5-474">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-474">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-475">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, podczas gdy bieżący `TransactionScope` jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="4b7e5-476">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-476">**New behavior**</span></span>

<span data-ttu-id="4b7e5-477">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="4b7e5-478">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-478">**Why**</span></span>

<span data-ttu-id="4b7e5-479">Ta zmiana umożliwia użycie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="4b7e5-480">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="4b7e5-481">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-481">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-482">Jeśli połączenie musi pozostać otwarte jawne wywołanie do `OpenConnection()`, zapewni, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="4b7e5-483">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="4b7e5-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="4b7e5-484">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="4b7e5-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="4b7e5-485">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-485">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-486">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="4b7e5-487">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-487">**New behavior**</span></span>

<span data-ttu-id="4b7e5-488">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="4b7e5-489">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="4b7e5-490">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-490">**Why**</span></span>

<span data-ttu-id="4b7e5-491">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="4b7e5-492">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-492">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-493">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="4b7e5-494">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="4b7e5-495">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-495">Backing fields are used by default</span></span>

[<span data-ttu-id="4b7e5-496">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="4b7e5-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="4b7e5-497">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-497">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-498">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="4b7e5-499">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="4b7e5-500">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-500">**New behavior**</span></span>

<span data-ttu-id="4b7e5-501">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="4b7e5-502">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="4b7e5-503">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-503">**Why**</span></span>

<span data-ttu-id="4b7e5-504">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="4b7e5-505">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-505">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-506">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu dostępu do właściwości w `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="4b7e5-507">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="4b7e5-508">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="4b7e5-509">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="4b7e5-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="4b7e5-510">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-510">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-511">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="4b7e5-512">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="4b7e5-513">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-513">**New behavior**</span></span>

<span data-ttu-id="4b7e5-514">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="4b7e5-515">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-515">**Why**</span></span>

<span data-ttu-id="4b7e5-516">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="4b7e5-517">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-517">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-518">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="4b7e5-519">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="4b7e5-520">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="4b7e5-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="4b7e5-521">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-521">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-522">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="4b7e5-523">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-523">**New behavior**</span></span>

<span data-ttu-id="4b7e5-524">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="4b7e5-525">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-525">**Why**</span></span>

<span data-ttu-id="4b7e5-526">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="4b7e5-527">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-527">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-528">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="4b7e5-529">W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="4b7e5-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="4b7e5-530">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="4b7e5-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="4b7e5-531">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="4b7e5-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="4b7e5-532">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-532">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-533">Przed EF Core 3,0 wywoływanie `AddDbContext` lub `AddDbContextPool` spowoduje również zarejestrowanie usługi rejestrowania i buforowania pamięci z D. I przez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="4b7e5-534">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-534">**New behavior**</span></span>

<span data-ttu-id="4b7e5-535">Począwszy od EF Core 3,0, `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="4b7e5-536">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-536">**Why**</span></span>

<span data-ttu-id="4b7e5-537">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="4b7e5-538">Jeśli jednak `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="4b7e5-539">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-539">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-540">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="4b7e5-541">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="4b7e5-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="4b7e5-542">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="4b7e5-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="4b7e5-543">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-543">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-544">Przed EF Core 3,0 wywołania `DbContext.Entry` spowodują wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="4b7e5-545">Zapewnia to aktualność stanu ujawnionego w `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="4b7e5-546">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-546">**New behavior**</span></span>

<span data-ttu-id="4b7e5-547">Począwszy od EF Core 3,0, wywoływanie `DbContext.Entry` podejmie teraz próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="4b7e5-548">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="4b7e5-549">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false`, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="4b7e5-550">Inne metody, które powodują wykrycie zmiany — na przykład `ChangeTracker.Entries` i `SaveChanges`--nadal powodują pełne `DetectChanges` wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="4b7e5-551">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-551">**Why**</span></span>

<span data-ttu-id="4b7e5-552">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="4b7e5-553">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-553">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-554">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry`, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="4b7e5-555">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="4b7e5-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="4b7e5-556">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="4b7e5-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="4b7e5-557">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-557">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-558">Przed EF Core 3,0, `string` i właściwości klucza `byte[]` mogą być używane bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="4b7e5-559">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, serializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="4b7e5-560">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-560">**New behavior**</span></span>

<span data-ttu-id="4b7e5-561">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="4b7e5-562">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-562">**Why**</span></span>

<span data-ttu-id="4b7e5-563">Ta zmiana została wprowadzona, ponieważ wartości wygenerowane przez klienta `string` @ no__t-1 @ no__t-2 zazwyczaj nie są przydatne, a zachowanie domyślne spowodowało trudne przyczyny dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="4b7e5-564">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-564">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-565">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="4b7e5-566">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="4b7e5-567">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="4b7e5-568">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="4b7e5-569">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="4b7e5-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="4b7e5-570">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-570">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-571">Przed EF Core 3,0, `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="4b7e5-572">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-572">**New behavior**</span></span>

<span data-ttu-id="4b7e5-573">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowane jako zakres.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="4b7e5-574">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-574">**Why**</span></span>

<span data-ttu-id="4b7e5-575">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z wystąpieniem `DbContext`, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak wybuch wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="4b7e5-576">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-576">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-577">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="4b7e5-578">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-578">This isn't common.</span></span>
<span data-ttu-id="4b7e5-579">W takich przypadkach większość elementów będzie nadal działać, ale każda usługa singleton, która była zależna od `ILoggerFactory`, będzie musiała zostać zmieniona w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="4b7e5-580">Jeśli używasz takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak korzystasz z `ILoggerFactory`, aby lepiej zrozumieć, jak nie należy ponownie rozbić tego w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="4b7e5-581">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="4b7e5-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="4b7e5-582">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="4b7e5-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="4b7e5-583">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-583">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-584">Przed EF Core 3,0, po usunięciu `DbContext` nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="4b7e5-585">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="4b7e5-586">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="4b7e5-587">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-587">**New behavior**</span></span>

<span data-ttu-id="4b7e5-588">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="4b7e5-589">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="4b7e5-590">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="4b7e5-591">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="4b7e5-592">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-592">**Why**</span></span>

<span data-ttu-id="4b7e5-593">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie podczas próby załadowania opóźnionego na usunięte wystąpienie `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="4b7e5-594">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-594">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-595">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="4b7e5-596">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="4b7e5-597">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="4b7e5-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="4b7e5-598">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-598">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-599">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="4b7e5-600">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-600">**New behavior**</span></span>

<span data-ttu-id="4b7e5-601">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="4b7e5-602">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-602">**Why**</span></span>

<span data-ttu-id="4b7e5-603">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="4b7e5-604">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-604">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-605">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="4b7e5-606">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="4b7e5-607">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="4b7e5-608">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="4b7e5-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="4b7e5-609">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="4b7e5-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="4b7e5-610">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-610">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-611">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="4b7e5-612">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="4b7e5-613">Kod wygląda podobnie do `Samurai` do innego typu jednostki przy użyciu właściwości nawigacji `Entrance`, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="4b7e5-614">W rzeczywistości ten kod próbuje utworzyć relację z typem jednostki o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="4b7e5-615">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-615">**New behavior**</span></span>

<span data-ttu-id="4b7e5-616">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="4b7e5-617">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-617">**Why**</span></span>

<span data-ttu-id="4b7e5-618">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="4b7e5-619">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-619">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-620">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="4b7e5-621">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-621">This is not common.</span></span>
<span data-ttu-id="4b7e5-622">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` dla nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="4b7e5-623">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="4b7e5-624">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="4b7e5-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="4b7e5-625">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="4b7e5-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="4b7e5-626">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-626">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-627">Następujące metody asynchroniczne wcześniej zwracały `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="4b7e5-628">`ValueGenerator.NextValueAsync()` (i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="4b7e5-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="4b7e5-629">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-629">**New behavior**</span></span>

<span data-ttu-id="4b7e5-630">Powyższe metody zwracają teraz `ValueTask<T>` w tym samym `T` tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="4b7e5-631">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-631">**Why**</span></span>

<span data-ttu-id="4b7e5-632">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="4b7e5-633">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-633">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-634">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="4b7e5-635">Bardziej złożone użycie (np. przekazywanie zwróconych `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwracane `ValueTask<T>` były konwertowane na `Task<T>` przez wywołanie `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="4b7e5-636">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="4b7e5-637">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="4b7e5-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="4b7e5-638">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="4b7e5-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="4b7e5-639">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-639">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-640">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="4b7e5-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="4b7e5-641">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-641">**New behavior**</span></span>

<span data-ttu-id="4b7e5-642">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="4b7e5-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="4b7e5-643">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-643">**Why**</span></span>

<span data-ttu-id="4b7e5-644">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="4b7e5-645">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-645">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-646">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="4b7e5-647">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="4b7e5-648">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="4b7e5-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="4b7e5-649">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="4b7e5-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="4b7e5-650">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-650">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-651">Przed EF Core 3,0, `ToTable()` wywołane dla typu pochodnego zostałyby zignorowane, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="4b7e5-652">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-652">**New behavior**</span></span>

<span data-ttu-id="4b7e5-653">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, `ToTable()` wywołana dla typu pochodnego będzie teraz zgłaszać wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="4b7e5-654">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-654">**Why**</span></span>

<span data-ttu-id="4b7e5-655">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="4b7e5-656">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="4b7e5-657">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-657">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-658">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="4b7e5-659">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="4b7e5-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="4b7e5-660">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="4b7e5-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="4b7e5-661">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-661">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-662">Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` udostępnia sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="4b7e5-663">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-663">**New behavior**</span></span>

<span data-ttu-id="4b7e5-664">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="4b7e5-665">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="4b7e5-666">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-666">**Why**</span></span>

<span data-ttu-id="4b7e5-667">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów z `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="4b7e5-668">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-668">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-669">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="4b7e5-670">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="4b7e5-670">Metadata API changes</span></span>

[<span data-ttu-id="4b7e5-671">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="4b7e5-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="4b7e5-672">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-672">**New behavior**</span></span>

<span data-ttu-id="4b7e5-673">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="4b7e5-674">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-674">**Why**</span></span>

<span data-ttu-id="4b7e5-675">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="4b7e5-676">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-676">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-677">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="4b7e5-678">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="4b7e5-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="4b7e5-679">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="4b7e5-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="4b7e5-680">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-680">**New behavior**</span></span>

<span data-ttu-id="4b7e5-681">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="4b7e5-682">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-682">**Why**</span></span>

<span data-ttu-id="4b7e5-683">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="4b7e5-684">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-684">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-685">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="4b7e5-686">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="4b7e5-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="4b7e5-687">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="4b7e5-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="4b7e5-688">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-688">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-689">Przed EF Core 3,0, EF Core wyśle `PRAGMA foreign_keys = 1` w przypadku otwarcia połączenia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="4b7e5-690">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-690">**New behavior**</span></span>

<span data-ttu-id="4b7e5-691">Począwszy od EF Core 3,0, EF Core nie będzie już wysyłać `PRAGMA foreign_keys = 1` w przypadku otwarcia połączenia z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="4b7e5-692">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-692">**Why**</span></span>

<span data-ttu-id="4b7e5-693">Ta zmiana została wprowadzona, ponieważ EF Core domyślnie używa `SQLitePCLRaw.bundle_e_sqlite3`, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="4b7e5-694">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-694">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-695">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="4b7e5-696">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="4b7e5-697">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="4b7e5-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="4b7e5-698">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-698">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-699">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="4b7e5-700">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-700">**New behavior**</span></span>

<span data-ttu-id="4b7e5-701">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="4b7e5-702">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-702">**Why**</span></span>

<span data-ttu-id="4b7e5-703">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="4b7e5-704">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-704">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-705">Aby użyć natywnej wersji programu SQLite w systemie iOS, skonfiguruj `Microsoft.Data.Sqlite`, aby użyć innego pakietu `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="4b7e5-706">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="4b7e5-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="4b7e5-707">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="4b7e5-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="4b7e5-708">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-708">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-709">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="4b7e5-710">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-710">**New behavior**</span></span>

<span data-ttu-id="4b7e5-711">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="4b7e5-712">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-712">**Why**</span></span>

<span data-ttu-id="4b7e5-713">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="4b7e5-714">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="4b7e5-715">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-715">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-716">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="4b7e5-717">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="4b7e5-718">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="4b7e5-719">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="4b7e5-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="4b7e5-720">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="4b7e5-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="4b7e5-721">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-721">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-722">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="4b7e5-723">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="4b7e5-724">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-724">**New behavior**</span></span>

<span data-ttu-id="4b7e5-725">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="4b7e5-726">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-726">**Why**</span></span>

<span data-ttu-id="4b7e5-727">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="4b7e5-728">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-728">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-729">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="4b7e5-730">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="4b7e5-731">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="4b7e5-732">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="4b7e5-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="4b7e5-733">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="4b7e5-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="4b7e5-734">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-734">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-735">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="4b7e5-736">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-736">**New behavior**</span></span>

<span data-ttu-id="4b7e5-737">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="4b7e5-738">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-738">**Why**</span></span>

<span data-ttu-id="4b7e5-739">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="4b7e5-740">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="4b7e5-741">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-741">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-742">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="4b7e5-743">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="4b7e5-744">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="4b7e5-745">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="4b7e5-746">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="4b7e5-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="4b7e5-747">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="4b7e5-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="4b7e5-748">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-748">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-749">Przed EF Core 3,0, `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="4b7e5-750">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-750">**New behavior**</span></span>

<span data-ttu-id="4b7e5-751">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="4b7e5-752">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-752">**Why**</span></span>

<span data-ttu-id="4b7e5-753">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="4b7e5-754">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-754">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-755">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="4b7e5-756">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="4b7e5-757">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="4b7e5-758">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="4b7e5-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="4b7e5-759">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="4b7e5-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="4b7e5-760">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-760">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-761">`IDbContextOptionsExtension` zawiera metody przesyłania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="4b7e5-762">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-762">**New behavior**</span></span>

<span data-ttu-id="4b7e5-763">Te metody zostały przeniesione na nową abstrakcyjną klasę bazową `DbContextOptionsExtensionInfo`, która jest zwracana z nowej właściwości `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="4b7e5-764">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-764">**Why**</span></span>

<span data-ttu-id="4b7e5-765">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="4b7e5-766">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="4b7e5-767">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-767">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-768">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="4b7e5-769">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="4b7e5-770">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="4b7e5-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="4b7e5-771">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="4b7e5-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="4b7e5-772">**Stąp**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-772">**Change**</span></span>

<span data-ttu-id="4b7e5-773">Zmieniono nazwę `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="4b7e5-774">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-774">**Why**</span></span>

<span data-ttu-id="4b7e5-775">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="4b7e5-776">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-776">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-777">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-777">Use the new name.</span></span> <span data-ttu-id="4b7e5-778">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="4b7e5-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="4b7e5-779">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="4b7e5-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="4b7e5-780">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="4b7e5-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="4b7e5-781">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-781">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-782">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="4b7e5-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="4b7e5-783">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="4b7e5-784">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-784">**New behavior**</span></span>

<span data-ttu-id="4b7e5-785">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="4b7e5-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="4b7e5-786">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="4b7e5-787">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-787">**Why**</span></span>

<span data-ttu-id="4b7e5-788">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="4b7e5-789">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-789">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-790">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="4b7e5-791">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="4b7e5-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="4b7e5-792">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="4b7e5-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="4b7e5-793">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-793">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-794">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="4b7e5-795">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-795">**New behavior**</span></span>

<span data-ttu-id="4b7e5-796">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="4b7e5-797">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-797">**Why**</span></span>

<span data-ttu-id="4b7e5-798">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="4b7e5-799">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="4b7e5-800">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-800">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-801">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="4b7e5-802">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="4b7e5-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="4b7e5-803">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="4b7e5-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="4b7e5-804">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-804">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-805">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="4b7e5-806">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-806">**New behavior**</span></span>

<span data-ttu-id="4b7e5-807">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="4b7e5-808">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="4b7e5-809">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-809">**Why**</span></span>

<span data-ttu-id="4b7e5-810">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="4b7e5-811">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="4b7e5-812">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="4b7e5-813">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-813">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-814">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="4b7e5-815">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="4b7e5-816">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="4b7e5-817">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="4b7e5-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="4b7e5-818">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-818">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-819">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="4b7e5-820">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-820">**New behavior**</span></span>

<span data-ttu-id="4b7e5-821">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="4b7e5-822">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-822">**Why**</span></span>

<span data-ttu-id="4b7e5-823">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="4b7e5-824">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="4b7e5-825">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-825">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-826">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="4b7e5-827">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="4b7e5-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="4b7e5-828">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4b7e5-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="4b7e5-829">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="4b7e5-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="4b7e5-830">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-830">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-831">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="4b7e5-832">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-832">**New behavior**</span></span>

<span data-ttu-id="4b7e5-833">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="4b7e5-834">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-834">**Why**</span></span>

<span data-ttu-id="4b7e5-835">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="4b7e5-836">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-836">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-837">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="4b7e5-838">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="4b7e5-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="4b7e5-839">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="4b7e5-840">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="4b7e5-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="4b7e5-841">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-841">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-842">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="4b7e5-843">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-843">For example:</span></span>

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

<span data-ttu-id="4b7e5-844">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-844">**New behavior**</span></span>

<span data-ttu-id="4b7e5-845">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="4b7e5-846">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-846">**Why**</span></span>

<span data-ttu-id="4b7e5-847">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="4b7e5-848">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-848">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-849">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="4b7e5-850">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b7e5-850">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="4b7e5-851">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="4b7e5-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="4b7e5-852">Śledzenie problemu #12757</span><span class="sxs-lookup"><span data-stu-id="4b7e5-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="4b7e5-853">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-853">**Old behavior**</span></span>

<span data-ttu-id="4b7e5-854">Funkcja dbzostała skonfigurowana ze schematem jako pusty ciąg był traktowany jak wbudowana funkcja bez schematu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="4b7e5-855">Na przykład poniższy kod mapuje funkcję CLR `DatePart` na funkcję wbudowaną `DATEPART` na serwerze SqlServer.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="4b7e5-856">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-856">**New behavior**</span></span>

<span data-ttu-id="4b7e5-857">Wszystkie mapowania funkcji dbfunction są uznawane za mapowane do funkcji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="4b7e5-858">W związku z tym wartość pustego ciągu spowodowałaby umieszczenie funkcji wewnątrz domyślnego schematu modelu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="4b7e5-859">Może to być schemat skonfigurowany jawnie za pośrednictwem interfejsu API Fluent `modelBuilder.HasDefaultSchema()` lub `dbo` w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="4b7e5-860">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-860">**Why**</span></span>

<span data-ttu-id="4b7e5-861">Poprzednio pusty schemat był sposobem traktowania tej funkcji, ale ta logika jest stosowana tylko dla SqlServer, w której wbudowane funkcje nie należą do żadnego schematu.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="4b7e5-862">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="4b7e5-862">**Mitigations**</span></span>

<span data-ttu-id="4b7e5-863">Skonfiguruj ręcznie tłumaczenie funkcji dbfunction, aby zamapować ją na wbudowaną funkcję.</span><span class="sxs-lookup"><span data-stu-id="4b7e5-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
