---
title: Parametry połączenia i modeli — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949103"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="35145-102">Parametry połączenia i modeli</span><span class="sxs-lookup"><span data-stu-id="35145-102">Connection strings and models</span></span>
<span data-ttu-id="35145-103">W tym temacie opisano, jak Entity Framework umożliwia odnalezienie połączenie bazy danych i jak można ją zmienić.</span><span class="sxs-lookup"><span data-stu-id="35145-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="35145-104">Modele utworzone przy użyciu Code First i projektancie platformy EF zostały omówione w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="35145-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="35145-105">Zazwyczaj aplikacja programu Entity Framework używa klasy pochodzącej od typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="35145-106">Ta klasa pochodna wywoła jednym z konstruktorów w klasie bazowej DbContext do sterowania:</span><span class="sxs-lookup"><span data-stu-id="35145-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="35145-107">Jak kontekst połączy się z bazą danych — oznacza to, jak ciąg połączenia jest znaleziono użyć</span><span class="sxs-lookup"><span data-stu-id="35145-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="35145-108">Kontekst użyje obliczania modelu za pomocą funkcji Code First czy załadować model utworzony za pomocą projektanta EF</span><span class="sxs-lookup"><span data-stu-id="35145-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="35145-109">Dodatkowe opcje zaawansowane</span><span class="sxs-lookup"><span data-stu-id="35145-109">Additional advanced options</span></span>  

<span data-ttu-id="35145-110">Następujące fragmenty pokazano niektóre sposoby konstruktorów typu DbContext może być używana.</span><span class="sxs-lookup"><span data-stu-id="35145-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="35145-111">Za pomocą Code First połączenia zgodnie z Konwencją</span><span class="sxs-lookup"><span data-stu-id="35145-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="35145-112">W przypadku dowolnej innej konfiguracji nie zostało wykonane w aplikacji, wywołując konstruktora bez parametrów na DbContext spowoduje, że DbContext do pracy w trybie Code First przy użyciu połączenia z bazą danych utworzonych przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="35145-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="35145-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="35145-114">W tym przykładzie DbContext używa w przestrzeni nazw kwalifikowana nazwa Twojej class—Demo.EF.BloggingContext—as pochodnej kontekstu nazwy bazy danych i tworzy ciąg połączenia dla tej bazy danych przy użyciu programu SQL Express lub LocalDB.</span><span class="sxs-lookup"><span data-stu-id="35145-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="35145-115">Jeśli obie są zainstalowane, będzie używany program SQL Express.</span><span class="sxs-lookup"><span data-stu-id="35145-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="35145-116">Visual Studio 2010 zawiera programu SQL Express, domyślnie i programu Visual Studio 2012 i nowszych obejmują LocalDB.</span><span class="sxs-lookup"><span data-stu-id="35145-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="35145-117">Podczas instalacji pakietu EntityFramework NuGet sprawdza, czy serwer bazy danych, która jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="35145-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="35145-118">Pakiet NuGet zostanie następnie zaktualizuj plik konfiguracji ustawienia domyślnego serwera bazy danych, który używa Code First, podczas tworzenia połączenia zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="35145-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="35145-119">Jeśli program SQL Express jest uruchomiona, będzie używany.</span><span class="sxs-lookup"><span data-stu-id="35145-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="35145-120">Jeśli program SQL Express nie jest dostępna następnie LocalDB zostanie zarejestrowana jako domyślny zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="35145-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="35145-121">Nie zmian do pliku konfiguracji, jeśli zawiera on już ustawienie domyślna fabryka połączenia.</span><span class="sxs-lookup"><span data-stu-id="35145-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="35145-122">Za pomocą Code First połączenia, Konwencji i określoną nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="35145-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="35145-123">Jeśli nie wykonano dowolnej innej konfiguracji w aplikacji, następnie wywoływania konstruktora ciągu na DbContext, nazwą bazy danych, którego chcesz użyć spowoduje, że DbContext do pracy w trybie Code First przy użyciu połączenia z bazą danych utworzony zgodnie z Konwencją do bazy danych tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="35145-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="35145-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="35145-125">W tym przykładzie DbContext używa "BloggingDatabase" jako nazwy bazy danych i tworzy ciąg połączenia dla tej bazy danych przy użyciu programu SQL Express (zainstalowany za pomocą programu Visual Studio 2010) lub LocalDB (zainstalowany za pomocą programu Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="35145-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="35145-126">Jeśli obie są zainstalowane, będzie używany program SQL Express.</span><span class="sxs-lookup"><span data-stu-id="35145-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="35145-127">Za pomocą Code First parametrów połączenia w pliku app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="35145-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="35145-128">Można umieścić ciąg połączenia w pliku app.config lub web.config.</span><span class="sxs-lookup"><span data-stu-id="35145-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="35145-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="35145-130">Jest to prosty sposób sprawdzić DbContext do korzystania z serwera bazy danych innych niż SQL Express lub LocalDB — w powyższym przykładzie określa bazę danych programu SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="35145-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="35145-131">Jeśli nazwa ciągu połączenia jest zgodna z nazwą kontekstu (z lub bez kwalifikacji przestrzeni nazw) następnie go będą mogli odnaleźć DbContext stosowania konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="35145-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="35145-132">Nazwa parametrów połączenia jest inna niż Nazwa kontekstu można określić typu DbContext, aby używać tego połączenia w trybie Code First, przekazując nazwę parametrów połączenia do konstruktora typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="35145-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="35145-134">Alternatywnie można użyć formy "nazwa =\<nazwa parametrów połączenia\>" dla ciągu przekazany do konstruktora typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="35145-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="35145-136">Ten formularz, sprawia, że jawne, oczekiwany ciąg połączenia, można znaleźć w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="35145-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="35145-137">Jeśli nie można odnaleźć parametrów połączenia o podanej nazwie, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="35145-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="35145-138">Pierwszy Model/bazy danych przy użyciu parametrów połączenia w pliku app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="35145-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="35145-139">Modele utworzone za pomocą projektanta EF różnią się od Code First w tym modelu już istnieje i nie jest generowany z kodu, gdy aplikacja zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="35145-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="35145-140">Ten model jest zwykle istnieje jako plik EDMX w projekcie.</span><span class="sxs-lookup"><span data-stu-id="35145-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="35145-141">Projektant doda EF parametry połączenia do pliku app.config lub web.config.</span><span class="sxs-lookup"><span data-stu-id="35145-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="35145-142">Parametry połączenia są specjalne, ponieważ zawiera informacje o sposobach znajdowania informacji w pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="35145-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="35145-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="35145-144">W Projektancie platformy EF także wygeneruje kod, który zawiera informacje dla kontekstu DbContext dla tego połączenia, przekazując nazwę parametrów połączenia do konstruktora typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="35145-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35145-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="35145-146">Kontekst DbContext wie, można załadować istniejącego modelu (a nie za pomocą Code First go obliczyć z kodu), ponieważ parametry połączenia są parametry połączenia platformy EF zawierającego szczegółowe informacje o modelu do użycia.</span><span class="sxs-lookup"><span data-stu-id="35145-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="35145-147">Inne opcje konstruktora typu DbContext</span><span class="sxs-lookup"><span data-stu-id="35145-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="35145-148">Klasy DbContext zawiera inne konstruktory i wzorce użycia, które umożliwiają niektórych bardziej zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="35145-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="35145-149">Niektóre z nich to:</span><span class="sxs-lookup"><span data-stu-id="35145-149">Some of these are:</span></span>  

