---
title: Połączenie odporności logikę ponawiania prób i - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997488"
---
# <a name="connection-resiliency-and-retry-logic"></a>Logika połączenia odporności, a następnie spróbuj ponownie
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Aplikacje nawiązywania połączenia z serwerem bazy danych zawsze były podatne na podziały połączenia z powodu niepowodzenia zaplecza i niestabilności sieci. Jednak w środowisku sieci LAN, na podstawie działa z serwerów dedykowanych bazy danych te błędy są wystarczająco rzadkie, że dodatkowa logika do obsługi tych błędów nie jest często wymagane. Za pomocą w chmurze oparte mniej niezawodnej sieci, z którymi jest teraz bardziej powszechne podziału połączenia wystąpienia serwerów baz danych, takich jak Windows Azure SQL Database oraz połączeń. Może to być spowodowane obrony technik, które w chmurze Użyj bazy danych, aby zapewnić sprawiedliwe usługi, takie jak ograniczanie przepustowości połączenia lub do niestabilności w sieci, powodują sporadyczne przekroczeń limitu czasu i innych błędów przejściowych.  

Elastyczność połączenia odnosi się do możliwości EF do automatycznego ponawiania próby wykonania dowolne polecenia, które się nie powieść z powodu przerwy te połączenia.  

## <a name="execution-strategies"></a>Strategii wykonywania  

Ponów próbę połączenia są wykonywane przez implementację interfejsu IDbExecutionStrategy. Implementacje IDbExecutionStrategy będzie odpowiedzialny za akceptować operacji, a jeśli wystąpi wyjątek, określająca, czy ponowienie próby jest odpowiednia i ponawianie próby, jeśli jest. Istnieją cztery strategii wykonywania, które są dostarczane z programem EF:  

1. **DefaultExecutionStrategy**: Ta strategia wykonywania nie ponawia próby żadnych operacji, jest ustawieniem domyślnym dla baz danych innych niż sql server.  
2. **DefaultSqlExecutionStrategy**: jest to strategii wykonywania wewnętrzny używany domyślnie. Ta strategia nie ponawia próby w ogóle, jednak opakować wszystkie wyjątki, które mogą być przejściowe poinformować użytkowników, którzy chcą włączyć elastyczność połączenia.  
3. **DbExecutionStrategy**: Ta klasa jest odpowiednie, jako klasę bazową dla innych strategii wykonywania, w tym własne niestandardowe. Implementuje zasady ponawiania wykładniczego, gdzie początkowej ponawiania się dzieje z zero opóźnienia i opóźnienia rośnie wykładniczo, dopóki nie zostanie osiągnięty jako maksymalna liczba ponowień. Ta klasa posiada metodę ShouldRetryOn abstrakcyjną, która może być implementowany w strategii wykonywania pochodnych do kontrolowania, wyjątki, które należy wykonać ponownie.  
4. **SqlAzureExecutionStrategy**: Ta strategia wykonywania dziedziczy DbExecutionStrategy i spróbuje ponowić operację na wyjątki, które są znane jako prawdopodobnie przejściowy podczas pracy z usługą Azure SQL Database.

> [!NOTE]
> Strategii wykonywania 2 i 4 są uwzględnione w dostawcy programu Sql Server, który jest dostarczany z programem EF, który znajduje się w zestawie EntityFramework.SqlServer i zostały zaprojektowane do pracy z programem SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Włączanie strategii wykonywania  

Najprostszym sposobem Poinformuj EF, aby użyć strategii wykonywania jest przy użyciu metody SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) klasy:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Ten kod informuje EF, aby użyć SqlAzureExecutionStrategy podczas nawiązywania połączenia z programem SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Konfigurowanie strategii wykonywania  

Konstruktor obiektu SqlAzureExecutionStrategy może akceptować dwa parametry: wartość MaxRetryCount i MaxDelay. Liczba MaxRetry jest maksymalna liczba strategii ponownych prób. MaxDelay jest element TimeSpan reprezentujący Maksymalne opóźnienie między kolejnymi próbami używających strategii wykonywania.  

Aby ustawić maksymalną liczbę ponownych prób 1 i maksymalne opóźnienie do 30 sekund będzie się execue następujące czynności:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

SqlAzureExecutionStrategy ponowi próbę natychmiast po raz pierwszy błąd przejściowy, który występuje, ale będzie już opóźnienie między kolejnymi próbami, dopóki albo maksymalny limit ponownych prób zostanie przekroczony lub łącznego czasu trafienia Maksymalne opóźnienie.  

Strategii wykonywania ponowi tylko ograniczoną liczbę wyjątków, które są zwykle tansient, nadal konieczne będzie obsługiwać inne błędy, a także przechwytywanie wyjątku RetryLimitExceeded w przypadku których błąd nie jest przejściowy lub trwa zbyt długo rozwiązać samego siebie.  

## <a name="limitations"></a>Ograniczenia  

