---
title: Konfiguracja oparta na kodu - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325356"
---
# <a name="code-based-configuration"></a><span data-ttu-id="b7512-102">Konfiguracja na podstawie kodu</span><span class="sxs-lookup"><span data-stu-id="b7512-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="b7512-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b7512-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b7512-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="b7512-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b7512-105">Konfigurację dla aplikacji platformy Entity Framework można określić w pliku konfiguracji (app.config/web.config) lub za pomocą kodu.</span><span class="sxs-lookup"><span data-stu-id="b7512-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="b7512-106">Który jest znany jako Konfiguracja na podstawie kodu.</span><span class="sxs-lookup"><span data-stu-id="b7512-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="b7512-107">Konfiguracja w pliku konfiguracji jest opisana w [oddzielny artykuł](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="b7512-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="b7512-108">Plik konfiguracji, który ma pierwszeństwo przed konfiguracja na podstawie kodu.</span><span class="sxs-lookup"><span data-stu-id="b7512-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="b7512-109">Innymi słowy Jeśli opcja konfiguracji jest ustawiona, zarówno kod i w pliku konfiguracji, następnie ustawienie w pliku konfiguracji jest używany.</span><span class="sxs-lookup"><span data-stu-id="b7512-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="b7512-110">Za pomocą DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b7512-110">Using DbConfiguration</span></span>  

<span data-ttu-id="b7512-111">Konfiguracja oparta na kod w EF6 i nowszych można uzyskać, tworząc podklasa System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b7512-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="b7512-112">Poniższe wskazówki dotyczą sytuacji, gdy Tworzenie podklasy DbConfiguration:</span><span class="sxs-lookup"><span data-stu-id="b7512-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="b7512-113">Utwórz tylko jedną klasę DbConfiguration dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7512-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="b7512-114">Ta klasa określa ustawienia dla całego domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7512-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="b7512-115">Umieść klasy DbConfiguration z tego samego zestawu jako swojej klasy DbContext.</span><span class="sxs-lookup"><span data-stu-id="b7512-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b7512-116">(Zobacz *przenoszenie DbConfiguration* sekcji, jeśli chcesz to zmienić.)</span><span class="sxs-lookup"><span data-stu-id="b7512-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="b7512-117">Nadaj swojej klasy DbConfiguration publicznego konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b7512-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="b7512-118">Ustawianie opcji konfiguracji, wywołując DbConfiguration metody chronionej z w ramach tego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b7512-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="b7512-119">Następujące wskazówki umożliwia EF wykrywanie i używanie konfigurację automatycznie, zarówno narzędzi, który ma dostęp do modelu i uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7512-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="b7512-120">Przykład</span><span class="sxs-lookup"><span data-stu-id="b7512-120">Example</span></span>  

<span data-ttu-id="b7512-121">Klasy pochodzącej od DbConfiguration może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b7512-121">A class derived from DbConfiguration might look like this:</span></span>  

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

<span data-ttu-id="b7512-122">Ta klasa konfiguruje EF, aby użyć strategii wykonywania SQL Azure — do automatycznego ponawiania próby wykonania operacji bazy danych nie powiodło się — i Użyj lokalnej bazy danych dla baz danych, które są tworzone zgodnie z Konwencją z Code First.</span><span class="sxs-lookup"><span data-stu-id="b7512-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="b7512-123">Przenoszenie DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b7512-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="b7512-124">Istnieją przypadki, w którym nie jest możliwe do umieszczenia na klasę DbConfiguration z tego samego zestawu jako swojej klasy DbContext.</span><span class="sxs-lookup"><span data-stu-id="b7512-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b7512-125">Na przykład masz dwie klasy DbContext, każdy w różnych zestawach.</span><span class="sxs-lookup"><span data-stu-id="b7512-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="b7512-126">Dostępne są dwie opcje do obsługi tego.</span><span class="sxs-lookup"><span data-stu-id="b7512-126">There are two options for handling this.</span></span>  

<span data-ttu-id="b7512-127">Pierwszym z nich jest określenie wystąpienia DbConfiguration do użycia przy użyciu pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b7512-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="b7512-128">Aby to zrobić, należy ustawić atrybut codeConfigurationType sekcji platformy entityFramework.</span><span class="sxs-lookup"><span data-stu-id="b7512-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="b7512-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b7512-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="b7512-130">Wartość codeConfigurationType musi być zestawu i przestrzeni nazw kwalifikowana nazwa klasy DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b7512-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="b7512-131">Drugą opcją jest umieścić DbConfigurationTypeAttribute na klasie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="b7512-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="b7512-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b7512-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="b7512-133">Wartość przekazywana do atrybutu mogą być typu DbConfiguration — jak pokazano powyżej — lub zestawu i przestrzeni nazw kwalifikowana ciąg nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="b7512-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="b7512-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b7512-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="b7512-135">Jawne ustawianie DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b7512-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="b7512-136">Istnieją sytuacje, w którym konfiguracji może być wymagane przed dowolnego typu DbContext został już użyty.</span><span class="sxs-lookup"><span data-stu-id="b7512-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="b7512-137">Przykłady to między innymi:</span><span class="sxs-lookup"><span data-stu-id="b7512-137">Examples of this include:</span></span>  

