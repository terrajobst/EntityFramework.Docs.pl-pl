---
title: Dostawcy struktury podmiotów — EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419364"
---
# <a name="entity-framework-6-providers"></a>Dostawcy struktury podmiotów 6
> [!NOTE]
> **Ef6 Tylko —** funkcje, interfejsy API, itp omówione na tej stronie zostały wprowadzone w entity framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie mają zastosowania.

Entity Framework jest obecnie opracowywany w ramach licencji open-source i EF6 i powyżej nie zostaną wysłane w ramach programu .NET Framework. Ma to wiele zalet, ale wymaga również, aby dostawcy EF zostać przebudowany względem zestawów EF6. Oznacza to, że ef dostawców EF5 i poniżej nie będzie działać z EF6, dopóki nie zostały przebudowane.

## <a name="which-providers-are-available-for-ef6"></a>Którzy dostawcy są dostępni dla EF6?

Dostawcy, których jesteśmy świadomi, że zostały przebudowane dla EF6 obejmują:

*   **Dostawca programu Microsoft SQL Server**
    *   Zbudowany z [bazy kodu open source Entity Framework](https://github.com/aspnet/EntityFramework6)
    *   Dostarczany jako część [pakietu EntityFramework NuGet](https://nuget.org/packages/EntityFramework)
*   **Dostawca programu Microsoft SQL Server Compact Edition**
    *   Zbudowany z [bazy kodu open source Entity Framework](https://github.com/aspnet/EntityFramework6)
    *   Dostarczane w [pakiecie EntityFramework.SqlServerCompact NuGet](https://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Dostawcy danych Devart dotConnect**](https://www.devart.com/dotconnect/)
    *   Istnieją dostawcy zewnętrzni z [Devart](https://www.devart.com/) dla różnych baz danych, w tym Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 i SQL Server
*   [**Dostawcy oprogramowania CData**](https://www.cdata.com/ado/)
    *   Istnieją dostawcy zewnętrzni z [CData Software](https://www.cdata.com/ado/) dla różnych magazynów danych, w tym Salesforce, Azure Table Storage, MySql i wielu innych
*   **Dostawca firebird**
    *   Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)
*   **Dostawca visual fox pro**
    *   Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [Łącznik MySQL/NET dla entity framework](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)
*   **Oracle**
    *   ODP.NET jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Należy zauważyć, że włączenie do tej listy nie wskazuje poziomu funkcjonalności lub obsługi dla danego dostawcy, tylko, że kompilacja dla EF6 została udostępniona.

## <a name="registering-ef-providers"></a>Rejestrowanie dostawców EF

Począwszy od entity framework 6 EF dostawców mogą być rejestrowane przy użyciu konfiguracji opartej na kodzie lub w pliku konfiguracyjnym aplikacji.

### <a name="config-file-registration"></a>Rejestracja pliku konfiguracyjnego

Rejestracja dostawcy EF w app.config lub web.config ma następujący format:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Należy zauważyć, że często, jeśli dostawca EF jest zainstalowany z NuGet, a następnie pakiet NuGet automatycznie dodać tę rejestrację do pliku konfiguracyjnego. Jeśli zainstalujesz pakiet NuGet w projekcie, który nie jest projektem startowym dla aplikacji, może być konieczne skopiowanie rejestracji do pliku konfiguracyjnego dla projektu uruchamiania.

"InvariantName" w tej rejestracji jest taka sama niezmienna nazwa używana do identyfikowania dostawcy ADO.NET. Można to znaleźć jako atrybut "niezmienny" w DbProviderFactories rejestracji i jako "providerName" atrybut w rejestracji ciągu połączenia. Niezmienna nazwa do użycia powinna być również uwzględniona w dokumentacji dla dostawcy. Przykładami niezmiennych nazw są "System.Data.SqlClient" dla programu SQL Server i "System.Data.SqlServerCe.4.0" dla programu SQL Server Compact.

"Typ" w tej rejestracji jest nazwą kwalifikowaną do zestawu typu dostawcy, która pochodzi od "System.Data.Entity.Core.Common.DbProviderServices". Na przykład ciąg używany dla SQL Compact to "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact". Typ do użycia w tym miejscu powinny być zawarte w dokumentacji dla dostawcy.

### <a name="code-based-registration"></a>Rejestracja oparta na kodzie

Począwszy od entity framework 6 konfiguracji całej aplikacji dla EF można określić w kodzie. Aby uzyskać szczegółowe informacje, zobacz _[Konfiguracja oparta na kodzie entity framework](https://msdn.microsoft.com/data/jj680699)_. Normalnym sposobem zarejestrowania dostawcy ef przy użyciu konfiguracji opartej na kodzie jest utworzenie nowej klasy, która wywodzi się z System.Data.Entity.DbConfiguration i umieścić go w tym samym zestawie co klasa DbContext. Klasa DbConfiguration powinna następnie zarejestrować dostawcę w jego konstruktorze. Na przykład, aby zarejestrować dostawcę sql compact klasa DbConfiguration wygląda następująco:

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

W tym kodzie "SqlCeProviderServices.ProviderInvariantName" jest wygoda dla dostawcy SQL Server Compact niezmienny ciąg nazwy ("System.Data.SqlServerCe.4.0") i SqlCeProviderServices.Instance zwraca pojedyncze wystąpienie dostawcy SQL Compact EF.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co zrobić, jeśli potrzebny mi dostawca nie jest dostępny?

Jeśli dostawca jest dostępny dla poprzednich wersji EF, firma Microsoft zachęcamy do skontaktowania się z właścicielem dostawcy i poprosić o utworzenie wersji EF6. Należy dołączyć odwołanie do [dokumentacji dla modelu dostawcy EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Czy mogę napisać dostawcę samodzielnie?

Z pewnością możliwe jest samodzielne utworzenie dostawcy ef, chociaż nie należy go uważać za trywialne przedsięwzięcie. Powyższe łącze dotyczące modelu dostawcy EF6 jest dobrym miejscem do rozpoczęcia. Może się również okazać przydatne użycie kodu dla dostawcy SQL Server i SQL CE zawarte w [bazie kodu open source EF](https://github.com/aspnet/EntityFramework6) jako punkt wyjścia lub w celach informacyjnych.

Należy zauważyć, że począwszy od EF6 dostawcy EF jest mniej ściśle powiązane z podstawowym dostawcą ADO.NET. Ułatwia to pisanie dostawcy EF bez konieczności pisania lub zawijania klas ADO.NET.
