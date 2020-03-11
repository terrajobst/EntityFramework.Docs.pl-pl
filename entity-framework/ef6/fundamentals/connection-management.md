---
title: Zarządzanie połączeniami — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417988"
---
# <a name="connection-management"></a><span data-ttu-id="98aef-102">Zarządzanie połączeniami</span><span class="sxs-lookup"><span data-stu-id="98aef-102">Connection management</span></span>
<span data-ttu-id="98aef-103">Na tej stronie opisano zachowanie Entity Framework w odniesieniu do przekazywania połączeń z kontekstem i funkcjonalnością interfejsu API **Database. Connection. Open ()** .</span><span class="sxs-lookup"><span data-stu-id="98aef-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="98aef-104">Przekazywanie połączeń do kontekstu</span><span class="sxs-lookup"><span data-stu-id="98aef-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="98aef-105">Zachowanie dla EF5 i starszych wersji</span><span class="sxs-lookup"><span data-stu-id="98aef-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="98aef-106">Istnieją dwa konstruktory, które akceptują połączenia:</span><span class="sxs-lookup"><span data-stu-id="98aef-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="98aef-107">Można z nich korzystać, ale musisz obejść kilka ograniczeń:</span><span class="sxs-lookup"><span data-stu-id="98aef-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="98aef-108">W przypadku przekazania otwartego połączenia z jedną z tych sytuacji, gdy platforma próbuje użyć jej po raz pierwszy, zostanie wygenerowane InvalidOperationException, co oznacza, że nie może ponownie otworzyć otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="98aef-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="98aef-109">Flaga contextOwnsConnection jest interpretowana tak, aby oznaczała, czy podstawowe połączenie z magazynem powinno zostać usunięte, gdy kontekst zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="98aef-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="98aef-110">Jednak niezależnie od tego ustawienia połączenie ze sklepem jest zawsze zamykane po usunięciu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="98aef-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="98aef-111">Tak więc jeśli masz więcej niż jeden DbContext z tym samym połączeniem, niezależnie od tego, że kontekst zostanie usunięty, zamknie połączenie (podobnie, jeśli połączysz istniejące połączenie ADO.NET z DbContext, DbContext zawsze zamknie połączenie, gdy zostanie usunięte) .</span><span class="sxs-lookup"><span data-stu-id="98aef-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="98aef-112">Istnieje możliwość obejścia pierwszego ograniczenia powyżej przez przekazanie zamkniętego połączenia i wykonanie tylko kodu, który otworzy go po utworzeniu wszystkich kontekstów:</span><span class="sxs-lookup"><span data-stu-id="98aef-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="98aef-113">Drugie ograniczenie oznacza jedynie, że należy zrezygnować z wyrzucenia dowolnego obiektu DbContext do momentu, aż będzie można zamknąć połączenie.</span><span class="sxs-lookup"><span data-stu-id="98aef-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="98aef-114">Zachowanie w EF6 i przyszłych wersjach</span><span class="sxs-lookup"><span data-stu-id="98aef-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="98aef-115">W EF6 i przyszłych wersjach, DbContext ma te same dwa konstruktory, ale nie wymaga już zamknięcia połączenia przenoszonego do konstruktora po odebraniu.</span><span class="sxs-lookup"><span data-stu-id="98aef-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="98aef-116">Jest to teraz możliwe:</span><span class="sxs-lookup"><span data-stu-id="98aef-116">So this is now possible:</span></span>  

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

<span data-ttu-id="98aef-117">Flaga contextOwnsConnection teraz kontroluje, czy połączenie jest zamknięte i usuwane po usunięciu kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="98aef-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="98aef-118">Dlatego w powyższym przykładzie połączenie nie jest zamykane, gdy kontekst zostanie usunięty (wiersz 32), tak jak w poprzednich wersjach EF, ale zamiast w przypadku usunięcia samego połączenia (wiersz 40).</span><span class="sxs-lookup"><span data-stu-id="98aef-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="98aef-119">Oczywiście jest nadal możliwe, aby w kontekście DbContext przejąć kontrolę nad połączeniem (po prostu ustawić contextOwnsConnection na true lub użyć jednego z innych konstruktorów), jeśli wolisz.</span><span class="sxs-lookup"><span data-stu-id="98aef-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="98aef-120">W przypadku korzystania z transakcji z nowym modelem należy wziąć pod uwagę pewne dodatkowe zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="98aef-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="98aef-121">Aby uzyskać szczegółowe informacje, zobacz [Praca z transakcjami](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="98aef-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="98aef-122">Database. Connection. Open ()</span><span class="sxs-lookup"><span data-stu-id="98aef-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="98aef-123">Zachowanie dla EF5 i starszych wersji</span><span class="sxs-lookup"><span data-stu-id="98aef-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="98aef-124">W programie EF5 i starszych wersjach występuje usterka taka, że obiekt **ObjectContext. Connection. State** nie został zaktualizowany w celu odzwierciedlenia stanu rzeczywistego połączenia z magazynem.</span><span class="sxs-lookup"><span data-stu-id="98aef-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="98aef-125">Na przykład, jeśli został wykonany następujący kod, można zwrócić stan **zamknięte** , nawet jeśli w rzeczywistości jest **otwarte**podstawowe połączenie z magazynem.</span><span class="sxs-lookup"><span data-stu-id="98aef-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="98aef-126">Niezależnie od tego, czy połączenie z bazą danych zostanie otwarte przez wywołanie metody Database. Connection. Open (), zostanie otwarte do momentu następnego wykonania zapytania lub wywołania dowolnego, które wymaga połączenia z bazą danych (na przykład metody SaveChanges ()), ale po tym źródłowym magazynie połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="98aef-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="98aef-127">Kontekst zostanie następnie ponownie otwarty i ponownie zamknięty przy każdej operacji na innej bazie danych:</span><span class="sxs-lookup"><span data-stu-id="98aef-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="98aef-128">Zachowanie w EF6 i przyszłych wersjach</span><span class="sxs-lookup"><span data-stu-id="98aef-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="98aef-129">W przypadku EF6 i przyszłych wersji wprowadziliśmy podejście, które w przypadku, gdy wywołujący kod wybiera, aby otworzyć połączenie przez wywołanie kontekstu. Plik Database. Connection. Open () jest dobrym powodem do tego celu, a platforma zakłada, że chce ona kontrolować otwieranie i zamykanie połączenia i nie będzie już zamykać połączenia automatycznie.</span><span class="sxs-lookup"><span data-stu-id="98aef-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="98aef-130">Może to potencjalnie prowadzić do połączeń, które są otwarte przez długi czas, dlatego należy korzystać z nich.</span><span class="sxs-lookup"><span data-stu-id="98aef-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="98aef-131">Zaktualizowaliśmy również kod, tak aby obiekt ObjectContext. Connection. State teraz śledził stan podstawowego połączenia poprawnie.</span><span class="sxs-lookup"><span data-stu-id="98aef-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
