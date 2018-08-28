---
title: Transakcje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 7083a1228420416a1b60d9744ca2dad2339be53f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993608"
---
# <a name="using-transactions"></a>Za pomocą transakcji

Transakcje pozwalają na wykonanie kilku operacji na bazie danych w sposób niepodzielny. Jeżeli transakcja zostanie zatwierdzona, wszystkie operacje zostaną pomyślnie wykonane na bazie danych. Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie wykonana na bazie danych.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w pojedynczym wywołaniu operacji `SaveChanges()` są stosowane w transakcji. Jeżeli jakakolwiek z tych zmian nie powiedzie się, transakcja zostanie wycofana i zmiany nie zostaną zapisane. Gwarantuje to, że operacja `SaveChanges()` albo powiedzie się całkowicie, albo spowoduje pozostawienie niezmodyfikowanej bazy danych w przypadku błędu.

W przypadku większości zastosowań to domyślne zachowanie jest wystarczające. Transakcje powinny być sterowane ręcznie, tylko jeżeli taka konieczność wynika z wymagań danego zastosowania.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Transakcje można rozpoczynać, zatwierdzać i wycofywać w interfejsie API `DbContext.Database`. Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytanie LINQ wykonane w ramach jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługują transakcje. Niektórzy dostawcy mogą zgłaszać wyjątki lub nie wykonywać żadnych operacji po wywołaniu interfejsu API transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakcja międzykontekstowa (tylko relacyjne bazy danych)

Transakcje mogą być również współdzielone przez wiele wystąpień kontekstów. Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia parametrów`DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby współdzielić transakcję, konteksty muszą współdzielić zarówno parameter `DbConnection`, jak i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalanie na dostarczanie połączenia z zewnątrz

Współdzielenie parametru `DbConnection` wymaga, aby była możliwość przekazania połączenia do kontekstu podczas jego tworzenia.

Najprostszym sposobem na to, aby dostarczanie parametru `DbConnection` było możliwe z zewnątrz, jest zrezygnowanie z używania metody `DbContext.OnConfiguring` do konfigurowania kontekstu i utworzenie opcji `DbContextOptions` na zewnątrz, a następnie przekazanie ich do konstruktora kontekstu.

> [!TIP]  
> Parametr `DbContextOptionsBuilder` jest interfejsem API używanym w parametrze `DbContext.OnConfiguring` do konfigurowania kontekstu. Zostanie on teraz użyty zewnętrznie do utworzenia parametru `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatywą jest dalsze używanie parametru `DbContext.OnConfiguring`, ale akceptowanie parametru `DbConnection`, który jest zapisywany i następnie używany w parametrze `DbContext.OnConfiguring`.

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

Teraz możesz utworzyć wiele wystąpień kontekstu współużytkujących to samo połączenie. Następnie użyj interfejsu API `DbContext.Database.UseTransaction(DbTransaction)` do zarejestrowania wielu kontekstów do jednej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Korzystanie z zewnętrznych transakcji `DbTransaction` (tylko relacyjne bazy danych)

Korzystając z wielu metod dostępu do danych w relacyjnej bazie danych, można współdzielić transakcje z operacjami wykonywanymi przez te różne metody.

Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework Core w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Używanie System.Transactions

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Istnieje możliwość użycia transakcji otoczenia, aby umożliwić koordynację na większą skalę.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Istnieje również możliwość zarejestrowania w transakcji jawnej.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia przestrzeni nazw System.Transactions  

1. Platforma EF Core korzysta z dostawców baz danych do implementowania obsługi przestrzeni nazw `System.Transactions`. Mimo że dostawcy ADO.NET często obsługują platformę .NET Framework, ten interfejs API został dopiero niedawno dodany do platformy EF Core. Jeżeli dostawca nie implementuje obsługi przestrzeni nazw `System.Transactions,` możliwe jest, że wywołania do tego interfejsu API zostaną zignorowane. SqlClient dla platformy .NET Core obsługuje je od wersji 2.1 w górę. SqlClient dla platformy .NET Core 2.0 zgłosi wyjątek w przypadku próby skorzystania z tej funkcji. 

   > [!IMPORTANT]  
   > Zaleca się przetestowanie, czy ten interfejs API działa poprawnie z Twoim dostawcą, zanim skorzystasz z niego do zarządzania transakcjami. W przypadku problemów zachęcamy do kontaktu z osobami obsługującymi danego dostawcę bazy danych. 

2. Począwszy od wersji 2.1 implementacja przestrzeni nazw `System.Transactions` na platformie .NET Core nie obsługuje transakcji rozproszonych, dlatego nie można używać parametrów `TransactionScope` lub `CommittableTransaction` do koordynowania transakcji w wielu menedżerach zasobów.  
