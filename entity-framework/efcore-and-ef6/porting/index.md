---
title: Przenoszenie z EF6 do EF Core - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416951"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="1ef7c-102">Przenoszenie z programu EF6 do programu EF Core</span><span class="sxs-lookup"><span data-stu-id="1ef7c-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="1ef7c-103">Ze względu na podstawowe zmiany w EF Core nie zaleca się próby przeniesienia aplikacji EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="1ef7c-104">Należy wyświetlić przejście z EF6 do EF Core jako port, a nie uaktualnienie.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1ef7c-105">Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy ef core spełnia wymagania dotyczące dostępu do danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="1ef7c-106">Brakujące funkcje</span><span class="sxs-lookup"><span data-stu-id="1ef7c-106">Missing features</span></span>

<span data-ttu-id="1ef7c-107">Upewnij się, że EF Core ma wszystkie funkcje, które należy użyć w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="1ef7c-108">Zobacz [porównanie funkcji, aby](xref:efcore-and-ef6/index) uzyskać szczegółowe porównanie sposobu porównywania funkcji w ef core z EF6.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="1ef7c-109">Jeśli brakuje żadnych wymaganych funkcji, upewnij się, że można zrekompensować brak tych funkcji przed przeniesieniem do EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="1ef7c-110">Zmiany w zachowaniu</span><span class="sxs-lookup"><span data-stu-id="1ef7c-110">Behavior changes</span></span>

<span data-ttu-id="1ef7c-111">Jest to niewyczerpywna lista niektórych zmian w zachowaniu między EF6 i EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="1ef7c-112">Ważne jest, aby zachować te na uwadze jako portu aplikacji, ponieważ mogą one zmienić sposób zachowania aplikacji, ale nie pojawi się jako błędy kompilacji po zamianie do EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="1ef7c-113">DbSet.Add/Attach i zachowanie wykresu</span><span class="sxs-lookup"><span data-stu-id="1ef7c-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="1ef7c-114">W EF6 `DbSet.Add()` wywołanie jednostki powoduje cykliczne wyszukiwanie dla wszystkich jednostek, do których odwołuje się jego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="1ef7c-115">Wszystkie jednostki, które zostały znalezione i nie są już śledzone przez kontekst, są również oznaczone jako dodane.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="1ef7c-116">`DbSet.Attach()`zachowuje się tak samo, z wyjątkiem wszystkich jednostek są oznaczone jako niezmienione.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="1ef7c-117">**EF Core wykonuje podobne wyszukiwanie cykliczne, ale z niektórymi nieco innymi regułami.**</span><span class="sxs-lookup"><span data-stu-id="1ef7c-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="1ef7c-118">Jednostka główna jest zawsze w żądanym `DbSet.Add` stanie (dodane `DbSet.Attach`dla i bez zmian dla ).</span><span class="sxs-lookup"><span data-stu-id="1ef7c-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="1ef7c-119">**Dla jednostek, które znajdują się podczas wyszukiwania cyklicznego właściwości nawigacji:**</span><span class="sxs-lookup"><span data-stu-id="1ef7c-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="1ef7c-120">**Jeśli klucz podstawowy jednostki jest magazynowy generowany**</span><span class="sxs-lookup"><span data-stu-id="1ef7c-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="1ef7c-121">Jeśli klucz podstawowy nie jest ustawiony na wartość, stan jest ustawiony na dodane.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="1ef7c-122">Wartość klucza podstawowego jest uważana za "nie ustawioną", jeśli jest przypisana `0` `int`wartość `null` `string`domyślna CLR dla typu właściwości (na przykład dla , for , itp.).</span><span class="sxs-lookup"><span data-stu-id="1ef7c-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="1ef7c-123">Jeśli klucz podstawowy jest ustawiony na wartość, stan jest ustawiony na niezmienionym.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="1ef7c-124">Jeśli klucz podstawowy nie jest generowany przez bazę danych, jednostka jest umieszczana w tym samym stanie co katalog główny.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="1ef7c-125">Inicjowanie bazy danych Code First</span><span class="sxs-lookup"><span data-stu-id="1ef7c-125">Code First database initialization</span></span>

<span data-ttu-id="1ef7c-126">**EF6 ma znaczną ilość magii wykonuje wokół wybierania połączenia bazy danych i inicjowania bazy danych. Niektóre z tych zasad obejmują:**</span><span class="sxs-lookup"><span data-stu-id="1ef7c-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="1ef7c-127">Jeśli nie zostanie wykonana żadna konfiguracja, EF6 wybierze bazę danych w programie SQL Express lub LocalDb.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="1ef7c-128">Jeśli ciąg połączenia o takiej samej nazwie `App/Web.config` jak kontekst znajduje się w pliku aplikacji, to połączenie będzie używane.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="1ef7c-129">Jeśli baza danych nie istnieje, jest tworzona.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="1ef7c-130">Jeśli żadna z tabel z modelu istnieje w bazie danych, schemat dla bieżącego modelu jest dodawany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="1ef7c-131">Jeśli migracje są włączone, są one używane do tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="1ef7c-132">Jeśli baza danych istnieje i EF6 wcześniej utworzył schemat, schemat jest sprawdzany pod kątem zgodności z bieżącym modelem.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="1ef7c-133">Wyjątek jest zgłaszany, jeśli model uległ zmianie od czasu utworzenia schematu.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="1ef7c-134">**EF Core nie wykonuje żadnej z tej magii.**</span><span class="sxs-lookup"><span data-stu-id="1ef7c-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="1ef7c-135">Połączenie z bazą danych musi być jawnie skonfigurowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="1ef7c-136">Nie jest wykonywana inicjalizacja.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-136">No initialization is performed.</span></span> <span data-ttu-id="1ef7c-137">Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji `DbContext.Database.EnsureCreated()` `EnsureDeleted()` (lub i utworzyć/usunąć bazę danych bez użycia migracji).</span><span class="sxs-lookup"><span data-stu-id="1ef7c-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="1ef7c-138">Konwencja nazewnictwa pierwszej tabeli kodu</span><span class="sxs-lookup"><span data-stu-id="1ef7c-138">Code First table naming convention</span></span>

<span data-ttu-id="1ef7c-139">EF6 uruchamia nazwę klasy jednostki za pośrednictwem usługi pluralizacji, aby obliczyć domyślną nazwę tabeli, do której jednostka jest mapowana.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="1ef7c-140">EF Core używa nazwy `DbSet` właściwości, która jest widoczna w kontekście pochodnym.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="1ef7c-141">Jeśli jednostka nie ma `DbSet` właściwości, używana jest nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="1ef7c-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
