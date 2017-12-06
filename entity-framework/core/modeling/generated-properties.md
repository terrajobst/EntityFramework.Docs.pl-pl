---
title: "Wygenerowany wartości - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 2d79bf1339ebe522c39fe8971d908c30e1f4dca0
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="generated-values"></a>Wygenerowany wartości

## <a name="value-generation-patterns"></a>Wartość wzorców generowania

Istnieją trzy wzorców generowania wartości, które mogą być używane dla właściwości.

### <a name="no-value-generation"></a>Generowanie nie wartości

Generowanie wartości nie oznacza, że będzie zawsze podać prawidłową wartość do zapisania w bazie danych. To nieprawidłowa wartość muszą być przypisane do nowych jednostek zanim zostaną one dodane do kontekstu.

### <a name="value-generated-on-add"></a>Dodaj wartość wygenerowaną na

Wartość wygenerowana na dodawanie oznacza, że generowany wartość dla nowych jednostek.

W zależności od używanego dostawcy bazy danych wartości mogą być generowane po stronie klienta EF lub w bazie danych. Jeśli wartość jest generowany przez bazę danych, następnie EF może przypisać wartości tymczasowej po dodaniu jednostkę do kontekstu. Tej wartości tymczasowej następnie zostanie zastąpiony wartością generowany przez bazę danych podczas `SaveChanges()`.

Jeśli dodasz jednostkę do kontekstu, który ma wartość przypisana do właściwości EF będzie podejmować próby wstawienia tej wartości, a nie generuje nowy. Właściwość została uznana za wartość przypisane, jeśli nie jest przypisany CLR wartość domyślna (`null` dla `string`, `0` dla `int`, `Guid.Empty` dla `Guid`itp.). Aby uzyskać więcej informacji, zobacz [jawnej wartości dla właściwości wygenerowanego](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Jak jest generowany wartości dla jednostek dodano będzie zależeć od używany dostawca bazy danych. Dostawcy bazy danych może automatycznie skonfigurować Generowanie wartości dla niektórych typów właściwości, ale inne może wymagać można skonfigurować ręcznie, jak jest generowany wartość.
>
> Na przykład, korzystając z programu SQL Server, wartości będą automatycznie generowane dla `GUID` właściwości (przy użyciu algorytmu GUID sekwencyjnych programu SQL Server). Jednak jeśli użytkownik określi, że `DateTime` właściwości jest generowany na dodać, a następnie należy skonfigurować to rozwiązanie dla wartości do wygenerowania. Jest jeden ze sposobów, aby skonfigurować domyślną wartość `GETDATE()`, zobacz [wartości domyślne](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Wartość wygenerowana na dodania lub zaktualizowania

Wartość wygenerowana przy dodawaniu lub aktualizacji oznacza, że nowa wartość jest generowany za każdym razem, gdy zapisaniu rekordu (insert lub update).

Podobnie jak `value generated on add`, jeśli określono wartości dla właściwości dla nowo dodanego wystąpienia jednostki, że wartość zostanie wstawiony zamiast wartości generowany. Istnieje również możliwość ustawienia jawną wartość podczas aktualizowania. Aby uzyskać więcej informacji, zobacz [jawnej wartości dla właściwości wygenerowanego](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Jak jest generowany wartości dla jednostek dodany i zaktualizowane będzie zależeć od używany dostawca bazy danych. Dostawcy bazy danych może automatycznie Instalator Generowanie wartości dla niektórych typów właściwości, a inne będzie można skonfigurować ręcznie, jak wartość jest generowany wymagają.
>
> Na przykład w przypadku korzystania z programu SQL Server `byte[]` właściwości, które są ustawione, tak jak w dodać lub zaktualizować i oznaczone jako tokeny współbieżności będzie Instalatorowi `rowversion` typów danych - tak, aby wartości zostaną wygenerowane w bazie danych. Jednak jeśli użytkownik określi, że `DateTime` właściwości jest generowany na dodać lub zaktualizować, a następnie należy skonfigurować to rozwiązanie dla wartości do wygenerowania. Jest jeden ze sposobów, aby skonfigurować domyślną wartość `GETDATE()` (zobacz [wartości domyślne](relational/default-values.md)) do generowania wartości dla nowych wierszy. Następnie można użyć wyzwalacza bazy danych do generowania wartości podczas aktualizacji (na przykład następujący przykład wyzwalacz).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konwencje

Według Konwencji jest liczbą całkowitą lub identyfikator GUID typu danych kluczy podstawowych będzie Instalatora wartościami wygenerowany na Dodaj. Wszystkie inne właściwości będzie Instalatora nie generacji wartość.

## <a name="data-annotations"></a>Adnotacji danych

### <a name="no-value-generation-data-annotations"></a>Generowanie nie wartości (adnotacji danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Wartość wygenerowana na dodawanie (adnotacji danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Umożliwia to po prostu EF wiedzieć, czy wartości są generowane dla jednostek dodany, nie gwarantuje, że EF umożliwią skonfigurowanie konkretny mechanizm do generowania wartości. Zobacz [dodać wartość wygenerowaną na](#value-generated-on-add) sekcji, aby uzyskać więcej informacji.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Dodaj wartość wygenerowaną na lub aktualizacji (adnotacji danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Umożliwia to po prostu EF wiedzieć, czy wartości są generowane dla jednostek dodane lub zaktualizowane, nie gwarantuje, że EF umożliwią skonfigurowanie konkretny mechanizm do generowania wartości. Zobacz [wartość wygenerowaną na dodania lub zaktualizowania](#value-generated-on-add-or-update) sekcji, aby uzyskać więcej informacji.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby zmienić wzorzec generowania wartości dla danej właściwości można Użyj interfejsu API Fluent.

### <a name="no-value-generation-fluent-api"></a>Generowanie nie wartości (interfejsu API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Wartość wygenerowana na dodawanie (interfejsu API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`po prostu umożliwia EF wiedzieć, czy wartości są generowane dla jednostek dodany, nie gwarantuje, że EF umożliwią skonfigurowanie konkretny mechanizm do generowania wartości.  Zobacz [dodać wartość wygenerowaną na](#value-generated-on-add) sekcji, aby uzyskać więcej informacji.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Dodaj wartość wygenerowaną na lub aktualizacji (interfejsu API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Umożliwia to po prostu EF wiedzieć, czy wartości są generowane dla jednostek dodane lub zaktualizowane, nie gwarantuje, że EF umożliwią skonfigurowanie konkretny mechanizm do generowania wartości. Zobacz [wartość wygenerowaną na dodania lub zaktualizowania](#value-generated-on-add-or-update) sekcji, aby uzyskać więcej informacji.
