---
title: "Parametry połączenia - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a><span data-ttu-id="f81f6-102">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="f81f6-102">Connection Strings</span></span>

<span data-ttu-id="f81f6-103">Większość dostawców bazy danych wymaga pewnej formy ciągu połączenia w celu połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f81f6-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="f81f6-104">Czasami ta parametry połączenia zawierają poufne informacje, które wymagają ochrony.</span><span class="sxs-lookup"><span data-stu-id="f81f6-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="f81f6-105">Należy również zmienić parametry połączenia, ponieważ przenoszenie aplikacji między środowiskach, na przykład programowania, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="f81f6-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="f81f6-106">Aplikacje środowiska .NET framework</span><span class="sxs-lookup"><span data-stu-id="f81f6-106">.NET Framework Applications</span></span>

<span data-ttu-id="f81f6-107">Aplikacji .NET framework, takich jak WinForms, WPF konsoli i platformy ASP.NET 4 mają wzorzec ciągu połączenia próby sklonowania i przetestowane.</span><span class="sxs-lookup"><span data-stu-id="f81f6-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="f81f6-108">Parametry połączenia należy dodać do pliku App.config aplikacji (Web.config Jeśli używasz programu ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="f81f6-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="f81f6-109">Jeśli parametry połączenia zawiera poufne informacje, takie jak nazwa użytkownika i hasło, chronić zawartość przy użyciu pliku konfiguracji [chronione konfiguracji](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="f81f6-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="f81f6-110">`providerName` Ustawienie nie jest wymagane na parametry połączenia podstawowej EF przechowywane w pliku App.config, ponieważ dostawca bazy danych są skonfigurowane za pomocą kodu.</span><span class="sxs-lookup"><span data-stu-id="f81f6-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="f81f6-111">Następnie można znaleźć przy użyciu ciągu połączenia `ConfigurationManager` interfejsu API w sieci w kontekście `OnConfiguring` metody.</span><span class="sxs-lookup"><span data-stu-id="f81f6-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="f81f6-112">Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f81f6-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="f81f6-113">Platforma uniwersalna systemu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="f81f6-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="f81f6-114">Parametry połączenia w aplikacji platformy uniwersalnej systemu Windows są zwykle połączenie SQLite, które właśnie Określa nazwę pliku lokalnego.</span><span class="sxs-lookup"><span data-stu-id="f81f6-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="f81f6-115">One zazwyczaj nie zawierają informacji poufnych, a nie trzeba można zmienić, ponieważ aplikacja jest wdrażana.</span><span class="sxs-lookup"><span data-stu-id="f81f6-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="f81f6-116">Tak te parametry połączenia są zwykle małe pozostać w kodzie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f81f6-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="f81f6-117">Jeśli chcesz przenieść je z kodu to platformy uniwersalnej systemu Windows obsługuje pojęcie ustawień, zobacz [ustawień aplikacji części dokumentacji platformy uniwersalnej systemu Windows](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="f81f6-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="f81f6-118">Platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f81f6-118">ASP.NET Core</span></span>

<span data-ttu-id="f81f6-119">W przypadku platformy ASP.NET Core jest bardzo elastyczny system konfiguracji i ciągu połączenia może być przechowywany w `appsettings.json`, wartość zmiennej środowiskowej, tajne magazynie użytkownika lub inne źródło konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f81f6-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="f81f6-120">Zobacz [sekcji konfiguracji w dokumentacji platformy ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f81f6-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="f81f6-121">W poniższym przykładzie przedstawiono parametry połączenia, przechowywane w `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="f81f6-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="f81f6-122">Kontekst jest zazwyczaj skonfigurowany w `Startup.cs` ciągu połączenia jest odczytywana z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f81f6-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="f81f6-123">Uwaga `GetConnectionString()` metoda szuka wartości konfiguracji, w których klucz jest `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="f81f6-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
