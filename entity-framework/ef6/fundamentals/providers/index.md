---
title: Dostawcy w programie Entity Framework - EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: 98dc34591e8b2724401810a13f363382dce53218
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997921"
---
# <a name="entity-framework-6-providers"></a>Dostawcy programu Entity Framework 6
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Entity Framework jest obecnie opracowywany w ramach licencji open source i EF6 i powyżej nie zostanie dostarczona w ramach programu .NET Framework. Ma wiele zalet, ale wymaga również odbudowany EF dostawców dla zestawów platformy EF6. Oznacza to, że EF dostawców dla EF5, jak i poniżej nie będą działać przy użyciu platformy EF6, dopóki ich ponownej kompilacji.

## <a name="which-providers-are-available-for-ef6"></a>Które są dostępni dostawcy dla platformy EF6?

Sobie sprawę z dostawcami, ponownej kompilacji dla platformy EF6 obejmują:

*   **Dostawca programu Microsoft SQL Server**
    *   Utworzona na podstawie [Entity Framework Otwórz bazy kodu źródłowego](http://github.com/aspnet/EntityFramework6)
    *   Dostarczany jako część [pakiet NuGet platformy EntityFramework](http://nuget.org/packages/EntityFramework)
*   **Dostawca programu Microsoft SQL Server Compact Edition**
    *   Utworzona na podstawie [Entity Framework Otwórz bazy kodu źródłowego](http://github.com/aspnet/EntityFramework6)
    *   Dostarczana z [pakietu EntityFramework.SqlServerCompact NuGet](http://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Devart dotConnect dostawców danych**](http://www.devart.com/dotconnect/)
    *   Brak dostawców innych firm [Devart](http://www.devart.com/) dla wielu baz danych w tym Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 i programu SQL Server
*   [**Dostawców oprogramowania CData**](http://www.cdata.com/ado/)
    *   Brak dostawców innych firm [oprogramowania CData](http://www.cdata.com/ado/) z różnych magazynów danych, w tym Salesforce, usługa Azure Table Storage, MySql i wielu innych
*   **Dostawca firebird**
    *   Dostępne jako [pakietu NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)
*   **Visual Fox Pro dostawcy**
    *   Dostępne jako [pakietu NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)
*   **PostgreSQL**
    *   Npgsql jest dostępna jako [pakietu NuGet](http://www.nuget.org/packages/Npgsql.EF6/)
*   **Oracle**
    *   ODP.NET jest dostępna jako [pakietu NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Należy pamiętać, włączenia na tej liście nie wskazuje poziom funkcjonalności lub pomocy technicznej dla danego dostawcy, tylko że kompilacji dla platformy EF6 została udostępniona.

## <a name="registering-ef-providers"></a>Rejestrowanie dostawców EF

Począwszy od dostawcy programu Entity Framework 6 EF można zarejestrować za pomocą konfiguracji albo oparte na kodzie lub w pliku konfiguracji aplikacji.

### <a name="config-file-registration"></a>Rejestracja pliku konfiguracji

Rejestracja dostawcy EF w pliku app.config lub web.config ma następujący format:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Należy pamiętać, że często Jeśli dostawca programu EF jest instalowany z pakietów NuGet, a następnie pakietu NuGet spowoduje automatyczne dodanie tej rejestracji do pliku konfiguracji. Po zainstalowaniu pakietu NuGet do projektu, który nie jest projektem startowym aplikacji może należy skopiować rejestracji do pliku konfiguracji dla Twój projekt startowy.

"Invatiantname" w tym rejestracji jest taka sama nazwa niezmienna, używany do identyfikowania dostawcy ADO.NET. Ten znajduje się jako atrybut "niezmiennej" w rejestracji DbProviderFactories, a atrybut "providerName" podczas rejestracji ciąg połączenia. Niezmienna Nazwa do użycia powinny też obejmować w dokumentacji dostawcy. Przykłady nazw niezmiennej są "System.Data.SqlClient" dla programu SQL Server i "System.Data.SqlServerCe.4.0" dla programu SQL Server Compact.

"Type" w tym rejestracji jest nazwa kwalifikowanego dla zestawu typu dostawcy, która dziedziczy po "System.Data.Entity.Core.Common.DbProviderServices". Na przykład ciąg do użycia dla programu SQL Compact jest "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact". Typ do użycia w tym miejscu powinny być objęte dokumentacji dostawcy.

### <a name="code-based-registration"></a>Oparte na kodzie rejestracji

Począwszy od platformy Entity Framework 6 konfiguracji całej aplikacji na platformie EF można określić w kodzie. Szczegółowe informacje można znaleźć  _[konfiguracji Entity Framework Code-Based](https://msdn.microsoft.com/en-us/data/jj680699)_. Normalny sposób, aby zarejestrować dostawcę EF przy użyciu konfiguracji na podstawie kodu jest Utwórz nową klasę dziedziczącą po System.Data.Entity.DbConfiguration i umieść go w tym samym zestawie jako swojej klasy DbContext. Klasa DbConfiguration należy następnie zarejestruj dostawcę w jego konstruktorze. Na przykład aby zarejestrować SQL Compact dostawcy klasy DbConfiguration wygląda następująco:

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

W tym kodzie "SqlCeProviderServices.ProviderInvariantName" to wygodne dla programu SQL Server Compact ciągu niezmienna Nazwa dostawcy ("System.Data.SqlServerCe.4.0") i SqlCeProviderServices.Instance zwraca pojedyncze wystąpienie SQL Compact Dostawca EF.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co się stanie, jeśli dostawca potrzebuję nie jest dostępna?

Jeśli dostawca jest dostępny we wcześniejszych wersjach programu EF, następnie zachęcamy Cię do skontaktuj się z właścicielem dostawcy i poproś go o utworzenia wersji EF6. Należy dołączyć odwołanie do [dokumentacji dla modelu dostawcy EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Można napisać dostawcę samodzielnie?

Oczywiście prawdopodobnie utworzyć dostawcę EF samodzielnie, mimo że nie należy rozważyć trivial przedsiębiorstwa. Powyższe łącze o modelu dostawcy EF6 jest dobrym miejscem do rozpoczęcia. Możesz również przydatne może okazać się do użycia kodu dla programu SQL Server i SQL CE dostawcy uwzględnione w [codebase "open source" EF](https://github.com/aspnet/EntityFramework6) jako punktu wyjścia lub odwołania.

Należy pamiętać, że począwszy od platformy EF6 dostawcy EF jest mniej ściśle powiązany źródłowy dostawca ADO.NET. Ta funkcja ułatwia pisanie dostawcy programu EF bez konieczności pisania lub opakowywania klas ADO.NET.
