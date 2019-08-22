---
title: Ustawienia pliku konfiguracji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886554"
---
# <a name="configuration-file-settings"></a>Ustawienia pliku konfiguracji
Entity Framework umożliwia określenie wielu ustawień z pliku konfiguracji. Ogólnie EF stosuje się zasadę "Konwencja przed konfiguracją": wszystkie ustawienia omówione w tym wpisie mają zachowanie domyślne, ale trzeba się martwić o zmianę ustawienia, gdy wartość domyślna nie spełnia już wymagań.  

## <a name="a-code-based-alternative"></a>Alternatywa oparta na kodzie  

Wszystkie te ustawienia można również zastosować przy użyciu kodu. Począwszy od EF6 wprowadziliśmy [konfigurację opartą na kodzie](code-based.md), która stanowi centralny sposób zastosowania konfiguracji z kodu. W systemach starszych niż EF6 konfiguracja może być nadal stosowana z kodu, ale w celu skonfigurowania różnych obszarów należy użyć różnych interfejsów API. Opcja plik konfiguracji pozwala łatwo zmieniać te ustawienia podczas wdrażania bez aktualizowania kodu.

## <a name="the-entity-framework-configuration-section"></a>Sekcja konfiguracji Entity Framework  

Począwszy od EF 4.1 można ustawić inicjator bazy danych dla kontekstu przy użyciu sekcji **AppSettings** w pliku konfiguracji. W EF 4,3 wprowadzono niestandardową sekcję **entityFramework** do obsługi nowych ustawień. Entity Framework nadal będzie rozpoznawał inicjatory bazy danych przy użyciu starego formatu, ale zalecamy przechodzenie do nowego formatu, jeśli jest to możliwe.

Sekcja **entityFramework** została automatycznie dodana do pliku konfiguracji projektu po zainstalowaniu pakietu NuGet entityFramework.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Parametry połączeń  

[Ta strona](~/ef6/fundamentals/configuring/connection-strings.md) zawiera więcej informacji na temat sposobu, w jaki Entity Framework określa bazę danych, która ma być używana, w tym parametry połączenia w pliku konfiguracji.  

Parametry połączenia przechodzą do standardowego elementu **connectionStrings** i nie wymagają sekcji **entityFramework** .  

Modele oparte na Code First używają zwykłych parametrów połączenia ADO.NET. Na przykład:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Modele oparte na programie Dr Designer używają specjalnych parametrów połączenia EF. Na przykład:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Typ konfiguracji oparty na kodzie (EF6 lub nowszym)  

Począwszy od EF6, można określić dbconfiguration for EF do użycia w [konfiguracji opartej na kodzie](code-based.md) w aplikacji. W większości przypadków nie trzeba podawać tego ustawienia, ponieważ EF automatycznie odnajdzie konfigurację dbconfiguration. Aby uzyskać szczegółowe informacje na temat sytuacji, w których może być konieczne określenie dbconfiguration w pliku konfiguracji, zobacz sekcję **przeniesienie Dbconfiguration** [konfiguracji opartej na kodzie](code-based.md).  

Aby ustawić typ dbconfiguration, należy określić nazwę typu kwalifikowanego zestawu w elemencie **codeConfigurationType** .  

> [!NOTE]
> Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ. Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Dostawcy bazy danych EF (EF6 lub nowszym)  

Przed EF6 należy uwzględnić Entity Framework części dostawcy bazy danych jako część podstawowego dostawcy ADO.NET. Począwszy od EF6, te części EF są teraz zarządzane i rejestrowane osobno.  

Zwykle nie trzeba samodzielnie rejestrować dostawców. Jest to zwykle wykonywane przez dostawcę podczas instalacji.  

Dostawcy są rejestrowani przez dołączenie elementu **dostawcy** w sekcji podrzędnej **dostawcy** sekcji **entityFramework** . Istnieją dwa wymagane atrybuty dla wpisu dostawcy:  

- niezmiennaname identyfikuje podstawowego dostawcę ADO.NET, którego celem jest ten dostawca EF  
- **Typ** to kwalifikowana nazwa typu zestawu dla implementacji dostawcy EF  

> [!NOTE]
> Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ. Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.  

Przykładem jest wpis utworzony w celu zarejestrowania domyślnego dostawcy SQL Server podczas instalacji Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptory (EF 6.1 i nowsze)  

Począwszy od EF 6.1, można zarejestrować Interceptory w pliku konfiguracji. Interceptory umożliwiają uruchamianie dodatkowej logiki, gdy EF wykonuje pewne operacje, takie jak wykonywanie zapytań bazy danych, otwieranie połączeń itp.  

