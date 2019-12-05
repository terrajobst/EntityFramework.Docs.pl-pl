---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: d614103169837238810fabd0a8889043c851ef14
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824866"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="ffa9b-102">Istotne zmiany zawarte w EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="ffa9b-103">Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="ffa9b-104">Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="ffa9b-105">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-105">Summary</span></span>

| <span data-ttu-id="ffa9b-106">**Zmiana powodująca niezgodność**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="ffa9b-107">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="ffa9b-108">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="ffa9b-109">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-109">High</span></span>       |
| [<span data-ttu-id="ffa9b-110">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="ffa9b-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-111">High</span></span>      |
| [<span data-ttu-id="ffa9b-112">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ffa9b-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="ffa9b-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-113">High</span></span>      |
| [<span data-ttu-id="ffa9b-114">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="ffa9b-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="ffa9b-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-115">High</span></span>      |
| [<span data-ttu-id="ffa9b-116">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="ffa9b-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="ffa9b-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-117">High</span></span>      |
| [<span data-ttu-id="ffa9b-118">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="ffa9b-119">Wysoka</span><span class="sxs-lookup"><span data-stu-id="ffa9b-119">High</span></span>      |
| [<span data-ttu-id="ffa9b-120">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="ffa9b-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="ffa9b-121">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-121">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-122">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="ffa9b-123">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-123">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-124">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="ffa9b-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="ffa9b-125">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-125">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-126">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="ffa9b-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="ffa9b-127">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-127">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-128">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="ffa9b-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="ffa9b-129">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-129">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-130">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="ffa9b-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="ffa9b-131">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-131">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-132">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="ffa9b-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="ffa9b-133">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-133">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-134">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="ffa9b-135">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-135">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-136">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="ffa9b-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="ffa9b-137">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-137">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-138">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="ffa9b-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="ffa9b-139">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-139">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-140">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="ffa9b-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="ffa9b-141">Średnia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-141">Medium</span></span>      |
| [<span data-ttu-id="ffa9b-142">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="ffa9b-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="ffa9b-143">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-143">Low</span></span>      |
| [<span data-ttu-id="ffa9b-144">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="ffa9b-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="ffa9b-145">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-145">Low</span></span>      |
| [<span data-ttu-id="ffa9b-146">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="ffa9b-147">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-147">Low</span></span>      |
| [<span data-ttu-id="ffa9b-148">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="ffa9b-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="ffa9b-149">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-149">Low</span></span>      |
| [<span data-ttu-id="ffa9b-150">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="ffa9b-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="ffa9b-151">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-151">Low</span></span>      |
| [<span data-ttu-id="ffa9b-152">Nie można wykonać zapytania dotyczącego jednostek będących własnością, bez właściciela przy użyciu zapytania śledzenia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="ffa9b-153">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-153">Low</span></span>      |
| [<span data-ttu-id="ffa9b-154">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="ffa9b-155">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-155">Low</span></span>      |
| [<span data-ttu-id="ffa9b-156">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="ffa9b-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="ffa9b-157">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-157">Low</span></span>      |
| [<span data-ttu-id="ffa9b-158">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="ffa9b-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="ffa9b-159">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-159">Low</span></span>      |
| [<span data-ttu-id="ffa9b-160">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="ffa9b-161">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-161">Low</span></span>      |
| [<span data-ttu-id="ffa9b-162">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="ffa9b-163">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-163">Low</span></span>      |
| [<span data-ttu-id="ffa9b-164">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="ffa9b-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="ffa9b-165">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-165">Low</span></span>      |
| [<span data-ttu-id="ffa9b-166">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ffa9b-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="ffa9b-167">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-167">Low</span></span>      |
| [<span data-ttu-id="ffa9b-168">AddEntityFramework \* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="ffa9b-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="ffa9b-169">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-169">Low</span></span>      |
| [<span data-ttu-id="ffa9b-170">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="ffa9b-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="ffa9b-171">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-171">Low</span></span>      |
| [<span data-ttu-id="ffa9b-172">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="ffa9b-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="ffa9b-173">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-173">Low</span></span>      |
| [<span data-ttu-id="ffa9b-174">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="ffa9b-175">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-175">Low</span></span>      |
| [<span data-ttu-id="ffa9b-176">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="ffa9b-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="ffa9b-177">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-177">Low</span></span>      |
| [<span data-ttu-id="ffa9b-178">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="ffa9b-179">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-179">Low</span></span>      |
| [<span data-ttu-id="ffa9b-180">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="ffa9b-181">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-181">Low</span></span>      |
| [<span data-ttu-id="ffa9b-182">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="ffa9b-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="ffa9b-183">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-183">Low</span></span>      |
| [<span data-ttu-id="ffa9b-184">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="ffa9b-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="ffa9b-185">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-185">Low</span></span>      |
| [<span data-ttu-id="ffa9b-186">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="ffa9b-187">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-187">Low</span></span>      |
| [<span data-ttu-id="ffa9b-188">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="ffa9b-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="ffa9b-189">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-189">Low</span></span>      |
| [<span data-ttu-id="ffa9b-190">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="ffa9b-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="ffa9b-191">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-191">Low</span></span>      |
| [<span data-ttu-id="ffa9b-192">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="ffa9b-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="ffa9b-193">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-193">Low</span></span>      |
| [<span data-ttu-id="ffa9b-194">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="ffa9b-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="ffa9b-195">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-195">Low</span></span>      |
| [<span data-ttu-id="ffa9b-196">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="ffa9b-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="ffa9b-197">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-197">Low</span></span>      |
| [<span data-ttu-id="ffa9b-198">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="ffa9b-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="ffa9b-199">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-199">Low</span></span>      |
| [<span data-ttu-id="ffa9b-200">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="ffa9b-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="ffa9b-201">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-201">Low</span></span>      |
| [<span data-ttu-id="ffa9b-202">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="ffa9b-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="ffa9b-203">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-203">Low</span></span>      |
| [<span data-ttu-id="ffa9b-204">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="ffa9b-205">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-205">Low</span></span>      |
| [<span data-ttu-id="ffa9b-206">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="ffa9b-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="ffa9b-207">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-207">Low</span></span>      |
| [<span data-ttu-id="ffa9b-208">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="ffa9b-209">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-209">Low</span></span>      |
| [<span data-ttu-id="ffa9b-210">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="ffa9b-211">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-211">Low</span></span>      |
| [<span data-ttu-id="ffa9b-212">Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="ffa9b-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="ffa9b-213">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-213">Low</span></span>      |
| [<span data-ttu-id="ffa9b-214">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="ffa9b-215">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-215">Low</span></span>      |
| [<span data-ttu-id="ffa9b-216">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="ffa9b-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="ffa9b-217">Niski</span><span class="sxs-lookup"><span data-stu-id="ffa9b-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="ffa9b-218">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="ffa9b-219">[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[zobacz również problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="ffa9b-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="ffa9b-220">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-220">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-221">Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="ffa9b-222">Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="ffa9b-223">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-223">**New behavior**</span></span>

<span data-ttu-id="ffa9b-224">Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołaniu zapytania) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="ffa9b-225">Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="ffa9b-226">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-226">**Why**</span></span>

<span data-ttu-id="ffa9b-227">Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="ffa9b-228">Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="ffa9b-229">Na przykład warunek w wywołaniu `Where()`, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych oraz filtr, który ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="ffa9b-230">Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="ffa9b-231">Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="ffa9b-232">W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="ffa9b-233">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-233">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-234">Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć, lub użyć `AsEnumerable()`, `ToList()`lub podobnego do jawnego przywrócenia danych z powrotem do klienta, na którym można następnie przetworzyć je przy użyciu LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="ffa9b-235">EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="ffa9b-236">Śledzenie problemu #15498</span><span class="sxs-lookup"><span data-stu-id="ffa9b-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="ffa9b-237">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-237">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-238">Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-238">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="ffa9b-239">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-239">**New behavior**</span></span>

<span data-ttu-id="ffa9b-240">Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-240">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="ffa9b-241">Nie obejmuje to .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-241">This does not include .NET Framework.</span></span>

<span data-ttu-id="ffa9b-242">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-242">**Why**</span></span>

<span data-ttu-id="ffa9b-243">Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-243">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="ffa9b-244">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-244">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-245">Rozważ przeniesienie do nowoczesnej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-245">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="ffa9b-246">Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-246">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="ffa9b-247">Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury</span><span class="sxs-lookup"><span data-stu-id="ffa9b-247">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="ffa9b-248">Śledzenie anonsów problemów # 325</span><span class="sxs-lookup"><span data-stu-id="ffa9b-248">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="ffa9b-249">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-249">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-250">Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`będzie ona obejmować EF Core i niektórych spośród EF Core dostawców danych, takich jak Dostawca SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-250">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="ffa9b-251">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-251">**New behavior**</span></span>

<span data-ttu-id="ffa9b-252">Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-252">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="ffa9b-253">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-253">**Why**</span></span>

<span data-ttu-id="ffa9b-254">Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-254">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="ffa9b-255">Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-255">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="ffa9b-256">W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-256">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="ffa9b-257">Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-257">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="ffa9b-258">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-258">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-259">Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-259">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="ffa9b-260">Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ffa9b-260">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="ffa9b-261">Śledzenie problemu #14016</span><span class="sxs-lookup"><span data-stu-id="ffa9b-261">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="ffa9b-262">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-262">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-263">Przed 3,0 Narzędzie `dotnet ef` zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-263">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="ffa9b-264">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-264">**New behavior**</span></span>

<span data-ttu-id="ffa9b-265">Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera narzędzia `dotnet ef`, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-265">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="ffa9b-266">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-266">**Why**</span></span>

<span data-ttu-id="ffa9b-267">Ta zmiana umożliwia nam dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-267">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="ffa9b-268">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-268">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-269">Aby móc zarządzać migracjami lub szkieletem `DbContext`, zainstaluj `dotnet-ef` jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-269">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="ffa9b-270">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-270">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="ffa9b-271">Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="ffa9b-271">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="ffa9b-272">Śledzenie problemu #10996</span><span class="sxs-lookup"><span data-stu-id="ffa9b-272">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="ffa9b-273">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-273">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-274">Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-274">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="ffa9b-275">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-275">**New behavior**</span></span>

<span data-ttu-id="ffa9b-276">Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw`i `ExecuteSqlRawAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-276">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="ffa9b-277">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-277">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="ffa9b-278">Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated`i `ExecuteSqlInterpolatedAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane w ramach interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-278">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="ffa9b-279">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-279">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="ffa9b-280">Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-280">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="ffa9b-281">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-281">**Why**</span></span>

<span data-ttu-id="ffa9b-282">Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-282">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="ffa9b-283">Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-283">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="ffa9b-284">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-284">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-285">Przełącz, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-285">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="ffa9b-286">Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną</span><span class="sxs-lookup"><span data-stu-id="ffa9b-286">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="ffa9b-287">Śledzenie problemu #15392</span><span class="sxs-lookup"><span data-stu-id="ffa9b-287">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="ffa9b-288">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-288">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-289">Przed EF Core 3,0, Metoda Z tabel podjęła próbę wykrycia, czy może być złożona pozostała wartość SQL.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-289">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="ffa9b-290">Klient przeprowadził ocenę, gdy nie udało się utworzyć kodu SQL, podobnie jak procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-290">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="ffa9b-291">Następujące zapytanie działało przez uruchomienie procedury składowanej na serwerze i wykonanie FirstOrDefault po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-291">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="ffa9b-292">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-292">**New behavior**</span></span>

<span data-ttu-id="ffa9b-293">Począwszy od EF Core 3,0, EF Core nie będzie próbować przeanalizować bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-293">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="ffa9b-294">Dlatego w przypadku redagowania po FromSqlRaw/FromSqlInterpolated, EF Core nastąpi złożenie zapytania podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-294">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="ffa9b-295">Dlatego jeśli używasz procedury składowanej z kompozycją, zostanie pobrany wyjątek dla nieprawidłowej składni SQL.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-295">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="ffa9b-296">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-296">**Why**</span></span>

<span data-ttu-id="ffa9b-297">EF Core 3,0 nie obsługuje automatycznej oceny klienta, ponieważ została ona podatna na błędy, jak wyjaśniono [tutaj](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-297">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="ffa9b-298">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-298">**Mitigation**</span></span>

<span data-ttu-id="ffa9b-299">Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można jej składować, więc możesz dodać __AsEnumerable/AsAsyncEnumerable__ bezpośrednio po wywołaniu metody z tabel, aby uniknąć jakiegokolwiek złożenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-299">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="ffa9b-300">Metody Z tabel można określić tylko dla katalogów głównych zapytań</span><span class="sxs-lookup"><span data-stu-id="ffa9b-300">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="ffa9b-301">Śledzenie problemu #15704</span><span class="sxs-lookup"><span data-stu-id="ffa9b-301">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="ffa9b-302">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-302">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-303">Przed EF Core 3,0 można określić metodę `FromSql` w dowolnym miejscu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-303">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="ffa9b-304">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-304">**New behavior**</span></span>

<span data-ttu-id="ffa9b-305">Począwszy od EF Core 3,0, nowe metody `FromSqlRaw` i `FromSqlInterpolated` (które zastępują `FromSql`) można określić tylko dla katalogów głównych zapytań, tj. bezpośrednio na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-305">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="ffa9b-306">Próba określenia ich w innym miejscu spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-306">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="ffa9b-307">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-307">**Why**</span></span>

<span data-ttu-id="ffa9b-308">Określanie `FromSql` w dowolnym miejscu niż na `DbSet` nie miało znaczenia ani dodanej wartości i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-308">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="ffa9b-309">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-309">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-310">wywołania `FromSql` powinny zostać przeniesione bezpośrednio na `DbSet`, do których mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-310">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="ffa9b-311">Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="ffa9b-311">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="ffa9b-312">Śledzenie problemu #13518</span><span class="sxs-lookup"><span data-stu-id="ffa9b-312">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="ffa9b-313">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-313">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-314">Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-314">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="ffa9b-315">Jest to zgodne z zachowaniem śledzenia zapytań.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-315">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="ffa9b-316">Na przykład to zapytanie:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-316">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="ffa9b-317">zwróci to samo wystąpienie `Category` dla każdej `Product` skojarzonej z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-317">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="ffa9b-318">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-318">**New behavior**</span></span>

<span data-ttu-id="ffa9b-319">Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-319">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="ffa9b-320">Na przykład zapytanie powyżej zwróci teraz nowe wystąpienie `Category` dla każdej `Product`, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-320">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="ffa9b-321">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-321">**Why**</span></span>

<span data-ttu-id="ffa9b-322">Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-322">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="ffa9b-323">Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-323">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="ffa9b-324">Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-324">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="ffa9b-325">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-325">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-326">Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-326">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="ffa9b-327">~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono</span><span class="sxs-lookup"><span data-stu-id="ffa9b-327">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="ffa9b-328">Śledzenie problemu #14523</span><span class="sxs-lookup"><span data-stu-id="ffa9b-328">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="ffa9b-329">Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-329">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="ffa9b-330">Aby na przykład przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-330">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="ffa9b-331">Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-331">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="ffa9b-332">Śledzenie problemu #12378</span><span class="sxs-lookup"><span data-stu-id="ffa9b-332">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="ffa9b-333">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-333">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-334">Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-334">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="ffa9b-335">Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-335">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="ffa9b-336">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-336">**New behavior**</span></span>

<span data-ttu-id="ffa9b-337">Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-337">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="ffa9b-338">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-338">**Why**</span></span>

<span data-ttu-id="ffa9b-339">Ta zmiana została wprowadzona w celu zapobieżenia błędnemu utracie wartości klucza tymczasowego, gdy jednostka, która została wcześniej prześledzona przez niektóre `DbContext` wystąpienia, zostanie przeniesiona do innego wystąpienia `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-339">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="ffa9b-340">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-340">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-341">Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w stanie `Added`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-341">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="ffa9b-342">Można to uniknąć przez:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-342">This can be avoided by:</span></span>
* <span data-ttu-id="ffa9b-343">Nie używa kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-343">Not using store-generated keys.</span></span>
* <span data-ttu-id="ffa9b-344">Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-344">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="ffa9b-345">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-345">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="ffa9b-346">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową, nawet jeśli nie został ustawiony `blog.Id` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-346">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="ffa9b-347">DetectChanges uznaje wartości klucza generowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="ffa9b-347">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="ffa9b-348">Śledzenie problemu #14616</span><span class="sxs-lookup"><span data-stu-id="ffa9b-348">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="ffa9b-349">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-349">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-350">Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` byłaby śledzona w `Added` stanie i wstawiona jako nowy wiersz po wywołaniu `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-350">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="ffa9b-351">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-351">**New behavior**</span></span>

<span data-ttu-id="ffa9b-352">Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono wartość klucza, jednostka będzie śledzona w stanie `Modified`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-352">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="ffa9b-353">Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany po wywołaniu `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-353">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="ffa9b-354">Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona jako `Added` jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-354">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="ffa9b-355">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-355">**Why**</span></span>

<span data-ttu-id="ffa9b-356">Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-356">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="ffa9b-357">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-357">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-358">Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-358">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="ffa9b-359">Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-359">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="ffa9b-360">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-360">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="ffa9b-361">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-361">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="ffa9b-362">Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-362">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="ffa9b-363">Śledzenie problemu #10114</span><span class="sxs-lookup"><span data-stu-id="ffa9b-363">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="ffa9b-364">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-364">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-365">Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-365">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="ffa9b-366">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-366">**New behavior**</span></span>

<span data-ttu-id="ffa9b-367">Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-367">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="ffa9b-368">Na przykład wywołanie `context.Remove()` w celu usunięcia podmiotu zabezpieczeń spowoduje również, że wszystkie śledzone powiązane wymagane elementy zależne są ustawiane na `Deleted` natychmiast.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-368">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="ffa9b-369">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-369">**Why**</span></span>

<span data-ttu-id="ffa9b-370">Ta zmiana została wprowadzona w celu poprawy środowiska dla scenariuszy powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ wywołaniem `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-370">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="ffa9b-371">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-371">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-372">Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-372">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="ffa9b-373">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-373">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="ffa9b-374">Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu</span><span class="sxs-lookup"><span data-stu-id="ffa9b-374">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="ffa9b-375">Śledzenie problemu #18022</span><span class="sxs-lookup"><span data-stu-id="ffa9b-375">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="ffa9b-376">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-376">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-377">Przed 3,0 eagerly załadowanie nawigacji kolekcji za pośrednictwem operatorów `Include` spowodowało generowanie wielu zapytań w relacyjnej bazie danych, po jednej dla każdego powiązanego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-377">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="ffa9b-378">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-378">**New behavior**</span></span>

<span data-ttu-id="ffa9b-379">Począwszy od 3,0, EF Core generuje pojedyncze zapytanie z sprzężeniami w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-379">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="ffa9b-380">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-380">**Why**</span></span>

<span data-ttu-id="ffa9b-381">Wygenerowanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ powodowało wiele problemów, w tym negatywną wydajność, ponieważ wiele danych jest niezbędnych, a błędy spójność danych, ponieważ każde zapytanie może obserwować inny stan bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-381">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="ffa9b-382">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-382">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-383">Chociaż technicznie nie jest to istotna zmiana, może ona mieć znaczny wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę `Include` operatora na nawigowaniu po kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-383">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="ffa9b-384">[Zobacz ten komentarz](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , aby uzyskać więcej informacji i przepisać zapytania w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-384">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="ffa9b-385">DeleteBehavior. ograniczanie ma semantykę oczyszczarki</span><span class="sxs-lookup"><span data-stu-id="ffa9b-385">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="ffa9b-386">Śledzenie problemu #12661</span><span class="sxs-lookup"><span data-stu-id="ffa9b-386">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="ffa9b-387">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-387">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-388">Przed 3,0 `DeleteBehavior.Restrict` utworzone klucze obce w bazie danych z semantyką `Restrict`, ale również zmieniono wewnętrzną korektę w niewidoczny sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-388">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="ffa9b-389">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-389">**New behavior**</span></span>

<span data-ttu-id="ffa9b-390">Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone z semantyką `Restrict`, czyli bez kaskad; Zgłoś naruszenie ograniczenia — bez również wpływu na wewnętrzną korektę EF.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-390">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="ffa9b-391">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-391">**Why**</span></span>

<span data-ttu-id="ffa9b-392">Ta zmiana została wprowadzona w celu poprawy środowiska korzystania z `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-392">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="ffa9b-393">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-393">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-394">Poprzednie zachowanie można przywrócić przy użyciu `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-394">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="ffa9b-395">Typy zapytań są konsolidowane z typami jednostek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-395">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="ffa9b-396">Śledzenie problemu #14194</span><span class="sxs-lookup"><span data-stu-id="ffa9b-396">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="ffa9b-397">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-397">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-398">Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-398">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="ffa9b-399">Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-399">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="ffa9b-400">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-400">**New behavior**</span></span>

<span data-ttu-id="ffa9b-401">Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-401">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="ffa9b-402">Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-402">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="ffa9b-403">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-403">**Why**</span></span>

<span data-ttu-id="ffa9b-404">Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-404">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="ffa9b-405">W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-405">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="ffa9b-406">Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-406">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="ffa9b-407">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-407">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-408">Następujące części interfejsu API są obecnie przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-408">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="ffa9b-409">należy wywołać `ModelBuilder.Entity<>().HasNoKey()` **`ModelBuilder.Query<>()`** , aby oznaczyć typ jednostki jako bez kluczy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-409">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="ffa9b-410">Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-410">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="ffa9b-411">należy użyć `DbSet<>` **`DbQuery<>`** .</span><span class="sxs-lookup"><span data-stu-id="ffa9b-411">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="ffa9b-412">należy użyć `DbContext.Set<>()` **`DbContext.Query<>()`** .</span><span class="sxs-lookup"><span data-stu-id="ffa9b-412">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="ffa9b-413">Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony</span><span class="sxs-lookup"><span data-stu-id="ffa9b-413">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="ffa9b-414">Problem ze śledzeniem [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
śledzenia [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148) problem ze [śledzeniem
#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="ffa9b-414">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="ffa9b-415">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-415">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-416">Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po wywołaniu `OwnsOne` lub `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-416">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="ffa9b-417">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-417">**New behavior**</span></span>

<span data-ttu-id="ffa9b-418">Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-418">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="ffa9b-419">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-419">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="ffa9b-420">Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem po `WithOwner()` podobnie jak inne relacje są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-420">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="ffa9b-421">Mimo że konfiguracja dla samego typu, nadal będzie łańcucha po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-421">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="ffa9b-422">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-422">For example:</span></span>

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

<span data-ttu-id="ffa9b-423">Dodatkowo wywoływanie `Entity()`, `HasOne()`lub `Set()` z elementem docelowym typu będącego własnością spowoduje teraz zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-423">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="ffa9b-424">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-424">**Why**</span></span>

<span data-ttu-id="ffa9b-425">Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-425">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="ffa9b-426">To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, takich jak `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-426">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="ffa9b-427">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-427">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-428">Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-428">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="ffa9b-429">Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="ffa9b-429">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="ffa9b-430">Śledzenie problemu #9005</span><span class="sxs-lookup"><span data-stu-id="ffa9b-430">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="ffa9b-431">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-431">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-432">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-432">Consider the following model:</span></span>
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
<span data-ttu-id="ffa9b-433">Przed EF Core 3,0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zamapowane do tej samej tabeli, wystąpienie `OrderDetails` było zawsze wymagane podczas dodawania nowego `Order`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-433">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="ffa9b-434">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-434">**New behavior**</span></span>

<span data-ttu-id="ffa9b-435">Począwszy od 3,0, EF Core umożliwia dodanie `Order` bez `OrderDetails` i mapuje wszystkie właściwości `OrderDetails` z wyjątkiem klucza podstawowego na kolumny dopuszczające wartość null.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-435">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="ffa9b-436">Podczas wykonywania zapytania o EF Core zestawy `OrderDetails` do `null`, jeśli żadna z jej wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-436">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="ffa9b-437">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-437">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-438">Jeśli Twój model ma zależne od siebie wszystkie opcjonalne kolumny, ale nawigacja nie powinna być `null`, należy ją zmodyfikować, aby obsługiwać przypadki, gdy nawigacja jest `null`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-438">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="ffa9b-439">Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć przypisaną wartość inną niż`null`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-439">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="ffa9b-440">Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość</span><span class="sxs-lookup"><span data-stu-id="ffa9b-440">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="ffa9b-441">Śledzenie problemu #14154</span><span class="sxs-lookup"><span data-stu-id="ffa9b-441">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="ffa9b-442">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-442">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-443">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-443">Consider the following model:</span></span>
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
<span data-ttu-id="ffa9b-444">Przed EF Core 3,0, jeśli `OrderDetails` jest własnością `Order` lub jawnie zamapowane do tej samej tabeli, aktualizacja tylko `OrderDetails` nie spowoduje aktualizacji wartości `Version` na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-444">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="ffa9b-445">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-445">**New behavior**</span></span>

<span data-ttu-id="ffa9b-446">Począwszy od 3,0, EF Core propaguje nową wartość `Version` do `Order`, jeśli należy ona do `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-446">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="ffa9b-447">W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-447">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="ffa9b-448">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-448">**Why**</span></span>

<span data-ttu-id="ffa9b-449">Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-449">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="ffa9b-450">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-450">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-451">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-451">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="ffa9b-452">Możliwe jest utworzenie jednego w tle:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-452">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="ffa9b-453">Nie można wykonać zapytania dotyczącego jednostek będących własnością, bez właściciela przy użyciu zapytania śledzenia</span><span class="sxs-lookup"><span data-stu-id="ffa9b-453">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="ffa9b-454">Śledzenie problemu #18876</span><span class="sxs-lookup"><span data-stu-id="ffa9b-454">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="ffa9b-455">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-455">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-456">Przed EF Core 3,0 jednostki będące własnością mogą być badane jako każda inna nawigacja.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-456">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="ffa9b-457">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-457">**New behavior**</span></span>

<span data-ttu-id="ffa9b-458">Począwszy od 3,0, EF Core zostanie zgłoszony, jeśli zapytanie śledzenia projektuje jednostkę będącą własnością, bez właściciela.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-458">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="ffa9b-459">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-459">**Why**</span></span>

<span data-ttu-id="ffa9b-460">Elementy należące do użytkownika nie mogą być manipulowane bez właściciela, więc w większości przypadków, w których zapytania są w ten sposób, są błędem.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-460">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="ffa9b-461">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-461">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-462">Jeśli element będący własnością ma być śledzony do zmodyfikowania w dowolny sposób, właściciel powinien zostać uwzględniony w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-462">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="ffa9b-463">W przeciwnym razie Dodaj wywołanie `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-463">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="ffa9b-464">Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-464">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="ffa9b-465">Śledzenie problemu #13998</span><span class="sxs-lookup"><span data-stu-id="ffa9b-465">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="ffa9b-466">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-466">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-467">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-467">Consider the following model:</span></span>
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

<span data-ttu-id="ffa9b-468">Przed EF Core 3,0 Właściwość `ShippingAddress` zostanie zmapowana do oddzielnych kolumn dla `BulkOrder` i `Order` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-468">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="ffa9b-469">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-469">**New behavior**</span></span>

<span data-ttu-id="ffa9b-470">Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-470">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="ffa9b-471">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-471">**Why**</span></span>

<span data-ttu-id="ffa9b-472">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-472">The old behavoir was unexpected.</span></span>

<span data-ttu-id="ffa9b-473">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-473">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-474">Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-474">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="ffa9b-475">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="ffa9b-475">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="ffa9b-476">Śledzenie problemu #13274</span><span class="sxs-lookup"><span data-stu-id="ffa9b-476">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="ffa9b-477">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-477">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-478">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-478">Consider the following model:</span></span>
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
<span data-ttu-id="ffa9b-479">Przed EF Core 3,0 Właściwość `CustomerId` byłaby użyta dla klucza obcego według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-479">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="ffa9b-480">Jeśli jednak `Order` jest typem będącym własnością, to również `CustomerId` klucz podstawowy i zwykle nie jest to oczekiwane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-480">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="ffa9b-481">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-481">**New behavior**</span></span>

<span data-ttu-id="ffa9b-482">Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-482">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="ffa9b-483">Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-483">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="ffa9b-484">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-484">For example:</span></span>

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

<span data-ttu-id="ffa9b-485">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-485">**Why**</span></span>

<span data-ttu-id="ffa9b-486">Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-486">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="ffa9b-487">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-487">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-488">Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-488">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="ffa9b-489">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="ffa9b-489">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="ffa9b-490">Śledzenie problemu #14218</span><span class="sxs-lookup"><span data-stu-id="ffa9b-490">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="ffa9b-491">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-491">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-492">Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, gdy bieżące `TransactionScope` jest aktywne.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-492">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="ffa9b-493">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-493">**New behavior**</span></span>

<span data-ttu-id="ffa9b-494">Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-494">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="ffa9b-495">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-495">**Why**</span></span>

<span data-ttu-id="ffa9b-496">Ta zmiana umożliwia używanie wielu kontekstów w tym samym `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-496">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="ffa9b-497">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-497">The new behavior also matches EF6.</span></span>

<span data-ttu-id="ffa9b-498">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-498">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-499">Jeśli połączenie musi pozostać otwarte jawne wywołanie do `OpenConnection()`, zapewni, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-499">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="ffa9b-500">Każda właściwość używa niezależnej generacji klucza w pamięci</span><span class="sxs-lookup"><span data-stu-id="ffa9b-500">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="ffa9b-501">Śledzenie problemu #6872</span><span class="sxs-lookup"><span data-stu-id="ffa9b-501">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="ffa9b-502">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-502">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-503">Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-503">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="ffa9b-504">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-504">**New behavior**</span></span>

<span data-ttu-id="ffa9b-505">Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-505">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="ffa9b-506">Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-506">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="ffa9b-507">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-507">**Why**</span></span>

<span data-ttu-id="ffa9b-508">Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-508">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="ffa9b-509">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-509">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-510">Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-510">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="ffa9b-511">Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-511">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="ffa9b-512">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-512">Backing fields are used by default</span></span>

[<span data-ttu-id="ffa9b-513">Śledzenie problemu #12430</span><span class="sxs-lookup"><span data-stu-id="ffa9b-513">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="ffa9b-514">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-514">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-515">Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-515">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="ffa9b-516">Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-516">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="ffa9b-517">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-517">**New behavior**</span></span>

<span data-ttu-id="ffa9b-518">Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-518">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="ffa9b-519">Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-519">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="ffa9b-520">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-520">**Why**</span></span>

<span data-ttu-id="ffa9b-521">Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-521">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="ffa9b-522">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-522">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-523">Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu dostępu do właściwości w `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-523">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="ffa9b-524">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-524">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="ffa9b-525">Zgłoś, czy znaleziono wiele zgodnych pól zapasowych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-525">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="ffa9b-526">Śledzenie problemu #12523</span><span class="sxs-lookup"><span data-stu-id="ffa9b-526">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="ffa9b-527">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-527">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-528">Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-528">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="ffa9b-529">Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-529">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="ffa9b-530">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-530">**New behavior**</span></span>

<span data-ttu-id="ffa9b-531">Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-531">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="ffa9b-532">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-532">**Why**</span></span>

<span data-ttu-id="ffa9b-533">Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-533">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="ffa9b-534">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-534">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-535">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-535">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="ffa9b-536">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-536">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="ffa9b-537">Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="ffa9b-537">Field-only property names should match the field name</span></span>

<span data-ttu-id="ffa9b-538">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-538">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-539">Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-539">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="ffa9b-540">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-540">**New behavior**</span></span>

<span data-ttu-id="ffa9b-541">Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-541">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="ffa9b-542">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-542">**Why**</span></span>

<span data-ttu-id="ffa9b-543">Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-543">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="ffa9b-544">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-544">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-545">Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-545">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="ffa9b-546">W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="ffa9b-546">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="ffa9b-547">AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ffa9b-547">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="ffa9b-548">Śledzenie problemu #14756</span><span class="sxs-lookup"><span data-stu-id="ffa9b-548">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="ffa9b-549">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-549">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-550">Przed EF Core 3,0, wywoływanie `AddDbContext` lub `AddDbContextPool` spowoduje również zarejestrowanie usługi rejestrowania i buforowania pamięci przy użyciu funkcji DI poprzez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-550">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="ffa9b-551">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-551">**New behavior**</span></span>

<span data-ttu-id="ffa9b-552">Począwszy od EF Core 3,0, `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-552">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="ffa9b-553">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-553">**Why**</span></span>

<span data-ttu-id="ffa9b-554">EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-554">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="ffa9b-555">Jeśli jednak `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-555">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="ffa9b-556">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-556">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-557">Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-557">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="ffa9b-558">AddEntityFramework \* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="ffa9b-558">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="ffa9b-559">Śledzenie problemu #12905</span><span class="sxs-lookup"><span data-stu-id="ffa9b-559">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="ffa9b-560">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-560">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-561">Przed EF Core 3,0, wywoływanie metod `AddEntityFramework*` również rejestruje usługi pamięci podręcznej z systemem DI bez limitu rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-561">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="ffa9b-562">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-562">**New behavior**</span></span>

<span data-ttu-id="ffa9b-563">Począwszy od EF Core 3,0, `AddEntityFramework*` zarejestruje usługę IMemoryCache z limitem rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-563">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="ffa9b-564">Jeśli jakiekolwiek inne usługi dodane w późniejszym czasie zależą od IMemoryCache, mogą szybko dotrzeć do domyślnego limitu powodującego wyjątki lub obniżoną wydajność.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-564">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="ffa9b-565">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-565">**Why**</span></span>

<span data-ttu-id="ffa9b-566">Użycie IMemoryCache bez ograniczenia może spowodować niekontrolowane użycie pamięci, jeśli wystąpi błąd w logice buforowania zapytań lub zapytania są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-566">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="ffa9b-567">Ustawienie domyślnego ograniczenia ogranicza potencjalny atak w systemie DoS.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-567">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="ffa9b-568">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-568">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-569">W większości przypadków wywoływanie `AddEntityFramework*` nie jest konieczne w przypadku wywołania `AddDbContext` lub `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-569">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="ffa9b-570">W związku z tym najlepszym rozwiązaniem jest usunięcie wywołania `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-570">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="ffa9b-571">Jeśli aplikacja wymaga tych usług, należy zarejestrować implementację IMemoryCache jawnie przy użyciu funkcji DI Container z góry [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-571">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="ffa9b-572">DbContext. entry wykonuje teraz lokalną DetectChanges</span><span class="sxs-lookup"><span data-stu-id="ffa9b-572">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="ffa9b-573">Śledzenie problemu #13552</span><span class="sxs-lookup"><span data-stu-id="ffa9b-573">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ffa9b-574">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-574">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-575">Przed EF Core 3,0 wywołanie `DbContext.Entry` mogłoby spowodować wykrycie zmian dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-575">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="ffa9b-576">Zapewnia to aktualność stanu ujawnionego w `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-576">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="ffa9b-577">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-577">**New behavior**</span></span>

<span data-ttu-id="ffa9b-578">Począwszy od EF Core 3,0, wywoływanie `DbContext.Entry` podejmie teraz próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-578">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="ffa9b-579">Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-579">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="ffa9b-580">Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false`, nawet to lokalne wykrywanie zmian zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-580">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="ffa9b-581">Inne metody, które powodują wykrycie zmiany — na przykład `ChangeTracker.Entries` i `SaveChanges`--nadal powodują pełne `DetectChanges` wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-581">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="ffa9b-582">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-582">**Why**</span></span>

<span data-ttu-id="ffa9b-583">Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania z `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-583">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="ffa9b-584">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-584">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-585">Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry`, aby upewnić się, że zachowanie poprzedzające 3,0.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-585">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="ffa9b-586">Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta</span><span class="sxs-lookup"><span data-stu-id="ffa9b-586">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="ffa9b-587">Śledzenie problemu #14617</span><span class="sxs-lookup"><span data-stu-id="ffa9b-587">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="ffa9b-588">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-588">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-589">Przed EF Core 3,0 można użyć właściwości klucza `string` i `byte[]` bez jawnego ustawienia wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-589">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="ffa9b-590">W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, serializowany do bajtów dla `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-590">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="ffa9b-591">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-591">**New behavior**</span></span>

<span data-ttu-id="ffa9b-592">Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-592">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="ffa9b-593">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-593">**Why**</span></span>

<span data-ttu-id="ffa9b-594">Ta zmiana została wprowadzona, ponieważ wygenerowane przez klienta `string`/wartości `byte[]` zwykle nie są przydatne, a zachowanie domyślne spowodowało trudne przyczyny dotyczące wygenerowanych wartości kluczy w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-594">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="ffa9b-595">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-595">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-596">Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-596">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="ffa9b-597">Na przykład za pomocą interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-597">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="ffa9b-598">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-598">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="ffa9b-599">ILoggerFactory jest teraz usługą objętą zakresem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-599">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="ffa9b-600">Śledzenie problemu #14698</span><span class="sxs-lookup"><span data-stu-id="ffa9b-600">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="ffa9b-601">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-601">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-602">Przed EF Core 3,0 `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-602">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="ffa9b-603">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-603">**New behavior**</span></span>

<span data-ttu-id="ffa9b-604">Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-604">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="ffa9b-605">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-605">**Why**</span></span>

<span data-ttu-id="ffa9b-606">Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z wystąpieniem `DbContext`, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak rozłożenie wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-606">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="ffa9b-607">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-607">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-608">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-608">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="ffa9b-609">To nie jest typowe.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-609">This isn't common.</span></span>
<span data-ttu-id="ffa9b-610">W takich przypadkach większość elementów będzie nadal działać, ale każda usługa singleton, która była zależna od `ILoggerFactory` będzie musiała zostać zmieniona w celu uzyskania `ILoggerFactory` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-610">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="ffa9b-611">Jeśli używasz takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak korzystasz z `ILoggerFactory`, dzięki czemu możemy lepiej zrozumieć, jak nie należy ponownie rozbić tego w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-611">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="ffa9b-612">Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="ffa9b-612">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="ffa9b-613">Śledzenie problemu #12780</span><span class="sxs-lookup"><span data-stu-id="ffa9b-613">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="ffa9b-614">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-614">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-615">Przed EF Core 3,0, gdy `DbContext` został usunięty, nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-615">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="ffa9b-616">Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-616">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="ffa9b-617">W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-617">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="ffa9b-618">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-618">**New behavior**</span></span>

<span data-ttu-id="ffa9b-619">Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-619">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="ffa9b-620">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-620">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="ffa9b-621">Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-621">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="ffa9b-622">Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-622">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="ffa9b-623">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-623">**Why**</span></span>

<span data-ttu-id="ffa9b-624">Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie podczas próby załadowania opóźnionego na usunięte wystąpienie `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-624">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="ffa9b-625">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-625">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-626">Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-626">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="ffa9b-627">Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-627">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="ffa9b-628">Śledzenie problemu #10236</span><span class="sxs-lookup"><span data-stu-id="ffa9b-628">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="ffa9b-629">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-629">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-630">Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-630">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="ffa9b-631">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-631">**New behavior**</span></span>

<span data-ttu-id="ffa9b-632">Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-632">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="ffa9b-633">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-633">**Why**</span></span>

<span data-ttu-id="ffa9b-634">Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-634">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="ffa9b-635">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-635">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-636">Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-636">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="ffa9b-637">Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-637">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="ffa9b-638">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-638">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="ffa9b-639">Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem</span><span class="sxs-lookup"><span data-stu-id="ffa9b-639">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="ffa9b-640">Śledzenie problemu #9171</span><span class="sxs-lookup"><span data-stu-id="ffa9b-640">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="ffa9b-641">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-641">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-642">Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-642">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="ffa9b-643">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-643">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="ffa9b-644">Kod wygląda tak, jak odnosi się `Samurai` do innego typu jednostki przy użyciu właściwości nawigacji `Entrance`, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-644">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="ffa9b-645">W rzeczywistości ten kod próbuje utworzyć relację z typem jednostki o nazwie `Entrance` bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-645">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="ffa9b-646">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-646">**New behavior**</span></span>

<span data-ttu-id="ffa9b-647">Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-647">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="ffa9b-648">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-648">**Why**</span></span>

<span data-ttu-id="ffa9b-649">Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-649">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="ffa9b-650">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-650">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-651">Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-651">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="ffa9b-652">Nie jest to typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-652">This is not common.</span></span>
<span data-ttu-id="ffa9b-653">Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-653">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="ffa9b-654">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-654">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="ffa9b-655">Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask</span><span class="sxs-lookup"><span data-stu-id="ffa9b-655">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="ffa9b-656">Śledzenie problemu #15184</span><span class="sxs-lookup"><span data-stu-id="ffa9b-656">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="ffa9b-657">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-657">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-658">Następujące metody asynchroniczne wcześniej zwracały `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-658">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="ffa9b-659">`ValueGenerator.NextValueAsync()` (i pochodne klasy)</span><span class="sxs-lookup"><span data-stu-id="ffa9b-659">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="ffa9b-660">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-660">**New behavior**</span></span>

<span data-ttu-id="ffa9b-661">Powyższe metody teraz zwracają `ValueTask<T>` w tym samym `T` jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-661">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="ffa9b-662">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-662">**Why**</span></span>

<span data-ttu-id="ffa9b-663">Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-663">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="ffa9b-664">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-664">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-665">Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-665">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="ffa9b-666">Bardziej złożone użycie (np. przekazywanie zwróconych `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwrócone `ValueTask<T>` być konwertowane na `Task<T>` przez wywołanie `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-666">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="ffa9b-667">Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-667">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="ffa9b-668">Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping</span><span class="sxs-lookup"><span data-stu-id="ffa9b-668">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="ffa9b-669">Śledzenie problemu #9913</span><span class="sxs-lookup"><span data-stu-id="ffa9b-669">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="ffa9b-670">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-670">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-671">Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="ffa9b-671">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="ffa9b-672">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-672">**New behavior**</span></span>

<span data-ttu-id="ffa9b-673">Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="ffa9b-673">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="ffa9b-674">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-674">**Why**</span></span>

<span data-ttu-id="ffa9b-675">Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-675">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="ffa9b-676">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-676">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-677">Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-677">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="ffa9b-678">Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-678">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="ffa9b-679">ToTable dla typu pochodnego zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="ffa9b-679">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="ffa9b-680">Śledzenie problemu #11811</span><span class="sxs-lookup"><span data-stu-id="ffa9b-680">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="ffa9b-681">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-681">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-682">Przed EF Core 3,0, `ToTable()` wywołana dla typu pochodnego zostałaby zignorowana, ponieważ strategia mapowania dziedziczenia była TPH, gdzie jest to nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-682">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="ffa9b-683">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-683">**New behavior**</span></span>

<span data-ttu-id="ffa9b-684">Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, `ToTable()` wywołana dla typu pochodnego spowoduje teraz zgłoszenie wyjątku, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-684">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="ffa9b-685">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-685">**Why**</span></span>

<span data-ttu-id="ffa9b-686">Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-686">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="ffa9b-687">Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-687">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="ffa9b-688">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-688">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-689">Usuń wszystkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-689">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="ffa9b-690">ForSqlServerHasIndex zastąpione HasIndex</span><span class="sxs-lookup"><span data-stu-id="ffa9b-690">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="ffa9b-691">Śledzenie problemu #12366</span><span class="sxs-lookup"><span data-stu-id="ffa9b-691">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="ffa9b-692">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-692">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-693">Przed EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` zapewnić sposób konfigurowania kolumn używanych z `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-693">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="ffa9b-694">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-694">**New behavior**</span></span>

<span data-ttu-id="ffa9b-695">Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-695">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="ffa9b-696">Użyj `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-696">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="ffa9b-697">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-697">**Why**</span></span>

<span data-ttu-id="ffa9b-698">Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów z `Include` w jednym miejscu dla wszystkich dostawców baz danych.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-698">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="ffa9b-699">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-699">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-700">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-700">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="ffa9b-701">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="ffa9b-701">Metadata API changes</span></span>

[<span data-ttu-id="ffa9b-702">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="ffa9b-702">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ffa9b-703">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-703">**New behavior**</span></span>

<span data-ttu-id="ffa9b-704">Następujące właściwości zostały przekonwertowane na metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-704">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="ffa9b-705">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-705">**Why**</span></span>

<span data-ttu-id="ffa9b-706">Ta zmiana upraszcza implementację powyższych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-706">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="ffa9b-707">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-707">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-708">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-708">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="ffa9b-709">Zmiany w interfejsie API metadanych specyficzne dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="ffa9b-709">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="ffa9b-710">Śledzenie problemu #214</span><span class="sxs-lookup"><span data-stu-id="ffa9b-710">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ffa9b-711">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-711">**New behavior**</span></span>

<span data-ttu-id="ffa9b-712">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-712">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="ffa9b-713">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-713">**Why**</span></span>

<span data-ttu-id="ffa9b-714">Ta zmiana upraszcza implementację powyższych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-714">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="ffa9b-715">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-715">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-716">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-716">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="ffa9b-717">Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK</span><span class="sxs-lookup"><span data-stu-id="ffa9b-717">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="ffa9b-718">Śledzenie problemu #12151</span><span class="sxs-lookup"><span data-stu-id="ffa9b-718">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="ffa9b-719">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-719">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-720">Przed EF Core 3,0, EF Core wyśle `PRAGMA foreign_keys = 1` w przypadku otwarcia połączenia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-720">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ffa9b-721">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-721">**New behavior**</span></span>

<span data-ttu-id="ffa9b-722">Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` po otwarciu połączenia z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-722">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ffa9b-723">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-723">**Why**</span></span>

<span data-ttu-id="ffa9b-724">Ta zmiana została wprowadzona, ponieważ EF Core domyślnie używa `SQLitePCLRaw.bundle_e_sqlite3`, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-724">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="ffa9b-725">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-725">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-726">Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-726">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="ffa9b-727">W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-727">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="ffa9b-728">Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="ffa9b-728">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="ffa9b-729">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-729">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-730">Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-730">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="ffa9b-731">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-731">**New behavior**</span></span>

<span data-ttu-id="ffa9b-732">Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-732">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="ffa9b-733">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-733">**Why**</span></span>

<span data-ttu-id="ffa9b-734">Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-734">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="ffa9b-735">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-735">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-736">Aby użyć natywnej wersji oprogramowania SQLite w systemie iOS, skonfiguruj `Microsoft.Data.Sqlite` tak, aby korzystała z innego pakietu `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-736">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ffa9b-737">Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="ffa9b-737">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ffa9b-738">Śledzenie problemu #15078</span><span class="sxs-lookup"><span data-stu-id="ffa9b-738">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="ffa9b-739">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-739">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-740">Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-740">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="ffa9b-741">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-741">**New behavior**</span></span>

<span data-ttu-id="ffa9b-742">Wartości identyfikatorów GUID są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-742">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="ffa9b-743">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-743">**Why**</span></span>

<span data-ttu-id="ffa9b-744">Format binarny identyfikatorów GUID nie jest znormalizowany.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-744">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="ffa9b-745">Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-745">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ffa9b-746">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-746">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-747">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-747">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="ffa9b-748">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-748">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="ffa9b-749">Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-749">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ffa9b-750">Wartości char są teraz przechowywane jako tekst na komputerze SQLite</span><span class="sxs-lookup"><span data-stu-id="ffa9b-750">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ffa9b-751">Śledzenie problemu #15020</span><span class="sxs-lookup"><span data-stu-id="ffa9b-751">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="ffa9b-752">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-752">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-753">Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-753">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="ffa9b-754">Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-754">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="ffa9b-755">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-755">**New behavior**</span></span>

<span data-ttu-id="ffa9b-756">Wartości char są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-756">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="ffa9b-757">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-757">**Why**</span></span>

<span data-ttu-id="ffa9b-758">Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-758">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ffa9b-759">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-759">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-760">Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-760">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="ffa9b-761">W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-761">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="ffa9b-762">Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-762">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="ffa9b-763">Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury</span><span class="sxs-lookup"><span data-stu-id="ffa9b-763">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="ffa9b-764">Śledzenie problemu #12978</span><span class="sxs-lookup"><span data-stu-id="ffa9b-764">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="ffa9b-765">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-765">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-766">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-766">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="ffa9b-767">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-767">**New behavior**</span></span>

<span data-ttu-id="ffa9b-768">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-768">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="ffa9b-769">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-769">**Why**</span></span>

<span data-ttu-id="ffa9b-770">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-770">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="ffa9b-771">Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-771">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="ffa9b-772">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-772">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-773">Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-773">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="ffa9b-774">Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-774">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="ffa9b-775">Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-775">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="ffa9b-776">Należy również zaktualizować tabelę historii migracji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-776">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="ffa9b-777">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="ffa9b-777">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="ffa9b-778">Śledzenie problemu #16400</span><span class="sxs-lookup"><span data-stu-id="ffa9b-778">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="ffa9b-779">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-779">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-780">Przed EF Core 3,0, `UseRowNumberForPaging` może być użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-780">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="ffa9b-781">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-781">**New behavior**</span></span>

<span data-ttu-id="ffa9b-782">Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-782">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="ffa9b-783">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-783">**Why**</span></span>

<span data-ttu-id="ffa9b-784">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-784">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="ffa9b-785">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-785">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-786">Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-786">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="ffa9b-787">Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-787">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="ffa9b-788">Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-788">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="ffa9b-789">Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="ffa9b-789">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="ffa9b-790">Śledzenie problemu #16119</span><span class="sxs-lookup"><span data-stu-id="ffa9b-790">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="ffa9b-791">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-791">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-792">`IDbContextOptionsExtension` zawierały metody przekazywania metadanych o rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-792">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="ffa9b-793">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-793">**New behavior**</span></span>

<span data-ttu-id="ffa9b-794">Te metody zostały przeniesione na nową `DbContextOptionsExtensionInfo` abstrakcyjną klasę bazową, która jest zwracana z nowej właściwości `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-794">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="ffa9b-795">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-795">**Why**</span></span>

<span data-ttu-id="ffa9b-796">W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-796">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="ffa9b-797">Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-797">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="ffa9b-798">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-798">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-799">Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-799">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="ffa9b-800">Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-800">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="ffa9b-801">Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="ffa9b-801">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="ffa9b-802">Śledzenie problemu #10985</span><span class="sxs-lookup"><span data-stu-id="ffa9b-802">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="ffa9b-803">**Zmień**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-803">**Change**</span></span>

<span data-ttu-id="ffa9b-804">Zmieniono nazwę `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="ffa9b-805">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-805">**Why**</span></span>

<span data-ttu-id="ffa9b-806">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-806">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="ffa9b-807">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-807">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-808">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-808">Use the new name.</span></span> <span data-ttu-id="ffa9b-809">(Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).</span><span class="sxs-lookup"><span data-stu-id="ffa9b-809">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="ffa9b-810">Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="ffa9b-810">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="ffa9b-811">Śledzenie problemu #10730</span><span class="sxs-lookup"><span data-stu-id="ffa9b-811">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="ffa9b-812">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-812">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-813">Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa".</span><span class="sxs-lookup"><span data-stu-id="ffa9b-813">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="ffa9b-814">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-814">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="ffa9b-815">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-815">**New behavior**</span></span>

<span data-ttu-id="ffa9b-816">Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="ffa9b-816">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="ffa9b-817">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-817">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="ffa9b-818">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-818">**Why**</span></span>

<span data-ttu-id="ffa9b-819">Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-819">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="ffa9b-820">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-820">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-821">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-821">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="ffa9b-822">IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie</span><span class="sxs-lookup"><span data-stu-id="ffa9b-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="ffa9b-823">Śledzenie problemu #15997</span><span class="sxs-lookup"><span data-stu-id="ffa9b-823">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="ffa9b-824">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-824">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-825">Przed EF Core 3,0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-825">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="ffa9b-826">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-826">**New behavior**</span></span>

<span data-ttu-id="ffa9b-827">Począwszy od EF Core 3,0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-827">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="ffa9b-828">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-828">**Why**</span></span>

<span data-ttu-id="ffa9b-829">Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-829">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="ffa9b-830">Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-830">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="ffa9b-831">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-831">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-832">Zmień dostępność wszelkich zastąpień.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-832">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="ffa9b-833">Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="ffa9b-833">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="ffa9b-834">Śledzenie problemu #11506</span><span class="sxs-lookup"><span data-stu-id="ffa9b-834">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="ffa9b-835">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-835">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-836">Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-836">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="ffa9b-837">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-837">**New behavior**</span></span>

<span data-ttu-id="ffa9b-838">Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-838">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="ffa9b-839">Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-839">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="ffa9b-840">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-840">**Why**</span></span>

<span data-ttu-id="ffa9b-841">Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-841">This package is only intended to be used at design time.</span></span> <span data-ttu-id="ffa9b-842">Wdrożone aplikacje nie powinny się do niej odwoływać.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-842">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="ffa9b-843">Pakiet a DevelopmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-843">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="ffa9b-844">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-844">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-845">Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-845">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="ffa9b-846">Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-846">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="ffa9b-847">SQLitePCL. Raw Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-847">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="ffa9b-848">Śledzenie problemu #14824</span><span class="sxs-lookup"><span data-stu-id="ffa9b-848">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="ffa9b-849">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-849">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-850">Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-850">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="ffa9b-851">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-851">**New behavior**</span></span>

<span data-ttu-id="ffa9b-852">Zaktualizowaliśmy pakiet, który jest zależny od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-852">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="ffa9b-853">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-853">**Why**</span></span>

<span data-ttu-id="ffa9b-854">Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-854">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="ffa9b-855">Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-855">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="ffa9b-856">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-856">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-857">SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-857">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="ffa9b-858">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="ffa9b-858">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="ffa9b-859">NetTopologySuite Zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffa9b-859">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="ffa9b-860">Śledzenie problemu #14825</span><span class="sxs-lookup"><span data-stu-id="ffa9b-860">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="ffa9b-861">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-861">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-862">Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-862">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="ffa9b-863">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-863">**New behavior**</span></span>

<span data-ttu-id="ffa9b-864">Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-864">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="ffa9b-865">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-865">**Why**</span></span>

<span data-ttu-id="ffa9b-866">Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-866">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="ffa9b-867">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-867">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-868">NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-868">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="ffa9b-869">Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="ffa9b-869">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="ffa9b-870">Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="ffa9b-870">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="ffa9b-871">Śledzenie problemu #15636</span><span class="sxs-lookup"><span data-stu-id="ffa9b-871">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="ffa9b-872">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-872">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-873">Microsoft. EntityFrameworkCore. SqlServer poprzednio zależała od typu System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-873">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="ffa9b-874">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-874">**New behavior**</span></span>

<span data-ttu-id="ffa9b-875">Zaktualizowaliśmy pakiet, który jest zależny od firmy Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-875">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="ffa9b-876">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-876">**Why**</span></span>

<span data-ttu-id="ffa9b-877">Microsoft. Data. SqlClient to sterownik dostępu do danych sztandarowe, który umożliwia SQL Server przechodzenie do przodu, a system. Data. SqlClient nie jest już fokusem rozwoju.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-877">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="ffa9b-878">Niektóre ważne funkcje, takie jak Always Encrypted, są dostępne tylko w firmie Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-878">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="ffa9b-879">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-879">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-880">Jeśli kod przyjmuje bezpośrednią zależność od elementu System. Data. SqlClient, należy zmienić go na odwołanie Microsoft. Data. SqlClient zamiast tego. ponieważ dwa pakiety obsługują bardzo wysoki poziom zgodności interfejsów API, powinno to być tylko proste zmiany pakietów i przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-880">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="ffa9b-881">Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-881">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="ffa9b-882">Śledzenie problemu #13573</span><span class="sxs-lookup"><span data-stu-id="ffa9b-882">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="ffa9b-883">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-883">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-884">Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-884">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="ffa9b-885">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-885">For example:</span></span>

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

<span data-ttu-id="ffa9b-886">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-886">**New behavior**</span></span>

<span data-ttu-id="ffa9b-887">Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-887">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="ffa9b-888">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-888">**Why**</span></span>

<span data-ttu-id="ffa9b-889">Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-889">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="ffa9b-890">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-890">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-891">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-891">Use full configuration of the relationship.</span></span> <span data-ttu-id="ffa9b-892">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ffa9b-892">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="ffa9b-893">Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu</span><span class="sxs-lookup"><span data-stu-id="ffa9b-893">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="ffa9b-894">Śledzenie problemu #12757</span><span class="sxs-lookup"><span data-stu-id="ffa9b-894">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="ffa9b-895">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-895">**Old behavior**</span></span>

<span data-ttu-id="ffa9b-896">Funkcja dbzostała skonfigurowana ze schematem jako pusty ciąg był traktowany jak wbudowana funkcja bez schematu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-896">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="ffa9b-897">Na przykład poniższy kod mapuje `DatePart` funkcję CLR, aby `DATEPART` funkcję wbudowaną na serwerze SqlServer.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-897">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="ffa9b-898">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-898">**New behavior**</span></span>

<span data-ttu-id="ffa9b-899">Wszystkie mapowania funkcji dbfunction są uznawane za mapowane do funkcji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-899">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="ffa9b-900">W związku z tym wartość pustego ciągu spowodowałaby umieszczenie funkcji wewnątrz domyślnego schematu modelu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-900">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="ffa9b-901">Może to być schemat skonfigurowany jawnie za pośrednictwem interfejsu API Fluent `modelBuilder.HasDefaultSchema()` lub `dbo` w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-901">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="ffa9b-902">**Zalet**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-902">**Why**</span></span>

<span data-ttu-id="ffa9b-903">Poprzednio pusty schemat był sposobem traktowania tej funkcji, ale ta logika jest stosowana tylko dla SqlServer, w której wbudowane funkcje nie należą do żadnego schematu.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-903">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="ffa9b-904">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="ffa9b-904">**Mitigations**</span></span>

<span data-ttu-id="ffa9b-905">Skonfiguruj ręcznie tłumaczenie funkcji dbfunction, aby zamapować ją na wbudowaną funkcję.</span><span class="sxs-lookup"><span data-stu-id="ffa9b-905">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
