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
# <a name="working-with-transactions"></a><span data-ttu-id="f75f5-102">Praca z transakcjami</span><span class="sxs-lookup"><span data-stu-id="f75f5-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="f75f5-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f75f5-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f75f5-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="f75f5-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f75f5-105">W tym dokumencie opisano użycie transakcji w programie EF6, w tym ulepszeń dodanych przez nas od EF5 w celu ułatwienia pracy z transakcjami.</span><span class="sxs-lookup"><span data-stu-id="f75f5-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="f75f5-106">Domyślne działanie EF</span><span class="sxs-lookup"><span data-stu-id="f75f5-106">What EF does by default</span></span>  

<span data-ttu-id="f75f5-107">We wszystkich wersjach Entity Framework, za każdym razem, gdy wykonasz **metody SaveChanges ()** do wstawiania, aktualizowania lub usuwania bazy danych, struktura spowoduje zawinięcie tej operacji w transakcji.</span><span class="sxs-lookup"><span data-stu-id="f75f5-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="f75f5-108">Ta transakcja trwa tylko wystarczająco długo, aby można było wykonać operację, a następnie została ukończona.</span><span class="sxs-lookup"><span data-stu-id="f75f5-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="f75f5-109">Po wykonaniu innej operacji jest uruchomiona nowa transakcja.</span><span class="sxs-lookup"><span data-stu-id="f75f5-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="f75f5-110">Począwszy od programu EF6 **Database. ExecuteSqlCommand ()** domyślnie umieści polecenie w transakcji, jeśli jeszcze nie zostało to zrobione.</span><span class="sxs-lookup"><span data-stu-id="f75f5-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="f75f5-111">Istnieją przeciążenia tej metody, które umożliwiają przesłonięcie tego zachowania, jeśli chcesz.</span><span class="sxs-lookup"><span data-stu-id="f75f5-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="f75f5-112">Również w EF6 wykonywania procedur składowanych zawartych w modelu za pomocą interfejsów API, takich jak **ObjectContext. ExecuteFunction ()** , jest taka sama (z tą różnicą, że zachowanie domyślne nie może zostać zastąpione).</span><span class="sxs-lookup"><span data-stu-id="f75f5-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="f75f5-113">W obu przypadkach poziom izolacji transakcji to każdy poziom izolacji dostawcy bazy danych traktuje ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="f75f5-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="f75f5-114">Domyślnie na przykład na SQL Server jest to READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="f75f5-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="f75f5-115">Entity Framework nie otacza zapytań w transakcji.</span><span class="sxs-lookup"><span data-stu-id="f75f5-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="f75f5-116">Ta funkcja domyślna jest odpowiednia dla dużej liczby użytkowników, a więc nie trzeba wykonywać żadnych innych czynności w EF6; wystarczy napisać kod, tak jak zawsze.</span><span class="sxs-lookup"><span data-stu-id="f75f5-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="f75f5-117">Jednak niektórzy użytkownicy wymagają większej kontroli nad ich transakcjami — jest to omówione w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="f75f5-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="f75f5-118">Jak działają interfejsy API</span><span class="sxs-lookup"><span data-stu-id="f75f5-118">How the APIs work</span></span>  

