---
title: Transakcje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 952cb891d145a47666f1d506ec00f066be9f245d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654755"
---
# <a name="using-transactions"></a>Korzystanie z transakcji

Transakcje umożliwiają wykonywanie wielu operacji w bazie danych w sposób niepodzielny. Jeśli transakcja została zatwierdzona, wszystkie operacje zostaną pomyślnie zastosowane do bazy danych. Jeśli transakcja zostanie wycofana, żadna z operacji nie zostanie zastosowana do bazy danych.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w pojedynczym wywołaniu `SaveChanges()` są stosowane w transakcji. Jeśli jakakolwiek zmiana nie powiedzie się, transakcja zostanie wycofana i żadna zmiana nie zostanie zastosowana do bazy danych. Oznacza to, że `SaveChanges()` gwarantuje całkowite pomyślne lub pozostawienie niezmodyfikowanej bazy danych w przypadku wystąpienia błędu.

W przypadku większości aplikacji to zachowanie domyślne jest wystarczające. Należy ręcznie kontrolować transakcje tylko wtedy, gdy wymagania dotyczące aplikacji uznają to za konieczne.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Aby rozpocząć, zatwierdzić i wycofać transakcje, można użyć interfejsu API `DbContext.Database`. Poniższy przykład przedstawia dwie operacje `SaveChanges()` i zapytania LINQ wykonywane w jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługują transakcje. Niektórzy dostawcy mogą zgłosić lub nie ponowić działania w przypadku wywołania interfejsów API transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakcja między kontekstami (tylko relacyjne bazy danych)

Można również udostępnić transakcję w wielu wystąpieniach kontekstu. Ta funkcja jest dostępna tylko wtedy, gdy jest używany dostawca relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby udostępnić transakcję, konteksty muszą współdzielić zarówno `DbConnection`, jak i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalaj na połączenie z zewnątrz

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

### <a name="share-connection-and-transaction"></a>Udostępnianie połączenia i transakcji

Teraz można utworzyć wiele wystąpień kontekstu, które współużytkują to samo połączenie. Następnie użyj interfejsu API `DbContext.Database.UseTransaction(DbTransaction)`, aby zarejestrować oba konteksty w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Korzystanie z zewnętrznych baz transakcji (tylko relacyjne bazy danych)

W przypadku korzystania z wielu technologii dostępu do danych w celu uzyskania dostępu do relacyjnej bazy danych warto udostępnić transakcję między operacjami wykonywanymi przez te różne technologie.

Poniższy przykład pokazuje, jak wykonać operację ADO.NET i operację Entity Framework Core w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Korzystanie z System. Transactions

> [!NOTE]  
> Ta funkcja jest nowa w EF Core 2,1.

Można używać transakcji otoczenia, jeśli trzeba skoordynować w większym zakresie.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Istnieje również możliwość rejestrowania w jawnej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia dotyczące elementu System. Transactions  

1. EF Core opiera się na dostawcach baz danych w celu zaimplementowania obsługi dla elementu System. Transactions. Mimo że pomoc techniczna jest dość wspólna wśród dostawców ADO.NET .NET Framework, interfejs API został niedawno dodany do programu .NET Core i dlatego pomoc techniczna nie jest tak szeroka. Jeśli dostawca nie implementuje obsługi dla elementu System. Transactions, istnieje możliwość, że wywołania tych interfejsów API zostaną całkowicie zignorowane. Klient SqlClient dla platformy .NET Core obsługuje go od 2,1 do wewnątrz. Program SqlClient dla programu .NET Core 2,0 zgłosi wyjątek, jeśli spróbujesz użyć tej funkcji.

   > [!IMPORTANT]  
   > Zaleca się przetestowanie, czy interfejs API działa prawidłowo z dostawcą, zanim będzie on używany do zarządzania transakcjami. Zalecamy skontaktowanie się z działem IT dostawcy bazy danych, jeśli nie.

2. Począwszy od wersji 2,1, implementacja System. Transactions w programie .NET Core nie obejmuje obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` ani `CommittableTransaction` do koordynowania transakcji w wielu menedżerach zasobów.
