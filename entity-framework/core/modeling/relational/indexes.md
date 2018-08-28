---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993220"
---
# <a name="indexes"></a>Indeksy

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Indeks w relacyjnej bazie danych jest mapowany na tę samą koncepcję jako indeks w obszarach podstawowych programu Entity Framework.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją indeksy są nazywane `IX_<type name>_<property name>`. Dla indeksów złożonych `<property name>` staje się listę nazw właściwości oddzielonych znakiem podkreślenia.

## <a name="data-annotations"></a>Adnotacje danych

Indeksy nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Można również określić filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Po dodaniu przy użyciu dostawcy programu SQL Server EF 'IS NOT NULL' filtrować wszystkie kolumny dopuszczające wartość null, które są częścią unikatowego indeksu. Aby zastąpić tę Konwencję, możesz podać `null` wartość.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
