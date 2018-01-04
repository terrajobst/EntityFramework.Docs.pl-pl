---
title: Migracje - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 24fbe344eba9b99929d905ac2b9e49c68a1a4323
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
<a name="migrations"></a><span data-ttu-id="a2f0a-102">Migracje</span><span class="sxs-lookup"><span data-stu-id="a2f0a-102">Migrations</span></span>
==========
<span data-ttu-id="a2f0a-103">Migracje umożliwiają przyrostowo Zastosuj zmiany schematu do bazy danych, aby zachować synchronizację z modelem EF Core przy zachowaniu istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="a2f0a-104">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="a2f0a-104">Creating the database</span></span>
---------------------
<span data-ttu-id="a2f0a-105">Po wprowadzeniu [zdefiniowane początkowej modelu][1], należy utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="a2f0a-106">Aby to zrobić, należy dodać początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="a2f0a-107">Zainstaluj [EF podstawowe narzędzia] [ 2] i uruchom odpowiednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="a2f0a-108">Trzy pliki zostaną dodane do projektu pod **migracje** katalogu:</span><span class="sxs-lookup"><span data-stu-id="a2f0a-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="a2f0a-109">**00000000000000_InitialCreate.cs**--pliku głównego migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="a2f0a-110">Zawiera operacji konieczne jest stosowanie migracji (w `Up()`) i można go przywrócić (w `Down()`).</span><span class="sxs-lookup"><span data-stu-id="a2f0a-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="a2f0a-111">**00000000000000_InitialCreate.Designer.cs**--migracji pliku metadanych.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="a2f0a-112">Zawiera informacje używane przez EF.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-112">Contains information used by EF.</span></span>
* <span data-ttu-id="a2f0a-113">**MyContextModelSnapshot.cs**--migawkę bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="a2f0a-114">Używany do określania, jakie zmienione podczas dodawania następnej migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="a2f0a-115">Sygnatury czasowej w nazwie pliku pomaga zachować uporządkowanych w porządku chronologicznym, zostanie wyświetlony postęp zmiany.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="a2f0a-116">Mogą przenosić pliki migracji i zmienić ich nazw.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="a2f0a-117">Nowe migracji są tworzone jako elementy równorzędne ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="a2f0a-118">Następnie dotyczą migracji bazy danych można utworzyć schematu.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="a2f0a-119">Dodawanie innego migracji</span><span class="sxs-lookup"><span data-stu-id="a2f0a-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="a2f0a-120">Po wprowadzeniu zmian w modelu podstawowej EF, schemat bazy danych będzie zsynchronizowany. Aby przywrócić dane, należy dodać innego migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="a2f0a-121">Nazwa migracji można jak komunikat zatwierdzenia w systemie kontroli wersji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="a2f0a-122">Na przykład, jeśli wprowadzone zmiany, aby zapisać klienta recenzje produktów, może wybrać przypominać *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="a2f0a-123">Po migracji jest szkieletu, możesz poprawność i Dodaj wszelkie dodatkowe operacje wymagane się poprawnie.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="a2f0a-124">Na przykład migracja może zawierać następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="a2f0a-124">For example, your migration might contain the following operations:</span></span>

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

