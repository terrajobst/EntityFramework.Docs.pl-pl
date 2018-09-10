---
title: Praca z transakcje — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 26473e1e52a6044babc717d5b158ad73aac5c738
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250612"
---
# <a name="working-with-transactions"></a>Praca z transakcji
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

W tym dokumencie opisano, za pomocą transakcji w EF6, w tym rozszerzeń, które dodaliśmy od EF5, aby ułatwić pracę z transakcji.  

## <a name="what-ef-does-by-default"></a>Domyślne działanie EF  

We wszystkich wersjach programu Entity Framework, każde wykonanie **SaveChanges()** do wstawiania, aktualizacji lub Usuń w bazie danych w ramach będzie zawijany w tej operacji w transakcji. Ta transakcja obowiązuje tylko wystarczająco długi, aby wykonać operację, a następnie kończy. Podczas wykonywania innej takich operacji nowa transakcja jest uruchomiona.  

Począwszy od platformy EF6 **Database.ExecuteSqlCommand()** domyślnie będzie zawijany polecenia w transakcji, jeśli odpowiedni klucz nie został już istnieje. Istnieją przeciążenia tej metody, które pozwalają na zastąpienie tego zachowania, jeśli chcesz, aby. Również w EF6 wykonywania procedur przechowywanych, które są uwzględnione w modelu za pośrednictwem interfejsów API, takich jak **ObjectContext.ExecuteFunction()** działa tak samo (z tą różnicą, że zachowanie domyślne w tej chwili przesłanianie).  

W obu przypadkach poziom izolacji transakcji jest poziom izolacji, niezależnie od dostawcy bazy danych uwzględnia ustawienie domyślne. Domyślnie na przykład w programie SQL Server to READ COMMITTED.  

Entity Framework nie jest zawijany zapytań w ramach transakcji.  

Ta funkcja domyślne jest odpowiednie dla wielu użytkowników i jeśli istnieje więc nie trzeba wykonywać dodatkowe czynności w EF6; Wystarczy napisać odpowiedni kod, tak jak zawsze.  

Jednak niektóre użytkownicy muszą mieć większą kontrolę nad ich transakcje — to zagadnienie opisano w poniższych sekcjach.  

## <a name="how-the-apis-work"></a>Jak działają interfejsy API  

Przed EF6 Entity Framework nalegała na otwieranie połączenia z bazą danych sam (spowodował on wyjątek została przekazana połączenia, który był wcześniej otwarty). Ponieważ transakcji można uruchomić tylko na otwartego połączenia, oznacza to, jedynym sposobem użytkownika można opakować kilka operacji jako jedna transakcja została programu [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) lub użyj  **ObjectContext.Connection** właściwości i wywoływania start **Open()** i **BeginTransaction()** bezpośrednio na zwracanego **EntityConnection** obiekt. Dodatkowo wywołania interfejsu API, które skontaktować się z bazy danych zakończy się niepowodzeniem, jeśli transakcji zostało uruchomione na podstawowej połączenia z bazą danych na własną rękę.  

> [!NOTE]
> Ograniczenie tylko przyjmować połączenia zamknięte został usunięty w Entity Framework 6. Aby uzyskać więcej informacji, zobacz [zarządzania połączeniami](~/ef6/fundamentals/connection-management.md).  

Począwszy od platformy EF6 ramach teraz zawiera:  

1. **Database.BeginTransaction()** : łatwiejszy dla użytkownika uruchomić i realizowania transakcji siebie w ramach istniejącego typu DbContext — co kilka operacji, które można połączyć w ramach tej samej transakcji i dlatego wszystkie zatwierdzone lub wszystkie wycofane jako jeden. Umożliwia użytkownikowi łatwiej określić poziom izolacji transakcji.  
2. **Database.UseTransaction()** : pozwalający DbContext używać transakcji, która została uruchomiona poza programem Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Łącząc kilka operacji w jednej transakcji, w tym samym kontekście  

**Database.BeginTransaction()** ma dwa zastąpienia — taki, który przyjmuje jawnego [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) i jedną, która nie przyjmuje żadnych argumentów i używa domyślnego IsolationLevel od podstawowego dostawcy bazy danych. Zwraca zarówno zastąpienia **DbContextTransaction** obiektu, który zapewnia **Commit()** i **Rollback()** metod, których wykonywanie zatwierdzenia i wycofywania na odpowiedni magazyn transakcja.  

**DbContextTransaction** ma być usuwane, gdy została przekazana lub wycofana. Jeden łatwy sposób osiągnąć ten cel jest **using(...) {...}** Składnia, która będzie automatycznie wywoływać **Dispose()** przypadku za pomocą bloku kończy:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> Rozpoczęcia transakcji wymaga, że połączenie podstawowe magazynu jest otwarty. Dlatego wywołanie Database.BeginTransaction() spowoduje otwarcie połączenia, jeśli nie jest już otwarty. Jeśli DbContextTransaction otwarte połączenie następnie zostaną zamknięte go po wywołaniu metody Dispose().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Przekazywanie istniejącej transakcji do kontekstu  

Czasami chcesz transakcji jest szersze w zakresie i który obejmuje operacje, w tej samej bazy danych, ale poza programem EF całkowicie. W tym celu należy otworzyć połączenie i rozpocząć transakcję, samodzielnie i następnie poinformuj EF) do użycia już otwarte połączenia bazy danych i (b) korzystania z istniejącej transakcji dla tego połączenia.  

