---
title: Obsługa transakcji zatwierdzenia awarie - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250865"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="59dc6-102">Obsługa błędów zatwierdzenie transakcji</span><span class="sxs-lookup"><span data-stu-id="59dc6-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="59dc6-103">**EF6.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="59dc6-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="59dc6-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="59dc6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="59dc6-105">Jako część 6.1 wprowadziliśmy nową funkcję odporności połączenia na platformie EF: możliwość wykrywania i automatycznego odzyskania po awarii przejściowych połączenia mają wpływ na potwierdzenia zatwierdzeń transakcji.</span><span class="sxs-lookup"><span data-stu-id="59dc6-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="59dc6-106">Pełne szczegóły tego scenariusza są najlepiej opisać wpis w blogu [łączność z bazą danych SQL i problem idempotentności](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="59dc6-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="59dc6-107">Podsumowując scenariusz zakłada, że gdy wyjątek jest zgłaszany podczas zatwierdzania transakcji dwie możliwe przyczyny:</span><span class="sxs-lookup"><span data-stu-id="59dc6-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="59dc6-108">Zatwierdzanie transakcji nie powiodło się na serwerze</span><span class="sxs-lookup"><span data-stu-id="59dc6-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="59dc6-109">Zatwierdzanie transakcji zakończyło się pomyślnie na serwerze, ale problem z łącznością uniemożliwił powiadomienie o powodzeniu, zanim klient</span><span class="sxs-lookup"><span data-stu-id="59dc6-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="59dc6-110">Po pierwszym sytuacji się dzieje w aplikacji lub użytkownika, można, spróbuj ponownie wykonać operację, ale gdy druga sytuacja ma miejsce należy unikać ponownych prób i aplikacji może automatycznie odzyskać.</span><span class="sxs-lookup"><span data-stu-id="59dc6-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="59dc6-111">Żądania jest to, że bez możliwości wykrywania, jaki był rzeczywista Przyczyna, wyjątek został zgłoszony podczas zatwierdzania, aplikacja nie może wybrać właściwą drogą akcji.</span><span class="sxs-lookup"><span data-stu-id="59dc6-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="59dc6-112">Nowa funkcja w EF 6.1 umożliwia EF dokładnie z bazą danych, jeśli transakcja zakończyła się pomyślnie i podjąć właściwą drogą akcji w sposób niewidoczny dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="59dc6-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="59dc6-113">Za pomocą funkcji</span><span class="sxs-lookup"><span data-stu-id="59dc6-113">Using the feature</span></span>  

<span data-ttu-id="59dc6-114">Aby włączyć tę funkcję musi zawierać wywołanie [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) w Konstruktorze typu usługi **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="59dc6-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="59dc6-115">Jeśli znasz **DbConfiguration**, zobacz [konfiguracja na podstawie kodu](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="59dc6-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="59dc6-116">Ta funkcja może być używana w połączeniu z automatycznym ponawianiem wprowadzona w EF6, które pomagają w sytuacji, w którym faktycznie nie można zatwierdzić transakcji na serwerze z powodu przejściowego błędu:</span><span class="sxs-lookup"><span data-stu-id="59dc6-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="59dc6-117">Jak są śledzone transakcji</span><span class="sxs-lookup"><span data-stu-id="59dc6-117">How transactions are tracked</span></span>  

<span data-ttu-id="59dc6-118">Po włączeniu funkcji EF automatycznie Dodaj nową tabelę w bazie danych o nazwie **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="59dc6-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="59dc6-119">Nowy wiersz jest wstawiana w tej tabeli, każdym razem, gdy transakcja jest tworzona przez EF i tym wierszu są sprawdzane pod kątem istnienia Jeśli wystąpi awaria transakcji podczas zatwierdzania.</span><span class="sxs-lookup"><span data-stu-id="59dc6-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="59dc6-120">Mimo że EF wykona najlepszy nakład pracy, aby oczyścić wiersze z tabeli, gdy nie są już potrzebne, tabelę można powiększać, jeśli aplikacja kończy działanie przedwcześnie i z tego powodu, czego potrzebujesz do przeczyszczenia tabeli ręcznie w niektórych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="59dc6-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="59dc6-121">Sposób obsługi błędów zatwierdzenia z poprzednimi wersjami</span><span class="sxs-lookup"><span data-stu-id="59dc6-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="59dc6-122">Przed EF 6.1 był nie mechanizmem do obsługi zatwierdzenia awarie w produkcie EF.</span><span class="sxs-lookup"><span data-stu-id="59dc6-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="59dc6-123">Istnieje kilka sposobów radzenia sobie z tą sytuacją, które mogą być stosowane do poprzednich wersji EF6:</span><span class="sxs-lookup"><span data-stu-id="59dc6-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="59dc6-124">Opcja 1 — nic nie rób</span><span class="sxs-lookup"><span data-stu-id="59dc6-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="59dc6-125">Prawdopodobieństwo awarii połączenia podczas zatwierdzania transakcji jest niska, dlatego może być akceptowalne, aby aplikacja została właśnie się niepowodzeniem, jeśli rzeczywiście występuje ten problem.</span><span class="sxs-lookup"><span data-stu-id="59dc6-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="59dc6-126">Opcja 2 — umożliwia resetowanie stanu bazy danych</span><span class="sxs-lookup"><span data-stu-id="59dc6-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="59dc6-127">Odrzuć bieżącego kontekstu DbContext</span><span class="sxs-lookup"><span data-stu-id="59dc6-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="59dc6-128">Tworzenie nowego typu DbContext i przywrócenia stanu aplikacji z bazy danych</span><span class="sxs-lookup"><span data-stu-id="59dc6-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="59dc6-129">Informuje użytkownika, że ostatnia operacja może nie zostały zakończone pomyślnie</span><span class="sxs-lookup"><span data-stu-id="59dc6-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="59dc6-130">Opcja 3 — ręcznie śledzić transakcji</span><span class="sxs-lookup"><span data-stu-id="59dc6-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="59dc6-131">Dodaj tabelę — śledzone w bazie danych używane do śledzenia stanu transakcji.</span><span class="sxs-lookup"><span data-stu-id="59dc6-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="59dc6-132">Wstaw wiersz do tabeli na początku każdej transakcji.</span><span class="sxs-lookup"><span data-stu-id="59dc6-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="59dc6-133">Jeśli połączenie nie powiedzie się podczas zatwierdzania, sprawdź, czy obecność odpowiedni wiersz w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="59dc6-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="59dc6-134">Jeśli wiersz jest obecny, kontynuował normalne, ponieważ transakcja została pomyślnie zatwierdzona</span><span class="sxs-lookup"><span data-stu-id="59dc6-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="59dc6-135">Jeśli wiersz jest nieobecne, należy użyć strategii wykonywania, aby ponowić próbę wykonania bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="59dc6-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="59dc6-136">Jeśli zatwierdzenie zakończy się pomyślnie, usuń odpowiedni wiersz w celu uniknięcia wzrostu tabeli.</span><span class="sxs-lookup"><span data-stu-id="59dc6-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="59dc6-137">[Ten wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) zawiera przykładowy kod realizacji tego zadania na platformie Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="59dc6-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
