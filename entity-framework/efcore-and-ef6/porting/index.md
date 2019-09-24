---
title: Przenoszenie z programu EF6 do programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198029"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="dd551-102">Przenoszenie z programu EF6 do programu EF Core</span><span class="sxs-lookup"><span data-stu-id="dd551-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="dd551-103">Ze względu na podstawowe zmiany w EF Core nie zalecamy próby przeniesienia aplikacji EF6 do EF Core, chyba że masz istotny powód wprowadzenia zmiany.</span><span class="sxs-lookup"><span data-stu-id="dd551-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="dd551-104">Należy wyświetlić przechodzenie z EF6 do EF Core jako port zamiast uaktualnienia.</span><span class="sxs-lookup"><span data-stu-id="dd551-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dd551-105">Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy EF Core spełnia wymagania dotyczące dostępu do danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dd551-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="dd551-106">Brakujące funkcje</span><span class="sxs-lookup"><span data-stu-id="dd551-106">Missing features</span></span>

<span data-ttu-id="dd551-107">Upewnij się, że EF Core ma wszystkie funkcje, których potrzebujesz użyć w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dd551-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="dd551-108">Zobacz [porównanie funkcji](xref:efcore-and-ef6/index) , aby uzyskać szczegółowe informacje na temat sposobu, w jaki zestaw funkcji w EF Core jest porównywany z Ef6.</span><span class="sxs-lookup"><span data-stu-id="dd551-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="dd551-109">Jeśli brakuje wymaganych funkcji, upewnij się, że można wyrównać brak tych funkcji przed rozpoczęciem przenoszenia do EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd551-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="dd551-110">Zmiany zachowania</span><span class="sxs-lookup"><span data-stu-id="dd551-110">Behavior changes</span></span>

<span data-ttu-id="dd551-111">To nie jest wyczerpująca lista niektórych zmian w zachowaniu między EF6 i EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd551-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="dd551-112">Ważne jest, aby zachować te kwestie jako portów, które mogą zmienić sposób działania aplikacji, ale nie będą wyświetlane jako błędy kompilacji po zamianie na EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd551-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="dd551-113">Nieogólnymi. Add/Attach i Graph Behavior</span><span class="sxs-lookup"><span data-stu-id="dd551-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="dd551-114">W Ef6 wywoływanie `DbSet.Add()` jednostki powoduje cykliczne wyszukiwanie wszystkich jednostek, do których odwołuje się we właściwościach nawigacji.</span><span class="sxs-lookup"><span data-stu-id="dd551-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="dd551-115">Wszystkie znalezione jednostki i nie są już śledzone przez kontekst, również są oznaczone jako dodane.</span><span class="sxs-lookup"><span data-stu-id="dd551-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="dd551-116">`DbSet.Attach()`zachowuje się tak samo, chyba że wszystkie jednostki są oznaczone jako niezmienione.</span><span class="sxs-lookup"><span data-stu-id="dd551-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="dd551-117">**EF Core wykonuje podobne wyszukiwanie cykliczne, ale przy niewielkich różnych regułach.**</span><span class="sxs-lookup"><span data-stu-id="dd551-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="dd551-118">Jednostka główna jest zawsze w żądanym stanie (dodana do `DbSet.Add` i niezmieniona dla `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="dd551-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="dd551-119">**Dla jednostek znalezionych podczas cyklicznego wyszukiwania właściwości nawigacji:**</span><span class="sxs-lookup"><span data-stu-id="dd551-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="dd551-120">**Jeśli klucz podstawowy jednostki jest generowany przez magazyn**</span><span class="sxs-lookup"><span data-stu-id="dd551-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="dd551-121">Jeśli klucz podstawowy nie jest ustawiony na wartość, stan jest ustawiany na dodane.</span><span class="sxs-lookup"><span data-stu-id="dd551-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="dd551-122">Wartość klucza podstawowego jest uznawana za "nie ustawiono", jeśli jest przypisana wartość domyślna CLR dla typu właściwości ( `0` na przykład `null` dla `int` `string`, dla, itp.).</span><span class="sxs-lookup"><span data-stu-id="dd551-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="dd551-123">Jeśli klucz podstawowy ma ustawioną wartość, stan jest ustawiony na niezmienione.</span><span class="sxs-lookup"><span data-stu-id="dd551-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="dd551-124">Jeśli klucz podstawowy nie jest generowany w bazie danych, jednostka zostanie umieszczona w tym samym stanie co główny.</span><span class="sxs-lookup"><span data-stu-id="dd551-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="dd551-125">Code First inicjalizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="dd551-125">Code First database initialization</span></span>

<span data-ttu-id="dd551-126">**EF6 ma znaczną ilość Magic, która wykonuje wokół wyboru połączenia bazy danych i zainicjowania bazy danych. Niektóre z tych reguł obejmują:**</span><span class="sxs-lookup"><span data-stu-id="dd551-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="dd551-127">Jeśli konfiguracja nie zostanie wykonana, EF6 wybierze bazę danych w programie SQL Express lub LocalDb.</span><span class="sxs-lookup"><span data-stu-id="dd551-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="dd551-128">Jeśli parametr połączenia o takiej samej nazwie jak kontekst znajduje się w pliku aplikacji `App/Web.config` , zostanie użyte połączenie.</span><span class="sxs-lookup"><span data-stu-id="dd551-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="dd551-129">Jeśli baza danych nie istnieje, zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="dd551-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="dd551-130">Jeśli żadna z tabel z modelu nie istnieje w bazie danych, schemat dla bieżącego modelu zostanie dodany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dd551-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="dd551-131">Jeśli migracja jest włączona, są one używane do tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dd551-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="dd551-132">Jeśli baza danych istnieje i EF6 wcześniej utworzyła schemat, schemat jest sprawdzany pod kątem zgodności z bieżącym modelem.</span><span class="sxs-lookup"><span data-stu-id="dd551-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="dd551-133">Wyjątek jest zgłaszany, jeśli model został zmieniony od czasu utworzenia schematu.</span><span class="sxs-lookup"><span data-stu-id="dd551-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="dd551-134">**EF Core nie wykonuje żadnego z tych magicznych.**</span><span class="sxs-lookup"><span data-stu-id="dd551-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="dd551-135">Połączenie z bazą danych musi być jawnie skonfigurowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="dd551-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="dd551-136">Nie są wykonywane żadne inicjalizacje.</span><span class="sxs-lookup"><span data-stu-id="dd551-136">No initialization is performed.</span></span> <span data-ttu-id="dd551-137">Należy użyć `DbContext.Database.Migrate()` , aby zastosować migracje (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` utworzyć/usunąć bazę danych bez użycia migracji).</span><span class="sxs-lookup"><span data-stu-id="dd551-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="dd551-138">Konwencja nazewnictwa tabel Code First</span><span class="sxs-lookup"><span data-stu-id="dd551-138">Code First table naming convention</span></span>

<span data-ttu-id="dd551-139">EF6 uruchamia nazwę klasy jednostki za pomocą usługi pluralizacja, aby obliczyć domyślną nazwę tabeli, do której jednostka jest zamapowana.</span><span class="sxs-lookup"><span data-stu-id="dd551-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="dd551-140">EF Core używa nazwy `DbSet` właściwości, która jest widoczna dla jednostki w kontekście pochodnym.</span><span class="sxs-lookup"><span data-stu-id="dd551-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="dd551-141">Jeśli jednostka nie ma `DbSet` właściwości, zostanie użyta nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="dd551-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
