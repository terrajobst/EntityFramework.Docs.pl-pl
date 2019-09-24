---
title: Domyślny schemat — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197133"
---
# <a name="default-schema"></a>Schemat domyślny

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Domyślny schemat to schemat bazy danych, w którym obiekty zostaną utworzone w przypadku, gdy schemat nie jest jawnie skonfigurowany dla tego obiektu.

## <a name="conventions"></a>Konwencje

Według Konwencji dostawca bazy danych wybierze najbardziej odpowiedni domyślny schemat. Na przykład Microsoft SQL Server będzie używać `dbo` schematu, a program SQLite nie użyje schematu (ponieważ schematy nie są obsługiwane w programie SQLite).

## <a name="data-annotations"></a>Adnotacje danych

Nie można ustawić domyślnego schematu przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby określić domyślny schemat.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
