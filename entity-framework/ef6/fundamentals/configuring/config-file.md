---
title: Ustawienia pliku konfiguracji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490879"
---
# <a name="configuration-file-settings"></a>Ustawienia pliku konfiguracji
Entity Framework umożliwia wiele ustawień, należy określić w pliku konfiguracji. Ogólnie rzecz biorąc EF jest zgodna z zasadą "Konwencja, za pośrednictwem konfiguracji": wszystkie ustawienia, które są omawiane w tym wpisie ma domyślne zachowanie, musisz się martwić o zmianę ustawienia, gdy domyślny nie spełnia wymagań.  

## <a name="a-code-based-alternative"></a>To oparte na kodzie alternatywa  

Wszystkie te ustawienia można również będą stosowane przy użyciu kodu. Począwszy od platformy EF6 wprowadziliśmy [konfiguracja na podstawie kodu](code-based.md), który zapewnia centralny sposób stosowania konfiguracji z poziomu kodu. Przed EF6 nadal można zastosować konfiguracji z kodu, ale trzeba skonfigurować różne obszary za pomocą różnych interfejsów API. Opcja pliku konfiguracji umożliwia te ustawienia można łatwo zmienić podczas wdrażania bez aktualizowania kodu.

## <a name="the-entity-framework-configuration-section"></a>Sekcja konfiguracji programu Entity Framework  

Uruchamianie EF4.1 można ustawić inicjator bazy danych przy użyciu kontekstu **appSettings** sekcję pliku konfiguracji. W wersji 4.3 platformy EF wprowadziliśmy niestandardowej **entityFramework** sekcji, aby obsługiwać nowe ustawienia. Entity Framework nadal będzie także rozpoznawał inicjatory bazy danych można ustawić przy użyciu starego formatu, ale zaleca się przejście na nowy format, gdzie to możliwe.

**EntityFramework** sekcji została automatycznie dodana do pliku konfiguracji projektu, po zainstalowaniu pakiet NuGet platformy EntityFramework.  

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

## <a name="connection-strings"></a>Parametry połączenia  

[Ta strona](~/ef6/fundamentals/configuring/connection-strings.md) więcej szczegółów na temat jak Entity Framework określa bazy danych ma być używana, w tym parametry połączenia w pliku konfiguracji.  

Parametry połączenia, przejdź w standardzie **connectionStrings** elementu i nie wymagają **entityFramework** sekcji.  

Modele kodu najpierw na podstawie Użyj normalnej parametry połączenia ADO.NET. Na przykład:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Projektancie platformy EF na podstawie parametrów połączenia platformy EF specjalne użycia modeli. Na przykład:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Typ konfiguracji oparte na kodzie (od wersji EF6)  

Począwszy od platformy EF6, można określić DbConfiguration na platformie EF na potrzeby [konfiguracja na podstawie kodu](code-based.md) w aplikacji. W większości przypadków nie trzeba określić tego ustawienia, jak EF automatycznie wykryje Twoje DbConfiguration. Zobacz szczegóły po użytkownik może być konieczne określenie DbConfiguration w pliku config **przenoszenie DbConfiguration** części [konfiguracja na podstawie kodu](code-based.md).  

Aby ustawić typ DbConfiguration, należy określić nazwę typu kwalifikowanego zestawu w **codeConfigurationType** elementu.  

> [!NOTE]
> Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w. Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Dostawcy baz danych EF (od wersji EF6)  

Przed EF6, musiała być dołączane jako część dostawcy ADO.NET core Framework określonej części dostawcy bazy danych. Począwszy od platformy EF6 EF określone fragmenty są teraz zarządzane i zarejestrowane oddzielnie.  

Zwykle nie trzeba samodzielnie zarejestrować dostawców. To są zwykle wykonywane przez dostawcę po jego zainstalowaniu.  

Dostawcy są rejestrowane przez dołączenie **dostawcy** pod **dostawców** części podrzędnej **entityFramework** sekcji. Istnieją dwa atrybuty wymagane dla wpisu dostawcy:  

- **Invatiantname** identyfikuje dostawcy ADO.NET core że EF dostawcy cele  
- **Typ** jest kwalifikowaną nazwą typu zestawu EF implementacji dostawcy  

> [!NOTE]
> Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w. Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.  

Na przykład Oto wpis utworzone w celu zarejestrowania domyślny dostawca programu SQL Server po zainstalowaniu programu Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptory (EF6.1 lub nowszy)  

Uruchamianie EF6.1 można zarejestrować interceptory w pliku konfiguracji. Interceptory umożliwiają uruchamianie dodatkowej logiki, gdy EF wykonuje niektóre operacje, takie jak wykonywanie kwerend bazy danych otwarcia połączeń, itp.  

