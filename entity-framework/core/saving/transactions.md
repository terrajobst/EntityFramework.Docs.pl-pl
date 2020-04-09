---
title: Transakcje - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417556"
---
# <a name="using-transactions"></a>Korzystanie z transakcji

Transakcje umożliwiają kilka operacji bazy danych do przetworzenia w sposób niepodzielny. Jeśli transakcja zostanie zatwierdzona, wszystkie operacje są pomyślnie stosowane do bazy danych. Jeśli transakcja zostanie wycofana, żadna z operacji nie są stosowane do bazy danych.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) przykład artykułu na GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie `SaveChanges()` zmiany w jednym wywołaniu są stosowane w transakcji. Jeśli którakolwiek ze zmian nie powiedzie się, transakcja jest przywracana i żadna ze zmian nie jest stosowana do bazy danych. Oznacza to, że `SaveChanges()` jest gwarantowane albo całkowicie zakończyć się pomyślnie lub pozostawić bazy danych bez modyfikacji, jeśli wystąpi błąd.

W przypadku większości aplikacji to domyślne zachowanie jest wystarczające. Transakcje należy kontrolować ręcznie tylko wtedy, gdy wymagania aplikacji uznają to za konieczne.

## <a name="controlling-transactions"></a>Kontrola transakcji

Za pomocą `DbContext.Database` interfejsu API można rozpoczynać, zatwierdzać i wycofywać transakcje. W poniższym `SaveChanges()` przykładzie przedstawiono dwie operacje i kwerendę LINQ wykonywane w pojedynczej transakcji.

Nie wszyscy dostawcy baz danych obsługują transakcje. Niektórzy dostawcy mogą zgłaszać lub nie op, gdy są wywoływane interfejsy API transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakcja między kontekstem (tylko relacyjne bazy danych)

Można również udostępnić transakcję w wielu wystąpieniach kontekstu. Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy `DbTransaction` `DbConnection`relacyjnej bazy danych, ponieważ wymaga użycia i , które są specyficzne dla relacyjnych baz danych.

Aby udostępnić transakcję, konteksty `DbConnection` muszą `DbTransaction`współużytkować zarówno a, jak i .

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalaj na podłączanie na zewnątrz

Udostępnianie `DbConnection` wymaga możliwość przekazania połączenia do kontekstu podczas konstruowania go.

Najprostszym sposobem, `DbConnection` aby umożliwić być zewnętrznie pod `DbContext.OnConfiguring` warunkiem, jest, aby zatrzymać `DbContextOptions` przy użyciu metody, aby skonfigurować kontekst i zewnętrznie utworzyć i przekazać je do konstruktora kontekstu.

> [!TIP]  
> `DbContextOptionsBuilder`to interfejs API `DbContext.OnConfiguring` użyty do skonfigurowania kontekstu, teraz będzie go `DbContextOptions`używać zewnętrznie do tworzenia .

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatywą jest, `DbContext.OnConfiguring`aby zachować `DbConnection` przy użyciu , `DbContext.OnConfiguring`ale zaakceptować, który jest zapisywany, a następnie używany w .

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

### <a name="share-connection-and-transaction"></a>Udostępnij połączenie i transakcję

Teraz można utworzyć wiele wystąpień kontekstu, które współużytkuje to samo połączenie. Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API, aby zarejestrować oba konteksty w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Korzystanie z zewnętrznych dbtransakcje (tylko relacyjne bazy danych)

Jeśli używasz wielu technologii dostępu do danych, aby uzyskać dostęp do relacyjnej bazy danych, możesz chcieć udostępnić transakcję między operacjami wykonywanymi przez te różne technologie.

W poniższym przykładzie pokazano, jak wykonać ADO.NET operacji SqlClient i operacji Core programu Entity Framework w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Korzystanie z system.transactions

> [!NOTE]  
> Ta funkcja jest nowa w EF Core 2.1.

Istnieje możliwość użycia transakcji otoczenia, jeśli trzeba koordynować w większym zakresie.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Istnieje również możliwość zarejestrowania się w jawnej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia systemowych.Transakcji  

1. EF Core opiera się na dostawców bazy danych do wdrożenia obsługi system.Transactions. Mimo że obsługa jest dość powszechne wśród dostawców ADO.NET dla platformy .NET Framework, interfejs API został niedawno dodany do platformy .NET Core i dlatego obsługa nie jest tak powszechna. Jeśli dostawca nie implementuje obsługi system.Transactions, jest możliwe, że wywołania tych interfejsów API zostaną całkowicie zignorowane. SqlClient for .NET Core obsługuje go od 2.1. SqlClient dla .NET Core 2.0 zda wyjątek, jeśli spróbujesz użyć tej funkcji.

   > [!IMPORTANT]  
   > Zaleca się, aby przetestować, że interfejs API zachowuje się poprawnie z dostawcą, zanim będzie polegać na nim do zarządzania transakcjami. Zachęcamy do skontaktowania się z opiekunem dostawcy bazy danych, jeśli tak nie jest.

2. W wersji 2.1 implementacja System.Transactions w .NET Core nie obejmuje obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` ani `CommittableTransaction` koordynować transakcji między wieloma menedżerami zasobów.
