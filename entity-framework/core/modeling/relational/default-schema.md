---
title: Domyślny schemat - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054188"
---
# <a name="default-schema"></a>Schemat domyślny

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Schemat domyślny jest schemat bazy danych, które obiekty zostaną utworzone w przypadku schemat nie jest jawnie skonfigurowany dla tego obiektu.

## <a name="conventions"></a>Konwencje

Według Konwencji dostawcy bazy danych będzie wybierz najbardziej odpowiedni schemat domyślny. Na przykład Microsoft SQL Server będzie używać `dbo` schematu i SQLite nie będzie używać schematu (ponieważ schematy nie są obsługiwane w SQLite).

## <a name="data-annotations"></a>Adnotacji danych

Nie można ustawić domyślnego schematu za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

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
