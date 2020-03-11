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
# <a name="handling-transaction-commit-failures"></a>Obsługa niepowodzeń zatwierdzania transakcji
> [!NOTE]
> **EF 6.1 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6,1. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

W ramach 6,1 wprowadzamy nową funkcję odporności połączenia dla EF: możliwość automatycznego wykrywania i odzyskiwania w przypadku niepowodzeń połączeń przejściowych wpływających na potwierdzenie transakcji. Szczegółowe informacje o tym scenariuszu są najlepiej opisane w wpisie w blogu [SQL Database łączności i problem idempotentności](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Podsumowując, scenariusz polega na tym, że gdy wyjątek jest wywoływany podczas zatwierdzania transakcji, istnieją dwie możliwe przyczyny:  

1. Zatwierdzenie transakcji na serwerze nie powiodło się
2. Zatwierdzenie transakcji na serwerze zakończyło się pomyślnie, ale problem z połączeniem uniemożliwił powiadomienie klienta  

Gdy w pierwszej sytuacji występuje aplikacja lub użytkownik może ponowić próbę wykonania operacji, ale w przypadku powtórnej sytuacji należy unikać ponownych prób, a aplikacja może automatycznie odzyskiwać. Wyzwanie polega na tym, że bez możliwości wykrycia przyczyny tego wyjątku podczas zatwierdzania, aplikacja nie może wybrać właściwego przebiegu akcji. Nowa funkcja w EF 6,1 umożliwia programowi EF sprawdzenie podwójnej kontroli z bazą danych, jeśli transakcja zakończyła się pomyślnie i podejmuje właściwy przebieg działania w sposób przezroczysty.  

## <a name="using-the-feature"></a>Korzystanie z funkcji  

Aby włączyć wymaganą funkcję, należy dołączyć wywołanie do [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) w konstruktorze **dbconfiguration**. Jeśli nie znasz **konfiguracji dbconfiguration**, zobacz [Konfiguracja oparta na kodzie](~/ef6/fundamentals/configuring/code-based.md). Ta funkcja może być używana w połączeniu z automatycznymi ponownymi próbami wprowadzonymi w EF6, które pomagają w sytuacji, w której transakcja faktycznie nie została zatwierdzona na serwerze z powodu przejściowego błędu:  

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

## <a name="how-transactions-are-tracked"></a>Jak są śledzone transakcje  

Gdy funkcja jest włączona, EF automatycznie doda nową tabelę do bazy danych o nazwie **__Transactions**. Nowy wiersz jest wstawiany w tej tabeli za każdym razem, gdy transakcja jest tworzona przez EF i ten wiersz jest sprawdzany pod kątem istnienia błędu transakcji podczas zatwierdzania.  

Mimo że Dr będzie najlepszym wysiłkiem w celu oczyszczenia wierszy z tabeli, gdy nie są one już potrzebne, tabela może się zwiększać, jeśli aplikacja zakończy się przedwcześnie i z tego powodu może być konieczne ręczne przeczyszczenie tabeli w niektórych przypadkach.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Jak obsługiwać błędy zatwierdzania z poprzednimi wersjami

Przed Dr 6,1 nie było mechanizmu obsługi błędów zatwierdzania w produkcie EF. Istnieje kilka sposobów postępowania z tą sytuacją, którą można zastosować do poprzednich wersji EF6:  

* Opcja 1 — nic nie rób  

  Prawdopodobieństwo błędu połączenia podczas zatwierdzania transakcji jest niskie, dlatego może być akceptowalne, aby aplikacja mogła zakończyć się niepowodzeniem, jeśli ten stan rzeczywiście wystąpi.  

* Opcja 2 — używanie bazy danych do resetowania stanu  

  1. Odrzuć bieżący kontekst  
  2. Utwórz nowy DbContext i Przywróć stan aplikacji z bazy danych  
  3. Informowanie użytkownika o tym, że Ostatnia operacja mogła nie zostać ukończona pomyślnie  

* Opcja 3 — ręcznie Śledź transakcję  

  1. Dodaj nieśledzoną tabelę do bazy danych używanej do śledzenia stanu transakcji.  
  2. Wstaw wiersz do tabeli na początku każdej transakcji.  
  3. Jeśli połączenie zakończy się niepowodzeniem podczas zatwierdzania, sprawdź obecność odpowiedniego wiersza w bazie danych.  
     - Jeśli wiersz jest obecny, kontynuuj normalnie, ponieważ transakcja została pomyślnie zatwierdzona  
     - Jeśli wiersz jest nieobecny, Użyj strategii wykonywania, aby ponowić bieżącą operację.  
  4. Jeśli zatwierdzenie zakończy się pomyślnie, Usuń odpowiedni wiersz, aby uniknąć wzrostu tabeli.  

[Ten wpis w blogu](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) zawiera przykładowy kod do wykonania na platformie SQL Azure.  
