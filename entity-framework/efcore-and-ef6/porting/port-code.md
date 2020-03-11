---
title: Przenoszenie z EF6 do EF Core-Porting model-EF oparty na kodzie
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419638"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Przenoszenie modelu opartego na kodzie EF6 do EF Core

Jeśli przeczytasz wszystkie zastrzeżenia i wszystko jest gotowe do portu, Oto kilka wytycznych, które ułatwią Ci rozpoczęcie pracy.

## <a name="install-ef-core-nuget-packages"></a>Zainstaluj EF Core pakiety NuGet

Aby użyć EF Core, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć. Na przykład podczas określania wartości docelowej SQL Server należy zainstalować `Microsoft.EntityFrameworkCore.SqlServer`. Aby uzyskać szczegółowe informacje, zobacz [dostawcy bazy danych](../../core/providers/index.md) .

Jeśli planujesz używać migracji, należy również zainstalować pakiet `Microsoft.EntityFrameworkCore.Tools`.

Warto pozostawić zainstalowany pakiet NuGet EF6 (EntityFramework), ponieważ EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jeśli jednak nie zamierzasz korzystać z EF6 w żadnych obszarach aplikacji, dezinstalacja pakietu pomoże Ci w wydaniu błędów kompilacji dla fragmentów kodu, które wymagają uwagi.

## <a name="swap-namespaces"></a>Zamień przestrzenie nazw

Większość interfejsów API, które są używane w EF6, znajduje się w przestrzeni nazw `System.Data.Entity` (i powiązanych podrzędnych przestrzeni nazw). Pierwsza zmiana kodu polega na zamianie na przestrzeń nazw `Microsoft.EntityFrameworkCore`. Zwykle zaczynasz od pochodnego pliku kodu kontekstu, a następnie z tego powodu wykorzystasz błędy kompilacji w miarę ich występowania.

## <a name="context-configuration-connection-etc"></a>Konfiguracja kontekstu (połączenie itp.)

Zgodnie z opisem w temacie upewnij się, że [EF Core będzie działała dla aplikacji](ensure-requirements.md), EF Core ma mniej Magic wokół wykrywania bazy danych w celu nawiązania połączenia. Należy przesłonić metodę `OnConfiguring` w kontekście pochodnym i skonfigurować połączenie z bazą danych przy użyciu interfejsu API specyficznego dla dostawcy bazy danych.

Większość aplikacji EF6 przechowuje parametry połączenia w pliku `App/Web.config` aplikacji. W EF Core należy odczytać te parametry połączenia za pomocą interfejsu API `ConfigurationManager`. Może być konieczne dodanie odwołania do zestawu programu `System.Configuration` Framework, aby można było używać tego interfejsu API.

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

W tym momencie jest kwestią rozwiązywania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania wpłyną na Ciebie.

## <a name="existing-migrations"></a>Istniejące migracje

Nie istnieje naprawdę możliwy do przenoszenia istniejących EF6 migracji do EF Core.

Jeśli to możliwe, najlepiej założyć, że wszystkie poprzednie migracje z EF6 zostały zastosowane do bazy danych, a następnie rozpocznij migrację schematu z tego punktu przy użyciu EF Core. W tym celu należy użyć `Add-Migration` polecenie, aby dodać migrację po przeprowadzeniu modelu do EF Core. Następnie można usunąć cały kod z `Up` i `Down` metod migracji szkieletowej. Kolejne migracje zostaną porównane z modelem, gdy początkowa migracja została poddana migracji.

## <a name="test-the-port"></a>Testowanie portu

Tak samo, jak Twoja aplikacja kompiluje, nie oznacza, że została pomyślnie przeniesiono do EF Core. Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna zmiana zachowania nie miała negatywnego wpływu na aplikację.
