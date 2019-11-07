---
title: Co nowego w EF Core 1,0 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655860"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="4c658-102">Funkcje zawarte w EF Core 1,0</span><span class="sxs-lookup"><span data-stu-id="4c658-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="4c658-103">Platformy</span><span class="sxs-lookup"><span data-stu-id="4c658-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="4c658-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="4c658-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="4c658-105">Obejmuje konsole, WPF, WinForms, ASP.NET 4 itd.</span><span class="sxs-lookup"><span data-stu-id="4c658-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="4c658-106">.NET Standard 1,3</span><span class="sxs-lookup"><span data-stu-id="4c658-106">.NET Standard 1.3</span></span>

<span data-ttu-id="4c658-107">Uwzględnienie ASP.NET Core .NET Framework i .NET Core w systemach Windows, OSX i Linux.</span><span class="sxs-lookup"><span data-stu-id="4c658-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="4c658-108">Modelowania</span><span class="sxs-lookup"><span data-stu-id="4c658-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="4c658-109">Podstawowe modelowanie</span><span class="sxs-lookup"><span data-stu-id="4c658-109">Basic modelling</span></span>

<span data-ttu-id="4c658-110">Na podstawie jednostek POCO z właściwościami get/set wspólnych typów skalarnych (`int`, `string`itp.).</span><span class="sxs-lookup"><span data-stu-id="4c658-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="4c658-111">Relacje i właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="4c658-111">Relationships and navigation properties</span></span>

<span data-ttu-id="4c658-112">Relacje "jeden do wielu" i "jeden do zera" można określić w modelu na podstawie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="4c658-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="4c658-113">Właściwości nawigacji prostej kolekcji lub typów referencyjnych mogą być skojarzone z tymi relacjami.</span><span class="sxs-lookup"><span data-stu-id="4c658-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="4c658-114">Konwencje wbudowane</span><span class="sxs-lookup"><span data-stu-id="4c658-114">Built-in conventions</span></span>

<span data-ttu-id="4c658-115">Te kompilują początkowy model na podstawie kształtu klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="4c658-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="4c658-116">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="4c658-116">Fluent API</span></span>

<span data-ttu-id="4c658-117">Umożliwia przesłonięcie metody `OnModelCreating` w kontekście w celu dodatkowego skonfigurowania modelu, który został odnaleziony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="4c658-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="4c658-118">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="4c658-118">Data annotations</span></span>

<span data-ttu-id="4c658-119">Są atrybutami, które mogą być dodawane do klas jednostek/właściwości i mają wpływ na model EF.</span><span class="sxs-lookup"><span data-stu-id="4c658-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="4c658-120">Na przykład dodanie `[Required]` pozwoli na dr znać, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4c658-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="4c658-121">Mapowanie tabeli relacyjnej</span><span class="sxs-lookup"><span data-stu-id="4c658-121">Relational Table mapping</span></span>

<span data-ttu-id="4c658-122">Zezwala na mapowanie jednostek na tabele/kolumny.</span><span class="sxs-lookup"><span data-stu-id="4c658-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="4c658-123">Generowanie wartości klucza</span><span class="sxs-lookup"><span data-stu-id="4c658-123">Key value generation</span></span>

<span data-ttu-id="4c658-124">Obejmuje to generowanie po stronie klienta i generowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="4c658-125">Wartości wygenerowane przez bazę danych</span><span class="sxs-lookup"><span data-stu-id="4c658-125">Database generated values</span></span>

<span data-ttu-id="4c658-126">Umożliwia generowanie wartości przez bazę danych przy wstawianiu (wartości domyślne) lub aktualizacji (kolumny obliczane).</span><span class="sxs-lookup"><span data-stu-id="4c658-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="4c658-127">Sekwencje w SQL Server</span><span class="sxs-lookup"><span data-stu-id="4c658-127">Sequences in SQL Server</span></span>

<span data-ttu-id="4c658-128">Umożliwia zdefiniowanie obiektów sekwencji w modelu.</span><span class="sxs-lookup"><span data-stu-id="4c658-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="4c658-129">Ograniczenia UNIQUE</span><span class="sxs-lookup"><span data-stu-id="4c658-129">Unique constraints</span></span>

<span data-ttu-id="4c658-130">Umożliwia definiowanie kluczy alternatywnych i możliwość definiowania relacji przeznaczonych dla tego klucza.</span><span class="sxs-lookup"><span data-stu-id="4c658-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="4c658-131">Zwiększa</span><span class="sxs-lookup"><span data-stu-id="4c658-131">Indexes</span></span>

