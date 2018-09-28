---
title: Ustawienia pliku konfiguracji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: faba4e406b9f26f5bed6149f75c59da362d84692
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415786"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="76bba-102">Ustawienia pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="76bba-102">Configuration File Settings</span></span>
<span data-ttu-id="76bba-103">Entity Framework umożliwia wiele ustawień, należy określić w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76bba-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="76bba-104">Ogólnie rzecz biorąc EF jest zgodna z zasadą "Konwencja, za pośrednictwem konfiguracji": wszystkie ustawienia, które są omawiane w tym wpisie ma domyślne zachowanie, musisz się martwić o zmianę ustawienia, gdy domyślny nie spełnia wymagań.</span><span class="sxs-lookup"><span data-stu-id="76bba-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="76bba-105">To oparte na kodzie alternatywa</span><span class="sxs-lookup"><span data-stu-id="76bba-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="76bba-106">Wszystkie te ustawienia można również będą stosowane przy użyciu kodu.</span><span class="sxs-lookup"><span data-stu-id="76bba-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="76bba-107">Począwszy od platformy EF6 wprowadziliśmy [konfiguracja na podstawie kodu](code-based.md), który zapewnia centralny sposób stosowania konfiguracji z poziomu kodu.</span><span class="sxs-lookup"><span data-stu-id="76bba-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="76bba-108">Przed EF6 nadal można zastosować konfiguracji z kodu, ale trzeba skonfigurować różne obszary za pomocą różnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="76bba-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="76bba-109">Opcja pliku konfiguracji umożliwia te ustawienia można łatwo zmienić podczas wdrażania bez aktualizowania kodu.</span><span class="sxs-lookup"><span data-stu-id="76bba-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="76bba-110">Sekcja konfiguracji programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="76bba-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="76bba-111">Uruchamianie EF4.1 można ustawić inicjator bazy danych przy użyciu kontekstu **appSettings** sekcję pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76bba-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="76bba-112">W wersji 4.3 platformy EF wprowadziliśmy niestandardowej **entityFramework** sekcji, aby obsługiwać nowe ustawienia.</span><span class="sxs-lookup"><span data-stu-id="76bba-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="76bba-113">Entity Framework nadal będzie także rozpoznawał inicjatory bazy danych można ustawić przy użyciu starego formatu, ale zaleca się przejście na nowy format, gdzie to możliwe.</span><span class="sxs-lookup"><span data-stu-id="76bba-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="76bba-114">**EntityFramework** sekcji została automatycznie dodana do pliku konfiguracji projektu, po zainstalowaniu pakiet NuGet platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="76bba-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="76bba-115">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="76bba-115">Connection Strings</span></span>  

