---
title: Dostawcy baz danych — EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: c9db4921f3f1dc7647c0a877e1bbf8fcef1bc0cd
ms.sourcegitcommit: 9c881e334c57c902795f32858877d7a8f18c8cb3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58872789"
---
# <a name="database-providers"></a>Dostawcy baz danych

Entity Framework Core mogą uzyskiwać dostęp w wielu różnych baz danych za pomocą wtyczki bibliotek nazywanych dostawcami bazy danych.

## <a name="current-providers"></a>Bieżącego dostawcy
> [!IMPORTANT]  
> EF Core dostawcy są tworzone za pomocą różnych źródeł. Nie wszyscy dostawcy są przechowywane jako część [projektu programu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Podczas wybierania dostawcy, pamiętaj ocenić jakość, licencjonowanie, pomocy technicznej, itp., aby upewnić się, że spełniają one wymagania. Ponadto upewnij się, że przejrzenie dokumentacji poszczególnych dostawców wersji szczegółowych informacji dotyczących zgodności.

| Pakiet NuGet                                                                                                        | Obsługiwanych aparatów bazy danych | Element utrzymujący / dostawcy                                                           | Informacje o / wymagań | Przydatne linki                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 lub nowszy    | [Projekt programu EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Dokumentacja](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | Bazy danych SQLite 3.7 lub nowszy         | [Projekt programu EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Dokumentacja](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Baza danych programu EF Core w pamięci | [Projekt programu EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Tylko do celów testowych     | [Dokumentacja](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Interfejs API SQL usługi Azure Cosmos DB    | [Projekt programu EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Tylko wersja zapoznawcza         | [blog](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Zespół programistyczny Npgsql](https://github.com/npgsql)                          |                      | [Dokumentacja](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              |                      | [Plik Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              | Tylko wersję wstępną      | [Plik Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [witryny typu wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [witryny typu wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 i 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [Dokumentacja](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 i 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [witryny typu wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) (Oracle)                                |                      | [Dokumentacja](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle                     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Wersja wstępna           | [Witryny sieci Web](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersji systemu Linux        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | wersja systemu macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Pliki programu Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Plik Readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | OpenEdge postępu          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Plik Readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 lub nowszy     | [DevArt](https://www.devart.com/)                                             | Płatne                 | [Dokumentacja](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 lub nowszy     | [DevArt](https://www.devart.com/)                                             | Płatne                 | [Dokumentacja](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | 3 bazy danych SQLite           | [DevArt](https://www.devart.com/)                                             | Płatne                 | [Dokumentacja](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 lub nowszy            | [DevArt](https://www.devart.com/)                                             | Płatne                 | [Dokumentacja](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="future-providers"></a>Przyszłe dostawców

### <a name="cosmos-db"></a>Cosmos DB

Mają firma Microsoft opracowuje dostawcę programu EF Core dla interfejsu API SQL w usłudze Cosmos DB.
Jest to pierwszy dostawca pełnej korzystający z dokumentów bazy danych, który możemy zostały utworzone, a informacje w tym ćwiczeniu będzie informować ulepszenia w projekcie przyszłych wersjach programu EF Core i prawdopodobnie innych dostawców nierelacyjnych.
Podgląd jest dostępny w [galerii pakietów NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos).

### <a name="oracle-first-party-provider"></a>Dostawca programu Oracle firmy Microsoft
Zespół Oracle .NET został opublikowany wersją beta [dostawca programu Oracle dla platformy EF Core](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/).
Pytania dotyczące tego dostawcy, w tym do osi czasu w wersji należy kierować [witryny społeczności Oracle](https://community.oracle.com/).

## <a name="adding-a-database-provider-to-your-application"></a>Dodawanie dostawcy bazy danych do aplikacji

Większość dostawców bazy danych dla platformy EF Core są dystrybuowane jako pakiety NuGet. Oznacza to, mogą być instalowane za pomocą `dotnet` narzędzia w wierszu polecenia:

``` console
dotnet add package provider_package_name
```

Lub w programie Visual Studio przy użyciu konsoli Menedżera pakietów NuGet:

``` powershell
install-package provider_package_name
```

Po zainstalowaniu skonfiguruje dostawcy w Twojej `DbContext`, albo w `OnConfiguring` metody lub `AddDbContext` metody korzystania z kontenera iniekcji zależności.
Na przykład następujący wiersz konfiguruje dostawcę programu SQL Server przy użyciu parametrów połączenia przekazany:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Dostawcy baz danych można rozszerzyć programu EF Core, aby włączyć funkcje unikatowe dla konkretnych baz danych.
Niektóre pojęcia są wspólne dla większości baz danych i są objęte podstawowe składniki programu EF Core.
Takie pojęcia zawierają wyrażenia zapytania w LINQ, transakcji i śledzenie zmian obiektów, gdy są one załadowane z bazy danych.
Niektóre pojęcia są specyficzne dla określonego dostawcy.
Na przykład dostawca programu SQL Server umożliwia [skonfigurować tabele zoptymalizowane pod kątem pamięci](xref:core/providers/sql-server/memory-optimized-tables) (funkcja specyficzna dla programu SQL Server).
Inne pojęcia są specyficzne dla klasy dostawców.
Na przykład utworzyć dostawcy programu EF Core dla relacyjnych baz danych na wspólnej `Microsoft.EntityFrameworkCore.Relational` biblioteki, która udostępnia interfejsy API do skonfigurowania mapowania tabel i kolumn, ograniczeń klucza obcego, itp. Dostawcy są zazwyczaj dystrybuowane jako pakiety NuGet.

> [!IMPORTANT]  
> Po wydaniu nowej wersji poprawki programu EF Core często obejmuje aktualizacje `Microsoft.EntityFrameworkCore.Relational` pakietu.
> Po dodaniu dostawcy relacyjnej bazy danych, ten pakiet uzależnienie przechodnie aplikacji.
> Jednak wielu dostawców są wydawane niezależnie od programu EF Core i nie mogą być aktualizowane są zależne od nowszej wersji tego pakietu poprawek.
> Aby upewnić się, otrzymasz wszystkie poprawki błędów, zaleca się dodawania wersję poprawki `Microsoft.EntityFrameworkCore.Relational` jako bezpośrednie zależności aplikacji.
