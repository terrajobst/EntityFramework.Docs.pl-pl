---
title: Uaktualnianie z poprzedniej wersji do programu EF Core 2 - programu EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: f0d85b3ba22c09d2bd48e8b34ed628a7474322d3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490496"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uaktualnianie aplikacji z poprzedniej wersji do programu EF Core 2.0

Podejmujemy szansy sprzedaży, aby znacznie udoskonalić naszych istniejących interfejsów API i zachowań w wersji 2.0. Istnieje kilka ulepszeń, które mogą wymagać od modyfikowania istniejącego kodu aplikacji, ale uważamy, że dla większości aplikacji wpływ będzie niska, w większości przypadków wymaga podania tylko ponownej kompilacji i minimalnych zmianach z przewodnikiem, aby zastąpić przestarzałe interfejsy API.

## <a name="procedures-common-to-all-applications"></a>Procedury wspólne dla wszystkich aplikacji

Aktualizowanie istniejącej aplikacji do programu EF Core 2.0 mogą wymagać:

1. Uaktualnianie platformy docelowej .NET w aplikacji na taki, który obsługuje .NET Standard 2.0. Zobacz [obsługiwane platformy](../platforms/index.md) Aby uzyskać więcej informacji.

2. Określ dostawcę dla docelowej bazy danych, które są zgodne z programem EF Core 2.0. Zobacz [EF Core 2.0 wymaga dostawcy bazy danych 2.0](#ef-core-20-requires-a-20-database-provider) poniżej.

3. Uaktualnienie wszystkich pakietów programu EF Core (środowiska uruchomieniowego i narzędzi) do wersji 2.0. Zapoznaj się [Instalowanie programu EF Core](../get-started/install/index.md) Aby uzyskać więcej informacji.

4. Wprowadź wszelkie zmiany niezbędny kod w celu kompensacji na temat przełomowych zmian. Zobacz [fundamentalne zmiany](#breaking-changes) sekcji poniżej, aby uzyskać więcej informacji.

## <a name="aspnet-core-applications"></a>Aplikacje platformy ASP.NET Core

1. Zobacz, w szczególności [nowego wzorca do inicjowania dostawcy usług aplikacji](#new-way-of-getting-application-services) opisane poniżej.

> [!TIP]  
> Przyjęcie tego nowego wzorca podczas aktualizowania aplikacji w wersji 2.0 zdecydowanie zaleca się i jest wymagane dla funkcji produktów, takich jak migracje Entity Framework Core pracować. Powszechną alternatywą jest [zaimplementować *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Aplikacje przeznaczone na platformy ASP.NET Core 2.0 mogą używać programu EF Core 2.0 bez dodatkowe zależności oprócz dostawcy bazy danych. Jednak aplikacje przeznaczone na poprzednie wersje platformy ASP.NET Core konieczne uaktualnienie do programu ASP.NET Core 2.0, aby można było używać programu EF Core 2.0. Aby uzyskać więcej informacji na temat uaktualniania aplikacji platformy ASP.NET Core 2.0, zobacz [dokumentacji platformy ASP.NET Core na ten temat](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services"></a>Nowy sposób, w jakim usługi aplikacji

Zaktualizowano zalecany wzorzec dla aplikacji sieci web platformy ASP.NET Core 2.0 w taki sposób, że Przerwano logiki czasu projektowania, jakie programu EF Core w 1.x. Wcześniej w czasie projektowania programu EF Core będzie spróbować ponownie wywołać `Startup.ConfigureServices` bezpośrednio po to, aby uzyskać dostęp do aplikacji dostawcy usług. W programie ASP.NET Core 2.0 konfiguracji jest inicjowany poza `Startup` klasy. Aplikacje zazwyczaj przy użyciu programu EF Core uzyskiwać dostęp do swoich parametrów połączenia z konfiguracji, więc `Startup` przez siebie nie jest już wystarczająca. Jeśli zaktualizujesz aplikację ASP.NET Core 1.x, otrzymasz następujący błąd podczas korzystania z narzędzi programu EF Core.

> Żaden konstruktor bez parametrów nie został odnaleziony w "ApplicationContext". Dodaj konstruktor bez parametrów do "ApplicationContext" lub Dodaj implementację "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" z tego samego zestawu jako "ApplicationContext"

Nowe hook czasu projektowania został dodany w szablonie domyślnego programu ASP.NET Core 2.0. Statyczne `Program.BuildWebHost` metoda umożliwia programu EF Core dostępu do aplikacji usługi dostawcy w czasie projektowania. Jeśli uaktualniasz aplikacji ASP.NET Core 1.x, konieczne będzie można zaktualizować `Program` klasy na podobny do następującego.

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

## <a name="idbcontextfactory-renamed"></a>Zmieniono nazwę IDbContextFactory

W celu obsługi wzorców dla różnych aplikacji i udostępniać użytkownikom większą kontrolę nad jak ich `DbContext` jest używany w czasie projektowania, mamy, w przeszłości, pod warunkiem `IDbContextFactory<TContext>` interfejsu. W czasie projektowania, narzędzi programu EF Core odnajdzie implementacje tego interfejsu w projekcie i użyć go do utworzenia `DbContext` obiektów.

Ten interfejs ma bardzo ogólne nazwy, która wprowadzać w błąd niektórych użytkowników, aby spróbować ponownie go dla innych `DbContext`— tworzenie scenariuszy. Były one przechwytywane wyłączona ochrona, gdy narzędzia EF następnie dotarła do ich wykonania w czasie projektowania i spowodowane poleceń, takich jak `Update-Database` lub `dotnet ef database update` nie powiedzie się.

Aby komunikować się z silną semantyką czasu projektowania tego interfejsu, zmieniono jego `IDesignTimeDbContextFactory<TContext>`.

Dla wersji 2.0 wersji `IDbContextFactory<TContext>` nadal istnieje, ale jest oznaczony jako przestarzały.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions usunięte

Ze względu na zmiany ASP.NET Core 2.0, opisane powyżej, Odkryliśmy, że `DbContextFactoryOptions` został nie będą już potrzebne w nowym `IDesignTimeDbContextFactory<TContext>` interfejsu. Poniżej przedstawiono rozwiązań alternatywnych, z których powinien być używany zamiast tego.

| DbContextFactoryOptions | Alternatywna                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Katalog roboczy czasu projektowania został zmieniony

Katalog roboczy używany przez również wymaganych zmian platformy ASP.NET Core 2.0 `dotnet ef` wyrównać katalog roboczy używany przez program Visual Studio, podczas uruchamiania aplikacji. Jeden dostrzegalnych efekt uboczny tej jest tej bazy danych SQLite nazwy plików są teraz względem katalogu projektu, a nie katalog wyjściowy jak kiedyś.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 wymaga dostawcy 2.0 bazy danych

EF Core 2.0 wprowadzono wiele definiowaniu i ulepszeń w dostawcy baz danych w sposób pracy. Oznacza to, że dostawcy 1.0.x i 1.1.x, nie będą działać przy użyciu programu EF Core 2.0.

Dostawcy programu SQL Server i bazy danych SQLite są dostarczane przez zespół programu EF i wersji 2.0 będzie dostępna w wersji 2.0 w ramach wersji. Dostawcy typu open-source innych firm dla [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), i [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) są aktualizowane dla wersji 2.0. W przypadku innych dostawców, skontaktuj się z modułu zapisującego dostawcy.

## <a name="logging-and-diagnostics-events-have-changed"></a>Zmieniono zdarzenia rejestrowania i diagnostyki

Uwaga: te zmiany nie powinien wpływać na większość kodu aplikacji.

Komunikaty wysyłane do identyfikatorów zdarzeń [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) zostały zmienione w wersji 2.0. Identyfikatory zdarzeń obecnie są unikatowe w obrębie kod programem EF Core. Te komunikaty teraz również wykonać standardowego wzorca dla rejestrowaniem strukturalnym używane przez, na przykład MVC.

Kategorie rejestratora również zostały zmienione. Ma teraz dobrze znanego zestawu kategorii dostępne za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) zdarzenia teraz użyć takich samych nazwach identyfikator zdarzenia odpowiadającego `ILogger` wiadomości. Ładunki zdarzeń są wszystkie typy nominalna pochodną [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Identyfikatory zdarzeń, typy ładunku i kategorie są udokumentowane w [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) i [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) klasy.

Identyfikatory również zostały przeniesione z Microsoft.EntityFrameworkCore.Infraestructure do nowej przestrzeni nazw Microsoft.EntityFrameworkCore.Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core relacyjnych metadanych interfejsu API zmiany

EF Core 2.0 będą teraz tworzyć inną [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego innego dostawcy używane. Jest to zazwyczaj niewidoczna dla aplikacji. Ma to ułatwione uproszczenia metadanych niskiego poziomu interfejsy API, taki sposób, że dowolny dostęp do _typowe pojęcia relacyjnych metadanych_ zawsze jest wykonywane za pomocą wywołania `.Relational` zamiast `.SqlServer`, `.Sqlite`itp. Na przykład 1.1.x kod następująco:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Powinny teraz być zapisany jako:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Zamiast używać metod, takich jak `ForSqlServerToTable`, metody rozszerzające są teraz dostępne do zapisu kod warunkowy, w oparciu o bieżący Dostawca używany. Na przykład:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Należy zauważyć, że ta zmiana ma zastosowanie tylko do interfejsów API/metadanych, który jest zdefiniowany dla _wszystkich_ relacyjnych dostawców. Interfejs API i metadanych pozostaje taki sam, gdy jest ona specyficzne dla tylko jednego dostawcę. Na przykład klastrowanych indeksów są specyficzne dla SQL Server, dzięki czemu `ForSqlServerIsClustered` i `.SqlServer().IsClustered()` nadal muszą być używane.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Nie przejąć kontrolę nad EF dostawcy usług

EF Core używa wewnętrznego `IServiceProvider` (kontener iniekcji zależności) do swojej wewnętrznej implementacji. Aplikacje powinny zezwalać programu EF Core utworzyć i zarządzać tego dostawcy, z wyjątkiem w szczególnych przypadkach. Zdecydowanie rozważyć usunięcie wszelkie wywołania `UseInternalServiceProvider`. Jeśli aplikacja musi wywołać `UseInternalServiceProvider`, należy rozważyć [rejestrując problem](https://github.com/aspnet/EntityFramework/Issues) , dzięki czemu firma Microsoft można zbadać inne sposoby obsługi danego scenariusza.

Wywoływanie `AddEntityFramework`, `AddEntityFrameworkSqlServer`, itp. nie jest wymagane przez kod aplikacji, chyba że `UseInternalServiceProvider` skrót. Usuń wszystkie istniejące wywołania `AddEntityFramework` lub `AddEntityFrameworkSqlServer`itd `AddDbContext` należy nadal używać w taki sam sposób jak wcześniej.

## <a name="in-memory-databases-must-be-named"></a>Musi mieć nazwę bazy danych w pamięci

Globalne bez nazwy bazy danych w pamięci została usunięta, a zamiast tego muszą nosić wszystkich baz danych w pamięci. Na przykład:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

To tworzy/używa bazy danych o nazwie "Moja_baza_danych". Jeśli `UseInMemoryDatabase` wywoływana jest ponownie o takiej samej nazwie tej samej bazy danych w pamięci zostanie użyta, dzięki któremu być współużytkowane przez wiele wystąpień kontekstu.

## <a name="read-only-api-changes"></a>Zmiany interfejsu API tylko do odczytu

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, i `IsStoreGeneratedAlways` zdezaktualizowane i zastąpione [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) i [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Te zachowania mają zastosowanie do wszystkich właściwości (nie tylko generowane przez Magazyn właściwości) i określić, jak wartość właściwości powinna być używana w przypadku wstawiania do wiersza bazy danych (`BeforeSaveBehavior`) lub podczas aktualizowania istniejącej bazy danych wiersza (`AfterSaveBehavior`).

Właściwości są oznaczone jako [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (na przykład w przypadku kolumn obliczanych) domyślnie zignoruje dowolną wartość aktualnie ustawiona we właściwości. Oznacza to, że wygenerowana przez Magazyn wartość, zawsze można uzyskać niezależnie od tego, czy dowolną wartość ma ustawić lub zmodyfikować śledzonych jednostki. Można to zmienić, ustawiając inną `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nowe zachowanie dotyczące usuwania ClientSetNull

W poprzednich wersjach [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) miał zachowanie w przypadku jednostek śledzone przez kontekst, jeden zamknięty dopasowane `SetNull` semantyki. W programie EF Core 2.0 nową `ClientSetNull` zachowanie została wprowadzona jako domyślne dla relacji opcjonalne. To zachowanie ma `SetNull` semantyki śledzonych jednostek i `Restrict` zachowanie dla baz danych utworzonych przy użyciu programu EF Core. W naszych doświadczeń wynika są to najbardziej oczekiwano/przydatne zachowania dla jednostek śledzone i bazy danych. `DeleteBehavior.Restrict` teraz zostanie uznane dla obiektów śledzonych dla relacji opcjonalne.

## <a name="provider-design-time-packages-removed"></a>Dostawca czasu projektowania pakietów usunięte

`Microsoft.EntityFrameworkCore.Relational.Design` Pakiet został usunięty. Jego zawartości zostały skonsolidowane w `Microsoft.EntityFrameworkCore.Relational` i `Microsoft.EntityFrameworkCore.Design`.

Propaguje to do pakietów projektowania dostawcy. Te pakiety (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, itp.) zostały usunięte i ich zawartość skonsolidowane w pakiety główne dostawcy.

Aby włączyć `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold` w programie EF Core 2.0 wystarczy odwołują się do jednego dostawcy pakietu:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