<span data-ttu-id="76bba-116">[Ta strona](~/ef6/fundamentals/configuring/connection-strings.md) więcej szczegółów na temat jak Entity Framework określa bazy danych ma być używana, w tym parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76bba-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="76bba-117">Parametry połączenia, przejdź w standardzie **connectionStrings** elementu i nie wymagają **entityFramework** sekcji.</span><span class="sxs-lookup"><span data-stu-id="76bba-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="76bba-118">Modele kodu najpierw na podstawie Użyj normalnej parametry połączenia ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="76bba-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="76bba-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="76bba-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="76bba-120">Projektancie platformy EF na podstawie parametrów połączenia platformy EF specjalne użycia modeli.</span><span class="sxs-lookup"><span data-stu-id="76bba-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="76bba-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="76bba-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="76bba-122">Typ konfiguracji oparte na kodzie (od wersji EF6)</span><span class="sxs-lookup"><span data-stu-id="76bba-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="76bba-123">Począwszy od platformy EF6, można określić DbConfiguration na platformie EF na potrzeby [konfiguracja na podstawie kodu](code-based.md) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76bba-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="76bba-124">W większości przypadków nie trzeba określić tego ustawienia, jak EF automatycznie wykryje Twoje DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="76bba-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="76bba-125">Zobacz szczegóły po użytkownik może być konieczne określenie DbConfiguration w pliku config **przenoszenie DbConfiguration** części [konfiguracja na podstawie kodu](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="76bba-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="76bba-126">Aby ustawić typ DbConfiguration, należy określić nazwę typu kwalifikowanego zestawu w **codeConfigurationType** elementu.</span><span class="sxs-lookup"><span data-stu-id="76bba-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="76bba-127">Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w.</span><span class="sxs-lookup"><span data-stu-id="76bba-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76bba-128">Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="76bba-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="76bba-129">Dostawcy baz danych EF (od wersji EF6)</span><span class="sxs-lookup"><span data-stu-id="76bba-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="76bba-130">Przed EF6, musiała być dołączane jako część dostawcy ADO.NET core Framework określonej części dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76bba-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="76bba-131">Począwszy od platformy EF6 EF określone fragmenty są teraz zarządzane i zarejestrowane oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="76bba-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="76bba-132">Zwykle nie trzeba samodzielnie zarejestrować dostawców.</span><span class="sxs-lookup"><span data-stu-id="76bba-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="76bba-133">To są zwykle wykonywane przez dostawcę po jego zainstalowaniu.</span><span class="sxs-lookup"><span data-stu-id="76bba-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="76bba-134">Dostawcy są rejestrowane przez dołączenie **dostawcy** pod **dostawców** części podrzędnej **entityFramework** sekcji.</span><span class="sxs-lookup"><span data-stu-id="76bba-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="76bba-135">Istnieją dwa atrybuty wymagane dla wpisu dostawcy:</span><span class="sxs-lookup"><span data-stu-id="76bba-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="76bba-136">**Invatiantname** identyfikuje dostawcy ADO.NET core że EF dostawcy cele</span><span class="sxs-lookup"><span data-stu-id="76bba-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="76bba-137">**Typ** jest kwalifikowaną nazwą typu zestawu EF implementacji dostawcy</span><span class="sxs-lookup"><span data-stu-id="76bba-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="76bba-138">Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w.</span><span class="sxs-lookup"><span data-stu-id="76bba-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76bba-139">Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="76bba-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="76bba-140">Na przykład Oto wpis utworzone w celu zarejestrowania domyślny dostawca programu SQL Server po zainstalowaniu programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="76bba-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="76bba-141">Interceptory (EF6.1 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="76bba-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="76bba-142">Uruchamianie EF6.1 można zarejestrować interceptory w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76bba-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="76bba-143">Interceptory umożliwiają uruchamianie dodatkowej logiki, gdy EF wykonuje niektóre operacje, takie jak wykonywanie kwerend bazy danych otwarcia połączeń, itp.</span><span class="sxs-lookup"><span data-stu-id="76bba-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="76bba-144">Interceptory są rejestrowane przez dołączenie **interceptor** pod **interceptory** części podrzędnej **entityFramework** sekcji.</span><span class="sxs-lookup"><span data-stu-id="76bba-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="76bba-145">Na przykład następująca konfiguracja rejestruje wbudowane **DatabaseLogger** interceptor, która zarejestruje wszystkie operacje bazy danych do konsoli.</span><span class="sxs-lookup"><span data-stu-id="76bba-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="76bba-146">Rejestrowanie operacji bazy danych do pliku (EF6.1 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="76bba-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="76bba-147">Rejestrowanie interceptory za pomocą pliku konfiguracji jest szczególnie przydatne w przypadku, gdy użytkownik chce dodać rejestrowania do istniejącej aplikacji, aby pomóc w debugowaniu problemu.</span><span class="sxs-lookup"><span data-stu-id="76bba-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="76bba-148">**DatabaseLogger** obsługuje rejestrowanie do pliku przez podanie nazwy pliku jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="76bba-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="76bba-149">Domyślnie to spowoduje, że plik dziennika zostaną zastąpione za pomocą nowego pliku każdym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76bba-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="76bba-150">Aby dołączyć do dziennika pliku Jeśli już istnieje Użyj podobny do:</span><span class="sxs-lookup"><span data-stu-id="76bba-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="76bba-151">Aby uzyskać dodatkowe informacje na temat **DatabaseLogger** i rejestrowanie interceptory, zobacz wpis w blogu [EF 6.1: włączenie rejestrowania bez konieczności ponownego kompilowania](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="76bba-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="76bba-152">Fabryka połączenia domyślne pierwszy kodu</span><span class="sxs-lookup"><span data-stu-id="76bba-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="76bba-153">Sekcja konfiguracji pozwala określić domyślną fabrykę połączenia, która Code First powinna być używana do lokalizowania bazę danych do użycia dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="76bba-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="76bba-154">Domyślna fabryka połączenia jest używane tylko w sytuacji, gdy parametry połączenia, nie został dodany do pliku konfiguracji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="76bba-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="76bba-155">Po zainstalowaniu pakietu NuGet programu EF domyślną fabrykę połączenia został zarejestrowany, wskazujący SQL Express lub LocalDB, w zależności od tego, który z nich został zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="76bba-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="76bba-156">Aby ustawić fabryka połączenia, określ nazwę typu kwalifikowanego zestawu w **defaultConnectionFactory** elementu.</span><span class="sxs-lookup"><span data-stu-id="76bba-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="76bba-157">Kwalifikowana nazwa zestawu jest kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, następnie zestawu, który typ, który znajduje się w.</span><span class="sxs-lookup"><span data-stu-id="76bba-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76bba-158">Opcjonalnie możesz również określić zestaw wersji, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="76bba-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="76bba-159">Poniżej przedstawiono przykładową konfigurację własnych domyślną fabrykę połączenia:</span><span class="sxs-lookup"><span data-stu-id="76bba-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="76bba-160">Powyższy przykład wymaga niestandardowych fabryki, aby mieć konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="76bba-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="76bba-161">Jeśli to konieczne, można określić parametry konstruktora przy użyciu **parametry** elementu.</span><span class="sxs-lookup"><span data-stu-id="76bba-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="76bba-162">Na przykład SqlCeConnectionFactory, który znajduje się w programie Entity Framework, wymaga podania nazwę niezmienną dostawcy do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="76bba-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="76bba-163">Nazwa niezmienna dostawcy identyfikuje wersję programu SQL Compact chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="76bba-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="76bba-164">Następująca konfiguracja spowoduje, że kontekst do użycia w wersji SQL Compact 4.0 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="76bba-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="76bba-165">Jeśli nie ustawisz domyślną fabrykę połączenia Code First używa SqlConnectionFactory, wskazując `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="76bba-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="76bba-166">SqlConnectionFactory również ma konstruktora, który zezwala na zastąpienie części ciągu połączenia.</span><span class="sxs-lookup"><span data-stu-id="76bba-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="76bba-167">Jeśli chcesz użyć wystąpienia programu SQL Server w innych niż `.\SQLEXPRESS` można skonfigurować serwera, można użyć tego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="76bba-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="76bba-168">Następująca konfiguracja spowoduje, że Code First użyć **MyDatabaseServer** dla kontekstów, które nie mają ciągu jawne połączenie zestawu.</span><span class="sxs-lookup"><span data-stu-id="76bba-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="76bba-169">Domyślnie zakłada się, że argumenty konstruktora są typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="76bba-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="76bba-170">Aby zmienić to ustawienie, można użyć tego typu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76bba-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="76bba-171">Inicjatory bazy danych</span><span class="sxs-lookup"><span data-stu-id="76bba-171">Database Initializers</span></span>  

<span data-ttu-id="76bba-172">Inicjatory bazy danych są skonfigurowane na podstawie na kontekście.</span><span class="sxs-lookup"><span data-stu-id="76bba-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="76bba-173">Można je skonfigurować przy użyciu pliku konfiguracji **kontekstu** elementu.</span><span class="sxs-lookup"><span data-stu-id="76bba-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="76bba-174">Ten element używa nazwy kwalifikowanej zestawu do zidentyfikowania kontekstu jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="76bba-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="76bba-175">Domyślnie program Code First kontekstów są skonfigurowane do używania inicjatora CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="76bba-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="76bba-176">Brak **disableDatabaseInitialization** atrybutu na **kontekstu** element, który może służyć do wyłączenia inicjowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76bba-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="76bba-177">Na przykład następująca konfiguracja wyłącza inicjowanie bazy danych dla kontekstu Blogging.BlogContext zdefiniowane w MyAssembly.dll.</span><span class="sxs-lookup"><span data-stu-id="76bba-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="76bba-178">Możesz użyć **databaseInitializer** elementu, aby ustawić niestandardowe inicjatora.</span><span class="sxs-lookup"><span data-stu-id="76bba-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="76bba-179">Parametry Konstruktora użyć tej samej składni jako domyślnego połączenia fabryk.</span><span class="sxs-lookup"><span data-stu-id="76bba-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="76bba-180">Można skonfigurować jeden inicjatory ogólną bazę danych, które są objęte Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="76bba-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="76bba-181">**Typu** atrybutu używa formatu .NET Framework dla typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="76bba-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="76bba-182">Na przykład, jeśli używasz migracje Code First można skonfigurować bazy danych powinny być migrowane automatycznie przy użyciu `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicjatora.</span><span class="sxs-lookup"><span data-stu-id="76bba-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
