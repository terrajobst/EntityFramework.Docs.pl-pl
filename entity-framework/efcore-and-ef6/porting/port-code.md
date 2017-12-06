---
title: Eksportowanie z EF6 do EF Core - eksportowanie Model oparty na kod
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Eksportowanie EF6 opartej na kodzie modelu EF Core

Jeśli wszystkie ostrzeżenia zostały przeczytane i wszystko jest gotowe do portu, poniżej przedstawiono wskazówki, które pomogą Ci rozpocząć.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów EF Core NuGet

Aby używać EF Core, należy zainstalować pakiet NuGet dostawcy bazy danych, którego chcesz użyć. Na przykład gdy program SQL Server, czy zainstalować `Microsoft.EntityFrameworkCore.SqlServer`. Zobacz [dostawcy bazy danych](../../core/providers/index.md) szczegółowe informacje.

Jeśli planujesz użyć migracji, należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu.

Bardzo można pozostawić pakiet EF6 NuGet (EntityFramework), zainstalowane, EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jednak jeśli nie są zamierza użyć EF6 w obszary aplikacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.

## <a name="swap-namespaces"></a>Zamień przestrzenie nazw

Większość interfejsów API, które są używane w EF6 znajdują się w `System.Data.Entity` przestrzeni nazw (i związane z przestrzeni nazw sub). Pierwsza zmiana kodu jest można zamienić na `Microsoft.EntityFrameworkCore` przestrzeni nazw. Czy zazwyczaj rozpoczyna się z plikiem pochodnej kontekstu kodu i następnie wyglądają stamtąd adresowania błędy kompilacji w miarę ich występowania.

## <a name="context-configuration-connection-etc"></a>Kontekst konfiguracji (połączenie itp.)

Zgodnie z opisem w [upewnij się, EF rdzenie będą pracy Twoja aplikacja](ensure-requirements.md), EF Core ma mniej magic wokół wykrywanie bazy danych, aby nawiązać połączenie. Należy zastąpić `OnConfiguring` metodę w pochodnej kontekstu i nawiązanie połączenia z bazą danych, użyj interfejsu API określonego dostawcy bazy danych.

Większość aplikacji EF6 przechowywanie parametrów połączenia w aplikacjach `App/Web.config` pliku. W podstawowej EF przeczytanie tego parametry połączenia za pomocą `ConfigurationManager` interfejsu API. Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.

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

## <a name="update-your-code"></a>Zaktualizuj kod

W tym momencie jest kwestią adresowania błędy kompilacji i przeglądania kodu w celu zmiany zachowania będzie miało wpływ możesz się w temacie.

## <a name="existing-migrations"></a>Migracja istniejących

Naprawdę jest możliwe sposobem portu istniejących migracje EF6 EF Core.

Jeśli to możliwe najlepiej założono, że wszystkie poprzednich migracji z EF6 zostały zastosowane do bazy danych i następnie rozpoczęcia migracji schematu od tego punktu, przy użyciu EF Core. Aby to zrobić, należy użyć `Add-Migration` polecenie, aby dodać migracji, gdy model jest przenoszone do EF Core. Następnie spowoduje usunięcie całego kodu ze `Up` i `Down` metody szkieletu migracji. Kolejne migracje zostanie porównany do modelu po tej migracji początkowe Tworzenie szkieletu wykonano.

## <a name="test-the-port"></a>Testowanie portu

Tak, ponieważ aplikacja kompiluje, oznacza to, że pomyślnie jest przenoszone na EF rdzeń. Należy przetestować wszystkie obszary w aplikacji w celu zapewnienia, że zmiany zachowania ma negatywny wpływ na aplikację.
