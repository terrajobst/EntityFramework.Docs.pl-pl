---
title: Co nowego w EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417525"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="f436c-102">Funkcje zawarte w EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="f436c-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="f436c-103">Platformy</span><span class="sxs-lookup"><span data-stu-id="f436c-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="f436c-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="f436c-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="f436c-105">Zawiera konsolę, WPF, WinForms, ASP.NET 4 itp.</span><span class="sxs-lookup"><span data-stu-id="f436c-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="f436c-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="f436c-106">.NET Standard 1.3</span></span>

<span data-ttu-id="f436c-107">W tym ASP.NET Core kierowania zarówno .NET Framework i .NET Core w systemie Windows, OSX i Linux.</span><span class="sxs-lookup"><span data-stu-id="f436c-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="f436c-108">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="f436c-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="f436c-109">Modelowanie podstawowe</span><span class="sxs-lookup"><span data-stu-id="f436c-109">Basic modelling</span></span>

<span data-ttu-id="f436c-110">Na podstawie jednostek POCO z właściwościami get/set`int` `string`typowych typów skalarnych ( , itp.).</span><span class="sxs-lookup"><span data-stu-id="f436c-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="f436c-111">Relacje i właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="f436c-111">Relationships and navigation properties</span></span>

<span data-ttu-id="f436c-112">Relacje jeden do wielu i jeden do zera lub jeden można określić w modelu na podstawie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="f436c-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="f436c-113">Właściwości nawigacji typów kolekcji prostej lub odwołania mogą być skojarzone z tymi relacjami.</span><span class="sxs-lookup"><span data-stu-id="f436c-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="f436c-114">Wbudowane konwencje</span><span class="sxs-lookup"><span data-stu-id="f436c-114">Built-in conventions</span></span>

<span data-ttu-id="f436c-115">Te kompilacji modelu początkowego na podstawie kształtu klas jednostki.</span><span class="sxs-lookup"><span data-stu-id="f436c-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="f436c-116">Płynne API</span><span class="sxs-lookup"><span data-stu-id="f436c-116">Fluent API</span></span>

<span data-ttu-id="f436c-117">Umożliwia zastąpienie `OnModelCreating` metody w kontekście, aby dodatkowo skonfigurować model, który został wykryty przez konwencję.</span><span class="sxs-lookup"><span data-stu-id="f436c-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="f436c-118">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f436c-118">Data annotations</span></span>

<span data-ttu-id="f436c-119">Są atrybuty, które mogą być dodawane do klasy/właściwości jednostki i będzie mieć wpływ na model EF.</span><span class="sxs-lookup"><span data-stu-id="f436c-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="f436c-120">Na przykład `[Required]` dodanie poinformuje EF, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="f436c-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="f436c-121">Mapowanie tabeli relacyjnej</span><span class="sxs-lookup"><span data-stu-id="f436c-121">Relational Table mapping</span></span>

<span data-ttu-id="f436c-122">Umożliwia mapowanie elementów do tabel/kolumn.</span><span class="sxs-lookup"><span data-stu-id="f436c-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="f436c-123">Generowanie kluczowych wartości</span><span class="sxs-lookup"><span data-stu-id="f436c-123">Key value generation</span></span>

<span data-ttu-id="f436c-124">W tym generowanie po stronie klienta i generowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="f436c-125">Wartości wygenerowane przez bazę danych</span><span class="sxs-lookup"><span data-stu-id="f436c-125">Database generated values</span></span>

<span data-ttu-id="f436c-126">Umożliwia generowanie wartości przez bazę danych przy wstawiania (wartości domyślne) lub aktualizacji (kolumny obliczane).</span><span class="sxs-lookup"><span data-stu-id="f436c-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="f436c-127">Sekwencje w programie SQL Server</span><span class="sxs-lookup"><span data-stu-id="f436c-127">Sequences in SQL Server</span></span>

<span data-ttu-id="f436c-128">Umożliwia definiowanie obiektów sekwencji w modelu.</span><span class="sxs-lookup"><span data-stu-id="f436c-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="f436c-129">Unique</span><span class="sxs-lookup"><span data-stu-id="f436c-129">Unique constraints</span></span>

<span data-ttu-id="f436c-130">Umożliwia definicję kluczy alternatywnych i możliwość definiowania relacji, które są przeznaczone dla tego klucza.</span><span class="sxs-lookup"><span data-stu-id="f436c-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="f436c-131">Indeksy</span><span class="sxs-lookup"><span data-stu-id="f436c-131">Indexes</span></span>