<span data-ttu-id="a2f0a-125">Gdy te operacje zapewnić zgodność ze schematem bazy danych, nie przechowują nazwy istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="a2f0a-126">Aby umożliwić lepsze, przepisać w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-126">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="a2f0a-127">Dodawanie nowych migracji wyświetli ostrzeżenie, gdy operacja jest szkieletu, która może spowodować utratę danych (takich jak porzucenie kolumny).</span><span class="sxs-lookup"><span data-stu-id="a2f0a-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="a2f0a-128">Należy przejrzeć szczególnie tych migracje dokładności.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="a2f0a-129">Zastosowanie migracji w bazie danych za pomocą odpowiedniego polecenia.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="a2f0a-130">Usuwanie migracji</span><span class="sxs-lookup"><span data-stu-id="a2f0a-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="a2f0a-131">Czasami Dodaj migracji i należy pamiętać, że należy wprowadzać dodatkowych zmian modelu, EF Core przed zastosowaniem.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="a2f0a-132">Aby usunąć ostatniego migracji, użyj tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="a2f0a-133">Po usunięciu go można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="a2f0a-134">Powrót do migracji</span><span class="sxs-lookup"><span data-stu-id="a2f0a-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="a2f0a-135">Jeśli migracji (lub kilka migracje) już zastosowana do bazy danych, trzeba przywrócić ją, korzystając tego samego polecenia Zastosuj migracje, ale należy określić nazwę migracji, który chcesz przywrócić.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="a2f0a-136">Pusty migracji</span><span class="sxs-lookup"><span data-stu-id="a2f0a-136">Empty migrations</span></span>
----------------
<span data-ttu-id="a2f0a-137">Czasami jest przydatne do dodania do migracji bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="a2f0a-138">W takim przypadku Dodawanie nowych migracji tworzy pusty.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="a2f0a-139">Można dostosować tej migracji w celu wykonania operacji, które bezpośrednio nie odnoszą się do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="a2f0a-140">Niektóre elementy, można zarządzać w ten sposób są:</span><span class="sxs-lookup"><span data-stu-id="a2f0a-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="a2f0a-141">Wyszukiwanie pełnotekstowe</span><span class="sxs-lookup"><span data-stu-id="a2f0a-141">Full-Text Search</span></span>
* <span data-ttu-id="a2f0a-142">Funkcje</span><span class="sxs-lookup"><span data-stu-id="a2f0a-142">Functions</span></span>
* <span data-ttu-id="a2f0a-143">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="a2f0a-143">Stored procedures</span></span>
* <span data-ttu-id="a2f0a-144">Wyzwalacze</span><span class="sxs-lookup"><span data-stu-id="a2f0a-144">Triggers</span></span>
* <span data-ttu-id="a2f0a-145">Widoki</span><span class="sxs-lookup"><span data-stu-id="a2f0a-145">Views</span></span>
* <span data-ttu-id="a2f0a-146">itp.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="a2f0a-147">Generowanie skryptu SQL</span><span class="sxs-lookup"><span data-stu-id="a2f0a-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="a2f0a-148">Podczas debugowania migracji lub ich wdrożeniem w produkcyjnej bazie danych, jest przydatne do generowania skryptu SQL.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="a2f0a-149">Skrypt można następnie można dodatkowo sprawdzone pod kątem dokładności i dopasowane do potrzeb produkcyjną bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="a2f0a-150">Skryptu można również w połączeniu z technologii wdrażania.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="a2f0a-151">Poniżej przedstawiono podstawowe polecenia.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="a2f0a-152">Dostępnych jest kilka opcji tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-152">There are several options to this command.</span></span>

<span data-ttu-id="a2f0a-153">**z** migracji należy ostatniej migracji, które są stosowane do bazy danych przed uruchomieniem skryptu.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="a2f0a-154">Jeśli żadne migracje nie zostały zastosowane, należy określić `0` (jest to wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="a2f0a-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="a2f0a-155">**Do** migracji jest ostatniej migracji, które zostaną zastosowane do bazy danych po uruchomieniu skryptu.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="a2f0a-156">Domyślnie ostatni migracji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="a2f0a-157">**Idempotentności** Opcjonalnie można wygenerować skryptów.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="a2f0a-158">Ten skrypt tylko w przypadku migracji nie zostały jeszcze zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="a2f0a-159">Jest to przydatne, jeśli nie dokładnie wiadomo, ostatniej migracji stosowane do bazy danych został lub jeśli wdrażasz wiele baz danych, które mogą znajdować się w różnych migracji.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="a2f0a-160">Stosowanie migracje w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="a2f0a-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="a2f0a-161">Niektóre aplikacje możesz zastosować migracji w środowisku uruchomieniowym podczas uruchamiania lub pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="a2f0a-162">To zrobić przy użyciu `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="a2f0a-163">Uwaga: Ta metoda nie jest dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="a2f0a-164">Mimo że jest doskonałym dla aplikacji z lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="a2f0a-165">Nie wywołuj `EnsureCreated()` przed `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="a2f0a-166">`EnsureCreated()`Pomija migracji, aby utworzyć schemat i spowodować `Migrate()` się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-166">`EnsureCreated()` bypasses Migrations to create the schema and cause `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="a2f0a-167">Ta metoda tworzy nad `IMigrator` usługi, która może służyć do bardziej zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="a2f0a-168">Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="a2f0a-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
