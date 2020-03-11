---
title: Rozpoznawanie zależności — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417863"
---
# <a name="dependency-resolution"></a>Rozwiązywanie zależności
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Począwszy od EF6, Entity Framework zawiera mechanizm ogólnego przeznaczenia do uzyskiwania implementacji wymaganych usług. Oznacza to, że gdy EF używa wystąpienia niektórych interfejsów lub klas bazowych, będzie pytał o konkretną implementację interfejsu lub klasy bazowej do użycia. Jest to realizowane za pomocą interfejsu IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Metoda GetService jest zazwyczaj wywoływana przez EF i jest obsługiwana przez implementację IDbDependencyResolver dostarczonej przez EF lub przez aplikację. Gdy wywoływana, argument typu jest interfejsem lub typem klasy bazowej żądanej usługi, a obiekt klucza ma wartość null lub obiekt zawierający informacje kontekstowe dotyczące żądanej usługi.  

O ile nie określono inaczej, wszelkie zwracane obiekty muszą być bezpieczne wątkowo, ponieważ mogą być używane jako pojedyncze. W wielu przypadkach zwracanym obiektem jest fabryka, w której przypadku sama fabryka musi być bezpieczna wątkowo, ale obiekt zwrócony z fabryki nie musi być bezpieczny wątkowo, ponieważ nowe wystąpienie jest wymagane od fabryki dla każdego użycia.

Ten artykuł nie zawiera pełnych informacji na temat implementowania IDbDependencyResolver, ale zamiast tego działa jako odwołanie do typów usług (czyli interfejsów i typów klas podstawowych), dla których EF wywołuje metodę GetService i semantykę obiektu klucza dla każdej z tych elementów Rozmowa.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System. Data. Entity. IDatabaseInitializer < TContext\>  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: inicjator bazy danych dla danego typu kontekstu  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System. Data. Entity. migrations. SQL. MigrationSqlGenerator\>  

**Wprowadzona wersja**: Dr 6.0.0

**Zwrócony obiekt**: Fabryka do tworzenia generatora SQL, który może służyć do migracji i innych akcji, które powodują utworzenie bazy danych, takich jak tworzenie bazy danych z inicjatorami bazy danych.  

**Klucz**: ciąg zawierający niezmienną nazwę dostawcy ADO.NET określającą typ bazy danych, dla której zostanie wygenerowany program SQL Server. Na przykład SQL Server Generator SQL jest zwracany dla klucza "System. Data. SqlClient".  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycorecommondbproviderservices"></a>System. Data. Entity. Core. Common. DbProviderServices  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: dostawca EF do użycia dla danej niezmiennej nazwy dostawcy  

**Klucz**: ciąg zawierający nazwę niezmiennej dostawcy ADO.NET określającą typ bazy danych, dla której jest wymagany dostawca. Na przykład dostawca SQL Server jest zwracany dla klucza "System. Data. SqlClient".  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System. Data. Entity. Infrastructure. IDbConnectionFactory  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Fabryka połączeń, która będzie używana podczas tworzenia połączenia z bazą danych za pomocą Konwencji EF. Oznacza to, że w przypadku braku połączenia lub parametrów połączenia z EF nie można odnaleźć parametrów połączenia w pliku App. config lub Web. config. Ta usługa jest używana do tworzenia połączenia według Konwencji. Zmiana fabryki połączeń może pozwolić, aby EF domyślnie używała innego typu bazy danych (na przykład SQL Server Compact Edition).  

**Klucz**: nieużywany; będzie mieć wartość null  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System. Data. Entity. Infrastructure. IManifestTokenService  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: usługa, która może generować token manifestu dostawcy z połączenia. Ta usługa jest zwykle używana na dwa sposoby. Po pierwsze, można go użyć, aby uniknąć Code First łączenia się z bazą danych podczas kompilowania modelu. Następnie można go użyć do wymuszenia Code First tworzenia modelu dla określonej wersji bazy danych — na przykład, aby wymusić model dla SQL Server 2005, nawet jeśli czasami SQL Server 2008 jest używany.  

**Okres istnienia obiektu**: pojedynczy — ten sam obiekt może być wielokrotnie używany i jednocześnie przez różne wątki  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System. Data. Entity. Infrastructure. IDbProviderFactoryService  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: usługa, która może uzyskać fabrykę dostawcy z danego połączenia. W przypadku programu .NET 4,5 dostawca jest publicznie dostępny w ramach połączenia. W programie .NET 4 domyślna implementacja tej usługi używa pewnych algorytmów heurystycznych w celu znalezienia pasującego dostawcy. Jeśli te błędy zakończą się niepowodzeniem, można zarejestrować nową implementację tej usługi, aby zapewnić odpowiednie rozwiązanie.  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, system. Data. Entity. Infrastructure. IDbModelCacheKey\>  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Fabryka, która będzie generować klucz pamięci podręcznej modelu dla danego kontekstu. Domyślnie EF zapisuje w pamięci podręcznej jeden model na typ kontekstu dbdla każdego dostawcy. Inna implementacja tej usługi może służyć do dodawania innych informacji, takich jak nazwa schematu, do klucza pamięci podręcznej.  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="systemdataentityspatialdbspatialservices"></a>System. Data. Entity. przestrzenny. DbSpatialServices  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: dostawca przestrzenny EF, który dodaje obsługę podstawowego dostawcy EF dla typów przestrzennych i geograficznych.  

