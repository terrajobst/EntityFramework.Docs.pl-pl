---
title: Parametry połączenia i modele — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419298"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="d62a1-102">Parametry połączenia i modele</span><span class="sxs-lookup"><span data-stu-id="d62a1-102">Connection strings and models</span></span>
<span data-ttu-id="d62a1-103">W tym temacie opisano, w jaki sposób Entity Framework wykrywają używane połączenie z bazą danych i jak można je zmienić.</span><span class="sxs-lookup"><span data-stu-id="d62a1-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="d62a1-104">Modele utworzone przy użyciu Code First i programu Dr Designer zostały omówione w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="d62a1-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="d62a1-105">Zazwyczaj aplikacja Entity Framework używa klasy pochodnej DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="d62a1-106">Ta klasa pochodna wywoła jeden z konstruktorów w klasie Base DbContext, aby kontrolować:</span><span class="sxs-lookup"><span data-stu-id="d62a1-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="d62a1-107">Jak kontekst będzie łączyć się z bazą danych — to znaczy, jak znaleziono/użyto parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="d62a1-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="d62a1-108">Czy kontekst będzie używać obliczeń dla modelu przy użyciu Code First lub załadowania modelu utworzonego za pomocą narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="d62a1-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="d62a1-109">Dodatkowe opcje zaawansowane</span><span class="sxs-lookup"><span data-stu-id="d62a1-109">Additional advanced options</span></span>  

<span data-ttu-id="d62a1-110">Poniższe fragmenty pokazują, jak można użyć konstruktorów DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="d62a1-111">Używanie Code First z połączeniem według Konwencji</span><span class="sxs-lookup"><span data-stu-id="d62a1-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="d62a1-112">Jeśli nie wykonano żadnej innej konfiguracji w aplikacji, wywołanie konstruktora bez parametrów w DbContext spowoduje uruchomienie DbContext w trybie Code First przy użyciu połączenia bazy danych utworzonego zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="d62a1-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="d62a1-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-113">For example:</span></span>  

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