- <span data-ttu-id="b7512-138">Za pomocą DbModelBuilder w celu zbudowania modelu bez kontekstu</span><span class="sxs-lookup"><span data-stu-id="b7512-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="b7512-139">Przy użyciu innych framework/narzędzie kodu korzystającej z typu DbContext, gdzie tego kontekstu jest używana, zanim kontekst aplikacji jest używana</span><span class="sxs-lookup"><span data-stu-id="b7512-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="b7512-140">W takich sytuacjach EF jest nie można automatycznie wykryć konfiguracji i zamiast tego należy wykonać jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b7512-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="b7512-141">Ustaw typ DbConfiguration w pliku konfiguracji, zgodnie z opisem w *przenoszenie DbConfiguration* powyższej sekcji</span><span class="sxs-lookup"><span data-stu-id="b7512-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="b7512-142">Wywołanie metody statycznej DbConfiguration.SetConfiguration podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="b7512-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="b7512-143">Zastępowanie DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b7512-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="b7512-144">Istnieją sytuacje, w których trzeba zastąpić konfigurację w DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b7512-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="b7512-145">Nie jest to zazwyczaj wykonywane przez deweloperów aplikacji, ale raczej przez dostawców innych firm i wtyczek, które nie mogą używać klasy pochodnej DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b7512-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="b7512-146">W tym celu EntityFramework umożliwia program obsługi zdarzeń do zarejestrowania, które można zmodyfikować istniejącą konfigurację, po prostu, zanim zostanie zablokowane w dół.</span><span class="sxs-lookup"><span data-stu-id="b7512-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="b7512-147">Zapewnia także metody sugar specjalnie w celu zastąpienia dowolnej usługi zwrócony przez EF wspólnym lokalizatorze usług.</span><span class="sxs-lookup"><span data-stu-id="b7512-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="b7512-148">Jest to, jak jest przeznaczony do użycia:</span><span class="sxs-lookup"><span data-stu-id="b7512-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="b7512-149">Przy uruchamianiu aplikacji (przed użyciem EF) wtyczki lub dostawcy należy zarejestrować metody obsługi zdarzeń dla tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="b7512-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="b7512-150">(Zwróć uwagę, że to musi się zdarzyć, zanim aplikacja używa EF).</span><span class="sxs-lookup"><span data-stu-id="b7512-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="b7512-151">Program obsługi zdarzeń wywołuje ReplaceService dla każdej usługi, który ma zostać zastąpione.</span><span class="sxs-lookup"><span data-stu-id="b7512-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="b7512-152">Na przykład repalce IDbConnectionFactory i DbProviderService należy zarejestrować program obsługi podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="b7512-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="b7512-153">W kodzie powyżej MyProviderServices i MyConnectionFactory reprezentują usługi implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="b7512-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="b7512-154">Można również dodać dodatkową zależność programów obsługi, aby uzyskać ten sam efekt.</span><span class="sxs-lookup"><span data-stu-id="b7512-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="b7512-155">Należy pamiętać, że DbProviderFactory można również opakować w ten sposób, ale spowoduje to więc mają wpływ tylko na EF i nie używa DbProviderFactory poza EF.</span><span class="sxs-lookup"><span data-stu-id="b7512-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="b7512-156">Z tego powodu należy prawdopodobnie opakować DbProviderFactory, jak mają przed w dalszym ciągu.</span><span class="sxs-lookup"><span data-stu-id="b7512-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="b7512-157">Należy również mieć na uwadze usług, które uruchamiasz zewnętrznie do Twojej aplikacji — na przykład podczas uruchamiania migracji z konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="b7512-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="b7512-158">Po uruchomieniu migracji z konsoli, spróbuje ona znaleźć Twoje DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b7512-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="b7512-159">Niezależnie od tego czy pobierze opakowana usługi zależy jednak gdzie on zarejestrowany program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="b7512-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="b7512-160">Jeśli jest zarejestrowany w ramach konstrukcji swoje DbConfiguration kod powinien zostać wykonany, a powinien pobrać opakowane usługi.</span><span class="sxs-lookup"><span data-stu-id="b7512-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="b7512-161">Zazwyczaj nie będzie to mieć miejsce, i oznacza to, że narzędzia nie będą otrzymywać opakowana usługi.</span><span class="sxs-lookup"><span data-stu-id="b7512-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