**Klucz**: DbSptialServices jest proszony na dwa sposoby. Najpierw żąda się usług przestrzennych specyficznych dla dostawcy przy użyciu obiektu DbProviderInfo (który zawiera nazwę niezmienną i token manifestu) jako klucz. Po drugie, DbSpatialServices może być poproszony o brak klucza. Służy do rozpoznawania globalnego dostawcy przestrzennego, który jest używany podczas tworzenia autonomicznych typów DbGeography lub DbGeometry.  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\>  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Fabryka do tworzenia usługi, która umożliwia dostawcy wdrożenie ponownych prób lub inne zachowanie w przypadku wykonywania zapytań i poleceń względem bazy danych. Jeśli nie podano implementacji, EF po prostu wykona polecenia i propaguje wszystkie zgłoszone wyjątki. W przypadku SQL Server ta usługa jest używana w celu zapewnienia zasad ponawiania, która jest szczególnie przydatna w przypadku uruchamiania na serwerach bazy danych opartych na chmurze, takich jak SQL Azure.  

**Klucz**: obiekt ExecutionStrategyKey, który zawiera niezmienną nazwę dostawcy i opcjonalnie nazwę serwera, dla którego zostanie użyta strategia wykonywania.  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, String, system. Data. Entity. migrations. history. HistoryContext\>  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Fabryka, która pozwala dostawcy konfigurować mapowanie HistoryContext do tabeli `__MigrationHistory` używanej przez migracje EF. HistoryContext to Code First DbContext i można go skonfigurować przy użyciu normalnego interfejsu API Fluent, aby zmienić elementy, takie jak nazwa tabeli i specyfikacje mapowania kolumn.  

**Klucz**: nieużywany; będzie mieć wartość null  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdatacommondbproviderfactory"></a>System. Data. Common. DbProviderFactory  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: dostawca ADO.NET do użycia dla danej niezmiennej nazwy dostawcy.  

**Klucz**: ciąg zawierający niezmienną nazwę dostawcy ADO.NET  

>[!NOTE]
> Ta usługa nie jest zwykle zmieniana bezpośrednio od czasu implementacji domyślnej przy użyciu standardowej rejestracji dostawcy ADO.NET. Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System. Data. Entity. Infrastructure. IProviderInvariantName  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: usługa, która jest używana do określenia niezmiennej nazwy dostawcy dla danego typu DbProviderFactory. Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawca ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozpoznawany przez EF, konieczne będzie również rozwiązanie tej usługi.  

**Key**: Wystąpienie DbProviderFactory, dla którego wymagana jest nazwa niezmienna.  

>[!NOTE]
> Aby uzyskać więcej informacji na temat usług związanych z dostawcą w programie EF6, zobacz sekcję [model dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: pamięć podręczna zestawów, które zawierają wstępnie wygenerowane widoki. Zastąpienie jest zwykle używane do informowania EF o tym, które zestawy zawierają wstępnie wygenerowane widoki, bez konieczności odnajdywania.  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System. Data. Entity. Infrastructure. pluralizacja. IPluralizationService

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Usługa używana przez EF do pluralize i nazw nazwom. Domyślnie używana jest usługa pluralizacja w języku angielskim.  

**Klucz**: nieużywany; będzie mieć wartość null  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System. Data. Entity. Infrastructure. przechwytując. IDbInterceptor  

**Wprowadzona wersja**: Dr 6.0.0

**Zwrócone obiekty**: wszystkie Interceptory, które powinny być zarejestrowane podczas uruchamiania aplikacji. Należy zauważyć, że te obiekty są żądane przy użyciu wywołania GetServices i wszystkich przechwyceń zwracanych przez dowolny program rozpoznawania zależności zostanie zarejestrowany.

**Klucz**: nieużywany; będzie mieć wartość null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System. Data. Entity. DbContext, Akcja < ciąg\>, system. Data. Entity. Infrastructure. przechwytując. DatabaseLogFormatter\>  

**Wprowadzona wersja**: Dr 6.0.0  

**Zwrócony obiekt**: Fabryka, która zostanie użyta do utworzenia programu formatującego dziennika bazy danych, który będzie używany w przypadku kontekstu. Właściwość Database. log jest ustawiona w danym kontekście.  

**Klucz**: nieużywany; będzie mieć wartość null.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System. Data. Entity. DbContext\>  

**Wprowadzona wersja**: Dr 6.1.0  

**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia wystąpień kontekstu dla migracji, gdy kontekst nie ma dostępnego konstruktora bez parametrów.  

**Key**: obiekt Type dla typu pochodnego DbContext, dla którego jest wymagana fabryka.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\>  

**Wprowadzona wersja**: Dr 6.1.0  

**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia serializatorów do serializacji niestandardowych adnotacji o jednoznacznie określonym typie, które mogą być serializowane i desterylizowane w kodzie XML do użycia w migracje Code First.  

**Key**: nazwa adnotacji, która jest serializowana lub deserializowana.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System. Data. Entity. Infrastructure. TransactionHandler\>  

**Wprowadzona wersja**: Dr 6.1.0  

**Zwrócony obiekt**: Fabryka, która będzie używana do tworzenia programów obsługi dla transakcji, dzięki czemu można zastosować obsługę specjalną w przypadku sytuacji, takich jak obsługa niepowodzeń zatwierdzeń.  

**Klucz**: obiekt ExecutionStrategyKey, który zawiera niezmienną nazwę dostawcy i opcjonalnie nazwę serwera, dla którego zostanie użyta procedura obsługi transakcji.  
