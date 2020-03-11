---
title: Praca z transakcjami — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419686"
---
# <a name="working-with-transactions"></a>Praca z transakcjami
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

W tym dokumencie opisano użycie transakcji w programie EF6, w tym ulepszeń dodanych przez nas od EF5 w celu ułatwienia pracy z transakcjami.  

## <a name="what-ef-does-by-default"></a>Domyślne działanie EF  

We wszystkich wersjach Entity Framework, za każdym razem, gdy wykonasz **metody SaveChanges ()** do wstawiania, aktualizowania lub usuwania bazy danych, struktura spowoduje zawinięcie tej operacji w transakcji. Ta transakcja trwa tylko wystarczająco długo, aby można było wykonać operację, a następnie została ukończona. Po wykonaniu innej operacji jest uruchomiona nowa transakcja.  

Począwszy od programu EF6 **Database. ExecuteSqlCommand ()** domyślnie umieści polecenie w transakcji, jeśli jeszcze nie zostało to zrobione. Istnieją przeciążenia tej metody, które umożliwiają przesłonięcie tego zachowania, jeśli chcesz. Również w EF6 wykonywania procedur składowanych zawartych w modelu za pomocą interfejsów API, takich jak **ObjectContext. ExecuteFunction ()** , jest taka sama (z tą różnicą, że zachowanie domyślne nie może zostać zastąpione).  

W obu przypadkach poziom izolacji transakcji to każdy poziom izolacji dostawcy bazy danych traktuje ustawienie domyślne. Domyślnie na przykład na SQL Server jest to READ COMMITTED.  

Entity Framework nie otacza zapytań w transakcji.  

Ta funkcja domyślna jest odpowiednia dla dużej liczby użytkowników, a więc nie trzeba wykonywać żadnych innych czynności w EF6; wystarczy napisać kod, tak jak zawsze.  

Jednak niektórzy użytkownicy wymagają większej kontroli nad ich transakcjami — jest to omówione w poniższych sekcjach.  

## <a name="how-the-apis-work"></a>Jak działają interfejsy API  

Przed EF6 Entity Framework nawiązać połączenie z bazą danych (wyjątek został zakończony, jeśli przekazano połączenie, które zostało już otwarte). Ponieważ transakcja może być uruchamiana tylko w ramach otwartego połączenia, oznacza to, że jedynym sposobem, w jaki użytkownik może otoczyć kilka operacji do jednej transakcji, było użycie elementu [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) lub użycie właściwości **ObjectContext. Connection** i rozpoczęcie wywoływania metody **Open ()** i **BeginTransaction ()** bezpośrednio na zwracanym obiekcie **EntityConnection** . Dodatkowo wywołania interfejsu API, które skontaktowali się z bazą danych, byłyby niepowodzeniem, jeśli rozpoczęto transakcję na źródłowym połączeniu z bazą danych.  

> [!NOTE]
> Ograniczenie tylko akceptowania zamkniętych połączeń zostało usunięte w Entity Framework 6. Aby uzyskać szczegółowe informacje, zobacz [Zarządzanie połączeniami](~/ef6/fundamentals/connection-management.md).  

Począwszy od EF6, platforma udostępnia teraz:  

1. **Database. BeginTransaction ()** : łatwiejsza metoda umożliwiająca użytkownikowi uruchamianie i Kończenie transakcji w ramach istniejącego DbContext — pozwala to na łączenie kilku operacji w ramach tej samej transakcji, a więc wszystkie zatwierdzone lub wszystkie wycofane jako jeden. Umożliwia także użytkownikowi łatwiejsze Określanie poziomu izolacji transakcji.  
2. **Database. UseTransaction ()** : który umożliwia DbContext używanie transakcji, która została uruchomiona poza Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Łączenie kilku operacji w jedną transakcję w tym samym kontekście  

**Database. BeginTransaction ()** ma dwa przesłonięcia — jeden, który pobiera jawne [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) i jeden nie przyjmuje argumentów i używa domyślnego IsolationLevel z dostawcy bazy danych. Oba zastąpienia zwracają obiekt **DbContextTransaction** , który dostarcza metody **commit ()** i **Rollback ()** , które wykonują zatwierdzenie i wycofywanie w źródłowej transakcji magazynu.  