<span data-ttu-id="f436c-132">Definiowanie indeksów w modelu automatycznie wprowadza indeksy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="f436c-133">Obsługiwane są również unikatowe indeksy.</span><span class="sxs-lookup"><span data-stu-id="f436c-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="f436c-134">Właściwości stanu cienia</span><span class="sxs-lookup"><span data-stu-id="f436c-134">Shadow state properties</span></span>

<span data-ttu-id="f436c-135">Umożliwia zdefiniowanie właściwości w modelu, które nie są zadeklarowane i nie są przechowywane w klasie .NET, ale mogą być śledzone i aktualizowane przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="f436c-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="f436c-136">Często używane dla właściwości klucza obcego podczas wystawiania ich w obiekcie nie jest pożądane.</span><span class="sxs-lookup"><span data-stu-id="f436c-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="f436c-137">Wzorzec dziedziczenia tabela na hierarchię</span><span class="sxs-lookup"><span data-stu-id="f436c-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="f436c-138">Umożliwia zapisywanie jednostek w hierarchii dziedziczenia w jednej tabeli przy użyciu kolumny rozróżniacza w celu zidentyfikowania ich typu jednostki dla danego rekordu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="f436c-139">Walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="f436c-139">Model validation</span></span>

<span data-ttu-id="f436c-140">Wykrywa nieprawidłowe wzorce w modelu i udostępnia przydatne komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="f436c-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="f436c-141">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="f436c-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="f436c-142">Śledzenie zmian migawek</span><span class="sxs-lookup"><span data-stu-id="f436c-142">Snapshot change tracking</span></span>

<span data-ttu-id="f436c-143">Umożliwia automatyczne wykrywanie zmian w jednostkach przez porównanie bieżącego stanu z kopią (migawką) stanu oryginalnego.</span><span class="sxs-lookup"><span data-stu-id="f436c-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="f436c-144">Śledzenie zmian powiadomień</span><span class="sxs-lookup"><span data-stu-id="f436c-144">Notification change tracking</span></span>

<span data-ttu-id="f436c-145">Umożliwia jednostkom powiadamianie śledzenia zmian, gdy wartości właściwości są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="f436c-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="f436c-146">Uzyskiwanie dostępu do śledzonego stanu</span><span class="sxs-lookup"><span data-stu-id="f436c-146">Accessing tracked state</span></span>

<span data-ttu-id="f436c-147">Przez `DbContext.Entry` `DbContext.ChangeTracker`i .</span><span class="sxs-lookup"><span data-stu-id="f436c-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="f436c-148">Dołączanie odłączonych elementów/wykresów</span><span class="sxs-lookup"><span data-stu-id="f436c-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="f436c-149">Nowy `DbContext.AttachGraph` interfejs API pomaga ponownie dołączyć jednostki do kontekstu w celu zapisania nowych/zmodyfikowanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="f436c-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="f436c-150">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="f436c-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="f436c-151">Podstawowa funkcja zapisywania</span><span class="sxs-lookup"><span data-stu-id="f436c-151">Basic save functionality</span></span>

<span data-ttu-id="f436c-152">Umożliwia utrwalenie zmian w wystąpieniach encji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="f436c-153">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="f436c-153">Optimistic Concurrency</span></span>

<span data-ttu-id="f436c-154">Chroni przed zastąpieniem zmian wprowadzonych przez innego użytkownika, ponieważ dane zostały pobrane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="f436c-155">Async SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f436c-155">Async SaveChanges</span></span>

<span data-ttu-id="f436c-156">Można zwolnić bieżący wątek do przetwarzania innych żądań, `SaveChanges`podczas gdy baza danych przetwarza polecenia wydane z .</span><span class="sxs-lookup"><span data-stu-id="f436c-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="f436c-157">Transakcje bazy danych</span><span class="sxs-lookup"><span data-stu-id="f436c-157">Database Transactions</span></span>

