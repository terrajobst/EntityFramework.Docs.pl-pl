---
title: Przełomowe zmiany w EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417462"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="a826e-102">Przełomowe zmiany zawarte w EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="a826e-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="a826e-103">Następujące zmiany interfejsu API i zachowania mają potencjał, aby przerwać istniejące aplikacje podczas uaktualniania ich do 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="a826e-104">Zmiany, które mają mieć wpływ tylko na dostawców baz danych, są dokumentowane w obszarze [zmiany dostawcy.](xref:core/providers/provider-log)</span><span class="sxs-lookup"><span data-stu-id="a826e-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="a826e-105">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a826e-105">Summary</span></span>

| <span data-ttu-id="a826e-106">**Przełomowa zmiana**</span><span class="sxs-lookup"><span data-stu-id="a826e-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="a826e-107">**Wpływ**</span><span class="sxs-lookup"><span data-stu-id="a826e-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="a826e-108">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="a826e-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="a826e-109">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-109">High</span></span>       |
| [<span data-ttu-id="a826e-110">Cele EF Core 3.0 .NET Standard 2.1 zamiast .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="a826e-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="a826e-111">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-111">High</span></span>      |
| [<span data-ttu-id="a826e-112">Narzędzie wiersza polecenia EF Core, dotnet ef, nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a826e-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="a826e-113">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-113">High</span></span>      |
| [<span data-ttu-id="a826e-114">DetectChanges honoruje wartości kluczy wygenerowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="a826e-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="a826e-115">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-115">High</span></span>      |
| [<span data-ttu-id="a826e-116">Zmieniono nazwę fromSql, ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="a826e-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="a826e-117">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-117">High</span></span>      |
| [<span data-ttu-id="a826e-118">Typy zapytań są konsolidowane za pomocą typów encji</span><span class="sxs-lookup"><span data-stu-id="a826e-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="a826e-119">Wysoka</span><span class="sxs-lookup"><span data-stu-id="a826e-119">High</span></span>      |
| [<span data-ttu-id="a826e-120">Entity Framework Core nie jest już częścią ASP.NET core współdzielonych</span><span class="sxs-lookup"><span data-stu-id="a826e-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="a826e-121">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-121">Medium</span></span>      |
| [<span data-ttu-id="a826e-122">Kaskadowe usunięcia są teraz usuwane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="a826e-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="a826e-123">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-123">Medium</span></span>      |
| [<span data-ttu-id="a826e-124">Wczesne ładowanie powiązanych jednostek odbywa się teraz w jednej kwerendzie</span><span class="sxs-lookup"><span data-stu-id="a826e-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="a826e-125">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-125">Medium</span></span>      |
| [<span data-ttu-id="a826e-126">DeleteBehavior.Restrict ma czystsze semantyki</span><span class="sxs-lookup"><span data-stu-id="a826e-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="a826e-127">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-127">Medium</span></span>      |
| [<span data-ttu-id="a826e-128">Interfejs API konfiguracji dla relacji typu należącego uległ zmianie</span><span class="sxs-lookup"><span data-stu-id="a826e-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="a826e-129">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-129">Medium</span></span>      |
| [<span data-ttu-id="a826e-130">Każda właściwość używa niezależnego generowania klucza całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="a826e-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="a826e-131">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-131">Medium</span></span>      |
| [<span data-ttu-id="a826e-132">Kwerendy bez śledzenia nie wykonują już rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="a826e-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="a826e-133">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-133">Medium</span></span>      |
| [<span data-ttu-id="a826e-134">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="a826e-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="a826e-135">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-135">Medium</span></span>      |
| [<span data-ttu-id="a826e-136">Zmiany interfejsu API metadanych specyficznych dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="a826e-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="a826e-137">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-137">Medium</span></span>      |
| [<span data-ttu-id="a826e-138">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="a826e-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="a826e-139">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-139">Medium</span></span>      |
| [<span data-ttu-id="a826e-140">Metoda FromSql używana z procedurą składowaną nie może być</span><span class="sxs-lookup"><span data-stu-id="a826e-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="a826e-141">Medium</span><span class="sxs-lookup"><span data-stu-id="a826e-141">Medium</span></span>      |
| [<span data-ttu-id="a826e-142">Metody FromSql można określić tylko w katalogach głównych kwerend</span><span class="sxs-lookup"><span data-stu-id="a826e-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="a826e-143">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-143">Low</span></span>      |
| [<span data-ttu-id="a826e-144">~~Wykonywanie kwerendy jest rejestrowane na poziomie debugowania~~ Przywrócone</span><span class="sxs-lookup"><span data-stu-id="a826e-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="a826e-145">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-145">Low</span></span>      |
| [<span data-ttu-id="a826e-146">Tymczasowe wartości klucza nie są już ustawiane w wystąpieniach encji</span><span class="sxs-lookup"><span data-stu-id="a826e-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="a826e-147">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-147">Low</span></span>      |
| [<span data-ttu-id="a826e-148">Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="a826e-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="a826e-149">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-149">Low</span></span>      |
| [<span data-ttu-id="a826e-150">Wszystkie jednostki współużytkujące tabelę z kolumną tokenu współbieżności muszą mapować ją na właściwość</span><span class="sxs-lookup"><span data-stu-id="a826e-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="a826e-151">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-151">Low</span></span>      |
| [<span data-ttu-id="a826e-152">Nie można wyszukiwać jednostek będących własnością bez użycia zapytania śledzenia przez właściciela</span><span class="sxs-lookup"><span data-stu-id="a826e-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="a826e-153">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-153">Low</span></span>      |
| [<span data-ttu-id="a826e-154">Właściwości dziedziczone z typów niezamapowanych są teraz mapowane na jedną kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="a826e-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="a826e-155">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-155">Low</span></span>      |
| [<span data-ttu-id="a826e-156">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość główna</span><span class="sxs-lookup"><span data-stu-id="a826e-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="a826e-157">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-157">Low</span></span>      |
| [<span data-ttu-id="a826e-158">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest już używane przed zakończeniem transakcji TransactionScope</span><span class="sxs-lookup"><span data-stu-id="a826e-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="a826e-159">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-159">Low</span></span>      |
| [<span data-ttu-id="a826e-160">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="a826e-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="a826e-161">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-161">Low</span></span>      |
| [<span data-ttu-id="a826e-162">Throw, jeśli znaleziono wiele zgodnych pól podkładowych</span><span class="sxs-lookup"><span data-stu-id="a826e-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="a826e-163">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-163">Low</span></span>      |
| [<span data-ttu-id="a826e-164">Nazwy właściwości tylko do pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="a826e-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="a826e-165">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-165">Low</span></span>      |
| [<span data-ttu-id="a826e-166">AddDbContext/AddDbContextPool nie będzie już wywoływać addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="a826e-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="a826e-167">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-167">Low</span></span>      |
| [<span data-ttu-id="a826e-168">AddEntityFramework\* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="a826e-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="a826e-169">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-169">Low</span></span>      |
| [<span data-ttu-id="a826e-170">DbContext.Entry wykonuje teraz lokalne zmiany wykrywania</span><span class="sxs-lookup"><span data-stu-id="a826e-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="a826e-171">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-171">Low</span></span>      |
| [<span data-ttu-id="a826e-172">Klucze tablicy ciągów i bajtów nie są domyślnie generowane przez klienta</span><span class="sxs-lookup"><span data-stu-id="a826e-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="a826e-173">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-173">Low</span></span>      |
| [<span data-ttu-id="a826e-174">ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="a826e-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="a826e-175">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-175">Low</span></span>      |
| [<span data-ttu-id="a826e-176">Serwery proxy z opóźnieniem nie zakładają już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="a826e-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="a826e-177">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-177">Low</span></span>      |
| [<span data-ttu-id="a826e-178">Nadmierne tworzenie wewnętrznych usługodawców jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="a826e-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="a826e-179">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-179">Low</span></span>      |
| [<span data-ttu-id="a826e-180">Nowe zachowanie hasone/hasmany wywoływane z jednym ciągiem</span><span class="sxs-lookup"><span data-stu-id="a826e-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="a826e-181">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-181">Low</span></span>      |
| [<span data-ttu-id="a826e-182">Typ zwracany dla kilku metod asynchronicznego został zmieniony z Task na ValueTask</span><span class="sxs-lookup"><span data-stu-id="a826e-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="a826e-183">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-183">Low</span></span>      |
| [<span data-ttu-id="a826e-184">Adnotacja relacyjna:TypeMapping jest teraz tylko TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a826e-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="a826e-185">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-185">Low</span></span>      |
| [<span data-ttu-id="a826e-186">ToTable na typ pochodny zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="a826e-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="a826e-187">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-187">Low</span></span>      |
| [<span data-ttu-id="a826e-188">EF Core nie wysyła już pragmy do egzekwowania SQLite FK</span><span class="sxs-lookup"><span data-stu-id="a826e-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="a826e-189">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-189">Low</span></span>      |
| [<span data-ttu-id="a826e-190">Microsoft.EntityFrameworkCore.Sqlite teraz zależy od SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="a826e-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="a826e-191">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-191">Low</span></span>      |
| [<span data-ttu-id="a826e-192">Wartości Guid są teraz przechowywane jako tekst na SQLite</span><span class="sxs-lookup"><span data-stu-id="a826e-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="a826e-193">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-193">Low</span></span>      |
| [<span data-ttu-id="a826e-194">Wartości char są teraz przechowywane jako tekst na SQLite</span><span class="sxs-lookup"><span data-stu-id="a826e-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="a826e-195">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-195">Low</span></span>      |
| [<span data-ttu-id="a826e-196">Identyfikatory migracji są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="a826e-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="a826e-197">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-197">Low</span></span>      |
| [<span data-ttu-id="a826e-198">Informacje/metadane rozszerzenia zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="a826e-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="a826e-199">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-199">Low</span></span>      |
| [<span data-ttu-id="a826e-200">Nazwa logqueryPossibleExceptionWithAggregateOperator została zmieniona</span><span class="sxs-lookup"><span data-stu-id="a826e-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="a826e-201">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-201">Low</span></span>      |
| [<span data-ttu-id="a826e-202">Wyjaśnienie interfejsu API dla nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="a826e-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="a826e-203">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-203">Low</span></span>      |
| [<span data-ttu-id="a826e-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync zostały upublicznione</span><span class="sxs-lookup"><span data-stu-id="a826e-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="a826e-205">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-205">Low</span></span>      |
| [<span data-ttu-id="a826e-206">Microsoft.EntityFrameworkCore.Design jest teraz pakietem rozwojuzależności</span><span class="sxs-lookup"><span data-stu-id="a826e-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="a826e-207">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-207">Low</span></span>      |
| [<span data-ttu-id="a826e-208">SQLitePCL.raw zaktualizowany do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a826e-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="a826e-209">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-209">Low</span></span>      |
| [<span data-ttu-id="a826e-210">NetTopologySuite zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a826e-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="a826e-211">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-211">Low</span></span>      |
| [<span data-ttu-id="a826e-212">Microsoft.Data.SqlClient jest używany zamiast System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="a826e-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="a826e-213">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-213">Low</span></span>      |
| [<span data-ttu-id="a826e-214">Należy skonfigurować wiele niejednoznacznych relacji samonawodujących się</span><span class="sxs-lookup"><span data-stu-id="a826e-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="a826e-215">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-215">Low</span></span>      |
| [<span data-ttu-id="a826e-216">DbFunction.Schema jest null lub pusty ciąg konfiguruje go do domyślnego schematu modelu</span><span class="sxs-lookup"><span data-stu-id="a826e-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="a826e-217">Małe</span><span class="sxs-lookup"><span data-stu-id="a826e-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="a826e-218">Zapytania LINQ nie są już oceniane na kliencie</span><span class="sxs-lookup"><span data-stu-id="a826e-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="a826e-219">[#14935 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
śledzenia[zobacz również #12795 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="a826e-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="a826e-220">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-220">**Old behavior**</span></span>

<span data-ttu-id="a826e-221">Przed 3.0, gdy EF Core nie można przekonwertować wyrażenie, które było częścią kwerendy do SQL lub parametru, automatycznie oceniane wyrażenie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a826e-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="a826e-222">Domyślnie ocena klienta potencjalnie kosztownych wyrażeń wyzwoliła tylko ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a826e-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="a826e-223">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-223">**New behavior**</span></span>

<span data-ttu-id="a826e-224">Począwszy od 3.0, EF Core umożliwia tylko wyrażenia w `Select()` projekcji najwyższego poziomu (ostatnie wywołanie w kwerendzie) do oceny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a826e-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="a826e-225">Gdy wyrażenia w innej części kwerendy nie można przekonwertować na SQL lub parametr, wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a826e-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="a826e-226">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-226">**Why**</span></span>

<span data-ttu-id="a826e-227">Automatyczna ocena zapytań przez klienta umożliwia wykonanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych ich części.</span><span class="sxs-lookup"><span data-stu-id="a826e-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="a826e-228">To zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w produkcji.</span><span class="sxs-lookup"><span data-stu-id="a826e-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="a826e-229">Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli zostaną przeniesione z serwera bazy danych, a filtr ma zostać zastosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a826e-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="a826e-230">Ta sytuacja może łatwo przejść niezauważone, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale mocno uderzyć, gdy aplikacja przenosi się do produkcji, gdzie tabela może zawierać miliony wierszy.</span><span class="sxs-lookup"><span data-stu-id="a826e-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="a826e-231">Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas opracowywania.</span><span class="sxs-lookup"><span data-stu-id="a826e-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="a826e-232">Poza tym automatyczna ocena klienta może prowadzić do problemów, w których poprawa tłumaczenia zapytań dla określonych wyrażeń spowodował niezamierzone zmiany podziału między wersjami.</span><span class="sxs-lookup"><span data-stu-id="a826e-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="a826e-233">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-233">**Mitigations**</span></span>

<span data-ttu-id="a826e-234">Jeśli kwerendy nie można w pełni przetłumaczyć, a następnie przepisać kwerendę `AsEnumerable()`w `ToList()`formularzu, który może być przetłumaczony lub użyć , lub podobne do jawnie przenieść dane z powrotem do klienta, gdzie następnie mogą być dalej przetwarzane przy użyciu LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="a826e-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="a826e-235">Cele EF Core 3.0 .NET Standard 2.1 zamiast .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="a826e-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="a826e-236">#15498 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="a826e-237">EF Core 3.1 ponownie celuje w .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="a826e-238">Spowoduje to przywrócenie obsługi programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a826e-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="a826e-239">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-239">**Old behavior**</span></span>

<span data-ttu-id="a826e-240">Przed 3.0 EF Core ukierunkowane .NET Standard 2.0 i będzie działać na wszystkich platformach, które obsługują tego standardu, w tym .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a826e-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="a826e-241">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-241">**New behavior**</span></span>

<span data-ttu-id="a826e-242">Począwszy od 3.0, EF Core cele .NET Standard 2.1 i będzie działać na wszystkich platformach, które obsługują ten standard.</span><span class="sxs-lookup"><span data-stu-id="a826e-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="a826e-243">Nie obejmuje to programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a826e-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="a826e-244">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-244">**Why**</span></span>

<span data-ttu-id="a826e-245">Jest to część strategicznej decyzji w ramach technologii platformy .NET, aby skupić energię na programie .NET Core i innych nowoczesnych platformach platformy .NET, takich jak Xamarin.</span><span class="sxs-lookup"><span data-stu-id="a826e-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="a826e-246">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-246">**Mitigations**</span></span>

<span data-ttu-id="a826e-247">Użyj EF Core 3.1.</span><span class="sxs-lookup"><span data-stu-id="a826e-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="a826e-248">Entity Framework Core nie jest już częścią ASP.NET core współdzielonych</span><span class="sxs-lookup"><span data-stu-id="a826e-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="a826e-249">Śledzenie ogłoszeń o problemach#325</span><span class="sxs-lookup"><span data-stu-id="a826e-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="a826e-250">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-250">**Old behavior**</span></span>

<span data-ttu-id="a826e-251">Przed ASP.NET Core 3.0, po dodaniu `Microsoft.AspNetCore.App` odwołania `Microsoft.AspNetCore.All`do pakietu lub , to będzie zawierać EF Core i niektórych dostawców danych EF Core, takich jak dostawca programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a826e-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="a826e-252">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-252">**New behavior**</span></span>

<span data-ttu-id="a826e-253">Począwszy od 3.0, ASP.NET Core udostępnionej struktury nie obejmuje EF Core lub żadnych dostawców danych EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="a826e-254">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-254">**Why**</span></span>

<span data-ttu-id="a826e-255">Przed tą zmianą uzyskanie EF Core wymagało różnych kroków w zależności od tego, czy aplikacja jest ukierunkowana na ASP.NET Core i SQL Server, czy nie.</span><span class="sxs-lookup"><span data-stu-id="a826e-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="a826e-256">Ponadto uaktualnienie ASP.NET Core wymusił uaktualnienie EF Core i dostawcy programu SQL Server, co nie zawsze jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="a826e-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="a826e-257">Dzięki tej zmianie środowisko uzyskiwania EF Core jest taka sama we wszystkich dostawcach, obsługiwanych implementacjach platformy .NET i typach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="a826e-258">Deweloperzy mogą również teraz kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych są uaktualniane.</span><span class="sxs-lookup"><span data-stu-id="a826e-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="a826e-259">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-259">**Mitigations**</span></span>

<span data-ttu-id="a826e-260">Aby użyć EF Core w aplikacji ASP.NET Core 3.0 lub innej obsługiwanej aplikacji, jawnie dodaj odwołanie do pakietu ef core dostawcy bazy danych, który będzie używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="a826e-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="a826e-261">Narzędzie wiersza polecenia EF Core, dotnet ef, nie jest już częścią zestawu .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a826e-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="a826e-262">#14016 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="a826e-263">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-263">**Old behavior**</span></span>

<span data-ttu-id="a826e-264">Przed 3.0 `dotnet ef` narzędzie zostało uwzględnione w zestawie .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności dodatkowych kroków.</span><span class="sxs-lookup"><span data-stu-id="a826e-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="a826e-265">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-265">**New behavior**</span></span>

<span data-ttu-id="a826e-266">Począwszy od 3.0, .NET SDK `dotnet ef` nie zawiera narzędzia, więc przed jego użyciem należy jawnie zainstalować go jako narzędzie lokalne lub globalne.</span><span class="sxs-lookup"><span data-stu-id="a826e-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="a826e-267">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-267">**Why**</span></span>

<span data-ttu-id="a826e-268">Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako zwykłe narzędzie interfejsu wiersza polecenia .NET na NuGet, zgodnie z faktem, że EF Core 3.0 jest również zawsze dystrybuowane jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="a826e-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="a826e-269">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-269">**Mitigations**</span></span>

<span data-ttu-id="a826e-270">Aby móc zarządzać migracjami lub szkieletem `DbContext` `dotnet-ef` , zainstalować jako narzędzie globalne:</span><span class="sxs-lookup"><span data-stu-id="a826e-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="a826e-271">Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzi przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="a826e-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="a826e-272">Zmieniono nazwę fromSql, ExecuteSql i ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="a826e-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="a826e-273">#10996 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="a826e-274">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-274">**Old behavior**</span></span>

<span data-ttu-id="a826e-275">Przed EF Core 3.0 te nazwy metod były przeciążone do pracy z normalnym ciągiem lub ciągiem, który powinien być interpolowany do sql i parametrów.</span><span class="sxs-lookup"><span data-stu-id="a826e-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="a826e-276">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-276">**New behavior**</span></span>

<span data-ttu-id="a826e-277">Począwszy od EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`i `ExecuteSqlRawAsync` utworzyć sparametryzowane zapytanie, gdzie parametry są przekazywane oddzielnie od ciągu kwerendy.</span><span class="sxs-lookup"><span data-stu-id="a826e-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="a826e-278">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="a826e-279">Użyj `FromSqlInterpolated` `ExecuteSqlInterpolated`, `ExecuteSqlInterpolatedAsync` i utworzyć sparametryzowaną kwerendę, w której parametry są przekazywane jako część interpolowanego ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a826e-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="a826e-280">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="a826e-281">Należy zauważyć, że oba powyższe zapytania będą tworzyć ten sam sparametryzowany SQL z tymi samymi parametrami SQL.</span><span class="sxs-lookup"><span data-stu-id="a826e-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="a826e-282">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-282">**Why**</span></span>

<span data-ttu-id="a826e-283">Przeciążenia metody, takie jak ten sprawiają, że bardzo łatwo przypadkowo wywołać metodę ciągu nieprzetworzonego, gdy intencją było wywołanie interpolowanej metody ciągu i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="a826e-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="a826e-284">Może to spowodować kwerendy nie są parametryzowane, kiedy powinny być.</span><span class="sxs-lookup"><span data-stu-id="a826e-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="a826e-285">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-285">**Mitigations**</span></span>

<span data-ttu-id="a826e-286">Przełącz się, aby użyć nowych nazw metod.</span><span class="sxs-lookup"><span data-stu-id="a826e-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="a826e-287">Metoda FromSql używana z procedurą składowaną nie może być</span><span class="sxs-lookup"><span data-stu-id="a826e-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="a826e-288">#15392 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="a826e-289">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-289">**Old behavior**</span></span>

<span data-ttu-id="a826e-290">Przed EF Core 3.0, FromSql metoda próbowała wykryć, czy przekazany SQL może być skomponowany na.</span><span class="sxs-lookup"><span data-stu-id="a826e-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="a826e-291">To nie oceny klienta, gdy SQL był niekomponowany jak procedura składowana.</span><span class="sxs-lookup"><span data-stu-id="a826e-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="a826e-292">Poniższa kwerenda działała przez uruchomienie procedury składowanej na serwerze i wykonanie firstordefault po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a826e-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="a826e-293">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-293">**New behavior**</span></span>

<span data-ttu-id="a826e-294">Począwszy od EF Core 3.0, EF Core nie spróbuje przeanalizować SQL.</span><span class="sxs-lookup"><span data-stu-id="a826e-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="a826e-295">Więc jeśli są komponowania po FromSqlRaw/FromSqlInterpolated, a następnie EF Core będzie komponować SQL, powodując sub kwerendy.</span><span class="sxs-lookup"><span data-stu-id="a826e-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="a826e-296">Więc jeśli używasz procedury składowanej ze składem, otrzymasz wyjątek dla nieprawidłowej składni SQL.</span><span class="sxs-lookup"><span data-stu-id="a826e-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="a826e-297">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-297">**Why**</span></span>

<span data-ttu-id="a826e-298">EF Core 3.0 nie obsługuje automatycznej oceny klienta, ponieważ był podatny na błędy, jak wyjaśniono [tutaj.](#linq-queries-are-no-longer-evaluated-on-the-client)</span><span class="sxs-lookup"><span data-stu-id="a826e-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="a826e-299">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-299">**Mitigation**</span></span>

<span data-ttu-id="a826e-300">Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można składać na, więc można dodać __AsEnumerable/AsAsyncEnumerable__ zaraz po FromSql wywołanie metody, aby uniknąć jakiejkolwiek kompozycji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="a826e-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="a826e-301">Metody FromSql można określić tylko w katalogach głównych kwerend</span><span class="sxs-lookup"><span data-stu-id="a826e-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="a826e-302">#15704 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="a826e-303">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-303">**Old behavior**</span></span>

<span data-ttu-id="a826e-304">Przed EF Core 3.0, `FromSql` metoda może być określona w dowolnym miejscu w kwerendzie.</span><span class="sxs-lookup"><span data-stu-id="a826e-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="a826e-305">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-305">**New behavior**</span></span>

<span data-ttu-id="a826e-306">Począwszy `FromSqlRaw` od EF Core 3.0, nowe i `FromSqlInterpolated` metody (które zastępują) `FromSql`mogą być określone `DbSet<>`tylko na korzeniach zapytań, czyli bezpośrednio na .</span><span class="sxs-lookup"><span data-stu-id="a826e-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="a826e-307">Próba określenia ich w dowolnym miejscu innego spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="a826e-308">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-308">**Why**</span></span>

<span data-ttu-id="a826e-309">Określanie `FromSql` w dowolnym `DbSet` miejscu innym niż na nie ma żadnego znaczenia dodanego lub wartości dodanej i może spowodować niejednoznaczność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="a826e-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="a826e-310">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-310">**Mitigations**</span></span>

<span data-ttu-id="a826e-311">`FromSql`wywołania powinny być przenoszone bezpośrednio `DbSet` na to, do czego się odnoszą.</span><span class="sxs-lookup"><span data-stu-id="a826e-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="a826e-312">Kwerendy bez śledzenia nie wykonują już rozpoznawania tożsamości</span><span class="sxs-lookup"><span data-stu-id="a826e-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="a826e-313">#13518 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="a826e-314">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-314">**Old behavior**</span></span>

<span data-ttu-id="a826e-315">Przed EF Core 3.0 tego samego wystąpienia jednostki będzie używany dla każdego wystąpienia jednostki z danego typu i identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="a826e-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="a826e-316">Odpowiada to zachowaniu zapytań śledzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="a826e-317">Na przykład ta kwerenda:</span><span class="sxs-lookup"><span data-stu-id="a826e-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="a826e-318">zwraca to `Category` samo wystąpienie dla każdego, `Product` który jest skojarzony z daną kategorią.</span><span class="sxs-lookup"><span data-stu-id="a826e-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="a826e-319">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-319">**New behavior**</span></span>

<span data-ttu-id="a826e-320">Począwszy od EF Core 3.0, różne wystąpienia jednostek zostaną utworzone, gdy jednostka o danym typie i identyfikatorze zostanie napotkana w różnych miejscach na zwróconym wykresie.</span><span class="sxs-lookup"><span data-stu-id="a826e-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="a826e-321">Na przykład powyższa kwerenda zwróci `Category` teraz `Product` nowe wystąpienie dla każdego, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.</span><span class="sxs-lookup"><span data-stu-id="a826e-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="a826e-322">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-322">**Why**</span></span>

<span data-ttu-id="a826e-323">Rozpoznawanie tożsamości (czyli określanie, że jednostka ma ten sam typ i identyfikator co wcześniej napotkana jednostka) dodaje dodatkową wydajność i obciążenie pamięci.</span><span class="sxs-lookup"><span data-stu-id="a826e-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="a826e-324">To zwykle jest sprzeczne z dlaczego nie śledzenia kwerendy są używane w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="a826e-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="a826e-325">Ponadto podczas rozpoznawania tożsamości czasami może być przydatne, nie jest potrzebne, jeśli jednostki mają być serializowane i wysyłane do klienta, co jest wspólne dla kwerend bez śledzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="a826e-326">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-326">**Mitigations**</span></span>

<span data-ttu-id="a826e-327">Użyj kwerendy śledzącej, jeśli wymagane jest rozpoznawanie tożsamości.</span><span class="sxs-lookup"><span data-stu-id="a826e-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="a826e-328">~~Wykonywanie kwerendy jest rejestrowane na poziomie debugowania~~ Przywrócone</span><span class="sxs-lookup"><span data-stu-id="a826e-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="a826e-329">#14523 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="a826e-330">Firma We cofnięte tej zmiany, ponieważ nowa konfiguracja w EF Core 3.0 umożliwia poziom dziennika dla dowolnego zdarzenia, które mają być określone przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="a826e-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="a826e-331">Na przykład, aby przełączyć `Debug`rejestrowanie SQL do , `OnConfiguring` `AddDbContext`jawnie skonfigurować poziom w lub:</span><span class="sxs-lookup"><span data-stu-id="a826e-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="a826e-332">Tymczasowe wartości klucza nie są już ustawiane w wystąpieniach encji</span><span class="sxs-lookup"><span data-stu-id="a826e-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="a826e-333">#12378 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="a826e-334">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-334">**Old behavior**</span></span>

<span data-ttu-id="a826e-335">Przed EF Core 3.0, wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później mają rzeczywistą wartość generowaną przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a826e-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="a826e-336">Zazwyczaj te wartości tymczasowe były duże liczby ujemne.</span><span class="sxs-lookup"><span data-stu-id="a826e-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="a826e-337">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-337">**New behavior**</span></span>

<span data-ttu-id="a826e-338">Począwszy od 3.0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia właściwość klucza się bez zmian.</span><span class="sxs-lookup"><span data-stu-id="a826e-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="a826e-339">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-339">**Why**</span></span>

<span data-ttu-id="a826e-340">Ta zmiana została wnieszona, aby zapobiec tymczasowe wartości klucza błędnie staje `DbContext` się trwałe, gdy `DbContext` jednostka, która została wcześniej śledzona przez niektóre wystąpienie jest przenoszony do innego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a826e-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="a826e-341">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-341">**Mitigations**</span></span>

<span data-ttu-id="a826e-342">Aplikacje, które przypisują wartości klucza podstawowego do kluczy obcych w celu utworzenia skojarzeń między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w `Added` stanie.</span><span class="sxs-lookup"><span data-stu-id="a826e-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="a826e-343">Można tego uniknąć poprzez:</span><span class="sxs-lookup"><span data-stu-id="a826e-343">This can be avoided by:</span></span>
* <span data-ttu-id="a826e-344">Nie używanie kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="a826e-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="a826e-345">Ustawianie właściwości nawigacji w celu utworzenia relacji zamiast ustawiania wartości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a826e-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="a826e-346">Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="a826e-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="a826e-347">Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową, nawet jeśli `blog.Id` sama nie została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="a826e-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="a826e-348">DetectChanges honoruje wartości kluczy wygenerowane przez magazyn</span><span class="sxs-lookup"><span data-stu-id="a826e-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="a826e-349">#14616 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="a826e-350">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-350">**Old behavior**</span></span>

<span data-ttu-id="a826e-351">Przed EF Core 3.0, nieśledzona jednostka znaleziona przez `DetectChanges` będzie śledzona w `Added` stanie i wstawiana jako nowy wiersz, gdy `SaveChanges` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a826e-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="a826e-352">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-352">**New behavior**</span></span>

<span data-ttu-id="a826e-353">Począwszy od EF Core 3.0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono `Modified` pewną wartość klucza, jednostka będzie śledzona w stanie.</span><span class="sxs-lookup"><span data-stu-id="a826e-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="a826e-354">Oznacza to, że zakłada się, że wiersz dla `SaveChanges` jednostki istnieje i zostanie zaktualizowany, gdy zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="a826e-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="a826e-355">Jeśli wartość klucza nie jest ustawiona lub typ jednostki nie używa wygenerowanych kluczy, `Added` nowa encja będzie nadal śledzona tak, jak w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="a826e-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="a826e-356">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-356">**Why**</span></span>

<span data-ttu-id="a826e-357">Ta zmiana została włączona, aby ułatwić i bardziej spójne do pracy z wykresów jednostki rozłączone podczas korzystania z kluczy generowanych przez magazyn.</span><span class="sxs-lookup"><span data-stu-id="a826e-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="a826e-358">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-358">**Mitigations**</span></span>

<span data-ttu-id="a826e-359">Ta zmiana może podzielić aplikację, jeśli typ jednostki jest skonfigurowany do używania wygenerowanych kluczy, ale wartości klucza są jawnie ustawiane dla nowych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a826e-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="a826e-360">Poprawka jest jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="a826e-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="a826e-361">Na przykład z płynnym interfejsem API:</span><span class="sxs-lookup"><span data-stu-id="a826e-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="a826e-362">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="a826e-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="a826e-363">Kaskadowe usunięcia są teraz usuwane natychmiast domyślnie</span><span class="sxs-lookup"><span data-stu-id="a826e-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="a826e-364">#10114 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="a826e-365">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-365">**Old behavior**</span></span>

<span data-ttu-id="a826e-366">Przed 3.0 EF Core stosowane akcje kaskadowe (usuwanie jednostek zależnych, gdy wymagany podmiot zabezpieczeń jest usuwany lub gdy relacja z wymaganym podmiotem jest zerwana) nie stało się, dopóki SaveChanges został wywołany.</span><span class="sxs-lookup"><span data-stu-id="a826e-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="a826e-367">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-367">**New behavior**</span></span>

<span data-ttu-id="a826e-368">Począwszy od 3.0, EF Core stosuje akcje kaskadowe, gdy tylko zostanie wykryty warunek wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="a826e-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="a826e-369">Na przykład `context.Remove()` wywołanie usunięcia jednostki głównej spowoduje, że wszystkie śledzone powiązane `Deleted` wymagane zależności również zostaną ustawione natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a826e-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="a826e-370">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-370">**Why**</span></span>

<span data-ttu-id="a826e-371">Ta zmiana została wywiązywana w celu poprawy środowiska dla scenariuszy powiązania danych i inspekcji, w których ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="a826e-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="a826e-372">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-372">**Mitigations**</span></span>

<span data-ttu-id="a826e-373">Poprzednie zachowanie można przywrócić za `context.ChangeTracker`pomocą ustawień na .</span><span class="sxs-lookup"><span data-stu-id="a826e-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="a826e-374">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="a826e-375">Wczesne ładowanie powiązanych jednostek odbywa się teraz w jednej kwerendzie</span><span class="sxs-lookup"><span data-stu-id="a826e-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="a826e-376">#18022 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="a826e-377">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-377">**Old behavior**</span></span>

<span data-ttu-id="a826e-378">Przed 3.0, chętnie ładowania nawigacji `Include` kolekcji za pośrednictwem operatorów spowodowało wiele zapytań do wygenerowania w relacyjnej bazy danych, po jednym dla każdego typu jednostki pokrewnej.</span><span class="sxs-lookup"><span data-stu-id="a826e-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="a826e-379">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-379">**New behavior**</span></span>

<span data-ttu-id="a826e-380">Począwszy od 3.0, EF Core generuje pojedyncze zapytanie z JOIN w relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="a826e-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="a826e-381">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-381">**Why**</span></span>

<span data-ttu-id="a826e-382">Wydawanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ spowodowało wiele problemów, w tym negatywną wydajność, ponieważ konieczne było wiele rund w obie strony bazy danych, a problemy z spójnością danych, ponieważ każda kwerenda mogła obserwować inny stan bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a826e-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="a826e-383">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-383">**Mitigations**</span></span>

<span data-ttu-id="a826e-384">Chociaż technicznie nie jest to zmiana podziału, może mieć znaczący wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę operatorów `Include` na nawigacji zbierania.</span><span class="sxs-lookup"><span data-stu-id="a826e-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="a826e-385">[Zobacz ten komentarz,](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) aby uzyskać więcej informacji i przepisywania zapytań w bardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="a826e-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="a826e-386">DeleteBehavior.Restrict ma czystsze semantyki</span><span class="sxs-lookup"><span data-stu-id="a826e-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="a826e-387">#12661 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="a826e-388">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-388">**Old behavior**</span></span>

<span data-ttu-id="a826e-389">Przed 3.0, `DeleteBehavior.Restrict` utworzone klucze `Restrict` obce w bazie danych z semantyki, ale także zmienił wewnętrzne korekty w sposób nieoczywidycy.</span><span class="sxs-lookup"><span data-stu-id="a826e-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="a826e-390">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-390">**New behavior**</span></span>

<span data-ttu-id="a826e-391">Począwszy od 3.0, zapewnia, `DeleteBehavior.Restrict` że `Restrict` klucze obce są tworzone z semantyki - to znaczy, bez kaskad; rzut na naruszenie ograniczeń - bez wpływu również EF wewnętrzne korekty.</span><span class="sxs-lookup"><span data-stu-id="a826e-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="a826e-392">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-392">**Why**</span></span>

<span data-ttu-id="a826e-393">Ta zmiana została wniesienia w celu poprawy doświadczenia podczas korzystania `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych skutków ubocznych.</span><span class="sxs-lookup"><span data-stu-id="a826e-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="a826e-394">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-394">**Mitigations**</span></span>

<span data-ttu-id="a826e-395">Poprzednie zachowanie można przywrócić `DeleteBehavior.ClientNoAction`za pomocą programu .</span><span class="sxs-lookup"><span data-stu-id="a826e-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="a826e-396">Typy zapytań są konsolidowane za pomocą typów encji</span><span class="sxs-lookup"><span data-stu-id="a826e-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="a826e-397">#14194 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="a826e-398">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-398">**Old behavior**</span></span>

<span data-ttu-id="a826e-399">Przed EF Core 3.0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na zapytanie danych, które nie definiują klucza podstawowego w sposób strukturalny.</span><span class="sxs-lookup"><span data-stu-id="a826e-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="a826e-400">Oznacza to, że typ kwerendy był używany do mapowania typów jednostek bez kluczy (bardziej prawdopodobne z widoku, ale prawdopodobnie z tabeli), podczas gdy zwykły typ jednostki był używany, gdy klucz był dostępny (bardziej prawdopodobne z tabeli, ale prawdopodobnie z widoku).</span><span class="sxs-lookup"><span data-stu-id="a826e-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="a826e-401">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-401">**New behavior**</span></span>

<span data-ttu-id="a826e-402">Typ kwerendy staje się teraz tylko typem jednostki bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a826e-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="a826e-403">Typy jednostek bezkluzywowe mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="a826e-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="a826e-404">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-404">**Why**</span></span>

<span data-ttu-id="a826e-405">Ta zmiana została wydana w celu zmniejszenia zamieszania wokół celu typów kwerend.</span><span class="sxs-lookup"><span data-stu-id="a826e-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="a826e-406">W szczególności są one bezkluzywne typy jednostek i są one z natury tylko do odczytu z tego powodu, ale nie powinny być używane tylko dlatego, że typ jednostki musi być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="a826e-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="a826e-407">Podobnie są one często mapowane do widoków, ale to tylko dlatego, że widoki często nie definiują kluczy.</span><span class="sxs-lookup"><span data-stu-id="a826e-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="a826e-408">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-408">**Mitigations**</span></span>

<span data-ttu-id="a826e-409">Następujące części interfejsu API są przestarzałe:</span><span class="sxs-lookup"><span data-stu-id="a826e-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="a826e-410">**`ModelBuilder.Query<>()`**- Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.</span><span class="sxs-lookup"><span data-stu-id="a826e-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="a826e-411">To nadal nie będzie skonfigurowany przez konwencję, aby uniknąć błędnej konfiguracji, gdy oczekuje się klucza podstawowego, ale nie pasuje do konwencji.</span><span class="sxs-lookup"><span data-stu-id="a826e-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="a826e-412">**`DbQuery<>`**- Zamiast `DbSet<>` tego należy użyć.</span><span class="sxs-lookup"><span data-stu-id="a826e-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="a826e-413">**`DbContext.Query<>()`**- Zamiast `DbContext.Set<>()` tego należy użyć.</span><span class="sxs-lookup"><span data-stu-id="a826e-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="a826e-414">Interfejs API konfiguracji dla relacji typu należącego uległ zmianie</span><span class="sxs-lookup"><span data-stu-id="a826e-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="a826e-415">[Problem śledzenia #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[problem śledzenia #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="a826e-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="a826e-416">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-416">**Old behavior**</span></span>

<span data-ttu-id="a826e-417">Przed EF Core 3.0 konfiguracja relacji należącej `OwnsOne` do `OwnsMany` własnością została wykonana bezpośrednio po lub wywołania.</span><span class="sxs-lookup"><span data-stu-id="a826e-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="a826e-418">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-418">**New behavior**</span></span>

<span data-ttu-id="a826e-419">Począwszy od EF Core 3.0, istnieje teraz płynny interfejs API, aby skonfigurować właściwość nawigacji do właściciela za pomocą `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="a826e-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="a826e-420">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="a826e-421">Konfiguracja związana z relacją między właścicielem a `WithOwner()` własnością powinna być teraz powiązana po podobnej do sposobu konfigurowania innych relacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="a826e-422">Podczas gdy konfiguracja dla samego typu należącego do sieci nadal będzie pokutowana łańcuchem po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="a826e-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="a826e-423">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-423">For example:</span></span>

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

<span data-ttu-id="a826e-424">Dodatkowo `Entity()`wywołanie `HasOne()`, `Set()` lub z własnością docelowej typu będzie teraz zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a826e-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="a826e-425">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-425">**Why**</span></span>

<span data-ttu-id="a826e-426">Ta zmiana została wydzielona w celu utworzenia czystszego oddzielenia między konfigurowaniem samego typu należącego a _relacją z_ typem należącym do państwa.</span><span class="sxs-lookup"><span data-stu-id="a826e-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="a826e-427">To z kolei usuwa niejednoznaczności i zamieszanie `HasForeignKey`wokół metod takich jak .</span><span class="sxs-lookup"><span data-stu-id="a826e-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="a826e-428">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-428">**Mitigations**</span></span>

<span data-ttu-id="a826e-429">Zmień konfigurację relacji typu należącego do sieci, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a826e-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="a826e-430">Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne</span><span class="sxs-lookup"><span data-stu-id="a826e-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="a826e-431">#9005 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="a826e-432">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-432">**Old behavior**</span></span>

<span data-ttu-id="a826e-433">Należy wziąć pod uwagę następujący model:</span><span class="sxs-lookup"><span data-stu-id="a826e-433">Consider the following model:</span></span>
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
<span data-ttu-id="a826e-434">Przed EF Core `OrderDetails` 3.0, `Order` jeśli jest własnością lub jawnie `OrderDetails` mapowane do tej samej `Order`tabeli, a następnie wystąpienie było zawsze wymagane podczas dodawania nowego . .</span><span class="sxs-lookup"><span data-stu-id="a826e-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="a826e-435">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-435">**New behavior**</span></span>

<span data-ttu-id="a826e-436">Począwszy od 3.0 EF Core `Order` pozwala `OrderDetails` dodać bez `OrderDetails` i mapuje wszystkie właściwości z wyjątkiem klucza podstawowego do kolumn nullable.</span><span class="sxs-lookup"><span data-stu-id="a826e-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="a826e-437">Podczas wykonywania zapytań `OrderDetails` `null` EF Core ustawia, czy którykolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie `null`ma wymaganych właściwości oprócz klucza podstawowego i wszystkie właściwości są .</span><span class="sxs-lookup"><span data-stu-id="a826e-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="a826e-438">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-438">**Mitigations**</span></span>

<span data-ttu-id="a826e-439">Jeśli model ma udostępnianie tabeli zależne od wszystkich kolumn opcjonalnych, ale `null` nawigacja wskazująca na niego nie powinna `null`być, aplikacja powinna zostać zmodyfikowana do obsługi przypadków, gdy nawigacja jest .</span><span class="sxs-lookup"><span data-stu-id="a826e-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="a826e-440">Jeśli nie jest to możliwe, wymagana właściwość powinna zostać dodana do typu`null` jednostki lub co najmniej jedna właściwość powinna mieć przypisaną do niej wartość nienabytą.</span><span class="sxs-lookup"><span data-stu-id="a826e-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="a826e-441">Wszystkie jednostki współużytkujące tabelę z kolumną tokenu współbieżności muszą mapować ją na właściwość</span><span class="sxs-lookup"><span data-stu-id="a826e-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="a826e-442">#14154 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="a826e-443">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-443">**Old behavior**</span></span>

<span data-ttu-id="a826e-444">Należy wziąć pod uwagę następujący model:</span><span class="sxs-lookup"><span data-stu-id="a826e-444">Consider the following model:</span></span>
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
<span data-ttu-id="a826e-445">Przed EF Core `OrderDetails` 3.0, `Order` jeśli jest własnością lub jawnie mapowane `OrderDetails` do `Version` tej samej tabeli, a następnie aktualizowanie po prostu nie zaktualizuje wartość na kliencie i następna aktualizacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="a826e-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="a826e-446">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-446">**New behavior**</span></span>

<span data-ttu-id="a826e-447">Począwszy od 3.0, EF Core `Version` propaguje nową wartość, jeśli `Order` jest właścicielem `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="a826e-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="a826e-448">W przeciwnym razie wyjątek jest zgłaszany podczas sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="a826e-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="a826e-449">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-449">**Why**</span></span>

<span data-ttu-id="a826e-450">Ta zmiana została wniesienia, aby uniknąć przestarzałej wartości tokenu współbieżności, gdy tylko jedna z jednostek mapowanych do tej samej tabeli jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="a826e-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="a826e-451">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-451">**Mitigations**</span></span>

<span data-ttu-id="a826e-452">Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość mapowane do kolumny tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="a826e-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="a826e-453">Jest możliwe, aby utworzyć jeden w stanie cienia:</span><span class="sxs-lookup"><span data-stu-id="a826e-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="a826e-454">Nie można wyszukiwać jednostek będących własnością bez użycia zapytania śledzenia przez właściciela</span><span class="sxs-lookup"><span data-stu-id="a826e-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="a826e-455">#18876 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="a826e-456">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-456">**Old behavior**</span></span>

<span data-ttu-id="a826e-457">Przed EF Core 3.0, należące do jednostek można wyszukiwać jako inne nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="a826e-458">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-458">**New behavior**</span></span>

<span data-ttu-id="a826e-459">Począwszy od 3.0 EF Core zrzuci, jeśli kwerenda śledzenia projektów jednostki własnością bez właściciela.</span><span class="sxs-lookup"><span data-stu-id="a826e-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="a826e-460">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-460">**Why**</span></span>

<span data-ttu-id="a826e-461">Jednostki należące nie mogą być manipulowane bez właściciela, więc w większości przypadków ich wykonywanie zapytań w ten sposób jest błędem.</span><span class="sxs-lookup"><span data-stu-id="a826e-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="a826e-462">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-462">**Mitigations**</span></span>

<span data-ttu-id="a826e-463">Jeśli jednostka należąca powinna być śledzona do zmodyfikowannia w jakikolwiek sposób później, właściciel powinien zostać uwzględniony w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="a826e-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="a826e-464">W przeciwnym `AsNoTracking()` razie dodaj połączenie:</span><span class="sxs-lookup"><span data-stu-id="a826e-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="a826e-465">Właściwości dziedziczone z typów niezamapowanych są teraz mapowane na jedną kolumnę dla wszystkich typów pochodnych</span><span class="sxs-lookup"><span data-stu-id="a826e-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="a826e-466">#13998 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="a826e-467">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-467">**Old behavior**</span></span>

<span data-ttu-id="a826e-468">Należy wziąć pod uwagę następujący model:</span><span class="sxs-lookup"><span data-stu-id="a826e-468">Consider the following model:</span></span>
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

<span data-ttu-id="a826e-469">Przed EF Core 3.0, `ShippingAddress` właściwość będzie mapowane `BulkOrder` `Order` do oddzielnych kolumn dla i domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a826e-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="a826e-470">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-470">**New behavior**</span></span>

<span data-ttu-id="a826e-471">Począwszy od 3.0, EF Core `ShippingAddress`tworzy tylko jedną kolumnę dla .</span><span class="sxs-lookup"><span data-stu-id="a826e-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="a826e-472">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-472">**Why**</span></span>

<span data-ttu-id="a826e-473">Stary behavoir był nieoczekiwany.</span><span class="sxs-lookup"><span data-stu-id="a826e-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="a826e-474">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-474">**Mitigations**</span></span>

<span data-ttu-id="a826e-475">Właściwość nadal może być jawnie mapowane do oddzielnej kolumny na typy pochodne:</span><span class="sxs-lookup"><span data-stu-id="a826e-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="a826e-476">Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość główna</span><span class="sxs-lookup"><span data-stu-id="a826e-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="a826e-477">#13274 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="a826e-478">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-478">**Old behavior**</span></span>

<span data-ttu-id="a826e-479">Należy wziąć pod uwagę następujący model:</span><span class="sxs-lookup"><span data-stu-id="a826e-479">Consider the following model:</span></span>
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
<span data-ttu-id="a826e-480">Przed EF Core 3.0, `CustomerId` właściwość będzie używana dla klucza obcego przez konwencję.</span><span class="sxs-lookup"><span data-stu-id="a826e-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="a826e-481">Jednak jeśli `Order` jest typem własnością, to `CustomerId` również zrobić klucz podstawowy i zwykle nie jest to oczekiwanie.</span><span class="sxs-lookup"><span data-stu-id="a826e-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="a826e-482">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-482">**New behavior**</span></span>

<span data-ttu-id="a826e-483">Począwszy od 3.0, EF Core nie próbuje używać właściwości dla kluczy obcych przez konwencję, jeśli mają taką samą nazwę jak principal właściwości.</span><span class="sxs-lookup"><span data-stu-id="a826e-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="a826e-484">Nazwa typu głównego połączona z nazwą właściwości głównej i nazwa nawigacji połączona z wzorcami nazw właściwości głównej są nadal dopasowywać.</span><span class="sxs-lookup"><span data-stu-id="a826e-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="a826e-485">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-485">For example:</span></span>

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

<span data-ttu-id="a826e-486">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-486">**Why**</span></span>

<span data-ttu-id="a826e-487">Ta zmiana została wyjęty, aby uniknąć błędnego definiowania właściwości klucza podstawowego na typie należącym do organizacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="a826e-488">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-488">**Mitigations**</span></span>

<span data-ttu-id="a826e-489">Jeśli właściwość miała być kluczem obcym, a tym samym częścią klucza podstawowego, a następnie jawnie skonfigurować go jako takie.</span><span class="sxs-lookup"><span data-stu-id="a826e-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="a826e-490">Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest już używane przed zakończeniem transakcji TransactionScope</span><span class="sxs-lookup"><span data-stu-id="a826e-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="a826e-491">#14218 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="a826e-492">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-492">**Old behavior**</span></span>

<span data-ttu-id="a826e-493">Przed EF Core 3.0, jeśli kontekst `TransactionScope`otwiera połączenie wewnątrz , połączenie `TransactionScope` pozostaje otwarte, gdy bieżący jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="a826e-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="a826e-494">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-494">**New behavior**</span></span>

<span data-ttu-id="a826e-495">Począwszy od 3.0, EF Core zamyka połączenie, gdy tylko zostanie to zrobione przy użyciu go.</span><span class="sxs-lookup"><span data-stu-id="a826e-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="a826e-496">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-496">**Why**</span></span>

<span data-ttu-id="a826e-497">Ta zmiana pozwala na użycie wielu `TransactionScope`kontekstów w tym samym .</span><span class="sxs-lookup"><span data-stu-id="a826e-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="a826e-498">Nowe zachowanie jest również zgodne z EF6.</span><span class="sxs-lookup"><span data-stu-id="a826e-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="a826e-499">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-499">**Mitigations**</span></span>

<span data-ttu-id="a826e-500">Jeśli połączenie musi pozostać otwarte `OpenConnection()` jawne wywołanie zapewni, że EF Core nie zamyka go przedwcześnie:</span><span class="sxs-lookup"><span data-stu-id="a826e-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="a826e-501">Każda właściwość używa niezależnego generowania klucza całkowitej w pamięci</span><span class="sxs-lookup"><span data-stu-id="a826e-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="a826e-502">#6872 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="a826e-503">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-503">**Old behavior**</span></span>

<span data-ttu-id="a826e-504">Przed EF Core 3.0 jeden generator wartości udostępnionej został użyty dla wszystkich właściwości klucza całkowitej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a826e-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="a826e-505">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-505">**New behavior**</span></span>

<span data-ttu-id="a826e-506">Począwszy od EF Core 3.0, każda właściwość klucza całkowitej pobiera własny generator wartości podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a826e-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="a826e-507">Ponadto jeśli baza danych zostanie usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.</span><span class="sxs-lookup"><span data-stu-id="a826e-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="a826e-508">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-508">**Why**</span></span>

<span data-ttu-id="a826e-509">Ta zmiana została wydzielona w celu dostosowania generowania klucza w pamięci bliżej do generowania klucza prawdziwej bazy danych i poprawić możliwość izolowania testów od siebie podczas korzystania z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a826e-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="a826e-510">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-510">**Mitigations**</span></span>

<span data-ttu-id="a826e-511">Może to spowodować przerwanie aplikacji, która polega na określonych wartościach klucza w pamięci, które mają zostać ustawione.</span><span class="sxs-lookup"><span data-stu-id="a826e-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="a826e-512">Zamiast tego należy rozważyć nie poleganie na określonych wartości klucza lub aktualizowanie, aby dopasować nowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="a826e-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="a826e-513">Pola zapasowe są używane domyślnie</span><span class="sxs-lookup"><span data-stu-id="a826e-513">Backing fields are used by default</span></span>

[<span data-ttu-id="a826e-514">#12430 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="a826e-515">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-515">**Old behavior**</span></span>

<span data-ttu-id="a826e-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span><span class="sxs-lookup"><span data-stu-id="a826e-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="a826e-517">Wyjątkiem było wykonanie kwerendy, gdzie pole kopii będzie ustawiane bezpośrednio, jeśli jest znane.</span><span class="sxs-lookup"><span data-stu-id="a826e-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="a826e-518">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-518">**New behavior**</span></span>

<span data-ttu-id="a826e-519">Począwszy od EF Core 3.0, jeśli pole zapasowe dla właściwości jest znane, a następnie EF Core zawsze odczytuje i zapisuje tę właściwość przy użyciu pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="a826e-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="a826e-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span><span class="sxs-lookup"><span data-stu-id="a826e-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="a826e-521">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-521">**Why**</span></span>

<span data-ttu-id="a826e-522">Ta zmiana została włączona, aby zapobiec EF Core błędnie wyzwalania logiki biznesowej domyślnie podczas wykonywania operacji bazy danych z udziałem jednostek.</span><span class="sxs-lookup"><span data-stu-id="a826e-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="a826e-523">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-523">**Mitigations**</span></span>

<span data-ttu-id="a826e-524">Zachowanie pre-3.0 można przywrócić poprzez konfigurację trybu `ModelBuilder`dostępu do właściwości na .</span><span class="sxs-lookup"><span data-stu-id="a826e-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="a826e-525">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="a826e-526">Throw, jeśli znaleziono wiele zgodnych pól podkładowych</span><span class="sxs-lookup"><span data-stu-id="a826e-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="a826e-527">#12523 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="a826e-528">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-528">**Old behavior**</span></span>

<span data-ttu-id="a826e-529">Przed EF Core 3.0, jeśli wiele pól pasuje do reguł znajdowania pola zapasowego właściwości, a następnie jedno pole zostanie wybrany na podstawie pewnej kolejności pierwszeństwa.</span><span class="sxs-lookup"><span data-stu-id="a826e-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="a826e-530">Może to spowodować, że niewłaściwe pole będzie używane w niejednoznacznych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="a826e-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="a826e-531">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-531">**New behavior**</span></span>

<span data-ttu-id="a826e-532">Począwszy od EF Core 3.0, jeśli wiele pól są dopasowane do tej samej właściwości, a następnie wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a826e-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="a826e-533">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-533">**Why**</span></span>

<span data-ttu-id="a826e-534">Ta zmiana została wyjęna, aby uniknąć dyskretnego używania jednego pola nad drugim, gdy tylko jedno może być poprawne.</span><span class="sxs-lookup"><span data-stu-id="a826e-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="a826e-535">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-535">**Mitigations**</span></span>

<span data-ttu-id="a826e-536">Właściwości z niejednoznacznymi polami zapasowymi muszą mieć pole do jawnego użycia.</span><span class="sxs-lookup"><span data-stu-id="a826e-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="a826e-537">Na przykład przy użyciu płynnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a826e-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="a826e-538">Nazwy właściwości tylko do pól powinny być zgodne z nazwą pola</span><span class="sxs-lookup"><span data-stu-id="a826e-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="a826e-539">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-539">**Old behavior**</span></span>

<span data-ttu-id="a826e-540">Przed EF Core 3.0, właściwość może być określona przez wartość ciągu i jeśli nie znaleziono żadnej właściwości o tej nazwie w typie .NET, a następnie EF Core spróbuje dopasować go do pola przy użyciu reguł konwencji.</span><span class="sxs-lookup"><span data-stu-id="a826e-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="a826e-541">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-541">**New behavior**</span></span>

<span data-ttu-id="a826e-542">Począwszy od EF Core 3.0, właściwość tylko do pól musi dokładnie odpowiadać nazwie pola.</span><span class="sxs-lookup"><span data-stu-id="a826e-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="a826e-543">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-543">**Why**</span></span>

<span data-ttu-id="a826e-544">Ta zmiana została wytyczona w celu uniknięcia używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale także sprawia, że pasujące reguły dla właściwości tylko w polu są takie same, jak w przypadku właściwości mapowanych do właściwości CLR.</span><span class="sxs-lookup"><span data-stu-id="a826e-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="a826e-545">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-545">**Mitigations**</span></span>

<span data-ttu-id="a826e-546">Właściwości tylko pola muszą mieć taką samą nazwę jak pole, do którego są mapowane.</span><span class="sxs-lookup"><span data-stu-id="a826e-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="a826e-547">W przyszłej wersji EF Core po 3.0 planujemy ponownie włączyć jawne skonfigurowanie nazwy pola, która różni się od nazwy właściwości (patrz [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)problem):</span><span class="sxs-lookup"><span data-stu-id="a826e-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="a826e-548">AddDbContext/AddDbContextPool nie będzie już wywoływać addlogging i AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="a826e-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="a826e-549">#14756 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="a826e-550">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-550">**Old behavior**</span></span>

<span data-ttu-id="a826e-551">Przed EF Core `AddDbContext` 3.0, wywołanie lub `AddDbContextPool` również zarejestrować rejestrowanie i buforowanie pamięci usług z DI poprzez wywołania [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a826e-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="a826e-552">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-552">**New behavior**</span></span>

<span data-ttu-id="a826e-553">Począwszy od EF Core 3.0 `AddDbContext` i `AddDbContextPool` nie będzie już rejestrować tych usług z iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="a826e-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="a826e-554">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-554">**Why**</span></span>

<span data-ttu-id="a826e-555">EF Core 3.0 nie wymaga, aby te usługi znajdują się w kontenerze DI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="a826e-556">Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie będzie nadal używany przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="a826e-557">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-557">**Mitigations**</span></span>

<span data-ttu-id="a826e-558">Jeśli aplikacja potrzebuje tych usług, a następnie zarejestrować je jawnie z kontenera DI przy użyciu [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a826e-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="a826e-559">AddEntityFramework\* dodaje IMemoryCache z limitem rozmiaru</span><span class="sxs-lookup"><span data-stu-id="a826e-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="a826e-560">#12905 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="a826e-561">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-561">**Old behavior**</span></span>

<span data-ttu-id="a826e-562">Przed EF Core 3.0 metody wywoływania `AddEntityFramework*` również zarejestrować usługi buforowania pamięci z DI bez limitu rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="a826e-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="a826e-563">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-563">**New behavior**</span></span>

<span data-ttu-id="a826e-564">Począwszy od EF Core 3.0, `AddEntityFramework*` zarejestruje usługę IMemoryCache z limitem rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="a826e-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="a826e-565">Jeśli inne usługi dodane później zależą od IMemoryCache mogą szybko osiągnąć domyślny limit powodujący wyjątki lub obniżoną wydajność.</span><span class="sxs-lookup"><span data-stu-id="a826e-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="a826e-566">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-566">**Why**</span></span>

<span data-ttu-id="a826e-567">Przy użyciu IMemoryCache bez limitu może spowodować użycie pamięci niekontrolowane, jeśli występuje błąd w logice buforowania kwerend lub kwerendy są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="a826e-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="a826e-568">Posiadanie domyślnego limitu zmniejsza potencjalny atak DoS.</span><span class="sxs-lookup"><span data-stu-id="a826e-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="a826e-569">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-569">**Mitigations**</span></span>

<span data-ttu-id="a826e-570">W większości `AddEntityFramework*` przypadków wywołanie `AddDbContext` `AddDbContextPool` nie jest konieczne, jeśli lub jest wywoływana, jak również.</span><span class="sxs-lookup"><span data-stu-id="a826e-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="a826e-571">W związku z tym najlepszym środkiem zaradczym jest usunięcie wywołania. `AddEntityFramework*`</span><span class="sxs-lookup"><span data-stu-id="a826e-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="a826e-572">Jeśli aplikacja potrzebuje tych usług, a następnie zarejestrować implementację IMemoryCache jawnie z kontenerem DI wcześniej przy użyciu [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a826e-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="a826e-573">DbContext.Entry wykonuje teraz lokalne zmiany wykrywania</span><span class="sxs-lookup"><span data-stu-id="a826e-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="a826e-574">#13552 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a826e-575">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-575">**Old behavior**</span></span>

<span data-ttu-id="a826e-576">Przed EF Core 3.0 wywołanie `DbContext.Entry` spowoduje, że zmiany mają być wykrywane dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="a826e-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="a826e-577">Dzięki temu stan ujawniony `EntityEntry` w tym kraju był aktualny.</span><span class="sxs-lookup"><span data-stu-id="a826e-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="a826e-578">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-578">**New behavior**</span></span>

<span data-ttu-id="a826e-579">Począwszy od EF Core 3.0, wywołanie `DbContext.Entry` będzie teraz tylko próbować wykryć zmiany w danej jednostce i wszystkie śledzone jednostki główne związane z nim.</span><span class="sxs-lookup"><span data-stu-id="a826e-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="a826e-580">Oznacza to, że zmiany w innym miejscu może nie zostały wykryte przez wywołanie tej metody, co może mieć wpływ na stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="a826e-581">Należy zauważyć, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false` to nawet to wykrywanie zmian lokalnych zostanie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="a826e-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="a826e-582">Inne metody, które powodują wykrywanie `SaveChanges`zmian — na `DetectChanges` przykład `ChangeTracker.Entries` i --nadal powodują pełne wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="a826e-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="a826e-583">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-583">**Why**</span></span>

<span data-ttu-id="a826e-584">Ta zmiana została włączona w `context.Entry`celu poprawy domyślnej wydajności używania programu .</span><span class="sxs-lookup"><span data-stu-id="a826e-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="a826e-585">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-585">**Mitigations**</span></span>

<span data-ttu-id="a826e-586">Wywołanie `ChangeTracker.DetectChanges()` jawnie `Entry` przed wywołaniem, aby upewnić się, że zachowanie pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="a826e-587">Klucze tablicy ciągów i bajtów nie są domyślnie generowane przez klienta</span><span class="sxs-lookup"><span data-stu-id="a826e-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="a826e-588">#14617 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="a826e-589">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-589">**Old behavior**</span></span>

<span data-ttu-id="a826e-590">Przed EF Core 3.0 `string` i `byte[]` właściwości klucza może być używany bez jawnie ustawienie wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="a826e-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="a826e-591">W takim przypadku wartość klucza zostanie wygenerowana na kliencie jako identyfikator `byte[]`GUID, serializowany do bajtów dla .</span><span class="sxs-lookup"><span data-stu-id="a826e-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="a826e-592">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-592">**New behavior**</span></span>

<span data-ttu-id="a826e-593">Począwszy od EF Core 3.0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="a826e-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="a826e-594">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-594">**Why**</span></span>

<span data-ttu-id="a826e-595">Ta zmiana została wnieszona, ponieważ wartości generowane przez `string` / `byte[]` klienta zazwyczaj nie są przydatne, a domyślne zachowanie utrudniało rozumienie o generowanych wartościach klucza w typowy sposób.</span><span class="sxs-lookup"><span data-stu-id="a826e-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="a826e-596">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-596">**Mitigations**</span></span>

<span data-ttu-id="a826e-597">Zachowanie pre-3.0 można uzyskać, wyraźnie określając, że właściwości klucza należy użyć wygenerowanych wartości, jeśli nie ma innej wartości innej wartości nie null.</span><span class="sxs-lookup"><span data-stu-id="a826e-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="a826e-598">Na przykład z płynnym interfejsem API:</span><span class="sxs-lookup"><span data-stu-id="a826e-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="a826e-599">Lub z adnotacjami danych:</span><span class="sxs-lookup"><span data-stu-id="a826e-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="a826e-600">ILoggerFactory jest teraz usługą o określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="a826e-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="a826e-601">#14698 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="a826e-602">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-602">**Old behavior**</span></span>

<span data-ttu-id="a826e-603">Przed EF Core 3.0, `ILoggerFactory` został zarejestrowany jako usługa singleton.</span><span class="sxs-lookup"><span data-stu-id="a826e-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="a826e-604">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-604">**New behavior**</span></span>

<span data-ttu-id="a826e-605">Począwszy od EF Core 3.0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.</span><span class="sxs-lookup"><span data-stu-id="a826e-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="a826e-606">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-606">**Why**</span></span>

<span data-ttu-id="a826e-607">Ta zmiana została wytyczona w `DbContext` celu umożliwienia skojarzenia rejestratora z wystąpieniem, co umożliwia inne funkcje i usuwa niektóre przypadki patologicznych zachowań, takich jak eksplozja wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="a826e-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="a826e-608">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-608">**Mitigations**</span></span>

<span data-ttu-id="a826e-609">Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na wewnętrznym dostawcy usług EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="a826e-610">To nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="a826e-610">This isn't common.</span></span>
<span data-ttu-id="a826e-611">W takich przypadkach większość rzeczy będzie nadal działać, ale `ILoggerFactory` każda usługa singleton, `ILoggerFactory` która była zależna od, będzie musiała zostać zmieniona, aby uzyskać w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="a826e-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="a826e-612">Jeśli napotkasz takie sytuacje, zgłoś problem w [monitorze problemów EF Core GitHub,](https://github.com/aspnet/EntityFrameworkCore/issues) aby poinformować nas, jak używasz, `ILoggerFactory` abyśmy mogli lepiej zrozumieć, jak nie złamać tego ponownie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="a826e-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="a826e-613">Serwery proxy z opóźnieniem nie zakładają już, że właściwości nawigacji są w pełni załadowane</span><span class="sxs-lookup"><span data-stu-id="a826e-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="a826e-614">#12780 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="a826e-615">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-615">**Old behavior**</span></span>

<span data-ttu-id="a826e-616">Przed EF Core 3.0, po a `DbContext` został usunięty nie było możliwości dowiedzenia się, czy dana właściwość nawigacji na jednostki uzyskane z tego kontekstu został w pełni załadowany, czy nie.</span><span class="sxs-lookup"><span data-stu-id="a826e-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="a826e-617">Zamiast tego serwery proxy załóżmy, że nawigacja referencyjna jest ładowana, jeśli ma wartość niezerową i że nawigacja z kolekcją jest ładowana, jeśli nie jest pusta.</span><span class="sxs-lookup"><span data-stu-id="a826e-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="a826e-618">W takich przypadkach próby z opóźnieniem obciążenia byłoby no-op.</span><span class="sxs-lookup"><span data-stu-id="a826e-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="a826e-619">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-619">**New behavior**</span></span>

<span data-ttu-id="a826e-620">Począwszy od EF Core 3.0, serwery proxy śledzić, czy właściwość nawigacji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="a826e-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="a826e-621">Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu zawsze będzie no-op, nawet wtedy, gdy załadowana nawigacja jest pusta lub null.</span><span class="sxs-lookup"><span data-stu-id="a826e-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="a826e-622">Z drugiej strony próba uzyskania dostępu do właściwości nawigacji, która nie jest ładowana, zgłosić wyjątek, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest niepustą kolekcją.</span><span class="sxs-lookup"><span data-stu-id="a826e-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="a826e-623">Jeśli taka sytuacja wystąpi, oznacza to, że kod aplikacji próbuje użyć ładowania z opóźnieniem w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona, aby tego nie robić.</span><span class="sxs-lookup"><span data-stu-id="a826e-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="a826e-624">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-624">**Why**</span></span>

<span data-ttu-id="a826e-625">Ta zmiana została wniesiena, aby zachowanie spójne i poprawne `DbContext` podczas próby z opóźnieniem obciążenia na disposed wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a826e-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="a826e-626">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-626">**Mitigations**</span></span>

<span data-ttu-id="a826e-627">Zaktualizuj kod aplikacji, aby nie podejmować prób ładowania z opóźnieniem z usuniętym kontekstem lub skonfigurować go jako no-op, jak opisano w komunikacie o wyjątku.</span><span class="sxs-lookup"><span data-stu-id="a826e-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="a826e-628">Nadmierne tworzenie wewnętrznych usługodawców jest teraz domyślnie błędem</span><span class="sxs-lookup"><span data-stu-id="a826e-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="a826e-629">#10236 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="a826e-630">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-630">**Old behavior**</span></span>

<span data-ttu-id="a826e-631">Przed EF Core 3.0, ostrzeżenie będzie rejestrowane dla aplikacji tworzącej patologiczną liczbę wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="a826e-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="a826e-632">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-632">**New behavior**</span></span>

<span data-ttu-id="a826e-633">Począwszy od EF Core 3.0, to ostrzeżenie jest teraz brane pod uwagę i błąd i wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a826e-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="a826e-634">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-634">**Why**</span></span>

<span data-ttu-id="a826e-635">Ta zmiana została wprowadzona do kierowania lepszym kodem aplikacji poprzez uwidacznianie tego przypadku patologicznego bardziej jawnie.</span><span class="sxs-lookup"><span data-stu-id="a826e-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="a826e-636">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-636">**Mitigations**</span></span>

<span data-ttu-id="a826e-637">Najbardziej odpowiednią przyczyną działania w przypadku napotkania tego błędu jest zrozumienie głównej przyczyny i zaprzestanie tworzenia tak wielu wewnętrznych dostawców usług.</span><span class="sxs-lookup"><span data-stu-id="a826e-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="a826e-638">Jednak błąd można przekonwertować z powrotem na ostrzeżenie (lub zignorować) za pomocą konfiguracji na . `DbContextOptionsBuilder`</span><span class="sxs-lookup"><span data-stu-id="a826e-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="a826e-639">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="a826e-640">Nowe zachowanie hasone/hasmany wywoływane z jednym ciągiem</span><span class="sxs-lookup"><span data-stu-id="a826e-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="a826e-641">#9171 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="a826e-642">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-642">**Old behavior**</span></span>

<span data-ttu-id="a826e-643">Przed EF Core 3.0, wywoływanie `HasOne` kodu lub `HasMany` z jednego ciągu został zinterpretowany w sposób mylący.</span><span class="sxs-lookup"><span data-stu-id="a826e-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="a826e-644">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="a826e-645">Kod wygląda jak odnosi `Samurai` się do innego typu `Entrance` jednostki przy użyciu właściwości nawigacji, która może być prywatna.</span><span class="sxs-lookup"><span data-stu-id="a826e-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="a826e-646">W rzeczywistości ten kod próbuje utworzyć relację `Entrance` do pewnego typu jednostki o nazwie bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="a826e-647">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-647">**New behavior**</span></span>

<span data-ttu-id="a826e-648">Począwszy od EF Core 3.0, kod powyżej teraz robi to, co wyglądało to powinno było robić wcześniej.</span><span class="sxs-lookup"><span data-stu-id="a826e-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="a826e-649">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-649">**Why**</span></span>

<span data-ttu-id="a826e-650">Stare zachowanie było bardzo mylące, zwłaszcza podczas odczytywania kodu konfiguracji i szukania błędów.</span><span class="sxs-lookup"><span data-stu-id="a826e-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="a826e-651">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-651">**Mitigations**</span></span>

<span data-ttu-id="a826e-652">Spowoduje to tylko przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów dla nazw typów i bez jawnego określania właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="a826e-653">Nie jest to powszechne.</span><span class="sxs-lookup"><span data-stu-id="a826e-653">This is not common.</span></span>
<span data-ttu-id="a826e-654">Poprzednie zachowanie można uzyskać za `null` pośrednictwem jawnie przekazywania nazwy właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="a826e-655">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="a826e-656">Typ zwracany dla kilku metod asynchronicznego został zmieniony z Task na ValueTask</span><span class="sxs-lookup"><span data-stu-id="a826e-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="a826e-657">#15184 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="a826e-658">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-658">**Old behavior**</span></span>

<span data-ttu-id="a826e-659">Następujące metody asynchronowe `Task<T>`wcześniej zwróciły:</span><span class="sxs-lookup"><span data-stu-id="a826e-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="a826e-660">`ValueGenerator.NextValueAsync()`(i klasy pochodne)</span><span class="sxs-lookup"><span data-stu-id="a826e-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="a826e-661">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-661">**New behavior**</span></span>

<span data-ttu-id="a826e-662">Wyżej wymienione metody zwracają `ValueTask<T>` teraz ponad `T` to samo, co poprzednio.</span><span class="sxs-lookup"><span data-stu-id="a826e-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="a826e-663">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-663">**Why**</span></span>

<span data-ttu-id="a826e-664">Ta zmiana zmniejsza liczbę alokacji sterty poniesione podczas wywoływania tych metod, poprawiając ogólną wydajność.</span><span class="sxs-lookup"><span data-stu-id="a826e-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="a826e-665">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-665">**Mitigations**</span></span>

<span data-ttu-id="a826e-666">Aplikacje po prostu oczekujące na powyższe interfejsy API muszą być tylko ponownie skompilowane — nie są konieczne żadne zmiany źródła.</span><span class="sxs-lookup"><span data-stu-id="a826e-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="a826e-667">Bardziej złożone użycie (np. przekazywanie `Task` `Task.WhenAny()`zwrócone do) zazwyczaj `ValueTask<T>` wymagają, aby `Task<T>` zwrócony być konwertowane na a, wywołując `AsTask()` na nim.</span><span class="sxs-lookup"><span data-stu-id="a826e-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="a826e-668">Należy zauważyć, że to neguje redukcję alokacji, która przynosi tę zmianę.</span><span class="sxs-lookup"><span data-stu-id="a826e-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="a826e-669">Adnotacja relacyjna:TypeMapping jest teraz tylko TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a826e-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="a826e-670">#9913 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="a826e-671">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-671">**Old behavior**</span></span>

<span data-ttu-id="a826e-672">Nazwa adnotacji adnotacji mapowania typów to "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a826e-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="a826e-673">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-673">**New behavior**</span></span>

<span data-ttu-id="a826e-674">Nazwa adnotacji adnotacji mapowania typów jest teraz "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a826e-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="a826e-675">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-675">**Why**</span></span>

<span data-ttu-id="a826e-676">Mapowania typów są teraz używane dla więcej niż tylko relacyjnych dostawców bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a826e-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="a826e-677">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-677">**Mitigations**</span></span>

<span data-ttu-id="a826e-678">Spowoduje to tylko przerwanie aplikacji, które uzyskują dostęp do mapowania typów bezpośrednio jako adnotacji, co nie jest powszechne.</span><span class="sxs-lookup"><span data-stu-id="a826e-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="a826e-679">Najbardziej odpowiednią akcją do naprawienia jest użycie powierzchni interfejsu API, aby uzyskać dostęp do mapowań typu, a nie bezpośrednio przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="a826e-680">ToTable na typ pochodny zgłasza wyjątek</span><span class="sxs-lookup"><span data-stu-id="a826e-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="a826e-681">#11811 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="a826e-682">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-682">**Old behavior**</span></span>

<span data-ttu-id="a826e-683">Przed EF Core 3.0, `ToTable()` wywoływane na typ pochodny będą ignorowane, ponieważ tylko strategia mapowania dziedziczenia był TPH, gdzie to nie jest prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="a826e-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="a826e-684">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-684">**New behavior**</span></span>

<span data-ttu-id="a826e-685">Począwszy od EF Core 3.0 i w ramach przygotowań do `ToTable()` dodawania obsługi TPT i TPC w nowszej wersji, wywoływane na typ pochodny będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="a826e-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="a826e-686">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-686">**Why**</span></span>

<span data-ttu-id="a826e-687">Obecnie nie jest prawidłowe mapowanie typu pochodnego do innej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a826e-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="a826e-688">Ta zmiana pozwala uniknąć zerwania w przyszłości, gdy staje się ważną rzeczą do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="a826e-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="a826e-689">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-689">**Mitigations**</span></span>

<span data-ttu-id="a826e-690">Usuń wszelkie próby mapowania typów pochodnych do innych tabel.</span><span class="sxs-lookup"><span data-stu-id="a826e-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="a826e-691">ForSqlServerHasIndex zastąpiony hasindexem</span><span class="sxs-lookup"><span data-stu-id="a826e-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="a826e-692">#12366 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="a826e-693">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-693">**Old behavior**</span></span>

<span data-ttu-id="a826e-694">Przed EF Core 3.0, pod warunkiem, `ForSqlServerHasIndex().ForSqlServerInclude()` że `INCLUDE`sposób konfigurowania kolumn używanych z .</span><span class="sxs-lookup"><span data-stu-id="a826e-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="a826e-695">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-695">**New behavior**</span></span>

<span data-ttu-id="a826e-696">Począwszy od EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwany na poziomie relacyjnej.</span><span class="sxs-lookup"><span data-stu-id="a826e-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="a826e-697">Użyj witryny `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="a826e-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="a826e-698">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-698">**Why**</span></span>

<span data-ttu-id="a826e-699">Ta zmiana została wprowadzona w celu `Include` konsolidacji interfejsu API dla indeksów w jednym miejscu dla wszystkich dostawców bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a826e-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="a826e-700">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-700">**Mitigations**</span></span>

<span data-ttu-id="a826e-701">Użyj nowego interfejsu API, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="a826e-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="a826e-702">Zmiany interfejsu API metadanych</span><span class="sxs-lookup"><span data-stu-id="a826e-702">Metadata API changes</span></span>

[<span data-ttu-id="a826e-703">#214 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a826e-704">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-704">**New behavior**</span></span>

<span data-ttu-id="a826e-705">Następujące właściwości zostały przekonwertowane na metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="a826e-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="a826e-706">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-706">**Why**</span></span>

<span data-ttu-id="a826e-707">Zmiana ta upraszcza implementację wyżej wymienionych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="a826e-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="a826e-708">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-708">**Mitigations**</span></span>

<span data-ttu-id="a826e-709">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="a826e-710">Zmiany interfejsu API metadanych specyficznych dla dostawcy</span><span class="sxs-lookup"><span data-stu-id="a826e-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="a826e-711">#214 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a826e-712">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-712">**New behavior**</span></span>

<span data-ttu-id="a826e-713">Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:</span><span class="sxs-lookup"><span data-stu-id="a826e-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="a826e-714">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-714">**Why**</span></span>

<span data-ttu-id="a826e-715">Zmiana ta upraszcza wdrażanie wyżej wymienionych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="a826e-716">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-716">**Mitigations**</span></span>

<span data-ttu-id="a826e-717">Użyj nowych metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="a826e-718">EF Core nie wysyła już pragmy do egzekwowania SQLite FK</span><span class="sxs-lookup"><span data-stu-id="a826e-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="a826e-719">#12151 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="a826e-720">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-720">**Old behavior**</span></span>

<span data-ttu-id="a826e-721">Przed EF Core 3.0 EF `PRAGMA foreign_keys = 1` Core będzie wysyłać po otwarciu połączenia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="a826e-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a826e-722">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-722">**New behavior**</span></span>

<span data-ttu-id="a826e-723">Począwszy od EF Core 3.0, `PRAGMA foreign_keys = 1` EF Core nie wysyła już po otwarciu połączenia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="a826e-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a826e-724">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-724">**Why**</span></span>

<span data-ttu-id="a826e-725">Ta zmiana została włączona, `SQLitePCLRaw.bundle_e_sqlite3` ponieważ EF Core używa domyślnie, co z kolei oznacza, że wymuszanie FK jest domyślnie włączone i nie musi być jawnie włączone przy każdym otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="a826e-726">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-726">**Mitigations**</span></span>

<span data-ttu-id="a826e-727">Klucze obce są domyślnie włączone w pliku SQLitePCLRaw.bundle_e_sqlite3, który jest domyślnie używany dla EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="a826e-728">W innych przypadkach klucze obce można `Foreign Keys=True` włączyć, określając w ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="a826e-729">Microsoft.EntityFrameworkCore.Sqlite teraz zależy od SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="a826e-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="a826e-730">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-730">**Old behavior**</span></span>

<span data-ttu-id="a826e-731">Przed EF Core 3.0, `SQLitePCLRaw.bundle_green`EF Core używane .</span><span class="sxs-lookup"><span data-stu-id="a826e-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="a826e-732">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-732">**New behavior**</span></span>

<span data-ttu-id="a826e-733">Począwszy od EF Core 3.0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="a826e-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="a826e-734">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-734">**Why**</span></span>

<span data-ttu-id="a826e-735">Ta zmiana została wydzielona tak, że wersja SQLite używane na iOS zgodne z innymi platformami.</span><span class="sxs-lookup"><span data-stu-id="a826e-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="a826e-736">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-736">**Mitigations**</span></span>

<span data-ttu-id="a826e-737">Aby użyć natywnej wersji SQLite `Microsoft.Data.Sqlite` w systemach iOS, skonfiguruj użycie innego `SQLitePCLRaw` pakietu.</span><span class="sxs-lookup"><span data-stu-id="a826e-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a826e-738">Wartości Guid są teraz przechowywane jako tekst na SQLite</span><span class="sxs-lookup"><span data-stu-id="a826e-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a826e-739">#15078 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="a826e-740">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-740">**Old behavior**</span></span>

<span data-ttu-id="a826e-741">Wartości guid były wcześniej przechowywane jako wartości blob na SQLite.</span><span class="sxs-lookup"><span data-stu-id="a826e-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="a826e-742">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-742">**New behavior**</span></span>

<span data-ttu-id="a826e-743">Wartości guid są teraz przechowywane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="a826e-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="a826e-744">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-744">**Why**</span></span>

<span data-ttu-id="a826e-745">Format binarny Guids nie jest standaryzowany.</span><span class="sxs-lookup"><span data-stu-id="a826e-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="a826e-746">Przechowywanie wartości jako TEXT sprawia, że baza danych jest bardziej zgodna z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="a826e-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a826e-747">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-747">**Mitigations**</span></span>

<span data-ttu-id="a826e-748">Istniejące bazy danych można migrować do nowego formatu, wykonując program SQL, podobnie jak następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="a826e-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="a826e-749">W EF Core można również kontynuować przy użyciu poprzedniego zachowania, konfigurując konwerter wartości na tych właściwościach.</span><span class="sxs-lookup"><span data-stu-id="a826e-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="a826e-750">Microsoft.Data.Sqlite pozostaje w stanie odczytywać wartości Guid zarówno z kolumny BLOB i TEXT; Jednak ponieważ domyślny format parametrów i stałych uległ zmianie, prawdopodobnie będziesz musiał podjąć działania w przypadku większości scenariuszy dotyczących guidów.</span><span class="sxs-lookup"><span data-stu-id="a826e-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a826e-751">Wartości char są teraz przechowywane jako tekst na SQLite</span><span class="sxs-lookup"><span data-stu-id="a826e-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a826e-752">#15020 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="a826e-753">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-753">**Old behavior**</span></span>

<span data-ttu-id="a826e-754">Wartości char były wcześniej sored jako wartości INTEGER na SQLite.</span><span class="sxs-lookup"><span data-stu-id="a826e-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="a826e-755">Na przykład wartość char *A* została przechowywana jako wartość całkowita 65.</span><span class="sxs-lookup"><span data-stu-id="a826e-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="a826e-756">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-756">**New behavior**</span></span>

<span data-ttu-id="a826e-757">Wartości char są teraz przechowywane jako TEKST.</span><span class="sxs-lookup"><span data-stu-id="a826e-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="a826e-758">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-758">**Why**</span></span>

<span data-ttu-id="a826e-759">Przechowywanie wartości jako TEKST jest bardziej naturalne i sprawia, że baza danych bardziej zgodne z innymi technologiami.</span><span class="sxs-lookup"><span data-stu-id="a826e-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a826e-760">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-760">**Mitigations**</span></span>

<span data-ttu-id="a826e-761">Istniejące bazy danych można migrować do nowego formatu, wykonując program SQL, podobnie jak następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="a826e-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="a826e-762">W EF Core można również kontynuować przy użyciu poprzedniego zachowania, konfigurując konwerter wartości na tych właściwościach.</span><span class="sxs-lookup"><span data-stu-id="a826e-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="a826e-763">Microsoft.Data.Sqlite również pozostaje w stanie odczytywać wartości znaków z kolumn INTEGER i TEXT, więc niektóre scenariusze nie mogą wymagać żadnych akcji.</span><span class="sxs-lookup"><span data-stu-id="a826e-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="a826e-764">Identyfikatory migracji są teraz generowane przy użyciu kalendarza kultury niezmiennej</span><span class="sxs-lookup"><span data-stu-id="a826e-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="a826e-765">#12978 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="a826e-766">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-766">**Old behavior**</span></span>

<span data-ttu-id="a826e-767">Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.</span><span class="sxs-lookup"><span data-stu-id="a826e-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="a826e-768">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-768">**New behavior**</span></span>

<span data-ttu-id="a826e-769">Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza kultury niezmiennej (gregoriańskiego).</span><span class="sxs-lookup"><span data-stu-id="a826e-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="a826e-770">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-770">**Why**</span></span>

<span data-ttu-id="a826e-771">Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania.</span><span class="sxs-lookup"><span data-stu-id="a826e-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="a826e-772">Za pomocą kalendarza niezmienne pozwala uniknąć zamawiania problemów, które mogą wynikać z członków zespołu o różnych kalendarzy systemowych.</span><span class="sxs-lookup"><span data-stu-id="a826e-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="a826e-773">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-773">**Mitigations**</span></span>

<span data-ttu-id="a826e-774">Ta zmiana dotyczy każdego, kto używa kalendarza niegregoriańskiego, w którym rok jest większy niż kalendarz gregoriański (jak tajski kalendarz buddyjski).</span><span class="sxs-lookup"><span data-stu-id="a826e-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="a826e-775">Istniejące identyfikatory migracji będą musiały zostać zaktualizowane, aby nowe migracje były porządkowane po istniejących migracjach.</span><span class="sxs-lookup"><span data-stu-id="a826e-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="a826e-776">Identyfikator migracji można znaleźć w atrybucie Migracja w plikach projektanta migracji.</span><span class="sxs-lookup"><span data-stu-id="a826e-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="a826e-777">Tabela historia migracji również musi zostać zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="a826e-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="a826e-778">UseRowNumberForPaging został usunięty</span><span class="sxs-lookup"><span data-stu-id="a826e-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="a826e-779">#16400 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="a826e-780">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-780">**Old behavior**</span></span>

<span data-ttu-id="a826e-781">Przed EF Core 3.0, `UseRowNumberForPaging` może służyć do generowania SQL dla stronicowania, który jest zgodny z programem SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="a826e-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="a826e-782">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-782">**New behavior**</span></span>

<span data-ttu-id="a826e-783">Począwszy od EF Core 3.0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny tylko z nowszych wersji programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a826e-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="a826e-784">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-784">**Why**</span></span>

<span data-ttu-id="a826e-785">Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy ze zmianami kwerend wprowadzonych w EF Core 3.0 jest znaczącą pracę.</span><span class="sxs-lookup"><span data-stu-id="a826e-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="a826e-786">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-786">**Mitigations**</span></span>

<span data-ttu-id="a826e-787">Zaleca się aktualizowanie do nowszej wersji programu SQL Server lub przy użyciu wyższego poziomu zgodności, tak aby wygenerowany SQL jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="a826e-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="a826e-788">Mając na to na myśli, jeśli nie jesteś w stanie tego zrobić, prosimy o [komentarz w sprawie śledzenia problemu](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółami.</span><span class="sxs-lookup"><span data-stu-id="a826e-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="a826e-789">Możemy ponownie przyjrzeć się tej decyzji na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="a826e-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="a826e-790">Informacje/metadane rozszerzenia zostały usunięte z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="a826e-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="a826e-791">#16119 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="a826e-792">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-792">**Old behavior**</span></span>

<span data-ttu-id="a826e-793">`IDbContextOptionsExtension`zawiera metody dostarczania metadanych dotyczących rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a826e-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="a826e-794">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-794">**New behavior**</span></span>

<span data-ttu-id="a826e-795">Te metody zostały przeniesione `DbContextOptionsExtensionInfo` do nowej abstrakcyjnej klasy podstawowej, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a826e-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="a826e-796">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-796">**Why**</span></span>

<span data-ttu-id="a826e-797">W wersjach od 2.0 do 3.0 musieliśmy dodać lub zmienić te metody kilka razy.</span><span class="sxs-lookup"><span data-stu-id="a826e-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="a826e-798">Podział ich na nową abstrakcyjną klasę podstawową ułatwi wprowadzanie tego rodzaju zmian bez przerywania istniejących rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="a826e-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="a826e-799">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-799">**Mitigations**</span></span>

<span data-ttu-id="a826e-800">Aktualizuj rozszerzenia, aby postępować zgodnie z nowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="a826e-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="a826e-801">Przykłady znajdują się w wielu `IDbContextOptionsExtension` implementacjach dla różnych rodzajów rozszerzeń w kodzie źródłowym EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="a826e-802">Nazwa logqueryPossibleExceptionWithAggregateOperator została zmieniona</span><span class="sxs-lookup"><span data-stu-id="a826e-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="a826e-803">#10985 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="a826e-804">**Zmień**</span><span class="sxs-lookup"><span data-stu-id="a826e-804">**Change**</span></span>

<span data-ttu-id="a826e-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na .</span><span class="sxs-lookup"><span data-stu-id="a826e-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="a826e-806">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-806">**Why**</span></span>

<span data-ttu-id="a826e-807">Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.</span><span class="sxs-lookup"><span data-stu-id="a826e-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="a826e-808">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-808">**Mitigations**</span></span>

<span data-ttu-id="a826e-809">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a826e-809">Use the new name.</span></span> <span data-ttu-id="a826e-810">(Należy zauważyć, że numer identyfikatora zdarzenia nie uległ zmianie).</span><span class="sxs-lookup"><span data-stu-id="a826e-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="a826e-811">Wyjaśnienie interfejsu API dla nazw ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="a826e-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="a826e-812">#10730 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="a826e-813">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-813">**Old behavior**</span></span>

<span data-ttu-id="a826e-814">Przed EF Core 3.0 nazwy ograniczeń klucza obcego były określane jako po prostu "nazwa".</span><span class="sxs-lookup"><span data-stu-id="a826e-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="a826e-815">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="a826e-816">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-816">**New behavior**</span></span>

<span data-ttu-id="a826e-817">Począwszy od EF Core 3.0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia".</span><span class="sxs-lookup"><span data-stu-id="a826e-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="a826e-818">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="a826e-819">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-819">**Why**</span></span>

<span data-ttu-id="a826e-820">Ta zmiana zapewnia spójność nazewnictwa w tym obszarze, a także wyjaśnia, że jest to nazwa ograniczenia klucza obcego, a nie kolumna lub nazwa właściwości, na którym jest zdefiniowany klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="a826e-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="a826e-821">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-821">**Mitigations**</span></span>

<span data-ttu-id="a826e-822">Użyj nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a826e-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="a826e-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync zostały upublicznione</span><span class="sxs-lookup"><span data-stu-id="a826e-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="a826e-824">#15997 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="a826e-825">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-825">**Old behavior**</span></span>

<span data-ttu-id="a826e-826">Przed EF Core 3.0 te metody były chronione.</span><span class="sxs-lookup"><span data-stu-id="a826e-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="a826e-827">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-827">**New behavior**</span></span>

<span data-ttu-id="a826e-828">Począwszy od EF Core 3.0, te metody są publiczne.</span><span class="sxs-lookup"><span data-stu-id="a826e-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="a826e-829">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-829">**Why**</span></span>

<span data-ttu-id="a826e-830">Te metody są używane przez EF do określenia, czy baza danych jest tworzona, ale puste.</span><span class="sxs-lookup"><span data-stu-id="a826e-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="a826e-831">Może to być również przydatne spoza EF przy określaniu, czy należy zastosować migracje.</span><span class="sxs-lookup"><span data-stu-id="a826e-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="a826e-832">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-832">**Mitigations**</span></span>

<span data-ttu-id="a826e-833">Zmień dostępność wszystkich nadpisań.</span><span class="sxs-lookup"><span data-stu-id="a826e-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="a826e-834">Microsoft.EntityFrameworkCore.Design jest teraz pakietem rozwojuzależności</span><span class="sxs-lookup"><span data-stu-id="a826e-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="a826e-835">#11506 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="a826e-836">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-836">**Old behavior**</span></span>

<span data-ttu-id="a826e-837">Przed EF Core 3.0, Microsoft.EntityFrameworkCore.Design był regularnym pakietem NuGet, którego zestaw może być przywoływane przez projekty, które od niego zależały.</span><span class="sxs-lookup"><span data-stu-id="a826e-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="a826e-838">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-838">**New behavior**</span></span>

<span data-ttu-id="a826e-839">Począwszy od EF Core 3.0, jest pakietem rozwojuzależności.</span><span class="sxs-lookup"><span data-stu-id="a826e-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="a826e-840">Oznacza to, że zależność nie będzie przepływać przechodnie do innych projektów i że nie można już, domyślnie, odwoływać się do jego zestawu.</span><span class="sxs-lookup"><span data-stu-id="a826e-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="a826e-841">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-841">**Why**</span></span>

<span data-ttu-id="a826e-842">Ten pakiet jest przeznaczony tylko do użycia w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="a826e-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="a826e-843">Wdrożone aplikacje nie powinny się do niego odwoływać.</span><span class="sxs-lookup"><span data-stu-id="a826e-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="a826e-844">Uczynienie pakietu developmentDependency wzmacnia to zalecenie.</span><span class="sxs-lookup"><span data-stu-id="a826e-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="a826e-845">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-845">**Mitigations**</span></span>

<span data-ttu-id="a826e-846">Jeśli chcesz odwołać się do tego pakietu, aby zastąpić zachowanie EF Core w czasie projektowania, następnie można zaktualizować packagereference metadanych elementu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a826e-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="a826e-847">Jeśli pakiet jest przywoływany przechodnie za pośrednictwem microsoft.EntityFrameworkCore.Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.</span><span class="sxs-lookup"><span data-stu-id="a826e-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="a826e-848">Takie wyraźne odwołanie należy dodać do każdego projektu, w którym potrzebne są typy z pakietu.</span><span class="sxs-lookup"><span data-stu-id="a826e-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="a826e-849">SQLitePCL.raw zaktualizowany do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a826e-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="a826e-850">#14824 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="a826e-851">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-851">**Old behavior**</span></span>

<span data-ttu-id="a826e-852">Microsoft.EntityFrameworkCore.Sqlite wcześniej zależało od wersji 1.1.12 sqlitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="a826e-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="a826e-853">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-853">**New behavior**</span></span>

<span data-ttu-id="a826e-854">Zaktualizowaliśmy nasz pakiet tak, aby był zależny od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a826e-855">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-855">**Why**</span></span>

<span data-ttu-id="a826e-856">Wersja 2.0.0 celów SQLitePCL.raw .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="a826e-857">Wcześniej była ukierunkowana na standard .NET Standard 1.1, który wymagał dużego zamknięcia przechodnich paczek do pracy.</span><span class="sxs-lookup"><span data-stu-id="a826e-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="a826e-858">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-858">**Mitigations**</span></span>

<span data-ttu-id="a826e-859">SQLitePCL.raw wersja 2.0.0 zawiera pewne przełomowe zmiany.</span><span class="sxs-lookup"><span data-stu-id="a826e-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a826e-860">Szczegółowe informacje można znaleźć w [informacjach o wersji.](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)</span><span class="sxs-lookup"><span data-stu-id="a826e-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="a826e-861">NetTopologySuite zaktualizowano do wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a826e-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="a826e-862">#14825 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="a826e-863">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-863">**Old behavior**</span></span>

<span data-ttu-id="a826e-864">Pakiety przestrzenne wcześniej zależały od wersji 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="a826e-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="a826e-865">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-865">**New behavior**</span></span>

<span data-ttu-id="a826e-866">Zaktualizowaliśmy nasz pakiet, aby był zależny od wersji 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="a826e-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a826e-867">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-867">**Why**</span></span>

<span data-ttu-id="a826e-868">Wersja 2.0.0 NetTopologySuite ma na celu rozwiązanie kilku problemów z użytecznością napotkanych przez użytkowników EF Core.</span><span class="sxs-lookup"><span data-stu-id="a826e-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="a826e-869">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-869">**Mitigations**</span></span>

<span data-ttu-id="a826e-870">NetTopologySuite wersja 2.0.0 zawiera kilka przełomowych zmian.</span><span class="sxs-lookup"><span data-stu-id="a826e-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a826e-871">Szczegółowe informacje można znaleźć w [informacjach o wersji.](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)</span><span class="sxs-lookup"><span data-stu-id="a826e-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="a826e-872">Microsoft.Data.SqlClient jest używany zamiast System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="a826e-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="a826e-873">#15636 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="a826e-874">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-874">**Old behavior**</span></span>

<span data-ttu-id="a826e-875">Microsoft.EntityFrameWorkCore.SqlServer wcześniej zależało od System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="a826e-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="a826e-876">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-876">**New behavior**</span></span>

<span data-ttu-id="a826e-877">Zaktualizowaliśmy nasz pakiet, aby był zależny od pliku Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="a826e-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="a826e-878">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-878">**Why**</span></span>

<span data-ttu-id="a826e-879">Microsoft.Data.SqlClient jest flagowym sterownikiem dostępu do danych dla programu SQL Server w przyszłości, a System.Data.SqlClient nie jest już przedmiotem rozwoju.</span><span class="sxs-lookup"><span data-stu-id="a826e-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="a826e-880">Niektóre ważne funkcje, takie jak Zawsze zaszyfrowane, są dostępne tylko w witrynie Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="a826e-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="a826e-881">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-881">**Mitigations**</span></span>

<span data-ttu-id="a826e-882">Jeśli kod ma bezpośrednią zależność od System.Data.SqlClient, należy go zmienić na odwołanie Microsoft.Data.SqlClient zamiast; ponieważ dwa pakiety zachowują bardzo wysoki stopień zgodności interfejsu API, powinna to być tylko prosta zmiana pakietu i obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="a826e-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="a826e-883">Należy skonfigurować wiele niejednoznacznych relacji samonawodujących się</span><span class="sxs-lookup"><span data-stu-id="a826e-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="a826e-884">#13573 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="a826e-885">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-885">**Old behavior**</span></span>

<span data-ttu-id="a826e-886">Typ jednostki z wieloma właściwościami nawigacji jednokierunkowej i pasującymi FK został niepoprawnie skonfigurowany jako pojedyncza relacja.</span><span class="sxs-lookup"><span data-stu-id="a826e-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="a826e-887">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-887">For example:</span></span>

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

<span data-ttu-id="a826e-888">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-888">**New behavior**</span></span>

<span data-ttu-id="a826e-889">Ten scenariusz jest teraz wykrywany w budynku modelu i wyjątek wskazuje, że model jest niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="a826e-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="a826e-890">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-890">**Why**</span></span>

<span data-ttu-id="a826e-891">Wynikowy model był niejednoznaczny i prawdopodobnie zwykle będzie błędny w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="a826e-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="a826e-892">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-892">**Mitigations**</span></span>

<span data-ttu-id="a826e-893">Użyj pełnej konfiguracji relacji.</span><span class="sxs-lookup"><span data-stu-id="a826e-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="a826e-894">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a826e-894">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="a826e-895">DbFunction.Schema jest null lub pusty ciąg konfiguruje go do domyślnego schematu modelu</span><span class="sxs-lookup"><span data-stu-id="a826e-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="a826e-896">#12757 problemu śledzenia</span><span class="sxs-lookup"><span data-stu-id="a826e-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="a826e-897">**Stare zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-897">**Old behavior**</span></span>

<span data-ttu-id="a826e-898">DbFunction skonfigurowany ze schematem jako pusty ciąg został potraktowany jako wbudowana funkcja bez schematu.</span><span class="sxs-lookup"><span data-stu-id="a826e-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="a826e-899">Na przykład następujący `DatePart` kod będzie `DATEPART` mapować funkcję CLR do wbudowanej funkcji na SqlServer.</span><span class="sxs-lookup"><span data-stu-id="a826e-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="a826e-900">**Nowe zachowanie**</span><span class="sxs-lookup"><span data-stu-id="a826e-900">**New behavior**</span></span>

<span data-ttu-id="a826e-901">Wszystkie mapowania DbFunction są uważane za mapowane na funkcje zdefiniowane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a826e-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="a826e-902">W związku z tym wartość pustego ciągu spowoduje umieszczenie funkcji wewnątrz domyślnego schematu dla modelu.</span><span class="sxs-lookup"><span data-stu-id="a826e-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="a826e-903">Który może być schemat skonfigurowany jawnie `modelBuilder.HasDefaultSchema()` za `dbo` pośrednictwem płynnego interfejsu API lub w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="a826e-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="a826e-904">**Dlaczego**</span><span class="sxs-lookup"><span data-stu-id="a826e-904">**Why**</span></span>

<span data-ttu-id="a826e-905">Wcześniej schemat jest pusty był sposób leczenia tej funkcji jest wbudowany, ale logika ta ma zastosowanie tylko do SqlServer, gdzie wbudowane funkcje nie należą do żadnego schematu.</span><span class="sxs-lookup"><span data-stu-id="a826e-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="a826e-906">**Środki zaradcze**</span><span class="sxs-lookup"><span data-stu-id="a826e-906">**Mitigations**</span></span>

<span data-ttu-id="a826e-907">Skonfiguruj tłumaczenie DbFunction ręcznie, aby zamapować je na wbudowaną funkcję.</span><span class="sxs-lookup"><span data-stu-id="a826e-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
