---
title: Migracje — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: ffa9a34f13ab29f0ba93f9fd1f469398630604ce
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005458"
---
<a name="migrations"></a>Migracje
==========

Model danych zmienia się podczas opracowywania i nie jest synchronizowany z bazą danych. Możesz porzucić bazę danych i pozwolić, aby EF utworzyły nową, zgodną z modelem, ale ta procedura powoduje utratę danych. Funkcja migracji w EF Core zapewnia sposób stopniowego aktualizowania schematu bazy danych, aby zachować synchronizację z modelem danych aplikacji przy zachowaniu istniejących danych w bazie danych.

Migracje obejmują narzędzia wiersza polecenia i interfejsy API, które ułatwiają wykonywanie następujących zadań:

* [Tworzenie migracji](#create-a-migration). Generuj kod, który może aktualizować bazę danych w celu zsynchronizowania jej z zestawem zmian modelu.
* [Zaktualizuj bazę danych](#update-the-database). Zastosuj oczekujące migracje, aby zaktualizować schemat bazy danych.
* [Dostosuj kod migracji](#customize-migration-code). Czasami wygenerowany kod musi być modyfikowany lub uzupełniany.
* [Usuń migrację](#remove-a-migration). Usuń wygenerowany kod.
* [Przywrócenie migracji](#revert-a-migration). Cofnij zmiany w bazie danych.
* [Generuj skrypty SQL](#generate-sql-scripts). Może być potrzebny skrypt do zaktualizowania produkcyjnej bazy danych lub rozwiązywania problemów z kodem migracji.
* [Zastosuj migracje w czasie wykonywania](#apply-migrations-at-runtime). W przypadku aktualizacji i uruchamiania skryptów w czasie projektowania nie są najlepszym rozwiązaniem, należy `Migrate()` wywołać metodę.

> [!TIP]
> Jeśli znajduje się w innym zestawie niż projekt startowy, można jawnie określić projekty docelowe i uruchomieniowe w [narzędziu Konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell#target-and-startup-project) lub [Narzędzia interfejs wiersza polecenia platformy .NET Core.](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project) `DbContext`

<a name="install-the-tools"></a>Instalowanie narzędzi
-----------------

Zainstaluj [narzędzia wiersza polecenia](xref:core/miscellaneous/cli/index):
* W przypadku programu Visual Studio zalecamy korzystanie z [narzędzi konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell).
* W przypadku innych środowisk programistycznych wybierz [narzędzia interfejs wiersza polecenia platformy .NET Core](xref:core/miscellaneous/cli/dotnet).

<a name="create-a-migration"></a>Tworzenie migracji
------------------

Po [zdefiniowaniu modelu początkowego](xref:core/modeling/index)należy utworzyć bazę danych. Aby dodać początkową migrację, uruchom następujące polecenie.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Do projektu są dodawane trzy pliki w katalogu **migracji** :

* **XXXXXXXXXXXXXX_InitialCreate. cs**— główny plik migracji. Zawiera operacje niezbędne do zastosowania migracji (w programie `Up()`) i przywrócenia jej (w programie `Down()`).
* **XXXXXXXXXXXXXX_InitialCreate. Designer. cs**— plik metadanych migracji. Zawiera informacje używane przez EF.
* **MyContextModelSnapshot.cs**— migawka bieżącego modelu. Służy do określania, co zmieniło się podczas dodawania następnej migracji.

Sygnatura czasowa w nazwie pliku pomaga zachować ich uporządkowane chronologicznie, aby zobaczyć postęp zmian.

> [!TIP]
> Możesz przenieść pliki migracji i zmienić ich przestrzeń nazw. Nowe migracje są tworzone jako elementy równorzędne ostatniej migracji.

<a name="update-the-database"></a>Aktualizowanie bazy danych
-------------------

Następnie Zastosuj migrację do bazy danych, aby utworzyć schemat.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Dostosowywanie kodu migracji
------------------------

Po wprowadzeniu zmian w modelu EF Core schemat bazy danych może nie być zsynchronizowany. Aby zapewnić aktualność, Dodaj kolejną migrację. Nazwa migracji może być używana jak komunikat zatwierdzenia w systemie kontroli wersji. Na przykład można wybrać nazwę, np. *AddProductReviews* , jeśli zmiana jest nową klasą jednostek do przeglądu.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po utworzeniu szkieletowej migracji (kod wygenerowany dla niego), Przejrzyj kod pod kątem dokładności i Dodaj, Usuń lub zmodyfikuj wszystkie operacje wymagane do poprawnego zastosowania.

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

Chociaż te operacje sprawiają, że schemat bazy danych jest zgodny, nie zachowuje istniejących nazw klientów. Aby go ulepszyć, napisz ponownie w następujący sposób.

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
> Proces tworzenia szkieletu migracji ostrzega, gdy operacja może spowodować utratę danych (np. upuszczenie kolumny). Jeśli widzisz to ostrzeżenie, pamiętaj o tym, aby sprawdzić poprawność kodu migracji.

Zastosuj migrację do bazy danych przy użyciu odpowiedniego polecenia.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Puste migracje

Czasami warto dodać migrację bez wprowadzania żadnych zmian modelu. W takim przypadku dodanie nowej migracji powoduje utworzenie plików kodu z pustymi klasami. Można dostosować tę migrację do wykonywania operacji, które nie są bezpośrednio powiązane z modelem EF Core. W ten sposób możesz chcieć zarządzać tymi elementami:

* Wyszukiwanie pełnotekstowe
* Funkcje
* Procedury składowane
* Wyzwalacze
* Widoki

<a name="remove-a-migration"></a>Usuń migrację
------------------
Czasami należy dodać migrację i zdawać sobie sprawę, że należy wprowadzić dodatkowe zmiany w modelu EF Core przed ich zastosowaniem. Aby usunąć ostatnią migrację, użyj tego polecenia.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po usunięciu migracji możesz wprowadzić dodatkowe zmiany modelu i dodać je ponownie.

<a name="revert-a-migration"></a>Przywracanie migracji
------------------
Jeśli migracja (lub kilka migracji) została już zastosowana do bazy danych, ale konieczne jest jej przywrócenie, można użyć tego samego polecenia do zastosowania migracji, ale określić nazwę migracji, do której chcesz przeprowadzić przywracanie.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>Generuj skrypty SQL
--------------------
Podczas debugowania migracji lub wdrażania ich w produkcyjnej bazie danych warto wygenerować skrypt SQL. Skrypt może następnie być ponownie przeglądany pod kątem dokładności i dostrojony, aby dopasować potrzeby produkcyjnej bazy danych. Skrypt może być również używany w połączeniu z technologią wdrażania. Podstawowe polecenie jest następujące.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Istnieje kilka opcji tego polecenia.

Migracja **z** migracji powinna być ostatnią zastosowana do bazy danych przed uruchomieniem skryptu. Jeśli nie zastosowano żadnych migracji, określ `0` (jest to ustawienie domyślne).

Migracja **do** migracji to Ostatnia migracja, która zostanie zastosowana do bazy danych po uruchomieniu skryptu. Ta wartość domyślna to Ostatnia migracja w projekcie.

Opcjonalnie można wygenerować skrypt **idempotentne** . Ten skrypt stosuje tylko migracje, jeśli nie zostały one jeszcze zastosowane do bazy danych. Jest to przydatne, jeśli nie masz dokładnej znajomości ostatniej migracji zastosowanej do bazy danych lub Jeśli wdrażasz ją w wielu bazach danych, które mogą znajdować się w innej migracji.

<a name="apply-migrations-at-runtime"></a>Zastosuj migracje w czasie wykonywania
---------------------------
Niektóre aplikacje mogą chcieć zastosować migracje w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia. Zrób to przy użyciu `Migrate()` metody.

Ta metoda jest oparta na `IMigrator` usłudze, która może być używana w bardziej zaawansowanych scenariuszach. Użyj `myDbContext.GetInfrastructure().GetService<IMigrator>()` , aby uzyskać do niej dostęp.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Takie podejście nie jest przeznaczone dla wszystkich użytkowników. Chociaż jest to doskonałe rozwiązanie w przypadku aplikacji z lokalną bazą danych, większość aplikacji będzie wymagała bardziej niezawodnej strategii wdrażania, takiej jak generowanie skryptów SQL.
> * Nie wywołuj `EnsureCreated()` przed `Migrate()`. `EnsureCreated()`pomija migracje, aby utworzyć schemat, co powoduje `Migrate()` niepowodzenie.

<a name="next-steps"></a>Następne kroki
----------

Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.
