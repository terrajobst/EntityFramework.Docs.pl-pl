---
title: Ustawianie wartości jawnych dla wygenerowanych właściwości — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 43c4ab3c2a60645cdeff2a6cc40ce979f832f2fd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417570"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Ustawianie wartości jawnych dla właściwości wygenerowanych

Wygenerowana właściwość jest właściwością, której wartość jest generowana (przez EF lub bazę danych) po dodaniu i/lub zaktualizowaniu jednostki. Zobacz [wygenerowane właściwości, aby](../modeling/generated-properties.md) uzyskać więcej informacji.

Mogą wystąpić sytuacje, w których chcesz ustawić jawną wartość dla wygenerowanej właściwości, a nie o jeden wygenerowany.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) przykład artykułu na GitHub.

## <a name="the-model"></a>Model

Model używany w tym artykule zawiera jedną `Employee` jednostkę.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Zapisywanie jawnej wartości podczas dodawania

Właściwość `Employee.EmploymentStarted` jest skonfigurowana tak, aby były generowane przez bazę danych dla nowych jednostek (przy użyciu wartości domyślnej).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Poniższy kod wstawia dwóch pracowników do bazy danych.

* W przypadku pierwszej nie jest `Employee.EmploymentStarted` przypisana żadna wartość do właściwości, więc `DateTime`pozostaje ustawiona na wartość domyślną CLR dla .
* Dla drugiego, mamy ustawić wyraźną `1-Jan-2000`wartość .

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Dane wyjściowe pokazuje, że baza danych wygenerowała wartość dla pierwszego pracownika, a nasza jawna wartość została użyta dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Jawne wartości w kolumnach TOŻSAMOŚCI programu SQL Server

Zgodnie z `Employee.EmployeeId` konwencją właściwość jest kolumną wygenerowaną `IDENTITY` przez magazyn.

W większości sytuacji podejście pokazane powyżej będzie działać dla kluczowych właściwości. Jednak aby wstawić jawne `IDENTITY` wartości do kolumny programu `IDENTITY_INSERT` SQL `SaveChanges()`Server, należy ręcznie włączyć przed wywołaniem .

> [!NOTE]  
> Mamy [żądanie funkcji](https://github.com/aspnet/EntityFramework/issues/703) na nasze zaległości, aby to zrobić automatycznie w ramach dostawcy programu SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Dane wyjściowe pokazują, że dostarczone identyfikatory zostały zapisane w bazie danych.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Ustawianie jawnej wartości podczas aktualizacji

Właściwość `Employee.LastPayRaise` jest skonfigurowana do wartości generowanych przez bazę danych podczas aktualizacji.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Domyślnie EF Core zgłosić wyjątek, jeśli spróbujesz zapisać jawną wartość dla właściwości, która jest skonfigurowana do generowania podczas aktualizacji. Aby tego uniknąć, należy rozwijać do niższego poziomu `AfterSaveBehavior` interfejsu API metadanych i ustawić (jak pokazano powyżej).

> [!NOTE]  
> **Zmiany w EF Core 2.0:** W poprzednich wersjach zachowanie po zapisaniu było kontrolowane przez flagę. `IsReadOnlyAfterSave` Ta flaga została przestarzała i zastąpiona przez `AfterSaveBehavior`.

Istnieje również wyzwalacz w bazie danych `LastPayRaise` do `UPDATE` generowania wartości dla kolumny podczas operacji.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Poniższy kod zwiększa wynagrodzenie dwóch pracowników w bazie danych.

* Dla pierwszego nie wartość jest `Employee.LastPayRaise` przypisana do właściwości, więc pozostaje ustawiona na wartość null.
* Po drugie, ustawiliśmy wyraźną wartość sprzed tygodnia (z powrotem datuje podwyżkę płac).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Dane wyjściowe pokazuje, że baza danych wygenerowała wartość dla pierwszego pracownika, a nasza jawna wartość została użyta dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
