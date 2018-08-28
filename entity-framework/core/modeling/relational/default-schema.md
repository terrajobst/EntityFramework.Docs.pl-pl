---
title: Schemat domyślny - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995369"
---
# <a name="default-schema"></a>Schemat domyślny

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Schemat domyślny jest schemat bazy danych, które obiekty zostaną utworzone w, jeśli schemat nie jest jawnie skonfigurowany dla tego obiektu.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją dostawcy bazy danych będzie wybrać najbardziej odpowiedni schemat domyślny. Na przykład Microsoft SQL Server będzie używać `dbo` bazy danych SQLite i schematu nie będzie używać schematu (ponieważ schematy nie są obsługiwane w SQLite).

## <a name="data-annotations"></a>Adnotacje danych

Nie można ustawić domyślny schemat przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Aby określić domyślny schemat, można użyć interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
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
