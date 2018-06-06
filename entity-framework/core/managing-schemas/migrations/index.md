---
title: Migracje - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754512"
---
<a name="migrations"></a>Migracje
==========
Migracje umożliwiają przyrostowo Zastosuj zmiany schematu do bazy danych, aby zachować synchronizację z modelem EF Core przy zachowaniu istniejących danych w bazie danych.

<a name="creating-the-database"></a>Tworzenie bazy danych
---------------------
Po wprowadzeniu [zdefiniowane początkowej modelu][1], należy utworzyć bazę danych. Aby to zrobić, należy dodać początkowej migracji.
Zainstaluj [EF podstawowe narzędzia] [ 2] i uruchom odpowiednie polecenie.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Trzy pliki zostaną dodane do projektu pod **migracje** katalogu:

* **00000000000000_InitialCreate.cs**--pliku głównego migracji. Zawiera operacji konieczne jest stosowanie migracji (w `Up()`) i można go przywrócić (w `Down()`).
* **00000000000000_InitialCreate.Designer.cs**--migracji pliku metadanych. Zawiera informacje używane przez EF.
* **MyContextModelSnapshot.cs**--migawkę bieżącego modelu. Używany do określania, jakie zmienione podczas dodawania następnej migracji.

Sygnatury czasowej w nazwie pliku pomaga zachować uporządkowanych w porządku chronologicznym, zostanie wyświetlony postęp zmiany.

> [!TIP]
> Mogą przenosić pliki migracji i zmienić ich nazw. Nowe migracji są tworzone jako elementy równorzędne ostatniej migracji.

Następnie dotyczą migracji bazy danych można utworzyć schematu.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Dodawanie innego migracji
------------------------
Po wprowadzeniu zmian w modelu podstawowej EF, schemat bazy danych będzie zsynchronizowany. Aby przywrócić dane, należy dodać innego migracji. Nazwa migracji można jak komunikat zatwierdzenia w systemie kontroli wersji. Na przykład, jeśli wprowadzone zmiany, aby zapisać klienta recenzje produktów, może wybrać przypominać *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migracji jest szkieletu, możesz poprawność i Dodaj wszelkie dodatkowe operacje wymagane się poprawnie. Na przykład migracja może zawierać następujące operacje:

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

Gdy te operacje zapewnić zgodność ze schematem bazy danych, nie przechowują nazwy istniejących klientów. Aby umożliwić lepsze, przepisać w następujący sposób.

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
> Dodawanie nowych migracji wyświetli ostrzeżenie, gdy operacja jest szkieletu, która może spowodować utratę danych (takich jak porzucenie kolumny). Należy przejrzeć szczególnie tych migracje dokładności.

Zastosowanie migracji w bazie danych za pomocą odpowiedniego polecenia.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Usuwanie migracji
--------------------
Czasami Dodaj migracji i należy pamiętać, że należy wprowadzać dodatkowych zmian modelu, EF Core przed zastosowaniem.
Aby usunąć ostatniego migracji, użyj tego polecenia.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po usunięciu go można wprowadzić zmiany modelu dodatkowe i dodaj go ponownie.

<a name="reverting-a-migration"></a>Powrót do migracji
---------------------
Jeśli migracji (lub kilka migracje) już zastosowana do bazy danych, trzeba przywrócić ją, korzystając tego samego polecenia Zastosuj migracje, ale należy określić nazwę migracji, który chcesz przywrócić.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Pusty migracji
----------------
Czasami jest przydatne do dodania do migracji bez wprowadzania żadnych zmian w modelu. W takim przypadku Dodawanie nowych migracji tworzy pusty. Można dostosować tej migracji w celu wykonania operacji, które bezpośrednio nie odnoszą się do modelu EF Core.
Niektóre elementy, można zarządzać w ten sposób są:

* Wyszukiwanie pełnotekstowe
* Funkcje
* Procedury składowane
* Wyzwalacze
* Widoki
* itp.

<a name="generating-a-sql-script"></a>Generowanie skryptu SQL
-----------------------
Podczas debugowania migracji lub ich wdrożeniem w produkcyjnej bazie danych, jest przydatne do generowania skryptu SQL. Skrypt można następnie można dodatkowo sprawdzone pod kątem dokładności i dopasowane do potrzeb produkcyjną bazę danych. Skryptu można również w połączeniu z technologii wdrażania. Poniżej przedstawiono podstawowe polecenia.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Dostępnych jest kilka opcji tego polecenia.

**z** migracji należy ostatniej migracji, które są stosowane do bazy danych przed uruchomieniem skryptu. Jeśli żadne migracje nie zostały zastosowane, należy określić `0` (jest to wartość domyślna).

**Do** migracji jest ostatniej migracji, które zostaną zastosowane do bazy danych po uruchomieniu skryptu. Domyślnie ostatni migracji w projekcie.

**Idempotentności** Opcjonalnie można wygenerować skryptów. Ten skrypt tylko w przypadku migracji nie zostały jeszcze zastosowane do bazy danych. Jest to przydatne, jeśli nie dokładnie wiadomo, ostatniej migracji stosowane do bazy danych został lub jeśli wdrażasz wiele baz danych, które mogą znajdować się w różnych migracji.

<a name="applying-migrations-at-runtime"></a>Stosowanie migracje w czasie wykonywania
------------------------------
Niektóre aplikacje możesz zastosować migracji w środowisku uruchomieniowym podczas uruchamiania lub pierwszego uruchomienia. To zrobić przy użyciu `Migrate()` metody.

Uwaga: Ta metoda nie jest dla wszystkich użytkowników. Mimo że jest doskonałym dla aplikacji z lokalnej bazy danych, większość aplikacji będzie wymagać bardziej niezawodne strategię wdrażania, takie jak Generowanie skryptów SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Nie wywołuj `EnsureCreated()` przed `Migrate()`. `EnsureCreated()` Pomija migracji można utworzyć schematu, co powoduje, że `Migrate()` się niepowodzeniem.

> [!NOTE]
> Ta metoda tworzy nad `IMigrator` usługi, która może służyć do bardziej zaawansowanych scenariuszy. Użyj `DbContext.GetService<IMigrator>()` do niego dostęp.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