Interceptory są rejestrowane przez dołączenie elementu **interceptora** w sekcji podrzędnej **Interceptory** w sekcji **entityFramework** . Na przykład następująca konfiguracja rejestruje wbudowany Interceptor **DatabaseLogger** , który będzie rejestrował wszystkie operacje bazy danych w konsoli programu.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Rejestrowanie operacji bazy danych w pliku (EF 6.1 lub nowszym)  

Rejestrowanie przechwyceń za pośrednictwem pliku konfiguracji jest szczególnie przydatne w przypadku dodawania rejestrowania do istniejącej aplikacji w celu ułatwienia debugowania problemu. **DatabaseLogger** obsługuje rejestrowanie do pliku, dostarczając nazwę pliku jako parametr konstruktora.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Domyślnie spowoduje to zastąpienie pliku dziennika nowym plikiem przy każdym uruchomieniu aplikacji. Aby zamiast tego dołączyć do pliku dziennika, jeśli już istnieje, można go użyć:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Aby uzyskać dodatkowe informacje na temat **DatabaseLogger** i rejestrowania interceptorów, zobacz wpis [w blogu EF 6,1: Włączenie rejestrowania bez ponownego kompilowania](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Code First domyślną fabrykę połączeń  

Sekcja konfiguracji umożliwia określenie domyślnej fabryki połączeń, która Code First powinna być używana do lokalizowania bazy danych używanej w kontekście. Domyślna fabryka połączeń jest używana tylko wtedy, gdy żadne parametry połączenia nie zostały dodane do pliku konfiguracji dla kontekstu.  

Po zainstalowaniu pakietu NuGet EF zarejestrowano domyślną fabrykę połączeń, która wskazuje na SQL Express lub LocalDB, w zależności od tego, który z nich jest zainstalowany.  

Aby ustawić fabrykę połączeń, należy określić nazwę typu kwalifikowanego zestawu w elemencie **defaultConnectionFactory** .  

> [!NOTE]
> Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ. Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.  

Oto przykład ustawiania domyślnej fabryki połączeń:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Powyższy przykład wymaga, aby fabryka niestandardowa miała konstruktora bez parametrów. W razie konieczności można określić parametry konstruktora przy użyciu elementu **Parameters** .  

Na przykład SqlCeConnectionFactory, który jest zawarty w Entity Framework, wymaga podania niezmiennej nazwy dostawcy do konstruktora. Niezmienna nazwa dostawcy identyfikuje wersję programu SQL Compact, której chcesz użyć. Następująca konfiguracja spowoduje, że konteksty domyślnie korzystają z programu SQL Compact w wersji 4,0.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Jeśli nie ustawisz domyślnej fabryki połączeń, Code First używa SqlConnectionFactory, wskazując na `.\SQLEXPRESS`. SqlConnectionFactory ma także konstruktora, który umożliwia przesłonięcie części parametrów połączenia. Jeśli chcesz użyć wystąpienia SQL Server innego niż `.\SQLEXPRESS` można użyć tego konstruktora do ustawienia serwera.  

Następująca konfiguracja spowoduje, że Code First używać **MyDatabaseServer** dla kontekstów, które nie mają jawnie ustawionych parametrów połączenia.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Domyślnie przyjmuje się, że argumenty konstruktora są typu String. Możesz użyć atrybutu typu, aby to zmienić.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicjatory bazy danych  

Inicjatory bazy danych są konfigurowane dla poszczególnych kontekstów. Można je ustawić w pliku konfiguracji przy użyciu elementu **Context** . Ten element używa kwalifikowanej nazwy zestawu do identyfikowania konfigurowanego kontekstu.  

Domyślnie konteksty Code First są skonfigurowane do używania inicjatora CreateDatabaseIfNotExists. W elemencie **kontekstu** istnieje atrybut **disableDatabaseInitialization** , którego można użyć do wyłączenia inicjowania bazy danych.  

Na przykład następująca konfiguracja wyłącza inicjalizację bazy danych dla kontekstu blog. BlogContext zdefiniowanego w pliku. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Można użyć elementu **databaseInitializer** , aby ustawić inicjatora niestandardowego.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Parametry konstruktora używają tej samej składni co domyślne fabryki połączeń.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Istnieje możliwość skonfigurowania jednego z inicjatorów ogólnych baz danych, które znajdują się w Entity Framework. Atrybut **Type** używa formatu .NET Framework dla typów ogólnych.  

Na przykład, jeśli używasz migracje Code First, można skonfigurować bazę danych do automatycznego migrowania przy użyciu `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicjatora.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
