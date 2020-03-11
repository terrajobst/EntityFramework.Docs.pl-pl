---
title: Przegląd Entity Framework Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416847"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core to [lekkie, rozszerzalne i](https://github.com/aspnet/EntityFrameworkCore) wieloplatformowe wersje popularnej Entity Framework technologii dostępu do danych.

EF Core może służyć jako maper obiektowo relacyjny (O/RM), dzięki czemu deweloperzy platformy .NET mogą pracować z bazą danych, używając obiektów platformy .NET i eliminując potrzebę pisania większości kodu dostępu do danych.

EF Core obsługuje wiele aparatów baz danych, zobacz [dostawcy bazy danych](providers/index.md) , aby uzyskać szczegółowe informacje.

## <a name="the-model"></a>Model

W przypadku EF Core dostęp do danych odbywa się przy użyciu modelu. Model składa się z klas jednostek i obiektu kontekstu, który reprezentuje sesję z bazą danych, co pozwala na wykonywanie zapytań i zapisywanie danych. Zobacz [Tworzenie modelu](modeling/index.md) , aby dowiedzieć się więcej.

Można wygenerować model z istniejącej bazy danych, ręcznie nakodować model do bazy danych lub użyć [migracji EF](managing-schemas/migrations/index.md) do utworzenia bazy danych z modelu, a następnie przydzielenia go wraz ze zmianą modelu w miarę upływu czasu.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Wykonywanie zapytania

Wystąpienia klas jednostek są pobierane z bazy danych przy użyciu języka Language Integrated Query (LINQ). Zobacz [wykonywanie zapytań o dane](querying/index.md) , aby dowiedzieć się więcej.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Zapisywanie danych

Dane są tworzone, usuwane i modyfikowane w bazie danych za pomocą wystąpień klas jednostek. Aby dowiedzieć się więcej, zobacz [Zapisywanie danych](saving/index.md) .

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Następne kroki

Aby zapoznać się z samouczkami wprowadzającymi, zobacz [wprowadzenie z Entity Framework Core](get-started/index.md).
