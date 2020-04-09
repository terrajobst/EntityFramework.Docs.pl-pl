---
title: Transakcje - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417556"
---
# <a name="using-transactions"></a><span data-ttu-id="8db5d-102">Korzystanie z transakcji</span><span class="sxs-lookup"><span data-stu-id="8db5d-102">Using Transactions</span></span>

<span data-ttu-id="8db5d-103">Transakcje umożliwiają kilka operacji bazy danych do przetworzenia w sposób niepodzielny.</span><span class="sxs-lookup"><span data-stu-id="8db5d-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="8db5d-104">Jeśli transakcja zostanie zatwierdzona, wszystkie operacje są pomyślnie stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8db5d-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="8db5d-105">Jeśli transakcja zostanie wycofana, żadna z operacji nie są stosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8db5d-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="8db5d-106">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="8db5d-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="8db5d-107">Domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="8db5d-107">Default transaction behavior</span></span>

<span data-ttu-id="8db5d-108">Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie `SaveChanges()` zmiany w jednym wywołaniu są stosowane w transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="8db5d-109">Jeśli którakolwiek ze zmian nie powiedzie się, transakcja jest przywracana i żadna ze zmian nie jest stosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8db5d-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="8db5d-110">Oznacza to, że `SaveChanges()` jest gwarantowane albo całkowicie zakończyć się pomyślnie lub pozostawić bazy danych bez modyfikacji, jeśli wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="8db5d-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="8db5d-111">W przypadku większości aplikacji to domyślne zachowanie jest wystarczające.</span><span class="sxs-lookup"><span data-stu-id="8db5d-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="8db5d-112">Transakcje należy kontrolować ręcznie tylko wtedy, gdy wymagania aplikacji uznają to za konieczne.</span><span class="sxs-lookup"><span data-stu-id="8db5d-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="8db5d-113">Kontrola transakcji</span><span class="sxs-lookup"><span data-stu-id="8db5d-113">Controlling transactions</span></span>

<span data-ttu-id="8db5d-114">Za pomocą `DbContext.Database` interfejsu API można rozpoczynać, zatwierdzać i wycofywać transakcje.</span><span class="sxs-lookup"><span data-stu-id="8db5d-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="8db5d-115">W poniższym `SaveChanges()` przykładzie przedstawiono dwie operacje i kwerendę LINQ wykonywane w pojedynczej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="8db5d-116">Nie wszyscy dostawcy baz danych obsługują transakcje.</span><span class="sxs-lookup"><span data-stu-id="8db5d-116">Not all database providers support transactions.</span></span> <span data-ttu-id="8db5d-117">Niektórzy dostawcy mogą zgłaszać lub nie op, gdy są wywoływane interfejsy API transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="8db5d-118">Transakcja między kontekstem (tylko relacyjne bazy danych)</span><span class="sxs-lookup"><span data-stu-id="8db5d-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="8db5d-119">Można również udostępnić transakcję w wielu wystąpieniach kontekstu.</span><span class="sxs-lookup"><span data-stu-id="8db5d-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="8db5d-120">Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy `DbTransaction` `DbConnection`relacyjnej bazy danych, ponieważ wymaga użycia i , które są specyficzne dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="8db5d-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="8db5d-121">Aby udostępnić transakcję, konteksty `DbConnection` muszą `DbTransaction`współużytkować zarówno a, jak i .</span><span class="sxs-lookup"><span data-stu-id="8db5d-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="8db5d-122">Zezwalaj na podłączanie na zewnątrz</span><span class="sxs-lookup"><span data-stu-id="8db5d-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="8db5d-123">Udostępnianie `DbConnection` wymaga możliwość przekazania połączenia do kontekstu podczas konstruowania go.</span><span class="sxs-lookup"><span data-stu-id="8db5d-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="8db5d-124">Najprostszym sposobem, `DbConnection` aby umożliwić być zewnętrznie pod `DbContext.OnConfiguring` warunkiem, jest, aby zatrzymać `DbContextOptions` przy użyciu metody, aby skonfigurować kontekst i zewnętrznie utworzyć i przekazać je do konstruktora kontekstu.</span><span class="sxs-lookup"><span data-stu-id="8db5d-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="8db5d-125">`DbContextOptionsBuilder`to interfejs API `DbContext.OnConfiguring` użyty do skonfigurowania kontekstu, teraz będzie go `DbContextOptions`używać zewnętrznie do tworzenia .</span><span class="sxs-lookup"><span data-stu-id="8db5d-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="8db5d-126">Alternatywą jest, `DbContext.OnConfiguring`aby zachować `DbConnection` przy użyciu , `DbContext.OnConfiguring`ale zaakceptować, który jest zapisywany, a następnie używany w .</span><span class="sxs-lookup"><span data-stu-id="8db5d-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="8db5d-127">Udostępnij połączenie i transakcję</span><span class="sxs-lookup"><span data-stu-id="8db5d-127">Share connection and transaction</span></span>

