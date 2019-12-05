---
title: Odtwarzanie przez EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824469"
---
# <a name="reverse-engineering"></a>Odwrócenie inżynierii

Odtwarzanie jest procesem tworzenia szkieletu klas typu jednostki i klasy DbContext na podstawie schematu bazy danych. Można ją wykonać przy użyciu polecenia `Scaffold-DbContext` narzędzia EF Core Console Menedżera pakietów (PMC) lub polecenia `dotnet ef dbcontext scaffold` narzędzia interfejsu wiersza polecenia (CLI) platformy .NET.

## <a name="installing"></a>Instalowanie programu

Przed odwróceniem zespołu należy zainstalować [Narzędzia PMC](xref:core/miscellaneous/cli/powershell) (tylko Visual Studio) lub [Narzędzia interfejsu wiersza polecenia](xref:core/miscellaneous/cli/dotnet). Zobacz linki, aby uzyskać szczegółowe informacje.

Należy również zainstalować odpowiedniego [dostawcę bazy danych](xref:core/providers/index) dla schematu bazy danych, który ma być odtwarzany.

## <a name="connection-string"></a>Parametry połączenia

Pierwszym argumentem polecenia są parametry połączenia z bazą danych. Narzędzia użyją tych parametrów połączenia, aby odczytać schemat bazy danych.

