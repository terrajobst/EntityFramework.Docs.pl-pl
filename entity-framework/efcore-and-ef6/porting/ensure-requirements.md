---
title: Przenoszenie z programu EF6 do programu EF Core — sprawdzanie poprawności wymagań
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993443"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="51cf2-102">Przed przenoszenie z programu EF6 do programu EF Core: Sprawdzanie poprawności wymagań aplikacji</span><span class="sxs-lookup"><span data-stu-id="51cf2-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="51cf2-103">Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy programu EF Core spełnia wymagania dotyczące dostępu do danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51cf2-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="51cf2-104">Brak funkcji</span><span class="sxs-lookup"><span data-stu-id="51cf2-104">Missing features</span></span>

<span data-ttu-id="51cf2-105">Upewnij się, że programu EF Core zawiera wszystkie funkcje potrzebne do użycia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51cf2-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="51cf2-106">Zobacz [porównanie funkcji](../features.md) szczegółowe porównanie porównanie zestaw w wersji EF Core funkcji platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="51cf2-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="51cf2-107">Jeśli brakuje dowolnej wymagane funkcje, upewnij się, że możesz kompensuje braku tych funkcji, przed eksportowanie do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="51cf2-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="51cf2-108">Zmiany zachowania</span><span class="sxs-lookup"><span data-stu-id="51cf2-108">Behavior changes</span></span>

<span data-ttu-id="51cf2-109">To jest niepełna lista zmian w zachowaniu między EF6 i EF Core.</span><span class="sxs-lookup"><span data-stu-id="51cf2-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="51cf2-110">Należy zachować te należy pamiętać, jako port aplikacji zgodnie z ich może zmienić sposób, w jaki aplikacja działa, ale nie będą wyświetlane jako błędy kompilacji po zamianie do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="51cf2-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="51cf2-111">Zachowanie DbSet.Add/Attach i wykres</span><span class="sxs-lookup"><span data-stu-id="51cf2-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="51cf2-112">W EF6 wywołanie `DbSet.Add()` na jednostki skutkuje Wyszukiwanie cykliczne dla wszystkich jednostek, do którego odwołuje się jego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="51cf2-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="51cf2-113">Wszystkie jednostki, które znajdują się i nie są już śledzone przez kontekście, są również oznaczane w miarę dodawania.</span><span class="sxs-lookup"><span data-stu-id="51cf2-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="51cf2-114">`DbSet.Attach()` zachowuje się tak samo, z wyjątkiem wszystkie jednostki są oznaczone jako niezmieniony.</span><span class="sxs-lookup"><span data-stu-id="51cf2-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="51cf2-115">**EF Core wykonuje wyszukiwanie cykliczne podobne, ale niektóre nieco inne reguły.**</span><span class="sxs-lookup"><span data-stu-id="51cf2-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="51cf2-116">Jednostka główna jest zawsze żądany stan (dodane do `DbSet.Add` bez zmian dla `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="51cf2-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="51cf2-117">**W przypadku jednostek, które zostały znalezione w czasie cykliczne wyszukiwanie właściwości nawigacji:**</span><span class="sxs-lookup"><span data-stu-id="51cf2-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="51cf2-118">**Jeśli klucz podstawowy jednostki jest generowany magazynu**</span><span class="sxs-lookup"><span data-stu-id="51cf2-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="51cf2-119">Jeśli nie ustawiono klucza podstawowego z wartością, stan jest ustawiony do dodanych.</span><span class="sxs-lookup"><span data-stu-id="51cf2-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="51cf2-120">Wartość klucza podstawowego jest uznawany za "nie jest ustawiona" Jeśli jest ona przypisana wartość domyślna CLR dla typu właściwości (na przykład `0` dla `int`, `null` dla `string`itp.).</span><span class="sxs-lookup"><span data-stu-id="51cf2-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="51cf2-121">Jeśli klucz podstawowy jest ustawiona na wartość, stan jest ustawiony bez zmian.</span><span class="sxs-lookup"><span data-stu-id="51cf2-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="51cf2-122">Jeśli klucz podstawowy nie jest generowany w bazie danych, jednostka jest umieszczany w tym samym stanie, jako katalog główny.</span><span class="sxs-lookup"><span data-stu-id="51cf2-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="51cf2-123">Kod pierwszy inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="51cf2-123">Code First database initialization</span></span>

<span data-ttu-id="51cf2-124">**EF6 ma znacznej ilości magic, który wykonuje wokół wybranie połączenia z bazą danych i inicjowania bazy danych. Oto niektóre z tych reguł:**</span><span class="sxs-lookup"><span data-stu-id="51cf2-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="51cf2-125">Jeśli konfiguracja nie jest wykonywana, EF6 wybierze bazę danych programu SQL Express lub LocalDb.</span><span class="sxs-lookup"><span data-stu-id="51cf2-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="51cf2-126">Jeśli parametry połączenia z taką samą nazwę jak kontekstu znajduje się w aplikacji `App/Web.config` pliku, to połączenie będzie używane.</span><span class="sxs-lookup"><span data-stu-id="51cf2-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="51cf2-127">Jeśli baza danych nie istnieje, zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="51cf2-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="51cf2-128">Jeśli żadna z tabel z modelu istnieje w bazie danych, schematów dla bieżącego modelu jest dodawany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="51cf2-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="51cf2-129">Jeśli migracja jest włączona, następnie służą one do utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="51cf2-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="51cf2-130">Jeśli baza danych istnieje i EF6 poprzednio utworzono schemat, schemat jest sprawdzane pod kątem zgodności z bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="51cf2-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="51cf2-131">Wyjątek jest generowany, jeśli model został zmieniony od czasu utworzenia schematu.</span><span class="sxs-lookup"><span data-stu-id="51cf2-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="51cf2-132">**EF Core nie wykonuje tego magic.**</span><span class="sxs-lookup"><span data-stu-id="51cf2-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="51cf2-133">Połączenie z bazą danych muszą być jawnie skonfigurowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="51cf2-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="51cf2-134">Inicjowanie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="51cf2-134">No initialization is performed.</span></span> <span data-ttu-id="51cf2-135">Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` do tworzenia/usuwania bazy danych bez używania migracji).</span><span class="sxs-lookup"><span data-stu-id="51cf2-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="51cf2-136">Pierwsza tabela kodu konwencje nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="51cf2-136">Code First table naming convention</span></span>

<span data-ttu-id="51cf2-137">EF6 uruchamia Nazwa klasy jednostki za pośrednictwem usługi pluralizacja do obliczania jednostki jest mapowany na domyślną nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="51cf2-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="51cf2-138">EF Core używa nazwy `DbSet` właściwości jednostki jest widoczna w w kontekście pochodnych.</span><span class="sxs-lookup"><span data-stu-id="51cf2-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="51cf2-139">Jeśli jednostka ma `DbSet` właściwości, a następnie nazwę klasy jest używany.</span><span class="sxs-lookup"><span data-stu-id="51cf2-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
