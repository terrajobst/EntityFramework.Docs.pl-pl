---
title: "Uaktualnianie z programu EF Core 1.0 RC1 do RC2 — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Uaktualnianie z EF Core 1.0 RC1 do 1.0 RC2

Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC1 do RC2.

## <a name="package-names-and-versions"></a>Pakiet nazwy i wersje

Między RC1 i RC2 możemy zmieniony z "Programu Entity Framework 7" na "Entity Framework Core". Możesz przeczytać dodatkowe informacje ze względu na zmianę w [to post, w którym Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Z powodu tej zmiany nazwy naszych pakietu zmieniła się z `EntityFramework.*` do `Microsoft.EntityFrameworkCore.*` i naszych w wersjach od `7.0.0-rc1-final` do `1.0.0-rc2-final` (lub `1.0.0-preview1-final` dla narzędzi).

**Konieczne będzie całkowicie Usuń pakiety RC1, a następnie zainstaluj RC2 te.** Oto mapowania dla niektórych typowych pakietów.

| Pakiet RC1                                               | Odpowiednik RC2                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres<to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Nie są jeszcze dostępne dla RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Wraz z nazwy pakietu, przestrzenie nazw zmieniła się z `Microsoft.Data.Entity.*` do `Microsoft.EntityFrameworkCore.*`. Może obsłużyć tej zmiany z Znajdź/Zamień z `using Microsoft.Data.Entity` z `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabela zmian konwencji nazewnictwa

Znaczące zmiany funkcjonalne Wybraliśmy w RC2 było użyć nazwy `DbSet<TEntity>` właściwości dla danej jednostki jako nazwy tabeli mapowania, a nie tylko nazwę klasy. Więcej o tej zmianie w [problem pokrewne anonsu](https://github.com/aspnet/Announcements/issues/167).

Istniejące aplikacje RC1, zaleca się dodanie poniższego kodu do uruchomienia programu `OnModelCreating` Metoda aktualizowania strategia nazewnictwa RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Jeśli chcesz przyjąć nowy strategia nazewnictwa może zalecamy pomyślnie Kończenie pozostałe kroki uaktualniania, a następnie usunięcie kodu i tworzenia migrację, aby zastosować tabelę zmienia nazwę.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs zmian (tylko dla projektów platformy ASP.NET Core)

W RC1, trzeba było dodać usługi programu Entity Framework dla dostawcy usługi aplikacji — w `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2, można usunąć wywołań `AddEntityFramework()`, `AddSqlServer()`itp.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Należy również Dodaj Konstruktor do pochodnej kontekstu, która przyjmuje opcje kontekstu i przekazuje je do podstawowego konstruktora. Jest to niezbędne, ponieważ firma Microsoft usunęła niektóre magic przerażenie, który je w snuck w tle:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>W interfejsie IServiceProvider może być przekazywany

Kod RC1, która przekazuje `IServiceProvider` do kontekstu, to teraz przeniesiono do `DbContextOptions`, a parametr oddzielne konstruktora. Użyj `DbContextOptionsBuilder.UseInternalServiceProvider(...)` można ustawić dostawcy usług.

### <a name="testing"></a>Testowanie

Najbardziej typowym scenariuszem dla tej czynności było kontrolowanie zakresu InMemory bazy danych podczas testowania. Zobacz zaktualizowanego [testowanie](testing/index.md) artykule przykład temu RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Rozpoznawanie wewnętrznych usług od dostawcy usług aplikacji (tylko dla projektów platformy ASP.NET Core)

Jeśli korzystasz z aplikacji platformy ASP.NET Core i chcesz EF do rozpoznania usług wewnętrznego od dostawcy usług aplikacji, jest przeciążenie metody `AddDbContext` która pozwala na skonfigurowanie to:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Firma Microsoft zaleca stosowanie EF wewnętrznie Zarządzanie usługach, chyba że przyczyny, aby połączyć wewnętrznych usług EF w aplikacji usługodawcy. Głównym celem się, że chcesz to zrobić jest na potrzeby Zastąp usług, które jest używane wewnętrznie EF dostawcą usług aplikacji

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Polecenia DNX = > .NET interfejsu wiersza polecenia (tylko dla projektów platformy ASP.NET Core)

Jeśli poprzednio korzystano `dnx ef` poleceń dla projektów ASP.NET 5, te zostały przeniesione do `dotnet ef` poleceń. Taka sama składnia polecenia nadal istnieje. Można użyć `dotnet ef --help` dla informacji o składni.

Sposób zarejestrowanych poleceń została zmieniona w RC2 z powodu DNX zastępowane przez .NET interfejsu wiersza polecenia. Polecenia są obecnie zarejestrowane w `tools` sekcji `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Jeśli używasz programu Visual Studio, można teraz używać konsoli Menedżera pakietów do uruchomienia poleceń EF dla projektów platformy ASP.NET Core (to nie jest obsługiwana w RC1). Nadal musisz zarejestrować polecenia w `tools` sekcji `project.json` w tym celu.

## <a name="package-manager-commands-require-powershell-5"></a>Menedżer pakietów polecenia wymagają programu PowerShell 5

Użycie poleceń programu Entity Framework w konsoli Menedżera pakietów w programie Visual Studio, następnie należy upewnić się, że zainstalowana 5 programu PowerShell. Jest to wymagane tymczasowe, które zostaną usunięte w następnej wersji (zobacz [wystawiać #5327](https://github.com/aspnet/EntityFramework/issues/5327) więcej szczegółów).

## <a name="using-imports-in-projectjson"></a>Za pomocą "imports" w pliku project.json

Część EF Core zależności nie obsługują .NET Standard jeszcze. Podstawowe EF w projektach platformy .NET Standard i .NET Core mogą wymagać dodawanie "imports" do pliku project.json tymczasowo.

Podczas dodawania EF, przywracanie NuGet spowoduje wyświetlenie tego komunikatu o błędzie:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

Obejście jest ręcznie zaimportować profil przenośnych "portable net451 + win8". To wymusza NuGet traktować to pliki binarne spełniające to Podaj jako zgodne framework przy użyciu platformy .NET Standard, nawet jeśli nie są one. Mimo że "portable net451 + win8" nie jest zgodny z .NET Standard 100%, jest zgodna wystarczająco przejście od PCL do .NET Standard. Importy można usunąć zależności w EF po pewnym czasie uaktualnić do wersji .NET Standard.

Wiele platform można dodać do "imports" w składni tablicy. Inne importów może być konieczne, jeśli Dodawanie dodatkowych bibliotek do projektu.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Zobacz [wystawiać #5176](https://github.com/aspnet/EntityFramework/issues/5176).
