---
title: Odporność połączeń i logika ponawiania — EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824839"
---
# <a name="connection-resiliency-and-retry-logic"></a>Odporność połączeń i logika ponowień
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Aplikacje łączące się z serwerem bazy danych zawsze były podatne na przerwy z połączeniami z powodu awarii zaplecza i niestabilności sieci. Jednak w środowisku opartym na sieci LAN pracującym z serwerami dedykowanych baz danych te błędy są wystarczająco rzadko, ponieważ dodatkowa logika obsługi tych awarii nie jest często wymagana. Ze wzrostem liczby serwerów baz danych opartych na chmurze, takich jak Azure SQL Database systemu Windows i połączeń za pośrednictwem mniej niezawodnych sieci, jest teraz bardziej często spotykane w celu nawiązania połączenia. Może to być spowodowane technikami obronnymi używanymi przez bazy danych w chmurze w celu zapewnienia sprawiedliwości usług, takich jak ograniczanie połączenia lub niestabilność w sieci, powodującej sporadyczne przekroczenia limitu czasu i innych błędów przejściowych.  

Odporność połączenia odnosi się do zdolności programu EF do automatycznego ponawiania prób wszelkich poleceń, które kończą się niepowodzeniem z powodu tych przerw połączeń.  

## <a name="execution-strategies"></a>Strategie wykonywania  

Ponowienie połączenia jest wykonywane przez implementację interfejsu IDbExecutionStrategy. Implementacje IDbExecutionStrategy będą odpowiedzialne za zaakceptowanie operacji, a jeśli wystąpi wyjątek, określenie, czy ponowienie próby jest odpowiednie i ponawianie próby, jeśli jest to możliwe. Istnieją cztery strategie wykonywania dostarczane z EF:  

1. **DefaultExecutionStrategy**: ta strategia wykonywania nie ponawia żadnej operacji, jest to wartość domyślna dla baz danych innych niż SQL Server.  
2. **DefaultSqlExecutionStrategy**: jest to wewnętrzna strategia wykonywania, która jest używana domyślnie. Ta strategia nie ponawia żadnej próby, jednak spowoduje zawinięcie wszelkich wyjątków, które mogą być przejściowe, aby poinformować użytkowników, że mogą chcieć włączyć odporność połączenia.  
3. **DbExecutionStrategy**: Ta klasa jest odpowiednia jako klasa bazowa dla innych strategii wykonywania, w tym własnych niestandardowych. Implementuje on zasady ponowień wykładniczych, w przypadku których początkowa ponowna próba nastąpi z opóźnieniem równym zero, a opóźnienie wzrasta wykładniczo, dopóki Maksymalna liczba ponownych prób zostanie osiągnięta Ta klasa ma abstrakcyjną metodę ShouldRetryOn, która może być implementowana w pochodnych strategiach wykonywania w celu kontrolowania, które wyjątki powinny być ponawiane.  
4. **SqlAzureExecutionStrategy**: ta strategia wykonywania dziedziczy z DbExecutionStrategy i ponowi próbę po wyjątkach, które są prawdopodobnie przejściowe podczas pracy z Azure SQL Database.

> [!NOTE]
> Strategie wykonywania 2 i 4 są zawarte w dostawcy programu SQL Server, który jest dostarczany z dr, który znajduje się w zestawie EntityFramework. SqlServer i jest przeznaczony do pracy z SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Włączanie strategii wykonywania  

Najprostszym sposobem, aby poinformować Dr o korzystanie z strategii wykonywania, jest metoda SetExecutionStrategy klasy [dbconfiguration](~/ef6/fundamentals/configuring/code-based.md) :  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Ten kod nakazuje programowi Dr użycie SqlAzureExecutionStrategy podczas nawiązywania połączenia z SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Konfigurowanie strategii wykonywania  

Konstruktor elementu SqlAzureExecutionStrategy może przyjmować dwa parametry, wartość MaxRetryCount i MaxDelay. Liczba MaxRetry jest maksymalną liczbą ponownych prób w strategii. MaxDelay to przedział czasu reprezentujący maksymalne opóźnienie między ponownymi próbami, które będą używane przez strategię wykonywania.  

