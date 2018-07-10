---
title: Zarządzanie połączeniami - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 361065def0e83a097d4bb0109d468983ce41cd86
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914047"
---
# <a name="connection-management"></a>Zarządzanie połączeniami
Na tej stronie opisano zachowanie programu Entity Framework w odniesieniu do przekazywania połączenia do kontekstu i funkcjonalność **Database.Connection.Open()** interfejsu API.  

## <a name="passing-connections-to-the-context"></a>Przekazywanie połączeń do kontekstu  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Zachowanie EF5 i wcześniejszymi wersjami  

Istnieją dwa konstruktory, które akceptują połączenia:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Istnieje możliwość, aby móc ich używać, ale trzeba pracować na kilka ograniczeń:  

1. W przypadku przekazania otwartego połączenia do jednego z tych następnie po raz pierwszy ramach podejmują próbę użycia go, który jest generowany, InvalidOperationException stwierdzającego nie można ponownie otworzyć już otwartego połączenia.  
2. Flaga contextOwnsConnection jest interpretowany oznacza informację określającą, czy połączenie podstawowe magazynu powinny zostać usunięte, jeśli kontekst zostanie usunięty. Jednak niezależnie od tego ustawienia połączenia magazynu jest zawsze zamknięte, jeśli kontekst zostanie usunięty. Dlatego jeśli masz więcej niż jednego typu DbContext w ramach tego samego niezależnie od kontekst zostanie usunięty, najpierw połączenie zostanie zamknięte (podobnie jeśli mają różne istniejącego połączenia ADO.NET z typu DbContext, DbContext zawsze zamknie połączenie po jego usunięciu) .  

Istnieje możliwość obejść pierwszy ograniczenie powyżej, przekazując zamkniętego połączenia i tylko wykonywanie kodu, który otwiera go po utworzeniu wszystkich kontekstach:  

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

Drugi ograniczenie po prostu oznacza, że musisz punktowanych — usuwanie wszystkich obiektów typu DbContext, dopóki nie będziesz gotowy dla połączenia zostanie zamknięty.  

### <a name="behavior-in-ef6-and-future-versions"></a>Zachowanie w przyszłych wersjach i EF6  

W przyszłych wersjach i EF6 kontekstu DbContext ma ten sam dwa konstruktory, ale nie wymaga już zamknięte połączenia przekazany do konstruktora po ich odebraniu. Dlatego teraz jest to możliwe:  

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

Flaga contextOwnsConnection teraz kontroluje również informację, czy połączenie jest zamknięte i usunięty po usunięciu kontekstu DbContext. Dlatego w powyższym przykładzie połączenie nie jest zamknięte w kontekście usunięty (wiersz 32), ponieważ miałoby to miejsce w poprzednich wersjach programu EF, ale raczej, jeśli jest to samo połączenie zostanie usunięty (wiersz 40).  

Oczywiście jest nadal możliwe dla kontekstu DbContext przejąć kontrolę nad połączenia (tylko zestaw contextOwnsConnection na wartość true, lub użyj jednego z innych konstruktory) Jeśli więc chcesz.  

> [!NOTE]
> Istnieje kilka dodatkowych kwestii dotyczących podczas korzystania z transakcji za pomocą tego nowego modelu. Aby uzyskać szczegółowe informacje, zobacz [Praca z transakcji](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Zachowanie EF5 i wcześniejszymi wersjami  

EF5 i wcześniejszych wersji jest to błąd, **ObjectContext.Connection.State** nie został zaktualizowany w celu odzwierciedlenia rzeczywisty stan bazowego połączenia magazynu. Na przykład, jeśli zostanie wykonane następujący kod należy mogą być zwracane stan **zamknięte** mimo, że w rzeczywistości bazowego przechowywania połączenia **Otwórz**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Oddzielnie Jeśli otworzysz połączenie z bazą danych, wywołując Database.Connection.Open() będą one otwarte do momentu przy następnym wykonania kwerendy lub wywołanie niczego, co wymaga połączenia z bazą danych (np. SaveChanges()) ale po czy bazowego przechowywania połączenia zostanie zamknięte. Kontekst będzie, a następnie ponownie otworzyć i ponownie zamknij połączenie, każdym razem, gdy inna operacja bazy danych jest wymagane:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>Zachowanie w przyszłych wersjach i EF6  

EF6 i przyszłych wersji iż podejmujemy podejście, jeśli kod wywołujący wybierze do otwierania połączenia przez kontekst wywołania. Database.Connection.Open(), a następnie go ma przyczyna to i platformę będzie założono, że chce kontrolować otwierające i zamykające połączenia i nie jest już połączenie zostanie zamknięte automatycznie.  

> [!NOTE]
> Może to prowadzić do połączenia, które są otwarte przez długi czas, więc używać ostrożnie.  

Zaktualizowaliśmy również kod tak, aby ObjectContext.Connection.State teraz śledzi informacje o stan połączenia podstawowej poprawnie.  

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