<span data-ttu-id="f75f5-119">Przed EF6 Entity Framework nawiązać połączenie z bazą danych (wyjątek został zakończony, jeśli przekazano połączenie, które zostało już otwarte).</span><span class="sxs-lookup"><span data-stu-id="f75f5-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="f75f5-120">Ponieważ transakcja może być uruchamiana tylko w ramach otwartego połączenia, oznacza to, że jedynym sposobem, w jaki użytkownik może otoczyć kilka operacji do jednej transakcji, było użycie elementu [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) lub użycie właściwości **ObjectContext. Connection** i rozpoczęcie wywoływania metody **Open ()** i **BeginTransaction ()** bezpośrednio na zwracanym obiekcie **EntityConnection** .</span><span class="sxs-lookup"><span data-stu-id="f75f5-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="f75f5-121">Dodatkowo wywołania interfejsu API, które skontaktowali się z bazą danych, byłyby niepowodzeniem, jeśli rozpoczęto transakcję na źródłowym połączeniu z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f75f5-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="f75f5-122">Ograniczenie tylko akceptowania zamkniętych połączeń zostało usunięte w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f75f5-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="f75f5-123">Aby uzyskać szczegółowe informacje, zobacz [Zarządzanie połączeniami](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="f75f5-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="f75f5-124">Począwszy od EF6, platforma udostępnia teraz:</span><span class="sxs-lookup"><span data-stu-id="f75f5-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="f75f5-125">**Database. BeginTransaction ()** : łatwiejsza metoda umożliwiająca użytkownikowi uruchamianie i Kończenie transakcji w ramach istniejącego DbContext — pozwala to na łączenie kilku operacji w ramach tej samej transakcji, a więc wszystkie zatwierdzone lub wszystkie wycofane jako jeden.</span><span class="sxs-lookup"><span data-stu-id="f75f5-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="f75f5-126">Umożliwia także użytkownikowi łatwiejsze Określanie poziomu izolacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="f75f5-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="f75f5-127">**Database. UseTransaction ()** : który umożliwia DbContext używanie transakcji, która została uruchomiona poza Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f75f5-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="f75f5-128">Łączenie kilku operacji w jedną transakcję w tym samym kontekście</span><span class="sxs-lookup"><span data-stu-id="f75f5-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="f75f5-129">**Database. BeginTransaction ()** ma dwa przesłonięcia — jeden, który pobiera jawne [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) i jeden nie przyjmuje argumentów i używa domyślnego IsolationLevel z dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f75f5-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="f75f5-130">Oba zastąpienia zwracają obiekt **DbContextTransaction** , który dostarcza metody **commit ()** i **Rollback ()** , które wykonują zatwierdzenie i wycofywanie w źródłowej transakcji magazynu.</span><span class="sxs-lookup"><span data-stu-id="f75f5-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="f75f5-131">**DbContextTransaction** jest przeznaczony do usunięcia, gdy zostanie on zatwierdzony lub wycofany.</span><span class="sxs-lookup"><span data-stu-id="f75f5-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="f75f5-132">Jednym z łatwych sposobów osiągnięcia tego jest **użycie (...) {.** ..}</span><span class="sxs-lookup"><span data-stu-id="f75f5-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="f75f5-133">Składnia, która automatycznie wywoła metodę **Dispose ()** , gdy zostanie ukończony blok using:</span><span class="sxs-lookup"><span data-stu-id="f75f5-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
> <span data-ttu-id="f75f5-134">Rozpoczęcie transakcji wymaga otwarcia podstawowego połączenia z magazynem.</span><span class="sxs-lookup"><span data-stu-id="f75f5-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="f75f5-135">Wywołanie metody Database. BeginTransaction () spowoduje otwarcie połączenia, jeśli nie zostało ono jeszcze otwarte.</span><span class="sxs-lookup"><span data-stu-id="f75f5-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="f75f5-136">Jeśli DbContextTransaction otwarto połączenie, zostanie ono zamknięte po wywołaniu metody Dispose ().</span><span class="sxs-lookup"><span data-stu-id="f75f5-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="f75f5-137">Przekazywanie istniejącej transakcji do kontekstu</span><span class="sxs-lookup"><span data-stu-id="f75f5-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="f75f5-138">Czasami chcesz, aby transakcja była jeszcze szerszym zakresem i która obejmuje operacje w tej samej bazie danych, ale poza całością.</span><span class="sxs-lookup"><span data-stu-id="f75f5-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="f75f5-139">Aby to osiągnąć, należy otworzyć połączenie i uruchomić transakcję samodzielnie, a następnie poinformować Dr a), aby użyć już otwartego połączenia bazy danych i b) w celu użycia istniejącej transakcji dla tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="f75f5-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="f75f5-140">Aby to zrobić, należy zdefiniować i użyć konstruktora w klasie kontekstu, która dziedziczy z jednego z konstruktorów DbContext, które przyjmują i) istniejący parametr połączenia i II) wartość logiczna contextOwnsConnection.</span><span class="sxs-lookup"><span data-stu-id="f75f5-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="f75f5-141">Flaga contextOwnsConnection musi być ustawiona na wartość false, gdy zostanie wywołana w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="f75f5-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="f75f5-142">Jest to ważne, ponieważ informuje Entity Framework, że nie powinna zamykać połączenia, gdy jest ono wykonywane (na przykład wiersz 4 poniżej):</span><span class="sxs-lookup"><span data-stu-id="f75f5-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="f75f5-143">Ponadto musisz samodzielnie uruchomić transakcję (w tym IsolationLevel, jeśli chcesz uniknąć domyślnego ustawienia) i powiadom Entity Framework, że istnieje już istniejąca transakcja w ramach połączenia (Zobacz wiersz 33 poniżej).</span><span class="sxs-lookup"><span data-stu-id="f75f5-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="f75f5-144">Następnie można wykonywać operacje bazy danych bezpośrednio z elementu SqlConnection lub w kontekście DbContext.</span><span class="sxs-lookup"><span data-stu-id="f75f5-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="f75f5-145">Wszystkie takie operacje są wykonywane w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="f75f5-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="f75f5-146">Użytkownik zobowiązuje się do zatwierdzania lub wycofywania transakcji oraz do wywoływania operacji Dispose (), a także do zamykania i usuwania połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f75f5-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="f75f5-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f75f5-147">For example:</span></span>  

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

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="f75f5-148">Czyszczenie transakcji</span><span class="sxs-lookup"><span data-stu-id="f75f5-148">Clearing up the transaction</span></span>

