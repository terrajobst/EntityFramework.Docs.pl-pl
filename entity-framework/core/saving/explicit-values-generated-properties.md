---
title: Ustawianie jawne wartości dla właściwości wygenerowanego - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Ustawianie jawne wartości dla właściwości wygenerowanego

Wygenerowanej właściwości jest właściwością, którego wartość jest generowany (albo przez EF lub baza danych) po dodane lub zaktualizowane jednostki. Zobacz [wygenerowane właściwości](../modeling/generated-properties.md) Aby uzyskać więcej informacji.

Mogą wystąpić sytuacje, w których chcesz ustawić jawną wartość dla wygenerowanej właściwości, zamiast plan wygenerowany.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) w witrynie GitHub.

## <a name="the-model"></a>Model

Model używany w tym artykule zawiera jeden `Employee` jednostki.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Zapisywanie podczas Dodaj jawną wartość

`Employee.EmploymentStarted` Skonfigurowano właściwości do wartości wygenerowanych przez bazę danych dla nowych jednostek (przy użyciu wartości domyślnej).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Poniższy kod wstawia dwóch pracowników do bazy danych.
* W pierwszym nie przypisano żadnej wartości do `Employee.EmploymentStarted` właściwości, więc pozostaje ustawioną wartość domyślną CLR `DateTime`.
* Dla drugiego, możemy ustawiono jawną wartość `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Dane wyjściowe pokazuje, że baza danych wygenerowała wartość dla pierwszej pracownika i użyto naszych jawną wartość dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Jawne wartości w kolumnach tożsamości serwera SQL

Konwencja `Employee.EmployeeId` właściwości jest magazynem wygenerowany `IDENTITY` kolumny.

W większości sytuacji podejście pokazanym powyżej będzie działać dla właściwości klucza. Jednak aby wstawić jawnej wartości do programu SQL Server `IDENTITY` kolumnę, musisz ręcznie włączyć `IDENTITY_INSERT` przed wywołaniem `SaveChanges()`.

> [!NOTE]  
> Mamy [żądanie funkcji](https://github.com/aspnet/EntityFramework/issues/703) na naszym zaległości się to automatycznie w ramach dostawcy programu SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Dane wyjściowe zawierają, że podane identyfikatory zostały zapisane w bazie danych.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Ustawienie wartości jawnej podczas aktualizacji

`Employee.LastPayRaise` Skonfigurowano właściwości do wartości wygenerowanych przez bazę danych podczas aktualizacji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Domyślnie EF Core spowoduje zgłoszenie wyjątku, jeśli użytkownik próbuje zapisać jawną wartość dla właściwości, która jest skonfigurowana do wygenerowania podczas aktualizacji. Aby tego uniknąć, należy rozwinąć niższy poziom metadanych interfejsu API i ustawić `AfterSaveBehavior` (jak pokazano powyżej).

> [!NOTE]  
> **Zmiany w programie EF Core 2.0:** w poprzednich wersjach zachowanie następującą Zapisz został steruje się za pomocą `IsReadOnlyAfterSave` flagi. Ta flaga zostały zdezaktualizowane i zastępuje `AfterSaveBehavior`.

Istnieje również wyzwalacza w bazie danych do generowania wartości dla `LastPayRaise` kolumny podczas `UPDATE` operacji.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Poniższy kod zwiększa wynagrodzenie dwóch pracowników w bazie danych.
* W pierwszym nie przypisano żadnej wartości do `Employee.LastPayRaise` właściwości, więc pozostanie ustawiona na wartość null.
* Dla drugiego możemy ustawiono jawną wartość tydzień temu (Zgłoś uaktualniania płatności wstecz).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Dane wyjściowe pokazuje, że baza danych wygenerowała wartość dla pierwszej pracownika i użyto naszych jawną wartość dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
