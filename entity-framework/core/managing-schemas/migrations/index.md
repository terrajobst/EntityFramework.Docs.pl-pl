---
title: Migracje - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136197"
---
# <a name="migrations"></a><span data-ttu-id="49a63-102">Migracje</span><span class="sxs-lookup"><span data-stu-id="49a63-102">Migrations</span></span>

<span data-ttu-id="49a63-103">Model danych zmienia się podczas tworzenia i pobiera zsynchronizowane z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="49a63-104">Można upuścić bazy danych i niech EF utworzyć nowy, który pasuje do modelu, ale ta procedura powoduje utratę danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="49a63-105">Funkcja migracji w EF Core umożliwia stopniową aktualizację schematu bazy danych, aby była zsynchronizowana z modelem danych aplikacji przy jednoczesnym zachowaniu istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="49a63-106">Migracje obejmują narzędzia wiersza polecenia i interfejsy API, które pomagają w następujących zadaniach:</span><span class="sxs-lookup"><span data-stu-id="49a63-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="49a63-107">[Utwórz migrację](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="49a63-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="49a63-108">Wygeneruj kod, który można zaktualizować bazę danych, aby zsynchronizować go z zestawem zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="49a63-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="49a63-109">[Zaktualizuj bazę danych](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="49a63-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="49a63-110">Zastosuj oczekujące migracje, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="49a63-111">[Dostosuj kod migracji](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="49a63-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="49a63-112">Czasami wygenerowany kod musi zostać zmodyfikowany lub uzupełniony.</span><span class="sxs-lookup"><span data-stu-id="49a63-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="49a63-113">[Usuń migrację](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="49a63-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="49a63-114">Usuń wygenerowany kod.</span><span class="sxs-lookup"><span data-stu-id="49a63-114">Delete the generated code.</span></span>
* <span data-ttu-id="49a63-115">[Przywróć migrację](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="49a63-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="49a63-116">Cofnij zmiany bazy danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-116">Undo the database changes.</span></span>
* <span data-ttu-id="49a63-117">[Generowanie skryptów SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="49a63-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="49a63-118">Może być potrzebny skrypt do aktualizacji produkcyjnej bazy danych lub do rozwiązywania problemów z kodem migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="49a63-119">[Zastosuj migracje w czasie wykonywania](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="49a63-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="49a63-120">Gdy aktualizacje czasu projektowania i uruchamianie skryptów `Migrate()` nie są najlepsze opcje, wywołaj metodę.</span><span class="sxs-lookup"><span data-stu-id="49a63-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="49a63-121">Jeśli `DbContext` jest w innym zestawie niż projekt startowy, można jawnie określić projekty docelowe i uruchamiania w [narzędziach Konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell#target-and-startup-project) lub [narzędzia .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="49a63-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="49a63-122">Zainstaluj narzędzia</span><span class="sxs-lookup"><span data-stu-id="49a63-122">Install the tools</span></span>

<span data-ttu-id="49a63-123">Zainstaluj [narzędzia wiersza polecenia:](xref:core/miscellaneous/cli/index)</span><span class="sxs-lookup"><span data-stu-id="49a63-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="49a63-124">W programie Visual Studio zalecamy [narzędzia konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="49a63-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="49a63-125">W przypadku innych środowisk programistycznych wybierz [narzędzie .NET Core CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="49a63-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="49a63-126">Tworzenie migracji</span><span class="sxs-lookup"><span data-stu-id="49a63-126">Create a migration</span></span>

<span data-ttu-id="49a63-127">Po [zdefiniowaniu modelu początkowego](xref:core/modeling/index)nadszedł czas, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="49a63-128">Aby dodać migrację początkową, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="49a63-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-129">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-130">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="49a63-131">Trzy pliki są dodawane do projektu w katalogu **Migracje:**</span><span class="sxs-lookup"><span data-stu-id="49a63-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="49a63-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--Główny plik migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="49a63-133">Zawiera operacje niezbędne do zastosowania `Up()`migracji (w) i `Down()`jej przywrócenia (w ).</span><span class="sxs-lookup"><span data-stu-id="49a63-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="49a63-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Plik metadanych migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="49a63-135">Zawiera informacje używane przez EF.</span><span class="sxs-lookup"><span data-stu-id="49a63-135">Contains information used by EF.</span></span>
* <span data-ttu-id="49a63-136">**MyContextModelSnapshot.cs**--Migawka bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="49a63-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="49a63-137">Służy do określenia, co zmieniło się podczas dodawania następnej migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="49a63-138">Sygnatura czasowa w nazwach plików pomaga zachować ich kolejność chronologicznie, dzięki czemu można zobaczyć postęp zmian.</span><span class="sxs-lookup"><span data-stu-id="49a63-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="49a63-139">Możesz przenosić pliki migracji i zmieniać ich obszar nazw.</span><span class="sxs-lookup"><span data-stu-id="49a63-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="49a63-140">Nowe migracje są tworzone jako elementy równorządowe ostatniej migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="49a63-141">Aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="49a63-141">Update the database</span></span>

<span data-ttu-id="49a63-142">Następnie zastosuj migrację do bazy danych, aby utworzyć schemat.</span><span class="sxs-lookup"><span data-stu-id="49a63-142">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-143">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-144">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="49a63-145">Dostosowywanie kodu migracji</span><span class="sxs-lookup"><span data-stu-id="49a63-145">Customize migration code</span></span>

<span data-ttu-id="49a63-146">Po wszczęciem zmian w modelu EF Core schemat bazy danych może być zsynchronizowany. Aby ją aktualizować, dodaj kolejną migrację.</span><span class="sxs-lookup"><span data-stu-id="49a63-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="49a63-147">Nazwa migracji może służyć jak komunikat zatwierdzania w systemie kontroli wersji.</span><span class="sxs-lookup"><span data-stu-id="49a63-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="49a63-148">Na przykład można wybrać nazwę, takich jak *AddProductReviews,* jeśli zmiana jest nową klasą jednostki dla opinii.</span><span class="sxs-lookup"><span data-stu-id="49a63-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-149">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-150">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="49a63-151">Gdy migracja jest szkieletu (kod wygenerowany dla niego), przejrzyj kod pod kątem dokładności i dodać, usunąć lub zmodyfikować wszelkie operacje wymagane do zastosowania go poprawnie.</span><span class="sxs-lookup"><span data-stu-id="49a63-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="49a63-152">Na przykład migracja może zawierać następujące operacje:</span><span class="sxs-lookup"><span data-stu-id="49a63-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="49a63-153">Podczas tych operacji, aby schemat bazy danych zgodne, nie zachowują istniejących nazw klientów.</span><span class="sxs-lookup"><span data-stu-id="49a63-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="49a63-154">Aby było lepiej, przepisz go w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="49a63-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="49a63-155">Proces szkieletowania migracji ostrzega, kiedy operacja może spowodować utratę danych (na to upuszczenie kolumny).</span><span class="sxs-lookup"><span data-stu-id="49a63-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="49a63-156">Jeśli zobaczysz to ostrzeżenie, należy szczególnie przejrzeć kod migracji pod kątem dokładności.</span><span class="sxs-lookup"><span data-stu-id="49a63-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="49a63-157">Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.</span><span class="sxs-lookup"><span data-stu-id="49a63-157">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-158">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-159">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="49a63-160">Puste migracje</span><span class="sxs-lookup"><span data-stu-id="49a63-160">Empty migrations</span></span>

<span data-ttu-id="49a63-161">Czasami warto dodać migrację bez wprowadzania jakichkolwiek zmian modelu.</span><span class="sxs-lookup"><span data-stu-id="49a63-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="49a63-162">W takim przypadku dodanie nowej migracji tworzy pliki kodu z pustymi klasami.</span><span class="sxs-lookup"><span data-stu-id="49a63-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="49a63-163">Tę migrację można dostosować do wykonywania operacji, które nie odnoszą się bezpośrednio do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="49a63-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="49a63-164">Niektóre rzeczy, którymi chcesz zarządzać w ten sposób, to:</span><span class="sxs-lookup"><span data-stu-id="49a63-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="49a63-165">Wyszukiwanie pełnotekstowe</span><span class="sxs-lookup"><span data-stu-id="49a63-165">Full-Text Search</span></span>
* <span data-ttu-id="49a63-166">Funkcje</span><span class="sxs-lookup"><span data-stu-id="49a63-166">Functions</span></span>
* <span data-ttu-id="49a63-167">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="49a63-167">Stored procedures</span></span>
* <span data-ttu-id="49a63-168">Wyzwalacze</span><span class="sxs-lookup"><span data-stu-id="49a63-168">Triggers</span></span>
* <span data-ttu-id="49a63-169">Widoki</span><span class="sxs-lookup"><span data-stu-id="49a63-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="49a63-170">Usuwanie migracji</span><span class="sxs-lookup"><span data-stu-id="49a63-170">Remove a migration</span></span>

<span data-ttu-id="49a63-171">Czasami dodać migracji i sobie sprawę, trzeba wprowadzić dodatkowe zmiany w modelu EF Core przed zastosowaniem go.</span><span class="sxs-lookup"><span data-stu-id="49a63-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="49a63-172">Aby usunąć ostatnią migrację, użyj tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="49a63-172">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-173">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-174">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="49a63-175">Po usunięciu migracji można wprowadzić dodatkowe zmiany modelu i dodać ją ponownie.</span><span class="sxs-lookup"><span data-stu-id="49a63-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="49a63-176">Przywróć migrację</span><span class="sxs-lookup"><span data-stu-id="49a63-176">Revert a migration</span></span>

<span data-ttu-id="49a63-177">Jeśli migracja (lub kilka migracji) została już zastosowana do bazy danych, ale trzeba ją przywrócić, można użyć tego samego polecenia do zastosowania migracji, ale określić nazwę migracji, do której chcesz wycofać.</span><span class="sxs-lookup"><span data-stu-id="49a63-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-178">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-179">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="49a63-180">Generowanie skryptów SQL</span><span class="sxs-lookup"><span data-stu-id="49a63-180">Generate SQL scripts</span></span>

<span data-ttu-id="49a63-181">Podczas debugowania migracji lub wdrażania ich do produkcyjnej bazy danych, jest przydatne do generowania skryptu SQL.</span><span class="sxs-lookup"><span data-stu-id="49a63-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="49a63-182">Skrypt można następnie dokonać dalszego przeglądu pod kątem dokładności i dostroić do potrzeb produkcyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="49a63-183">Skrypt może być również używany w połączeniu z technologią wdrażania.</span><span class="sxs-lookup"><span data-stu-id="49a63-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="49a63-184">Polecenie podstawowe jest następujące.</span><span class="sxs-lookup"><span data-stu-id="49a63-184">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="49a63-185">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="49a63-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="49a63-186">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="49a63-186">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="49a63-187">Z od (do dorozumianych)</span><span class="sxs-lookup"><span data-stu-id="49a63-187">With From (to implied)</span></span>
<span data-ttu-id="49a63-188">Spowoduje to wygenerowanie skryptu SQL z tej migracji do najnowszej migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-188">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="49a63-189">Z od i do</span><span class="sxs-lookup"><span data-stu-id="49a63-189">With From and To</span></span>
<span data-ttu-id="49a63-190">Spowoduje to wygenerowanie skryptu SQL z `from` migracji do określonej `to` migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-190">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="49a63-191">Można `from` użyć, który jest nowszy niż `to` w celu wygenerowania skryptu wycofywania.</span><span class="sxs-lookup"><span data-stu-id="49a63-191">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="49a63-192">*Należy wziąć pod uwagę potencjalne scenariusze utraty danych.*</span><span class="sxs-lookup"><span data-stu-id="49a63-192">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="49a63-193">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a63-193">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="49a63-194">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="49a63-194">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="49a63-195">Z od (do dorozumianych)</span><span class="sxs-lookup"><span data-stu-id="49a63-195">With From (to implied)</span></span>
<span data-ttu-id="49a63-196">Spowoduje to wygenerowanie skryptu SQL z tej migracji do najnowszej migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-196">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="49a63-197">Z od i do</span><span class="sxs-lookup"><span data-stu-id="49a63-197">With From and To</span></span>
<span data-ttu-id="49a63-198">Spowoduje to wygenerowanie skryptu SQL z `from` migracji do określonej `to` migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-198">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="49a63-199">Można `from` użyć, który jest nowszy niż `to` w celu wygenerowania skryptu wycofywania.</span><span class="sxs-lookup"><span data-stu-id="49a63-199">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="49a63-200">*Należy wziąć pod uwagę potencjalne scenariusze utraty danych.*</span><span class="sxs-lookup"><span data-stu-id="49a63-200">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="49a63-201">Istnieje kilka opcji tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="49a63-201">There are several options to this command.</span></span>

<span data-ttu-id="49a63-202">Migracja **z** migracji powinna być ostatnią migracją zastosowaną do bazy danych przed uruchomieniem skryptu.</span><span class="sxs-lookup"><span data-stu-id="49a63-202">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="49a63-203">Jeśli nie zastosowano żadnych `0` migracji, określ (jest to wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="49a63-203">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="49a63-204">Migracja **do** jest ostatnią migracją, która zostanie zastosowana do bazy danych po uruchomieniu skryptu.</span><span class="sxs-lookup"><span data-stu-id="49a63-204">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="49a63-205">Domyślnie jest to ostatnia migracja w projekcie.</span><span class="sxs-lookup"><span data-stu-id="49a63-205">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="49a63-206">Opcjonalnie można wygenerować skrypt **idempotentny.**</span><span class="sxs-lookup"><span data-stu-id="49a63-206">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="49a63-207">Ten skrypt stosuje migracje tylko wtedy, gdy nie zostały jeszcze zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="49a63-207">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="49a63-208">Jest to przydatne, jeśli nie dokładnie wiesz, co ostatnia migracja zastosowana do bazy danych było lub jeśli wdrażasz do wielu baz danych, które mogą być w innej migracji.</span><span class="sxs-lookup"><span data-stu-id="49a63-208">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="49a63-209">Stosowanie migracji w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="49a63-209">Apply migrations at runtime</span></span>

<span data-ttu-id="49a63-210">Niektóre aplikacje mogą chcieć zastosować migracje w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="49a63-210">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="49a63-211">Zrób to `Migrate()` za pomocą metody.</span><span class="sxs-lookup"><span data-stu-id="49a63-211">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="49a63-212">Ta metoda tworzy na `IMigrator` górze usługi, która może służyć do bardziej zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="49a63-212">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="49a63-213">Użyj, `myDbContext.GetInfrastructure().GetService<IMigrator>()` aby uzyskać do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="49a63-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="49a63-214">Takie podejście nie jest dla wszystkich.</span><span class="sxs-lookup"><span data-stu-id="49a63-214">This approach isn't for everyone.</span></span> <span data-ttu-id="49a63-215">Chociaż doskonale nadaje się do aplikacji z lokalną bazą danych, większość aplikacji będzie wymagać bardziej niezawodnej strategii wdrażania, takiej jak generowanie skryptów SQL.</span><span class="sxs-lookup"><span data-stu-id="49a63-215">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="49a63-216">Nie dzwonij `EnsureCreated()` `Migrate()`wcześniej .</span><span class="sxs-lookup"><span data-stu-id="49a63-216">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="49a63-217">`EnsureCreated()`pomija migracje, aby utworzyć schemat, `Migrate()` co powoduje niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="49a63-217">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49a63-218">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="49a63-218">Next steps</span></span>

<span data-ttu-id="49a63-219">Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="49a63-219">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
