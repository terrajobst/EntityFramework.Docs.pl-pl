---
title: Rozpoznawanie zależności — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: c6c56c3048e17a5c888ffe564e7606abf8b0c4ed
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251248"
---
# <a name="dependency-resolution"></a>Rozpoznawanie zależności
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Począwszy od platformy EF6 Entity Framework zawiera mechanizm ogólnego przeznaczenia do uzyskania implementacji usług, które są wymagane. Oznacza to kiedy EF używa wystąpienia niektóre interfejsy lub klas bazowych zostanie wyświetlony monit dla konkretnej implementacji interfejsu lub klasy bazowej do użycia. Jest to osiągane przy użyciu interfejsu IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Metoda GetService zwykle jest wywoływana przez EF i jest obsługiwane przez implementację IDbDependencyResolver EF lub aplikacji. Gdy zostanie wywołana, argument typu jest typem klasy interfejsu lub base żądanej usługi i klucz obiektu jest wartość null lub obiekt dostarczający informacje kontekstowe o żądanej usługi.  

Jeżeli nie określono inaczej, dowolny obiekt zwrócony musi być metodą o bezpiecznych wątkach, ponieważ może służyć jako pojedynczą. W wielu przypadkach, w których obiekt zwrócony, w którym to przypadku to fabryka sama fabryka musi być metodą o bezpiecznych wątkach, ale obiekt zwrócony z fabryki nie musi być metodą o bezpiecznych wątkach, ponieważ zażądano nowe wystąpienie z fabryki dla każdego zastosowania.

Ten artykuł zawiera szczegółowe informacje o sposobie implementacji IDbDependencyResolver, ale zamiast tego działa jako odwołanie dla typów usługi (oznacza to, że interfejs i podstawowej klasy typy), dla których EF wywołuje GetService i semantyka obiekt klucza dla każdego z nich wywołuje.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: Inicjator bazy danych dla typu podanym kontekście  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>FUNC < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Wprowadzona w wersji**: EF6.0.0

**Obiekt zwrócony**: fabrykę do tworzenia generator SQL, który może służyć do migracji i inne akcje, które powodują bazy danych ma zostać utworzony, takie jak tworzenie bazy danych z inicjatorami bazy danych.  

**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET, określanie typu bazy danych, dla którego zostanie wygenerowany SQL. Na przykład generator programu SQL Server SQL jest zwracana dla klucza "System.Data.SqlClient".  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: Dostawca EF do użycia dla danego dostawcy o niezmiennej nazwie  

**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET, określanie typu bazy danych, dla której dostawca jest wymagana. Na przykład dostawca programu SQL Server jest zwracana dla klucza "System.Data.SqlClient".  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: fabryka połączenia, który będzie używany podczas EF, tworzenia połączenia z bazą danych według Konwencji. Oznacza to gdy nie połączenia lub parametry połączenia znajduje się do programu EF, a nie ciągu połączenia można znaleźć w pliku app.config lub web.config, następnie ta usługa służy do tworzenia połączenia zgodnie z Konwencją. Zmiana fabryka połączenia można zezwolić EF użyć innego typu bazy danych (na przykład SQL Server Compact Edition) domyślnie.  

**Klucz**: nie jest używany; będzie miał wartość null  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: usługi, który można wygenerować token manifestu dostawcy z połączenia. Ta usługa jest zwykle używana na dwa sposoby. Po pierwsze może służyć w celu uniknięcia Code First połączenie z bazą danych, podczas tworzenia modelu. Po drugie może służyć do wymuszenia Code First na budowanie modelu na potrzeby wersji konkretnej bazy danych — na przykład, aby wymusić model dla programu SQL Server 2005, nawet jeśli czasami jest używany program SQL Server 2008.  

**Okres istnienia obiektu**: pojedyncze — ten sam obiekt może być używana wiele razy, a jednocześnie przez inne wątki  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: usługi, który można uzyskać fabryki dostawcy z danego połączenia. W .NET 4.5 dostawca jest publicznie dostępny z poziomu połączenia. W .NET 4 Domyślna implementacja tej usługi używa niektóre heurystyki w celu odnalezienia pasującego dostawcy. Jeśli te nie powiodą się następnie nową metodę implementacji tej usługi można zarejestrować zapewniające odpowiednie rozwiązanie.  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>FUNC < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: fabryki, który zostanie wygenerowany klucz pamięci podręcznej modelu dla danego kontekstu. Domyślnie program EF buforuje jednego modelu dla typu DbContext dla dostawcy. Inną implementację tej usługi można dodać inne informacje, takie jak nazwa schematu do klucza pamięci podręcznej.  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: EF przestrzenne dostawcy, który dodaje obsługę podstawowych EF dostawcy typów przestrzennych geometry i położenia geograficznego.  

