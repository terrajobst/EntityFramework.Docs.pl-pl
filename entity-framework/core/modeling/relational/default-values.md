---
title: Wartości domyślne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655904"
---
# <a name="default-values"></a>Wartości domyślne

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Wartość domyślna kolumny to wartość, która zostanie wstawiona, jeśli zostanie wstawiony nowy wiersz, ale nie określono wartości dla tej kolumny.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją wartość domyślna nie jest skonfigurowana.

## <a name="data-annotations"></a>Adnotacje danych

Nie można ustawić wartości domyślnej przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby określić wartość domyślną dla właściwości.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Można również określić fragment SQL, który jest używany do obliczania wartości domyślnej.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
