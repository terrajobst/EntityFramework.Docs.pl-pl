---
title: Odtwarzanie — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459188"
---
# <a name="reverse-engineering"></a>Odtwarzanie

Odtwarzanie jest proces tworzenia szkieletów klasy typów jednostek i klasy DbContext, na podstawie schematu bazy danych. Mogą być wykonywane przy użyciu `Scaffold-DbContext` polecenia narzędzia konsoli Menedżera pakietów (PMC) EF Core lub `dotnet ef dbcontext scaffold` polecenia narzędzia .NET interfejsu wiersza polecenia (CLI).

## <a name="installing"></a>Instalowanie programu

Przed odtwarzania, należy zainstalować jedną [narzędzia PMC](xref:core/miscellaneous/cli/powershell) (tylko Visual Studio) lub [narzędzi interfejsu wiersza polecenia](xref:core/miscellaneous/cli/dotnet). Zobacz linki, aby uzyskać szczegółowe informacje.

Należy także zainstalować odpowiednią [dostawcy bazy danych](xref:core/providers/index) dla schematu bazy danych ma zostać odtworzona.

## <a name="connection-string"></a>Parametry połączenia

Pierwszy argument polecenia jest ciąg połączenia z bazą danych. Narzędzia użyje te parametry połączenia można odczytać schematu bazy danych.

