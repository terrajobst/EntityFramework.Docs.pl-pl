---
title: Omówienie rdzenia struktury jednostki — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416847"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core to lekka, rozszerzalna, [otwarta](https://github.com/aspnet/EntityFrameworkCore) i wieloplatformowa wersja popularnej technologii dostępu do danych entity framework.

EF Core może służyć jako maper obiekt-relacyjne (O/RM), umożliwiając deweloperom platformy .NET do pracy z bazą danych przy użyciu obiektów .NET i eliminując potrzebę większości kodu dostępu do danych, które zwykle muszą napisać.

EF Core obsługuje wiele aparatów baz danych, zobacz [dostawców baz danych, aby](providers/index.md) uzyskać szczegółowe informacje.

## <a name="the-model"></a>The Model

Za pomocą EF Core dostęp do danych jest wykonywany przy użyciu modelu. Model składa się z klas jednostek i obiektu kontekstu, który reprezentuje sesję z bazą danych, umożliwiając wykonywanie zapytań i zapisywanie danych. Zobacz [Tworzenie modelu,](modeling/index.md) aby dowiedzieć się więcej.

Można wygenerować model z istniejącej bazy danych, kod ręczny modelu, aby dopasować bazę danych lub użyć [ef migracji](managing-schemas/migrations/index.md) do utworzenia bazy danych z modelu, a następnie rozwijać go w miarę zmian modelu w czasie.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Wykonywanie zapytania

Wystąpienia klas jednostek są pobierane z bazy danych przy użyciu zintegrowanej kwerendy języka (LINQ). Aby dowiedzieć się więcej, zobacz [Wyszukiwanie danych.](querying/index.md)

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Zapisywanie danych

Dane są tworzone, usuwane i modyfikowane w bazie danych przy użyciu wystąpień klas jednostek. Aby dowiedzieć się więcej, zobacz [Zapisywanie danych.](saving/index.md)

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Następne kroki

Aby zapoznać się z samouczkami wprowadzającymi, zobacz [Wprowadzenie do programu Entity Framework Core](get-started/index.md).
