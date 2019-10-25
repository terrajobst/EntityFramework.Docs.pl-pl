---
title: Migracje — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: e9c4013d17a2d41772822f77b3ceba15702ffc48
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812063"
---
# <a name="migrations"></a><span data-ttu-id="1e5ad-102">Migracje</span><span class="sxs-lookup"><span data-stu-id="1e5ad-102">Migrations</span></span>

<span data-ttu-id="1e5ad-103">Model danych zmienia się podczas opracowywania i nie jest synchronizowany z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="1e5ad-104">Możesz porzucić bazę danych i pozwolić, aby EF utworzyły nową, zgodną z modelem, ale ta procedura powoduje utratę danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="1e5ad-105">Funkcja migracji w EF Core zapewnia sposób stopniowego aktualizowania schematu bazy danych, aby zachować synchronizację z modelem danych aplikacji przy zachowaniu istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="1e5ad-106">Migracje obejmują narzędzia wiersza polecenia i interfejsy API, które ułatwiają wykonywanie następujących zadań:</span><span class="sxs-lookup"><span data-stu-id="1e5ad-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="1e5ad-107">[Tworzenie migracji](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="1e5ad-108">Generuj kod, który może aktualizować bazę danych w celu zsynchronizowania jej z zestawem zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="1e5ad-109">[Zaktualizuj bazę danych](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="1e5ad-110">Zastosuj oczekujące migracje, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="1e5ad-111">[Dostosuj kod migracji](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="1e5ad-112">Czasami wygenerowany kod musi być modyfikowany lub uzupełniany.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="1e5ad-113">[Usuń migrację](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="1e5ad-114">Usuń wygenerowany kod.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-114">Delete the generated code.</span></span>
* <span data-ttu-id="1e5ad-115">[Przywrócenie migracji](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="1e5ad-116">Cofnij zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-116">Undo the database changes.</span></span>
* <span data-ttu-id="1e5ad-117">[Generuj skrypty SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="1e5ad-118">Może być potrzebny skrypt do zaktualizowania produkcyjnej bazy danych lub rozwiązywania problemów z kodem migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="1e5ad-119">[Zastosuj migracje w czasie wykonywania](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="1e5ad-120">W przypadku aktualizacji i uruchamiania skryptów w czasie projektowania nie są najlepszym rozwiązaniem, należy wywołać metodę `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="1e5ad-121">Jeśli `DbContext` znajduje się w innym zestawie niż projekt startowy, można jawnie określić projekty docelowe i uruchomieniowe w [narzędziu Konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell#target-and-startup-project) lub [narzędziach interfejs wiersza polecenia platformy .NET Core](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="1e5ad-122">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="1e5ad-122">Install the tools</span></span>

<span data-ttu-id="1e5ad-123">Zainstaluj [narzędzia wiersza polecenia](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="1e5ad-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="1e5ad-124">W przypadku programu Visual Studio zalecamy korzystanie z [narzędzi konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="1e5ad-125">W przypadku innych środowisk programistycznych wybierz [narzędzia interfejs wiersza polecenia platformy .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="1e5ad-126">Tworzenie migracji</span><span class="sxs-lookup"><span data-stu-id="1e5ad-126">Create a migration</span></span>

<span data-ttu-id="1e5ad-127">Po [zdefiniowaniu modelu początkowego](xref:core/modeling/index)należy utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="1e5ad-128">Aby dodać początkową migrację, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-128">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```

``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="1e5ad-129">Do projektu są dodawane trzy pliki w katalogu **migracji** :</span><span class="sxs-lookup"><span data-stu-id="1e5ad-129">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="1e5ad-130">**XXXXXXXXXXXXXX_InitialCreate. cs**— główny plik migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-130">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="1e5ad-131">Zawiera operacje niezbędne do zastosowania migracji (w `Up()`) i przywrócenia jej (w `Down()`).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-131">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="1e5ad-132">**XXXXXXXXXXXXXX_InitialCreate. Designer. cs**— plik metadanych migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="1e5ad-133">Zawiera informacje używane przez EF.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-133">Contains information used by EF.</span></span>
* <span data-ttu-id="1e5ad-134">**MyContextModelSnapshot.cs**— migawka bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-134">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="1e5ad-135">Służy do określania, co zmieniło się podczas dodawania następnej migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-135">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="1e5ad-136">Sygnatura czasowa w nazwie pliku pomaga zachować ich uporządkowane chronologicznie, aby zobaczyć postęp zmian.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-136">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="1e5ad-137">Możesz przenieść pliki migracji i zmienić ich przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-137">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="1e5ad-138">Nowe migracje są tworzone jako elementy równorzędne ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-138">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="1e5ad-139">Aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="1e5ad-139">Update the database</span></span>

<span data-ttu-id="1e5ad-140">Następnie Zastosuj migrację do bazy danych, aby utworzyć schemat.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-140">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

## <a name="customize-migration-code"></a><span data-ttu-id="1e5ad-141">Dostosowywanie kodu migracji</span><span class="sxs-lookup"><span data-stu-id="1e5ad-141">Customize migration code</span></span>

<span data-ttu-id="1e5ad-142">Po wprowadzeniu zmian w modelu EF Core schemat bazy danych może nie być zsynchronizowany. Aby zapewnić aktualność, Dodaj kolejną migrację.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-142">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="1e5ad-143">Nazwa migracji może być używana jak komunikat zatwierdzenia w systemie kontroli wersji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-143">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="1e5ad-144">Na przykład można wybrać nazwę, np. *AddProductReviews* , jeśli zmiana jest nową klasą jednostek do przeglądu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-144">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```

``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="1e5ad-145">Po utworzeniu szkieletowej migracji (kod wygenerowany dla niego), Przejrzyj kod pod kątem dokładności i Dodaj, Usuń lub zmodyfikuj wszystkie operacje wymagane do poprawnego zastosowania.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-145">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="1e5ad-146">Na przykład migracja może zawierać następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="1e5ad-146">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="1e5ad-147">Chociaż te operacje sprawiają, że schemat bazy danych jest zgodny, nie zachowuje istniejących nazw klientów.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-147">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="1e5ad-148">Aby go ulepszyć, napisz ponownie w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-148">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="1e5ad-149">Proces tworzenia szkieletu migracji ostrzega, gdy operacja może spowodować utratę danych (np. upuszczenie kolumny).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-149">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="1e5ad-150">Jeśli widzisz to ostrzeżenie, pamiętaj o tym, aby sprawdzić poprawność kodu migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-150">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="1e5ad-151">Zastosuj migrację do bazy danych przy użyciu odpowiedniego polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-151">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="1e5ad-152">Puste migracje</span><span class="sxs-lookup"><span data-stu-id="1e5ad-152">Empty migrations</span></span>

<span data-ttu-id="1e5ad-153">Czasami warto dodać migrację bez wprowadzania żadnych zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-153">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="1e5ad-154">W takim przypadku dodanie nowej migracji powoduje utworzenie plików kodu z pustymi klasami.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-154">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="1e5ad-155">Można dostosować tę migrację do wykonywania operacji, które nie są bezpośrednio powiązane z modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-155">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="1e5ad-156">W ten sposób możesz chcieć zarządzać tymi elementami:</span><span class="sxs-lookup"><span data-stu-id="1e5ad-156">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="1e5ad-157">Wyszukiwanie pełnotekstowe</span><span class="sxs-lookup"><span data-stu-id="1e5ad-157">Full-Text Search</span></span>
* <span data-ttu-id="1e5ad-158">Funkcje</span><span class="sxs-lookup"><span data-stu-id="1e5ad-158">Functions</span></span>
* <span data-ttu-id="1e5ad-159">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="1e5ad-159">Stored procedures</span></span>
* <span data-ttu-id="1e5ad-160">Wyzwalacze</span><span class="sxs-lookup"><span data-stu-id="1e5ad-160">Triggers</span></span>
* <span data-ttu-id="1e5ad-161">Widoki</span><span class="sxs-lookup"><span data-stu-id="1e5ad-161">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="1e5ad-162">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="1e5ad-162">Remove a migration</span></span>

<span data-ttu-id="1e5ad-163">Czasami należy dodać migrację i zdawać sobie sprawę, że należy wprowadzić dodatkowe zmiany w modelu EF Core przed ich zastosowaniem.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-163">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="1e5ad-164">Aby usunąć ostatnią migrację, użyj tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-164">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```

``` Console
dotnet ef migrations remove
```

<span data-ttu-id="1e5ad-165">Po usunięciu migracji możesz wprowadzić dodatkowe zmiany modelu i dodać je ponownie.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-165">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="1e5ad-166">Przywracanie migracji</span><span class="sxs-lookup"><span data-stu-id="1e5ad-166">Revert a migration</span></span>

<span data-ttu-id="1e5ad-167">Jeśli migracja (lub kilka migracji) została już zastosowana do bazy danych, ale konieczne jest jej przywrócenie, można użyć tego samego polecenia do zastosowania migracji, ale określić nazwę migracji, do której chcesz przeprowadzić przywracanie.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-167">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```

``` Console
dotnet ef database update LastGoodMigration
```

## <a name="generate-sql-scripts"></a><span data-ttu-id="1e5ad-168">Generuj skrypty SQL</span><span class="sxs-lookup"><span data-stu-id="1e5ad-168">Generate SQL scripts</span></span>

<span data-ttu-id="1e5ad-169">Podczas debugowania migracji lub wdrażania ich w produkcyjnej bazie danych warto wygenerować skrypt SQL.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-169">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="1e5ad-170">Skrypt może następnie być ponownie przeglądany pod kątem dokładności i dostrojony, aby dopasować potrzeby produkcyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-170">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="1e5ad-171">Skrypt może być również używany w połączeniu z technologią wdrażania.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-171">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="1e5ad-172">Podstawowe polecenie jest następujące.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-172">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```

``` Console
dotnet ef migrations script
```

<span data-ttu-id="1e5ad-173">Istnieje kilka opcji tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-173">There are several options to this command.</span></span>

<span data-ttu-id="1e5ad-174">Migracja **z** migracji powinna być ostatnią zastosowana do bazy danych przed uruchomieniem skryptu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-174">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="1e5ad-175">Jeśli nie zastosowano żadnych migracji, określ `0` (jest to wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="1e5ad-175">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="1e5ad-176">Migracja **do** migracji to Ostatnia migracja, która zostanie zastosowana do bazy danych po uruchomieniu skryptu.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-176">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="1e5ad-177">Ta wartość domyślna to Ostatnia migracja w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-177">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="1e5ad-178">Opcjonalnie można wygenerować skrypt **idempotentne** .</span><span class="sxs-lookup"><span data-stu-id="1e5ad-178">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="1e5ad-179">Ten skrypt stosuje tylko migracje, jeśli nie zostały one jeszcze zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-179">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="1e5ad-180">Jest to przydatne, jeśli nie masz dokładnej znajomości ostatniej migracji zastosowanej do bazy danych lub Jeśli wdrażasz ją w wielu bazach danych, które mogą znajdować się w innej migracji.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-180">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="1e5ad-181">Zastosuj migracje w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="1e5ad-181">Apply migrations at runtime</span></span>

<span data-ttu-id="1e5ad-182">Niektóre aplikacje mogą chcieć zastosować migracje w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-182">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="1e5ad-183">Zrób to przy użyciu metody `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-183">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="1e5ad-184">Ta metoda jest oparta na usłudze `IMigrator`, która może być używana w bardziej zaawansowanych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-184">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="1e5ad-185">Użyj `myDbContext.GetInfrastructure().GetService<IMigrator>()`, aby uzyskać do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-185">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="1e5ad-186">Takie podejście nie jest przeznaczone dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-186">This approach isn't for everyone.</span></span> <span data-ttu-id="1e5ad-187">Chociaż jest to doskonałe rozwiązanie w przypadku aplikacji z lokalną bazą danych, większość aplikacji będzie wymagała bardziej niezawodnej strategii wdrażania, takiej jak generowanie skryptów SQL.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-187">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="1e5ad-188">Nie wywołuj `EnsureCreated()` przed `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-188">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="1e5ad-189">`EnsureCreated()` pomija migracje, aby utworzyć schemat, co powoduje niepowodzenie `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-189">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e5ad-190">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1e5ad-190">Next steps</span></span>

<span data-ttu-id="1e5ad-191">Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="1e5ad-191">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
