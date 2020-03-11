---
title: Konfiguracja oparta na kodzie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417870"
---
# <a name="code-based-configuration"></a><span data-ttu-id="dacb6-102">Konfiguracja oparta na kodzie</span><span class="sxs-lookup"><span data-stu-id="dacb6-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="dacb6-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dacb6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="dacb6-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="dacb6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="dacb6-105">Konfigurację aplikacji Entity Framework można określić w pliku konfiguracji (App. config/Web. config) lub za pomocą kodu.</span><span class="sxs-lookup"><span data-stu-id="dacb6-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="dacb6-106">Ta ostatnia jest znana jako Konfiguracja oparta na kodzie.</span><span class="sxs-lookup"><span data-stu-id="dacb6-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="dacb6-107">Konfiguracja w pliku konfiguracji jest opisana w [osobnym artykule](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="dacb6-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="dacb6-108">Plik konfiguracji ma pierwszeństwo przed konfiguracją opartą na kodzie.</span><span class="sxs-lookup"><span data-stu-id="dacb6-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="dacb6-109">Innymi słowy, jeśli opcja konfiguracji jest ustawiona w kodzie i w pliku konfiguracji, zostanie użyta wartość ustawienia w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="dacb6-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="dacb6-110">Korzystanie z usługi dbconfiguration</span><span class="sxs-lookup"><span data-stu-id="dacb6-110">Using DbConfiguration</span></span>  

<span data-ttu-id="dacb6-111">Konfiguracja oparta na kodzie w EF6 i powyżej zostanie osiągnięta przez utworzenie podklasy system. Data. Entity. config. dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="dacb6-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="dacb6-112">Podczas podklasy dbconfiguration należy przestrzegać następujących wytycznych:</span><span class="sxs-lookup"><span data-stu-id="dacb6-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="dacb6-113">Utwórz tylko jedną klasę dbconfiguration dla swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dacb6-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="dacb6-114">Ta klasa określa ustawienia całej domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dacb6-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="dacb6-115">Umieść klasę dbconfiguration w tym samym zestawie, co Klasa DbContext.</span><span class="sxs-lookup"><span data-stu-id="dacb6-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="dacb6-116">(Zapoznaj się z sekcją *przeniesienie dbconfiguration* , jeśli chcesz ją zmienić).</span><span class="sxs-lookup"><span data-stu-id="dacb6-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="dacb6-117">Nadaj klasie dbconfiguration konstruktorowi publiczny bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="dacb6-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="dacb6-118">Ustawianie opcji konfiguracji przez wywoływanie metod chronionych dbconfiguration z poziomu tego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="dacb6-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="dacb6-119">Zgodnie z poniższymi wskazówkami firma Dr może automatycznie odnajdywać i korzystać z konfiguracji w ramach obu narzędzi, które muszą uzyskać dostęp do modelu i kiedy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="dacb6-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="dacb6-120">Przykład</span><span class="sxs-lookup"><span data-stu-id="dacb6-120">Example</span></span>  

<span data-ttu-id="dacb6-121">Klasa pochodna klasy dbconfiguration może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="dacb6-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="dacb6-122">Ta klasa konfiguruje EF do korzystania z strategii wykonywania SQL Azure — do automatycznego ponawiania próby wykonania nieudanych operacji w bazie danych — oraz do korzystania z lokalnej bazy danych dla baz danych utworzonych przy użyciu konwencji z Code First.</span><span class="sxs-lookup"><span data-stu-id="dacb6-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="dacb6-123">Przeniesienie dbconfiguration</span><span class="sxs-lookup"><span data-stu-id="dacb6-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="dacb6-124">Istnieją przypadki, w których nie można umieścić klasy dbconfiguration w tym samym zestawie, co Klasa DbContext.</span><span class="sxs-lookup"><span data-stu-id="dacb6-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="dacb6-125">Można na przykład mieć dwie klasy DbContext, każdy w różnych zestawach.</span><span class="sxs-lookup"><span data-stu-id="dacb6-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="dacb6-126">Dostępne są dwie opcje obsługi tego elementu.</span><span class="sxs-lookup"><span data-stu-id="dacb6-126">There are two options for handling this.</span></span>  

<span data-ttu-id="dacb6-127">Pierwsza opcja polega na użyciu pliku konfiguracji, aby określić wystąpienie dbconfiguration, które ma być używane.</span><span class="sxs-lookup"><span data-stu-id="dacb6-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="dacb6-128">W tym celu należy ustawić atrybut codeConfigurationType sekcji entityFramework.</span><span class="sxs-lookup"><span data-stu-id="dacb6-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="dacb6-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dacb6-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="dacb6-130">Wartość codeConfigurationType musi być zestawu i kwalifikowana nazwa przestrzeni nazw klasy dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="dacb6-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="dacb6-131">Druga opcja polega na umieszczeniu DbConfigurationTypeAttribute w klasie kontekstowej.</span><span class="sxs-lookup"><span data-stu-id="dacb6-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="dacb6-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dacb6-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="dacb6-133">Wartość przekazana do atrybutu może być typu dbconfiguration — jak pokazano powyżej — lub zestawu i przestrzeni nazw kwalifikowana nazwa typu.</span><span class="sxs-lookup"><span data-stu-id="dacb6-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="dacb6-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dacb6-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="dacb6-135">Jawne ustawienie dbconfiguration</span><span class="sxs-lookup"><span data-stu-id="dacb6-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="dacb6-136">Istnieją sytuacje, w których może być wymagana konfiguracja przed użyciem dowolnego typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="dacb6-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="dacb6-137">Przykłady takich elementów obejmują:</span><span class="sxs-lookup"><span data-stu-id="dacb6-137">Examples of this include:</span></span>  