**DbContextTransaction** jest przeznaczony do usunięcia, gdy zostanie on zatwierdzony lub wycofany. Jednym z łatwych sposobów osiągnięcia tego jest **użycie (...) {.** ..} Składnia, która automatycznie wywoła metodę **Dispose ()** , gdy zostanie ukończony blok using:  

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
            }
        }
    }
}
```  

> [!NOTE]
> Rozpoczęcie transakcji wymaga otwarcia podstawowego połączenia z magazynem. Wywołanie metody Database. BeginTransaction () spowoduje otwarcie połączenia, jeśli nie zostało ono jeszcze otwarte. Jeśli DbContextTransaction otwarto połączenie, zostanie ono zamknięte po wywołaniu metody Dispose ().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Przekazywanie istniejącej transakcji do kontekstu  

Czasami chcesz, aby transakcja była jeszcze szerszym zakresem i która obejmuje operacje w tej samej bazie danych, ale poza całością. Aby to osiągnąć, należy otworzyć połączenie i uruchomić transakcję samodzielnie, a następnie poinformować Dr a), aby użyć już otwartego połączenia bazy danych i b) w celu użycia istniejącej transakcji dla tego połączenia.  

Aby to zrobić, należy zdefiniować i użyć konstruktora w klasie kontekstu, która dziedziczy z jednego z konstruktorów DbContext, które przyjmują i) istniejący parametr połączenia i II) wartość logiczna contextOwnsConnection.  

> [!NOTE]
> Flaga contextOwnsConnection musi być ustawiona na wartość false, gdy zostanie wywołana w tym scenariuszu. Jest to ważne, ponieważ informuje Entity Framework, że nie powinna zamykać połączenia, gdy jest ono wykonywane (na przykład wiersz 4 poniżej):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Ponadto musisz samodzielnie uruchomić transakcję (w tym IsolationLevel, jeśli chcesz uniknąć domyślnego ustawienia) i powiadom Entity Framework, że istnieje już istniejąca transakcja w ramach połączenia (Zobacz wiersz 33 poniżej).  

Następnie można wykonywać operacje bazy danych bezpośrednio z elementu SqlConnection lub w kontekście DbContext. Wszystkie takie operacje są wykonywane w ramach jednej transakcji. Użytkownik zobowiązuje się do zatwierdzania lub wycofywania transakcji oraz do wywoływania operacji Dispose (), a także do zamykania i usuwania połączenia z bazą danych. Na przykład:  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>Czyszczenie transakcji

Można przekazać wartość null do bazy danych. UseTransaction () w celu wyczyszczenia informacji o bieżącej transakcji Entity Framework. W takim przypadku Entity Framework nie zatwierdzi ani nie wycofał istniejącej transakcji, dlatego należy używać jej z opieką i tylko wtedy, gdy masz pewność, co chcesz zrobić.  

### <a name="errors-in-usetransaction"></a>Błędy w UseTransaction

Zostanie wyświetlony wyjątek z bazy danych. UseTransaction (), jeśli przejdziesz do transakcji, gdy:  
- Entity Framework ma już istniejącą transakcję  
- Entity Framework działa już w ramach elementu TransactionScope  
- Obiekt Connection w przekazaniu transakcji ma wartość null. Oznacza to, że transakcja nie jest skojarzona z połączeniem — zwykle jest to znak, który transakcja została już ukończona.  
- Obiekt połączenia w przekazaniu transakcji nie jest zgodny z połączeniem Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Używanie transakcji z innymi funkcjami  

W tej sekcji szczegółowo opisano sposób działania powyższych transakcji:  

- Elastyczność połączenia  
- Metody asynchroniczne  
- Transakcje elementu TransactionScope  

### <a name="connection-resiliency"></a>Elastyczność połączenia  

Nowa funkcja odporności połączenia nie działa z zainicjowanymi przez użytkownika transakcjami. Aby uzyskać szczegółowe informacje, zobacz [ponawianie strategii wykonywania](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programowanie asynchroniczne  

Podejście opisane w poprzednich sekcjach nie wymaga żadnych dalszych opcji ani ustawień do pracy z [zapytań asynchronicznych i zapisywania metod](~/ef6/fundamentals/async.md
). Należy jednak pamiętać, że w zależności od tego, co robisz w metodach asynchronicznych, może to skutkować długotrwałymi transakcjami, co może spowodować zakleszczenie lub blokowanie, które są nieprawidłowe dla wydajności ogólnej aplikacji.  

### <a name="transactionscope-transactions"></a>Transakcje elementu TransactionScope  

Przed EF6 zalecanym sposobem udostępniania większego zakresu transakcji było użycie obiektu TransactionScope:  

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

Elementy SqlConnection i Entity Framework mogą używać otoczenia transakcji TransactionScope i dlatego być zaangażowane ze sobą.  

Począwszy od platformy .NET 4.5.1 element TransactionScope został zaktualizowany, aby działał również z metodami asynchronicznymi przy użyciu wyliczenia [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :  

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

Nadal istnieją pewne ograniczenia dotyczące podejścia elementu TransactionScope:  

- Program wymaga programu .NET 4.5.1 lub nowszego do pracy z metodami asynchronicznymi.  
- Nie można jej używać w scenariuszach chmury, chyba że masz tylko jedno połączenie (scenariusze chmury nie obsługują transakcji rozproszonych).  
- Nie można go połączyć z podejściem Database. UseTransaction () w poprzednich sekcjach.  
- Spowoduje to wygenerowanie wyjątków w przypadku wystawiania jakichkolwiek DDL i niewłączonych transakcji rozproszonych za pomocą usługi MSDTC.  

Zalety podejścia elementu TransactionScope:  

- W przypadku wprowadzenia więcej niż jednego połączenia do danej bazy danych lub połączenia z jedną bazą danych przy użyciu połączenia z inną bazą danych w ramach tej samej transakcji zostanie automatycznie uaktualniona transakcja lokalna do transakcji rozproszonej. usługa MSDTC skonfigurowana tak, aby zezwalała na wykonywanie transakcji rozproszonych.  
- Łatwość kodowania. Jeśli wolisz, aby transakcja była otaczająca i niejawnie w tle, a nie w sposób jawny, to podejście TransactionScope może być bardziej odpowiednie.  

Podsumowując, z użyciem nowych interfejsów API Database. BeginTransaction () i Database. UseTransaction () powyżej, podejście TransactionScope nie jest już konieczne dla większości użytkowników. W przypadku kontynuowania korzystania z elementu TransactionScope należy pamiętać o powyższych ograniczeniach. Zalecamy używanie podejścia przedstawionego w poprzednich sekcjach zamiast tego, gdzie to możliwe.  
