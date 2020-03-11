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
# <a name="using-transactions"></a>Korzystanie z transakcji

Transakcje pozwalają na wykonanie kilku operacji na bazie danych w sposób niepodzielny. Jeżeli transakcja zostanie zatwierdzona, wszystkie operacje zostaną pomyślnie wykonane na bazie danych. Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie wykonana na bazie danych.

> [!TIP]  
> [Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w pojedynczym wywołaniu `SaveChanges()` są stosowane w transakcji. Jeżeli jakakolwiek z tych zmian nie powiedzie się, transakcja zostanie wycofana i zmiany nie zostaną zapisane. Oznacza to, że `SaveChanges()` gwarantuje całkowite pomyślne lub pozostawienie niezmodyfikowanej bazy danych w przypadku wystąpienia błędu.

W przypadku większości zastosowań to domyślne zachowanie jest wystarczające. Transakcje powinny być sterowane ręcznie, tylko jeżeli taka konieczność wynika z wymagań danego zastosowania.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Aby rozpocząć, zatwierdzić i wycofać transakcje, można użyć interfejsu API `DbContext.Database`. Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytania LINQ wykonywane w jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługują transakcje. Niektórzy dostawcy mogą zgłaszać wyjątki lub nie wykonywać żadnych operacji po wywołaniu interfejsu API transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakcja międzykontekstowa (tylko relacyjne bazy danych)

Transakcje mogą być również współdzielone przez wiele wystąpień kontekstów. Ta funkcja jest dostępna tylko wtedy, gdy jest używany dostawca relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby udostępnić transakcję, konteksty muszą współdzielić zarówno `DbConnection`, jak i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalanie na dostarczanie połączenia z zewnątrz

Udostępnienie `DbConnection` wymaga możliwości przekazania połączenia do kontekstu podczas jego konstruowania.

Najprostszym sposobem na umożliwienie `DbConnection` być podano zewnętrznie, jest zaprzestanie korzystania z metody `DbContext.OnConfiguring` w celu skonfigurowania kontekstu i zewnętrznych tworzenia `DbContextOptions` i przekazania ich do konstruktora kontekstowego.

> [!TIP]  
> `DbContextOptionsBuilder` jest interfejsem API używanym w `DbContext.OnConfiguring` do konfigurowania kontekstu, dlatego można używać go zewnętrznie do tworzenia `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatywą jest utrzymywanie `DbContext.OnConfiguring`, ale zaakceptowanie `DbConnection` zapisywanych, a następnie używanych w `DbContext.OnConfiguring`.

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

Teraz możesz utworzyć wiele wystąpień kontekstu współużytkujących to samo połączenie. Następnie użyj interfejsu API `DbContext.Database.UseTransaction(DbTransaction)`, aby zarejestrować oba konteksty w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Korzystanie z zewnętrznych transakcji `DbTransaction` (tylko relacyjne bazy danych)

Korzystając z wielu metod dostępu do danych w relacyjnej bazie danych, można współdzielić transakcje z operacjami wykonywanymi przez te różne metody.

Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework Core w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Korzystanie z System. Transactions

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Istnieje możliwość użycia transakcji otoczenia, aby umożliwić koordynację na większą skalę.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Istnieje również możliwość zarejestrowania w transakcji jawnej.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia przestrzeni nazw System.Transactions  

1. Platforma EF Core korzysta z dostawców baz danych do implementowania obsługi przestrzeni nazw `System.Transactions`. Mimo że dostawcy ADO.NET często obsługują platformę .NET Framework, ten interfejs API został dopiero niedawno dodany do platformy EF Core. Jeżeli dostawca nie implementuje obsługi przestrzeni nazw `System.Transactions,` możliwe jest, że wywołania do tego interfejsu API zostaną zignorowane. SqlClient dla platformy .NET Core obsługuje je od wersji 2.1 w górę. Program SqlClient dla programu .NET Core 2,0 zgłosi wyjątek, jeśli spróbujesz użyć tej funkcji.

   > [!IMPORTANT]  
   > Zaleca się przetestowanie, czy ten interfejs API działa poprawnie z Twoim dostawcą, zanim skorzystasz z niego do zarządzania transakcjami. W przypadku problemów zachęcamy do kontaktu z osobami obsługującymi danego dostawcę bazy danych.

2. Począwszy od wersji 2,1, implementacja System. Transactions w programie .NET Core nie obejmuje obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` ani `CommittableTransaction` do koordynowania transakcji w wielu menedżerach zasobów.
