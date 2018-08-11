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
# <a name="using-transactions"></a>Używanie transakcji

Transakcje pozwalają na wykonanie kilku operacji na bazie danych w sposób atomowy. Jeżeli transakcja zostanie zatwierdzona, wszystkie operacje zostaną pomyślnie zastosowane na bazie danych. Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie stosowana na bazy danych.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) wykorzystany w tym artykule możesz zobaczyć na GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, w jednym wywołaniu `SaveChanges()` wszystkie zmiany są stosowane w ramach transakcji. W przypadku błędów w trakcie stosowania zmian, transakcja zostanie wycofana i zmiany nie zostaną zapisane. Oznacza to gwarancję, że `SaveChanges()` całkowicie powiedzie się, lub w razie błędów pozostawi bazę danych niezmodyfikowaną.

W przypadku większości aplikacji domyślne zachowanie jest wystarczające. Transakcje powinny być dodawane ręcznie jeżeli aplikacja tego wymaga.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Można użyć API `DbContext.Database` aby rozpocząć, zatwiedzić i wycofać transakcje. Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytanie LINQ wykonane w ramach jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługują transakcje. Niektórzy dostawcy mogą rzucać wyjątki lub nie robić nic podczas korzystania z API transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakcje między `DbContext`ami (tylko relacyjne bazy danych)

Transakcje mogą być współdzielone przez wiele `DbContext`ów. Ta funkcja jest dostępna tylko w przypadku korzystania z relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby współdzielić transakcję, kontekst musi udostępniać zarówno `DbConnection` i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwól na przekazanie połączenia z zewnątrz

Współdzielenie `DbConnection` wymaga możliwości przekazania połączenia do kontekstu podczas jego tworzenia.

Najprostszym sposobem, aby umożliwić przekazanie `DbConnection` z zewnątrz jest zaprzestanie użycia `DbContext.OnConfiguring` w celu konfiguracji kontekstu i stworzyć `DbContextOptions` i przekazać je do konstruktora kontekstu.

> [!TIP]  
> `DbContextOptionsBuilder` jest używany w `DbContext.OnConfiguring` aby skonfigurować kontekst. Teraz będziesz użyty poza tą metodą celem utworzenia `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatywą jest nadal używanie `DbContext.OnConfiguring`, ale akceptowanie `DbConnection` które jest zapisywane i następnie używane w `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Współdzielenie połączenia i transakcji

Teraz możesz utworzyć wiele instancji kontekstu, które współużytkują to samo połączenie. Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` do zarejestrowania wielu kontekstów do jednej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Używanie `DbTransaction` z zewnątrz (tylko relacyjne baz danych)

Korzystając z wielu metod dostępu do danych w relacyjnej bazie danych, można współdzielić transakcje między tymi metodami.

Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework Core w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Przy użyciu System.Transactions

> [!NOTE]  
> Nowa funkcjonalność w EF Core 2.1.

Istnieje możliwość użycia transakcji _ambient_ w celu koordynacji na większą skalę.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Istnieje również możliwość skorzystania z jawnych transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia `System.Transactions`

1. EF Core zależy od obsługi `System.Transactions` przez dostawcę bazy danych. Mimo, że dostacy ADO.NET często obsługją .NET Framework, to API zostało dopiero niedawno dodane do EF Core. Stąd, wsparcie może nie być szerokie. Jeżeli dostawca nie wspiera `System.Transactions,` możliwe jest, że użycie tego API zostanie zignorowane. `SqlClient` dla .NET Core wspiera je od wersji 2.1 w górę. `SqlClient` dla .NET Core 2.0 rzuci wyjątek podczas próby skorzystania z ten funkcjonalności.

   > [!IMPORTANT]  
   > Zaleca się przetestowanie czy to API zachowuje się poprawnie w przypadku konkretnej bazy danych zanim zaczniesz polegać na transakcjach. Zachęcamy do kontaktowania się z osobami utrzymującymi mechanizmy dostępu do bazy danych w przypadku problemów.

2. Począwszy od wersji 2.1 implementacja `System.Transactions` w .NET Core nie obsługje transakcji rozproszonych, dlatego nie można używać `TransactionScope` lub `CommitableTransaction` aby koordynować transakcje w wielu menadżerach zasobów. 
