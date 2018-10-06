---
title: Migracje — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834750"
---
<a name="migrations"></a><span data-ttu-id="c0a40-102">Migracje</span><span class="sxs-lookup"><span data-stu-id="c0a40-102">Migrations</span></span>
==========

<span data-ttu-id="c0a40-103">Model danych zmienia się podczas tworzenia i jest niezsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="c0a40-104">Można porzucić bazy danych i umożliwić EF, Utwórz nową, który odpowiada modelu, ale ta procedura powoduje utratę danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="c0a40-105">Funkcja migracji w programie EF Core umożliwia przyrostowe Aktualizowanie schematu bazy danych, aby zachować synchronizację z modelem danych aplikacji przy jednoczesnym zachowaniu istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="c0a40-106">Migracja obejmuje narzędzia wiersza polecenia i interfejsów API za pomocą następujących zadań:</span><span class="sxs-lookup"><span data-stu-id="c0a40-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="c0a40-107">[Utwórz migracji](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c0a40-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="c0a40-108">Generowanie kodu, który umożliwia zaktualizowanie bazy danych, aby zsynchronizować go z zestawem zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="c0a40-109">[Aktualizacji bazy danych](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="c0a40-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="c0a40-110">Zastosuj oczekujące migracji do zaktualizowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="c0a40-111">[Dostosowywanie kodu migracji](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="c0a40-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="c0a40-112">Czasami wygenerowany kod, musi zostać zmodyfikowane albo uzupełniane.</span><span class="sxs-lookup"><span data-stu-id="c0a40-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="c0a40-113">[Usuń migrację](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c0a40-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="c0a40-114">Usuwanie wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-114">Delete the generated code.</span></span>
* <span data-ttu-id="c0a40-115">[Przywróć migracji](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c0a40-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="c0a40-116">Cofnąć zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-116">Undo the database changes.</span></span>
* <span data-ttu-id="c0a40-117">[Generuj skrypty SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="c0a40-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="c0a40-118">Może być konieczne skryptu zaktualizuj produkcyjnej bazy danych lub rozwiązywania problemów z kodem migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="c0a40-119">[Zastosuj migracji w środowisku uruchomieniowym](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="c0a40-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="c0a40-120">Po aktualizacji w czasie projektowania i uruchamianie skryptów nie są najlepsze opcje, wywołaj `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="c0a40-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="c0a40-121">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="c0a40-121">Install the tools</span></span>
-----------------

<span data-ttu-id="c0a40-122">Zainstaluj [narzędzia wiersza polecenia](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="c0a40-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="c0a40-123">Dla programu Visual Studio, firma Microsoft zaleca [narzędzia Konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="c0a40-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="c0a40-124">W innych środowiskach rozwojowych, wybierz [narzędzi interfejsu wiersza polecenia platformy .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="c0a40-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="c0a40-125">Utwórz migracji</span><span class="sxs-lookup"><span data-stu-id="c0a40-125">Create a migration</span></span>
------------------

<span data-ttu-id="c0a40-126">Po [zdefiniowany model początkowej](xref:core/modeling/index), nadszedł czas, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="c0a40-127">Aby dodać początkowej migracji, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="c0a40-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c0a40-128">Trzy pliki są dodawane do projektu w obszarze **migracje** katalogu:</span><span class="sxs-lookup"><span data-stu-id="c0a40-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="c0a40-129">**00000000000000_InitialCreate.cs**— plik główny migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-129">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="c0a40-130">Zawiera operacje wymagane do zastosowania migracji (w `Up()`) i można go przywrócić (w `Down()`).</span><span class="sxs-lookup"><span data-stu-id="c0a40-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="c0a40-131">**00000000000000_InitialCreate.Designer.cs**— plik metadanych migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-131">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="c0a40-132">Zawiera informacje używane przez EF.</span><span class="sxs-lookup"><span data-stu-id="c0a40-132">Contains information used by EF.</span></span>
* <span data-ttu-id="c0a40-133">**MyContextModelSnapshot.cs**--migawkę bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="c0a40-134">Używane do ustalenia, co zmienione podczas dodawania dalej migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="c0a40-135">Sygnatura czasowa w nazwie pliku pomaga im uporządkowane chronologicznie, tak aby był widoczny postęp zmiany.</span><span class="sxs-lookup"><span data-stu-id="c0a40-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="c0a40-136">Mogą przenieść pliki migracji i zmieniać ich nazw.</span><span class="sxs-lookup"><span data-stu-id="c0a40-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="c0a40-137">Nowe migracje są tworzone jako elementy równorzędne ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="c0a40-138">Aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="c0a40-138">Update the database</span></span>
-------------------

<span data-ttu-id="c0a40-139">Następnie Zastosuj migrację do bazy danych w celu utworzenia schematu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="c0a40-140">Dostosowywanie kodu migracji</span><span class="sxs-lookup"><span data-stu-id="c0a40-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="c0a40-141">Po wprowadzeniu zmian do modelu platformy EF Core, schemat bazy danych może nie być zsynchronizowany. Aby przywrócić je na bieżąco, Dodaj inna migracja.</span><span class="sxs-lookup"><span data-stu-id="c0a40-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="c0a40-142">Nazwa migracji mogą być używane jak wiadomość dotyczącą zatwierdzenia, w systemie kontroli wersji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="c0a40-143">Na przykład, możesz wybrać nazwę, takich jak *AddProductReviews* Jeśli ta zmiana jest nową klasę jednostki, aby.</span><span class="sxs-lookup"><span data-stu-id="c0a40-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="c0a40-144">Po migracji szkieletu (kod generowany dla niego), przejrzyj kod dokładności i dodać, usunąć lub zmodyfikować dowolne operacje wymagane do zastosowania je poprawnie.</span><span class="sxs-lookup"><span data-stu-id="c0a40-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="c0a40-145">Na przykład migracja może zawierać następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="c0a40-145">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="c0a40-146">Chociaż te operacje zapewnić zgodność schematu bazy danych, nie przechowują nazwy istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="c0a40-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="c0a40-147">Ją ulepszyć, przepisać go tak, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c0a40-147">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="c0a40-148">Proces tworzenia szkieletów migracji ostrzega, gdy operacja może spowodować utratę danych (np. porzucenie kolumny).</span><span class="sxs-lookup"><span data-stu-id="c0a40-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="c0a40-149">Jeśli widzisz tego ostrzeżenia, należy szczególnie przejrzeć kod migracje dokładności.</span><span class="sxs-lookup"><span data-stu-id="c0a40-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="c0a40-150">Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0a40-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="c0a40-151">Pusty migracji</span><span class="sxs-lookup"><span data-stu-id="c0a40-151">Empty migrations</span></span>

<span data-ttu-id="c0a40-152">Czasami przydatne jest dodać migrację bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="c0a40-153">W przypadku dodawania nowej migracji tworzy pliki kodu z puste klasy.</span><span class="sxs-lookup"><span data-stu-id="c0a40-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="c0a40-154">Można dostosować tej migracji, aby wykonywać operacje, które bezpośrednio nie odnoszą się do modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="c0a40-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="c0a40-155">Jest kilka rzeczy, które można zmienić, aby zarządzać w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="c0a40-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="c0a40-156">Wyszukiwanie pełnotekstowe</span><span class="sxs-lookup"><span data-stu-id="c0a40-156">Full-Text Search</span></span>
* <span data-ttu-id="c0a40-157">Funkcje</span><span class="sxs-lookup"><span data-stu-id="c0a40-157">Functions</span></span>
* <span data-ttu-id="c0a40-158">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="c0a40-158">Stored procedures</span></span>
* <span data-ttu-id="c0a40-159">Wyzwalacze</span><span class="sxs-lookup"><span data-stu-id="c0a40-159">Triggers</span></span>
* <span data-ttu-id="c0a40-160">Widoki</span><span class="sxs-lookup"><span data-stu-id="c0a40-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="c0a40-161">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="c0a40-161">Remove a migration</span></span>
------------------
<span data-ttu-id="c0a40-162">Czasami Dodaj migrację i należy pamiętać, że należy wprowadzić dodatkowe zmiany w modelu platformy EF Core, zanim zostaną one zastosowane.</span><span class="sxs-lookup"><span data-stu-id="c0a40-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="c0a40-163">Aby usunąć ostatniego migracji, użyj tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0a40-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="c0a40-164">Po usunięciu migracji, można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.</span><span class="sxs-lookup"><span data-stu-id="c0a40-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="c0a40-165">Przywróć migracji</span><span class="sxs-lookup"><span data-stu-id="c0a40-165">Revert a migration</span></span>
------------------
<span data-ttu-id="c0a40-166">Jeśli migracji (lub kilka migracje) już zastosowane do bazy danych, ale trzeba przywrócić ją, można użyć tego samego polecenia zastosowania migracji, ale określ nazwę migracji, które chcesz przywrócić.</span><span class="sxs-lookup"><span data-stu-id="c0a40-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="c0a40-167">Generuj skrypty SQL</span><span class="sxs-lookup"><span data-stu-id="c0a40-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="c0a40-168">Podczas debugowania migracji lub ich wdrożeniem w produkcyjnej bazie danych, jest przydatne, można wygenerować skryptu SQL.</span><span class="sxs-lookup"><span data-stu-id="c0a40-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="c0a40-169">Skrypt można być dodatkowo zweryfikowane pod kątem dokładności i dopasowane do potrzeb produkcyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="c0a40-170">Skrypt można również w połączeniu z technologią wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c0a40-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="c0a40-171">Podstawowe polecenia jest następująca.</span><span class="sxs-lookup"><span data-stu-id="c0a40-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="c0a40-172">Dostępnych jest kilka opcji tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0a40-172">There are several options to this command.</span></span>

<span data-ttu-id="c0a40-173">**z** migracji należy ostatniej migracji, które są stosowane do bazy danych przed uruchomieniem skryptu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="c0a40-174">Jeśli migracja nie zostały zastosowane, należy określić `0` (jest to wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="c0a40-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="c0a40-175">**Do** migracji jest ostatni migracji, która zostanie zastosowana do bazy danych po uruchomieniu skryptu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="c0a40-176">Domyślnie do ostatniego migracji w projekcie.</span><span class="sxs-lookup"><span data-stu-id="c0a40-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="c0a40-177">**Idempotentne** Opcjonalnie można wygenerować skryptu.</span><span class="sxs-lookup"><span data-stu-id="c0a40-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="c0a40-178">Ten skrypt tylko w przypadku migracji nie zostały jeszcze zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0a40-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="c0a40-179">Jest to przydatne, jeśli nie dokładnie wiesz ostatniej migracji zastosowana do bazy danych zostało lub jeśli są wdrażane do wielu baz danych, które mogą znajdować się w różnych migracji.</span><span class="sxs-lookup"><span data-stu-id="c0a40-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="c0a40-180">Zastosuj migracji w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="c0a40-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="c0a40-181">Niektóre aplikacje mogą chcieć zastosować migracji w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="c0a40-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="c0a40-182">To zrobić za pomocą `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="c0a40-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="c0a40-183">Ta metoda tworzy się w górnej części `IMigrator` usługa, która może służyć do bardziej zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="c0a40-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="c0a40-184">Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="c0a40-184">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="c0a40-185">Ta metoda nie jest dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c0a40-185">This approach isn't for everyone.</span></span> <span data-ttu-id="c0a40-186">Chociaż jest to doskonałe rozwiązanie dla aplikacji przy użyciu lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.</span><span class="sxs-lookup"><span data-stu-id="c0a40-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="c0a40-187">Nie wywołuj `EnsureCreated()` przed `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="c0a40-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="c0a40-188">`EnsureCreated()` Pomija migracji w celu utworzenia schematu, co powoduje, że `Migrate()` nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="c0a40-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="c0a40-189">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c0a40-189">Next steps</span></span>
----------

<span data-ttu-id="c0a40-190">Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="c0a40-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>