**Klucz**: DbSptialServices zostanie poproszony o na dwa sposoby. Po pierwsze, właściwe dla dostawcy usług przestrzennych, są żądane przy użyciu obiektu DbProviderInfo (zawierający niezmiennej token nazwy, a manifestu) jako klucza. Po drugie DbSpatialServices można monit o wpisanie bez klucza. Służy to rozwiązać "globalne przestrzenne dostawcę" który jest używany podczas tworzenia autonomicznego DbGeography lub DbGeometry typów.  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>FUNC < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: fabryki, aby utworzyć usługę, która umożliwia dostawcy zaimplementować ponownych prób lub inne zachowanie, gdy zapytań i poleceń, które są wykonywane względem bazy danych. Jeśli nie dostarczono żadnej implementacji, EF będzie po prostu wykonaj polecenia i propagowanie wyjątki zgłaszane. Dla programu SQL Server ta usługa służy do zapewnienia zasady ponawiania, co jest szczególnie przydatne, jeśli działających w odniesieniu do serwerów bazy danych opartej na chmurze, takich jak SQL Azure.  

**Klucz**: obiekt ExecutionStrategyKey, który zawiera nazwę niezmienną dostawcy i opcjonalnie nazwy serwera, dla której będzie służyć strategii wykonywania.  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>FUNC < DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\>  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: fabrykę, która umożliwia dostawcy skonfigurować mapowanie HistoryContext do `__MigrationHistory` tabeli używanej przez migracje EF. HistoryContext jest pierwszy typu DbContext kodu i można skonfigurować przy użyciu normalnych wygodnego interfejsu API można zmienić elementów, takich jak nazwy tabeli i specyfikacji mapowania kolumn.  

**Klucz**: nie jest używany; będzie miał wartość null  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: Dostawca ADO.NET do użycia dla danego dostawcy o niezmiennej nazwie.  

**Klucz**: ciąg zawierający nazwę niezmienną dostawcy ADO.NET  

>[!NOTE]
> Ta usługa nie jest zazwyczaj zmieniany bezpośrednio od implementacji domyślnej jest używana normalnej rejestracji dostawcy ADO.NET. Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: usługa, która jest używana do określenia dla danego typu DbProviderFactory nazwę niezmienną dostawcy. Domyślna implementacja tej usługi używa rejestracji dostawcy ADO.NET. Oznacza to, że jeśli dostawcy ADO.NET nie jest zarejestrowany w normalny sposób, ponieważ DbProviderFactory jest rozwiązywany za EF, następnie również będzie wymagany do rozwiązania tej usługi.  

**Klucz**: DbProviderFactory wystąpienia, dla których niezmienna nazwa jest wymagana.  

>[!NOTE]
> Zobacz szczegółowe informacje na temat związane z dostawcą usług w EF6 [modelu dostawca EF6](~/ef6/fundamentals/providers/provider-model.md) sekcji.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: pamięć podręczna zestawów, które zawierają wstępnie wygenerowanych widoków. Zazwyczaj służy zamiennika umożliwiające EF wiedzieć, zestawy, które zawierają wstępnie wygenerowanych widoków bez żadnych odnajdywania.  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: Usługa używana przez EF w liczbie mnogiej do nazw końcówek. Domyślnie usługa angielskiej pluralizacja jest używana.  

**Klucz**: nie jest używany; będzie miał wartość null  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Wprowadzona w wersji**: EF6.0.0

**Obiekty zwrócone**: wszelkie interceptory, które powinny być rejestrowane podczas uruchamiania aplikacji. Należy zauważyć, że te obiekty są żądane za pomocą wywołania funkcji GetServices i interceptory wszystkie zwrócone przez dowolnego mechanizmu rozpoznawania zależności zostanie zarejestrowana.

**Klucz**: nie jest używany; będzie miał wartość null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>FUNC < System.Data.Entity.DbContext, akcja < ciąg\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Wprowadzona w wersji**: EF6.0.0  

**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenia elementu formatującego dziennika bazy danych, który będzie używany podczas kontekstu. Database.Log właściwość jest ustawiona w danym kontekście.  

**Klucz**: nie jest używany; będzie miał wartość null.  

## <a name="funcsystemdataentitydbcontext"></a>FUNC < System.Data.Entity.DbContext\>  

**Wprowadzona w wersji**: EF6.1.0  

**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenia wystąpień kontekstu dla migracji, gdy kontekst nie jest dostępny konstruktor bez parametrów.  

**Klucz**: typ obiektu dla typu pochodnego typu DbContext potrzeby fabrykę.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>FUNC < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Wprowadzona w wersji**: EF6.1.0  

**Obiekt zwrócony**: fabrykę, która będzie służyć do utworzyć serializatorów serializacji silnie typizowane niestandardowe adnotacje taki sposób, że może być serializowany i desterilized do formatu XML do użycia w migracji Code First.  

**Klucz**: Nazwa adnotacji, który jest serializowany lub deserializowany.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>FUNC < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Wprowadzona w wersji**: EF6.1.0  

**Obiekt zwrócony**: fabrykę, która będzie służyć do tworzenie programów do obsługi transakcji, tak aby specjalnej obsługi można zastosować w sytuacjach, takich jak obsługa zatwierdzenia awarie.  

**Klucz**: obiekt ExecutionStrategyKey, który zawiera nazwę niezmienną dostawcy i opcjonalnie nazwy serwera, dla której będzie używany program obsługi transakcji.  
