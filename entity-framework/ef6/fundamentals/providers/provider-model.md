---
title: Model dostawcy platformy Entity Framework 6 — EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: e8b0552ec083d8ab276aa9de109650f423160269
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821390"
---
# <a name="the-entity-framework-6-provider-model"></a>Dostawca modelu Entity Framework 6

Model dostawcy programu Entity Framework umożliwia Entity Framework ma być używany z różnymi typami serwera bazy danych. Na przykład jeden dostawca można podłączyć umożliwia EF ma być używany dla programu Microsoft SQL Server, podczas gdy umożliwia EF ma być używany dla programu Microsoft SQL Server Compact Edition można podłączyć do innego dostawcy. Dostawcy dla platformy EF6, które sobie sprawę z znajduje się na [dostawcy programu Entity Framework](~/ef6/fundamentals/providers/index.md) strony.

Niektóre zmiany były wymagane do metody, gdy EF wchodzi w interakcję z dostawcami, aby umożliwić EF mogą być wprowadzane w ramach licencji open source. Te zmiany wymagają odbudowywania EF dostawców dla zestawów platformy EF6 wraz z nowe mechanizmy Rejestracja dostawcy.

## <a name="rebuilding"></a>Ponowne tworzenie

Przy użyciu platformy EF6 kodu core, który został wcześniej częścią programu .NET Framework jest teraz są dostarczane jako poza pasmem (OOB), zestawy. Szczegółowe informacje na temat tworzenia aplikacji w ramach platformy EF6 znajduje się na [aktualizowania aplikacji dla platformy EF6](~/ef6/what-is-new/upgrading-to-ef6.md) strony. Dostawców będzie również trzeba odbudować, korzystając z tych instrukcji.

## <a name="provider-types-overview"></a>Przegląd typów dostawcy

Dostawca usługi EF jest naprawdę kolekcja właściwe dla dostawcy usług zdefiniowane przez typy CLR, które tych usług rozszerza z (dla klasy bazowej) ani nie implementuje (w przypadku interfejsu). Są dwa z tych usług podstawowych i niezbędne dla platformy EF w ogóle działać. Inne są opcjonalne i wystarczy do zaimplementowania Jeśli określonych jest wymagane i/lub domyślnej implementacji tych usług nie działa dla konkretnej bazy danych serwer docelowy.

### <a name="fundamental-provider-types"></a>Typy podstawowe dostawców

#### <a name="dbproviderfactory"></a>DbProviderFactory

