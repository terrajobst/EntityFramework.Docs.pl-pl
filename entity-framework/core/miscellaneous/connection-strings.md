---
title: Parametry połączenia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416590"
---
# <a name="connection-strings"></a><span data-ttu-id="1ffec-102">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="1ffec-102">Connection Strings</span></span>

<span data-ttu-id="1ffec-103">Większość dostawców baz danych wymaga pewnego rodzaju parametrów połączenia w celu nawiązania połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1ffec-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="1ffec-104">Czasami te parametry połączenia zawierają poufne informacje, które muszą być chronione.</span><span class="sxs-lookup"><span data-stu-id="1ffec-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="1ffec-105">Może być również konieczna zmiana parametrów połączenia podczas przenoszenia aplikacji między środowiskami, takich jak programowanie, testowanie i środowisko produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="1ffec-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="1ffec-106">WinForms & aplikacje WPF</span><span class="sxs-lookup"><span data-stu-id="1ffec-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="1ffec-107">Aplikacje WinForms, WPF i ASP.NET 4 mają wypróbowany i przetestowany wzorzec parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="1ffec-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="1ffec-108">Parametry połączenia należy dodać do pliku App. config aplikacji (Web. config, jeśli używany jest program ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="1ffec-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="1ffec-109">Jeśli parametry połączenia zawierają informacje poufne, takie jak nazwa użytkownika i hasło, można chronić zawartość pliku konfiguracji za pomocą [Narzędzia do zarządzania kluczami tajnymi](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="1ffec-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="1ffec-110">Ustawienie `providerName` nie jest wymagane dla EF Core parametrów połączenia przechowywanych w pliku App. config, ponieważ dostawca bazy danych jest skonfigurowany za pośrednictwem kodu.</span><span class="sxs-lookup"><span data-stu-id="1ffec-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="1ffec-111">Następnie można odczytać parametry połączenia przy użyciu interfejsu API `ConfigurationManager` w metodzie `OnConfiguring` Twojego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1ffec-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="1ffec-112">Może być konieczne dodanie odwołania do zestawu programu `System.Configuration` Framework, aby można było używać tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1ffec-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="1ffec-113">Platforma uniwersalna systemu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="1ffec-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="1ffec-114">Parametry połączenia w aplikacji platformy UWP są zazwyczaj połączeniem z systemem SQLite, które tylko określa lokalną nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="1ffec-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="1ffec-115">Zazwyczaj nie zawierają one informacji poufnych i nie trzeba ich zmieniać w miarę wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ffec-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="1ffec-116">W związku z tym te parametry połączenia są zwykle bardziej dokładne w kodzie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="1ffec-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="1ffec-117">Jeśli chcesz przenieść je poza kod, platformy UWP obsługuje koncepcje ustawień, zobacz [sekcję Ustawienia aplikacji w dokumentacji platformy UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) , aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="1ffec-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="1ffec-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ffec-118">ASP.NET Core</span></span>

<span data-ttu-id="1ffec-119">W ASP.NET Core system konfiguracji jest bardzo elastyczny, a parametry połączenia mogą być przechowywane w `appsettings.json`, zmiennej środowiskowej, magazynie kluczy tajnych użytkownika lub w innym źródle konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ffec-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="1ffec-120">Aby uzyskać więcej informacji, zobacz [sekcję Konfiguracja w dokumentacji ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) .</span><span class="sxs-lookup"><span data-stu-id="1ffec-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="1ffec-121">W poniższym przykładzie przedstawiono parametry połączenia przechowywane w `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="1ffec-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="1ffec-122">Kontekst jest zazwyczaj konfigurowany w `Startup.cs` z parametrami połączenia, które są odczytywane z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1ffec-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="1ffec-123">Zwróć uwagę, że metoda `GetConnectionString()` szuka wartości konfiguracji, której klucz jest `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="1ffec-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="1ffec-124">Należy zaimportować przestrzeń nazw [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) , aby użyć tej metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1ffec-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