<span data-ttu-id="8db5d-128">Teraz można utworzyć wiele wystąpień kontekstu, które współużytkuje to samo połączenie.</span><span class="sxs-lookup"><span data-stu-id="8db5d-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="8db5d-129">Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API, aby zarejestrować oba konteksty w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="8db5d-130">Korzystanie z zewnętrznych dbtransakcje (tylko relacyjne bazy danych)</span><span class="sxs-lookup"><span data-stu-id="8db5d-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="8db5d-131">Jeśli używasz wielu technologii dostępu do danych, aby uzyskać dostęp do relacyjnej bazy danych, możesz chcieć udostępnić transakcję między operacjami wykonywanymi przez te różne technologie.</span><span class="sxs-lookup"><span data-stu-id="8db5d-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="8db5d-132">W poniższym przykładzie pokazano, jak wykonać ADO.NET operacji SqlClient i operacji Core programu Entity Framework w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="8db5d-133">Korzystanie z system.transactions</span><span class="sxs-lookup"><span data-stu-id="8db5d-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="8db5d-134">Ta funkcja jest nowa w EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8db5d-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="8db5d-135">Istnieje możliwość użycia transakcji otoczenia, jeśli trzeba koordynować w większym zakresie.</span><span class="sxs-lookup"><span data-stu-id="8db5d-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="8db5d-136">Istnieje również możliwość zarejestrowania się w jawnej transakcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="8db5d-137">Ograniczenia systemowych.Transakcji</span><span class="sxs-lookup"><span data-stu-id="8db5d-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="8db5d-138">EF Core opiera się na dostawców bazy danych do wdrożenia obsługi system.Transactions.</span><span class="sxs-lookup"><span data-stu-id="8db5d-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="8db5d-139">Mimo że obsługa jest dość powszechne wśród dostawców ADO.NET dla platformy .NET Framework, interfejs API został niedawno dodany do platformy .NET Core i dlatego obsługa nie jest tak powszechna.</span><span class="sxs-lookup"><span data-stu-id="8db5d-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="8db5d-140">Jeśli dostawca nie implementuje obsługi system.Transactions, jest możliwe, że wywołania tych interfejsów API zostaną całkowicie zignorowane.</span><span class="sxs-lookup"><span data-stu-id="8db5d-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="8db5d-141">SqlClient for .NET Core obsługuje go od 2.1.</span><span class="sxs-lookup"><span data-stu-id="8db5d-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="8db5d-142">SqlClient dla .NET Core 2.0 zda wyjątek, jeśli spróbujesz użyć tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="8db5d-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="8db5d-143">Zaleca się, aby przetestować, że interfejs API zachowuje się poprawnie z dostawcą, zanim będzie polegać na nim do zarządzania transakcjami.</span><span class="sxs-lookup"><span data-stu-id="8db5d-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="8db5d-144">Zachęcamy do skontaktowania się z opiekunem dostawcy bazy danych, jeśli tak nie jest.</span><span class="sxs-lookup"><span data-stu-id="8db5d-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="8db5d-145">W wersji 2.1 implementacja System.Transactions w .NET Core nie obejmuje obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` ani `CommittableTransaction` koordynować transakcji między wieloma menedżerami zasobów.</span><span class="sxs-lookup"><span data-stu-id="8db5d-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
