---
title: Ustawienia pliku konfiguracji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417974"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="25785-102">Ustawienia pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="25785-102">Configuration File Settings</span></span>
<span data-ttu-id="25785-103">Entity Framework umożliwia określenie wielu ustawień z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25785-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="25785-104">Ogólnie EF stosuje się zasadę "Konwencja przed konfiguracją": wszystkie ustawienia omówione w tym wpisie mają zachowanie domyślne, ale trzeba się martwić o zmianę ustawienia, gdy wartość domyślna nie spełnia już wymagań.</span><span class="sxs-lookup"><span data-stu-id="25785-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="25785-105">Alternatywa oparta na kodzie</span><span class="sxs-lookup"><span data-stu-id="25785-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="25785-106">Wszystkie te ustawienia można również zastosować przy użyciu kodu.</span><span class="sxs-lookup"><span data-stu-id="25785-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="25785-107">Począwszy od EF6 wprowadziliśmy [konfigurację opartą na kodzie](code-based.md), która stanowi centralny sposób zastosowania konfiguracji z kodu.</span><span class="sxs-lookup"><span data-stu-id="25785-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="25785-108">W systemach starszych niż EF6 konfiguracja może być nadal stosowana z kodu, ale w celu skonfigurowania różnych obszarów należy użyć różnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="25785-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="25785-109">Opcja plik konfiguracji pozwala łatwo zmieniać te ustawienia podczas wdrażania bez aktualizowania kodu.</span><span class="sxs-lookup"><span data-stu-id="25785-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="25785-110">Sekcja konfiguracji Entity Framework</span><span class="sxs-lookup"><span data-stu-id="25785-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="25785-111">Począwszy od EF 4.1 można ustawić inicjator bazy danych dla kontekstu przy użyciu sekcji **AppSettings** w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25785-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="25785-112">W EF 4,3 wprowadzono niestandardową sekcję **entityFramework** do obsługi nowych ustawień.</span><span class="sxs-lookup"><span data-stu-id="25785-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="25785-113">Entity Framework nadal będzie rozpoznawał inicjatory bazy danych przy użyciu starego formatu, ale zalecamy przechodzenie do nowego formatu, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="25785-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="25785-114">Sekcja **entityFramework** została automatycznie dodana do pliku konfiguracji projektu po zainstalowaniu pakietu NuGet entityFramework.</span><span class="sxs-lookup"><span data-stu-id="25785-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="25785-115">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="25785-115">Connection Strings</span></span>  

