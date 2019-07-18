---
title: Uaktualnianie z poprzednich wersji do EF Core 2 — EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 1222f10811914f65822a49e18522c287ece12174
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306500"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uaktualnianie aplikacji z poprzednich wersji do EF Core 2,0

Wprowadziliśmy okazję do znacznego zawężenia istniejących interfejsów API i zachowań w 2,0. Istnieje kilka ulepszeń, które mogą wymagać modyfikacji istniejącego kodu aplikacji, chociaż uważamy, że w przypadku większości aplikacji wpływ będzie niski, w większości przypadków konieczna jest tylko ponowna kompilacja i minimalne zmiany z uwzględnieniem nieaktualnych interfejsów API.

Aktualizacja istniejącej aplikacji do EF Core 2,0 może wymagać:

1. Uaktualnianie docelowej implementacji platformy .NET aplikacji do jednej, która obsługuje .NET Standard 2,0. Aby uzyskać więcej informacji, zobacz [obsługiwane implementacje platformy .NET](../platforms/index.md) .

2. Zidentyfikuj dostawcę docelowej bazy danych, która jest zgodna z EF Core 2,0. Zobacz [EF Core 2,0 wymaga dostawcy bazy danych 2,0](#ef-core-20-requires-a-20-database-provider) poniżej.

3. Uaktualnianie wszystkich pakietów EF Core (środowisko uruchomieniowe i narzędzia) do 2,0. Aby uzyskać więcej informacji, zobacz artykuł [instalowanie EF Core](../get-started/install/index.md) .

4. Wprowadź wszelkie niezbędne zmiany w kodzie, aby wyrównać zmiany, które zostały opisane w pozostałej części tego dokumentu.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core teraz zawiera EF Core

Aplikacje ukierunkowane na ASP.NET Core 2,0 mogą używać EF Core 2,0 bez dodatkowych zależności poza dostawcami baz danych innych firm. Jednak aplikacje zgodne z poprzednimi wersjami ASP.NET Core muszą zostać uaktualnione do ASP.NET Core 2,0, aby można było używać EF Core 2,0. Aby uzyskać więcej informacji na temat uaktualniania aplikacji ASP.NET Core do wersji 2,0, zobacz dokumentację dotyczącą [ASP.NET Core w temacie](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Nowy sposób pobierania usług aplikacji w ASP.NET Core

Zalecany wzorzec dla ASP.NET Core aplikacji sieci Web został zaktualizowany dla 2,0 w taki sposób, że EF Core logiki czasu projektowania używane w 1. x. Wcześniej w czasie projektowania EF Core próbował wywołać `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostępu do dostawcy usług aplikacji. W ASP.NET Core 2,0 konfiguracja jest inicjowana poza `Startup` klasą. Aplikacje korzystające z EF Core zwykle uzyskują dostęp do parametrów połączenia `Startup` z konfiguracji, więc nie są już wystarczające. W przypadku uaktualniania aplikacji ASP.NET Core 1. x podczas korzystania z narzędzi EF Core może zostać wyświetlony następujący błąd.

> Nie znaleziono konstruktora bez parametrów w "ApplicationContext". Dodaj Konstruktor bez parametrów do elementu "ApplicationContext" lub Dodaj implementację elementu "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" w tym samym zestawie co "ApplicationContext"

Nowy punkt zaczepienia czasu projektowania został dodany w domyślnym szablonie ASP.NET Core 2.0. Metoda statyczna `Program.BuildWebHost` umożliwia EF Core dostęp do dostawcy usług aplikacji w czasie projektowania. W przypadku uaktualniania aplikacji ASP.NET Core 1. x należy zaktualizować `Program` klasę, aby była podobna do następującej.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

Przyjęcie tego nowego wzorca w przypadku aktualizowania aplikacji do 2,0 jest zdecydowanie zalecane i jest wymagane w celu zapewnienia funkcji produktu, takich jak Entity Framework Core migracji. Innym typowym rozwiązaniem jest [wdrożenie *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory zmieniono nazwę

W celu zapewnienia obsługi różnorodnych wzorców aplikacji i zapewnienia użytkownikom większej kontroli nad sposobem `DbContext` ich użycia w czasie projektowania mamy w przeszłości, z `IDbContextFactory<TContext>` udostępnieniem interfejsu. W czasie projektowania narzędzia EF Core będą wykrywać implementacje tego interfejsu w projekcie i używać ich do tworzenia `DbContext` obiektów.

Ten interfejs miał bardzo ogólną nazwę, która wprowadza niektórym użytkownikom próbę ponownego użycia dla innych `DbContext`scenariuszy. Zostały one przechwycone, gdy narzędzia EF próbują użyć ich implementacji w czasie projektowania i powodowały niepowodzenia poleceń takich jak `Update-Database` lub `dotnet ef database update` .

Aby można było przekazać silną semantykę czasu projektowania tego interfejsu, nazwa została zmieniona na `IDesignTimeDbContextFactory<TContext>`.

W przypadku wersji 2,0 nadal `IDbContextFactory<TContext>` istnieje, ale jest oznaczony jako przestarzały.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions usunięte

Ze względu na opisane powyżej `DbContextFactoryOptions` ASP.NET Core 2,0 zmiany nie są już potrzebne w nowym `IDesignTimeDbContextFactory<TContext>` interfejsie. Poniżej znajdują się alternatywy, których należy użyć zamiast tego.

| DbContextFactoryOptions | Różne                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Katalog roboczy czasu projektowania został zmieniony

Zmiany w ASP.NET Core 2,0 także wymagały katalogu roboczego używanego przez `dotnet ef` program do dopasowania z katalogiem roboczym używanym w programie Visual Studio podczas uruchamiania aplikacji. Jednym z zauważalnych efektów ubocznych tego jest to, że nazwy plików oprogramowania SQLite są teraz względne dla katalogu projektu, a nie do katalogu wyjściowego, tak jak w przypadku programu.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 wymaga dostawcy bazy danych 2,0

W przypadku EF Core 2,0 wprowadziliśmy wiele uproszczeń i ulepszeń w sposobie działania dostawców baz danych. Oznacza to, że dostawcy 1.0. x i 1.1. x nie będą współpracować z EF Core 2,0.

Dostawcy usług SQL Server i SQLite są dostarczani przez zespół EF i wersje 2,0 będą dostępne w ramach wersji 2,0. Dostawcy Open-Source innych firm dla [programu SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)i [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) są aktualizowane dla 2,0. W przypadku wszystkich innych dostawców skontaktuj się z modułem zapisującym dostawcy.

## <a name="logging-and-diagnostics-events-have-changed"></a>Zdarzenia rejestrowania i diagnostyki zostały zmienione

Uwaga: te zmiany nie powinny mieć wpływu na większość kodu aplikacji.

Identyfikatory zdarzeń dla komunikatów wysyłanych do [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) uległy zmianie w 2,0. Identyfikatory zdarzeń są teraz unikatowe w EF Core kodzie. Te komunikaty są teraz zgodne ze standardowym wzorcem rejestrowania strukturalnego używanego przez program, na przykład MVC.

Zmieniono także kategorie rejestratora. Istnieje teraz dobrze znany zestaw kategorii, do których można uzyskać dostęp za pomocą [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Zdarzenia [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) teraz używają tych samych nazw identyfikatorów zdarzeń co odpowiadające `ILogger` im komunikaty. Ładunki zdarzeń to wszystkie typy nominalne pochodne od [EVENTDATA](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Identyfikatory zdarzeń, typy ładunku i kategorie są udokumentowane w klasach [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) i [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) .

Identyfikatory zostały również przeniesione z Microsoft. EntityFrameworkCore. Infrastructure do nowej przestrzeni nazw Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core zmiany relacyjnego interfejsu API metadanych

EF Core 2,0 będzie teraz kompilować inne [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego używanego dostawcy. Jest to zazwyczaj przezroczyste dla aplikacji. Ułatwia to uproszczenie interfejsów API metadanych niższego poziomu, takich jak każdy dostęp do _wspólnych koncepcji metadanych relacyjnych_ jest zawsze realizowany przez wywołanie `.Relational` zamiast `.SqlServer`, `.Sqlite`itp. Na przykład kod 1.1. x podobny do tego:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Powinien zostać teraz zapisany w następujący sposób:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Zamiast korzystać z metod, `ForSqlServerToTable`takich jak metody rozszerzające, są teraz dostępne do pisania kodu warunkowego na podstawie bieżącego dostawcy w użyciu. Na przykład:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Należy zauważyć, że ta zmiana dotyczy tylko interfejsów API/metadanych, które są zdefiniowane dla _wszystkich_ dostawców relacyjnych. Interfejs API i metadane pozostają takie same, jeśli są specyficzne tylko dla jednego dostawcy. Na przykład indeksy klastrowane są specyficzne dla programu SQL Server `ForSqlServerIsClustered` i `.SqlServer().IsClustered()` nadal muszą być używane.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Nie Przejmij kontroli nad dostawcą usług EF

EF Core używa wewnętrznego `IServiceProvider` (kontenera iniekcji zależności) do jego wewnętrznej implementacji. Aplikacje powinny zezwalać EF Core na tworzenie tego dostawcy i zarządzanie nim, z wyjątkiem przypadków specjalnych. Zdecydowanie należy rozważyć usunięcie wszelkich wywołań `UseInternalServiceProvider`do programu. Jeśli aplikacja wymaga wywołania `UseInternalServiceProvider`, rozważ [zgłoszenie problemu](https://github.com/aspnet/EntityFramework/Issues) , aby można było zbadać inne sposoby obsługi Twojego scenariusza.

`AddEntityFramework`Wywoływanie `AddEntityFrameworkSqlServer`, itp. nie jest wymagane przez kod aplikacji, `UseInternalServiceProvider` chyba że jest również wywoływana. Usuń wszystkie istniejące wywołania do `AddEntityFramework` lub `AddEntityFrameworkSqlServer`, itp. `AddDbContext` , powinny być nadal używane w taki sam sposób jak wcześniej.

## <a name="in-memory-databases-must-be-named"></a>Bazy danych w pamięci muszą mieć nazwę

Globalna nienazwana baza danych w pamięci została usunięta, a w zamian wszystkie bazy danych w pamięci muszą mieć nazwę. Przykład:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Spowoduje to utworzenie/użycie bazy danych o nazwie "Moja baza danych". Jeśli `UseInMemoryDatabase` zostanie wywołane ponownie o tej samej nazwie, zostanie użyta taka sama baza danych w pamięci, która będzie mogła być współużytkowana przez wiele wystąpień kontekstu.

## <a name="read-only-api-changes"></a>Zmiany w interfejsie API tylko do odczytu

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, i `IsStoreGeneratedAlways` są przestarzałe i zastąpione atrybutami [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) i [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Te zachowania mają zastosowanie do dowolnej właściwości (nie tylko właściwości generowane przez magazyn) i określają, w jaki sposób wartość właściwości ma być używana podczas wstawiania do wiersza bazy danych`BeforeSaveBehavior`() lub podczas aktualizowania istniejącego wiersza bazy danych`AfterSaveBehavior`().

Właściwości oznaczone jako [ValueGenerated. OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (na przykład w przypadku kolumn obliczanych) domyślnie ignorują każdą wartość ustawioną obecnie we właściwości. Oznacza to, że wygenerowana przez magazyn wartość zawsze zostanie uzyskana niezależnie od tego, czy dla śledzonej jednostki została ustawiona lub zmodyfikowana wartość. Można to zmienić, ustawiając inną `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nowe zachowanie dotyczące usuwania ClientSetNull

W poprzednich wersjach [DeleteBehavior. ograniczał](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) zachowanie dla jednostek śledzonych przez kontekst, który jest bardziej zamknięty `SetNull` . W EF Core 2,0 nowe `ClientSetNull` zachowanie zostało wprowadzone jako domyślne dla relacji opcjonalnych. To zachowanie ma `SetNull` semantykę dla śledzonych jednostek `Restrict` i zachowań dla baz danych utworzonych przy użyciu EF Core. W naszym środowisku są to najbardziej oczekiwane/przydatne zachowania dla śledzonych jednostek i bazy danych. `DeleteBehavior.Restrict`jest teraz uznawany za śledzone jednostki po ustawieniu dla relacji opcjonalnych.

## <a name="provider-design-time-packages-removed"></a>Usunięto pakiety czasu projektowania dostawcy

`Microsoft.EntityFrameworkCore.Relational.Design` Pakiet został usunięty. Zawartość została skonsolidowana w `Microsoft.EntityFrameworkCore.Relational` i. `Microsoft.EntityFrameworkCore.Design`

To propaguje do pakietów czasu projektowania dostawcy. Pakiety te (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`itp.) zostały usunięte i ich zawartość jest konsolidowana do głównych pakietów dostawcy.

Aby włączyć `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold` w EF Core 2,0, należy odwołać się tylko do pojedynczego pakietu dostawcy:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
