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
<a name="migrations"></a>Migracje
==========
Migracje umożliwiają dotyczą przyrostowe zmiany schematu bazy danych, aby zachować synchronizację z modelu platformy EF Core przy jednoczesnym zachowaniu istniejących danych w bazie danych.

<a name="creating-the-database"></a>Tworzenie bazy danych
---------------------
Po [zdefiniowany model początkowej][1], nadszedł czas, aby utworzyć bazę danych. Aby to zrobić, Dodaj początkowej migracji.
Zainstaluj [EF Core Tools] [ 2] i uruchom odpowiednie polecenie.

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

Następnie Zastosuj migrację do bazy danych w celu utworzenia schematu.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Dodawanie innego migracji
------------------------
Po wprowadzeniu zmian do modelu platformy EF Core, schemat bazy danych będą zsynchronizowane. Aby przywrócić je na bieżąco, Dodaj inna migracja. Nazwa migracji mogą być używane jak wiadomość dotyczącą zatwierdzenia, w systemie kontroli wersji. Na przykład, jeśli wprowadzone zmiany, aby zapisać przeglądy wykonywane przez klientów produktów, może wybrać podobny *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migracji jest działanie, należy poprawność i Dodaj wszelkie dodatkowe operacje wymagane do zastosowania je poprawnie. Na przykład migracja może zawierać następujące operacje:

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
> Dodawanie nowej migracji ostrzega, gdy działanie jest operacją, która może spowodować utratę danych (np. porzucenie kolumny). Należy zwłaszcza przejrzeć te migracje dokładność.

Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Usuwanie migracji
--------------------
Czasami Dodaj migrację i należy pamiętać, że należy wprowadzić dodatkowe zmiany w modelu platformy EF Core, zanim zostaną one zastosowane.
Aby usunąć ostatniego migracji, użyj tego polecenia.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po usunięciu go można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.

<a name="reverting-a-migration"></a>Powracanie do migracji
---------------------
Jeśli migracji (lub kilka migracje) już zastosowane do bazy danych, ale trzeba przywrócić ją, można użyć tego samego polecenia zastosowania migracji, ale określ nazwę migracji, które chcesz przywrócić.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Pusty migracji
----------------
Czasami przydatne jest dodać migrację bez wprowadzania żadnych zmian w modelu. W takim przypadku dodawania nowej migracji tworzy pusty. Można dostosować tej migracji, aby wykonywać operacje, które bezpośrednio nie odnoszą się do modelu platformy EF Core.
Jest kilka rzeczy, które można zmienić, aby zarządzać w ten sposób:

* Wyszukiwanie pełnotekstowe
* Funkcje
* Procedury składowane
* Wyzwalacze
* Widoki
* Itp.

<a name="generating-a-sql-script"></a>Generowanie skryptu SQL
-----------------------
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

<a name="applying-migrations-at-runtime"></a>Stosowanie migracji w czasie wykonywania
------------------------------
Niektóre aplikacje mogą chcieć zastosować migracji w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia. To zrobić za pomocą `Migrate()` metody.

Uwaga: Ta metoda nie jest dla wszystkich użytkowników. Chociaż jest to doskonałe rozwiązanie dla aplikacji przy użyciu lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Nie wywołuj `EnsureCreated()` przed `Migrate()`. `EnsureCreated()` Pomija migracji w celu utworzenia schematu, co powoduje, że `Migrate()` nie powiedzie się.

> [!NOTE]
> Ta metoda tworzy się w górnej części `IMigrator` usługa, która może służyć do bardziej zaawansowanych scenariuszy. Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