Korzystając z Trwa ponawianie próby strategii wykonywania są niektóre znane ograniczenia:  

### <a name="streaming-queries-are-not-supported"></a>Zapytania przesyłania strumieniowego nie są obsługiwane.  

Domyślnie programy EF6 i nowszym będzie buforować wyniki zapytania, a nie ich streaming. Jeśli chcesz mieć wyniki przesyłane strumieniowo można metoda AsStreaming służy do zmiany LINQ zapytania jednostki w celu przesyłania strumieniowego.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Przesyłania strumieniowego nie jest obsługiwana, gdy Trwa ponawianie próby strategii wykonywania jest zarejestrowany. To ograniczenie istnieje, ponieważ połączenie można porzucić część sposób wyniki są zwracane. W takiej sytuacji EF musi zostać ponownie uruchomiony całe zapytanie, ale nie ma niezawodne możliwości informacji o tym, co powoduje już zostały zwrócone (dane mogły ulec zmianie od momentu początkowego zapytania została wysłana, wyniki mogą wrócić w innej kolejności, wyniki nie może mieć unikatowy identyfikator itp.).  

### <a name="user-initiated-transactions-not-supported"></a>Użytkownik zainicjował transakcji nie jest obsługiwane  

Po skonfigurowaniu strategii wykonywania, który skutkuje ponownych prób, istnieją pewne ograniczenia dotyczące użycia transakcji.  

#### <a name="whats-supported-efs-default-transaction-behavior"></a>Jakie operacje są obsługiwane: firmy EF domyślne zachowanie transakcji  

Domyślnie program EF będzie wykonywać żadnych aktualizacji bazy danych w obrębie transakcji. Nie trzeba nic robić, aby włączyć tę opcję, EF zawsze wykonuje to automatycznie.  

Na przykład w poniższym kodzie SaveChanges jest wykonywana automatycznie w ramach transakcji. Gdyby SaveChanges się niepowodzeniem po Wstawianie jedną nową lokację, a następnie będzie można z powrotem obniżyć transakcji i nie zmiany zostały wprowadzone do bazy danych. Kontekst jest również pozostanie w stanie umożliwiającym Funkcja SaveChanges zapisuje do ponownie wywołany, aby ponowić próbę zastosowania zmian.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a>Co to jest nieobsługiwana: użytkownik zainicjował transakcji  

Bez korzystania z Trwa ponawianie próby strategii wykonywania może zawijać się wiele operacji w ramach jednej transakcji. Na przykład poniższy kod opakowuje dwóch wywołań funkcji SaveChanges w ramach jednej transakcji. Jeśli którejkolwiek którejkolwiek z tych operacji nie powiedzie się następnie zmiany zostaną zastosowane.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

To nie jest obsługiwana, gdy za pomocą Trwa ponawianie próby strategii wykonywania, ponieważ EF nie jest świadomy żadnych poprzedniej operacji i sposób ponowić próbę ich wykonania. Na przykład jeśli drugi SaveChanges nie powiodła się następnie EF już ma wymagane informacje, aby ponowić próbę wykonania pierwszego wywołania funkcji SaveChanges.  

#### <a name="possible-workarounds"></a>Możliwe obejścia  

##### <a name="suspend-execution-strategy"></a>Wstrzymywanie strategii wykonywania  

Jednym możliwym obejściem jest wstrzymania Trwa ponawianie próby strategia wykonywania dla fragmentu kodu, który musi używać użytkownik zainicjował transakcji. Najprostszym sposobem, w tym celu jest dodać flagę SuspendExecutionStrategy w kodzie na podstawie klasy konfiguracji, a także zmienić lambda strategii wykonywania do zwrócenia strategii wykonywania (inne niż retying) domyślny, jeśli flaga jest ustawiona.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Należy pamiętać, że używasz CallContext do przechowywania wartości flagi. Zapewnia funkcje podobne do pamięci lokalnej wątku, ale jest bezpieczne, za pomocą kodu asynchronicznego — w tym zapytania asynchroniczne i zapisywać przy użyciu platformy Entity Framework.  

Firma Microsoft jest teraz wstrzymywanie strategii wykonywania dla sekcji kodu, który używa transakcji zainicjowanej przez użytkownika.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a>Ręcznego wywoływania strategii wykonywania  

Inną opcją jest ręczne Użyj strategii wykonywania i nadaj cały zestaw logiki, aby uruchomić, dzięki czemu można ponów wszystko, jeśli jedna z operacji zakończy się niepowodzeniem. Nadal trzeba wstrzymywanie strategii wykonywania - korzystające z techniki powyżej — tak aby kontekstów używana wewnątrz blok kodu powtarzający operację nie należy podejmować próbę.  

Należy pamiętać, że w bloku kodu, ponowienie próby powinien być konstruowany kontekstów. Dzięki temu rozpoczynamy przy użyciu czystego stanu przeznaczonego do każdego ponownych prób.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
