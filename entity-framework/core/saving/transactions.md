---
title: Transakcje — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 21a03f8915cba926d55f8b122ae585bd8946eeee
ms.sourcegitcommit: 5715caf923c3761e72b1b4ae589547584b459708
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/09/2018
ms.locfileid: "35250770"
---
# <a name="using-transactions"></a>Używanie transakcji

Transakcje zezwala na kilka operacji bazy danych mają być przetwarzane w sposób atomic. Transakcja została zatwierdzona, wszystkie operacje są pomyślnie zastosowana do bazy danych. Jeśli transakcja zostanie wycofana, żadna z operacji są stosowane do bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) w witrynie GitHub.

## <a name="default-transaction-behavior"></a>Domyślne zachowanie transakcji

Domyślnie, jeśli dostawca bazy danych obsługuje transakcje, wszystkie zmiany w jednym wywołaniu `SaveChanges()` są stosowane w ramach transakcji. W przypadku awarii zmian, transakcja zostanie wycofana i zmiany są stosowane do bazy danych. Oznacza to, że `SaveChanges()` jest gwarantowana całkowicie powiedzie się, lub pozostaw bazę danych, nie mają być modyfikowane w przypadku wystąpienia błędu.

W przypadku większości aplikacji to domyślne zachowanie jest wystarczająca. Transakcje powinien kontroli tylko ręcznie, jeśli wymagania aplikacji uznać za konieczne.

## <a name="controlling-transactions"></a>Kontrolowanie transakcji

Można użyć `DbContext.Database` interfejsu API, aby rozpocząć, zatwierdzenia i wycofywania transakcji. Poniższy przykład przedstawia dwa `SaveChanges()` operacje i LINQ zapytania wykonywane w ramach jednej transakcji.

Nie wszyscy dostawcy bazy danych obsługuje transakcji. Niektórzy dostawcy może zgłaszać lub pusta podczas transakcji są nazywane interfejsów API.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Między kontekstu transakcji (relacyjnych baz danych tylko)

Możesz również udostępniać w wielu wystąpieniach kontekstu transakcji. Ta funkcja jest dostępna tylko w przypadku korzystania z dostawcy relacyjnej bazy danych, ponieważ wymaga użycia `DbTransaction` i `DbConnection`, które są specyficzne dla relacyjnych baz danych.

Aby udostępnić transakcji, kontekstów musi udostępniać zarówno `DbConnection` i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Zezwalaj na połączenia należy podać zewnętrznie

Udostępnianie `DbConnection` wymaga możliwości przekazania połączenie w kontekście podczas jego tworzenia.

Najprostszym sposobem, aby umożliwić `DbConnection` zewnętrznie zostać podany, jest zatrzymanie przy użyciu `DbContext.OnConfiguring` metody zewnętrznie utworzony i skonfigurowany kontekst `DbContextOptions` i przekazać je do konstruktora kontekstu.

> [!TIP]  
> `DbContextOptionsBuilder` jest używany w API `DbContext.OnConfiguring` można skonfigurować kontekstu, teraz będzie go zewnętrznie użyć do utworzenia `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatywą jest nadal używać `DbContext.OnConfiguring`, ale akceptują `DbConnection` które jest zapisywane i następnie używane w `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Połączenie udziału i transakcji

Teraz możesz utworzyć wiele wystąpień kontekstu, które współużytkują to samo połączenie. Następnie użyj `DbContext.Database.UseTransaction(DbTransaction)` interfejsu API można zarejestrować zarówno kontekstów w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Za pomocą zewnętrznego DbTransactions (relacyjnych baz danych tylko)

Korzystając z wielu technologii dostępu do danych relacyjnej bazy danych, można udostępniać transakcji między operacje wykonywane przez te różnych technologii.

Poniższy przykład przedstawia sposób wykonywania operacji ADO.NET SqlClient i Entity Framework podstawowej funkcjonalności w tej samej transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Przy użyciu System.Transactions

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 2.1.

Istnieje możliwość użycia otoczenia transakcji, aby koordynować w większego zakresu.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Istnieje również możliwość można zarejestrować w jawnych transakcji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>Ograniczenia obszaru nazw System.Transactions  

1. Podstawowe EF zależy od dostawcy bazy danych, obsługa System.Transactions. Mimo że pomocy technicznej jest dość często wśród dostawców ADO.NET dla programu .NET Framework, interfejsu API tylko został ostatnio dodany do platformy .NET Core i dlatego nie jest tak szerokie pomocy technicznej. Jeśli dostawca nie implementuje obsługę System.Transactions, istnieje możliwość, że wywołań do tych interfejsów API będą ignorowane całkowicie. Klient SQL dla platformy .NET Core obsługuje z 2.1 lub nowszej. SqlClient programu .NET Core 2.0 spowoduje zgłoszenie wyjątku z próby użycia funkcji. 

   > [!IMPORTANT]  
   > Zaleca się przetestowanie czy interfejsu API poprawne działanie u dostawcy przed polegać na niej zarządzania transakcji. Zachęcamy do kontaktowania się z Element utrzymujący dostawcy bazy danych, jeśli jej nie ma. 

2. Począwszy od wersji 2.1 implementacja System.Transactions w .NET Core nie ma obsługi transakcji rozproszonych, dlatego nie można używać `TransactionScope` lub `CommitableTransaction` aby koordynować transakcje w wielu menedżerów zasobów. 
