---
title: Dostawcy baz danych — EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: daf2e06c76ed55213243f5728548fdfd4be0e5e2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417826"
---
# <a name="database-providers"></a>Dostawcy baz danych

Entity Framework Core można uzyskać dostęp do wielu różnych baz danych za pośrednictwem bibliotek wtyczek o nazwie dostawców bazy danych.

## <a name="current-providers"></a>Obecni dostawcy

> [!IMPORTANT]  
> Ef Core dostawców są tworzone przez różnych źródeł. Nie wszyscy dostawcy są obsługiwana w ramach [projektu podstawowego entity framework](https://github.com/aspnet/EntityFrameworkCore). Rozważając dostawcę, należy ocenić jakość, licencjonowanie, wsparcie itp., aby upewnić się, że spełniają one Twoje wymagania. Upewnij się również, że przeglądasz dokumentację każdego dostawcy, aby uzyskać szczegółowe informacje o zgodności wersji.

> [!IMPORTANT]  
> Ef Core dostawców zazwyczaj działają w wersjach pomocniczych, ale nie w głównych wersjach. Na przykład dostawca wydany dla EF Core 2.1 powinien współpracować z EF Core 2.2, ale nie będzie działać z EF Core 3.0. 

| Pakiet NuGet                                                                                                        | Obsługiwane aparaty baz danych | Opiekun / Dostawca                                                           | Uwagi / Wymagania | Stworzony z myślą o wersji | Przydatne łącza                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Serwer Microsoft.EntityFrameCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | Sql Server 2012 — więcej    | [Ef Core Project](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Plik Microsoft.EntityFrameCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 i dalej         | [Ef Core Project](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameWorkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Baza danych EF Core w pamięci | [Ef Core Project](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | [Ograniczenia](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [Dokumenty](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameWorkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Azure Cosmos DB — interfejs SQL API    | [Ef Core Project](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Strona Npgsql.EntityFrameWorkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Zespół programistów Npgsql](https://github.com/npgsql)                          |                      | 3.1               | [Dokumenty](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFramecore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projekt Fundacji Pomelo](https://github.com/PomeloFoundation)              |                      | 3.1               | [Plik readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 i dalej            | [DevArt (DevArt)](https://www.devart.com/)                                             | Wypłacane                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 i dalej  | [DevArt (DevArt)](https://www.devart.com/)                                             | Wypłacane                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 i później     | [DevArt (DevArt)](https://www.devart.com/)                                             | Wypłacane                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 i dalej           | [DevArt (DevArt)](https://www.devart.com/)                                             | Wypłacane                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Wynik Kontekstu Pliku](https://www.nuget.org/packages/FileContextCore/)                                                   | Przechowuje dane w plikach       | [Morris Janatzek](https://github.com/morrisjdev)                              | Do celów rozwojowych | 3.0               | [Plik readme](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Pliki programu Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | 2.2               | [Plik readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 i 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | 2.2               | [Dokumenty](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [Teradata.EntityFramecore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                         | Baza danych Teradata 16.10 | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) | | 2.2               |[Stronie internetowej](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 i 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | 2.1               | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [Element EntityFrameWorkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Aleksandra Wiese](https://github.com/alexwiese)                                    |                      | 2.1               | [Plik readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [MySql.Data.EntityFrameCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](https://dev.mysql.com) (Oracle)                               |                      | 2.1               | [Dokumenty](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 — począwszy od     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   |                      | 2.1               | [Stronie internetowej](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [Ibm. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja systemu Windows      | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Ibm. EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja linuksa        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Ibm. EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Wersja z systemem macOS        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Serwer MyCAT               | [Projekt Fundacji Pomelo](https://github.com/PomeloFoundation)              | Tylko wydanie wstępne      | 1.1               | [Plik readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Dodawanie dostawcy bazy danych do aplikacji

Większość dostawców baz danych dla EF Core są dystrybuowane jako pakiety NuGet i mogą być instalowane w następujący sposób:

## <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Po zainstalowaniu, można skonfigurować `DbContext`dostawcę w `OnConfiguring` , w `AddDbContext` metodzie lub w metodzie, jeśli używasz kontenera iniekcji zależności.
Na przykład następujący wiersz konfiguruje dostawcę programu SQL Server z przekazanym ciągiem połączenia:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Dostawcy baz danych można rozszerzyć EF Core, aby włączyć funkcje unikatowe dla określonych baz danych.
Niektóre pojęcia są wspólne dla większości baz danych i są zawarte w podstawowych składników EF Core.
Takie pojęcia obejmują wyrażanie zapytań w LINQ, transakcji i śledzenia zmian do obiektów po ich załadowaniu z bazy danych.
Niektóre pojęcia są specyficzne dla określonego dostawcy.
Na przykład dostawca programu SQL Server umożliwia [skonfigurowanie tabel zoptymalizowanych pod kątem pamięci](xref:core/providers/sql-server/memory-optimized-tables) (funkcji specyficznej dla programu SQL Server).
Inne pojęcia są specyficzne dla klasy dostawców.
Na przykład ef core dostawców dla relacyjnych `Microsoft.EntityFrameworkCore.Relational` baz danych kompilacji na wspólnej biblioteki, która udostępnia interfejsy API do konfigurowania mapowania tabel i kolumn, ograniczenia klucza obcego, itp. Dostawcy są zwykle dystrybuowane jako pakiety NuGet.

> [!IMPORTANT]  
> Po wydaniu nowej wersji poprawki EF Core często `Microsoft.EntityFrameworkCore.Relational` zawiera aktualizacje pakietu.
> Po dodaniu dostawcy relacyjnej bazy danych ten pakiet staje się przechodnie zależności aplikacji.
> Ale wielu dostawców są wydawane niezależnie od EF Core i nie mogą być aktualizowane w zależności od nowszej wersji poprawki tego pakietu.
> Aby upewnić się, że otrzymasz wszystkie poprawki błędów, zaleca `Microsoft.EntityFrameworkCore.Relational` się dodanie wersji poprawki jako bezpośredniej zależności aplikacji.
