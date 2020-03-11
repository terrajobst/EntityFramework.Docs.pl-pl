---
title: Model dostawcy Entity Framework 6 — EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 8bda3f51e8934f2add862c30e60f1185f068c515
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419361"
---
# <a name="the-entity-framework-6-provider-model"></a>Model dostawcy Entity Framework 6

Model dostawcy Entity Framework umożliwia Entity Framework używany z różnymi typami serwera baz danych. Na przykład można podłączyć jednego dostawcę, aby zezwalać na korzystanie z EF w odniesieniu do Microsoft SQL Server, podczas gdy inny dostawca może być podłączony do programu, aby zezwalać na korzystanie z EF w odniesieniu do wersji Microsoft SQL Server Compact. Dostawcy dla EF6, których wiedzą, można znaleźć na stronie [dostawców Entity Framework](~/ef6/fundamentals/providers/index.md) .

Niektóre zmiany były wymagane do w sposób, w jaki firma Dr współpracuje z dostawcami, aby zezwolić na korzystanie z EF w ramach licencji Open Source. Te zmiany wymagają ponownego skompilowania dostawców EF względem zestawów EF6 wraz z nowym mechanizmem rejestracji dostawcy.

## <a name="rebuilding"></a>Ponowne tworzenie

Dzięki EF6 kod podstawowy, który był wcześniej częścią .NET Framework jest teraz dostarczany jako zestawy poza pasmem (OOB). Szczegółowe informacje na temat tworzenia aplikacji dla usługi EF6 można znaleźć na stronie [aktualizowanie aplikacji dla Ef6](~/ef6/what-is-new/upgrading-to-ef6.md) . Należy również ponownie skompilować dostawców przy użyciu tych instrukcji.

## <a name="provider-types-overview"></a>Przegląd typów dostawcy

Dostawca EF jest w rzeczywistości kolekcją usług specyficznych dla dostawcy zdefiniowanych przez typy CLR, z których te usługi wykraczają (dla klasy podstawowej) lub implementują (dla interfejsu). Dwie z tych usług są podstawowe i niezbędne, aby EF działały w ogóle. Inne są opcjonalne i muszą być implementowane tylko wtedy, gdy wymagane są określone funkcje i/lub domyślne implementacje tych usług nie będą działać dla określonego serwera bazy danych.

## <a name="fundamental-provider-types"></a>Podstawowe typy dostawców

### <a name="dbproviderfactory"></a>DbProviderFactory

