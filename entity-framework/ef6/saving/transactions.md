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
# <a name="working-with-transactions"></a><span data-ttu-id="9afc0-102">Praca z transakcji</span><span class="sxs-lookup"><span data-stu-id="9afc0-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="9afc0-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9afc0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9afc0-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="9afc0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="9afc0-105">W tym dokumencie opisano, za pomocą transakcji w EF6, w tym rozszerzeń, które dodaliśmy od EF5, aby ułatwić pracę z transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="9afc0-106">Domyślne działanie EF</span><span class="sxs-lookup"><span data-stu-id="9afc0-106">What EF does by default</span></span>  

<span data-ttu-id="9afc0-107">We wszystkich wersjach programu Entity Framework, każde wykonanie **SaveChanges()** do wstawiania, aktualizacji lub Usuń w bazie danych w ramach będzie zawijany w tej operacji w transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="9afc0-108">Ta transakcja obowiązuje tylko wystarczająco długi, aby wykonać operację, a następnie kończy.</span><span class="sxs-lookup"><span data-stu-id="9afc0-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="9afc0-109">Podczas wykonywania innej takich operacji nowa transakcja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="9afc0-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="9afc0-110">Począwszy od platformy EF6 **Database.ExecuteSqlCommand()** domyślnie będzie zawijany polecenia w transakcji, jeśli odpowiedni klucz nie został już istnieje.</span><span class="sxs-lookup"><span data-stu-id="9afc0-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="9afc0-111">Istnieją przeciążenia tej metody, które pozwalają na zastąpienie tego zachowania, jeśli chcesz, aby.</span><span class="sxs-lookup"><span data-stu-id="9afc0-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="9afc0-112">Również w EF6 wykonywania procedur przechowywanych, które są uwzględnione w modelu za pośrednictwem interfejsów API, takich jak **ObjectContext.ExecuteFunction()** działa tak samo (z tą różnicą, że zachowanie domyślne w tej chwili przesłanianie).</span><span class="sxs-lookup"><span data-stu-id="9afc0-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="9afc0-113">W obu przypadkach poziom izolacji transakcji jest poziom izolacji, niezależnie od dostawcy bazy danych uwzględnia ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="9afc0-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="9afc0-114">Domyślnie na przykład w programie SQL Server to READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="9afc0-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="9afc0-115">Entity Framework nie jest zawijany zapytań w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="9afc0-116">Ta funkcja domyślne jest odpowiednie dla wielu użytkowników i jeśli istnieje więc nie trzeba wykonywać dodatkowe czynności w EF6; Wystarczy napisać odpowiedni kod, tak jak zawsze.</span><span class="sxs-lookup"><span data-stu-id="9afc0-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="9afc0-117">Jednak niektóre użytkownicy muszą mieć większą kontrolę nad ich transakcje — to zagadnienie opisano w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="9afc0-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="9afc0-118">Jak działają interfejsy API</span><span class="sxs-lookup"><span data-stu-id="9afc0-118">How the APIs work</span></span>  

