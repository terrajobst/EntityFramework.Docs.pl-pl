---
title: Przenoszenie z EF6 do EF Core — przenoszenie modelu opartego na kodzie — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419638"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Przenoszenie modelu opartego na kodzie EF6 do ef core

Jeśli przeczytałeś wszystkie zastrzeżenia i jesteś gotowy do portu, oto kilka wskazówek, które pomogą Ci rozpocząć pracę.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów EF Core NuGet

Aby użyć EF Core, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć. Na przykład podczas kierowania na program `Microsoft.EntityFrameworkCore.SqlServer`SQL Server należy zainstalować program . Zobacz [dostawców baz danych, aby](../../core/providers/index.md) uzyskać szczegółowe informacje.

Jeśli planujesz używać migracji, należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakiet.

Jest w porządku, aby pozostawić ef6 NuGet pakiet (EntityFramework) zainstalowany, jak EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jednak jeśli nie zamierzasz używać EF6 w żadnych obszarach aplikacji, a następnie odinstalowanie pakietu pomoże dać błędy kompilacji na kawałki kodu, które wymagają uwagi.

## <a name="swap-namespaces"></a>Zamienianie obszarów nazw

Większość interfejsów API używanych w u `System.Data.Entity` ef6 znajdują się w obszarze nazw (i powiązanych pod-obszarów nazw). Pierwszą zmianą kodu jest `Microsoft.EntityFrameworkCore` zamiana na obszar nazw. Zazwyczaj należy rozpocząć od pliku kodu kontekstu pochodnego, a następnie wypracować stamtąd, adresowanie błędów kompilacji, jak występują.

## <a name="context-configuration-connection-etc"></a>Konfiguracja kontekstu (połączenie itp.)

Zgodnie z opisem w [Ensure EF Core will work for Your Application](ensure-requirements.md), EF Core ma mniej magii wokół wykrywania bazy danych, aby połączyć się z. Należy zastąpić `OnConfiguring` metodę w kontekście pochodnym i użyć interfejsu API specyficznego dla dostawcy bazy danych do skonfigurowania połączenia z bazą danych.

Większość aplikacji EF6 przechowuje parametry `App/Web.config` połączenia w pliku aplikacji. W EF Core odczytasz ten `ConfigurationManager` ciąg połączenia przy użyciu interfejsu API. Może być konieczne dodanie odwołania `System.Configuration` do zestawu framework, aby móc korzystać z tego interfejsu API.

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

## <a name="update-your-code"></a>Aktualizowanie kodu

W tym momencie jest to kwestia adresowania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania będą miały wpływ na Ciebie.

## <a name="existing-migrations"></a>Istniejące migracje

Nie ma naprawdę wykonalny sposób na przeniesienie istniejących migracji EF6 do EF Core.

Jeśli to możliwe, najlepiej jest założyć, że wszystkie poprzednie migracje z EF6 zostały zastosowane do bazy danych, a następnie rozpocząć migrację schematu z tego punktu przy użyciu EF Core. Aby to zrobić, należy `Add-Migration` użyć polecenia, aby dodać migracji, gdy model jest przeniesiony do EF Core. Następnie należy usunąć cały `Up` kod `Down` z i metody migracji szkieletu. Kolejne migracje zostaną porównane z modelem, gdy ta początkowa migracja została szkieletowa.

## <a name="test-the-port"></a>Przetestuj port

Tylko dlatego, że aplikacja kompiluje, nie oznacza, że jest pomyślnie przeniesiony do EF Core. Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna ze zmian zachowania nie wpłynęła negatywnie na aplikację.