- <span data-ttu-id="dacb6-138">Tworzenie modelu bez kontekstu przy użyciu DbModelBuilder</span><span class="sxs-lookup"><span data-stu-id="dacb6-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="dacb6-139">Korzystanie z innego kodu platformy/narzędzia, który korzysta z kontekstu DbContext, w którym ten kontekst jest używany przed użyciem kontekstu aplikacji</span><span class="sxs-lookup"><span data-stu-id="dacb6-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="dacb6-140">W takich sytuacjach EF nie jest możliwe automatyczne odnajdowanie konfiguracji i należy wykonać jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="dacb6-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="dacb6-141">Ustaw typ dbconfiguration w pliku konfiguracyjnym, zgodnie z opisem w sekcji *przeniesienie dbconfiguration* powyżej</span><span class="sxs-lookup"><span data-stu-id="dacb6-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="dacb6-142">Wywołaj metodę static dbconfiguration. SetConfiguration podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="dacb6-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="dacb6-143">Zastępowanie dbconfiguration</span><span class="sxs-lookup"><span data-stu-id="dacb6-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="dacb6-144">Istnieją sytuacje, w których należy zastąpić zestaw konfiguracyjny w konfiguracji dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="dacb6-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="dacb6-145">Nie jest to zwykle wykonywane przez deweloperów aplikacji, ale raczej od dostawców i wtyczek innych firm, które nie mogą używać pochodnej klasy dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="dacb6-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="dacb6-146">W tym celu EntityFramework umożliwia rejestrację programu obsługi zdarzeń, który może zmodyfikować istniejącą konfigurację tuż przed zablokowaniem.</span><span class="sxs-lookup"><span data-stu-id="dacb6-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="dacb6-147">Zapewnia również metodę cukru, w odniesieniu do zamiany każdej usługi zwróconej przez Lokalizator usługi EF.</span><span class="sxs-lookup"><span data-stu-id="dacb6-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="dacb6-148">Jest to sposób przeznaczony do użycia:</span><span class="sxs-lookup"><span data-stu-id="dacb6-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="dacb6-149">Podczas uruchamiania aplikacji (przed użyciem EF) wtyczka lub dostawca powinna zarejestrować metodę obsługi zdarzeń dla tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="dacb6-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="dacb6-150">(Należy pamiętać, że musi się to zdarzyć przed zastosowaniem EF przez aplikację.)</span><span class="sxs-lookup"><span data-stu-id="dacb6-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="dacb6-151">Program obsługi zdarzeń wywołuje ReplaceService dla każdej usługi, która musi zostać zastąpiona.</span><span class="sxs-lookup"><span data-stu-id="dacb6-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="dacb6-152">Na przykład, aby zastąpić IDbConnectionFactory i DbProviderService, należy zarejestrować procedurę obsługi w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dacb6-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="dacb6-153">W kodzie powyżej MyProviderServices i MyConnectionFactory reprezentuje Twoje wdrożenia usługi.</span><span class="sxs-lookup"><span data-stu-id="dacb6-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="dacb6-154">Możesz również dodać dodatkowe programy obsługi zależności, aby uzyskać ten sam efekt.</span><span class="sxs-lookup"><span data-stu-id="dacb6-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="dacb6-155">Należy zauważyć, że w ten sposób można również zawijać DbProviderFactory, ale tak samo będzie miało wpływ tylko na EF i nie używa DbProviderFactory poza EF.</span><span class="sxs-lookup"><span data-stu-id="dacb6-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="dacb6-156">Z tego powodu prawdopodobnie zechcesz nadal zawijać DbProviderFactory jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="dacb6-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="dacb6-157">Należy również pamiętać o usługach, które są uruchamiane zewnętrznie do aplikacji — na przykład podczas przeprowadzania migracji z konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="dacb6-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="dacb6-158">Po uruchomieniu migracji z konsoli, podejmie próbę znalezienia dbconfiguration.</span><span class="sxs-lookup"><span data-stu-id="dacb6-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="dacb6-159">Jednak niezależnie od tego, czy usługa zostanie zarejestrowana, zależy od tego, gdzie zarejestrowano procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="dacb6-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="dacb6-160">Jeśli jest ona zarejestrowana w ramach konstruowania dbconfiguration, kod powinien zostać wykonany i usługa powinna zostać opakowana.</span><span class="sxs-lookup"><span data-stu-id="dacb6-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="dacb6-161">Zwykle nie jest to przypadek i oznacza to, że narzędzia nie będą korzystać z opakowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="dacb6-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