<span data-ttu-id="9afc0-119">Przed EF6 Entity Framework nalegała na otwieranie połączenia z bazą danych sam (spowodował on wyjątek została przekazana połączenia, który był wcześniej otwarty).</span><span class="sxs-lookup"><span data-stu-id="9afc0-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="9afc0-120">Ponieważ transakcji można uruchomić tylko na otwartego połączenia, oznacza to, jedynym sposobem użytkownika można opakować kilka operacji jako jedna transakcja została programu [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) lub użyj  **ObjectContext.Connection** właściwości i wywoływania start **Open()** i **BeginTransaction()** bezpośrednio na zwracanego **EntityConnection** obiekt.</span><span class="sxs-lookup"><span data-stu-id="9afc0-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="9afc0-121">Dodatkowo wywołania interfejsu API, które skontaktować się z bazy danych zakończy się niepowodzeniem, jeśli transakcji zostało uruchomione na podstawowej połączenia z bazą danych na własną rękę.</span><span class="sxs-lookup"><span data-stu-id="9afc0-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="9afc0-122">Ograniczenie tylko przyjmować połączenia zamknięte został usunięty w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9afc0-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="9afc0-123">Aby uzyskać więcej informacji, zobacz [zarządzania połączeniami](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="9afc0-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="9afc0-124">Począwszy od platformy EF6 ramach teraz zawiera:</span><span class="sxs-lookup"><span data-stu-id="9afc0-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="9afc0-125">**Database.BeginTransaction()** : łatwiejszy dla użytkownika uruchomić i realizowania transakcji siebie w ramach istniejącego typu DbContext — co kilka operacji, które można połączyć w ramach tej samej transakcji i dlatego wszystkie zatwierdzone lub wszystkie wycofane jako jeden.</span><span class="sxs-lookup"><span data-stu-id="9afc0-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="9afc0-126">Umożliwia użytkownikowi łatwiej określić poziom izolacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="9afc0-127">**Database.UseTransaction()** : pozwalający DbContext używać transakcji, która została uruchomiona poza programem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9afc0-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="9afc0-128">Łącząc kilka operacji w jednej transakcji, w tym samym kontekście</span><span class="sxs-lookup"><span data-stu-id="9afc0-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="9afc0-129">**Database.BeginTransaction()** ma dwa zastąpienia — taki, który przyjmuje jawnego [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) i jedną, która nie przyjmuje żadnych argumentów i używa domyślnego IsolationLevel od podstawowego dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9afc0-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="9afc0-130">Zwraca zarówno zastąpienia **DbContextTransaction** obiektu, który zapewnia **Commit()** i **Rollback()** metod, których wykonywanie zatwierdzenia i wycofywania na odpowiedni magazyn transakcja.</span><span class="sxs-lookup"><span data-stu-id="9afc0-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="9afc0-131">**DbContextTransaction** ma być usuwane, gdy została przekazana lub wycofana.</span><span class="sxs-lookup"><span data-stu-id="9afc0-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="9afc0-132">Jeden łatwy sposób osiągnąć ten cel jest **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="9afc0-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="9afc0-133">Składnia, która będzie automatycznie wywoływać **Dispose()** przypadku za pomocą bloku kończy:</span><span class="sxs-lookup"><span data-stu-id="9afc0-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
> <span data-ttu-id="9afc0-134">Rozpoczęcia transakcji wymaga, że połączenie podstawowe magazynu jest otwarty.</span><span class="sxs-lookup"><span data-stu-id="9afc0-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="9afc0-135">Dlatego wywołanie Database.BeginTransaction() spowoduje otwarcie połączenia, jeśli nie jest już otwarty.</span><span class="sxs-lookup"><span data-stu-id="9afc0-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="9afc0-136">Jeśli DbContextTransaction otwarte połączenie następnie zostaną zamknięte go po wywołaniu metody Dispose().</span><span class="sxs-lookup"><span data-stu-id="9afc0-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="9afc0-137">Przekazywanie istniejącej transakcji do kontekstu</span><span class="sxs-lookup"><span data-stu-id="9afc0-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="9afc0-138">Czasami chcesz transakcji jest szersze w zakresie i który obejmuje operacje, w tej samej bazy danych, ale poza programem EF całkowicie.</span><span class="sxs-lookup"><span data-stu-id="9afc0-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="9afc0-139">W tym celu należy otworzyć połączenie i rozpocząć transakcję, samodzielnie i następnie poinformuj EF) do użycia już otwarte połączenia bazy danych i (b) korzystania z istniejącej transakcji dla tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9afc0-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="9afc0-140">W tym celu należy zdefiniować i użyć konstruktora w swojej klasy kontekstu, który dziedziczy z jednego typu DbContext konstruktorów przyjmujących i) istniejących parametrów połączenia i ii) contextOwnsConnection boolean.</span><span class="sxs-lookup"><span data-stu-id="9afc0-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="9afc0-141">Flaga contextOwnsConnection musi być równa FAŁSZ, gdy wywołuje się, w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="9afc0-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="9afc0-142">Jest to ważne, ponieważ informuje Entity Framework, go nie powinna zamykać połączenia po jej zakończeniu z nim (na przykład, zobacz wiersz do 4 opisane poniżej):</span><span class="sxs-lookup"><span data-stu-id="9afc0-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="9afc0-143">Ponadto należy rozpocząć transakcję, samodzielnie (łącznie IsolationLevel, jeśli chcesz uniknąć ustawienie domyślne) i umożliwić Entity Framework wiedzieć, że jest istniejącej transakcji, już uruchomiona w ramach połączenia (zobacz wiersz 33 poniżej).</span><span class="sxs-lookup"><span data-stu-id="9afc0-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="9afc0-144">To bezpłatna, można wykonać operacji bazy danych bezpośrednio na element SqlConnection, samego lub dla kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="9afc0-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="9afc0-145">Wszystkie operacje są wykonywane w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="9afc0-146">Możesz skorzystać z odpowiedzialności potwierdzenia lub wycofania transakcji i wywoływania metody Dispose() w nim, a także zamykania i usuwania połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="9afc0-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="9afc0-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9afc0-147">For example:</span></span>  

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

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="9afc0-148">Czyszczenie się na transakcję</span><span class="sxs-lookup"><span data-stu-id="9afc0-148">Clearing up the transaction</span></span>

