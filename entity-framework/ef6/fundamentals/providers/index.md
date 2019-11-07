---
title: Dostawcy Entity Framework — EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656200"
---
# <a name="entity-framework-6-providers"></a>Entity Framework 6 dostawców
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Entity Framework jest teraz opracowywany w ramach licencji Open Source i EF6, a powyższe nie będą wysyłane jako część .NET Framework. Ma wiele korzyści, ale wymaga również, aby dostawcy EF mogli odbudować zestawy EF6. Oznacza to, że dostawcy EF dla EF5 i poniżej nie będą współpracować z EF6, dopóki nie zostaną ponownie skompilowane.

## <a name="which-providers-are-available-for-ef6"></a>Którzy dostawcy są dostępni dla EF6?

Dostawcy wie, że zostały skompilowane dla EF6:

*   **Dostawca Microsoft SQL Server**
    *   Kompilacja oparta na [Entity Framework bazie kodu open source](https://github.com/aspnet/EntityFramework6)
    *   Dostarczane jako część [pakietu NuGet EntityFramework](https://nuget.org/packages/EntityFramework)
*   **Dostawca wersji Microsoft SQL Server Compact**
    *   Kompilacja oparta na [Entity Framework bazie kodu open source](https://github.com/aspnet/EntityFramework6)
    *   Dostarczone w [pakiecie NuGet EntityFramework. SqlServerCompact](https://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Dostawcy danych Devart dotConnect**](https://www.devart.com/dotconnect/)
    *   Istnieją dostawcy innych firm od [Devart](https://www.devart.com/) dla różnych baz danych, takich jak Oracle, MySQL, PostgreSQL, SQLite, SALESFORCE, DB2 i SQL Server
*   [**CData dostawcy oprogramowania**](https://www.cdata.com/ado/)
    *   Istnieją dostawcy innych firm z [CDATA oprogramowania](https://www.cdata.com/ado/) dla różnych magazynów danych, w tym usług Salesforce, Azure Table Storage, MySQL i wielu innych
*   **Dostawca Firebird**
    *   Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)
*   **Dostawca programu Visual Fox Pro**
    *   Dostępne jako [pakiet NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [Łącznik MySQL/NET dla Entity Framework](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)
*   **Database**
    *   ODP.NET jest dostępny jako [pakiet NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Należy pamiętać, że włączenie na tej liście nie wskazuje poziomu funkcjonalności ani wsparcia dla danego dostawcy, tylko wtedy, gdy udostępniono kompilację dla EF6.

## <a name="registering-ef-providers"></a>Rejestrowanie dostawców EF

Począwszy od Entity Frameworkych dostawców EF można zarejestrować przy użyciu konfiguracji opartej na kodzie lub w pliku konfiguracyjnym aplikacji.

### <a name="config-file-registration"></a>Rejestracja pliku konfiguracji

Rejestracja dostawcy EF w pliku App. config lub Web. config ma następujący format:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Należy pamiętać, że często Jeśli dostawca EF jest instalowany z narzędzia NuGet, pakiet NuGet automatycznie doda tę rejestrację do pliku konfiguracji. W przypadku zainstalowania pakietu NuGet w projekcie, który nie jest projektem startowym dla aplikacji, może być konieczne skopiowanie rejestracji do pliku konfiguracji dla projektu startowego.

"Invariantname" w tej rejestracji jest tą samą niezmienną nazwą, która jest używana do identyfikowania dostawcy ADO.NET. Można go znaleźć jako atrybut "Invariant" w rejestracji DbProviderFactories i jako atrybut "ProviderName" w rejestracji parametrów połączenia. Niezmienna nazwa do użycia powinna również być uwzględniona w dokumentacji dostawcy. Przykłady nazw niezmiennych to "System. Data. SqlClient" dla SQL Server i "System. Data. SqlServerCe. 4.0" dla SQL Server Compact.

"Typ" w tej rejestracji to kwalifikowana dla zestawu nazwa typu dostawcy, który pochodzi od "System. Data. Entity. Core. Common. DbProviderServices". Na przykład ciąg używany na potrzeby języka SQL Compact to "System. Data. Entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact". Typ, który ma być używany w tym miejscu, powinien zostać uwzględniony w dokumentacji dostawcy.

### <a name="code-based-registration"></a>Rejestracja oparta na kodzie

Począwszy od Entity Framework 6 Konfiguracja całej aplikacji dla EF można określić w kodzie. Aby uzyskać szczegółowe informacje, zobacz _[Entity Framework konfiguracji opartej na kodzie](https://msdn.microsoft.com/data/jj680699)_ . Normalny sposób zarejestrowania dostawcy EF przy użyciu konfiguracji opartej na kodzie polega na utworzeniu nowej klasy, która pochodzi od klasy System. Data. Entity. dbconfiguration i umieścić ją w tym samym zestawie co Klasa DbContext. Klasa dbconfiguration powinna następnie zarejestrować dostawcę w konstruktorze. Na przykład, aby zarejestrować dostawcę programu SQL Compact, Klasa dbconfiguration wygląda następująco:

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

W tym kodzie "SqlCeProviderServices. ProviderInvariantName" jest wygodnym ciągiem nazw niezmiennej dostawcy SQL Server Compact ("System. Data. SqlServerCe. 4.0") i SqlCeProviderServices. Instance zwraca pojedyncze wystąpienie programu SQL Compact Dostawca EF.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co zrobić, jeśli potrzebny dostawca nie jest dostępny?

Jeśli dostawca jest dostępny dla poprzednich wersji EF, zachęcamy do skontaktowania się z właścicielem dostawcy i poproszenia o utworzenie wersji EF6. Należy dołączyć odwołanie do [dokumentacji modelu dostawcy Ef6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Czy mogę napisać dostawcę samodzielnie?

Z pewnością można utworzyć dostawcę EF, chociaż nie należy go traktować jako prostego przedsiębiorstwa. Powyższy link dotyczący modelu dostawcy EF6 jest dobrym miejscem do uruchomienia. Przydatne może być również użycie kodu dla dostawcy SQL Server i programu SQL CE zawartego w bazie [kodu "open source" EF](https://github.com/aspnet/EntityFramework6) jako punktu początkowego lub do odwołania.

Należy pamiętać, że począwszy od EF6 dostawca EF jest mniej ściśle połączony z podstawowym dostawcą ADO.NET. Ułatwia to pisanie dostawcy EF bez konieczności pisania ani zawijania klas ADO.NET.
