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
# <a name="migrations"></a>Migracje

Model danych zmienia się podczas tworzenia i pobiera zsynchronizowane z bazą danych. Można upuścić bazy danych i niech EF utworzyć nowy, który pasuje do modelu, ale ta procedura powoduje utratę danych. Funkcja migracji w EF Core umożliwia stopniową aktualizację schematu bazy danych, aby była zsynchronizowana z modelem danych aplikacji przy jednoczesnym zachowaniu istniejących danych w bazie danych.

Migracje obejmują narzędzia wiersza polecenia i interfejsy API, które pomagają w następujących zadaniach:

* [Utwórz migrację](#create-a-migration). Wygeneruj kod, który można zaktualizować bazę danych, aby zsynchronizować go z zestawem zmian modelu.
* [Zaktualizuj bazę danych](#update-the-database). Zastosuj oczekujące migracje, aby zaktualizować schemat bazy danych.
* [Dostosuj kod migracji](#customize-migration-code). Czasami wygenerowany kod musi zostać zmodyfikowany lub uzupełniony.
* [Usuń migrację](#remove-a-migration). Usuń wygenerowany kod.
* [Przywróć migrację](#revert-a-migration). Cofnij zmiany bazy danych.
* [Generowanie skryptów SQL](#generate-sql-scripts). Może być potrzebny skrypt do aktualizacji produkcyjnej bazy danych lub do rozwiązywania problemów z kodem migracji.
* [Zastosuj migracje w czasie wykonywania](#apply-migrations-at-runtime). Gdy aktualizacje czasu projektowania i uruchamianie skryptów `Migrate()` nie są najlepsze opcje, wywołaj metodę.

> [!TIP]
> Jeśli `DbContext` jest w innym zestawie niż projekt startowy, można jawnie określić projekty docelowe i uruchamiania w [narzędziach Konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell#target-and-startup-project) lub [narzędzia .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

## <a name="install-the-tools"></a>Zainstaluj narzędzia

Zainstaluj [narzędzia wiersza polecenia:](xref:core/miscellaneous/cli/index)

* W programie Visual Studio zalecamy [narzędzia konsoli Menedżera pakietów](xref:core/miscellaneous/cli/powershell).
* W przypadku innych środowisk programistycznych wybierz [narzędzie .NET Core CLI](xref:core/miscellaneous/cli/dotnet).

## <a name="create-a-migration"></a>Tworzenie migracji

Po [zdefiniowaniu modelu początkowego](xref:core/modeling/index)nadszedł czas, aby utworzyć bazę danych. Aby dodać migrację początkową, uruchom następujące polecenie.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

Trzy pliki są dodawane do projektu w katalogu **Migracje:**

* **XXXXXXXXXXXXXX_InitialCreate.cs**--Główny plik migracji. Zawiera operacje niezbędne do zastosowania `Up()`migracji (w) i `Down()`jej przywrócenia (w ).
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Plik metadanych migracji. Zawiera informacje używane przez EF.
* **MyContextModelSnapshot.cs**--Migawka bieżącego modelu. Służy do określenia, co zmieniło się podczas dodawania następnej migracji.

Sygnatura czasowa w nazwach plików pomaga zachować ich kolejność chronologicznie, dzięki czemu można zobaczyć postęp zmian.

> [!TIP]
> Możesz przenosić pliki migracji i zmieniać ich obszar nazw. Nowe migracje są tworzone jako elementy równorządowe ostatniej migracji.

## <a name="update-the-database"></a>Aktualizowanie bazy danych

Następnie zastosuj migrację do bazy danych, aby utworzyć schemat.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>Dostosowywanie kodu migracji

Po wszczęciem zmian w modelu EF Core schemat bazy danych może być zsynchronizowany. Aby ją aktualizować, dodaj kolejną migrację. Nazwa migracji może służyć jak komunikat zatwierdzania w systemie kontroli wersji. Na przykład można wybrać nazwę, takich jak *AddProductReviews,* jeśli zmiana jest nową klasą jednostki dla opinii.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

Gdy migracja jest szkieletu (kod wygenerowany dla niego), przejrzyj kod pod kątem dokładności i dodać, usunąć lub zmodyfikować wszelkie operacje wymagane do zastosowania go poprawnie.

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

Podczas tych operacji, aby schemat bazy danych zgodne, nie zachowują istniejących nazw klientów. Aby było lepiej, przepisz go w następujący sposób.

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
> Proces szkieletowania migracji ostrzega, kiedy operacja może spowodować utratę danych (na to upuszczenie kolumny). Jeśli zobaczysz to ostrzeżenie, należy szczególnie przejrzeć kod migracji pod kątem dokładności.

Zastosuj migrację do bazy danych za pomocą odpowiedniego polecenia.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>Puste migracje

Czasami warto dodać migrację bez wprowadzania jakichkolwiek zmian modelu. W takim przypadku dodanie nowej migracji tworzy pliki kodu z pustymi klasami. Tę migrację można dostosować do wykonywania operacji, które nie odnoszą się bezpośrednio do modelu EF Core. Niektóre rzeczy, którymi chcesz zarządzać w ten sposób, to:

* Wyszukiwanie pełnotekstowe
* Funkcje
* Procedury składowane
* Wyzwalacze
* Widoki

## <a name="remove-a-migration"></a>Usuwanie migracji

Czasami dodać migracji i sobie sprawę, trzeba wprowadzić dodatkowe zmiany w modelu EF Core przed zastosowaniem go. Aby usunąć ostatnią migrację, użyj tego polecenia.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

Po usunięciu migracji można wprowadzić dodatkowe zmiany modelu i dodać ją ponownie.

## <a name="revert-a-migration"></a>Przywróć migrację

Jeśli migracja (lub kilka migracji) została już zastosowana do bazy danych, ale trzeba ją przywrócić, można użyć tego samego polecenia do zastosowania migracji, ale określić nazwę migracji, do której chcesz wycofać.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>Generowanie skryptów SQL

Podczas debugowania migracji lub wdrażania ich do produkcyjnej bazy danych, jest przydatne do generowania skryptu SQL. Skrypt można następnie dokonać dalszego przeglądu pod kątem dokładności i dostroić do potrzeb produkcyjnej bazy danych. Skrypt może być również używany w połączeniu z technologią wdrażania. Polecenie podstawowe jest następujące.

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Podstawowe użycie
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>Z od (do dorozumianych)
Spowoduje to wygenerowanie skryptu SQL z tej migracji do najnowszej migracji.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>Z od i do
Spowoduje to wygenerowanie skryptu SQL z `from` migracji do określonej `to` migracji.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Można `from` użyć, który jest nowszy niż `to` w celu wygenerowania skryptu wycofywania. *Należy wziąć pod uwagę potencjalne scenariusze utraty danych.*

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Podstawowe użycie
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>Z od (do dorozumianych)
Spowoduje to wygenerowanie skryptu SQL z tej migracji do najnowszej migracji.
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>Z od i do
Spowoduje to wygenerowanie skryptu SQL z `from` migracji do określonej `to` migracji.
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Można `from` użyć, który jest nowszy niż `to` w celu wygenerowania skryptu wycofywania. *Należy wziąć pod uwagę potencjalne scenariusze utraty danych.*

***

Istnieje kilka opcji tego polecenia.

Migracja **z** migracji powinna być ostatnią migracją zastosowaną do bazy danych przed uruchomieniem skryptu. Jeśli nie zastosowano żadnych `0` migracji, określ (jest to wartość domyślna).

Migracja **do** jest ostatnią migracją, która zostanie zastosowana do bazy danych po uruchomieniu skryptu. Domyślnie jest to ostatnia migracja w projekcie.

Opcjonalnie można wygenerować skrypt **idempotentny.** Ten skrypt stosuje migracje tylko wtedy, gdy nie zostały jeszcze zastosowane do bazy danych. Jest to przydatne, jeśli nie dokładnie wiesz, co ostatnia migracja zastosowana do bazy danych było lub jeśli wdrażasz do wielu baz danych, które mogą być w innej migracji.

## <a name="apply-migrations-at-runtime"></a>Stosowanie migracji w czasie wykonywania

Niektóre aplikacje mogą chcieć zastosować migracje w czasie wykonywania podczas uruchamiania lub pierwszego uruchomienia. Zrób to `Migrate()` za pomocą metody.

Ta metoda tworzy na `IMigrator` górze usługi, która może służyć do bardziej zaawansowanych scenariuszy. Użyj, `myDbContext.GetInfrastructure().GetService<IMigrator>()` aby uzyskać do niego dostęp.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * Takie podejście nie jest dla wszystkich. Chociaż doskonale nadaje się do aplikacji z lokalną bazą danych, większość aplikacji będzie wymagać bardziej niezawodnej strategii wdrażania, takiej jak generowanie skryptów SQL.
> * Nie dzwonij `EnsureCreated()` `Migrate()`wcześniej . `EnsureCreated()`pomija migracje, aby utworzyć schemat, `Migrate()` co powoduje niepowodzenie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz <xref:core/miscellaneous/cli/index>.
