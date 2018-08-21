---
title: Transakcje — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 6e6ded74e15187b387e8e0b2ad00cb47a84ff7e8
ms.sourcegitcommit: 6cf6493d81b6d81b0b0f37a00e0fc23ec7189158
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35323830"
---
# <a name="using-transactions"></a><span data-ttu-id="02b90-102">Używanie transakcji</span><span class="sxs-lookup"><span data-stu-id="02b90-102">Using Transactions</span></span>

<span data-ttu-id="02b90-103">Transakcje pozwalają na wykonanie kilku operacji na bazie danych w sposób niepodzielny.</span><span class="sxs-lookup"><span data-stu-id="02b90-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="02b90-104">Jeżeli transakcja zostanie zatwierdzona, wszystkie operacje zostaną pomyślnie wykonane na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="02b90-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="02b90-105">Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie wykonana na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="02b90-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="02b90-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="02b90-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="02b90-107">Domyślne zachowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="02b90-107">Default transaction behavior</span></span>

<span data-ttu-id="02b90-108">Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w pojedynczym wywołaniu operacji `SaveChanges()` są stosowane w transakcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="02b90-109">Jeżeli jakakolwiek z tych zmian nie powiedzie się, transakcja zostanie wycofana i zmiany nie zostaną zapisane.</span><span class="sxs-lookup"><span data-stu-id="02b90-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="02b90-110">Gwarantuje to, że operacja `SaveChanges()` albo powiedzie się całkowicie, albo spowoduje pozostawienie niezmodyfikowanej bazy danych w przypadku błędu.</span><span class="sxs-lookup"><span data-stu-id="02b90-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="02b90-111">W przypadku większości zastosowań to domyślne zachowanie jest wystarczające.</span><span class="sxs-lookup"><span data-stu-id="02b90-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="02b90-112">Transakcje powinny być sterowane ręcznie, tylko jeżeli taka konieczność wynika z wymagań danego zastosowania.</span><span class="sxs-lookup"><span data-stu-id="02b90-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="02b90-113">Kontrolowanie transakcji</span><span class="sxs-lookup"><span data-stu-id="02b90-113">Controlling transactions</span></span>

<span data-ttu-id="02b90-114">Transakcje można rozpoczynać, zatwierdzać i wycofywać w interfejsie API `DbContext.Database`.</span><span class="sxs-lookup"><span data-stu-id="02b90-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="02b90-115">Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytanie LINQ wykonane w ramach jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="02b90-116">Nie wszyscy dostawcy bazy danych obsługują transakcje.</span><span class="sxs-lookup"><span data-stu-id="02b90-116">Not all database providers support transactions.</span></span> <span data-ttu-id="02b90-117">Niektórzy dostawcy mogą zgłaszać wyjątki lub nie wykonywać żadnych operacji po wywołaniu interfejsu API transakcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="02b90-118">Transakcja międzykontekstowa (tylko relacyjne bazy danych)</span><span class="sxs-lookup"><span data-stu-id="02b90-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="02b90-119">Transakcje mogą być również współdzielone przez wiele wystąpień kontekstów.</span><span class="sxs-lookup"><span data-stu-id="02b90-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="02b90-120">Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia parametrów`DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="02b90-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="02b90-121">Aby współdzielić transakcję, konteksty muszą współdzielić zarówno parameter `DbConnection`, jak i `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="02b90-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="02b90-122">Zezwalanie na dostarczanie połączenia z zewnątrz</span><span class="sxs-lookup"><span data-stu-id="02b90-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="02b90-123">Współdzielenie parametru `DbConnection` wymaga, aby była możliwość przekazania połączenia do kontekstu podczas jego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="02b90-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="02b90-124">Najprostszym sposobem na to, aby dostarczanie parametru `DbConnection` było możliwe z zewnątrz, jest zrezygnowanie z używania metody `DbContext.OnConfiguring` do konfigurowania kontekstu i utworzenie opcji `DbContextOptions` na zewnątrz, a następnie przekazanie ich do konstruktora kontekstu.</span><span class="sxs-lookup"><span data-stu-id="02b90-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="02b90-125">`DbContextOptionsBuilder` jest używany w API `DbContext.OnConfiguring` można skonfigurować kontekstu, teraz będzie go zewnętrznie użyć do utworzenia `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="02b90-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="02b90-126">Alternatywą jest dalsze używanie parametru `DbContext.OnConfiguring`, ale akceptowanie parametru `DbConnection`, który jest zapisywany i następnie używany w parametrze `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="02b90-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="02b90-127">Współdzielenie połączenia i transakcji</span><span class="sxs-lookup"><span data-stu-id="02b90-127">Share connection and transaction</span></span>

<span data-ttu-id="02b90-128">Teraz możesz utworzyć wiele wystąpień kontekstu współużytkujących to samo połączenie.</span><span class="sxs-lookup"><span data-stu-id="02b90-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="02b90-129">Następnie użyj interfejsu API `DbContext.Database.UseTransaction(DbTransaction)` do zarejestrowania wielu kontekstów do jednej transakcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="02b90-130">Za pomocą zewnętrznego DbTransactions (relacyjnych baz danych tylko)</span><span class="sxs-lookup"><span data-stu-id="02b90-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="02b90-131">Korzystając z wielu metod dostępu do danych w relacyjnej bazie danych, można współdzielić transakcje z operacjami wykonywanymi przez te różne metody.</span><span class="sxs-lookup"><span data-stu-id="02b90-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="02b90-132">Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework Core w tej samej transakcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="02b90-133">Przy użyciu System.Transactions</span><span class="sxs-lookup"><span data-stu-id="02b90-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="02b90-134">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="02b90-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="02b90-135">Istnieje możliwość użycia transakcji otoczenia, aby umożliwić koordynację na większą skalę.</span><span class="sxs-lookup"><span data-stu-id="02b90-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="02b90-136">Istnieje również możliwość zarejestrowania w transakcji jawnej.</span><span class="sxs-lookup"><span data-stu-id="02b90-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="02b90-137">Ograniczenia przestrzeni nazw System.Transactions</span><span class="sxs-lookup"><span data-stu-id="02b90-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="02b90-138">Podstawowe EF zależy od dostawcy bazy danych, obsługa System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="02b90-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="02b90-139">Mimo że pomocy technicznej jest dość często wśród dostawców ADO.NET dla programu .NET Framework, interfejsu API tylko został ostatnio dodany do platformy .NET Core i dlatego nie jest tak szerokie pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="02b90-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="02b90-140">Jeśli dostawca nie implementuje obsługę System.Transactions, istnieje możliwość, że wywołań do tych interfejsów API będą ignorowane całkowicie.</span><span class="sxs-lookup"><span data-stu-id="02b90-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="02b90-141">Klient SQL dla platformy .NET Core obsługuje z 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="02b90-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="02b90-142">SqlClient programu .NET Core 2.0 spowoduje zgłoszenie wyjątku z próby użycia funkcji.</span><span class="sxs-lookup"><span data-stu-id="02b90-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="02b90-143">Zaleca się przetestowanie, czy ten interfejs API działa poprawnie z Twoim dostawcą, zanim skorzystasz z niego do zarządzania transakcjami.</span><span class="sxs-lookup"><span data-stu-id="02b90-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="02b90-144">W przypadku problemów zachęcamy do kontaktu z osobami obsługującymi danego dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="02b90-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="02b90-145">Począwszy od wersji 2.1 implementacja System.Transactions w .NET Core nie ma obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` lub `CommitableTransaction` aby koordynować transakcje w wielu menedżerów zasobów.</span><span class="sxs-lookup"><span data-stu-id="02b90-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