<span data-ttu-id="f75f5-149">Można przekazać wartość null do bazy danych. UseTransaction () w celu wyczyszczenia informacji o bieżącej transakcji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f75f5-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="f75f5-150">W takim przypadku Entity Framework nie zatwierdzi ani nie wycofał istniejącej transakcji, dlatego należy używać jej z opieką i tylko wtedy, gdy masz pewność, co chcesz zrobić.</span><span class="sxs-lookup"><span data-stu-id="f75f5-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="f75f5-151">Błędy w UseTransaction</span><span class="sxs-lookup"><span data-stu-id="f75f5-151">Errors in UseTransaction</span></span>

<span data-ttu-id="f75f5-152">Zostanie wyświetlony wyjątek z bazy danych. UseTransaction (), jeśli przejdziesz do transakcji, gdy:</span><span class="sxs-lookup"><span data-stu-id="f75f5-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="f75f5-153">Entity Framework ma już istniejącą transakcję</span><span class="sxs-lookup"><span data-stu-id="f75f5-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="f75f5-154">Entity Framework działa już w ramach elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f75f5-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="f75f5-155">Obiekt Connection w przekazaniu transakcji ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="f75f5-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="f75f5-156">Oznacza to, że transakcja nie jest skojarzona z połączeniem — zwykle jest to znak, który transakcja została już ukończona.</span><span class="sxs-lookup"><span data-stu-id="f75f5-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="f75f5-157">Obiekt połączenia w przekazaniu transakcji nie jest zgodny z połączeniem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f75f5-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="f75f5-158">Używanie transakcji z innymi funkcjami</span><span class="sxs-lookup"><span data-stu-id="f75f5-158">Using transactions with other features</span></span>  

<span data-ttu-id="f75f5-159">W tej sekcji szczegółowo opisano sposób działania powyższych transakcji:</span><span class="sxs-lookup"><span data-stu-id="f75f5-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="f75f5-160">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="f75f5-160">Connection resiliency</span></span>  
- <span data-ttu-id="f75f5-161">Metody asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="f75f5-161">Asynchronous methods</span></span>  
- <span data-ttu-id="f75f5-162">Transakcje elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f75f5-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="f75f5-163">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="f75f5-163">Connection Resiliency</span></span>  