<span data-ttu-id="9afc0-149">Można przekazać wartości null do Database.UseTransaction() wyczyść Entity Framework wiedzę na temat bieżącej transakcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="9afc0-150">Entity Framework będzie ani zatwierdzenia ani wycofać istniejącej transakcji, gdy to zrobisz, więc używać ostrożnie, i tylko wtedy, gdy wiesz, jest to, co chcesz zrobić.</span><span class="sxs-lookup"><span data-stu-id="9afc0-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="9afc0-151">Błędy w UseTransaction</span><span class="sxs-lookup"><span data-stu-id="9afc0-151">Errors in UseTransaction</span></span>

<span data-ttu-id="9afc0-152">Wyjątek z Database.UseTransaction() zostanie wyświetlony w przypadku przekazania transakcji po:</span><span class="sxs-lookup"><span data-stu-id="9afc0-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="9afc0-153">Entity Framework jest już w istniejącej transakcji</span><span class="sxs-lookup"><span data-stu-id="9afc0-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="9afc0-154">Entity Framework jest już działające w ramach elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9afc0-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="9afc0-155">Obiekt połączenia w ramach transakcji przekazywany jest pusty.</span><span class="sxs-lookup"><span data-stu-id="9afc0-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="9afc0-156">Oznacza to, że transakcja nie jest skojarzona z połączeniem — zazwyczaj jest to znak, że transakcja została już zakończona.</span><span class="sxs-lookup"><span data-stu-id="9afc0-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="9afc0-157">Obiekt połączenia w ramach transakcji przekazywany jest niezgodna z połączenia programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9afc0-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="9afc0-158">Za pomocą transakcji z innymi funkcjami</span><span class="sxs-lookup"><span data-stu-id="9afc0-158">Using transactions with other features</span></span>  

<span data-ttu-id="9afc0-159">Tej sekcji opisano szczegółowo sposób interakcji powyżej transakcji z:</span><span class="sxs-lookup"><span data-stu-id="9afc0-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="9afc0-160">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="9afc0-160">Connection resiliency</span></span>  
- <span data-ttu-id="9afc0-161">Metody asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9afc0-161">Asynchronous methods</span></span>  
- <span data-ttu-id="9afc0-162">Transakcje elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9afc0-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="9afc0-163">Elastyczność połączenia</span><span class="sxs-lookup"><span data-stu-id="9afc0-163">Connection Resiliency</span></span>  

