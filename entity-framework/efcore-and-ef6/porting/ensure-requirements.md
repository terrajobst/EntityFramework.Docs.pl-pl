---
title: Eksportowanie z EF6 do EF Core - weryfikacji wymagań
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054254"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="d292e-102">Przed eksportowanie z EF6 do EF Core: Sprawdzanie poprawności wymagań aplikacji</span><span class="sxs-lookup"><span data-stu-id="d292e-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="d292e-103">Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy podstawowe EF spełnia wymagania dotyczące dostępu do danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d292e-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="d292e-104">Brak funkcji</span><span class="sxs-lookup"><span data-stu-id="d292e-104">Missing features</span></span>

<span data-ttu-id="d292e-105">Upewnij się, że podstawowe EF ma wszystkich funkcji potrzebnych do użycia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d292e-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="d292e-106">Zobacz [porównanie funkcji](../features.md) szczegółowe porównanie jak porównuje EF6 zestawu w EF podstawowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="d292e-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="d292e-107">Brakuje niektórych wymaganych funkcji, upewnij się, że można zniwelować braku te funkcje przed eksportowanie do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d292e-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="d292e-108">Zmiany sposobu działania</span><span class="sxs-lookup"><span data-stu-id="d292e-108">Behavior changes</span></span>

<span data-ttu-id="d292e-109">Jest niepełna lista pewne zmiany w zachowaniu między EF6 i EF podstawowe.</span><span class="sxs-lookup"><span data-stu-id="d292e-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="d292e-110">Należy zachować te pamiętać jako port aplikacji, ponieważ mogą one ulec zmianie sposób aplikacja działa, ale nie będą widoczne jako błędy kompilacji po wymiany na EF rdzeń.</span><span class="sxs-lookup"><span data-stu-id="d292e-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="d292e-111">Wykres i DbSet.Add/Attach zachowanie</span><span class="sxs-lookup"><span data-stu-id="d292e-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="d292e-112">W EF6 wywoływania `DbSet.Add()` na powoduje cykliczne wyszukiwanie wszystkich jednostek, do którego odwołuje się jego właściwości nawigacji jednostki.</span><span class="sxs-lookup"><span data-stu-id="d292e-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="d292e-113">Wszystkie jednostki, które zostały znalezione i nie są już śledzone przez kontekst, również są oznaczone jako dodane.</span><span class="sxs-lookup"><span data-stu-id="d292e-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="d292e-114">`DbSet.Attach()`zachowuje się tak samo, z wyjątkiem wszystkie jednostki są oznaczone jako bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d292e-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="d292e-115">**EF Core wykonuje podobne Wyszukiwanie cykliczne, ale niektóre nieco inne reguły.**</span><span class="sxs-lookup"><span data-stu-id="d292e-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="d292e-116">Obiekt główny jest zawsze w żądany stan (dodany do `DbSet.Add` bez zmian dla `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="d292e-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="d292e-117">**Dla obiektów, które zostały znalezione podczas cyklicznego wyszukiwania właściwości nawigacji:**</span><span class="sxs-lookup"><span data-stu-id="d292e-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="d292e-118">**W przypadku klucza podstawowego jednostki wygenerowane**</span><span class="sxs-lookup"><span data-stu-id="d292e-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="d292e-119">Jeśli klucz podstawowy nie jest ustawiona na wartość, stan jest ustawiony do dodany.</span><span class="sxs-lookup"><span data-stu-id="d292e-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="d292e-120">Wartość klucza podstawowego jest uznawany za "nie jest ustawiona" przypisane wartości domyślne CLR dla typu właściwości (tj. `0` dla `int`, `null` dla `string`itp.).</span><span class="sxs-lookup"><span data-stu-id="d292e-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="d292e-121">Jeśli klucz podstawowy jest ustawiona na wartość, stan ustawiono bez zmian.</span><span class="sxs-lookup"><span data-stu-id="d292e-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="d292e-122">Jeśli klucz podstawowy nie jest generowany przez bazę danych, jednostka jest umieszczany w tym samym stanie, jako element główny.</span><span class="sxs-lookup"><span data-stu-id="d292e-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="d292e-123">Kod pierwszego inicjowania bazy danych</span><span class="sxs-lookup"><span data-stu-id="d292e-123">Code First database initialization</span></span>

<span data-ttu-id="d292e-124">**EF6 ma dużą magic, którą wykonuje wokół wybranie połączenia z bazą danych i inicjowania bazy danych. Oto niektóre z tych reguł:**</span><span class="sxs-lookup"><span data-stu-id="d292e-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="d292e-125">Jeśli konfiguracja nie jest wykonywana, EF6 wybierze bazy danych programu SQL Express lub LocalDb.</span><span class="sxs-lookup"><span data-stu-id="d292e-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="d292e-126">Jeśli parametry połączenia o tej samej nazwie jako kontekst jest w aplikacjach `App/Web.config` plików, to połączenie będzie używane.</span><span class="sxs-lookup"><span data-stu-id="d292e-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="d292e-127">Jeśli baza danych nie istnieje, jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="d292e-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="d292e-128">Jeśli żadna z tabel z modelu nie istnieją w bazie danych, schematu dla bieżącego modelu jest dodawany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d292e-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="d292e-129">Jeśli włączono migracje, następnie są one używane do utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d292e-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="d292e-130">Jeśli baza danych istnieje i EF6 wcześniej utworzyć schemat, schemat jest sprawdzane pod kątem zgodności z bieżącym modelem.</span><span class="sxs-lookup"><span data-stu-id="d292e-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="d292e-131">Jeśli model został zmieniony od czasu utworzenia schematu, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d292e-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="d292e-132">**Podstawowe EF nie wykonuje tego magic.**</span><span class="sxs-lookup"><span data-stu-id="d292e-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="d292e-133">Połączenie z bazą danych musi być jawnie skonfigurowany w kodzie.</span><span class="sxs-lookup"><span data-stu-id="d292e-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="d292e-134">Inicjowanie nie jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="d292e-134">No initialization is performed.</span></span> <span data-ttu-id="d292e-135">Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` do tworzenia/usuwania bazy danych bez korzystania z migracji).</span><span class="sxs-lookup"><span data-stu-id="d292e-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="d292e-136">Kod konwencji nazewnictwa pierwszej tabeli.</span><span class="sxs-lookup"><span data-stu-id="d292e-136">Code First table naming convention</span></span>

<span data-ttu-id="d292e-137">EF6 uruchamia Nazwa klasy jednostki za pośrednictwem usługi określania liczby mnogiej do obliczenia jednostka jest zamapowana na domyślna nazwa tabeli.</span><span class="sxs-lookup"><span data-stu-id="d292e-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="d292e-138">Podstawowe EF używa nazwy `DbSet` właściwości jednostki jest widoczna w pochodnej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d292e-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="d292e-139">Jeśli taka jednostka nie posiada `DbSet` właściwości, a następnie nazwę klasy jest używany.</span><span class="sxs-lookup"><span data-stu-id="d292e-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
