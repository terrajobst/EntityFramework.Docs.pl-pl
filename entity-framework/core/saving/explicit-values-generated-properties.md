---
title: Ustawianie wartości jawnych dla wygenerowanych właściwości — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: d6aa9a0a9ce34e09a39026ad7ea9195b6777858c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197856"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Ustawianie wartości jawnych dla wygenerowanych właściwości

Wygenerowana Właściwość to właściwość, której wartość jest generowana (przez EF lub bazę danych), gdy jednostka zostanie dodana i/lub zaktualizowana. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](../modeling/generated-properties.md) .

Mogą wystąpić sytuacje, w których chcesz ustawić wartość jawną dla wygenerowanej właściwości, zamiast wygenerowania jednego.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="the-model"></a>Model

Model używany w tym artykule zawiera jedną `Employee` jednostkę.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Zapisywanie wartości jawnej podczas dodawania

`Employee.EmploymentStarted` Właściwość jest skonfigurowana tak, aby wartości wygenerowały przez bazę danych dla nowych jednostek (przy użyciu wartości domyślnej).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Poniższy kod wstawia dwóch pracowników do bazy danych programu.
* Dla pierwszej, żadna wartość nie jest przypisana `Employee.EmploymentStarted` do właściwości, więc pozostaje ustawiona na wartość domyślną środowiska CLR dla `DateTime`.
* Dla drugiej ustawimy wartość `1-Jan-2000`jawną.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Dane wyjściowe pokazują, że baza danych wygenerowała wartość pierwszego pracownika, a nasza wartość została użyta w drugim.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Jawne wartości do kolumn tożsamości SQL Server

Według Konwencji `Employee.EmployeeId` właściwość jest kolumną wygenerowaną `IDENTITY` przez magazyn.

W większości sytuacji wskazane powyżej podejście będzie działało dla właściwości klucza. Aby jednak wstawić wartości jawne do kolumny SQL Server `IDENTITY` , musisz ręcznie włączyć `IDENTITY_INSERT` przed wywołaniem `SaveChanges()`.

> [!NOTE]  
> Mamy żądanie dotyczące [funkcji](https://github.com/aspnet/EntityFramework/issues/703) w naszym zaległości, aby to zrobić automatycznie w ramach dostawcy SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Dane wyjściowe pokazują, że podane identyfikatory zostały zapisane w bazie danych.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Ustawianie jawnej wartości podczas aktualizacji

`Employee.LastPayRaise` Właściwość jest skonfigurowana tak, aby wartości były generowane przez bazę danych podczas aktualizacji.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Domyślnie EF Core zgłosi wyjątek, jeśli spróbujesz zapisać wartość jawną dla właściwości, która jest skonfigurowana do generowania podczas aktualizacji. Aby tego uniknąć, należy rozwinąć do interfejsu API metadanych niższego poziomu i ustawić `AfterSaveBehavior` (jak pokazano powyżej).

> [!NOTE]  
> **Zmiany w EF Core 2,0:** W poprzednich wersjach zachowanie po zapisaniu było kontrolowane przez `IsReadOnlyAfterSave` flagę. Ta flaga jest przestarzała i zastąpiona przez `AfterSaveBehavior`.

W bazie danych istnieje również wyzwalacz służący do generowania wartości dla `LastPayRaise` kolumny podczas `UPDATE` operacji.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Poniższy kod zwiększa wynagrodzenie dwóch pracowników w bazie danych.
* Dla pierwszej, żadna wartość nie jest przypisana `Employee.LastPayRaise` do właściwości, więc pozostanie ustawiona na wartość null.
* Dla drugiej ustawimy jawną wartość jednego tygodnia temu (Wstecz Datowanie Zgłoś płatność).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Dane wyjściowe pokazują, że baza danych wygenerowała wartość pierwszego pracownika, a nasza wartość została użyta w drugim.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