EF zależy od tego, typ pochodzący od posiadania [System.Data.Common.DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) do wykonywania wszystkich dostęp do niskiego poziomu bazy danych. DbProviderFactory nie jest częścią platformy EF, lecz dotyczy klasy w .NET Framework, która służy punkt wejścia dla dostawcy ADO.NET, który może służyć przez EF, O/RMs lub bezpośrednio przez aplikację można uzyskać rodzajami połączeń, polecenia, parametry i inne abstrakcje ADO.NET dostawcy sposób niezależny od. Więcej informacji na temat DbProviderFactory można znaleźć w [dokumentacji MSDN dotyczącej ADO.NET](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

#### <a name="dbproviderservices"></a>DbProviderServices

EF zależy od tego, mające typ pochodzący od DbProviderServices zapewniające dodatkowe funkcje wymagane przez EF na podstawie funkcje już udostępniane przez dostawcę danych ADO.NET. W starszych wersjach programu EF klasy DbProviderServices było częścią programu .NET Framework i został znaleziony w przestrzeni nazw System.Data.Common. Począwszy od platformy EF6 tej klasy jest teraz częścią EntityFramework.dll i znajduje się w przestrzeni nazw System.Data.Entity.Core.Common.

Więcej informacji na temat podstawowych funkcji implementacji DbProviderServices znajduje się na [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx). Jednak należy pamiętać, że od czasu zapisania tych informacji nie jest aktualizowana dla platformy EF6 mimo że większość koncepcji są nadal ważne. Implementacji programu SQL Server i programu SQL Server Compact DbProviderServices również są sprawdzane w celu [bazy kodu typu open-source](https://github.com/aspnet/EntityFramework6/) i może służyć jako przydatne dane dla innych implementacji.

W starszych wersjach programu EF implementacji DbProviderServices używać pochodzi bezpośrednio od dostawcy ADO.NET. Zostało to zrobione, rzutowanie DbProviderFactory na IServiceProvider i wywołanie metody GetService. To ściśle powiązane dostawcy EF DbProviderFactory. Ta sprzężenia zablokowany EF jest przenoszony z .NET Framework i w związku z tym dla platformy EF6 to ścisłego sprzężenia została usunięta, a implementacja DbProviderServices jest obecnie zarejestrowany bezpośrednio w pliku konfiguracji aplikacji lub oparte na kodzie Konfiguracja opisany bardziej szczegółowo _rejestrowanie DbProviderServices_ poniższej sekcji.

### <a name="additional-services"></a>Usługi dodatkowe

Oprócz podstawowych usług opisanych powyżej są również wiele innych usług używanych przez EF, które są zawsze lub czasami specyficzne dla dostawcy. Domyślnej implementacji właściwe dla dostawcy, te usługi mogą być dostarczane przez implementację DbProviderServices. Aplikacje można również zastąpić implementacji tych usług lub dostarczać implementacje, gdy typem DbProviderServices nie dostarcza domyślny. Jest to opisane bardziej szczegółowo w _rozpoznawanie dodatkowych usług_ poniższej sekcji.

Poniżej przedstawiono typy dodatkowych usług, które dostawcy mogą być przydatne do dostawcy. Więcej szczegółów na temat każdego z tych typów usług można znaleźć w dokumentacji interfejsu API.

#### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Jest to opcjonalna usługa, która umożliwia dostawcy zaimplementować ponownych prób lub inne zachowanie, gdy zapytań i poleceń, które są wykonywane względem bazy danych. Jeśli nie dostarczono żadnej implementacji, EF będzie po prostu wykonaj polecenia i propagowanie wyjątki zgłaszane. Dla programu SQL Server ta usługa służy do zapewnienia zasady ponawiania, co jest szczególnie przydatne, jeśli działających w odniesieniu do serwerów bazy danych opartej na chmurze, takich jak SQL Azure.

#### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Jest to opcjonalna usługa, która umożliwia dostawcy utworzyć obiekty DbConnection zgodnie z Konwencją, gdy podana nazwa bazy danych. Należy pamiętać, że gdy ta usługa może zostać rozpoznana przez implementację DbProviderServices została już obecne od EF 4.1 i może również być jawnie ustawione w pliku konfiguracji lub w kodzie. Dostawca tylko otrzymają Państwo rozpoznać tej usługi, jeśli on zarejestrowany jako domyślnego dostawcę (zobacz _domyślny dostawca_ poniżej) i jeśli domyślna fabryka połączenia nie został ustawiony gdzie indziej.

#### <a name="dbspatialservices"></a>DbSpatialServices

Jest to opcjonalne usług, które umożliwia dostawcy dodać obsługę typów przestrzennych geometry i położenia geograficznego. Implementacja tej usługi należy podać w kolejności dla aplikacji EF za pomocą typów przestrzennych. DbSptialServices zostanie poproszony o na dwa sposoby. Po pierwsze, właściwe dla dostawcy usług przestrzennych, są żądane przy użyciu obiektu DbProviderInfo (zawierający niezmiennej token nazwy, a manifestu) jako klucza. Po drugie DbSpatialServices można monit o wpisanie bez klucza. Służy to rozwiązać "globalne przestrzenne dostawcę" użytą podczas tworzenia autonomicznego DbGeography lub DbGeometry typów.

#### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Jest to opcjonalna usługa, która umożliwia migracji EF, służący do generowania SQL używane podczas tworzenia i modyfikowania schematy bazy danych przez rozwiązanie Code First. Implementacja jest wymagana w celu zapewnienia obsługi migracji. Jeśli podano implementację go będzie również użyte podczas tworzenia bazy danych przy użyciu metody Database.Create lub inicjatory bazy danych.

#### <a name="funcdbconnection-string-historycontextfactory"></a>FUNC < DbConnection, string, HistoryContextFactory >

Jest to opcjonalna usługa, która umożliwia dostawcy skonfigurować mapowanie HistoryContext do `__MigrationHistory` tabeli używanej przez migracje EF. HistoryContext jest pierwszy typu DbContext kodu i można skonfigurować przy użyciu normalnych wygodnego interfejsu API można zmienić elementów, takich jak nazwy tabeli i specyfikacji mapowania kolumn. Domyślna implementacja tej usługi dla wszystkich dostawców zwracane przez EF mogą działać w przypadku serwera bazy danych, jeśli wszystkie domyślne tabel i kolumn mapowania są obsługiwane przez tego dostawcę. W takim przypadku dostawca nie musi dostarczyć implementację tej usługi.

#### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Jest to opcjonalne zapewniającej uzyskiwanie DbProviderFactory poprawne z danego obiektu DbConnection. Domyślna implementacja tej usługi dla wszystkich dostawców zwracane przez EF jest przeznaczony do pracy dla wszystkich dostawców. Jednak podczas uruchamiania na .NET 4, DbProviderFactory nie jest dostępny publicznie w jeden jego DbConnections. W związku z tym EF używa niektóre heurystyki do wyszukiwania zarejestrowanych dostawców w celu znalezienia dopasowania. Istnieje możliwość, że w przypadku niektórych dostawców te algorytmy heurystyczne zakończy się niepowodzeniem, a w takich sytuacjach dostawcy należy podać nową metodę implementacji.

## <a name="registering-dbproviderservices"></a>Rejestrowanie DbProviderServices

W pliku konfiguracji (app.config lub web.config) lub przy użyciu konfiguracji na podstawie kodu aplikacji można zarejestrować implementacji DbProviderServices do użycia. W obu przypadkach rejestracji używa dostawcy "niezmiennej nazwie" jako klucza. Dzięki temu wielu dostawców były rejestrowane i używane w jednej aplikacji. Niezmienna nazwa używana w przypadku rejestracji programu EF jest taka sama jak nazwa niezmienna, używana dla ciągów połączenia i Rejestracja dostawcy ADO.NET. Na przykład dla programu SQL Server niezmiennej nazwie "System.Data.SqlClient" jest używany.

### <a name="config-file-registration"></a>Rejestracja pliku konfiguracji

Typ DbProviderServices do użycia jest zarejestrowany jako element dostawcę na liście dostawców entityFramework części pliku konfiguracji aplikacji. Na przykład:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

_Typu_ ciąg musi być typu kwalifikowanego zestawu nazwę implementacji DbProviderServices do użycia.

### <a name="code-based-registration"></a>Oparte na kodzie rejestracji

Począwszy od platformy EF6 dostawców można również zarejestrować przy użyciu kodu. Dzięki temu dostawcy EF można używać bez zmian do pliku konfiguracji aplikacji. Używać konfiguracji oparte na kodzie aplikacji należy utworzyć klasę DbConfiguration, zgodnie z opisem w [dokumentacji oparty na kodzie konfiguracji](http://msdn.com/data/jj680699). Konstruktor klasy DbConfiguration następnie należy wywołać SetProviderServices, aby zarejestrować dostawcę EF. Na przykład:

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

Jak wspomniano powyżej, w _dostawcy typów Przegląd_ sekcji DbProviderServices klasy można również do rozpoznania usług dodatkowych. Jest to możliwe, ponieważ DbProviderServices implementuje IDbDependencyResolver i każdego zarejestrowanego typu DbProviderServices jest dodawany jako "domyślny mechanizm rozwiązywania konfliktów". Mechanizm IDbDpendencyResolver jest opisany bardziej szczegółowo w [Rozdziale zależności](~/ef6/fundamentals/configuring/dependency-resolution.md). Jednak nie jest niezbędne zapoznać się z pojęciami w tej specyfikacji do rozpoznania usług dodatkowych, dostawcy.

Najczęstszym sposobem dostawcy do rozpoznania usług, dodatkowe jest wywołanie DbProviderServices.AddDependencyResolver dla każdej usługi w konstruktorze klasy DbProviderServices. Na przykład SqlProviderServices (Dostawca EF dla programu SQL Server) ma kod podobny do tego inicjowania:

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

Ten konstruktor korzysta z następujących klas pomocniczych:

*   SingletonDependencyResolver: zapewnia prosty sposób rozwiązania pojedynczego wystąpienia usług — oznacza to, usług, dla których tego samego wystąpienia jest zwracana zawsze nazywa GetService. Przejściowy usługi często są rejestrowane jako fabryki singleton, która będzie służyć do tworzenia wystąpienia przejściowego na żądanie.
*   ExecutionStrategyResolver: program rozpoznawania nazw specyficzne dla zwracania IExecutionStrategy implementacji.

Zamiast używania DbProviderServices.AddDependencyResolver istnieje również możliwość zastąpienia DbProviderServices.GetService i rozwiązać dodatkowych usług bezpośrednio. Ta metoda zostanie wywołana, gdy EF potrzebuje usługa definiuje na podstawie określonego typu, a w niektórych przypadkach dla danego klucza. Metoda powinna zwrócić usługi, jeśli można lub zwraca wartość null, jak zrezygnować przekazujących usługi i zezwolić innej klasy go rozwiązać. Na przykład aby rozwiązać domyślną fabrykę połączenie kodu w GetService może wyglądać mniej więcej tak:

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

### <a name="registration-order"></a>Kolejność rejestracji

Gdy wiele implementacji DbProviderServices są rejestrowane w pliku konfiguracyjnym aplikacji będzie można dodać jako dodatkowej rozpoznawania nazw w kolejności, w jakiej występują na liście. Ponieważ rozwiązujący zawsze są dodawane do góry łańcucha dodatkowej programu rozpoznawania nazw, oznacza to, że dostawcy na końcu listy otrzymają Państwo rozpoznać zależności przed innymi. (To może wydawać się nieco counter-intuitive na początku, ale dobrym pomysłem Wyobraź sobie, biorąc każdy dostawca z listy i układania na podstawie istniejącego dostawcy).

Zazwyczaj ta kolejność nie ma znaczenia, ponieważ większość usług dostawcy są właściwe dla dostawcy i dostosowane przez nazwa niezmienna dostawcy. Jednak dla usług, są nie kluczach nazwa niezmienna dostawcy lub niektórych innych właściwe dla dostawcy klucz, który zostanie rozwiązany usługi oparte na ta kolejność. Na przykład jeśli go nie jest jawnie określona inaczej gdzieś else, domyślna fabryka połączenia będą pochodzić z dostawcę najwyższego poziomu w łańcuchu.

## <a name="additional-config-file-registrations"></a>Rejestracje plików dodatkowych konfiguracji

Istnieje możliwość jawnie zarejestrować niektórych usług dodatkowego dostawcę opisanych powyżej bezpośrednio w pliku konfiguracji aplikacji. Po zakończeniu to rejestrowania w pliku konfiguracji będą używane zamiast niczego zwracany przez metodę GetService implementacji DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Rejestrowanie domyślną fabrykę połączenia

Począwszy od EF5 pakiet NuGet platformy EntityFramework automatycznie zarejestrowana fabryka połączenia programu SQL Express lub LocalDb fabryka połączenia w pliku konfiguracji.

Na przykład:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Typu_ jest nazwą typu kwalifikowanego zestawu usługi fabryka połączenia domyślna musi implementować IDbConnectionFactory.

Zalecane jest dostawcy pakietu NuGet ustawienie domyślna fabryka połączenia w ten sposób podczas instalacji. Zobacz _pakietów NuGet dla dostawców_ poniżej.

## <a name="additional-ef6-provider-changes"></a>Dodatkowe zmiany dostawcy EF6

### <a name="spatial-provider-changes"></a>Zmiany dostawcy przestrzenne

Dostawcy, obsługujące typów przestrzennych teraz należy zaimplementować pewne dodatkowe metody na klasy wywodzące się z DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Dostępne są również nowe wersje asynchroniczne istniejących metod, które są zalecane do zastąpienia domyślnej implementacji delegowania do metod synchronicznych i dlatego nie są wykonywane asynchronicznie:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Natywna obsługa Enumerable.Contains

EF6 wprowadza nowy typ wyrażenia DbInExpression, który został dodany, aby rozwiązać problemy z wydajnością dotyczące użytkowania Enumerable.Contains w kwerendach LINQ. Klasa DbProviderManifest ma nowej metody wirtualnej SupportsInExpression, które jest wywoływane przez EF, aby określić, jeśli dostawca obsługuje nowy typ wyrażenia. Dla zachowania zgodności z istniejącej implementacji dostawcy, metoda zwraca wartość false. Aby korzystać z tego ulepszenia, dostawcę EF6 można dodać kod do obsługi DbInExpression i zastąpić SupportsInExpression aby zwracała wartość true. Przez wywołanie metody DbExpressionBuilder.In, można utworzyć wystąpienia elementu DbInExpression. Wystąpienie DbInExpression składa się z DbExpression, zwykle reprezentujący kolumnę tabeli oraz listę DbConstantExpression do testowania pod kątem dopasowania.

## <a name="nuget-packages-for-providers"></a>Pakiety NuGet dla dostawców

Jednym ze sposobów, aby udostępnić dostawcę EF6 jest zwolnij go jako pakiet NuGet. Przy użyciu pakietu NuGet ma następujące zalety:

*   Jest łatwa w użyciu pakietu NuGet, aby dodać rejestrację dostawcy do pliku konfiguracji aplikacji
*   Dodatkowych zmian do pliku konfiguracji można ustawić domyślną fabrykę połączenia tak, aby nawiązać połączenia przy Konwencji użyje zarejestrowanego dostawcy
*   NuGet obsługuje dodanie przekierowań powiązań, aby dostawca EF6 będą nadal działać, nawet po opublikowaniu nowego pakietu EF

Na przykład to pakiet EntityFramework.SqlServerCompact, który jest dostępny w ["open source" codebase](http://github.com/aspnet/entityframework6). Ten pakiet zawiera dobre szablon do tworzenia dostawcy EF pakietów NuGet.

### <a name="powershell-commands"></a>Polecenia programu PowerShell

Po zainstalowaniu pakietu EntityFramework NuGet rejestruje moduł programu PowerShell, który zawiera dwa polecenia, które są bardzo przydatne w przypadku pakietów dostawcy:

*   Dodaj EFProvider dodaje nową jednostkę dla dostawcy w pliku konfiguracji projektu docelowego i upewnia się, że jest na końcu listę zarejestrowanych dostawców.
*   Dodaj EFDefaultConnectionFactory dodaje lub aktualizuje rejestracji defaultConnectionFactory w pliku konfiguracji projektu docelowego.

Oba te polecenia zajmie się dodanie sekcji entityFramework do pliku konfiguracji i dodawania kolekcji dostawców, jeśli to konieczne.

Jest ona przeznaczona, że te polecenia można wywołać z install.ps1 skryptu NuGet. Na przykład install.ps1 dla dostawcy SQL Compact wygląda podobnie do następującej:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Więcej informacji na temat tych poleceń można uzyskać przy użyciu get-help w oknie Konsola Menedżera pakietów.

## <a name="wrapping-providers"></a>Zawijanie dostawców

Dostawca zawijania to dostawcy EF i/lub ADO.NET, który otacza istniejącego dostawcy rozszerzać go za pomocą innych funkcje, takie jak profilowania, lub śledzenia możliwości. Zawijanie dostawcy mogą być rejestrowane w normalny sposób, ale jest często bardziej wygodne skonfigurować dostawcę zawijania w czasie wykonywania, przechwytuje rozwiązania związane z dostawcą usług. OnLockingConfiguration zdarzeń statycznych w klasie DbConfiguration można to zrobić.

OnLockingConfiguration jest wywoływana po EF stwierdził, gdzie otrzymany EF konfiguracji dla domeny aplikacji, ale zanim zostanie zablokowane do użycia. Przy uruchamianiu aplikacji (przed użyciem EF) aplikacji należy zarejestrować program obsługi zdarzeń dla tego zdarzenia. (Są rozważane, dodanie obsługi rejestrowania ten program obsługi w pliku konfiguracji, ale to nie jest jeszcze obsługiwane). Program obsługi zdarzeń powinny następnie wywoływania ReplaceService dla każdej usługi, który ma zostać opakowany.  

Na przykład aby zawijać IDbConnectionFactory i DbProviderService, powinny być rejestrowane obsługi podobnie do następującej:

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

Usługa, która został rozwiązany i teraz powinna być otoczona wraz z kluczem, który został użyty do rozwiązania usługi są przekazywane do programu obsługi. Program obsługi można opakować tę usługę i Zastąp zwrócone usługi przy użyciu opakowanej wersji.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Rozpoznawanie DbProviderFactory przy użyciu programu EF

DbProviderFactory jest jednym z typów podstawowych dostawcy wymagane przez EF, zgodnie z opisem w _dostawcy typów Przegląd_ powyższej sekcji. Jak już wspomniano, nie jest typem EF i rejestracji zazwyczaj nie jest częścią konfiguracji EF, ale zamiast tego jest normalne rejestrację dostawcy ADO.NET w pliku machine.config lub pliku konfiguracji aplikacji.

Pomimo tego EF nadal używa jej mechanizm rozpoznawania zależności normalne podczas wyszukiwania DbProviderFactory do użycia. Domyślny mechanizm rozwiązywania konfliktów używa normalnej rejestracji ADO.NET w plikach konfiguracji, a więc jest to zazwyczaj niewidoczny. Jednak ze względu na rozwiązanie normalne zależności jest używany mechanizm oznacza, że IDbDependencyResolver może służyć do rozwiązania DbProviderFactory, nawet wtedy, gdy nie została wykonana normalnej rejestracji ADO.NET.

Rozpoznawanie DbProviderFactory w ten sposób ma kilka skutki:

*   Aplikację przy użyciu konfiguracji na podstawie kodu można dodać wywołania w klasie ich DbConfiguration w taki sposób, aby zarejestrować DbProviderFactory odpowiednie. Jest to szczególnie przydatne w przypadku aplikacji, które nie mają być (lub nie) należy użyć dowolnej z opartą na plikach konfiguracji na wszystkich.
*   Usługa może zostać zawinięty lub zastąpić, korzystając z ReplaceService zgodnie z opisem w _zawijania dostawców_ powyższej sekcji
*   Teoretycznie implementacji DbProviderServices można rozpoznać DbProviderFactory.

Istotną kwestią należy zwrócić uwagę na sposób żadnej z tych czynności jest, że wpływają one tylko wyszukiwania DbProviderFactory przez EF. Inny kod bez EF mogą nadal oczekiwać dostawcy ADO.NET do zarejestrowania się w zwykły sposób i może się nie powieść, jeśli rejestracja nie zostanie znaleziony. Z tego powodu jest zwykle lepszym miejscem dla DbProviderFactory do zarejestrowania się w zwykły sposób ADO.NET.

### <a name="related-services"></a>Powiązane usługi

Jeśli EF jest używany do rozpoznawania DbProviderFactory, również go powinno rozwiązać IProviderInvariantName i IDbProviderFactoryResolver usług.

IProviderInvariantName to usługa, która jest używana do określenia dla danego typu DbProviderFactory nazwę niezmienną dostawcy. Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawcy ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozwiązywany za EF, następnie również będzie wymagany do rozwiązania tej usługi. Należy pamiętać, że program rozpoznawania nazw dla tej usługi jest automatycznie dodawane, korzystając z metody DbConfiguration.SetProviderFactory.

Zgodnie z opisem w _dostawcy typów Przegląd_ Powyższa sekcja, IDbProviderFactoryResolver jest używany do uzyskiwania poprawne DbProviderFactory z danego obiektu DbConnection. Domyślna implementacja tej usługi podczas uruchamiania na .NET 4 używa rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawcy ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozwiązywany za EF, następnie również będzie wymagany do rozwiązania tej usługi.