<span data-ttu-id="f436c-158">Oznacza, `SaveChanges` że jest zawsze niepodzielny (co oznacza, że albo całkowicie powiedzie się lub nie wprowadza się żadnych zmian w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="f436c-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="f436c-159">Istnieją również interfejsy API związane z transakcjami, aby umożliwić udostępnianie transakcji między wystąpieniami kontekstu itp.</span><span class="sxs-lookup"><span data-stu-id="f436c-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="f436c-160">Relacyjne: Wsadowanie instrukcji</span><span class="sxs-lookup"><span data-stu-id="f436c-160">Relational: Batching of statements</span></span>

<span data-ttu-id="f436c-161">Zapewnia lepszą wydajność przez wsadowanie wiele wstawiania/aktualizacji / usuwania poleceń w jednym obie strony do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="f436c-162">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="f436c-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="f436c-163">Podstawowa obsługa LINQ</span><span class="sxs-lookup"><span data-stu-id="f436c-163">Basic LINQ support</span></span>

<span data-ttu-id="f436c-164">Zapewnia możliwość korzystania z LINQ do pobierania danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="f436c-165">Ocena klienta/serwera mieszanego</span><span class="sxs-lookup"><span data-stu-id="f436c-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="f436c-166">Umożliwia kwerendy zawierają logikę, która nie może być oceniana w bazie danych i dlatego muszą być oceniane po pobraniu danych do pamięci.</span><span class="sxs-lookup"><span data-stu-id="f436c-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="f436c-167">Notracking</span><span class="sxs-lookup"><span data-stu-id="f436c-167">NoTracking</span></span>

<span data-ttu-id="f436c-168">Kwerendy umożliwia szybsze wykonywanie zapytań, gdy kontekst nie musi monitorować zmian w wystąpieniach jednostki (jest to przydatne, jeśli wyniki są tylko do odczytu).</span><span class="sxs-lookup"><span data-stu-id="f436c-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="f436c-169">Gorliwy załadunek</span><span class="sxs-lookup"><span data-stu-id="f436c-169">Eager loading</span></span>

<span data-ttu-id="f436c-170">Zawiera `Include` metody `ThenInclude` i metody identyfikowania powiązanych danych, które również powinny być pobierane podczas wykonywania zapytań.</span><span class="sxs-lookup"><span data-stu-id="f436c-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="f436c-171">Zapytanie asynchroniowe</span><span class="sxs-lookup"><span data-stu-id="f436c-171">Async query</span></span>

<span data-ttu-id="f436c-172">Można zwolnić bieżący wątek (i jest skojarzone zasoby) do przetwarzania innych żądań, podczas gdy baza danych przetwarza kwerendę.</span><span class="sxs-lookup"><span data-stu-id="f436c-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="f436c-173">Nieprzetworzone zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="f436c-173">Raw SQL queries</span></span>

<span data-ttu-id="f436c-174">Udostępnia `DbSet.FromSql` metodę używania nieprzetworzonych zapytań SQL do pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="f436c-175">Te zapytania mogą również składać się przy użyciu LINQ.</span><span class="sxs-lookup"><span data-stu-id="f436c-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="f436c-176">Zarządzanie schematem bazy danych</span><span class="sxs-lookup"><span data-stu-id="f436c-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="f436c-177">Interfejsy API tworzenia/usuwania bazy danych</span><span class="sxs-lookup"><span data-stu-id="f436c-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="f436c-178">Są przeznaczone głównie do testowania, gdzie chcesz szybko utworzyć/usunąć bazę danych bez użycia migracji.</span><span class="sxs-lookup"><span data-stu-id="f436c-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="f436c-179">Migracje relacyjnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="f436c-179">Relational database migrations</span></span>

<span data-ttu-id="f436c-180">Zezwalaj na schemat relacyjnej bazy danych, aby ewoluować nadgodziny w miarę zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="f436c-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="f436c-181">Inżynier odwrotny z bazy danych</span><span class="sxs-lookup"><span data-stu-id="f436c-181">Reverse engineer from database</span></span>

<span data-ttu-id="f436c-182">Szkielety modelu EF na podstawie istniejącego schematu relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="f436c-183">Dostawcy baz danych</span><span class="sxs-lookup"><span data-stu-id="f436c-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="f436c-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="f436c-184">SQL Server</span></span>

<span data-ttu-id="f436c-185">Łączy się z programem Microsoft SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="f436c-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="f436c-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="f436c-186">SQLite</span></span>

<span data-ttu-id="f436c-187">Łączy się z bazą danych SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="f436c-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="f436c-188">W pamięci</span><span class="sxs-lookup"><span data-stu-id="f436c-188">In-Memory</span></span>

<span data-ttu-id="f436c-189">Został zaprojektowany, aby łatwo włączyć testowanie bez łączenia się z prawdziwą bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="f436c-190">Dostawcy zewnętrzni</span><span class="sxs-lookup"><span data-stu-id="f436c-190">3rd party providers</span></span>

<span data-ttu-id="f436c-191">Kilku dostawców są dostępne dla innych aparatów baz danych.</span><span class="sxs-lookup"><span data-stu-id="f436c-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="f436c-192">Zobacz [dostawców baz danych,](../providers/index.md) aby uzyskać pełną listę.</span><span class="sxs-lookup"><span data-stu-id="f436c-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
