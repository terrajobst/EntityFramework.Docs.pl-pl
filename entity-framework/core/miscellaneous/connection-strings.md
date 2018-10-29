---
title: Parametry połączenia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191345"
---
# <a name="connection-strings"></a>Parametry połączenia

Większość dostawców bazy danych wymaga pewnego rodzaju parametry połączenia do łączenia z bazą danych. Czasami te parametry połączenia zawiera poufne informacje, które muszą być chronione. Również może być konieczna zmiana parametrów połączenia, ponieważ przenoszenie aplikacji między środowiskami, takie jak programowania, testowania i produkcji.

## <a name="net-framework-applications"></a>Aplikacji .NET framework

Aplikacji .NET framework, takich jak WinForms, WPF, konsola i platformy ASP.NET 4 ma połączenie i przetestowanej wzorzec ciągu. Parametry połączenia należy dodać kod do pliku App.config aplikacji (Web.config, jeśli używasz programu ASP.NET). Jeśli parametry połączenia zawiera poufne informacje, takie jak nazwa użytkownika i hasło, chronić zawartość przy użyciu pliku konfiguracji [konfiguracji chronionej](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Ustawienie nie jest wymagane na parametry połączenia programu EF Core przechowywane w pliku App.config, ponieważ dostawca bazy danych jest skonfigurowana za pomocą kodu.

Możesz odczytywać parametry połączenia za pomocą `ConfigurationManager` interfejsu API w sieci w kontekście `OnConfiguring` metody. Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.

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

Parametry połączenia w aplikacji platformy uniwersalnej systemu Windows są zazwyczaj połączenia bazy danych SQLite, po prostu określający nazwę pliku lokalnego. Są zazwyczaj nie zawierają informacji poufnych, a nie powinny być można zmienić, ponieważ aplikacja jest wdrażana. W efekcie te parametry połączenia są zwykle wystarczające pozostać w kodzie, jak pokazano poniżej. Jeśli chcesz przenieść je poza kod to platformy uniwersalnej systemu Windows obsługuje pojęcie ustawień, zobacz [ustawienia aplikacji w sekcji dokumentacji platformy uniwersalnej systemu Windows](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Aby uzyskać szczegółowe informacje.

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

## <a name="aspnet-core"></a>ASP.NET Core

W programie ASP.NET Core jest bardzo elastyczny system konfiguracji i parametry połączenia mogą być przechowywane w `appsettings.json`, zmienną środowiskową, magazynu wpisów tajnych użytkownika lub innego źródła konfiguracji. Zobacz [konfiguracji w sekcji dokumentacji platformy ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) Aby uzyskać więcej informacji. W poniższym przykładzie przedstawiono parametry połączenia przechowywana w `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontekst jest zazwyczaj skonfigurowany w `Startup.cs` przy użyciu parametrów połączenia jest odczytywana z konfiguracji. Uwaga `GetConnectionString()` metoda szuka wartości konfiguracji, w których kluczem jest `ConnectionStrings:<connection string name>`. Należy zaimportować [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) przestrzeni nazw w celu używania tej metody rozszerzenia.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
