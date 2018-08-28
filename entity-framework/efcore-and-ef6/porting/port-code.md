---
title: Przenoszenie z programu EF6 do programu EF Core — przenoszenie modelu opartego na kodzie
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997052"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Przenoszenie modelu opartego na kodzie EF6 do programu EF Core

Jeśli wszystkie ostrzeżenia zostały przeczytane, możesz przystąpić do portu, poniżej przedstawiono wskazówki, które pomogą Ci rozpocząć pracę.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów programu EF Core NuGet

Aby korzystać z programu EF Core, zainstaluj pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć. Na przykład, gdy obiektem docelowym programu SQL Server, można zainstalować `Microsoft.EntityFrameworkCore.SqlServer`. Zobacz [dostawcy baz danych](../../core/providers/index.md) Aby uzyskać szczegółowe informacje.

Jeśli planujesz użyć migracje, a następnie należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu.

Jest dobrym rozwiązaniem pozostawić pakiet NuGet platformy EF6 (EntityFramework), zainstalowane, zgodnie z programem EF Core i EF6 mogą być używane side-by-side w tej samej aplikacji. Jednak w przypadku korzystania z platformy EF6 w żadnych obszarów aplikacji nie są pomyślny przebieg operacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.

## <a name="swap-namespaces"></a>Przestrzenie nazw wymiany

Większość interfejsów API, których używasz w EF6 znajdują się w `System.Data.Entity` przestrzeni nazw (i pokrewnych przestrzeniach nazw sub). Pierwszy zmiana kodu jest można zamienić na `Microsoft.EntityFrameworkCore` przestrzeni nazw. Będzie zazwyczaj rozpocząć pochodnej kontekstu pliku kodu, a następnie pozwolimy na opracowanie stamtąd adresowania błędy kompilacji w miarę ich występowania.

## <a name="context-configuration-connection-etc"></a>Konfiguracja kontekstu (połączenie itp.)

Zgodnie z opisem w [upewnij się, EF Core będzie pracy dla aplikacji](ensure-requirements.md), programem EF Core ma mniej magic wokół wykrywanie bazy danych, aby nawiązać połączenie. Należy zastąpić `OnConfiguring` metodę w pochodnej kontekstu i użyj interfejsu API określonego dostawcy bazy danych, aby skonfigurować połączenie z bazą danych.

Większość aplikacji EF6 przechowywanie parametrów połączenia w aplikacjach `App/Web.config` pliku. W programie EF Core przeczytanie tego parametry połączenia za pomocą `ConfigurationManager` interfejsu API. Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.

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

W tym momencie jest kwestią adresowania błędy kompilacji i przeglądania kodu, aby zobaczyć zmiany zachowania będzie miało wpływ użytkownik.

## <a name="existing-migrations"></a>Migracja istniejących

Tak naprawdę nie jest to możliwe sposobu portu istniejących migracje platformy EF6 do programu EF Core.

Jeśli to możliwe najlepiej Załóżmy, że zostały zastosowane wszystkie poprzednich migracji z programu EF6 do bazy danych, a następnie rozpoczęcia migracji schematu od tego punktu, przy użyciu programu EF Core. Aby to zrobić, należy użyć `Add-Migration` polecenie do dodania do migracji, gdy model jest przenoszone do programu EF Core. Następnie należy usunąć cały kod z `Up` i `Down` metody szkieletu migracji. Porównuje następnej migracji do modelu podczas tej początkowej migracji został szkielet.

## <a name="test-the-port"></a>Testowanie portu

Po prostu, ponieważ kompiluje aplikację, nie znaczy, że pomyślnie są przenoszone do programu EF Core. Należy przetestować wszystkie obszary w aplikacji, aby upewnić się, że żadne zmiany zachowania niekorzystnie wpłynąć na nie wpływ aplikacji.
