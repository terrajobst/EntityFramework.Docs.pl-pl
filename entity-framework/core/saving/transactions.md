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
# <a name="using-transactions"></a><span data-ttu-id="c2e9e-102">Używanie transakcji</span><span class="sxs-lookup"><span data-stu-id="c2e9e-102">Using Transactions</span></span>

<span data-ttu-id="c2e9e-103">Transakcje zezwala na kilka operacji bazy danych mają być przetwarzane w sposób atomic.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="c2e9e-104">Transakcja została zatwierdzona, wszystkie operacje są pomyślnie zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="c2e9e-105">Jeśli transakcja zostanie wycofana, żadna z operacji są stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="c2e9e-106">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="c2e9e-107">Domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="c2e9e-107">Default transaction behavior</span></span>

<span data-ttu-id="c2e9e-108">Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w jednym wywołaniu `SaveChanges()` są stosowane w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="c2e9e-109">W przypadku awarii zmian, transakcja zostanie wycofana i zmiany są stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="c2e9e-110">Oznacza to, że `SaveChanges()` jest gwarantowana całkowicie powiedzie się, lub pozostaw bazę danych, nie mają być modyfikowane w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="c2e9e-111">W przypadku większości aplikacji to domyślne zachowanie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="c2e9e-112">Transakcje powinien kontroli tylko ręcznie, jeśli wymagania aplikacji uznać za konieczne.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="c2e9e-113">Kontrolowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="c2e9e-113">Controlling transactions</span></span>

<span data-ttu-id="c2e9e-114">Można użyć `DbContext.Database` interfejsu API, aby rozpocząć, zatwierdzenia i wycofywania transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="c2e9e-115">Poniższy przykład przedstawia dwa `SaveChanges()` operacje i LINQ zapytania wykonywane w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="c2e9e-116">Nie wszyscy dostawcy bazy danych obsługuje transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-116">Not all database providers support transactions.</span></span> <span data-ttu-id="c2e9e-117">Niektórzy dostawcy może zgłaszać lub pusta podczas transakcji są nazywane interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="c2e9e-118">Między kontekstu transakcji (relacyjnych baz danych tylko)</span><span class="sxs-lookup"><span data-stu-id="c2e9e-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="c2e9e-119">Możesz również udostępniać w wielu wystąpieniach kontekstu transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="c2e9e-120">Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="c2e9e-121">Aby udostępnić transakcji, kontekstów musi udostępniać zarówno `DbConnection` i `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="c2e9e-122">Zezwalaj na połączenia należy podać zewnętrznie</span><span class="sxs-lookup"><span data-stu-id="c2e9e-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="c2e9e-123">Udostępnianie `DbConnection` wymaga możliwości przekazania połączenie w kontekście podczas jego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="c2e9e-124">Najprostszym sposobem, aby umożliwić `DbConnection` zewnętrznie zostać podany, jest zatrzymanie przy użyciu `DbContext.OnConfiguring` metody zewnętrznie utworzony i skonfigurowany kontekst `DbContextOptions` i przekazać je do konstruktora kontekstu.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="c2e9e-125">`DbContextOptionsBuilder`jest używany w API `DbContext.OnConfiguring` można skonfigurować kontekstu, teraz będzie go zewnętrznie użyć do utworzenia `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

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

<span data-ttu-id="c2e9e-126">Alternatywą jest nadal używać `DbContext.OnConfiguring`, ale akceptują `DbConnection` które jest zapisywane i następnie używane w `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="c2e9e-127">Połączenie udziału i transakcji</span><span class="sxs-lookup"><span data-stu-id="c2e9e-127">Share connection and transaction</span></span>

<span data-ttu-id="c2e9e-128">Teraz możesz utworzyć wiele wystąpień kontekstu, które współużytkują to samo połączenie.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="c2e9e-129">Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API można zarejestrować zarówno kontekstów w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

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

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="c2e9e-130">Za pomocą zewnętrznego DbTransactions (relacyjnych baz danych tylko)</span><span class="sxs-lookup"><span data-stu-id="c2e9e-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="c2e9e-131">Korzystając z wielu technologii dostępu do danych relacyjnej bazy danych, można udostępniać transakcji między operacje wykonywane przez te różnych technologii.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="c2e9e-132">Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework podstawowej funkcjonalności w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="c2e9e-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

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
