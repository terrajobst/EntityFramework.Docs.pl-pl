---
title: Obsługa niepowodzeń zatwierdzania transakcji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417371"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="a71fc-102">Obsługa niepowodzeń zatwierdzania transakcji</span><span class="sxs-lookup"><span data-stu-id="a71fc-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="a71fc-103">**EF 6.1 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="a71fc-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a71fc-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="a71fc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a71fc-105">W ramach 6,1 wprowadzamy nową funkcję odporności połączenia dla EF: możliwość automatycznego wykrywania i odzyskiwania w przypadku niepowodzeń połączeń przejściowych wpływających na potwierdzenie transakcji.</span><span class="sxs-lookup"><span data-stu-id="a71fc-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="a71fc-106">Szczegółowe informacje o tym scenariuszu są najlepiej opisane w wpisie w blogu [SQL Database łączności i problem idempotentności](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="a71fc-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="a71fc-107">Podsumowując, scenariusz polega na tym, że gdy wyjątek jest wywoływany podczas zatwierdzania transakcji, istnieją dwie możliwe przyczyny:</span><span class="sxs-lookup"><span data-stu-id="a71fc-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="a71fc-108">Zatwierdzenie transakcji na serwerze nie powiodło się</span><span class="sxs-lookup"><span data-stu-id="a71fc-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="a71fc-109">Zatwierdzenie transakcji na serwerze zakończyło się pomyślnie, ale problem z połączeniem uniemożliwił powiadomienie klienta</span><span class="sxs-lookup"><span data-stu-id="a71fc-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="a71fc-110">Gdy w pierwszej sytuacji występuje aplikacja lub użytkownik może ponowić próbę wykonania operacji, ale w przypadku powtórnej sytuacji należy unikać ponownych prób, a aplikacja może automatycznie odzyskiwać.</span><span class="sxs-lookup"><span data-stu-id="a71fc-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="a71fc-111">Wyzwanie polega na tym, że bez możliwości wykrycia przyczyny tego wyjątku podczas zatwierdzania, aplikacja nie może wybrać właściwego przebiegu akcji.</span><span class="sxs-lookup"><span data-stu-id="a71fc-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="a71fc-112">Nowa funkcja w EF 6,1 umożliwia programowi EF sprawdzenie podwójnej kontroli z bazą danych, jeśli transakcja zakończyła się pomyślnie i podejmuje właściwy przebieg działania w sposób przezroczysty.</span><span class="sxs-lookup"><span data-stu-id="a71fc-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="a71fc-113">Korzystanie z funkcji</span><span class="sxs-lookup"><span data-stu-id="a71fc-113">Using the feature</span></span>  

<span data-ttu-id="a71fc-114">Aby włączyć wymaganą funkcję, należy dołączyć wywołanie do [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) w konstruktorze **dbconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a71fc-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="a71fc-115">Jeśli nie znasz **konfiguracji dbconfiguration**, zobacz [Konfiguracja oparta na kodzie](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="a71fc-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="a71fc-116">Ta funkcja może być używana w połączeniu z automatycznymi ponownymi próbami wprowadzonymi w EF6, które pomagają w sytuacji, w której transakcja faktycznie nie została zatwierdzona na serwerze z powodu przejściowego błędu:</span><span class="sxs-lookup"><span data-stu-id="a71fc-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="a71fc-117">Jak są śledzone transakcje</span><span class="sxs-lookup"><span data-stu-id="a71fc-117">How transactions are tracked</span></span>  

<span data-ttu-id="a71fc-118">Gdy funkcja jest włączona, EF automatycznie doda nową tabelę do bazy danych o nazwie **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="a71fc-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="a71fc-119">Nowy wiersz jest wstawiany w tej tabeli za każdym razem, gdy transakcja jest tworzona przez EF i ten wiersz jest sprawdzany pod kątem istnienia błędu transakcji podczas zatwierdzania.</span><span class="sxs-lookup"><span data-stu-id="a71fc-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="a71fc-120">Mimo że Dr będzie najlepszym wysiłkiem w celu oczyszczenia wierszy z tabeli, gdy nie są one już potrzebne, tabela może się zwiększać, jeśli aplikacja zakończy się przedwcześnie i z tego powodu może być konieczne ręczne przeczyszczenie tabeli w niektórych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="a71fc-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="a71fc-121">Jak obsługiwać błędy zatwierdzania z poprzednimi wersjami</span><span class="sxs-lookup"><span data-stu-id="a71fc-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="a71fc-122">Przed Dr 6,1 nie było mechanizmu obsługi błędów zatwierdzania w produkcie EF.</span><span class="sxs-lookup"><span data-stu-id="a71fc-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="a71fc-123">Istnieje kilka sposobów postępowania z tą sytuacją, którą można zastosować do poprzednich wersji EF6:</span><span class="sxs-lookup"><span data-stu-id="a71fc-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="a71fc-124">Opcja 1 — nic nie rób</span><span class="sxs-lookup"><span data-stu-id="a71fc-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="a71fc-125">Prawdopodobieństwo błędu połączenia podczas zatwierdzania transakcji jest niskie, dlatego może być akceptowalne, aby aplikacja mogła zakończyć się niepowodzeniem, jeśli ten stan rzeczywiście wystąpi.</span><span class="sxs-lookup"><span data-stu-id="a71fc-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="a71fc-126">Opcja 2 — używanie bazy danych do resetowania stanu</span><span class="sxs-lookup"><span data-stu-id="a71fc-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="a71fc-127">Odrzuć bieżący kontekst</span><span class="sxs-lookup"><span data-stu-id="a71fc-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="a71fc-128">Utwórz nowy DbContext i Przywróć stan aplikacji z bazy danych</span><span class="sxs-lookup"><span data-stu-id="a71fc-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="a71fc-129">Informowanie użytkownika o tym, że Ostatnia operacja mogła nie zostać ukończona pomyślnie</span><span class="sxs-lookup"><span data-stu-id="a71fc-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="a71fc-130">Opcja 3 — ręcznie Śledź transakcję</span><span class="sxs-lookup"><span data-stu-id="a71fc-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="a71fc-131">Dodaj nieśledzoną tabelę do bazy danych używanej do śledzenia stanu transakcji.</span><span class="sxs-lookup"><span data-stu-id="a71fc-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="a71fc-132">Wstaw wiersz do tabeli na początku każdej transakcji.</span><span class="sxs-lookup"><span data-stu-id="a71fc-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="a71fc-133">Jeśli połączenie zakończy się niepowodzeniem podczas zatwierdzania, sprawdź obecność odpowiedniego wiersza w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a71fc-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="a71fc-134">Jeśli wiersz jest obecny, kontynuuj normalnie, ponieważ transakcja została pomyślnie zatwierdzona</span><span class="sxs-lookup"><span data-stu-id="a71fc-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="a71fc-135">Jeśli wiersz jest nieobecny, Użyj strategii wykonywania, aby ponowić bieżącą operację.</span><span class="sxs-lookup"><span data-stu-id="a71fc-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="a71fc-136">Jeśli zatwierdzenie zakończy się pomyślnie, Usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.</span><span class="sxs-lookup"><span data-stu-id="a71fc-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="a71fc-137">[Ten wpis w blogu](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) zawiera przykładowy kod do wykonania na platformie SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a71fc-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
