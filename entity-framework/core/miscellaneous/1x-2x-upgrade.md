---
title: Uaktualnianie z poprzedniej wersji EF 2 rdzeni — rdzenie EF
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 30f4de794d42b1385145286e77c2e7c67987fea6
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678617"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uaktualnianie aplikacji z poprzednich wersji 2.0 Core EF

## <a name="procedures-common-to-all-applications"></a>Procedury wspólne dla wszystkich aplikacji

Aktualizowanie istniejącej aplikacji do EF Core 2.0 mogą wymagać:

1. Uaktualnianie platformy docelowej .NET do wersji obsługującej .NET 2.0 standardowych aplikacji. Zobacz [obsługiwane platformy](../platforms/index.md) więcej szczegółów.

2. Określ dostawcę dla docelowej bazy danych, które są zgodne z EF Core 2.0. Zobacz [EF Core 2.0 wymaga dostawcy bazy danych 2.0](#ef-core-20-requires-a-20-database-provider) poniżej.

3. Uaktualnienie wszystkich pakietów EF Core (środowisko wykonawcze i narzędziami) 2.0. Zapoznaj się [instalowanie Core EF](../get-started/install/index.md) więcej szczegółów.

4. Zmiany kodu niezbędne do kompensacji fundamentalne zmiany. Zobacz [fundamentalne zmiany](#breaking-changes) sekcji poniżej, aby uzyskać więcej informacji.

## <a name="aspnet-core-applications"></a>Aplikacje platformy ASP.NET Core

1. W szczególności zobacz [nowy wzorzec do inicjowania dostawcy usług aplikacji](#new-way-of-getting-application-services) opisane poniżej.

> [!TIP]  
> Przyjęcie ten nowy wzorzec podczas aktualizacji aplikacji 2.0 zdecydowanie zaleca się i jest wymagane dla produktu funkcji, takich jak Entity Framework Core migracji do pracy. Inne typowe alternatywą jest [zaimplementować *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Aplikacji przeznaczonych dla platformy ASP.NET Core 2.0 służy EF Core 2.0 bez dodatkowe zależności oprócz dostawcy bazy danych. Jednak aplikacji na poprzednie wersje platformy ASP.NET Core konieczność uaktualnienia do programu ASP.NET 2.0 Core aby można było używać EF Core 2.0. Więcej informacji na temat uaktualniania aplikacji platformy ASP.NET Core 2.0 można znaleźć [dokumentacji platformy ASP.NET Core w sprawie](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Fundamentalne zmiany

Przekierowaliśmy możliwość znacznie uściślić naszych istniejących interfejsów API i zachowania w 2.0. Istnieje kilka ulepszeń, które mogą wymagać modyfikacji istniejącego kodu aplikacji, mimo że uważamy, że dla większości aplikacji wpływ będzie niskie, w większości przypadków wymagających tylko kompilację i minimalne zmiany z przewodnikiem, aby zamienić przestarzałe interfejsy API.

### <a name="new-way-of-getting-application-services"></a>Nowy sposób pobierania usługi aplikacji

Wzorzec zalecanym dla aplikacji sieci web platformy ASP.NET Core została zaktualizowana 2.0 w taki sposób, aby spowodowało przerwanie EF Core używane w 1.x logiki czasu projektowania. Wcześniej w czasie projektowania, EF Core może spróbować wywołania `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostępu do dostawcy usług aplikacji. W programie ASP.NET 2.0 Core, konfiguracja jest inicjowana poza `Startup` klasy. Aplikacji przy użyciu EF Core zwykle dostęp do ich parametry połączenia z konfiguracji, więc `Startup` samodzielnie nie jest już wystarczające. W przypadku uaktualniania aplikacji platformy ASP.NET Core 1.x następujący błąd może zostać wyświetlony podczas korzystania z narzędzi EF Core.

> W "ApplicationContext" został znaleziony żaden konstruktor bez parametrów. Dodaj konstruktor bez parametrów do "ApplicationContext" albo Dodaj implementację "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" w tym samym zestawie co "ApplicationContext"

Nowe haku czasu projektowania został dodany w szablonie domyślnego platformy ASP.NET Core 2.0. Statycznych `Program.BuildWebHost` metoda zapewnia podstawowe EF można uzyskać dostępu do aplikacji dostawcy usług w czasie projektowania. Jeśli uaktualniasz aplikacji platformy ASP.NET Core 1.x, konieczne będzie informować Cię o `Program` klasy na podobny do następującego.

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

### <a name="idbcontextfactory-renamed"></a>Interfejsu IDbContextFactory zmieniona

Aby można było obsługiwać wzorce różnych aplikacji i zapewniają użytkownikom większą kontrolę nad jak ich `DbContext` jest używany w czasie projektowania, udostępniamy, w przeszłości `IDbContextFactory<TContext>` interfejsu. W czasie projektowania, EF podstawowe narzędzia odnajdzie implementacje tego interfejsu w projekcie i umożliwia utworzenie `DbContext` obiektów.

Ten interfejs ma bardzo ogólne nazwę, która błąd w przypadku niektórych użytkowników, aby spróbować ponownie przy użyciu dla innych `DbContext`— tworzenie scenariuszy. Zostały przechwycono poza guard, gdy narzędzia EF następnie próbowano użyć ich implementacji w czasie projektowania i spowodował poleceń, takich jak `Update-Database` lub `dotnet ef database update` się niepowodzeniem.

Aby komunikować się silne semantykę czasu projektowania ten interfejs, możemy zmieniono jego `IDesignTimeDbContextFactory<TContext>`.

2.0 wersji `IDbContextFactory<TContext>` nadal istnieje, ale jest oznaczony jako przestarzały.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions usunięte

Ze względu na zmiany ASP.NET Core 2.0, opisane powyżej, który znaleziono `DbContextFactoryOptions` został już potrzebne na nowym `IDesignTimeDbContextFactory<TContext>` interfejsu. Poniżej przedstawiono opis rozwiązań alternatywnych, który powinien być używany zamiast tego.

| DbContextFactoryOptions | Alternatywne                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Katalog roboczy czasu projektowania zmienione

Zmiany platformy ASP.NET Core 2.0 wymaga również katalog roboczy używany przez `dotnet ef` , aby były wyrównane z katalogu roboczego używany przez Visual Studio podczas uruchamiania aplikacji. Jeden zauważalne efektem ubocznym tego jest tym SQLite nazwy plików są teraz względem katalogu projektu i nie do katalogu wyjściowego tak jak kiedyś.

### <a name="ef-core-20-requires-a-20-database-provider"></a>Podstawowe EF 2.0 wymaga dostawcy 2.0 bazy danych

EF Core 2.0 wprowadzono wiele uproszczenia i ulepszenia w sposób dostawcy bazy danych działa. Oznacza to, 1.0.x i 1.1.x dostawcy nie będą działać z EF Core 2.0.

Dostawcy programu SQL Server i bazy danych SQLite są dostarczane przez zespół EF i wersje 2.0 będą dostępne jako część programu 2.0 wersji. Dla dostawców innej open source [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), i [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) są aktualizowane 2.0. Dla wszystkich innych dostawców skontaktuj się z modułu zapisującego dostawcy.

### <a name="logging-and-diagnostics-events-have-changed"></a>Zmieniono zdarzenia rejestrowania i diagnostyki

Uwaga: te zmiany nie powinny mieć wpływ większość kodu aplikacji.

Identyfikatory zdarzeń komunikatów wysyłanych do [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) zostały zmienione w 2.0. Identyfikatory zdarzeń obecnie są unikatowe w EF rdzeń kodu. Te komunikaty teraz również wykonać standardowego wzorca strukturalnych rejestrowania używany przez, na przykład MVC.

Kategorie rejestratora również zostały zmienione. Ma teraz dobrze znanego zestawu kategorii dostępne za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) zdarzenia teraz użyć tej samej nazwy Identyfikatora zdarzenia odpowiadającego `ILogger` wiadomości. Ładunki zdarzeń są wszystkie nominalnego typów pochodnych [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Identyfikatory zdarzeń, typy ładunku i kategorie są udokumentowane w artykule [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) i [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) klasy.

Identyfikatory również został przeniesiony z Microsoft.EntityFrameworkCore.Infraestructure nowej Microsoft.EntityFrameworkCore.Diagnostics przestrzeni nazw.

### <a name="ef-core-relational-metadata-api-changes"></a>Zmiany relacyjne metadanych interfejsu API Core EF

Podstawowe EF 2.0 teraz kompilacji w innej [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego używanego innego dostawcy. Jest to zazwyczaj niewidoczne dla aplikacji. Ma to ułatwić uproszczenia metadanych niższego poziomu interfejsów API tak, aby wszelkie dostęp do _wspólne pojęcia relacyjne metadanych_ jest zawsze wykonywane za pośrednictwem wywołania `.Relational` zamiast `.SqlServer`, `.Sqlite`itp. Na przykład 1.1.x kod następująco:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Powinny teraz być zapisany jako:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Zamiast metody, takie jak `ForSqlServerToTable`, metody rozszerzenia są teraz dostępne do zapisu warunkowego kodu opartego na bieżącym dostawcy używany. Na przykład:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Należy pamiętać, że ta zmiana ma zastosowanie tylko do interfejsów API/metadanych, który jest zdefiniowany dla _wszystkie_ relacyjne dostawców. Interfejs API i metadanych jest taka sama, gdy nie jest specyficzne dla tylko jednego dostawcę. Na przykład indeksy klastrowane są specyficzne dla programu SQL Server, dlatego `ForSqlServerIsClustered` i `.SqlServer().IsClustered()` musi być używany.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Nie przejąć kontrolę nad dostawca programu EF usługi

Podstawowe EF używa wewnętrznego `IServiceProvider` (tj. kontener iniekcji zależności) do jego wewnętrznej implementacji. Aplikacje powinna zezwalać na podstawowe EF tworzenie i zarządzanie nimi ten dostawca, z wyjątkiem w szczególnych przypadkach. Zdecydowanie Rozważ usunięcie wszelkie wywołania `UseInternalServiceProvider`. Jeśli aplikacja musi wywołać `UseInternalServiceProvider`, należy rozważyć [składanie problemu](https://github.com/aspnet/EntityFramework/Issues) tak zbadanie inne sposoby obsługi danego scenariusza.

Wywoływanie `AddEntityFramework`, `AddEntityFrameworkSqlServer`, itp. nie jest wymagane przez kod aplikacji, chyba że `UseInternalServiceProvider` skrót. Usuń istniejące połączenia do `AddEntityFramework` lub `AddEntityFrameworkSqlServer`, itd. `AddDbContext` nadal stosuje się w taki sam sposób jak wcześniej.

### <a name="in-memory-databases-must-be-named"></a>Musi mieć nazwę bazy danych w pamięci

Globalne bez nazwy bazy danych w pamięci została usunięta i zamiast tego wszystkie bazy danych w pamięci musi mieć nazwę. Na przykład:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

To tworzy/używa bazy danych o nazwie "Mojabazadanych". Jeśli `UseInMemoryDatabase` nie zostanie ponownie wywołany o takiej samej nazwie, a następnie można użyć tej samej bazy danych w pamięci, dzięki któremu być współużytkowane przez wiele wystąpień kontekstu.

### <a name="read-only-api-changes"></a>Zmiany interfejsu API tylko do odczytu

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, i `IsStoreGeneratedAlways` zostały zdezaktualizowane i zastąpione [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) i [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Te zachowania zastosowania do żadnej właściwości (nie tylko generowanych przez Magazyn właściwości) i określić jak wartość właściwości powinien być używany podczas wstawiania do wiersza bazy danych (`BeforeSaveBehavior`) lub gdy aktualizacja istniejącej bazy danych wiersza (`AfterSaveBehavior`).

Właściwości oznaczone jako [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (np. dla kolumny obliczanej) domyślnie zignoruje wartości aktualnie ustawione na właściwość. Oznacza to, generowanych przez Magazyn wartość zawsze będzie można uzyskać niezależnie od tego, czy wartości został ustawiony lub zmodyfikowany śledzonych jednostki. Można to zmienić, ustawiając innej `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nowe zachowanie delete ClientSetNull

W poprzednich wersjach [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) miał zachowania jednostek śledzone przez kontekst, jeden zamknięty dopasowane `SetNull` semantyki. W programie EF Core 2.0 nowy `ClientSetNull` zachowanie została wprowadzona jako domyślny dla relacji opcjonalne. Jest to zachowanie `SetNull` semantyki dla śledzonych jednostek i `Restrict` zachowanie dla baz danych utworzonych przy użyciu EF Core. W naszym środowisku są oczekiwano/użyteczna zachowania śledzonych obiektów i bazy danych. `DeleteBehavior.Restrict` teraz jest honorowane dla śledzonych jednostek po ustawieniu dla relacji opcjonalne.

### <a name="provider-design-time-packages-removed"></a>Dostawca czasu projektowania pakietów usunięte

`Microsoft.EntityFrameworkCore.Relational.Design` Pakiet został usunięty. Jego zawartości są konsolidowane w `Microsoft.EntityFrameworkCore.Relational` i `Microsoft.EntityFrameworkCore.Design`.

Propaguje to do dostawcy pakietów czasu projektowania. Te pakiety (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, itd.) zostały usunięte i ich zawartość do pakietów głównego dostawcy.

Aby włączyć `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold` w programie EF Core 2.0, wystarczy odwołują się do jednego dostawcy pakietu:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
