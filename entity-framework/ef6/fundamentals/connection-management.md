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
# <a name="connection-management"></a>Zarządzanie połączeniami
Na tej stronie opisano zachowanie Entity Framework w odniesieniu do przekazywania połączeń z kontekstem i funkcjonalnością interfejsu API **Database. Connection. Open ()** .  

## <a name="passing-connections-to-the-context"></a>Przekazywanie połączeń do kontekstu  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Zachowanie dla EF5 i starszych wersji  

Istnieją dwa konstruktory, które akceptują połączenia:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Można z nich korzystać, ale musisz obejść kilka ograniczeń:  

1. W przypadku przekazania otwartego połączenia z jedną z tych sytuacji, gdy platforma próbuje użyć jej po raz pierwszy, zostanie wygenerowane InvalidOperationException, co oznacza, że nie może ponownie otworzyć otwartego połączenia.  
2. Flaga contextOwnsConnection jest interpretowana tak, aby oznaczała, czy podstawowe połączenie z magazynem powinno zostać usunięte, gdy kontekst zostanie usunięty. Jednak niezależnie od tego ustawienia połączenie ze sklepem jest zawsze zamykane po usunięciu kontekstu. Tak więc jeśli masz więcej niż jeden DbContext z tym samym połączeniem, niezależnie od tego, że kontekst zostanie usunięty, zamknie połączenie (podobnie, jeśli połączysz istniejące połączenie ADO.NET z DbContext, DbContext zawsze zamknie połączenie, gdy zostanie usunięte) .  

Istnieje możliwość obejścia pierwszego ograniczenia powyżej przez przekazanie zamkniętego połączenia i wykonanie tylko kodu, który otworzy go po utworzeniu wszystkich kontekstów:  

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

Drugie ograniczenie oznacza jedynie, że należy zrezygnować z wyrzucenia dowolnego obiektu DbContext do momentu, aż będzie można zamknąć połączenie.  

### <a name="behavior-in-ef6-and-future-versions"></a>Zachowanie w EF6 i przyszłych wersjach  

W EF6 i przyszłych wersjach, DbContext ma te same dwa konstruktory, ale nie wymaga już zamknięcia połączenia przenoszonego do konstruktora po odebraniu. Jest to teraz możliwe:  

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

Flaga contextOwnsConnection teraz kontroluje, czy połączenie jest zamknięte i usuwane po usunięciu kontekstu DbContext. Dlatego w powyższym przykładzie połączenie nie jest zamykane, gdy kontekst zostanie usunięty (wiersz 32), tak jak w poprzednich wersjach EF, ale zamiast w przypadku usunięcia samego połączenia (wiersz 40).  

Oczywiście jest nadal możliwe, aby w kontekście DbContext przejąć kontrolę nad połączeniem (po prostu ustawić contextOwnsConnection na true lub użyć jednego z innych konstruktorów), jeśli wolisz.  

> [!NOTE]
> W przypadku korzystania z transakcji z nowym modelem należy wziąć pod uwagę pewne dodatkowe zagadnienia. Aby uzyskać szczegółowe informacje, zobacz [Praca z transakcjami](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Zachowanie dla EF5 i starszych wersji  

W programie EF5 i starszych wersjach występuje usterka taka, że obiekt **ObjectContext. Connection. State** nie został zaktualizowany w celu odzwierciedlenia stanu rzeczywistego połączenia z magazynem. Na przykład, jeśli został wykonany następujący kod, można zwrócić stan **zamknięte** , nawet jeśli w rzeczywistości jest **otwarte**podstawowe połączenie z magazynem.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Niezależnie od tego, czy połączenie z bazą danych zostanie otwarte przez wywołanie metody Database. Connection. Open (), zostanie otwarte do momentu następnego wykonania zapytania lub wywołania dowolnego, które wymaga połączenia z bazą danych (na przykład metody SaveChanges ()), ale po tym źródłowym magazynie połączenie zostanie zamknięte. Kontekst zostanie następnie ponownie otwarty i ponownie zamknięty przy każdej operacji na innej bazie danych:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>Zachowanie w EF6 i przyszłych wersjach  

W przypadku EF6 i przyszłych wersji wprowadziliśmy podejście, które w przypadku, gdy wywołujący kod wybiera, aby otworzyć połączenie przez wywołanie kontekstu. Plik Database. Connection. Open () jest dobrym powodem do tego celu, a platforma zakłada, że chce ona kontrolować otwieranie i zamykanie połączenia i nie będzie już zamykać połączenia automatycznie.  

> [!NOTE]
> Może to potencjalnie prowadzić do połączeń, które są otwarte przez długi czas, dlatego należy korzystać z nich.  

Zaktualizowaliśmy również kod, tak aby obiekt ObjectContext. Connection. State teraz śledził stan podstawowego połączenia poprawnie.  

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
