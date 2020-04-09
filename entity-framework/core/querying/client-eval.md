---
title: Ocena klienta a serwer — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417761"
---
# <a name="client-vs-server-evaluation"></a>Ocena klienta a serwer

Zgodnie z ogólną zasadą Entity Framework Core próbuje ocenić kwerendę na serwerze w jak największej ilości. EF Core konwertuje części kwerendy na parametry, które można ocenić po stronie klienta. Pozostała część kwerendy (wraz z wygenerowanymi parametrami) jest podana do dostawcy bazy danych w celu określenia równoważnej kwerendy bazy danych do oceny na serwerze. EF Core obsługuje częściową ocenę klienta w projekcji najwyższego poziomu (zasadniczo ostatnie wywołanie `Select()`). Jeśli projekcji najwyższego poziomu w kwerendzie nie można przetłumaczyć na serwer, EF Core pobierze wszystkie wymagane dane z serwera i oceni pozostałe części kwerendy na kliencie. Jeśli EF Core wykrywa wyrażenie, w dowolnym miejscu innym niż projekcji najwyższego poziomu, które nie mogą być tłumaczone na serwer, a następnie zgłasza wyjątek środowiska wykonawczego. Zobacz, [jak działa kwerenda,](xref:core/querying/how-query-works) aby zrozumieć, jak EF Core określa, czego nie można przetłumaczyć na serwer.

> [!NOTE]
> Przed wersją 3.0, Entity Framework Core obsługiwane oceny klienta w dowolnym miejscu w kwerendzie. Aby uzyskać więcej informacji, zobacz [sekcję Poprzednie wersje](#previous-versions).

> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.

## <a name="client-evaluation-in-the-top-level-projection"></a>Ocena klienta w projekcji najwyższego poziomu

W poniższym przykładzie metoda pomocnika służy do standaryzacji adresów URL blogów, które są zwracane z bazy danych programu SQL Server. Ponieważ dostawca programu SQL Server nie ma wglądu w sposób implementacji tej metody, nie można przetłumaczyć jej na język SQL. Wszystkie inne aspekty kwerendy są oceniane w bazie `URL` danych, ale przekazywanie zwrócone za pośrednictwem tej metody odbywa się na kliencie.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Nieobsługiwała ocena klienta

Chociaż ocena klienta jest przydatna, może to czasami spowodować niską wydajność. Należy wziąć pod uwagę następujące zapytanie, w którym metoda pomocnika jest teraz używana w filtrze where. Ponieważ filtr nie można zastosować w bazie danych, wszystkie dane muszą być pobierane do pamięci, aby zastosować filtr na kliencie. Na podstawie filtru i ilości danych na serwerze ocena klienta może spowodować niską wydajność. Tak Entity Framework Core blokuje taką ocenę klienta i zgłasza wyjątek środowiska uruchomieniowego.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Jawna ocena klienta

W niektórych przypadkach, takich jak:

- Ilość danych jest niewielka, dzięki czemu ocena na kliencie nie wiąże się z ogromną karą za wydajność.
- Używany operator LINQ nie ma tłumaczenia po stronie serwera.

W takich przypadkach można jawnie zdecydować się na `AsEnumerable` `ToList` ocenę`AsAsyncEnumerable` `ToListAsync` klienta, wywołując metody takie jak lub (lub asynchronicznie). Za `AsEnumerable` pomocą będzie przesyłania strumieniowego `ToList` wyników, ale przy użyciu spowodowałoby buforowanie przez utworzenie listy, która również zajmuje dodatkową pamięć. Chociaż jeśli wyliczasz wiele razy, przechowywanie wyników na liście pomaga więcej, ponieważ istnieje tylko jedno zapytanie do bazy danych. W zależności od konkretnego użycia należy ocenić, która metoda jest bardziej przydatna w przypadku.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Potencjalny wyciek pamięci w ocenie klienta

Ponieważ translacji kwerendy i kompilacji są kosztowne, EF Core buforuje skompilowany plan kwerend. Pełnomocnik buforowane może używać kodu klienta podczas wykonywania oceny klienta projekcji najwyższego poziomu. EF Core generuje parametry dla części drzewa oceniane przez klienta i ponownie używa planu kwerend, zastępując wartości parametrów. Ale niektórych stałych w drzewie wyrażeń nie można przekonwertować na parametry. Jeśli pełnomocnik buforowane zawiera takie stałe, a następnie te obiekty nie mogą być zbierane śmieci, ponieważ są one nadal odwołuje. Jeśli taki obiekt zawiera DbContext lub innych usług w nim, a następnie może spowodować użycie pamięci aplikacji rośnie w czasie. To zachowanie jest zazwyczaj oznaką przecieku pamięci. EF Core zgłasza wyjątek, gdy natknie się na stałe typu, który nie może być mapowany przy użyciu bieżącego dostawcy bazy danych. Typowe przyczyny i ich rozwiązania są następujące:

- **Przy użyciu metody wystąpienia:** Podczas korzystania z metod wystąpienia w projekcji klienta, drzewo wyrażeń zawiera stałą wystąpienia. Jeśli metoda nie używa żadnych danych z wystąpienia, należy rozważyć uczynienie metody statyczne. Jeśli potrzebujesz danych wystąpienia w treści metody, następnie przekazać określone dane jako argument do metody.
- **Przekazywanie stałych argumentów do metody:** Ten `this` przypadek występuje zazwyczaj przy użyciu w argument do metody klienta. Należy rozważyć podzielenie argumentu na wiele argumentów skalarnych, które mogą być mapowane przez dostawcę bazy danych.
- **Inne stałe**: Jeśli stała jest natknąć się w każdym innym przypadku, a następnie można ocenić, czy stała jest potrzebna w przetwarzaniu. Jeśli jest to konieczne, aby mieć stałą lub jeśli nie można użyć rozwiązania z powyższych przypadków, a następnie utworzyć zmienną lokalną do przechowywania wartości i używać zmiennej lokalnej w kwerendzie. EF Core przekonwertuje zmienną lokalną na parametr.

## <a name="previous-versions"></a>Poprzednie wersje

Poniższa sekcja dotyczy wersji EF Core przed 3.0.

Starsze wersje EF Core obsługiwane oceny klienta w dowolnej części kwerendy — nie tylko projekcji najwyższego poziomu. Dlatego kwerendy podobne do tych zaksięgowanych w sekcji [Nieobsługiwany klient oceny](#unsupported-client-evaluation) działały poprawnie. Ponieważ to zachowanie może spowodować niezauważone problemy z wydajnością, EF Core rejestrowane ostrzeżenie oceny klienta. Aby uzyskać więcej informacji na temat wyświetlania danych wyjściowych rejestrowania, zobacz [Rejestrowanie](xref:core/miscellaneous/logging).

Opcjonalnie EF Core pozwoliło na zmianę domyślnego zachowania albo zgłosić wyjątek lub nic nie robić podczas wykonywania oceny klienta (z wyjątkiem w projekcji). Zachowanie zgłaszania wyjątków sprawi, że będzie podobny do zachowania w 3.0. Aby zmienić zachowanie, należy skonfigurować ostrzeżenia podczas konfigurowania opcji kontekstu — zazwyczaj w `DbContext.OnConfiguring`, lub w `Startup.cs` jeśli używasz ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