<span data-ttu-id="d62a1-114">W tym przykładzie DbContext używa kwalifikowanej nazwy klasy kontekstu pochodnego — demonstracyjne. EF. BloggingContext — jako nazwy bazy danych i tworzy parametry połączenia dla tej bazy danych przy użyciu programu SQL Express lub LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d62a1-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="d62a1-115">W przypadku zainstalowania obu tych programów zostanie użyta opcja SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d62a1-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="d62a1-116">Program Visual Studio 2010 zawiera domyślnie program SQL Express, a program Visual Studio 2012 i nowsze zawierają LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d62a1-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="d62a1-117">Podczas instalacji pakiet NuGet EntityFramework sprawdza, który serwer bazy danych jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="d62a1-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="d62a1-118">Pakiet NuGet zaktualizuje plik konfiguracyjny przez ustawienie domyślnego serwera bazy danych, który Code First używa podczas tworzenia połączenia według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="d62a1-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="d62a1-119">Jeśli program SQL Express jest uruchomiony, będzie używany.</span><span class="sxs-lookup"><span data-stu-id="d62a1-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="d62a1-120">Jeśli program SQL Express nie jest dostępny, LocalDB zostanie zarejestrowany jako domyślny.</span><span class="sxs-lookup"><span data-stu-id="d62a1-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="d62a1-121">Nie wprowadzono żadnych zmian w pliku konfiguracji, jeśli zawiera on już ustawienie domyślnej fabryki połączeń.</span><span class="sxs-lookup"><span data-stu-id="d62a1-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="d62a1-122">Użyj Code First z połączeniem według Konwencji i określonej nazwy bazy danych</span><span class="sxs-lookup"><span data-stu-id="d62a1-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="d62a1-123">Jeśli nie wykonano żadnej innej konfiguracji w aplikacji, wywołanie konstruktora String w DbContext z nazwą bazy danych, której chcesz użyć, spowoduje uruchomienie DbContext w trybie Code First przy użyciu połączenia z bazą danych utworzonego w ramach Konwencji do bazy danych programu Ta nazwa.</span><span class="sxs-lookup"><span data-stu-id="d62a1-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="d62a1-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="d62a1-125">W tym przykładzie DbContext wykorzystuje "BloggingDatabase" jako nazwę bazy danych i tworzy parametry połączenia dla tej bazy danych przy użyciu programu SQL Express (zainstalowanego z programem Visual Studio 2010) lub LocalDB (instalowany z programem Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="d62a1-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="d62a1-126">W przypadku zainstalowania obu tych programów zostanie użyta opcja SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d62a1-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d62a1-127">Użyj Code First z parametrami połączenia w pliku App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="d62a1-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d62a1-128">Możesz zdecydować się na umieszczenie parametrów połączenia w pliku App. config lub Web. config.</span><span class="sxs-lookup"><span data-stu-id="d62a1-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="d62a1-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="d62a1-130">Jest to prosty sposób, aby poinformować DbContext o korzystaniu z serwera bazy danych innego niż SQL Express lub LocalDB — powyższy przykład określa bazę danych SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="d62a1-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="d62a1-131">Jeśli nazwa parametrów połączenia jest zgodna z nazwą kontekstu (z kwalifikacją lub bez), zostanie ona znaleziona przez DbContext, gdy używany jest Konstruktor bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="d62a1-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="d62a1-132">Jeśli nazwa parametrów połączenia różni się od nazwy kontekstu, można powiedzieć DbContext, aby użyć tego połączenia w trybie Code First, przekazując nazwę ciągu połączenia do konstruktora DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d62a1-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d62a1-134">Alternatywnie można użyć formularza "name =\<nazwa parametrów połączenia\>" dla ciągu przesłanego do konstruktora DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="d62a1-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d62a1-136">Ten formularz określa, że oczekiwano, że parametry połączenia będą znajdować się w pliku konfiguracyjnym.</span><span class="sxs-lookup"><span data-stu-id="d62a1-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="d62a1-137">Jeśli nie zostanie znaleziony ciąg połączenia o podaną nazwę, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d62a1-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d62a1-138">Baza danych/Model First z parametrami połączenia w pliku App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="d62a1-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d62a1-139">Modele utworzone przy użyciu narzędzia Dr Designer różnią się od Code First w tym, że model już istnieje i nie jest generowany na podstawie kodu podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d62a1-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="d62a1-140">Model zazwyczaj istnieje jako plik EDMX w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d62a1-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="d62a1-141">Projektant doda parametry połączenia EF do pliku App. config lub Web. config.</span><span class="sxs-lookup"><span data-stu-id="d62a1-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="d62a1-142">Te parametry połączenia mają specjalne informacje o sposobach znajdowania informacji w pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="d62a1-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="d62a1-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-143">For example:</span></span>  

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

<span data-ttu-id="d62a1-144">Program Dr Designer generuje również kod, który informuje DbContext o konieczności użycia tego połączenia, przekazując nazwę ciągu połączenia do konstruktora DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d62a1-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d62a1-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="d62a1-146">DbContext wie, aby załadować istniejący model (zamiast używać Code First do obliczenia go z kodu), ponieważ parametry połączenia to parametry połączenia EF zawierające szczegółowe informacje o modelu do użycia.</span><span class="sxs-lookup"><span data-stu-id="d62a1-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="d62a1-147">Inne opcje konstruktora DbContext</span><span class="sxs-lookup"><span data-stu-id="d62a1-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="d62a1-148">Klasa DbContext zawiera inne konstruktory i wzorce użycia, które umożliwiają bardziej zaawansowane scenariusze.</span><span class="sxs-lookup"><span data-stu-id="d62a1-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="d62a1-149">Niektóre z nich są następujące:</span><span class="sxs-lookup"><span data-stu-id="d62a1-149">Some of these are:</span></span>  

- <span data-ttu-id="d62a1-150">Klasy DbModelBuilder można użyć do kompilowania modelu Code First bez wystąpienia wystąpienia DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="d62a1-151">Wynikiem tego jest obiekt dbmodel.</span><span class="sxs-lookup"><span data-stu-id="d62a1-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="d62a1-152">Następnie można przekazać ten obiekt dbmodel do jednego z konstruktorów DbContext, gdy wszystko jest gotowe do utworzenia wystąpienia DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="d62a1-153">Można przekazać pełne parametry połączenia do DbContext zamiast tylko nazwę bazy danych lub parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="d62a1-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="d62a1-154">Domyślnie te parametry połączenia są używane z dostawcą system. Data. SqlClient; można to zmienić, ustawiając inną implementację IConnectionFactory w kontekście. Database. DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="d62a1-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="d62a1-155">Istniejący obiekt DbConnection można użyć przez przekazanie go do konstruktora DbContext.</span><span class="sxs-lookup"><span data-stu-id="d62a1-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="d62a1-156">Jeśli obiekt połączenia jest wystąpieniem elementu EntityConnection, zostanie użyty model określony w połączeniu zamiast obliczania modelu przy użyciu Code First.</span><span class="sxs-lookup"><span data-stu-id="d62a1-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="d62a1-157">Jeśli obiekt jest wystąpieniem innego typu — na przykład sqlconnecting — kontekst będzie używać go dla trybu Code First.</span><span class="sxs-lookup"><span data-stu-id="d62a1-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="d62a1-158">Można przekazać istniejący obiekt ObjectContext do konstruktora DbContext, aby utworzyć DbContext zawija istniejący kontekst.</span><span class="sxs-lookup"><span data-stu-id="d62a1-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="d62a1-159">Ta funkcja może być używana w przypadku istniejących aplikacji, które używają obiektu ObjectContext, ale chcą korzystać z DbContext w niektórych częściach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d62a1-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