Sposób tworzenia cudzysłowów i ucieczki parametrów połączenia zależy od powłoki, która jest używana do wykonywania polecenia. Szczegółowe informacje można znaleźć w dokumentacji powłoki. Na przykład program PowerShell wymaga wyznaczenia znaku `$`, ale nie `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Klucze tajne konfiguracji i użytkownika

Jeśli masz projekt ASP.NET Core, możesz użyć składni `Name=<connection-string>`, aby odczytać parametry połączenia z konfiguracji.

Jest to dobre rozwiązanie przy użyciu [Narzędzia tajnego Menedżera](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) w celu zachowania podzielenia hasła bazy danych od kodu bazowej.

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nazwa dostawcy

Drugi argument jest nazwą dostawcy. Nazwa dostawcy jest zwykle taka sama jak nazwa pakietu NuGet dostawcy.

## <a name="specifying-tables"></a>Określanie tabel

Wszystkie tabele w schemacie bazy danych są domyślnie odtwarzane w postaci typów jednostek. Można ograniczyć, które tabele są odtworzone, określając schematy i tabele.

`-Schemas` parametru w parametrze PMC i opcji `--schema` w interfejsie wiersza polecenia można użyć do uwzględnienia każdej tabeli w schemacie.

`-Tables` (PMC) i `--table` (CLI) mogą być używane do dołączania określonych tabel.

Aby uwzględnić wiele tabel w kryterium PMC, użyj tablicy.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Aby dołączyć wiele tabel w interfejsie wiersza polecenia, należy wybrać opcję wiele razy.

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Zachowywanie nazw

Nazwy tabel i kolumn są rozwiązywane w celu lepszego dopasowania do konwencji nazewnictwa platformy .NET dla typów i właściwości. Określenie przełącznika `-UseDatabaseNames` w obszarze PMC lub opcji `--use-database-names` w interfejsie wiersza polecenia spowoduje wyłączenie tego zachowania, zachowując możliwie jak najwięcej nazw baz danych. Nieprawidłowe identyfikatory platformy .NET nadal będą stałe i są one zgodne z konwencjami nazewnictwa platformy .NET.

## <a name="fluent-api-or-data-annotations"></a>Interfejs API Fluent lub adnotacje danych

Typy jednostek są domyślnie konfigurowane przy użyciu interfejsu API Fluent. Określ `-DataAnnotations` (PMC) lub `--data-annotations` (CLI), aby zamiast tego używać adnotacji danych, gdy jest to możliwe.

Na przykład korzystanie z interfejsu API Fluent spowoduje przetworzenie szkieletu:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Używanie adnotacji danych spowoduje szkielet:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nazwa kontekstu

Nazwa klasy DbContext z szkieletem jest nazwą bazy danych domyślnie sufiksem z *kontekstem* . Aby określić inny sposób, użyj `-Context` w PMC i `--context` w interfejsie wiersza polecenia.

## <a name="directories-and-namespaces"></a>Katalogi i przestrzenie nazw

Klasy jednostek i Klasa DbContext są szkieletowe w katalogu głównym projektu i używają domyślnej przestrzeni nazw projektu. Można określić katalog, w którym są szkieletowe klasy przy użyciu `-OutputDir` (PMC) lub `--output-dir` (CLI). Przestrzeń nazw będzie główną przestrzenią nazw oraz nazwami wszystkich podkatalogów w katalogu głównym projektu.

Można również użyć `-ContextDir` (PMC) i `--context-dir` (CLI) do tworzenia szkieletu klasy DbContext w oddzielnym katalogu z klas typu jednostki.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Jak to działa

Odtwarzanie rozpocznie się, odczytując schemat bazy danych. Odczytuje informacje o tabelach, kolumnach, ograniczeniach i indeksach.

Następnie używa informacji o schemacie do tworzenia modelu EF Core. Tabele są używane do tworzenia typów jednostek; kolumny są używane do tworzenia właściwości; i klucze obce są używane do tworzenia relacji.

Na koniec model jest używany do generowania kodu. Odpowiednie klasy typu jednostki, interfejs API Fluent i adnotacje danych są szkieletami, aby można było ponownie utworzyć ten sam model z poziomu aplikacji.

## <a name="limitations"></a>Ograniczenia

* Nie wszystkie elementy dotyczące modelu mogą być reprezentowane za pomocą schematu bazy danych. Na przykład informacje o [**hierarchiach dziedziczenia**](../modeling/inheritance.md), [**typach będących własnością**](../modeling/owned-entities.md)i [**podziałie tabeli**](../modeling/table-splitting.md) nie są obecne w schemacie bazy danych. W związku z tym konstrukcje te nigdy nie będą odtwarzane.
* Ponadto **niektóre typy kolumn** mogą nie być obsługiwane przez dostawcę EF Core. Te kolumny nie zostaną uwzględnione w modelu.
* Można zdefiniować [**tokeny współbieżności**](../modeling/concurrency.md)w modelu EF Core, aby uniemożliwić dwóm użytkownikom Aktualizowanie tej samej jednostki w tym samym czasie. Niektóre bazy danych mają specjalny typ reprezentujący ten typ kolumny (na przykład rowversion w SQL Server), w którym można odtworzyć te informacje. Jednak inne tokeny współbieżności nie będą odtwarzane.
* [Funkcja C# typu referencyjnego 8 dopuszczających wartość null](/dotnet/csharp/tutorials/nullable-reference-types) nie jest obecnie obsługiwana w przypadku odtwarzania C# : EF Core zawsze generuje kod, który zakłada, że funkcja jest wyłączona. Na przykład kolumny tekstu dopuszczające wartość null będą szkieletem jako właściwość o typie `string`, a nie `string?`, z interfejsem API Fluent lub adnotacjami danych używanymi do konfigurowania, czy właściwość jest wymagana. Można edytować kod szkieletowy i zastąpić je adnotacjami o C# wartości null. Obsługa tworzenia szkieletów dla typów odwołań do wartości null jest śledzona przez [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problemu.

## <a name="customizing-the-model"></a>Dostosowywanie modelu

Kod wygenerowany przez EF Core to Twój kod. Możesz go zmienić. Zostanie on wygenerowany ponownie tylko w przypadku ponownego odtworzenia tego samego modelu. Kod szkieletowy reprezentuje *jeden* model, który może być używany w celu uzyskania dostępu do bazy danych, ale nie jest to *jedyny* model, który może być używany.

Dostosuj klasy typu jednostki i klasy DbContext do Twoich potrzeb. Można na przykład zmienić nazwy typów i właściwości, wprowadzić hierarchie dziedziczenia lub podzielić tabelę na wiele jednostek. Można również usunąć nieunikatowe indeksy, nieużywane sekwencje i właściwości nawigacji, opcjonalne właściwości skalarne i nazwy ograniczeń z modelu.

Można również dodać dodatkowe konstruktory, metody, właściwości itp. użycie innej klasy częściowej w oddzielnym pliku. Takie podejście działa nawet wtedy, gdy zamierzasz ponownie odtworzyć model.

## <a name="updating-the-model"></a>Aktualizowanie modelu

Po wprowadzeniu zmian w bazie danych może być konieczne zaktualizowanie modelu EF Core w celu odzwierciedlenia tych zmian. W przypadku, gdy baza danych jest prosta, może być najłatwiej ręcznie wprowadzić zmiany w modelu EF Core. Na przykład zmiana nazwy tabeli lub kolumny, usunięcie kolumny lub aktualizacja typu kolumny to proste zmiany wprowadzane w kodzie.

Bardziej znaczące zmiany nie są jednak łatwe do ręcznego wprowadzania. Jednym z typowych przepływów pracy jest ponowne odtwarzanie modelu z bazy danych przy użyciu `-Force` (PMC) lub `--force` (CLI), aby zastąpić istniejący model zaktualizowanym.

Inną często żądaną funkcją jest możliwość aktualizowania modelu z bazy danych przy zachowaniu dostosowania, takiego jak renames, hierarchie typów itd. Użyj [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) problemu, aby śledzić postęp tej funkcji.

> [!WARNING]
> Jeśli odwrócisz model z bazy danych, wszelkie zmiany wprowadzone w plikach zostaną utracone.
