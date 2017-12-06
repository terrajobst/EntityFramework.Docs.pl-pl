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
# <a name="connection-strings"></a>Parametry połączenia

Większość dostawców bazy danych wymaga pewnej formy ciągu połączenia w celu połączenia z bazą danych. Czasami ta parametry połączenia zawierają poufne informacje, które wymagają ochrony. Należy również zmienić parametry połączenia, ponieważ przenoszenie aplikacji między środowiskach, na przykład programowania, testowania i produkcji.

## <a name="net-framework-applications"></a>Aplikacje środowiska .NET framework

Aplikacji .NET framework, takich jak WinForms, WPF konsoli i platformy ASP.NET 4 mają wzorzec ciągu połączenia próby sklonowania i przetestowane. Parametry połączenia należy dodać do pliku App.config aplikacji (Web.config Jeśli używasz programu ASP.NET). Jeśli parametry połączenia zawiera poufne informacje, takie jak nazwa użytkownika i hasło, chronić zawartość przy użyciu pliku konfiguracji [chronione konfiguracji](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Ustawienie nie jest wymagane na parametry połączenia podstawowej EF przechowywane w pliku App.config, ponieważ dostawca bazy danych są skonfigurowane za pomocą kodu.

Następnie można znaleźć przy użyciu ciągu połączenia `ConfigurationManager` interfejsu API w sieci w kontekście `OnConfiguring` metody. Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.

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

## <a name="universal-windows-platform-uwp"></a>Platforma uniwersalna systemu Windows (UWP)

Parametry połączenia w aplikacji platformy uniwersalnej systemu Windows są zwykle połączenie SQLite, które właśnie Określa nazwę pliku lokalnego. One zazwyczaj nie zawierają informacji poufnych, a nie trzeba można zmienić, ponieważ aplikacja jest wdrażana. Tak te parametry połączenia są zwykle małe pozostać w kodzie, jak pokazano poniżej. Jeśli chcesz przenieść je z kodu to platformy uniwersalnej systemu Windows obsługuje pojęcie ustawień, zobacz [ustawień aplikacji części dokumentacji platformy uniwersalnej systemu Windows](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) szczegółowe informacje.

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

## <a name="aspnet-core"></a>Platformy ASP.NET Core

W przypadku platformy ASP.NET Core jest bardzo elastyczny system konfiguracji i ciągu połączenia może być przechowywany w `appsettings.json`, wartość zmiennej środowiskowej, tajne magazynie użytkownika lub inne źródło konfiguracji. Zobacz [sekcji konfiguracji w dokumentacji platformy ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) więcej szczegółów. W poniższym przykładzie przedstawiono parametry połączenia, przechowywane w `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontekst jest zazwyczaj skonfigurowany w `Startup.cs` ciągu połączenia jest odczytywana z konfiguracji. Uwaga `GetConnectionString()` metoda szuka wartości konfiguracji, w których klucz jest `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
