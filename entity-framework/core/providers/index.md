---
title: "Baza danych dostawców - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 0ca5cc0a3184ee602e4b9aea3abadf856163bda4
ms.sourcegitcommit: 5b4efb90222bdbc0da0b3049eb3a0a342197f047
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="database-providers"></a>Dostawcy bazy danych

Program Entity Framework Core mają dostęp do wielu różnych baz danych za pomocą wtyczki bibliotek nazywanych dostawcami bazy danych.

## <a name="current-providers"></a>Bieżący dostawców
> [!IMPORTANT]  
> EF podstawowi dostawcy są tworzone przez różnych źródeł. Nie wszyscy dostawcy są obsługiwane jako część [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore). Rozważając dostawcę, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania. Upewnij się, że Przejrzyj każdy dostawca dokumentacji dla wersji szczegółowe informacje o zgodności.

| Pakiet NuGet                                                                                                        | Obsługiwanych aparatów bazy danych | Element utrzymujący / dostawcy                                                           | Uwagi dotyczące / wymagania             | Przydatne łącza                                                                                                                                                              |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 lub nowszym    | [Projekt Core EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [Dokumentacja](xref:core/providers/sql-server/index)                                                                                                                              |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 lub nowszej         | [Projekt Core EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [Dokumentacja](xref:core/providers/sqlite/index)                                                                                                                                  |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Bazy danych w pamięci Core EF | [Projekt Core EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Tylko do celów testowych                 | [Dokumentacja](xref:core/providers/in-memory/index)                                                                                                                               |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)      | PostgreSQL                 | [Zespół deweloperów Npgsql](https://github.com/npgsql)                          |                                  | [Dokumentacja](http://www.npgsql.org/efcore/index.html)                                                                                                                           |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              |                                  | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                      |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              | Wersja wstępna maksymalnie EF Core 1.1   | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                      |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                            |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                            |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) (Oracle)                                | Wersja wstępna                      | [Dokumentacja](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                       |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 i 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | Podstawowe EF 2.0 lub nowszej, wersji wstępnej | [blog](https://www.tabsoverspaces.com/233653-preview-of-entity-framework-core-2-0-support-for-firebird-and-firebirdclient-6-0/)                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 i 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | EF Core 2.0 lub nowszej              | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                            |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Maksymalnie EF podstawowe 1.1 systemu Windows       | [FAQ](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Instructions_for_downloading_and_using_DB2_NET_Core_provider_package) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Maksymalnie EF podstawowe 1.1, systemu Linux         | [FAQ](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Instructions_for_downloading_and_using_DB2_NET_Core_provider_package) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 i jego nowszych wersjach     | [DevArt](https://www.devart.com/)                                             | Płatnej                             | [Dokumentacja](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                    |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 lub nowszej     | [DevArt](https://www.devart.com/)                                             | Płatnej                             | [Dokumentacja](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3           | [DevArt](https://www.devart.com/)                                             | Płatnej                             | [Dokumentacja](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                    |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5            | [DevArt](https://www.devart.com/)                                             | Płatnej                             | [Dokumentacja](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                     |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Pliki programu Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, .NET Framework      | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                  |

## <a name="future-providers"></a>Przyszłe dostawców

### <a name="cosmos-db"></a>Rozwiązania cosmos bazy danych

Ma firma Microsoft opracowuje EF Core dostawcy interfejsu API usługi DocumentDB w bazie danych rozwiązania Cosmos. Będzie to pierwszy dostawcy pełnej bazy danych, które są ukierunkowane dokumentu, który firma Microsoft tworzą i learnings w tym ćwiczeniu będzie informować ulepszenia projektu w późniejszych wersji po 2.1. Bieżący plan jest publikowanie wczesne Podgląd dostawcy w 2.1 przedziale czasu.

### <a name="oracle"></a>Oracle
Zespół Oracle .NET ma opublikowała planowania zwolnienia dostawcę firmy EF Core 2.0 w trzecim kwartale 2018. Zobacz ich [instrukcji kierunku .NET Core i Core Framework jednostki](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) Aby uzyskać więcej informacji.
Należy kierować bezpośrednio odpowiedzi na pytania dotyczące tego dostawcy do wersji osi czasu, w tym [witryna społeczności Oracle](https://community.oracle.com/).

W tym czasie opracowała zespołu EF [próbki EF Core dostawcy dla baz danych Oracle](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). Celem projektu nie jest do tworzenia dostawcy EF Core należących do firmy Microsoft, ale w celu ułatwienia identyfikacji luki funkcji relacyjnych i podstawowej EF podstawowych, które należy rozwiązać, aby zapewnić lepszą obsługę Oracle i Rozpocznij pracę rozwoju innych Oracle dostawcy podstawowych EF Oracle lub stron trzecich.

Firma Microsoft będzie uwzględniać wkładów zwiększających przykładowe zastosowanie. Firma Microsoft może również Witamy i zachęca społeczności próbuje utworzyć dostawca programu Oracle open source podstawowych EF, przy użyciu przykładu jako punktu wyjścia.

## <a name="adding-a-database-provider-to-your-application"></a>Dodawanie dostawcy bazy danych do swojej aplikacji

Większość dostawców bazy danych podstawowych EF są dystrybuowane jako pakietów NuGet. Oznacza to, mogą być instalowane za pomocą `dotnet` narzędzia w wierszu polecenia:

``` console
dotnet add package provider_package_name
```

Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów NuGet:

``` powershell
install-package provider_package_name
```

Po zakończeniu instalacji będzie skonfigurowania dostawcy w Twojej `DbContext`, albo w `OnConfiguring` metody lub w `AddDbContext` metodę, jeśli używasz kontener iniekcji zależności. Np. następujący wiersz Konfigurowanie dostawcy programu SQL Server z parametrami połączenia przekazany:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Dostawcy bazy danych można rozszerzyć EF Core, aby włączyć funkcję unikatowe dla baz danych. Niektóre pojęcia są wspólne dla baz danych i znajdują się w głównej EF podstawowych składników. Takie pojęcia zawierać wyrażenia zapytania składnika LINQ, transakcje i śledzenie zmian obiektów, gdy są załadowane z bazy danych. Niektóre pojęcia są specyficzne dla określonego dostawcy. Na przykład dostawcy programu SQL Server umożliwia [skonfigurować tabele zoptymalizowane pod kątem pamięci](xref:core/providers/sql-server/memory-optimized-tables) (funkcja specyficzna dla programu SQL Server). Inne pojęcia są specyficzne dla klasy dostawców. Na przykład tworzenie dostawców EF Core relacyjnych baz danych na typowe `Microsoft.EntityFrameworkCore.Relational` biblioteki, która udostępnia interfejsy API do skonfigurowania mapowania kolumn i tabel ograniczeń klucza obcego, itp. Dostawcy są zazwyczaj dystrybuowane jako pakietów NuGet.

> [!IMPORTANT]  
> Po wydaniu nowej wersji poprawki EF Core często obejmuje aktualizacje `Microsoft.EntityFrameworkCore.Relational` pakietu. Podczas dodawania dostawcy relacyjnej bazy danych, ten pakiet staje się przechodnie zależności aplikacji. Jednak wielu dostawców są wydawane niezależnie od EF Core i nie mogą być aktualizowane są zależne od nowszej wersji tego pakietu poprawek. Aby upewnić się, otrzymasz wszystkie poprawki błędów, zaleca się dodanie wersja poprawki `Microsoft.EntityFrameworkCore.Relational` jako bezpośrednie zależności aplikacji.