Jak oferty i wprowadzić ciąg połączenia zależy od Shell, które używają można wykonać polecenia. Zajrzyj do dokumentacji powłoki, aby uzyskać szczegółowe informacje. Na przykład programu PowerShell wymaga jako znak ucieczki `$` znaków, ale nie `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Konfiguracja i wpisami tajnymi użytkowników

Jeśli masz projekt platformy ASP.NET Core, możesz użyć `Name=<connection-string>` składni można odczytać parametrów połączenia z konfiguracji.

Ta funkcja działa dobrze z [narzędzie Menedżer klucz tajny](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) być oddzielone od kodu hasła bazy danych.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nazwa dostawcy

Drugi argument jest nazwą dostawcy. Nazwa dostawcy jest zwykle taka sama jak nazwa pakietu NuGet dostawcy.

## <a name="specifying-tables"></a>Określanie tabel

Wszystkie tabele w schemacie bazy danych są odtwarzane do typów jednostek, domyślnie. Można ograniczyć, które tabele są odtwarzane, określając schematy i tabele.

`-Schemas` Parametru w konsoli zarządzania Pakietami i `--schema` opcji interfejsu wiersza polecenia może służyć do uwzględnienia w każdej tabeli w schemacie.

`-Tables` (PMC) i `--table` (CLI) może służyć do uwzględnienia określonych tabel.

Aby uwzględnić wiele tabel w konsoli zarządzania Pakietami, skorzystaj z tablicy.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Aby uwzględnić wiele tabel w interfejsu wiersza polecenia, należy określić opcję wiele razy.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Zachowywanie nazwy

Nazwy tabel i kolumn są stałe, aby lepiej dopasować je do konwencji nazewnictwa platformy .NET, właściwości i typy domyślnie. Określanie `-UseDatabaseNames` przełącznika w konsoli zarządzania Pakietami lub `--use-database-names` opcji interfejsu wiersza polecenia spowoduje wyłączenie to zachowanie, zachowując możliwie oryginalne nazwy bazy danych. Nieprawidłowe identyfikatory .NET nadal zostanie rozwiązany i syntetyzowany nazwy, takie jak właściwości nawigacji nadal będą zgodne z konwencjami nazewnictwa platformy .NET.

## <a name="fluent-api-or-data-annotations"></a>Interfejs API Fluent lub adnotacji danych

Typy jednostek są konfigurowane przy użyciu interfejsu API Fluent domyślnie. Określ `-DataAnnotations` (PMC) lub `--data-annotations` (CLI), aby zamiast tego użyć adnotacji danych, gdy jest to możliwe.

Na przykład za pomocą interfejsu API Fluent będzie tworzenia szkieletu to:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Podczas korzystania z adnotacji danych będzie tworzenia szkieletu to:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nazwa typu DbContext

Nazwa klasy DbContext szkieletu będzie nazwa bazy danych z sufiksem *kontekstu* domyślnie. Aby określić inne konto, użyj `-Context` w konsoli zarządzania Pakietami i `--context` w interfejsie wiersza polecenia.

## <a name="directories-and-namespaces"></a>Katalogi i przestrzenie nazw

Klas jednostek i klasy DbContext są szkieletu do katalogu głównego projektu i użyj projektu domyślny obszar nazw. Można określić katalog, w przypadku, gdy klasy są szkieletu za pomocą `-OutputDir` (PMC) lub `--output-dir` (CLI). Przestrzeń nazw będzie głównej przestrzeni nazw, a także nazw podkatalogów w katalogu głównym projektu.

Można również użyć `-ContextDir` (PMC) i `--context-dir` (CLI) do tworzenia szkieletu klasy DbContext w oddzielnym katalogu z klasami typu jednostki.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Jak to działa

Odtwarzanie rozpoczyna się od odczytu schemat bazy danych. Odczytuje informacje o tabele, kolumny, ograniczenia i indeksy.

Następnie używa informacji o schemacie, aby utworzyć model programu EF Core. Tabele są używane do tworzenia typów jednostek. kolumny są używane do tworzenia właściwości; i kluczy obcych są używane do tworzenia relacji.

Na koniec model jest używany do generowania kodu. Odpowiednie jednostki typu klasy, interfejsu API Fluent i danych adnotacje są szkieletu Aby ponownie utworzyć ten sam model aplikacji.

## <a name="what-doesnt-work"></a>Co nie działa

Nie wszystkie informacje o modelu może być reprezentowany za pomocą schematu bazy danych. Na przykład informacje o **hierarchii dziedziczenia**, **należące do typów**, i **tabeli podział** nie są obecne w schemacie bazy danych. W związku z tym te konstrukcje będzie nigdy nie być odtwarzane.

Ponadto **niektóre typy kolumn** nie mogą być obsługiwane przez dostawcę programu EF Core. Kolumny te nie zostaną uwzględnione w modelu.

EF Core wymaga klucza każdego typu jednostki. Tabele, jednak nie są wymagane do określenia klucza podstawowego. **Tabele bez klucza podstawowego** obecnie nie są odtwarzane.

Można zdefiniować **tokeny współbieżności** w modelu platformy EF Core uniemożliwić dwa użytkownikom aktualizowanie tej samej jednostki w tym samym czasie. Niektóre bazy danych ma specjalny typ do reprezentowania kolumna (na przykład rowversion w programie SQL Server) tego typu w takiej sytuacji firma Microsoft może wycofać inżynier te informacje; jednak inne tokeny współbieżności będzie nie być odtwarzane.

## <a name="customizing-the-model"></a>Dostosowywanie modelu

Kod wygenerowany przez platformę EF Core to Twój kod. Możesz go zmienić. Zostaną tylko wygenerowane, jeśli możesz odtworzyć tego samego modelu ponownie. Reprezentuje utworzony szkielet kodu *jeden* modelu, który może służyć do uzyskania dostępu do bazy danych, ale na pewno nie jest *tylko* modelu, który może służyć.

Dostosuj klasy typów jednostek i klasy DbContext odpowiednio do potrzeb. Na przykład można zmienić nazwy właściwości i typy, wprowadzają hierarchii dziedziczenia lub dzielenie tabeli do wielu jednostek. Indeksy unikatowe, nieużywane sekwencji i właściwości nawigacji, opcjonalne właściwości skalarne i nazwy ograniczenia mogą również usuwać z modelu.

Można również dodać dodatkowe konstruktorów, metod, właściwości itd. w oddzielnym pliku, przy użyciu innej klasy częściowej. To podejście sprawdza się nawet wtedy, gdy zamierzasz odtwarzać modelu ponownie.

## <a name="updating-the-model"></a>Aktualizowanie modelu

Po wprowadzeniu zmian w bazie danych, może być konieczne zaktualizowanie modelu platformy EF Core w celu odzwierciedlenia tych zmian. W przypadku prostych zmian w bazie danych może być łatwiej tylko ręcznie wprowadź zmiany w modelu platformy EF Core. Na przykład zmiana nazwy tabeli lub kolumny, usunięcie kolumny lub aktualizowanie typ kolumny są proste zmiany w kodzie.

Najbardziej znaczące zmiany nie są jednak jako łatwe upewnij ręcznie. Jeden typowy przepływ pracy jest, aby odwrócić inżynier modelu z bazy danych ponownie, używając `-Force` (PMC) lub `--force` (CLI) w celu zastąpienia istniejącego modelu jeden z zaktualizowane.

Kolejną funkcją najczęściej żądanych jest możliwość aktualizacji modelu z bazy danych przy jednoczesnym zachowaniu dostosowywania, takich jak zmiana nazwy, typu hierarchie itp. Użyj problem [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) do śledzenia postępu dla tej funkcji.

> [!WARNING]
> Jeśli odtworzeniu modelu z bazy danych ponownie, wszelkie zmiany wprowadzone w plikach zostaną utracone.