<span data-ttu-id="f75f5-164">Nowa funkcja odporności połączenia nie działa z zainicjowanymi przez użytkownika transakcjami.</span><span class="sxs-lookup"><span data-stu-id="f75f5-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="f75f5-165">Aby uzyskać szczegółowe informacje, zobacz [ponawianie strategii wykonywania](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="f75f5-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="f75f5-166">Programowanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="f75f5-166">Asynchronous Programming</span></span>  

<span data-ttu-id="f75f5-167">Podejście opisane w poprzednich sekcjach nie wymaga żadnych dalszych opcji ani ustawień do pracy z [zapytań asynchronicznych i zapisywania metod](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="f75f5-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="f75f5-168">Należy jednak pamiętać, że w zależności od tego, co robisz w metodach asynchronicznych, może to skutkować długotrwałymi transakcjami, co może spowodować zakleszczenie lub blokowanie, które są nieprawidłowe dla wydajności ogólnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f75f5-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="f75f5-169">Transakcje elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f75f5-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="f75f5-170">Przed EF6 zalecanym sposobem udostępniania większego zakresu transakcji było użycie obiektu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="f75f5-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="f75f5-171">Elementy SqlConnection i Entity Framework mogą używać otoczenia transakcji TransactionScope i dlatego być zaangażowane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="f75f5-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="f75f5-172">Począwszy od platformy .NET 4.5.1 element TransactionScope został zaktualizowany, aby działał również z metodami asynchronicznymi przy użyciu wyliczenia [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :</span><span class="sxs-lookup"><span data-stu-id="f75f5-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="f75f5-173">Nadal istnieją pewne ograniczenia dotyczące podejścia elementu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="f75f5-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="f75f5-174">Program wymaga programu .NET 4.5.1 lub nowszego do pracy z metodami asynchronicznymi.</span><span class="sxs-lookup"><span data-stu-id="f75f5-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="f75f5-175">Nie można jej używać w scenariuszach chmury, chyba że masz tylko jedno połączenie (scenariusze chmury nie obsługują transakcji rozproszonych).</span><span class="sxs-lookup"><span data-stu-id="f75f5-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="f75f5-176">Nie można go połączyć z podejściem Database. UseTransaction () w poprzednich sekcjach.</span><span class="sxs-lookup"><span data-stu-id="f75f5-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="f75f5-177">Spowoduje to wygenerowanie wyjątków w przypadku wystawiania jakichkolwiek DDL i niewłączonych transakcji rozproszonych za pomocą usługi MSDTC.</span><span class="sxs-lookup"><span data-stu-id="f75f5-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="f75f5-178">Zalety podejścia elementu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="f75f5-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="f75f5-179">W przypadku wprowadzenia więcej niż jednego połączenia do danej bazy danych lub połączenia z jedną bazą danych przy użyciu połączenia z inną bazą danych w ramach tej samej transakcji zostanie automatycznie uaktualniona transakcja lokalna do transakcji rozproszonej. usługa MSDTC skonfigurowana tak, aby zezwalała na wykonywanie transakcji rozproszonych.</span><span class="sxs-lookup"><span data-stu-id="f75f5-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="f75f5-180">Łatwość kodowania.</span><span class="sxs-lookup"><span data-stu-id="f75f5-180">Ease of coding.</span></span> <span data-ttu-id="f75f5-181">Jeśli wolisz, aby transakcja była otaczająca i niejawnie w tle, a nie w sposób jawny, to podejście TransactionScope może być bardziej odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="f75f5-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="f75f5-182">Podsumowując, z użyciem nowych interfejsów API Database. BeginTransaction () i Database. UseTransaction () powyżej, podejście TransactionScope nie jest już konieczne dla większości użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f75f5-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="f75f5-183">W przypadku kontynuowania korzystania z elementu TransactionScope należy pamiętać o powyższych ograniczeniach.</span><span class="sxs-lookup"><span data-stu-id="f75f5-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="f75f5-184">Zalecamy używanie podejścia przedstawionego w poprzednich sekcjach zamiast tego, gdzie to możliwe.</span><span class="sxs-lookup"><span data-stu-id="f75f5-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
