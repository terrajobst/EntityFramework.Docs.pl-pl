---
title: Transakcje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417556"
---
# <a name="using-transactions"></a><span data-ttu-id="fe223-102">Korzystanie z transakcji</span><span class="sxs-lookup"><span data-stu-id="fe223-102">Using Transactions</span></span>

<span data-ttu-id="fe223-103">Transakcje pozwalają na wykonanie kilku operacji na bazie danych w sposób niepodzielny.</span><span class="sxs-lookup"><span data-stu-id="fe223-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="fe223-104">Jeżeli transakcja zostanie zatwierdzona, wszystkie operacje zostaną pomyślnie wykonane na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fe223-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="fe223-105">Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie wykonana na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fe223-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="fe223-106">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="fe223-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="fe223-107">Domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="fe223-107">Default transaction behavior</span></span>

<span data-ttu-id="fe223-108">Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w pojedynczym wywołaniu `SaveChanges()` są stosowane w transakcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="fe223-109">Jeżeli jakakolwiek z tych zmian nie powiedzie się, transakcja zostanie wycofana i zmiany nie zostaną zapisane.</span><span class="sxs-lookup"><span data-stu-id="fe223-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="fe223-110">Oznacza to, że `SaveChanges()` gwarantuje całkowite pomyślne lub pozostawienie niezmodyfikowanej bazy danych w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="fe223-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="fe223-111">W przypadku większości zastosowań to domyślne zachowanie jest wystarczające.</span><span class="sxs-lookup"><span data-stu-id="fe223-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="fe223-112">Transakcje powinny być sterowane ręcznie, tylko jeżeli taka konieczność wynika z wymagań danego zastosowania.</span><span class="sxs-lookup"><span data-stu-id="fe223-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="fe223-113">Kontrolowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="fe223-113">Controlling transactions</span></span>

<span data-ttu-id="fe223-114">Aby rozpocząć, zatwierdzić i wycofać transakcje, można użyć interfejsu API `DbContext.Database`.</span><span class="sxs-lookup"><span data-stu-id="fe223-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="fe223-115">Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytania LINQ wykonywane w jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="fe223-116">Nie wszyscy dostawcy bazy danych obsługują transakcje.</span><span class="sxs-lookup"><span data-stu-id="fe223-116">Not all database providers support transactions.</span></span> <span data-ttu-id="fe223-117">Niektórzy dostawcy mogą zgłaszać wyjątki lub nie wykonywać żadnych operacji po wywołaniu interfejsu API transakcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="fe223-118">Transakcja międzykontekstowa (tylko relacyjne bazy danych)</span><span class="sxs-lookup"><span data-stu-id="fe223-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="fe223-119">Transakcje mogą być również współdzielone przez wiele wystąpień kontekstów.</span><span class="sxs-lookup"><span data-stu-id="fe223-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="fe223-120">Ta funkcja jest dostępna tylko wtedy, gdy jest używany dostawca relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="fe223-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="fe223-121">Aby udostępnić transakcję, konteksty muszą współdzielić zarówno `DbConnection`, jak i `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="fe223-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="fe223-122">Zezwalanie na dostarczanie połączenia z zewnątrz</span><span class="sxs-lookup"><span data-stu-id="fe223-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="fe223-123">Udostępnienie `DbConnection` wymaga możliwości przekazania połączenia do kontekstu podczas jego konstruowania.</span><span class="sxs-lookup"><span data-stu-id="fe223-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="fe223-124">Najprostszym sposobem na umożliwienie `DbConnection` być podano zewnętrznie, jest zaprzestanie korzystania z metody `DbContext.OnConfiguring` w celu skonfigurowania kontekstu i zewnętrznych tworzenia `DbContextOptions` i przekazania ich do konstruktora kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="fe223-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="fe223-125">`DbContextOptionsBuilder` jest interfejsem API używanym w `DbContext.OnConfiguring` do konfigurowania kontekstu, dlatego można używać go zewnętrznie do tworzenia `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="fe223-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="fe223-126">Alternatywą jest utrzymywanie `DbContext.OnConfiguring`, ale zaakceptowanie `DbConnection` zapisywanych, a następnie używanych w `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="fe223-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="fe223-127">Współdzielenie połączenia i transakcji</span><span class="sxs-lookup"><span data-stu-id="fe223-127">Share connection and transaction</span></span>

<span data-ttu-id="fe223-128">Teraz możesz utworzyć wiele wystąpień kontekstu współużytkujących to samo połączenie.</span><span class="sxs-lookup"><span data-stu-id="fe223-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="fe223-129">Następnie użyj interfejsu API `DbContext.Database.UseTransaction(DbTransaction)`, aby zarejestrować oba konteksty w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="fe223-130">Korzystanie z zewnętrznych transakcji `DbTransaction` (tylko relacyjne bazy danych)</span><span class="sxs-lookup"><span data-stu-id="fe223-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="fe223-131">Korzystając z wielu metod dostępu do danych w relacyjnej bazie danych, można współdzielić transakcje z operacjami wykonywanymi przez te różne metody.</span><span class="sxs-lookup"><span data-stu-id="fe223-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="fe223-132">Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework Core w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="fe223-133">Korzystanie z System. Transactions</span><span class="sxs-lookup"><span data-stu-id="fe223-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="fe223-134">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="fe223-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="fe223-135">Istnieje możliwość użycia transakcji otoczenia, aby umożliwić koordynację na większą skalę.</span><span class="sxs-lookup"><span data-stu-id="fe223-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="fe223-136">Istnieje również możliwość zarejestrowania w transakcji jawnej.</span><span class="sxs-lookup"><span data-stu-id="fe223-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="fe223-137">Ograniczenia przestrzeni nazw System.Transactions</span><span class="sxs-lookup"><span data-stu-id="fe223-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="fe223-138">Platforma EF Core korzysta z dostawców baz danych do implementowania obsługi przestrzeni nazw `System.Transactions`.</span><span class="sxs-lookup"><span data-stu-id="fe223-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="fe223-139">Mimo że dostawcy ADO.NET często obsługują platformę .NET Framework, ten interfejs API został dopiero niedawno dodany do platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="fe223-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="fe223-140">Jeżeli dostawca nie implementuje obsługi przestrzeni nazw `System.Transactions,` możliwe jest, że wywołania do tego interfejsu API zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="fe223-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="fe223-141">SqlClient dla platformy .NET Core obsługuje je od wersji 2.1 w górę.</span><span class="sxs-lookup"><span data-stu-id="fe223-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="fe223-142">Program SqlClient dla programu .NET Core 2,0 zgłosi wyjątek, jeśli spróbujesz użyć tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="fe223-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="fe223-143">Zaleca się przetestowanie, czy ten interfejs API działa poprawnie z Twoim dostawcą, zanim skorzystasz z niego do zarządzania transakcjami.</span><span class="sxs-lookup"><span data-stu-id="fe223-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="fe223-144">W przypadku problemów zachęcamy do kontaktu z osobami obsługującymi danego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fe223-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="fe223-145">Począwszy od wersji 2,1, implementacja System. Transactions w programie .NET Core nie obejmuje obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` ani `CommittableTransaction` do koordynowania transakcji w wielu menedżerach zasobów.</span><span class="sxs-lookup"><span data-stu-id="fe223-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
