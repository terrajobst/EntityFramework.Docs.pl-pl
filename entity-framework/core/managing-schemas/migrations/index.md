---
title: Migracje — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996525"
---
<a name="migrations"></a><span data-ttu-id="d40a8-102">Migracje</span><span class="sxs-lookup"><span data-stu-id="d40a8-102">Migrations</span></span>
==========
<span data-ttu-id="d40a8-103">Migracje umożliwiają dotyczą przyrostowe zmiany schematu bazy danych, aby zachować synchronizację z modelu platformy EF Core przy jednoczesnym zachowaniu istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d40a8-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="d40a8-104">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d40a8-104">Creating the database</span></span>
---------------------
<span data-ttu-id="d40a8-105">Po [zdefiniowany model początkowej][1], nadszedł czas, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d40a8-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="d40a8-106">Aby to zrobić, Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="d40a8-107">Zainstaluj [EF Core Tools] [ 2] i uruchom odpowiednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="d40a8-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="d40a8-108">Trzy pliki są dodawane do projektu w obszarze **migracje** katalogu:</span><span class="sxs-lookup"><span data-stu-id="d40a8-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="d40a8-109">**00000000000000_InitialCreate.cs**— plik główny migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="d40a8-110">Zawiera operacje wymagane do zastosowania migracji (w `Up()`) i można go przywrócić (w `Down()`).</span><span class="sxs-lookup"><span data-stu-id="d40a8-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="d40a8-111">**00000000000000_InitialCreate.Designer.cs**— plik metadanych migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="d40a8-112">Zawiera informacje używane przez EF.</span><span class="sxs-lookup"><span data-stu-id="d40a8-112">Contains information used by EF.</span></span>
* <span data-ttu-id="d40a8-113">**MyContextModelSnapshot.cs**--migawkę bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="d40a8-114">Używane do ustalenia, co zmienione podczas dodawania dalej migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="d40a8-115">Sygnatura czasowa w nazwie pliku pomaga im uporządkowane chronologicznie, tak aby był widoczny postęp zmiany.</span><span class="sxs-lookup"><span data-stu-id="d40a8-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="d40a8-116">Mogą przenieść pliki migracji i zmieniać ich nazw.</span><span class="sxs-lookup"><span data-stu-id="d40a8-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="d40a8-117">Nowe migracje są tworzone jako elementy równorzędne ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="d40a8-118">Następnie Zastosuj migrację do bazy danych w celu utworzenia schematu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="d40a8-119">Dodawanie innego migracji</span><span class="sxs-lookup"><span data-stu-id="d40a8-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="d40a8-120">Po wprowadzeniu zmian do modelu platformy EF Core, schemat bazy danych będą zsynchronizowane. Aby przywrócić je na bieżąco, Dodaj inna migracja.</span><span class="sxs-lookup"><span data-stu-id="d40a8-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="d40a8-121">Nazwa migracji mogą być używane jak wiadomość dotyczącą zatwierdzenia, w systemie kontroli wersji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="d40a8-122">Na przykład, jeśli wprowadzone zmiany, aby zapisać przeglądy wykonywane przez klientów produktów, może wybrać podobny *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="d40a8-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="d40a8-123">Po migracji jest działanie, należy poprawność i Dodaj wszelkie dodatkowe operacje wymagane do zastosowania je poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d40a8-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="d40a8-124">Na przykład migracja może zawierać następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="d40a8-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="d40a8-125">Chociaż te operacje zapewnić zgodność schematu bazy danych, nie przechowują nazwy istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="d40a8-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="d40a8-126">Ją ulepszyć, przepisać go tak, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d40a8-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="d40a8-127">Dodawanie nowej migracji ostrzega, gdy działanie jest operacją, która może spowodować utratę danych (np. porzucenie kolumny).</span><span class="sxs-lookup"><span data-stu-id="d40a8-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="d40a8-128">Należy zwłaszcza przejrzeć te migracje dokładność.</span><span class="sxs-lookup"><span data-stu-id="d40a8-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="d40a8-129">Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.</span><span class="sxs-lookup"><span data-stu-id="d40a8-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="d40a8-130">Usuwanie migracji</span><span class="sxs-lookup"><span data-stu-id="d40a8-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="d40a8-131">Czasami Dodaj migrację i należy pamiętać, że należy wprowadzić dodatkowe zmiany w modelu platformy EF Core, zanim zostaną one zastosowane.</span><span class="sxs-lookup"><span data-stu-id="d40a8-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="d40a8-132">Aby usunąć ostatniego migracji, użyj tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="d40a8-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="d40a8-133">Po usunięciu go można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.</span><span class="sxs-lookup"><span data-stu-id="d40a8-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="d40a8-134">Powracanie do migracji</span><span class="sxs-lookup"><span data-stu-id="d40a8-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="d40a8-135">Jeśli migracji (lub kilka migracje) już zastosowane do bazy danych, ale trzeba przywrócić ją, można użyć tego samego polecenia zastosowania migracji, ale określ nazwę migracji, które chcesz przywrócić.</span><span class="sxs-lookup"><span data-stu-id="d40a8-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="d40a8-136">Pusty migracji</span><span class="sxs-lookup"><span data-stu-id="d40a8-136">Empty migrations</span></span>
----------------
<span data-ttu-id="d40a8-137">Czasami przydatne jest dodać migrację bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="d40a8-138">W takim przypadku dodawania nowej migracji tworzy pusty.</span><span class="sxs-lookup"><span data-stu-id="d40a8-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="d40a8-139">Można dostosować tej migracji, aby wykonywać operacje, które bezpośrednio nie odnoszą się do modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="d40a8-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="d40a8-140">Jest kilka rzeczy, które można zmienić, aby zarządzać w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="d40a8-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="d40a8-141">Wyszukiwanie pełnotekstowe</span><span class="sxs-lookup"><span data-stu-id="d40a8-141">Full-Text Search</span></span>
* <span data-ttu-id="d40a8-142">Funkcje</span><span class="sxs-lookup"><span data-stu-id="d40a8-142">Functions</span></span>
* <span data-ttu-id="d40a8-143">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="d40a8-143">Stored procedures</span></span>
* <span data-ttu-id="d40a8-144">Wyzwalacze</span><span class="sxs-lookup"><span data-stu-id="d40a8-144">Triggers</span></span>
* <span data-ttu-id="d40a8-145">Widoki</span><span class="sxs-lookup"><span data-stu-id="d40a8-145">Views</span></span>
* <span data-ttu-id="d40a8-146">Itp.</span><span class="sxs-lookup"><span data-stu-id="d40a8-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="d40a8-147">Generowanie skryptu SQL</span><span class="sxs-lookup"><span data-stu-id="d40a8-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="d40a8-148">Podczas debugowania migracji lub ich wdrożeniem w produkcyjnej bazie danych, jest przydatne, można wygenerować skryptu SQL.</span><span class="sxs-lookup"><span data-stu-id="d40a8-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="d40a8-149">Skrypt można być dodatkowo zweryfikowane pod kątem dokładności i dopasowane do potrzeb produkcyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d40a8-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="d40a8-150">Skrypt można również w połączeniu z technologią wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="d40a8-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="d40a8-151">Podstawowe polecenia jest następująca.</span><span class="sxs-lookup"><span data-stu-id="d40a8-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="d40a8-152">Dostępnych jest kilka opcji tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="d40a8-152">There are several options to this command.</span></span>

