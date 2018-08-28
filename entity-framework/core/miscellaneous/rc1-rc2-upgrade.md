---
title: Uaktualnianie z programu EF Core 1.0 RC1 do RC2 — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996900"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Uaktualnianie z programu EF Core 1.0 RC1 do wersji 1.0 RC2

Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC1 do RC2.

## <a name="package-names-and-versions"></a>Nazwy pakietów i wersje

Między RC1 i RC2 firma Microsoft zmieniony z "Entity Framework 7" na "Entity Framework Core". Możesz dowiedzieć się więcej o ze względu na zmianę w [ten wpis, Scott hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Ze względu na tę zmianę nazw naszych pakietów zmieniła się z `EntityFramework.*` do `Microsoft.EntityFrameworkCore.*` i nasze w wersjach od `7.0.0-rc1-final` do `1.0.0-rc2-final` (lub `1.0.0-preview1-final` narzędzi).

**Konieczne będzie całkowicie usunąć pakiety RC1, a następnie zainstaluj RC2 te.** Poniżej przedstawiono mapowanie niektóre popularne pakiety innych.

| Pakiet RC1                                               | Odpowiednik RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Nie jest jeszcze dostępna RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Wraz z nazwy pakietu, przestrzenie nazw zmieniła się z `Microsoft.Data.Entity.*` do `Microsoft.EntityFrameworkCore.*`. Może obsłużyć tej zmiany z Znajdź/Zamień z `using Microsoft.Data.Entity` z `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabela zmian konwencji nazewnictwa

Znaczące zmiany funkcjonalne w wersji RC2, Skorzystaliśmy było użyć nazwy `DbSet<TEntity>` właściwości dla danej jednostki, jako nazwę tabeli mapowania, a nie tylko nazwę klasy. Możesz dowiedzieć się więcej o tej zmianie w [problem powiązane ogłoszenie](https://github.com/aspnet/Announcements/issues/167).

W przypadku istniejących aplikacji RC1, firma Microsoft zaleca, dodając następujący kod na początku swoje `OnModelCreating` metodę, aby zachować strategia nazewnictwa RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Jeśli chcesz, który wdrożył nowy strategia nazewnictwa, czy zalecane jest pomyślnie Kończenie pozostałe kroki uaktualnienia, a następnie usunięcie kodu i tworzenia migracji do zastosowania w tabeli zmienia nazwę.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs zmian (tylko w przypadku projektów ASP.NET Core)

W wersji RC1, trzeba było dodać usługi Entity Framework do dostawcy usług w aplikacji — w `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

W wersji RC2, możesz usunąć wywołania `AddEntityFramework()`, `AddSqlServer()`itp.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Musisz również dodać Konstruktor pochodnej kontekst, który przyjmuje opcje kontekstu i przekazuje je do konstruktora podstawowego. Jest to niezbędne, ponieważ firma Microsoft usunęła niektóre scary magic, który snuck je w tle:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Przekazywanie w IServiceProvider.

Jeśli masz kod RC1, który przekazuje `IServiceProvider` kontekst, to teraz przeniesiono do `DbContextOptions`, a nie parametrem oddzielne konstruktora. Użyj `DbContextOptionsBuilder.UseInternalServiceProvider(...)` można ustawić dostawcę usług.

### <a name="testing"></a>Testowanie

Najbardziej typowym scenariuszem tego zrobić był umożliwiają kontrolowanie zakresu InMemory bazy danych, podczas testowania. Zobacz zaktualizowanego [testowania](testing/index.md) artykułu, na przykład w ten sposób za pomocą RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Rozpoznawanie wewnętrznych usług od dostawcy usług w aplikacji (tylko w przypadku projektów ASP.NET Core)

Aplikacja ASP.NET Core, i chcesz EF, aby rozwiązać wewnętrznych usług od dostawcy usług w aplikacji, czy przeciążenia `AddDbContext` pozwala skonfigurować to:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Firma Microsoft zaleca, dzięki czemu EF wewnętrznie Zarządzanie własnych usług, chyba że masz powód, aby połączyć wewnętrznych usług EF w aplikacji dostawcy usługi. Głównym powodem, czy chcesz to zrobić jest Zastąp usług, które jest używane wewnętrznie EF za pomocą dostawcy usługi aplikacji

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Poleceń środowiska DNX = > .NET interfejsu wiersza polecenia (tylko w przypadku projektów ASP.NET Core)

Jeśli wcześniej używano `dnx ef` poleceń dla projektów ASP.NET 5, te zostały przeniesione do `dotnet ef` poleceń. Tej samej składni polecenia nadal obowiązuje ograniczenie. Możesz użyć `dotnet ef --help` Aby uzyskać informacje o składni.

Sposób, w jaki są rejestrowane polecenia został zmieniony w wersji RC2 z powodu środowiska DNX, jest zastępowany przez interfejs wiersza polecenia platformy .NET. Polecenia są teraz zarejestrowane w usłudze `tools` sekcji `project.json`:

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
> Jeśli używasz programu Visual Studio, możesz teraz używać Konsola Menedżera pakietów do uruchamiania poleceń EF dla projektów ASP.NET Core (to zostało nie obsługiwane w wersji RC1). Nadal trzeba zarejestrować poleceń w `tools` części `project.json` w tym celu.

## <a name="package-manager-commands-require-powershell-5"></a>Menedżer pakietów polecenia wymagają programu PowerShell 5

Jeśli używasz poleceń programu Entity Framework w konsoli Menedżera pakietów w programie Visual Studio, następnie należy upewnić się, że masz zainstalowanego 5 programu PowerShell. Jest to wymagane tymczasowe, która zostanie usunięta w następnej wersji (zobacz [wystawiać #5327](https://github.com/aspnet/EntityFramework/issues/5327) Aby uzyskać więcej informacji).

## <a name="using-imports-in-projectjson"></a>Za pomocą "import" w pliku project.json

Niektóre z programem EF Core zależności nie obsługują .NET Standard jeszcze. EF Core w projektach .NET Standard i .NET Core może wymagać, dodając "imports" do pliku project.json jako rozwiązanie tymczasowe.

Podczas dodawania EF, przywracanie pakietów NuGet spowoduje wyświetlenie tego komunikatu o błędzie:

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

Obejście polega na ręcznie zaimportować przenośnej profilu "portable net451 + win8". Tego pakietu NuGet, aby traktować to pliki binarne, które odpowiadają tym wymusza działają jako środowisko zgodne z technologią .NET Standard, nawet jeśli nie są one. Mimo że "portable net451 + win8" nie jest zgodny z .NET Standard w 100%, są one zgodne, przejścia od PCL .NET Standard. Importy można usunąć zależności EF firmy po pewnym czasie uaktualnić do wersji .NET Standard.

Do "import" można dodać wielu platform przy użyciu składni tablicy. Pozostałe Importy może być konieczne, jeśli dodatkowe biblioteki zostaną dodane do projektu.

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
