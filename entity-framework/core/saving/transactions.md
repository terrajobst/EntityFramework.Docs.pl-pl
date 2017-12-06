---
title: "Transakcje — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>Używanie transakcji

Transakcje zezwala na kilka operacji bazy danych mają być przetwarzane w sposób atomic. Transakcja została zatwierdzona, wszystkie operacje są pomyślnie zastosowana do bazy danych. Jeśli transakcja zostanie wycofana, żadna z operacji są stosowane do bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) w witrynie GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w jednym wywołaniu `SaveChanges()` są stosowane w ramach transakcji. W przypadku awarii zmian, transakcja zostanie wycofana i zmiany są stosowane do bazy danych. Oznacza to, że `SaveChanges()` jest gwarantowana całkowicie powiedzie się, lub pozostaw bazę danych, nie mają być modyfikowane w przypadku wystąpienia błędu.

W przypadku większości aplikacji to domyślne zachowanie jest wystarczająca. Transakcje powinien kontroli tylko ręcznie, jeśli wymagania aplikacji uznać za konieczne.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Można użyć `DbContext.Database` interfejsu API, aby rozpocząć, zatwierdzenia i wycofywania transakcji. Poniższy przykład przedstawia dwa `SaveChanges()` operacje i LINQ zapytania wykonywane w ramach jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługuje transakcji. Niektórzy dostawcy może zgłaszać lub pusta podczas transakcji są nazywane interfejsów API.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Między kontekstu transakcji (relacyjnych baz danych tylko)

Możesz również udostępniać w wielu wystąpieniach kontekstu transakcji. Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby udostępnić transakcji, kontekstów musi udostępniać zarówno `DbConnection` i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalaj na połączenia należy podać zewnętrznie

Udostępnianie `DbConnection` wymaga możliwości przekazania połączenie w kontekście podczas jego tworzenia.

Najprostszym sposobem, aby umożliwić `DbConnection` zewnętrznie zostać podany, jest zatrzymanie przy użyciu `DbContext.OnConfiguring` metody zewnętrznie utworzony i skonfigurowany kontekst `DbContextOptions` i przekazać je do konstruktora kontekstu.

> [!TIP]  
> `DbContextOptionsBuilder`jest używany w API `DbContext.OnConfiguring` można skonfigurować kontekstu, teraz będzie go zewnętrznie użyć do utworzenia `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

Alternatywą jest nadal używać `DbContext.OnConfiguring`, ale akceptują `DbConnection` które jest zapisywane i następnie używane w `DbContext.OnConfiguring`.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Połączenie udziału i transakcji

Teraz możesz utworzyć wiele wystąpień kontekstu, które współużytkują to samo połączenie. Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API można zarejestrować zarówno kontekstów w tej samej transakcji.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a>Za pomocą zewnętrznego DbTransactions (relacyjnych baz danych tylko)

Korzystając z wielu technologii dostępu do danych relacyjnej bazy danych, można udostępniać transakcji między operacje wykonywane przez te różnych technologii.

Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework podstawowej funkcjonalności w tej samej transakcji.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
