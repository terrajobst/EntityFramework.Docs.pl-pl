---
title: Parametry połączenia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149108"
---
# <a name="connection-strings"></a>Parametry połączeń

Większość dostawców baz danych wymaga pewnego rodzaju parametrów połączenia w celu nawiązania połączenia z bazą danych. Czasami te parametry połączenia zawierają poufne informacje, które muszą być chronione. Może być również konieczna zmiana parametrów połączenia podczas przenoszenia aplikacji między środowiskami, takich jak programowanie, testowanie i środowisko produkcyjne.

## <a name="winforms--wpf-applications"></a>WinForms & aplikacje WPF

Aplikacje WinForms, WPF i ASP.NET 4 mają wypróbowany i przetestowany wzorzec parametrów połączenia. Parametry połączenia należy dodać do pliku App. config aplikacji (Web. config, jeśli używany jest program ASP.NET). Jeśli parametry połączenia zawierają informacje poufne, takie jak nazwa użytkownika i hasło, można chronić zawartość pliku konfiguracji za pomocą [Narzędzia do zarządzania kluczami tajnymi](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> To `providerName` ustawienie nie jest wymagane dla EF Core parametrów połączenia przechowywanych w pliku App. config, ponieważ dostawca bazy danych jest konfigurowany za pośrednictwem kodu.

Następnie można odczytać parametry połączenia przy użyciu `ConfigurationManager` interfejsu API w `OnConfiguring` metodzie kontekstu. Aby można było korzystać z tego interfejsu API, `System.Configuration` może być konieczne dodanie odwołania do zestawu struktury.

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

Parametry połączenia w aplikacji platformy UWP są zazwyczaj połączeniem z systemem SQLite, które tylko określa lokalną nazwę pliku. Zazwyczaj nie zawierają one informacji poufnych i nie trzeba ich zmieniać w miarę wdrażania aplikacji. W związku z tym te parametry połączenia są zwykle bardziej dokładne w kodzie, jak pokazano poniżej. Jeśli chcesz przenieść je poza kod, platformy UWP obsługuje koncepcje ustawień, zobacz [sekcję Ustawienia aplikacji w dokumentacji platformy UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) , aby uzyskać szczegółowe informacje.

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

W ASP.NET Core system konfiguracji jest bardzo elastyczny, a parametry połączenia mogą być przechowywane w `appsettings.json`, zmiennej środowiskowej, magazynie kluczy tajnych użytkownika lub w innym źródle konfiguracji. Aby uzyskać więcej informacji, zobacz [sekcję Konfiguracja w dokumentacji ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) . W poniższym przykładzie przedstawiono parametry połączenia przechowywane w `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontekst jest zazwyczaj skonfigurowany w `Startup.cs` połączeniu z parametrami połączenia, które są odczytywane z konfiguracji. Zwróć uwagę `GetConnectionString()` na to, że metoda poszukuje wartości konfiguracji, `ConnectionStrings:<connection string name>`której kluczem jest. Należy zaimportować przestrzeń nazw [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) , aby użyć tej metody rozszerzenia.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
