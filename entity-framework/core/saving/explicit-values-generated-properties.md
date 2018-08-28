---
title: Ustawienie jawne wartości dla wygenerowanych właściwości - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 00abef4d1208400ff68ced0a241b98b8dc9be5c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997856"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Ustawienie jawne wartości dla wygenerowanych właściwości

Wygenerowana właściwość jest właściwością, którego wartość jest generowana (albo przez EF lub bazy danych) podczas dodane lub zaktualizowane jednostki. Zobacz [wygenerowanych właściwości](../modeling/generated-properties.md) Aby uzyskać więcej informacji.

Mogą wystąpić sytuacje, w której chcesz ustawić jawną wartość dla wygenerowanej właściwości zamiast plan wygenerowany.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="the-model"></a>Model

Model używany w tym artykule zawiera pojedynczy `Employee` jednostki.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Zapisywanie jawną wartość podczas Dodaj

`Employee.EmploymentStarted` Skonfigurowano właściwości do wartości generowane przez bazę danych do nowych jednostek (przy użyciu wartości domyślnej).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Poniższy kod powoduje wstawienie dwóch pracowników do bazy danych.
* W pierwszym, zostanie przypisana żadna wartość, aby `Employee.EmploymentStarted` , dzięki czemu pozostanie ustawioną na wartość domyślną CLR dla `DateTime`.
* Z drugiej ustawimy mają jawne wartości `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Dane wyjściowe pokazują, że bazy danych wygenerowaną wartość dla pierwszego pracownika i użyto naszych jawną wartość dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Jawne wartości w kolumnach tożsamości serwera SQL

Zgodnie z Konwencją `Employee.EmployeeId` właściwość jest magazynem generowane `IDENTITY` kolumny.

W większości sytuacji to podejście pokazano powyżej będzie działać w przypadku właściwości klucza. Jednakże można wstawić jawnej wartości do programu SQL Server `IDENTITY` kolumnę, musisz ręcznie włączyć `IDENTITY_INSERT` przed wywołaniem `SaveChanges()`.

> [!NOTE]  
> Mamy [zgłoszenie dotyczące funkcji](https://github.com/aspnet/EntityFramework/issues/703) na naszej liście prac, aby to zrobić automatycznie przy użyciu dostawcy programu SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Dane wyjściowe pokazują, że podane identyfikatory zostały zapisane w bazie danych.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Ustawianie jawną wartość podczas aktualizacji

`Employee.LastPayRaise` Skonfigurowano właściwości do wartości generowane przez bazę danych podczas aktualizacji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Domyślnie program EF Core spowoduje zgłoszenie wyjątku, jeśli zostanie podjęta próba zapisania jawną wartość dla właściwości, który jest skonfigurowany do wygenerowania podczas aktualizacji. Aby tego uniknąć, należy do listy rozwijanej na niższym poziomie metadanych interfejsu API i ustaw `AfterSaveBehavior` (jak pokazano powyżej).

> [!NOTE]  
> **Zmiany w programie EF Core 2.0:** w poprzednich wersjach zachowanie wtórnym Zapisz został kontrolowane za pośrednictwem `IsReadOnlyAfterSave` flagi. Ta flaga został zamieniono przestarzały parametr i zastąpione przez `AfterSaveBehavior`.

Istnieje również wyzwalacza w bazie danych do generowania wartości dla `LastPayRaise` kolumny podczas `UPDATE` operacji.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Poniższy kod powoduje zwiększenie wynagrodzenia dwóch pracowników w bazie danych.
* W pierwszym, zostanie przypisana żadna wartość, aby `Employee.LastPayRaise` właściwości, dzięki czemu pozostanie ustawiony na wartość null.
* Dla drugiego firma Microsoft ustawiono jawną wartość jeden tydzień temu (podniesienie płatność sięga wstecz).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Dane wyjściowe pokazują, że bazy danych wygenerowaną wartość dla pierwszego pracownika i użyto naszych jawną wartość dla drugiego.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
