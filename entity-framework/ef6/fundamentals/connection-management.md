---
title: Zarządzanie połączeniami - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: dc405e1876edc850ae6e4ce4649da52635d316ae
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997908"
---
# <a name="connection-management"></a><span data-ttu-id="574fd-102">Zarządzanie połączeniami</span><span class="sxs-lookup"><span data-stu-id="574fd-102">Connection management</span></span>
<span data-ttu-id="574fd-103">Na tej stronie opisano zachowanie programu Entity Framework w odniesieniu do przekazywania połączenia do kontekstu i funkcjonalność **Database.Connection.Open()** interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="574fd-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="574fd-104">Przekazywanie połączeń do kontekstu</span><span class="sxs-lookup"><span data-stu-id="574fd-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="574fd-105">Zachowanie EF5 i wcześniejszymi wersjami</span><span class="sxs-lookup"><span data-stu-id="574fd-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="574fd-106">Istnieją dwa konstruktory, które akceptują połączenia:</span><span class="sxs-lookup"><span data-stu-id="574fd-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="574fd-107">Istnieje możliwość, aby móc ich używać, ale trzeba pracować na kilka ograniczeń:</span><span class="sxs-lookup"><span data-stu-id="574fd-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="574fd-108">W przypadku przekazania otwartego połączenia do jednego z tych następnie po raz pierwszy ramach podejmują próbę użycia go, który jest generowany, InvalidOperationException stwierdzającego nie można ponownie otworzyć już otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="574fd-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="574fd-109">Flaga contextOwnsConnection jest interpretowany oznacza informację określającą, czy połączenie podstawowe magazynu powinny zostać usunięte, jeśli kontekst zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="574fd-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="574fd-110">Jednak niezależnie od tego ustawienia połączenia magazynu jest zawsze zamknięte, jeśli kontekst zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="574fd-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="574fd-111">Dlatego jeśli masz więcej niż jednego typu DbContext w ramach tego samego niezależnie od kontekst zostanie usunięty, najpierw połączenie zostanie zamknięte (podobnie jeśli mają różne istniejącego połączenia ADO.NET z typu DbContext, DbContext zawsze zamknie połączenie po jego usunięciu) .</span><span class="sxs-lookup"><span data-stu-id="574fd-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="574fd-112">Istnieje możliwość obejść pierwszy ograniczenie powyżej, przekazując zamkniętego połączenia i tylko wykonywanie kodu, który otwiera go po utworzeniu wszystkich kontekstach:</span><span class="sxs-lookup"><span data-stu-id="574fd-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

<span data-ttu-id="574fd-113">Drugi ograniczenie po prostu oznacza, że musisz punktowanych — usuwanie wszystkich obiektów typu DbContext, dopóki nie będziesz gotowy dla połączenia zostanie zamknięty.</span><span class="sxs-lookup"><span data-stu-id="574fd-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="574fd-114">Zachowanie w przyszłych wersjach i EF6</span><span class="sxs-lookup"><span data-stu-id="574fd-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="574fd-115">W przyszłych wersjach i EF6 kontekstu DbContext ma ten sam dwa konstruktory, ale nie wymaga już zamknięte połączenia przekazany do konstruktora po ich odebraniu.</span><span class="sxs-lookup"><span data-stu-id="574fd-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="574fd-116">Dlatego teraz jest to możliwe:</span><span class="sxs-lookup"><span data-stu-id="574fd-116">So this is now possible:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

<span data-ttu-id="574fd-117">Flaga contextOwnsConnection teraz kontroluje również informację, czy połączenie jest zamknięte i usunięty po usunięciu kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="574fd-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="574fd-118">Dlatego w powyższym przykładzie połączenie nie jest zamknięte w kontekście usunięty (wiersz 32), ponieważ miałoby to miejsce w poprzednich wersjach programu EF, ale raczej, jeśli jest to samo połączenie zostanie usunięty (wiersz 40).</span><span class="sxs-lookup"><span data-stu-id="574fd-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="574fd-119">Oczywiście jest nadal możliwe dla kontekstu DbContext przejąć kontrolę nad połączenia (tylko zestaw contextOwnsConnection na wartość true, lub użyj jednego z innych konstruktory) Jeśli więc chcesz.</span><span class="sxs-lookup"><span data-stu-id="574fd-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="574fd-120">Istnieje kilka dodatkowych kwestii dotyczących podczas korzystania z transakcji za pomocą tego nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="574fd-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="574fd-121">Aby uzyskać szczegółowe informacje, zobacz [Praca z transakcji](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="574fd-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="574fd-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="574fd-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="574fd-123">Zachowanie EF5 i wcześniejszymi wersjami</span><span class="sxs-lookup"><span data-stu-id="574fd-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="574fd-124">EF5 i wcześniejszych wersji jest to błąd, **ObjectContext.Connection.State** nie został zaktualizowany w celu odzwierciedlenia rzeczywisty stan bazowego połączenia magazynu.</span><span class="sxs-lookup"><span data-stu-id="574fd-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="574fd-125">Na przykład, jeśli zostanie wykonane następujący kod należy mogą być zwracane stan **zamknięte** mimo, że w rzeczywistości bazowego przechowywania połączenia **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="574fd-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="574fd-126">Oddzielnie, jeśli otworzysz połączenie z bazą danych, wywołując Database.Connection.Open() będą one otwarte do momentu przy następnym wykonania kwerendy lub wywołanie niczego, co wymaga połączenia z bazą danych (na przykład SaveChanges()), ale po czy bazowego przechowywania połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="574fd-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="574fd-127">Kontekst będzie, a następnie ponownie otworzyć i ponownie zamknij połączenie, każdym razem, gdy inna operacja bazy danych jest wymagane:</span><span class="sxs-lookup"><span data-stu-id="574fd-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="574fd-128">Zachowanie w przyszłych wersjach i EF6</span><span class="sxs-lookup"><span data-stu-id="574fd-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="574fd-129">EF6 i przyszłych wersji iż podejmujemy podejście, jeśli kod wywołujący wybierze do otwierania połączenia przez kontekst wywołania. Database.Connection.Open(), a następnie go ma przyczyna to i platformę będzie założono, że chce kontrolować otwierające i zamykające połączenia i nie jest już połączenie zostanie zamknięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="574fd-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="574fd-130">Może to prowadzić do połączenia, które są otwarte przez długi czas, więc używać ostrożnie.</span><span class="sxs-lookup"><span data-stu-id="574fd-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="574fd-131">Zaktualizowaliśmy również kod tak, aby ObjectContext.Connection.State teraz śledzi informacje o stan połączenia podstawowej poprawnie.</span><span class="sxs-lookup"><span data-stu-id="574fd-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