<span data-ttu-id="d40a8-153">**z** migracji należy ostatniej migracji, które są stosowane do bazy danych przed uruchomieniem skryptu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="d40a8-154">Jeśli migracja nie zostały zastosowane, należy określić `0` (jest to wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="d40a8-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="d40a8-155">**Do** migracji jest ostatni migracji, która zostanie zastosowana do bazy danych po uruchomieniu skryptu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="d40a8-156">Domyślnie do ostatniego migracji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d40a8-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="d40a8-157">**Idempotentne** Opcjonalnie można wygenerować skryptu.</span><span class="sxs-lookup"><span data-stu-id="d40a8-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="d40a8-158">Ten skrypt tylko w przypadku migracji nie zostały jeszcze zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d40a8-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="d40a8-159">Jest to przydatne, jeśli nie dokładnie wiesz ostatniej migracji zastosowana do bazy danych zostało lub jeśli są wdrażane do wielu baz danych, które mogą znajdować się w różnych migracji.</span><span class="sxs-lookup"><span data-stu-id="d40a8-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="d40a8-160">Stosowanie migracji w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="d40a8-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="d40a8-161">Niektóre aplikacje mogą chcieć zastosować migracji w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="d40a8-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="d40a8-162">To zrobić za pomocą `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="d40a8-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="d40a8-163">Uwaga: Ta metoda nie jest dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d40a8-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="d40a8-164">Chociaż jest to doskonałe rozwiązanie dla aplikacji przy użyciu lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.</span><span class="sxs-lookup"><span data-stu-id="d40a8-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="d40a8-165">Nie wywołuj `EnsureCreated()` przed `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="d40a8-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="d40a8-166">`EnsureCreated()` Pomija migracji w celu utworzenia schematu, co powoduje, że `Migrate()` nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="d40a8-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="d40a8-167">Ta metoda tworzy się w górnej części `IMigrator` usługa, która może służyć do bardziej zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="d40a8-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="d40a8-168">Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="d40a8-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