<span data-ttu-id="4c658-132">Zdefiniowanie indeksów w modelu powoduje automatyczne wprowadzenie indeksów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="4c658-133">Obsługiwane są również unikatowe indeksy.</span><span class="sxs-lookup"><span data-stu-id="4c658-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="4c658-134">Właściwości stanu cienia</span><span class="sxs-lookup"><span data-stu-id="4c658-134">Shadow state properties</span></span>

<span data-ttu-id="4c658-135">Umożliwia zdefiniowanie właściwości w modelu, które nie są zadeklarowane i nie są przechowywane w klasie .NET, ale mogą być śledzone i aktualizowane przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c658-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="4c658-136">Często używane w przypadku właściwości klucza obcego podczas ujawniania ich w obiekcie nie jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="4c658-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="4c658-137">Wzorzec dziedziczenia na poziomie tabeli</span><span class="sxs-lookup"><span data-stu-id="4c658-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="4c658-138">Zezwala, aby jednostki w hierarchii dziedziczenia były zapisywane w pojedynczej tabeli przy użyciu kolumny rozróżniacza, aby identyfikować typy jednostek dla danego rekordu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="4c658-139">Walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="4c658-139">Model validation</span></span>

<span data-ttu-id="4c658-140">Wykrywa nieprawidłowe wzorce w modelu i udostępnia przydatne komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="4c658-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="4c658-141">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="4c658-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="4c658-142">Śledzenie zmian migawek</span><span class="sxs-lookup"><span data-stu-id="4c658-142">Snapshot change tracking</span></span>

<span data-ttu-id="4c658-143">Zezwala na automatyczne wykrywanie zmian w jednostkach, porównując bieżący stan z kopią (migawką) oryginalnego stanu.</span><span class="sxs-lookup"><span data-stu-id="4c658-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="4c658-144">Śledzenie zmian powiadomień</span><span class="sxs-lookup"><span data-stu-id="4c658-144">Notification change tracking</span></span>

<span data-ttu-id="4c658-145">Umożliwia jednostkom powiadamianie o śledzeniu zmian, gdy wartości właściwości są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="4c658-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="4c658-146">Uzyskiwanie dostępu do śledzonego stanu</span><span class="sxs-lookup"><span data-stu-id="4c658-146">Accessing tracked state</span></span>

<span data-ttu-id="4c658-147">Za pośrednictwem `DbContext.Entry` i `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="4c658-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="4c658-148">Dołączanie odłączonych jednostek/wykresów</span><span class="sxs-lookup"><span data-stu-id="4c658-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="4c658-149">Nowy interfejs API `DbContext.AttachGraph` pomaga ponownie dołączyć jednostki do kontekstu w celu zapisania nowych/zmodyfikowanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4c658-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="4c658-150">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="4c658-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="4c658-151">Podstawowe funkcje zapisywania</span><span class="sxs-lookup"><span data-stu-id="4c658-151">Basic save functionality</span></span>

<span data-ttu-id="4c658-152">Zezwala na utrwalanie zmian wystąpień jednostek w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="4c658-153">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="4c658-153">Optimistic Concurrency</span></span>

<span data-ttu-id="4c658-154">Chroni przed zastąpieniem zmian wprowadzonych przez innego użytkownika od momentu pobrania danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="4c658-155">Metody SaveChanges Async</span><span class="sxs-lookup"><span data-stu-id="4c658-155">Async SaveChanges</span></span>

<span data-ttu-id="4c658-156">Może zwolnić bieżący wątek, aby przetwarzać inne żądania, podczas gdy baza danych przetwarza polecenia wydane z `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4c658-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="4c658-157">Transakcje bazy danych</span><span class="sxs-lookup"><span data-stu-id="4c658-157">Database Transactions</span></span>