Aby ustawić maksymalną liczbę ponownych prób na 1 i maksymalne opóźnienie na 30 sekund, należy wykonać następujące czynności:  

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

SqlAzureExecutionStrategy ponowi próbę natychmiast po pierwszym wystąpieniu błędu przejściowego, ale wydłuża czas oczekiwania na przekroczenie limitu maksymalnej liczby ponownych prób lub łącznego czasu osiągnie maksymalne opóźnienie.  

W strategiach wykonywania będzie można ponowić tylko ograniczoną liczbę wyjątków, które zwykle są przejściowe. nadal trzeba będzie obsługiwać inne błędy, a także przechwycić wyjątek RetryLimitExceeded w przypadku, gdy błąd nie jest przejściowy lub trwa zbyt długo, aby rozwiązać ten problem. sam.  

Istnieją pewne znane ograniczenia związane z ponowną próbą wykonania:  

## <a name="streaming-queries-are-not-supported"></a>Zapytania przesyłania strumieniowego nie są obsługiwane  

Domyślnie EF6 i nowsze wersje będą buforować wyniki zapytania zamiast przesyłania strumieniowego. Jeśli chcesz uzyskać strumieniowe wyniki, możesz użyć metody AsStreaming, aby zmienić zapytanie LINQ to Entities do przesyłania strumieniowego.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Przesyłanie strumieniowe nie jest obsługiwane w przypadku zarejestrowania strategii wykonywania ponownych prób. To ograniczenie istnieje, ponieważ połączenie może zostać porzucane w formie częściowej przez zwracane wyniki. W takim przypadku EF musi ponownie uruchomić całe zapytanie, ale nie ma niezawodnego sposobu na poznanie wyników, które zostały już zwrócone (dane mogły zostać zmienione od czasu wysłania zapytania, wyniki mogą być ponownie w innej kolejności, wyniki mogą nie mieć unikatowego identyfikatora itp.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Transakcje zainicjowane przez użytkownika nie są obsługiwane  

Po skonfigurowaniu strategii wykonywania, która powoduje ponowną próbę, istnieją pewne ograniczenia dotyczące korzystania z transakcji.  

Domyślnie program EF wykona wszystkie aktualizacje bazy danych w ramach transakcji. Nie musisz nic robić, aby go włączyć. EF zawsze robi to automatycznie.  

Na przykład w poniższym kodzie metody SaveChanges jest automatycznie wykonywane w ramach transakcji. Jeśli metody SaveChanges nie powiedzie się po wstawieniu jednej z nowych witryn, transakcja zostanie wycofana i nie zostaną zastosowane żadne zmiany w bazie danych. Kontekst jest również pozostawiony w stanie, który umożliwia ponowne wywołanie metody SaveChanges, aby ponowić próbę zastosowania zmian.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Gdy nie korzystasz z kolejnej strategii wykonywania, możesz otoczyć wiele operacji w jednej transakcji. Na przykład poniższy kod otacza dwa wywołania metody SaveChanges w jednej transakcji. Jeśli jakakolwiek część jednej operacji nie powiedzie się, żadne zmiany nie zostaną zastosowane.  

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

Nie jest to obsługiwane w przypadku korzystania z metody ponawiania próby, ponieważ EF nie wie o żadnych poprzednich operacjach i sposobach ich ponowienia. Na przykład jeśli druga metody SaveChanges nie powiodła się, EF nie ma już wymaganych informacji, aby ponowić próbę wykonania pierwszego wywołania metody SaveChanges.  

### <a name="solution-manually-call-execution-strategy"></a>Rozwiązanie: ręczne wywoływanie strategii wykonywania  

Rozwiązaniem jest ręczne Użycie strategii wykonywania i nadanie jej całego zestawu logiki, dzięki czemu można ponowić próbę wszystkiego, jeśli jedna z operacji zakończy się niepowodzeniem. Po uruchomieniu strategii wykonywania pochodzącej od DbExecutionStrategy zostanie ona zawiesić niejawną strategię wykonywania używaną w metody SaveChanges.  

Należy zauważyć, że wszystkie konteksty należy skonstruować w bloku kodu, aby można było ponowić próbę. Gwarantuje to, że rozpoczynamy od stanu czystego dla każdej ponowienia próby.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

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
```  
