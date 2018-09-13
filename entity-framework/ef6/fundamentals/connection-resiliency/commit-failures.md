---
title: Obsługa transakcji zatwierdzenia awarie - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 71d5649dd993bb95e24165a55d812c71a37f03f3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489391"
---
# <a name="handling-transaction-commit-failures"></a>Obsługa błędów zatwierdzenie transakcji
> [!NOTE]
> **EF6.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.1. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Jako część 6.1 wprowadziliśmy nową funkcję odporności połączenia na platformie EF: możliwość wykrywania i automatycznego odzyskania po awarii przejściowych połączenia mają wpływ na potwierdzenia zatwierdzeń transakcji. Pełne szczegóły tego scenariusza są najlepiej opisać wpis w blogu [łączność z bazą danych SQL i problem idempotentności](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Podsumowując scenariusz zakłada, że gdy wyjątek jest zgłaszany podczas zatwierdzania transakcji dwie możliwe przyczyny:  

1. Zatwierdzanie transakcji nie powiodło się na serwerze
2. Zatwierdzanie transakcji zakończyło się pomyślnie na serwerze, ale problem z łącznością uniemożliwił powiadomienie o powodzeniu, zanim klient  

Po pierwszym sytuacji się dzieje w aplikacji lub użytkownika, można, spróbuj ponownie wykonać operację, ale gdy druga sytuacja ma miejsce należy unikać ponownych prób i aplikacji może automatycznie odzyskać. Żądania jest to, że bez możliwości wykrywania, jaki był rzeczywista Przyczyna, wyjątek został zgłoszony podczas zatwierdzania, aplikacja nie może wybrać właściwą drogą akcji. Nowa funkcja w EF 6.1 umożliwia EF dokładnie z bazą danych, jeśli transakcja zakończyła się pomyślnie i podjąć właściwą drogą akcji w sposób niewidoczny dla użytkownika.  

## <a name="using-the-feature"></a>Za pomocą funkcji  

Aby włączyć tę funkcję musi zawierać wywołanie [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) w Konstruktorze typu usługi **DbConfiguration**. Jeśli znasz **DbConfiguration**, zobacz [konfiguracja na podstawie kodu](~/ef6/fundamentals/configuring/code-based.md). Ta funkcja może być używana w połączeniu z automatycznym ponawianiem wprowadzona w EF6, które pomagają w sytuacji, w którym faktycznie nie można zatwierdzić transakcji na serwerze z powodu przejściowego błędu:  

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

## <a name="how-transactions-are-tracked"></a>Jak są śledzone transakcji  

Po włączeniu funkcji EF automatycznie Dodaj nową tabelę w bazie danych o nazwie **__Transactions**. Nowy wiersz jest wstawiana w tej tabeli, każdym razem, gdy transakcja jest tworzona przez EF i tym wierszu są sprawdzane pod kątem istnienia Jeśli wystąpi awaria transakcji podczas zatwierdzania.  

Mimo że EF wykona najlepszy nakład pracy, aby oczyścić wiersze z tabeli, gdy nie są już potrzebne, tabelę można powiększać, jeśli aplikacja kończy działanie przedwcześnie i z tego powodu, czego potrzebujesz do przeczyszczenia tabeli ręcznie w niektórych przypadkach.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Sposób obsługi błędów zatwierdzenia z poprzednimi wersjami

Przed EF 6.1 był nie mechanizmem do obsługi zatwierdzenia awarie w produkcie EF. Istnieje kilka sposobów radzenia sobie z tą sytuacją, które mogą być stosowane do poprzednich wersji EF6:  

* Opcja 1 — nic nie rób  

  Prawdopodobieństwo awarii połączenia podczas zatwierdzania transakcji jest niska, dlatego może być akceptowalne, aby aplikacja została właśnie się niepowodzeniem, jeśli rzeczywiście występuje ten problem.  

* Opcja 2 — umożliwia resetowanie stanu bazy danych  

  1. Odrzuć bieżącego kontekstu DbContext  
  2. Tworzenie nowego typu DbContext i przywrócenia stanu aplikacji z bazy danych  
  3. Informuje użytkownika, że ostatnia operacja może nie zostały zakończone pomyślnie  

* Opcja 3 — ręcznie śledzić transakcji  

  1. Dodaj tabelę — śledzone w bazie danych używane do śledzenia stanu transakcji.  
  2. Wstaw wiersz do tabeli na początku każdej transakcji.  
  3. Jeśli połączenie nie powiedzie się podczas zatwierdzania, sprawdź, czy obecność odpowiedni wiersz w bazie danych.  
     - Jeśli wiersz jest obecny, kontynuował normalne, ponieważ transakcja została pomyślnie zatwierdzona  
     - Jeśli wiersz jest nieobecne, należy użyć strategii wykonywania, aby ponowić próbę wykonania bieżącej operacji.  
  4. Jeśli zatwierdzenie zakończy się pomyślnie, usuń odpowiedni wiersz w celu uniknięcia wzrostu tabeli.  

[Ten wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) zawiera przykładowy kod realizacji tego zadania na platformie Azure SQL.  