<span data-ttu-id="4c658-158">Oznacza, że `SaveChanges` jest zawsze niepodzielna (oznacza to, że całkowicie kończy się powodzeniem lub nie wprowadzono żadnych zmian w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="4c658-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="4c658-159">Istnieją również interfejsy API powiązane z transakcjami umożliwiające udostępnianie transakcji między wystąpieniami kontekstu itd.</span><span class="sxs-lookup"><span data-stu-id="4c658-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="4c658-160">Relacyjne: przetwarzanie wsadowe instrukcji</span><span class="sxs-lookup"><span data-stu-id="4c658-160">Relational: Batching of statements</span></span>

<span data-ttu-id="4c658-161">Zapewnia lepszą wydajność dzięki zbiorom wsadowym wielokrotnego wstawiania/aktualizowania/usuwania w jednej komunikacji jednokierunkowej z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="4c658-162">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="4c658-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="4c658-163">Podstawowa obsługa LINQ</span><span class="sxs-lookup"><span data-stu-id="4c658-163">Basic LINQ support</span></span>

<span data-ttu-id="4c658-164">Umożliwia korzystanie z programu LINQ do pobierania danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="4c658-165">Mieszane Obliczanie klienta/serwera</span><span class="sxs-lookup"><span data-stu-id="4c658-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="4c658-166">Włącza zapytania, które mają zawierać logikę, której nie można oszacować w bazie danych i dlatego muszą być oceniane po pobraniu danych do pamięci.</span><span class="sxs-lookup"><span data-stu-id="4c658-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="4c658-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="4c658-167">NoTracking</span></span>

<span data-ttu-id="4c658-168">Zapytania umożliwiają szybsze wykonywanie zapytań, gdy kontekst nie musi monitorować zmian w wystąpieniach jednostek (jest to przydatne, jeśli wyniki są tylko do odczytu).</span><span class="sxs-lookup"><span data-stu-id="4c658-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="4c658-169">Ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="4c658-169">Eager loading</span></span>

<span data-ttu-id="4c658-170">Udostępnia metody `Include` i `ThenInclude` do identyfikowania powiązanych danych, które powinny być również pobierane podczas wykonywania zapytań.</span><span class="sxs-lookup"><span data-stu-id="4c658-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="4c658-171">Zapytanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="4c658-171">Async query</span></span>

<span data-ttu-id="4c658-172">Może zwolnić bieżący wątek (i skojarzone zasoby), aby przetwarzać inne żądania, gdy baza danych przetwarza zapytanie.</span><span class="sxs-lookup"><span data-stu-id="4c658-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="4c658-173">Nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="4c658-173">Raw SQL queries</span></span>

<span data-ttu-id="4c658-174">Udostępnia metodę `DbSet.FromSql` do pobierania danych przy użyciu nieprzetworzonych zapytań SQL.</span><span class="sxs-lookup"><span data-stu-id="4c658-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="4c658-175">Te zapytania mogą również składać się z użycia LINQ.</span><span class="sxs-lookup"><span data-stu-id="4c658-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="4c658-176">Zarządzanie schematami bazy danych</span><span class="sxs-lookup"><span data-stu-id="4c658-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="4c658-177">Interfejsy API tworzenia/usuwania bazy danych</span><span class="sxs-lookup"><span data-stu-id="4c658-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="4c658-178">Są głównie przeznaczone do testowania, gdzie chcesz szybko utworzyć/usunąć bazę danych bez użycia migracji.</span><span class="sxs-lookup"><span data-stu-id="4c658-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="4c658-179">Migracje relacyjnych baz danych</span><span class="sxs-lookup"><span data-stu-id="4c658-179">Relational database migrations</span></span>

<span data-ttu-id="4c658-180">Zezwól schematowi relacyjnej bazy danych na rozwijanie nadgodzin w miarę zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="4c658-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="4c658-181">Odtwarzanie z bazy danych</span><span class="sxs-lookup"><span data-stu-id="4c658-181">Reverse engineer from database</span></span>

<span data-ttu-id="4c658-182">Szkieletuje model EF oparty na istniejącym schemacie relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="4c658-183">Dostawcy baz danych</span><span class="sxs-lookup"><span data-stu-id="4c658-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="4c658-184">Serwer SQL</span><span class="sxs-lookup"><span data-stu-id="4c658-184">SQL Server</span></span>

<span data-ttu-id="4c658-185">Nawiązuje połączenie z Microsoft SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="4c658-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="4c658-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="4c658-186">SQLite</span></span>

<span data-ttu-id="4c658-187">Nawiązuje połączenie z bazą danych programu SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="4c658-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="4c658-188">W pamięci</span><span class="sxs-lookup"><span data-stu-id="4c658-188">In-Memory</span></span>

<span data-ttu-id="4c658-189">Program został zaprojektowany tak, aby można było łatwo włączyć testowanie bez łączenia się z rzeczywistą bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="4c658-190">dostawcy innych firm</span><span class="sxs-lookup"><span data-stu-id="4c658-190">3rd party providers</span></span>

<span data-ttu-id="4c658-191">Kilku dostawców jest dostępnych dla innych aparatów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c658-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="4c658-192">Aby uzyskać pełną listę, zobacz [dostawcy bazy danych](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="4c658-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
