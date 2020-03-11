---
title: Uaktualnianie z poprzednich wersji do EF Core 2 — EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416757"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uaktualnianie aplikacji z poprzednich wersji do EF Core 2,0

Wprowadziliśmy okazję do znacznego zawężenia istniejących interfejsów API i zachowań w 2,0. Istnieje kilka ulepszeń, które mogą wymagać modyfikacji istniejącego kodu aplikacji, chociaż uważamy, że w przypadku większości aplikacji wpływ będzie niski, w większości przypadków konieczna jest tylko ponowna kompilacja i minimalne zmiany z uwzględnieniem nieaktualnych interfejsów API.

Aktualizacja istniejącej aplikacji do EF Core 2,0 może wymagać:

1. Uaktualnianie docelowej implementacji platformy .NET aplikacji do jednej, która obsługuje .NET Standard 2,0. Aby uzyskać więcej informacji, zobacz [obsługiwane implementacje platformy .NET](xref:core/platforms/index) .

2. Zidentyfikuj dostawcę docelowej bazy danych, która jest zgodna z EF Core 2,0. Zobacz [EF Core 2,0 wymaga dostawcy bazy danych 2,0](#ef-core-20-requires-a-20-database-provider) poniżej.

3. Uaktualnianie wszystkich pakietów EF Core (środowisko uruchomieniowe i narzędzia) do 2,0. Aby uzyskać więcej informacji, zobacz artykuł [instalowanie EF Core](xref:core/get-started/install/index) .

4. Wprowadź wszelkie niezbędne zmiany w kodzie, aby wyrównać zmiany, które zostały opisane w pozostałej części tego dokumentu.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core teraz zawiera EF Core

Aplikacje ukierunkowane na ASP.NET Core 2,0 mogą używać EF Core 2,0 bez dodatkowych zależności poza dostawcami baz danych innych firm. Jednak aplikacje zgodne z poprzednimi wersjami ASP.NET Core muszą zostać uaktualnione do ASP.NET Core 2,0, aby można było używać EF Core 2,0. Aby uzyskać więcej informacji na temat uaktualniania aplikacji ASP.NET Core do wersji 2,0, zobacz dokumentację dotyczącą [ASP.NET Core w temacie](/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Nowy sposób pobierania usług aplikacji w ASP.NET Core

Zalecany wzorzec dla ASP.NET Core aplikacji sieci Web został zaktualizowany dla 2,0 w taki sposób, że EF Core logiki czasu projektowania używane w 1. x. Wcześniej w czasie projektowania EF Core próbował wywołać `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostępu do dostawcy usług aplikacji. W ASP.NET Core 2,0 konfiguracja jest inicjowana poza klasą `Startup`. Aplikacje korzystające z EF Core zwykle uzyskują dostęp do swoich parametrów połączenia z konfiguracji, więc `Startup` przez siebie nie są już wystarczające. W przypadku uaktualniania aplikacji ASP.NET Core 1. x podczas korzystania z narzędzi EF Core może zostać wyświetlony następujący błąd.

> Nie znaleziono konstruktora bez parametrów w "ApplicationContext". Dodaj Konstruktor bez parametrów do elementu "ApplicationContext" lub Dodaj implementację elementu "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" w tym samym zestawie co "ApplicationContext"

Nowy punkt zaczepienia czasu projektowania został dodany w domyślnym szablonie ASP.NET Core 2.0. Metoda static `Program.BuildWebHost` umożliwia EF Core dostęp do dostawcy usług aplikacji w czasie projektowania. W przypadku uaktualniania aplikacji ASP.NET Core 1. x należy zaktualizować klasę `Program`, aby była podobna do następującej.

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

Przyjęcie tego nowego wzorca w przypadku aktualizowania aplikacji do 2,0 jest zdecydowanie zalecane i jest wymagane w celu zapewnienia funkcji produktu, takich jak Entity Framework Core migracji. Inną typową alternatywą jest [implementacja *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory zmieniono nazwę

W celu zapewnienia obsługi różnorodnych wzorców aplikacji i zapewnienia użytkownikom większej kontroli nad sposobem korzystania z ich `DbContext` w czasie projektowania mamy w przeszłości podano interfejs `IDbContextFactory<TContext>`. W czasie projektowania narzędzia EF Core będą wykrywać implementacje tego interfejsu w projekcie i używać ich do tworzenia obiektów `DbContext`.

Ten interfejs miał bardzo ogólną nazwę, która wprowadza niektórym użytkownikom próbę ponownego użycia dla innych scenariuszy tworzenia `DbContext`. Zostały one przechwycone, gdy narzędzia EF próbują użyć ich implementacji w czasie projektowania i powodowały niepowodzenie poleceń takich jak `Update-Database` lub `dotnet ef database update`.

Aby można było przekazać silną semantykę czasu projektowania tego interfejsu, zmieniono jego nazwę na `IDesignTimeDbContextFactory<TContext>`.

W przypadku wersji 2,0 `IDbContextFactory<TContext>` nadal istnieje, ale jest oznaczony jako przestarzały.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions usunięte

Ze względu na opisane powyżej ASP.NET Core 2,0 zmiany w nowym interfejsie `IDesignTimeDbContextFactory<TContext>` nie `DbContextFactoryOptions` już potrzebne. Poniżej znajdują się alternatywy, których należy użyć zamiast tego.

| DbContextFactoryOptions | Różne                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Katalog roboczy czasu projektowania został zmieniony

Zmiany ASP.NET Core 2,0 także wymagały katalogu roboczego używanego przez `dotnet ef` do wyrównania z katalogiem roboczym używanym przez program Visual Studio podczas uruchamiania aplikacji. Jednym z zauważalnych efektów ubocznych tego jest to, że nazwy plików oprogramowania SQLite są teraz względne dla katalogu projektu, a nie do katalogu wyjściowego, tak jak w przypadku programu.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 wymaga dostawcy bazy danych 2,0

W przypadku EF Core 2,0 wprowadziliśmy wiele uproszczeń i ulepszeń w sposobie działania dostawców baz danych. Oznacza to, że dostawcy 1.0. x i 1.1. x nie będą współpracować z EF Core 2,0.

Dostawcy usług SQL Server i SQLite są dostarczani przez zespół EF i wersje 2,0 będą dostępne w ramach wersji 2,0. Dostawcy Open-Source innych firm dla [programu SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)i [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) są aktualizowane dla 2,0. W przypadku wszystkich innych dostawców skontaktuj się z modułem zapisującym dostawcy.

## <a name="logging-and-diagnostics-events-have-changed"></a>Zdarzenia rejestrowania i diagnostyki zostały zmienione

Uwaga: te zmiany nie powinny mieć wpływu na większość kodu aplikacji.

Identyfikatory zdarzeń dla komunikatów wysyłanych do [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) uległy zmianie w 2,0. Identyfikatory zdarzeń są teraz unikatowe w EF Core kodzie. Te komunikaty są teraz zgodne ze standardowym wzorcem rejestrowania strukturalnego używanego przez program, na przykład MVC.

Zmieniono także kategorie rejestratora. Istnieje teraz dobrze znany zestaw kategorii, do których można uzyskać dostęp za pomocą [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

Zdarzenia [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) teraz używają tych samych nazw identyfikatorów zdarzeń co odpowiadające im komunikaty `ILogger`. Ładunki zdarzeń to wszystkie typy nominalne pochodne od [EVENTDATA](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

Identyfikatory zdarzeń, typy ładunku i kategorie są udokumentowane w klasach [CoreEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) i [RelationalEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) .

Identyfikatory zostały również przeniesione z Microsoft. EntityFrameworkCore. Infrastructure do nowej przestrzeni nazw Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core zmiany relacyjnego interfejsu API metadanych

EF Core 2,0 będzie teraz kompilować inne [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) dla każdego używanego dostawcy. Jest to zazwyczaj przezroczyste dla aplikacji. Ułatwia to uproszczenie interfejsów API metadanych niższego poziomu, takich jak każdy dostęp do _wspólnych koncepcji metadanych relacyjnych_ jest zawsze realizowany przez wywołanie `.Relational` zamiast `.SqlServer`, `.Sqlite`itd. Na przykład kod 1.1. x podobny do tego:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Powinien zostać teraz zapisany w następujący sposób:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Zamiast korzystać z metod takich jak `ForSqlServerToTable`, metody rozszerzające są teraz dostępne do pisania kodu warunkowego na podstawie bieżącego dostawcy w użyciu. Na przykład:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Należy zauważyć, że ta zmiana dotyczy tylko interfejsów API/metadanych, które są zdefiniowane dla _wszystkich_ dostawców relacyjnych. Interfejs API i metadane pozostają takie same, jeśli są specyficzne tylko dla jednego dostawcy. Na przykład indeksy klastrowane są specyficzne dla programu SQL Server, więc `ForSqlServerIsClustered` i `.SqlServer().IsClustered()` nadal muszą być używane.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Nie Przejmij kontroli nad dostawcą usług EF

EF Core używa wewnętrznego `IServiceProvider` (kontenera iniekcji zależności) dla jego wewnętrznej implementacji. Aplikacje powinny zezwalać EF Core na tworzenie tego dostawcy i zarządzanie nim, z wyjątkiem przypadków specjalnych. Zdecydowanie należy rozważyć usunięcie wszelkich wywołań do `UseInternalServiceProvider`. Jeśli aplikacja wymaga wywołania `UseInternalServiceProvider`, należy rozważyć [zgłoszenie problemu](https://github.com/aspnet/EntityFramework/Issues) , aby można było zbadać inne sposoby obsługi Twojego scenariusza.

Wywoływanie `AddEntityFramework`, `AddEntityFrameworkSqlServer`itp. nie jest wymagane przez kod aplikacji, chyba że `UseInternalServiceProvider` jest również wywoływana. Usuń wszystkie istniejące wywołania do `AddEntityFramework` lub `AddEntityFrameworkSqlServer`itp. `AddDbContext` nadal powinny być używane w taki sam sposób jak wcześniej.

## <a name="in-memory-databases-must-be-named"></a>Bazy danych w pamięci muszą mieć nazwę

Globalna nienazwana baza danych w pamięci została usunięta, a w zamian wszystkie bazy danych w pamięci muszą mieć nazwę. Na przykład:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Spowoduje to utworzenie/użycie bazy danych o nazwie "Moja baza danych". Jeśli `UseInMemoryDatabase` jest ponownie wywoływana o tej samej nazwie, zostanie użyta taka sama baza danych w pamięci, która będzie mogła być współużytkowana przez wiele wystąpień kontekstu.

## <a name="read-only-api-changes"></a>Zmiany w interfejsie API tylko do odczytu

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`i `IsStoreGeneratedAlways` zostały przestarzałe i zastąpione atrybutami [BeforeSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) i [AfterSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Te zachowania mają zastosowanie do dowolnej właściwości (nie tylko właściwości generowane przez magazyn) i określają, w jaki sposób wartość właściwości ma być używana podczas wstawiania do wiersza bazy danych (`BeforeSaveBehavior`) lub podczas aktualizowania istniejącego wiersza bazy danych (`AfterSaveBehavior`).

Właściwości oznaczone jako [ValueGenerated. OnAddOrUpdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (na przykład w przypadku kolumn obliczanych) domyślnie ignorują każdą wartość ustawioną obecnie we właściwości. Oznacza to, że wygenerowana przez magazyn wartość zawsze zostanie uzyskana niezależnie od tego, czy dla śledzonej jednostki została ustawiona lub zmodyfikowana wartość. Można to zmienić, ustawiając inną `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nowe zachowanie dotyczące usuwania ClientSetNull

W poprzednich wersjach [DeleteBehavior. ograniczał](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) zachowanie dla jednostek śledzonych przez kontekst, który został jeszcze zamknięty, `SetNull` semantyki. W EF Core 2,0 wprowadzono nowe zachowanie `ClientSetNull` jako domyślne dla relacji opcjonalnych. To zachowanie ma `SetNull` semantyką dla śledzonych jednostek i zachowania `Restrict` dla baz danych utworzonych przy użyciu EF Core. W naszym środowisku są to najbardziej oczekiwane/przydatne zachowania dla śledzonych jednostek i bazy danych. `DeleteBehavior.Restrict` jest teraz uznawany za śledzone jednostki, gdy ustawiono dla relacji opcjonalnych.

## <a name="provider-design-time-packages-removed"></a>Usunięto pakiety czasu projektowania dostawcy

Pakiet `Microsoft.EntityFrameworkCore.Relational.Design` został usunięty. Zawartość została skonsolidowana w `Microsoft.EntityFrameworkCore.Relational` i `Microsoft.EntityFrameworkCore.Design`.

To propaguje do pakietów czasu projektowania dostawcy. Te pakiety (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`itp.) zostały usunięte i ich zawartość jest konsolidowana do głównych pakietów dostawcy.

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
