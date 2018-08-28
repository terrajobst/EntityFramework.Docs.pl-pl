---
title: Generowane wartości - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: a3656eb1d2dc79ceead04e3a142a58e8afb3cbce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996780"
---
# <a name="generated-values"></a>Generowane wartości

## <a name="value-generation-patterns"></a>Wzorce Generowanie wartości

Istnieją trzy wzorce Generowanie wartości, które mogą służyć do właściwości.

### <a name="no-value-generation"></a>Generowanie wartości nie jest

Generowanie wartość nie oznacza, że zawsze będzie podać prawidłową wartość do zapisania w bazie danych. To prawidłową wartość można przypisać nowe jednostki, zanim zostaną one dodane do kontekstu.

### <a name="value-generated-on-add"></a>Dodaj wartość wygenerowano

Wartość wygenerowano Dodaj oznacza, że wygenerowaną wartość dla nowych jednostek.

W zależności od używanego dostawcy bazy danych wartości mogą być generowane po stronie klienta lub w bazie danych EF. Jeśli wartość jest generowana przez bazę danych, następnie EF może przypisać wartości tymczasowej po dodaniu elementu do kontekstu. Tej wartości tymczasowej następnie zostanie zastąpiona przez wartości bazy danych, wygenerowane podczas `SaveChanges()`.

Jeśli dodasz jednostki do kontekstu, który ma wartość przypisana do właściwości EF będzie podejmować próby Wstaw tę wartość, zamiast generować nową. Właściwość została uznana za wartości przypisanej, jeśli nie jest przypisana wartość domyślna CLR (`null` dla `string`, `0` dla `int`, `Guid.Empty` dla `Guid`itp.). Aby uzyskać więcej informacji, zobacz [jawne wartości dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Jak wartość jest generowana dla jednostek dodano będzie zależeć od używanego dostawcy bazy danych. Dostawcy baz danych może automatycznie skonfigurować Generowanie wartości dla niektórych typów właściwości, ale innych może być konieczne, należy ręcznie skonfigurować, jak jest generowany wartość.
>
> Na przykład podczas korzystania z programu SQL Server, wartości będą automatycznie generowane dla `GUID` właściwości (przy użyciu programu SQL Server algorytm sekwencyjny identyfikatora GUID). Jednak jeśli określono `DateTime` właściwość jest generowany na dodać, a następnie należy skonfigurować sposób wartości do wygenerowania. Jednym ze sposobów, aby to zrobić, to aby skonfigurować domyślną wartość `GETDATE()`, zobacz [wartości domyślne](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Wartość wygenerowaną na dodawanie lub aktualizowanie

Wartość generowane przy dodawaniu lub aktualizacja oznacza, że nowa wartość jest generowany za każdym razem, gdy rekord zostanie zapisany (insert nebo update).

Podobnie jak `value generated on add`, jeśli określona wartość właściwości na nowo dodane wystąpienie jednostki, że wartość zostanie wstawiony zamiast wartości generowanych. Jest również możliwość określenia jawną wartość podczas aktualizacji. Aby uzyskać więcej informacji, zobacz [jawne wartości dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Jak wartość jest generowana dla dodanych i zaktualizowanych jednostek będzie zależeć od używanego dostawcy bazy danych. Dostawcy baz danych może automatycznie skonfigurować Generowanie wartości dla niektórych typów właściwości, podczas gdy inne będą wymagają ręcznego konfigurowania, jak jest generowany wartość.
> 
> Na przykład w przypadku korzystania z programu SQL Server `byte[]` właściwości, które są ustawione, tak jak w dodać lub zaktualizować i oznaczone jako tokeny współbieżności będzie instalacji z użyciem `rowversion` typ danych — tak, aby wartości, zostanie wygenerowany w bazie danych. Jednak jeśli określono `DateTime` właściwość jest generowany na dodać lub zaktualizować, a następnie należy skonfigurować sposób wartości do wygenerowania. Jednym ze sposobów, aby to zrobić, to aby skonfigurować domyślną wartość `GETDATE()` (zobacz [wartości domyślne](relational/default-values.md)) do generowania wartości dla nowych wierszy. Następnie można użyć wyzwalacza bazy danych do generowania wartości podczas aktualizacji (na przykład następujący wyzwalacz przykład).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją kluczy podstawowych innych niż złożone typu short, int, long lub identyfikator Guid będzie instalacji mogą mieć wartości wygenerowano Dodaj. Wszystkie pozostałe właściwości będzie Instalatora z Generowanie nie wartość.

## <a name="data-annotations"></a>Adnotacje danych

### <a name="no-value-generation-data-annotations"></a>Generowanie nie wartość (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Wartość wygenerowano Dodaj (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Dzięki temu wystarczy wiedzieć, że wartości są generowane dla jednostek dodano, nie gwarantuje, że EF skonfiguruje konkretny mechanizm do generowania wartości EF. Zobacz [Dodaj wartość wygenerowano](#value-generated-on-add) sekcji, aby uzyskać więcej informacji.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Dodaj wartość wygenerowano lub aktualizacji (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dzięki temu wystarczy wiedzieć, że wartości są generowane dla jednostek dodane lub zaktualizowane, nie gwarantuje, że EF skonfiguruje konkretny mechanizm do generowania wartości EF. Zobacz [wartość wygenerowano dodania lub zaktualizowania](#value-generated-on-add-or-update) sekcji, aby uzyskać więcej informacji.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwia zmienianie wzorca Generowanie wartości danej właściwości.

### <a name="no-value-generation-fluent-api"></a>Generowanie nie wartość (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Wartość wygenerowano Dodaj (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` po prostu umożliwia EF wiedzieć, że wartości są generowane dla jednostek dodano, nie gwarantuje, że EF skonfiguruje konkretny mechanizm do generowania wartości.  Zobacz [Dodaj wartość wygenerowano](#value-generated-on-add) sekcji, aby uzyskać więcej informacji.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Dodaj wartość wygenerowano lub aktualizacji (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dzięki temu wystarczy wiedzieć, że wartości są generowane dla jednostek dodane lub zaktualizowane, nie gwarantuje, że EF skonfiguruje konkretny mechanizm do generowania wartości EF. Zobacz [wartość wygenerowano dodania lub zaktualizowania](#value-generated-on-add-or-update) sekcji, aby uzyskać więcej informacji.