<span data-ttu-id="9afc0-164">Nowej funkcji elastyczności połączenia nie działa z transakcjami zainicjowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9afc0-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="9afc0-165">Aby uzyskać więcej informacji, zobacz [ponawiania strategii wykonywania](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="9afc0-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="9afc0-166">Programowanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="9afc0-166">Asynchronous Programming</span></span>  

<span data-ttu-id="9afc0-167">Podejście opisane w poprzednich sekcjach musi nie dalsze opcje lub ustawienia do pracy z [asynchronicznego zapytania i Zapisz metody](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="9afc0-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="9afc0-168">Należy jednak pamiętać, że, w zależności od tego, czy w ramach metod asynchronicznych, może to spowodować długotrwałych transakcji — które z kolei może spowodować zakleszczenia lub blokowania, czyli negatywnie wpływać na wydajność aplikacji ogólnej.</span><span class="sxs-lookup"><span data-stu-id="9afc0-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="9afc0-169">Transakcje elementu TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9afc0-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="9afc0-170">Przed EF6 zalecaną metodą udostępniania większy zakres transakcji było jednoczesne używanie obiektu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="9afc0-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="9afc0-171">Element SqlConnection i Entity Framework zarówno używa otoczenia transakcji TransactionScope i dlatego można zatwierdzić ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9afc0-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="9afc0-172">Począwszy od .NET 4.5.1 TransactionScope została zaktualizowana w celu również działać przy użyciu metod asynchronicznych za pomocą użytkowania [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) wyliczenia:</span><span class="sxs-lookup"><span data-stu-id="9afc0-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="9afc0-173">Nadal istnieją pewne ograniczenia związane z podejścia TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="9afc0-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="9afc0-174">Wymaga programu .NET 4.5.1 lub nowszej, aby pracować z metod asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="9afc0-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="9afc0-175">Jeśli nie masz pewności, masz tylko jedno połączenie nie można używać w chmurze (cloud scenariuszy nie obsługują transakcje rozproszone).</span><span class="sxs-lookup"><span data-stu-id="9afc0-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="9afc0-176">Nie można łączyć z podejściem Database.UseTransaction() w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="9afc0-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="9afc0-177">Będzie ona zgłaszają wyjątki, jeśli wystawiać wszelkie DDL i nie włączono transakcji rozproszonych za pomocą usługi MSDTC.</span><span class="sxs-lookup"><span data-stu-id="9afc0-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="9afc0-178">Korzyści wynikające z podejścia TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="9afc0-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="9afc0-179">Zostanie automatycznie uaktualniona lokalnej transakcji do poziomu transakcji rozproszonej Jeśli więcej niż jedno połączenie do określonej bazy danych lub połączyć połączenie z jedną bazę danych z połączeniem z innej bazy danych w ramach tej samej transakcji (Uwaga: konieczne jest posiadanie Usługa MSDTC skonfigurowane i umożliwiają transakcji rozproszonych, aby to działało).</span><span class="sxs-lookup"><span data-stu-id="9afc0-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="9afc0-180">Łatwość programowania.</span><span class="sxs-lookup"><span data-stu-id="9afc0-180">Ease of coding.</span></span> <span data-ttu-id="9afc0-181">Jeśli użytkownik sobie tego życzy transakcji otoczenia i rozdanych z niejawnie w tle, a nie jawnie w obszarze kontrolować następnie podejście TransactionScope może własnych możesz lepiej.</span><span class="sxs-lookup"><span data-stu-id="9afc0-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="9afc0-182">Podsumowując, za pomocą nowych Database.BeginTransaction() i API Database.UseTransaction() powyżej podejście TransactionScope nie jest już konieczne dla większości użytkowników.</span><span class="sxs-lookup"><span data-stu-id="9afc0-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="9afc0-183">Jeśli będziesz nadal używać elementu TransactionScope następnie Pamiętaj o powyższe ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="9afc0-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="9afc0-184">Zaleca się przy użyciu metody opisane w poprzednich sekcjach zamiast tego, gdzie to możliwe.</span><span class="sxs-lookup"><span data-stu-id="9afc0-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
