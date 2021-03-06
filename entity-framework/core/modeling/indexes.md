---
title: Indeksy — EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416396"
---
# <a name="indexes"></a>Indeksy

Indeksy są typowymi koncepcjami w wielu magazynach danych. Chociaż ich implementacja w magazynie danych może się różnić, są one używane do wykonywania wyszukiwania na podstawie kolumny (lub zestawu kolumn) bardziej wydajne.

Nie można tworzyć indeksów przy użyciu adnotacji danych. Możesz użyć interfejsu API Fluent, aby określić indeks w jednej kolumnie w następujący sposób:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

Można również określić indeks w więcej niż jednej kolumnie:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> Zgodnie z Konwencją indeks jest tworzony we wszystkich właściwościach (lub zestawach właściwości), które są używane jako klucz obcy.
>
> EF Core obsługuje tylko jeden indeks według odrębnego zestawu właściwości. Jeśli używasz interfejsu API Fluent do skonfigurowania indeksu dla zestawu właściwości, który ma już zdefiniowany indeks, w ramach Konwencji lub poprzedniej konfiguracji, zmienisz definicję tego indeksu. Jest to przydatne, jeśli chcesz skonfigurować indeks, który został utworzony przez Konwencję.

## <a name="index-uniqueness"></a>Unikatowość indeksu

Domyślnie indeksy nie są unikatowe: wiele wierszy może mieć te same wartości dla zestawu kolumn indeksu. Indeks można zmienić w następujący sposób:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

Próba wstawienia więcej niż jednej jednostki z tymi samymi wartościami dla zestawu kolumn indeksu spowoduje zgłoszenie wyjątku.

## <a name="index-name"></a>Nazwa indeksu

Według Konwencji indeksy utworzone w relacyjnej bazie danych mają nazwę `IX_<type name>_<property name>`. W przypadku indeksów złożonych `<property name>` stać się rozdzielaną podkreśleniem listą nazw właściwości.

Aby ustawić nazwę indeksu utworzonego w bazie danych, można użyć interfejsu API Fluent:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>Filtr indeksu

Niektóre relacyjne bazy danych umożliwiają określenie indeksu filtrowanego lub częściowego. Pozwala to na indeksowanie tylko podzbioru wartości kolumny, zmniejszenie rozmiaru indeksu i zwiększenie wydajności i miejsca na dysku. Więcej informacji o SQL Server filtrowanych indeksów [znajduje się w dokumentacji](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).

Korzystając z interfejsu API Fluent, można określić filtr dla indeksu, który jest dostarczany jako wyrażenie SQL:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

W przypadku używania dostawcy SQL Server Dr dodaje filtr `'IS NOT NULL'` dla wszystkich kolumn dopuszczających wartości null, które są częścią unikatowego indeksu. Aby zastąpić tę Konwencję, możesz podać wartość `null`.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Uwzględnione kolumny

Niektóre relacyjne bazy danych umożliwiają skonfigurowanie zestawu kolumn, które znajdują się w indeksie, ale nie są częścią swojego klucza. Może to znacząco poprawić wydajność zapytań, gdy wszystkie kolumny w zapytaniu są uwzględniane w indeksie jako kolumny Key lub nonkey, ponieważ sama tabela nie jest potrzebna. Aby uzyskać więcej informacji na temat SQL Server uwzględnionych kolumn, [zapoznaj się z dokumentacją](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).

W poniższym przykładzie kolumna `Url` jest częścią klucza indeksu, dlatego każde filtrowanie zapytań w tej kolumnie może korzystać z indeksu. Ponadto zapytania uzyskujące dostęp tylko do `Title` i `PublishedOn` kolumny nie będą miały dostępu do tabeli i będą działać bardziej wydajniej:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