<span data-ttu-id="25785-116">[Ta strona](~/ef6/fundamentals/configuring/connection-strings.md) zawiera więcej informacji na temat sposobu, w jaki Entity Framework określa bazę danych, która ma być używana, w tym parametry połączenia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25785-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="25785-117">Parametry połączenia przechodzą do standardowego elementu **connectionStrings** i nie wymagają sekcji **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="25785-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="25785-118">Modele oparte na Code First używają zwykłych parametrów połączenia ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="25785-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="25785-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="25785-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="25785-120">Modele oparte na programie Dr Designer używają specjalnych parametrów połączenia EF.</span><span class="sxs-lookup"><span data-stu-id="25785-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="25785-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="25785-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="25785-122">Typ konfiguracji oparty na kodzie (EF6 lub nowszym)</span><span class="sxs-lookup"><span data-stu-id="25785-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="25785-123">Począwszy od EF6, można określić dbconfiguration for EF do użycia w [konfiguracji opartej na kodzie](code-based.md) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25785-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="25785-124">W większości przypadków nie trzeba podawać tego ustawienia, ponieważ EF automatycznie odnajdzie konfigurację dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="25785-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="25785-125">Aby uzyskać szczegółowe informacje na temat sytuacji, w których może być konieczne określenie dbconfiguration w pliku konfiguracji, zobacz sekcję **przeniesienie Dbconfiguration** [konfiguracji opartej na kodzie](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="25785-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="25785-126">Aby ustawić typ dbconfiguration, należy określić nazwę typu kwalifikowanego zestawu w elemencie **codeConfigurationType** .</span><span class="sxs-lookup"><span data-stu-id="25785-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="25785-127">Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ.</span><span class="sxs-lookup"><span data-stu-id="25785-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="25785-128">Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="25785-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="25785-129">Dostawcy bazy danych EF (EF6 lub nowszym)</span><span class="sxs-lookup"><span data-stu-id="25785-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="25785-130">Przed EF6 należy uwzględnić Entity Framework części dostawcy bazy danych jako część podstawowego dostawcy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="25785-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="25785-131">Począwszy od EF6, te części EF są teraz zarządzane i rejestrowane osobno.</span><span class="sxs-lookup"><span data-stu-id="25785-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="25785-132">Zwykle nie trzeba samodzielnie rejestrować dostawców.</span><span class="sxs-lookup"><span data-stu-id="25785-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="25785-133">Jest to zwykle wykonywane przez dostawcę podczas instalacji.</span><span class="sxs-lookup"><span data-stu-id="25785-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="25785-134">Dostawcy są rejestrowani przez dołączenie elementu **dostawcy** w sekcji podrzędnej **dostawcy** sekcji **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="25785-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="25785-135">Istnieją dwa wymagane atrybuty dla wpisu dostawcy:</span><span class="sxs-lookup"><span data-stu-id="25785-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="25785-136">**niezmiennaname** identyfikuje podstawowego dostawcę ADO.NET, którego celem jest ten dostawca EF</span><span class="sxs-lookup"><span data-stu-id="25785-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="25785-137">**Typ** to kwalifikowana nazwa typu zestawu dla implementacji dostawcy EF</span><span class="sxs-lookup"><span data-stu-id="25785-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="25785-138">Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ.</span><span class="sxs-lookup"><span data-stu-id="25785-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="25785-139">Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="25785-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="25785-140">Przykładem jest wpis utworzony w celu zarejestrowania domyślnego dostawcy SQL Server podczas instalacji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25785-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="25785-141">Interceptory (EF 6.1 i nowsze)</span><span class="sxs-lookup"><span data-stu-id="25785-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="25785-142">Począwszy od EF 6.1, można zarejestrować Interceptory w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="25785-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="25785-143">Interceptory umożliwiają uruchamianie dodatkowej logiki, gdy EF wykonuje pewne operacje, takie jak wykonywanie zapytań bazy danych, otwieranie połączeń itp.</span><span class="sxs-lookup"><span data-stu-id="25785-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="25785-144">Interceptory są rejestrowane przez dołączenie elementu **interceptora** w sekcji podrzędnej **Interceptory** w sekcji **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="25785-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="25785-145">Na przykład następująca konfiguracja rejestruje wbudowany Interceptor **DatabaseLogger** , który będzie rejestrował wszystkie operacje bazy danych w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="25785-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="25785-146">Rejestrowanie operacji bazy danych w pliku (EF 6.1 lub nowszym)</span><span class="sxs-lookup"><span data-stu-id="25785-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="25785-147">Rejestrowanie przechwyceń za pośrednictwem pliku konfiguracji jest szczególnie przydatne w przypadku dodawania rejestrowania do istniejącej aplikacji w celu ułatwienia debugowania problemu.</span><span class="sxs-lookup"><span data-stu-id="25785-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="25785-148">**DatabaseLogger** obsługuje rejestrowanie do pliku, dostarczając nazwę pliku jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="25785-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="25785-149">Domyślnie spowoduje to zastąpienie pliku dziennika nowym plikiem przy każdym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25785-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="25785-150">Aby zamiast tego dołączyć do pliku dziennika, jeśli już istnieje, można go użyć:</span><span class="sxs-lookup"><span data-stu-id="25785-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="25785-151">Aby uzyskać dodatkowe informacje na temat **DatabaseLogger** i rejestrowania interceptorów, zobacz wpis w blogu [EF 6,1: Włączanie rejestrowania bez ponownego kompilowania](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="25785-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="25785-152">Code First domyślną fabrykę połączeń</span><span class="sxs-lookup"><span data-stu-id="25785-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="25785-153">Sekcja konfiguracji umożliwia określenie domyślnej fabryki połączeń, która Code First powinna być używana do lokalizowania bazy danych używanej w kontekście.</span><span class="sxs-lookup"><span data-stu-id="25785-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="25785-154">Domyślna fabryka połączeń jest używana tylko wtedy, gdy żadne parametry połączenia nie zostały dodane do pliku konfiguracji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="25785-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="25785-155">Po zainstalowaniu pakietu NuGet EF zarejestrowano domyślną fabrykę połączeń, która wskazuje na SQL Express lub LocalDB, w zależności od tego, który z nich jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="25785-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="25785-156">Aby ustawić fabrykę połączeń, należy określić nazwę typu kwalifikowanego zestawu w elemencie **defaultConnectionFactory** .</span><span class="sxs-lookup"><span data-stu-id="25785-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="25785-157">Kwalifikowana nazwa zestawu to kwalifikowana nazwa przestrzeni nazw, a następnie przecinek, a następnie zestaw, w którym znajduje się ten typ.</span><span class="sxs-lookup"><span data-stu-id="25785-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="25785-158">Opcjonalnie można również określić wersję zestawu, kulturę i token klucza publicznego.</span><span class="sxs-lookup"><span data-stu-id="25785-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="25785-159">Oto przykład ustawiania domyślnej fabryki połączeń:</span><span class="sxs-lookup"><span data-stu-id="25785-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="25785-160">Powyższy przykład wymaga, aby fabryka niestandardowa miała konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="25785-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="25785-161">W razie konieczności można określić parametry konstruktora przy użyciu elementu **Parameters** .</span><span class="sxs-lookup"><span data-stu-id="25785-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="25785-162">Na przykład SqlCeConnectionFactory, który jest zawarty w Entity Framework, wymaga podania niezmiennej nazwy dostawcy do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="25785-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="25785-163">Niezmienna nazwa dostawcy identyfikuje wersję programu SQL Compact, której chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="25785-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="25785-164">Następująca konfiguracja spowoduje, że konteksty domyślnie korzystają z programu SQL Compact w wersji 4,0.</span><span class="sxs-lookup"><span data-stu-id="25785-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="25785-165">Jeśli nie ustawisz domyślnej fabryki połączeń, Code First używa SqlConnectionFactory, wskazując `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="25785-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="25785-166">SqlConnectionFactory ma także konstruktora, który umożliwia przesłonięcie części parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="25785-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="25785-167">Jeśli chcesz użyć wystąpienia SQL Server innego niż `.\SQLEXPRESS` można użyć tego konstruktora do ustawienia serwera.</span><span class="sxs-lookup"><span data-stu-id="25785-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="25785-168">Następująca konfiguracja spowoduje, że Code First używać **MyDatabaseServer** dla kontekstów, które nie mają jawnie ustawionych parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="25785-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="25785-169">Domyślnie przyjmuje się, że argumenty konstruktora są typu String.</span><span class="sxs-lookup"><span data-stu-id="25785-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="25785-170">Możesz użyć atrybutu typu, aby to zmienić.</span><span class="sxs-lookup"><span data-stu-id="25785-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="25785-171">Inicjatory bazy danych</span><span class="sxs-lookup"><span data-stu-id="25785-171">Database Initializers</span></span>  

