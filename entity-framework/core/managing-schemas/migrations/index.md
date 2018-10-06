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
<a name="migrations"></a>Migracje
==========

Model danych zmienia się podczas tworzenia i jest niezsynchronizowana z bazą danych. Można porzucić bazy danych i umożliwić EF, Utwórz nową, który odpowiada modelu, ale ta procedura powoduje utratę danych. Funkcja migracji w programie EF Core umożliwia przyrostowe Aktualizowanie schematu bazy danych, aby zachować synchronizację z modelem danych aplikacji przy jednoczesnym zachowaniu istniejących danych w bazie danych.

Migracja obejmuje narzędzia wiersza polecenia i interfejsów API za pomocą następujących zadań:

* [Utwórz migracji](#create-a-migration). Generowanie kodu, który umożliwia zaktualizowanie bazy danych, aby zsynchronizować go z zestawem zmian modelu.
* [Aktualizacji bazy danych](#update-the-database). Zastosuj oczekujące migracji do zaktualizowania schematu bazy danych.
* [Dostosowywanie kodu migracji](#customize-migration-code). Czasami wygenerowany kod, musi zostać zmodyfikowane albo uzupełniane.
* [Usuń migrację](#remove-a-migration). Usuwanie wygenerowanego kodu.
* [Przywróć migracji](#revert-a-migration). Cofnąć zmiany w bazie danych.
* [Generuj skrypty SQL](#generate-sql-scripts). Może być konieczne skryptu zaktualizuj produkcyjnej bazy danych lub rozwiązywania problemów z kodem migracji.
* [Zastosuj migracji w środowisku uruchomieniowym](#apply-migrations-at-runtime). Po aktualizacji w czasie projektowania i uruchamianie skryptów nie są najlepsze opcje, wywołaj `Migrate()` metody.

<a name="install-the-tools"></a>Instalowanie narzędzi
-----------------

Zainstaluj [narzędzia wiersza polecenia](xref:core/miscellaneous/cli/index):
* Dla programu Visual Studio, firma Microsoft zaleca [narzędzia Konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell).
* W innych środowiskach rozwojowych, wybierz [narzędzi interfejsu wiersza polecenia platformy .NET Core](xref:core/miscellaneous/cli/dotnet).

<a name="create-a-migration"></a>Utwórz migracji
------------------

Po [zdefiniowany model początkowej](xref:core/modeling/index), nadszedł czas, aby utworzyć bazę danych. Aby dodać początkowej migracji, uruchom następujące polecenie.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Trzy pliki są dodawane do projektu w obszarze **migracje** katalogu:

* **00000000000000_InitialCreate.cs**— plik główny migracji. Zawiera operacje wymagane do zastosowania migracji (w `Up()`) i można go przywrócić (w `Down()`).
* **00000000000000_InitialCreate.Designer.cs**— plik metadanych migracji. Zawiera informacje używane przez EF.
* **MyContextModelSnapshot.cs**--migawkę bieżącego modelu. Używane do ustalenia, co zmienione podczas dodawania dalej migracji.

Sygnatura czasowa w nazwie pliku pomaga im uporządkowane chronologicznie, tak aby był widoczny postęp zmiany.

> [!TIP]
> Mogą przenieść pliki migracji i zmieniać ich nazw. Nowe migracje są tworzone jako elementy równorzędne ostatniej migracji.

<a name="update-the-database"></a>Aktualizowanie bazy danych
-------------------

Następnie Zastosuj migrację do bazy danych w celu utworzenia schematu.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Dostosowywanie kodu migracji
------------------------

Po wprowadzeniu zmian do modelu platformy EF Core, schemat bazy danych może nie być zsynchronizowany. Aby przywrócić je na bieżąco, Dodaj inna migracja. Nazwa migracji mogą być używane jak wiadomość dotyczącą zatwierdzenia, w systemie kontroli wersji. Na przykład, możesz wybrać nazwę, takich jak *AddProductReviews* Jeśli ta zmiana jest nową klasę jednostki, aby.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migracji szkieletu (kod generowany dla niego), przejrzyj kod dokładności i dodać, usunąć lub zmodyfikować dowolne operacje wymagane do zastosowania je poprawnie.

Na przykład migracja może zawierać następujące operacje:

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

Chociaż te operacje zapewnić zgodność schematu bazy danych, nie przechowują nazwy istniejących klientów. Ją ulepszyć, przepisać go tak, jak pokazano poniżej.

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
> Proces tworzenia szkieletów migracji ostrzega, gdy operacja może spowodować utratę danych (np. porzucenie kolumny). Jeśli widzisz tego ostrzeżenia, należy szczególnie przejrzeć kod migracje dokładności.

Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Pusty migracji

Czasami przydatne jest dodać migrację bez wprowadzania żadnych zmian w modelu. W przypadku dodawania nowej migracji tworzy pliki kodu z puste klasy. Można dostosować tej migracji, aby wykonywać operacje, które bezpośrednio nie odnoszą się do modelu platformy EF Core. Jest kilka rzeczy, które można zmienić, aby zarządzać w ten sposób:

* Wyszukiwanie pełnotekstowe
* Funkcje
* Procedury składowane
* Wyzwalacze
* Widoki

<a name="remove-a-migration"></a>Usuń migrację
------------------
Czasami Dodaj migrację i należy pamiętać, że należy wprowadzić dodatkowe zmiany w modelu platformy EF Core, zanim zostaną one zastosowane. Aby usunąć ostatniego migracji, użyj tego polecenia.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po usunięciu migracji, można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.

<a name="revert-a-migration"></a>Przywróć migracji
------------------
Jeśli migracji (lub kilka migracje) już zastosowane do bazy danych, ale trzeba przywrócić ją, można użyć tego samego polecenia zastosowania migracji, ale określ nazwę migracji, które chcesz przywrócić.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>Generuj skrypty SQL
--------------------
Podczas debugowania migracji lub ich wdrożeniem w produkcyjnej bazie danych, jest przydatne, można wygenerować skryptu SQL. Skrypt można być dodatkowo zweryfikowane pod kątem dokładności i dopasowane do potrzeb produkcyjnej bazy danych. Skrypt można również w połączeniu z technologią wdrożenia. Podstawowe polecenia jest następująca.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Dostępnych jest kilka opcji tego polecenia.

**z** migracji należy ostatniej migracji, które są stosowane do bazy danych przed uruchomieniem skryptu. Jeśli migracja nie zostały zastosowane, należy określić `0` (jest to wartość domyślna).

**Do** migracji jest ostatni migracji, która zostanie zastosowana do bazy danych po uruchomieniu skryptu. Domyślnie do ostatniego migracji w projekcie.

**Idempotentne** Opcjonalnie można wygenerować skryptu. Ten skrypt tylko w przypadku migracji nie zostały jeszcze zastosowane do bazy danych. Jest to przydatne, jeśli nie dokładnie wiesz ostatniej migracji zastosowana do bazy danych zostało lub jeśli są wdrażane do wielu baz danych, które mogą znajdować się w różnych migracji.

<a name="apply-migrations-at-runtime"></a>Zastosuj migracji w czasie wykonywania
---------------------------
Niektóre aplikacje mogą chcieć zastosować migracji w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia. To zrobić za pomocą `Migrate()` metody.

Ta metoda tworzy się w górnej części `IMigrator` usługa, która może służyć do bardziej zaawansowanych scenariuszy. Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Ta metoda nie jest dla wszystkich użytkowników. Chociaż jest to doskonałe rozwiązanie dla aplikacji przy użyciu lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.
> * Nie wywołuj `EnsureCreated()` przed `Migrate()`. `EnsureCreated()` Pomija migracji w celu utworzenia schematu, co powoduje, że `Migrate()` nie powiedzie się.

<a name="next-steps"></a>Następne kroki
----------

Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.