- <span data-ttu-id="35145-150">Klasa DbModelBuilder umożliwia tworzenie model Code First bez tworzenia wystąpienia wystąpienie typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="35145-151">Z tego powodu jest obiektem DbModel.</span><span class="sxs-lookup"><span data-stu-id="35145-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="35145-152">Możesz następnie przekazać ten obiekt DbModel do jednego z konstruktorów typu DbContext, gdy jesteś gotowy do utworzenia wystąpienia typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="35145-153">Pełny ciąg połączenia można przekazać do typu DbContext, zamiast tylko nazwy ciągu bazy danych lub połączenie.</span><span class="sxs-lookup"><span data-stu-id="35145-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="35145-154">Domyślnie ten ciąg połączenia jest używana z dostawcą System.Data.SqlClient; Można to zmienić, ustawiając z inną implementacją IConnectionFactory na kontekście. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="35145-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="35145-155">Można użyć istniejącego obiektu DbConnection, przekazując go do konstruktora typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="35145-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="35145-156">Jeśli obiekt połączenia jest wystąpieniem EntityConnection, określony w połączeniu model nie będą używane zamiast obliczania modelu przy użyciu najpierw kod.</span><span class="sxs-lookup"><span data-stu-id="35145-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="35145-157">Jeśli obiekt jest wystąpieniem innego typu — na przykład element SqlConnection — a następnie kontekst użyje go w trybie Code First.</span><span class="sxs-lookup"><span data-stu-id="35145-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="35145-158">Istniejący obiekt ObjectContext można przekazać do konstruktora typu DbContext do utworzenia typu DbContext zawijania istniejącego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="35145-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="35145-159">Może to służyć do istniejących aplikacji, które używają obiektu ObjectContext, ale którym chcesz korzystać z zalet DbContext w niektórych części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35145-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
