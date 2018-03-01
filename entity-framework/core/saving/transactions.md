---
title: "Transakcje — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="4412d-102">Używanie transakcji</span><span class="sxs-lookup"><span data-stu-id="4412d-102">Using Transactions</span></span>

<span data-ttu-id="4412d-103">Transakcje zezwala na kilka operacji bazy danych mają być przetwarzane w sposób atomic.</span><span class="sxs-lookup"><span data-stu-id="4412d-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="4412d-104">Transakcja została zatwierdzona, wszystkie operacje są pomyślnie zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4412d-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="4412d-105">Jeśli transakcja zostanie wycofana, żadna z operacji są stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4412d-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="4412d-106">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="4412d-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="4412d-107">Domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="4412d-107">Default transaction behavior</span></span>

<span data-ttu-id="4412d-108">Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w jednym wywołaniu `SaveChanges()` są stosowane w ramach transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="4412d-109">W przypadku awarii zmian, transakcja zostanie wycofana i zmiany są stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4412d-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="4412d-110">Oznacza to, że `SaveChanges()` jest gwarantowana całkowicie powiedzie się, lub pozostaw bazę danych, nie mają być modyfikowane w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="4412d-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="4412d-111">W przypadku większości aplikacji to domyślne zachowanie jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="4412d-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="4412d-112">Transakcje powinien kontroli tylko ręcznie, jeśli wymagania aplikacji uznać za konieczne.</span><span class="sxs-lookup"><span data-stu-id="4412d-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="4412d-113">Kontrolowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="4412d-113">Controlling transactions</span></span>

<span data-ttu-id="4412d-114">Można użyć `DbContext.Database` interfejsu API, aby rozpocząć, zatwierdzenia i wycofywania transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="4412d-115">Poniższy przykład przedstawia dwa `SaveChanges()` operacje i LINQ zapytania wykonywane w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="4412d-116">Nie wszyscy dostawcy bazy danych obsługuje transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-116">Not all database providers support transactions.</span></span> <span data-ttu-id="4412d-117">Niektórzy dostawcy może zgłaszać lub pusta podczas transakcji są nazywane interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="4412d-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="4412d-118">Między kontekstu transakcji (relacyjnych baz danych tylko)</span><span class="sxs-lookup"><span data-stu-id="4412d-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="4412d-119">Możesz również udostępniać w wielu wystąpieniach kontekstu transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="4412d-120">Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="4412d-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="4412d-121">Aby udostępnić transakcji, kontekstów musi udostępniać zarówno `DbConnection` i `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="4412d-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="4412d-122">Zezwalaj na połączenia należy podać zewnętrznie</span><span class="sxs-lookup"><span data-stu-id="4412d-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="4412d-123">Udostępnianie `DbConnection` wymaga możliwości przekazania połączenie w kontekście podczas jego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="4412d-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="4412d-124">Najprostszym sposobem, aby umożliwić `DbConnection` zewnętrznie zostać podany, jest zatrzymanie przy użyciu `DbContext.OnConfiguring` metody zewnętrznie utworzony i skonfigurowany kontekst `DbContextOptions` i przekazać je do konstruktora kontekstu.</span><span class="sxs-lookup"><span data-stu-id="4412d-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="4412d-125">`DbContextOptionsBuilder` jest używany w API `DbContext.OnConfiguring` można skonfigurować kontekstu, teraz będzie go zewnętrznie użyć do utworzenia `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4412d-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="4412d-126">Alternatywą jest nadal używać `DbContext.OnConfiguring`, ale akceptują `DbConnection` które jest zapisywane i następnie używane w `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="4412d-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="4412d-127">Połączenie udziału i transakcji</span><span class="sxs-lookup"><span data-stu-id="4412d-127">Share connection and transaction</span></span>

<span data-ttu-id="4412d-128">Teraz możesz utworzyć wiele wystąpień kontekstu, które współużytkują to samo połączenie.</span><span class="sxs-lookup"><span data-stu-id="4412d-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="4412d-129">Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API można zarejestrować zarówno kontekstów w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="4412d-130">Za pomocą zewnętrznego DbTransactions (relacyjnych baz danych tylko)</span><span class="sxs-lookup"><span data-stu-id="4412d-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="4412d-131">Korzystając z wielu technologii dostępu do danych relacyjnej bazy danych, można udostępniać transakcji między operacje wykonywane przez te różnych technologii.</span><span class="sxs-lookup"><span data-stu-id="4412d-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="4412d-132">Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework podstawowej funkcjonalności w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="4412d-133">Przy użyciu System.Transactions</span><span class="sxs-lookup"><span data-stu-id="4412d-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="4412d-134">Ta funkcja jest nowa w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4412d-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="4412d-135">Istnieje możliwość użycia otoczenia transakcji, aby koordynować w większego zakresu.</span><span class="sxs-lookup"><span data-stu-id="4412d-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="4412d-136">Istnieje również możliwość można zarejestrować w jawnych transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="4412d-137">Ograniczenia obszaru nazw System.Transactions</span><span class="sxs-lookup"><span data-stu-id="4412d-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="4412d-138">Podstawowe EF zależy od dostawcy bazy danych, obsługa System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="4412d-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="4412d-139">Mimo że obsługa jest dość często wśród dostawców ADO.NET dla .NET Framework, interfejsu API tylko został ostatnio dodany do platformy .NET Core i dlatego obsługi nie jest tak szerokie.</span><span class="sxs-lookup"><span data-stu-id="4412d-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="4412d-140">Jeśli dostawca nie implementuje obsługę System.Transactions, istnieje możliwość, że wywołań do tych interfejsów API będą ignorowane całkowicie.</span><span class="sxs-lookup"><span data-stu-id="4412d-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="4412d-141">Klient SQL dla platformy .NET Core obsługuje z 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="4412d-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="4412d-142">SqlClient programu .NET Core 2.0 spowoduje zgłoszenie wyjątku z próby użycia funkcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="4412d-143">Zaleca się przetestowanie czy interfejsu API poprawne działanie u dostawcy przed polegać na niej zarządzania transakcji.</span><span class="sxs-lookup"><span data-stu-id="4412d-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="4412d-144">Zachęcamy do kontaktowania się z Element utrzymujący dostawcy bazy danych, jeśli jej nie ma.</span><span class="sxs-lookup"><span data-stu-id="4412d-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="4412d-145">Począwszy od wersji 2.1 implementacja System.Transactions w .NET Core nie ma obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` lub `CommitableTransaction`aby koordynować transakcje w wielu menedżerów zasobów.</span><span class="sxs-lookup"><span data-stu-id="4412d-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction`to coordinate transactions across multiple resource managers.</span></span> 