<span data-ttu-id="25785-172">Inicjatory bazy danych są konfigurowane dla poszczególnych kontekstów.</span><span class="sxs-lookup"><span data-stu-id="25785-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="25785-173">Można je ustawić w pliku konfiguracji przy użyciu elementu **Context** .</span><span class="sxs-lookup"><span data-stu-id="25785-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="25785-174">Ten element używa kwalifikowanej nazwy zestawu do identyfikowania konfigurowanego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="25785-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="25785-175">Domyślnie konteksty Code First są skonfigurowane do używania inicjatora CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="25785-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="25785-176">W elemencie **kontekstu** istnieje atrybut **disableDatabaseInitialization** , którego można użyć do wyłączenia inicjowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="25785-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="25785-177">Na przykład następująca konfiguracja wyłącza inicjalizację bazy danych dla kontekstu blog. BlogContext zdefiniowanego w pliku. dll.</span><span class="sxs-lookup"><span data-stu-id="25785-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="25785-178">Można użyć elementu **databaseInitializer** , aby ustawić inicjatora niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="25785-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="25785-179">Parametry konstruktora używają tej samej składni co domyślne fabryki połączeń.</span><span class="sxs-lookup"><span data-stu-id="25785-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="25785-180">Istnieje możliwość skonfigurowania jednego z inicjatorów ogólnych baz danych, które znajdują się w Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25785-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="25785-181">Atrybut **Type** używa formatu .NET Framework dla typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="25785-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="25785-182">Na przykład, jeśli używasz Migracje Code First, można skonfigurować bazę danych do automatycznego migrowania przy użyciu inicjatora `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>`.</span><span class="sxs-lookup"><span data-stu-id="25785-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