Interceptory są rejestrowane przez dołączenie **interceptor** pod **interceptory** części podrzędnej **entityFramework** sekcji. Na przykład następująca konfiguracja rejestruje wbudowane **DatabaseLogger** interceptor, która zarejestruje wszystkie operacje bazy danych do konsoli.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Rejestrowanie operacji bazy danych do pliku (EF6.1 lub nowszy)  

Rejestrowanie interceptory za pomocą pliku konfiguracji jest szczególnie przydatne w przypadku, gdy użytkownik chce dodać rejestrowania do istniejącej aplikacji, aby pomóc w debugowaniu problemu. **DatabaseLogger** obsługuje rejestrowanie do pliku przez podanie nazwy pliku jako parametr konstruktora.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Domyślnie to spowoduje, że plik dziennika zostaną zastąpione za pomocą nowego pliku każdym uruchomieniu aplikacji. Aby dołączyć do dziennika pliku Jeśli już istnieje Użyj podobny do:  

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

Aby uzyskać dodatkowe informacje na temat **DatabaseLogger** i rejestrowanie interceptory, zobacz wpis w blogu [EF 6.1: włączenie rejestrowania bez konieczności ponownego kompilowania](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Fabryka połączenia domyślne pierwszy kodu  

Sekcja konfiguracji pozwala określić domyślną fabrykę połączenia, która Code First powinna być używana do lokalizowania bazę danych do użycia dla kontekstu. Domyślna fabryka połączenia jest używane tylko w sytuacji, gdy parametry połączenia, nie został dodany do pliku konfiguracji dla kontekstu.  

Po zainstalowaniu pakietu NuGet programu EF domyślną fabrykę połączenia został zarejestrowany, wskazujący SQL Express lub LocalDB, w zależności od tego, który z nich został zainstalowany.  

Aby ustawić fabryka połączenia, określ nazwę typu kwalifikowanego zestawu w **deafultConnectionFactory** elementu.  

> [!NOTE]
> Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w. Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.  

Poniżej przedstawiono przykładową konfigurację własnych domyślną fabrykę połączenia:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Powyższy przykład wymaga niestandardowych fabryki, aby mieć konstruktora bez parametrów. Jeśli to konieczne, można określić parametry konstruktora przy użyciu **parametry** elementu.  

Na przykład SqlCeConnectionFactory, który znajduje się w programie Entity Framework, wymaga podania nazwę niezmienną dostawcy do konstruktora. Nazwa niezmienna dostawcy identyfikuje wersję programu SQL Compact chcesz użyć. Następująca konfiguracja spowoduje, że kontekst do użycia w wersji SQL Compact 4.0 domyślnie.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Jeśli nie ustawisz domyślną fabrykę połączenia Code First używa SqlConnectionFactory, wskazując `.\SQLEXPRESS`. SqlConnectionFactory również ma konstruktora, który zezwala na zastąpienie części ciągu połączenia. Jeśli chcesz użyć wystąpienia programu SQL Server w innych niż `.\SQLEXPRESS` można skonfigurować serwera, można użyć tego konstruktora.  

Następująca konfiguracja spowoduje, że Code First użyć **MyDatabaseServer** dla kontekstów, które nie mają ciągu jawne połączenie zestawu.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Domyślnie zakłada się, że argumenty konstruktora są typu ciąg. Aby zmienić to ustawienie, można użyć tego typu atrybutu.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicjatory bazy danych  

Inicjatory bazy danych są skonfigurowane na podstawie na kontekście. Można je skonfigurować przy użyciu pliku konfiguracji **kontekstu** elementu. Ten element używa nazwy kwalifikowanej zestawu do zidentyfikowania kontekstu jest skonfigurowany.  

Domyślnie program Code First kontekstów są skonfigurowane do używania inicjatora CreateDatabaseIfNotExists. Brak **disableDatabaseInitialization** atrybutu na **kontekstu** element, który może służyć do wyłączenia inicjowanie bazy danych.  

Na przykład następująca konfiguracja wyłącza inicjowanie bazy danych dla kontekstu Blogging.BlogContext zdefiniowane w MyAssembly.dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Możesz użyć **databaseInitializer** elementu, aby ustawić niestandardowe inicjatora.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Parametry Konstruktora użyć tej samej składni jako domyślnego połączenia fabryk.  

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

Można skonfigurować jeden inicjatory ogólną bazę danych, które są objęte Entity Framework. **Typu** atrybutu używa formatu .NET Framework dla typów ogólnych.  

Na przykład, jeśli używasz migracje Code First można skonfigurować bazy danych powinny być migrowane automatycznie przy użyciu `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicjatora.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
