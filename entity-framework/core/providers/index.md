---
title: Dostawcy baz danych — EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 096d68f814e279bd224dfc50111d6576ab5eb223
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149241"
---
# <a name="database-providers"></a>Dostawcy baz danych

Entity Framework Core mogą uzyskać dostęp do wielu różnych baz danych za za poorednictwem bibliotek wtyczek nazywanych dostawcami baz danych.

## <a name="current-providers"></a>Bieżący dostawcy
> [!IMPORTANT]  
> Dostawcy EF Core są kompilowane przy użyciu różnych źródeł. Nie wszyscy dostawcy są obsługiwani jako część [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Rozważając dostawcę, należy oszacować jakość, licencjonowanie, pomoc techniczną itp., aby upewnić się, że spełniają one wymagania. Aby uzyskać szczegółowe informacje o zgodności wersji, należy również zapoznać się z dokumentacją każdego dostawcy.

| Pakiet NuGet                                                                                                        | Obsługiwane aparaty bazy danych | Element utrzymujący/dostawca                                                           | Uwagi/wymagania | Przydatne linki                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 do wewnątrz    | [Projekt EF Core](https://github.com/aspnet/EntityFrameworkCore/) Programu |                      | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3,7         | [Projekt EF Core](https://github.com/aspnet/EntityFrameworkCore/) Programu |                      | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core bazę danych w pamięci | [Projekt EF Core](https://github.com/aspnet/EntityFrameworkCore/) Programu | Tylko do testowania     | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Interfejs API SQL usługi Azure Cosmos DB    | [Projekt EF Core](https://github.com/aspnet/EntityFrameworkCore/) Programu |                      | [docs](xref:core/providers/cosmos/index)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Zespół deweloperów Npgsql](https://github.com/npgsql)                          |                      | [docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              |                      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Serwer MyCAT               | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              | Tylko wersja wstępna      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2,5 i 3. x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2,5 i 3. x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) Database                                |                      | [docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11,2 do wewnątrz     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Wersja wstępna           | [producenta](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja systemu Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja systemu Linux        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | wersja macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Pliki programu Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | OpenEdge postępu          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4  | [DevArt](https://www.devart.com/)                                             | Święcona                 | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8,0 do wewnątrz     | [DevArt](https://www.devart.com/)                                             | Święcona                 | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 lub nowszy           | [DevArt](https://www.devart.com/)                                             | Święcona                 | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | Baza danych MySQL 5            | [DevArt](https://www.devart.com/)                                             | Święcona                 | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="future-providers"></a>Dostawcy w przyszłości

### <a name="cosmos-db"></a>Cosmos DB

Utworzyliśmy dostawcę EF Core dla interfejsu API SQL w programie Cosmos DB.
Będzie to pierwszy kompletny, oparty na dokumentach dostawca bazy danych, który został utworzony, a informacje z tego ćwiczenia będą informować o ulepszeniach w projekcie przyszłych wydań EF Core i innych nierelacyjnych dostawców.
Wersja zapoznawcza jest dostępna w [galerii NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos).

## <a name="adding-a-database-provider-to-your-application"></a>Dodawanie dostawcy bazy danych do aplikacji

Większość dostawców baz danych dla EF Core są dystrybuowane jako pakiety NuGet. Oznacza to, `dotnet` że można je zainstalować za pomocą narzędzia w wierszu polecenia:

``` console
dotnet add package provider_package_name
```

Lub w programie Visual Studio, korzystając z konsoli Menedżera pakietów NuGet:

``` powershell
install-package provider_package_name
```

Po zainstalowaniu dostawca zostanie skonfigurowany w `DbContext` `OnConfiguring` metodzie lub w `AddDbContext` metodzie, jeśli używasz kontenera iniekcji zależności.
Na przykład poniższy wiersz konfiguruje dostawcę SQL Server z przekazywaniem parametrów połączenia:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Dostawcy bazy danych mogą rozciągnąć EF Core, aby umożliwić dostęp do funkcji unikatowych dla konkretnych baz danych.
Niektóre koncepcje są wspólne dla większości baz danych i znajdują się w głównych składnikach EF Core.
Takie koncepcje obejmują ekspresowe zapytania w LINQ, Transactions i śledzenie zmian w obiektach po ich załadowaniu z bazy danych.
Niektóre koncepcje są specyficzne dla określonego dostawcy.
Na przykład dostawca SQL Server umożliwia [skonfigurowanie tabel zoptymalizowanych pod kątem pamięci](xref:core/providers/sql-server/memory-optimized-tables) (funkcji specyficznych dla SQL Server).
Inne koncepcje są specyficzne dla klasy dostawców.
Na przykład EF Core dostawców dla relacyjnych baz danych kompilują się `Microsoft.EntityFrameworkCore.Relational` na wspólną bibliotekę, która udostępnia interfejsy API do konfigurowania mapowań tabel i kolumn, ograniczenia FOREIGN KEY itp. Dostawcy są zwykle dystrybuowane jako pakiety NuGet.

> [!IMPORTANT]  
> Gdy zostanie wydana nowa wersja poprawki EF Core, często zawiera ona aktualizacje `Microsoft.EntityFrameworkCore.Relational` pakietu.
> Po dodaniu dostawcy relacyjnej bazy danych pakiet ten zostanie przechodnią zależnością aplikacji.
> Ale wielu dostawców jest wydawanych niezależnie od EF Core i może nie zostać zaktualizowany w zależności od nowszej wersji poprawki tego pakietu.
> Aby upewnić się, że zostaną wyświetlone wszystkie poprawki błędów, zaleca się dodanie wersji poprawki programu `Microsoft.EntityFrameworkCore.Relational` jako bezpośredniej zależności aplikacji.