EF jest zależne od typu pochodzącego od klasy [System. Data. Common. DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) na potrzeby wykonywania wszystkich operacji dostępu do bazy danych niskiego poziomu. DbProviderFactory nie jest faktycznie częścią EF, ale zamiast tego jest klasą w .NET Framework, która obsługuje punkt wejścia dla dostawców ADO.NET, które mogą być używane przez EF, inne O/RMs lub bezpośrednio przez aplikację w celu uzyskania wystąpień połączeń, poleceń, parametrów i innych abstrakcyjnych ADO.NET w ramach dostawcy niezależny od. Więcej informacji na temat DbProviderFactory można znaleźć w [dokumentacji MSDN dotyczącej ADO.NET](https://msdn.microsoft.com/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

EF jest zależne od tego, czy typ pochodzący z DbProviderServices zapewnia dodatkowe funkcje, które jest wymagana przez EF w oparciu o funkcje już udostępnione przez dostawcę ADO.NET. W starszych wersjach EF Klasa DbProviderServices była częścią .NET Framework i została znaleziona w przestrzeni nazw System. Data. Common. Począwszy od EF6 Ta klasa jest teraz częścią EntityFramework. dll i znajduje się w przestrzeni nazw System. Data. Entity. Core. Common.

Więcej szczegółowych informacji na temat podstawowych funkcji implementacji DbProviderServices można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/ee789835.aspx). Należy jednak pamiętać, że od czasu pisania te informacje nie są aktualizowane dla EF6, chociaż większość koncepcji nadal jest ważna. SQL Server i SQL Server Compact implementacje DbProviderServices są również zaewidencjonowane do [bazy kodu typu open source](https://github.com/aspnet/EntityFramework6/) i mogą służyć jako przydatne odwołania do innych implementacji.

W starszych wersjach EF implementacja DbProviderServices do użycia została uzyskana bezpośrednio od dostawcy ADO.NET. Zostało to zrobione przez rzutowanie DbProviderFactory do IServiceProvider i wywołanie metody GetService. To ściśle połączoną dostawcę EF z DbProviderFactory. Ten sprzężenie uniemożliwiło przenoszenie EF z .NET Framework i w związku z tym do EF6 tego ścisłego sprzężenia zostało usunięte, a implementacja DbProviderServices jest teraz rejestrowana bezpośrednio w pliku konfiguracji aplikacji lub w konfiguracji opartej na kodzie, zgodnie z opisem w sekcji więcej szczegółów dotyczącej _rejestrowania DbProviderServices_ poniżej.

## <a name="additional-services"></a>Usługi dodatkowe

Oprócz podstawowych usług opisanych powyżej istnieje również wiele innych usług używanych przez EF, które są zawsze lub czasami specyficzne dla dostawcy. Domyślne implementacje konkretnych dostawców tych usług mogą być dostarczane przez implementację DbProviderServices. Aplikacje mogą również przesłaniać implementacje tych usług lub dostarczać implementacje, gdy typ DbProviderServices nie udostępnia wartości domyślnej. Opisano to szczegółowo w sekcji _Rozwiązywanie dodatkowych usług_ poniżej.

Poniżej znajdują się dodatkowe typy usług, które mogą być przydatne dla dostawcy. Więcej informacji na temat każdego z tych typów usług można znaleźć w dokumentacji interfejsu API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Jest to opcjonalna usługa, która umożliwia dostawcy wdrożenie ponownych prób lub inne zachowanie w przypadku wykonywania zapytań i poleceń względem bazy danych. Jeśli nie podano implementacji, EF po prostu wykona polecenia i propaguje wszystkie zgłoszone wyjątki. W przypadku SQL Server ta usługa jest używana w celu zapewnienia zasad ponawiania, która jest szczególnie przydatna w przypadku uruchamiania na serwerach bazy danych opartych na chmurze, takich jak SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Jest to opcjonalna usługa, która umożliwia dostawcy tworzenie DbConnection obiektów według Konwencji w przypadku podaniu tylko nazwy bazy danych. Należy zauważyć, że mimo że ta usługa może zostać rozwiązana przez implementację DbProviderServices, która była dostępna od czasu EF 4,1 i można ją jawnie ustawić w pliku konfiguracji lub w kodzie. Dostawca będzie miał możliwość rozpoznania tej usługi tylko wtedy, gdy jest ona zarejestrowana jako dostawca domyślny (zobacz _domyślnego dostawcę_ poniżej), a jeśli w innym miejscu nie ustawiono domyślnej fabryki połączeń.

### <a name="dbspatialservices"></a>DbSpatialServices

Jest to opcjonalne usługi, które umożliwiają dostawcy Dodawanie obsługi typów przestrzennych geograficznych i geometrycznych. Należy podać implementację tej usługi, aby aplikacja mogła używać EF z typami przestrzennymi. DbSptialServices jest proszony na dwa sposoby. Najpierw żąda się usług przestrzennych specyficznych dla dostawcy przy użyciu obiektu DbProviderInfo (który zawiera nazwę niezmienną i token manifestu) jako klucz. Po drugie, DbSpatialServices może być poproszony o brak klucza. Służy do rozpoznawania "globalnego dostawcy przestrzennego" używanego podczas tworzenia autonomicznych typów DbGeography lub DbGeometry.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Jest to opcjonalna usługa, która umożliwia migracje EF do użycia podczas generowania kodu SQL używanego w tworzeniu i modyfikowaniu schematów bazy danych za pomocą Code First. Implementacja jest wymagana w celu obsługi migracji. W przypadku podanej implementacji, będzie ona również używana podczas tworzenia baz danych przy użyciu inicjatorów bazy danych lub metody Database. Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, String, HistoryContextFactory >

Jest to opcjonalna usługa, która umożliwia dostawcy Konfigurowanie mapowania HistoryContext do tabeli `__MigrationHistory` używanej przez migracje EF. HistoryContext to Code First DbContext i można go skonfigurować przy użyciu normalnego interfejsu API Fluent, aby zmienić elementy, takie jak nazwa tabeli i specyfikacje mapowania kolumn. Domyślna implementacja tej usługi zwrócona przez EF dla wszystkich dostawców może współpracowała dla danego serwera bazy danych, jeśli wszystkie domyślne mapowania tabeli i kolumny są obsługiwane przez tego dostawcę. W takim przypadku dostawca nie musi podawać implementacji tej usługi.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Jest to opcjonalna usługa w celu uzyskania poprawnej DbProviderFactory z danego obiektu DbConnection. Domyślna implementacja tej usługi zwrócona przez EF dla wszystkich dostawców jest przeznaczona do pracy dla wszystkich dostawców. Jednak w przypadku korzystania z platformy .NET 4 DbProviderFactory nie jest publicznie dostępna z jednego, jeśli jego połączenia DB. W związku z tym EF używa pewnych heurystyki do przeszukiwania zarejestrowanych dostawców w celu znalezienia dopasowania. Istnieje możliwość, że w przypadku niektórych dostawców heurystyka nie powiedzie się, a w takich sytuacjach dostawca powinien dostarczyć nową implementację.

## <a name="registering-dbproviderservices"></a>Rejestrowanie DbProviderServices

Implementacja DbProviderServices do użycia może być zarejestrowana w pliku konfiguracji aplikacji (App. config lub Web. config) lub przy użyciu konfiguracji opartej na kodzie. W obu przypadkach Rejestracja używa jako klucza dostawcy "niezmienna nazwa". Umożliwia to rejestrowanie i używanie wielu dostawców w pojedynczej aplikacji. Niezmienna nazwa używana na potrzeby rejestracji EF jest taka sama jak nazwa niezmienna używana dla rejestracji dostawcy ADO.NET i parametrów połączenia. Na przykład dla SQL Server użyto niezmiennej nazwy "System. Data. SqlClient".

### <a name="config-file-registration"></a>Rejestracja pliku konfiguracji

Typ DbProviderServices, który ma być używany, jest zarejestrowany jako element dostawcy na liście dostawców w sekcji entityFramework w pliku konfiguracyjnym aplikacji. Na przykład:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

Ciąg _typu_ musi być nazwą typu kwalifikowana przez zestaw, który ma być używany przez implementację DbProviderServices.

### <a name="code-based-registration"></a>Rejestracja oparta na kodzie

Począwszy od dostawców EF6 można także zarejestrować za pomocą kodu. Pozwala to na użycie dostawcy EF bez wprowadzania zmian w pliku konfiguracji aplikacji. Aby skorzystać z konfiguracji opartej na kodzie, aplikacja powinna utworzyć klasę dbconfiguration, zgodnie z opisem w [dokumentacji konfiguracyjnej opartej na kodzie](https://msdn.com/data/jj680699). Konstruktor klasy dbconfiguration powinien następnie wywołać SetProviderServices w celu zarejestrowania dostawcy EF. Na przykład:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Rozpoznawanie dodatkowych usług

Jak wspomniano powyżej w sekcji _Przegląd typów dostawcy_ , Klasa DbProviderServices może być również używana do rozwiązywania dodatkowych usług. Jest to możliwe, ponieważ DbProviderServices implementuje IDbDependencyResolver, a każdy zarejestrowany typ DbProviderServices jest dodawany jako "domyślny program rozpoznawania nazw". Mechanizm IDbDpendencyResolver został opisany bardziej szczegółowo w temacie [rozpoznawanie zależności](~/ef6/fundamentals/configuring/dependency-resolution.md). Nie trzeba jednak zrozumieć wszystkich koncepcji w tej specyfikacji w celu rozwiązania dodatkowych usług w dostawcy.

Najczęstszym sposobem, w jaki dostawca rozpoznaje dodatkowe usługi, jest wywołanie DbProviderServices. AddDependencyResolver dla każdej usługi w konstruktorze klasy DbProviderServices. Na przykład SqlProviderServices (dostawca EF dla SQL Server) ma kod podobny do tego w przypadku inicjalizacji:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Ten konstruktor używa następujących klas pomocnika:

*   SingletonDependencyResolver: zapewnia prosty sposób rozpoznawania pojedynczych usług — to znaczy usług, dla których to samo wystąpienie jest zwracane za każdym razem, gdy wywoływana jest metoda GetService. Przejściowe usługi są często rejestrowane jako pojedyncze fabryki, które będą używane do tworzenia wystąpień przejściowych na żądanie.
*   ExecutionStrategyResolver: program rozpoznawania nazw specyficzny do zwracania implementacji IExecutionStrategy.

Zamiast używać DbProviderServices. AddDependencyResolver, można również przesłonić DbProviderServices. GetService i bezpośrednio rozwiązać dodatkowe usługi. Ta metoda zostanie wywołana, gdy EF potrzebuje usługi zdefiniowanej przez określony typ, a w niektórych przypadkach dla danego klucza. Metoda powinna zwrócić usługę, jeśli może, lub zwrócić wartość null, aby zrezygnować z zwracania usługi, a zamiast tego zezwolić innej klasie na jej rozwiązanie. Aby na przykład rozwiązać domyślną fabrykę połączeń, kod w GetService może wyglądać następująco:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Zamówienie rejestracji

Gdy wiele implementacji DbProviderServices jest zarejestrowanych w pliku konfiguracyjnym aplikacji, zostaną one dodane jako pomocnicze rozwiązania w kolejności, w jakiej są wyświetlane. Ponieważ resolvery są zawsze dodawane na początku pomocniczego łańcucha rozpoznawania, oznacza to, że dostawca na końcu listy uzyska szansę na rozwiązanie zależności przed innymi. (To może wydawać się nieco bardziej intuicyjne, ale jest to przydatne, jeśli przypuśćsz, że każdy dostawca został wystawiony z listy i zostanie umieszczony na szczycie istniejących dostawców).

Takie porządkowanie zazwyczaj nie jest istotne, ponieważ większość usług dostawcy jest specyficzna dla dostawcy i niezależna od nazwy dostawcy. Jednak w przypadku usług, które nie są oparte na nazwie niezmiennej dostawcy lub innego klucza specyficznego dla dostawcy, usługa zostanie rozwiązany w oparciu o tę kolejność. Na przykład jeśli nie jest jawnie ustawiona inaczej w innym miejscu, domyślną fabrykę połączeń będzie pochodzi od dostawcy najwyższego poziomu w łańcuchu.

## <a name="additional-config-file-registrations"></a>Dodatkowe rejestracje plików konfiguracji

Istnieje możliwość jawnego rejestrowania niektórych dodatkowych usług dostawcy opisanych powyżej bezpośrednio w pliku konfiguracji aplikacji. Gdy to zrobisz, zostanie użyta Rejestracja w pliku konfiguracji zamiast niczego zwracanego przez metodę GetService implementacji DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Rejestrowanie domyślnej fabryki połączeń

Począwszy od EF5 pakiet NuGet EntityFramework automatycznie zarejestrował fabrykę połączeń SQL Express lub fabrykę połączeń LocalDb w pliku konfiguracyjnym.

Na przykład:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Typ_ to kwalifikowana dla zestawu nazwa typu dla domyślnej fabryki połączeń, która musi implementować IDbConnectionFactory.

Zaleca się, aby pakiet NuGet dostawcy ustawił domyślną fabrykę połączeń w ten sposób w przypadku instalacji. Zobacz _pakiety NuGet dla dostawców_ poniżej.

## <a name="additional-ef6-provider-changes"></a>Dodatkowe zmiany dostawcy EF6

### <a name="spatial-provider-changes"></a>Zmiany dostawcy przestrzennego

Dostawcy obsługujący typy przestrzenne muszą teraz zaimplementować kilka dodatkowych metod w klasach pochodnych z DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Istnieją również nowe asynchroniczne wersje istniejących metod, które zaleca się przesłonić jako delegata implementacji domyślnej do metod synchronicznych i w związku z tym nie są wykonywane asynchronicznie:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Natywna obsługa wyliczalnych. zawiera

EF6 wprowadza nowy typ wyrażenia, DbInExpression, który został dodany do rozwiązywania problemów z wydajnością wokół użycia wartości wyliczalnej. zawiera zapytania LINQ. Klasa Metoda DbProviderManifest ma nową metodę wirtualną SupportsInExpression, która jest wywoływana przez EF w celu ustalenia, czy dostawca obsługuje nowy typ wyrażenia. W celu zapewnienia zgodności z istniejącymi implementacjami dostawcy Metoda zwraca wartość false. Aby skorzystać z tego ulepszenia, dostawca EF6 może dodać kod do obsługi DbInExpression i przesłania SupportsInExpression, aby zwrócić wartość true. Wystąpienie elementu DbInExpression można utworzyć, wywołując metodę DbExpressionBuilder.In. Wystąpienie DbInExpression składa się z DbExpression, zazwyczaj reprezentuje kolumnę tabeli i listę DbConstantExpression do przetestowania w celu dopasowania.

## <a name="nuget-packages-for-providers"></a>Pakiety NuGet dla dostawców

Jednym ze sposobów udostępnienia dostawcy EF6 jest udostępnienie go jako pakiet NuGet. Korzystanie z pakietu NuGet ma następujące zalety:

*   Aby dodać rejestrację dostawcy do pliku konfiguracji aplikacji, można łatwo użyć narzędzia NuGet
*   Dodatkowe zmiany można wprowadzić w pliku konfiguracyjnym, aby ustawić domyślną fabrykę połączeń, tak aby połączenia podejmowane przez Konwencję używały zarejestrowanego dostawcy
*   Program NuGet obsługuje dodawanie przekierowań powiązań, aby dostawca EF6 kontynuował pracę nawet po wydaniu nowego pakietu EF

Przykładem jest pakiet EntityFramework. SqlServerCompact, który znajduje się w bazie kodu typu [Open Source](https://github.com/aspnet/entityframework6). Ten pakiet zawiera dobry szablon do tworzenia pakietów NuGet dostawcy EF.

### <a name="powershell-commands"></a>Polecenia programu PowerShell

Po zainstalowaniu pakietu NuGet EntityFramework rejestruje moduł programu PowerShell, który zawiera dwa polecenia, które są bardzo przydatne dla pakietów dostawcy:

*   Add-EFProvider dodaje nową jednostkę dla dostawcy w pliku konfiguracyjnym projektu docelowego i sprawdza, czy znajduje się na końcu listy zarejestrowanych dostawców.
*   Parametr Add-EFDefaultConnectionFactory dodaje lub aktualizuje rejestrację defaultConnectionFactory w pliku konfiguracyjnym projektu docelowego.

Oba te polecenia zapewnią dodanie sekcji entityFramework do pliku konfiguracji i dodanie kolekcji dostawców w razie potrzeby.

Jest on przeznaczony do wywoływania tych poleceń ze skryptu install. ps1 NuGet. Na przykład install. ps1 dla dostawcy SQL Compact wygląda podobnie do przedstawionego poniżej:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Więcej informacji o tych poleceniach można uzyskać za pomocą polecenia Get-Help w oknie konsoli Menedżera pakietów.

## <a name="wrapping-providers"></a>Dostawcy zawijania

Dostawca otoki jest dostawcą EF i/lub ADO.NET, który zawija istniejący dostawca w celu jego rozwinięcia z innymi funkcjami, takimi jak profilowanie lub śledzenie. Dostawcy zawijania mogą być zarejestrowani w zwykły sposób, ale często łatwiej jest skonfigurować dostawcę otoki w środowisku uruchomieniowym przez przechwycenie rozwiązania usług związanych z dostawcą. W tym celu można użyć OnLockingConfiguration zdarzeń statycznych w klasie dbconfiguration.

OnLockingConfiguration jest wywoływana po ustaleniu, gdzie cała konfiguracja EF dla domeny aplikacji zostanie uzyskana z usługi, ale zanim zostanie zablokowana do użycia. Podczas uruchamiania aplikacji (przed użyciem EF) aplikacja powinna zarejestrować procedurę obsługi zdarzeń dla tego zdarzenia. (Rozważamy Dodawanie obsługi rejestrowania tego programu obsługi w pliku konfiguracji, ale nie jest to jeszcze obsługiwane). Program obsługi zdarzeń powinien następnie wykonać wywołanie ReplaceService dla każdej usługi, która musi zostać opakowana.  

Na przykład, aby zawijać IDbConnectionFactory i DbProviderService, należy zarejestrować procedurę obsługi podobną do tej:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Usługa, która została rozwiązana i powinna zostać teraz opakowana wraz z kluczem, który został użyty do rozpoznania usługi, jest przesyłana do procedury obsługi. Program obsługi może następnie otoczyć tę usługę i zamienić zwróconą usługę na zawiniętej wersji.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Rozpoznawanie DbProviderFactory z EF

DbProviderFactory jest jednym z podstawowych typów dostawców wymaganych przez EF zgodnie z opisem w powyższej sekcji _Przegląd typów dostawcy_ . Jak już wspomniano, nie jest to typ EF, a rejestracja nie jest zwykle częścią konfiguracji EF, ale zamiast tego jest to normalna Rejestracja dostawcy ADO.NET w pliku Machine. config i/lub pliku konfiguracyjnym aplikacji.

Mimo że ta EF nadal używa normalnego mechanizmu rozpoznawania zależności podczas wyszukiwania DbProviderFactory do użycia. Domyślny program rozpoznawania nazw używa normalnej rejestracji ADO.NET w plikach konfiguracji i dlatego jest zazwyczaj przezroczysty. Jednak ze względu na to, że używany jest standardowy mechanizm rozpoznawania zależności, oznacza to, że IDbDependencyResolver może służyć do rozwiązywania DbProviderFactory nawet wtedy, gdy normalne rejestrowanie ADO.NET nie zostało wykonane.

Rozwiązanie DbProviderFactory w ten sposób ma pewne konsekwencje:

*   Aplikacja korzystająca z konfiguracji opartej na kodzie może dodawać wywołania w klasie dbconfiguration, aby zarejestrować odpowiednie DbProviderFactory. Jest to szczególnie przydatne w przypadku aplikacji, które nie chcą korzystać z dowolnej konfiguracji opartej na plikach.
*   Usługa może być opakowana lub zastępowana przy użyciu ReplaceService, zgodnie z opisem w sekcji _dostawcy otoki_ powyżej
*   Teoretycznie implementacja DbProviderServices może rozpoznać DbProviderFactory.

Ważne jest, aby pamiętać o wykonywaniu którejkolwiek z tych rzeczy, które mają wpływ tylko na wyszukiwanie DbProviderFactory przez EF. Inny kod nieef może nadal oczekiwać, że dostawca ADO.NET jest zarejestrowany w normalny sposób i może się nie powieść, jeśli nie zostanie znaleziona Rejestracja. Z tego powodu zwykle lepiej jest zarejestrować DbProviderFactory w normalnym ADO.NET sposób.

### <a name="related-services"></a>Powiązane usługi

Jeśli program EF jest używany do rozpoznawania DbProviderFactory, należy również rozwiązać usługi IProviderInvariantName i IDbProviderFactoryResolver.

IProviderInvariantName to usługa, która jest używana do określenia niezmiennej nazwy dostawcy dla danego typu DbProviderFactory. Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawca ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozpoznawany przez EF, konieczne będzie również rozwiązanie tej usługi. Należy pamiętać, że program rozpoznawania nazw dla tej usługi jest automatycznie dodawany podczas korzystania z metody dbconfiguration. SetProviderFactory.

Zgodnie z opisem w powyższej sekcji _Przegląd typów dostawcy_ IDbProviderFactoryResolver służy do uzyskania prawidłowej DbProviderFactory z danego obiektu DbConnection. Domyślna implementacja tej usługi w przypadku korzystania z platformy .NET 4 korzysta z rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawca ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozpoznawany przez EF, konieczne będzie również rozwiązanie tej usługi.