W tym celu należy zdefiniować i użyć konstruktora w swojej klasy kontekstu, który dziedziczy z jednego typu DbContext konstruktorów przyjmujących i) istniejących parametrów połączenia i ii) contextOwnsConnection boolean.  

> [!NOTE]
> Flaga contextOwnsConnection musi być równa FAŁSZ, gdy wywołuje się, w tym scenariuszu. Jest to ważne, ponieważ informuje Entity Framework, go nie powinna zamykać połączenia po jej zakończeniu z nim (na przykład, zobacz wiersz do 4 opisane poniżej):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Ponadto należy rozpocząć transakcję, samodzielnie (łącznie IsolationLevel, jeśli chcesz uniknąć ustawienie domyślne) i umożliwić Entity Framework wiedzieć, że jest istniejącej transakcji, już uruchomiona w ramach połączenia (zobacz wiersz 33 poniżej).  

To bezpłatna, można wykonać operacji bazy danych bezpośrednio na element SqlConnection, samego lub dla kontekstu DbContext. Wszystkie operacje są wykonywane w ramach jednej transakcji. Możesz skorzystać z odpowiedzialności potwierdzenia lub wycofania transakcji i wywoływania metody Dispose() w nim, a także zamykania i usuwania połączenia z bazą danych. Na przykład:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>Czyszczenie się na transakcję

Można przekazać wartości null do Database.UseTransaction() wyczyść Entity Framework wiedzę na temat bieżącej transakcji. Entity Framework będzie ani zatwierdzenia ani wycofać istniejącej transakcji, gdy to zrobisz, więc używać ostrożnie, i tylko wtedy, gdy wiesz, jest to, co chcesz zrobić.  

### <a name="errors-in-usetransaction"></a>Błędy w UseTransaction

Wyjątek z Database.UseTransaction() zostanie wyświetlony w przypadku przekazania transakcji po:  
- Entity Framework jest już w istniejącej transakcji  
- Entity Framework jest już działające w ramach elementu TransactionScope  
- Obiekt połączenia w ramach transakcji przekazywany jest pusty. Oznacza to, że transakcja nie jest skojarzona z połączeniem — zazwyczaj jest to znak, że transakcja została już zakończona.  
- Obiekt połączenia w ramach transakcji przekazywany jest niezgodna z połączenia programu Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Za pomocą transakcji z innymi funkcjami  

Tej sekcji opisano szczegółowo sposób interakcji powyżej transakcji z:  

- Elastyczność połączenia  
- Metody asynchroniczne  
- Transakcje elementu TransactionScope  

### <a name="connection-resiliency"></a>Elastyczność połączenia  

Nowej funkcji elastyczności połączenia nie działa z transakcjami zainicjowanego przez użytkownika. Aby uzyskać więcej informacji, zobacz [ponawiania strategii wykonywania](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programowanie asynchroniczne  

Podejście opisane w poprzednich sekcjach musi nie dalsze opcje lub ustawienia do pracy z [asynchronicznego zapytania i Zapisz metody](~/ef6/fundamentals/async.md
). Należy jednak pamiętać, że, w zależności od tego, czy w ramach metod asynchronicznych, może to spowodować długotrwałych transakcji — które z kolei może spowodować zakleszczenia lub blokowania, czyli negatywnie wpływać na wydajność aplikacji ogólnej.  

### <a name="transactionscope-transactions"></a>Transakcje elementu TransactionScope  

Przed EF6 zalecaną metodą udostępniania większy zakres transakcji było jednoczesne używanie obiektu TransactionScope:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

Element SqlConnection i Entity Framework zarówno używa otoczenia transakcji TransactionScope i dlatego można zatwierdzić ze sobą.  

Począwszy od .NET 4.5.1 TransactionScope została zaktualizowana w celu również działać przy użyciu metod asynchronicznych za pomocą użytkowania [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) wyliczenia:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

Nadal istnieją pewne ograniczenia związane z podejścia TransactionScope:  

- Wymaga programu .NET 4.5.1 lub nowszej, aby pracować z metod asynchronicznych.  
- Jeśli nie masz pewności, masz tylko jedno połączenie nie można używać w chmurze (cloud scenariuszy nie obsługują transakcje rozproszone).  
- Nie można łączyć z podejściem Database.UseTransaction() w poprzedniej sekcji.  
- Będzie ona zgłaszają wyjątki, jeśli wystawiać wszelkie DDL i nie włączono transakcji rozproszonych za pomocą usługi MSDTC.  

Korzyści wynikające z podejścia TransactionScope:  

- Zostanie automatycznie uaktualniona lokalnej transakcji do poziomu transakcji rozproszonej Jeśli więcej niż jedno połączenie do określonej bazy danych lub połączyć połączenie z jedną bazę danych z połączeniem z innej bazy danych w ramach tej samej transakcji (Uwaga: konieczne jest posiadanie Usługa MSDTC skonfigurowane i umożliwiają transakcji rozproszonych, aby to działało).  
- Łatwość programowania. Jeśli użytkownik sobie tego życzy transakcji otoczenia i rozdanych z niejawnie w tle, a nie jawnie w obszarze kontrolować następnie podejście TransactionScope może własnych możesz lepiej.  

Podsumowując, za pomocą nowych Database.BeginTransaction() i API Database.UseTransaction() powyżej podejście TransactionScope nie jest już konieczne dla większości użytkowników. Jeśli będziesz nadal używać elementu TransactionScope następnie Pamiętaj o powyższe ograniczenia. Zaleca się przy użyciu metody opisane w poprzednich sekcjach zamiast tego, gdzie to możliwe.  
