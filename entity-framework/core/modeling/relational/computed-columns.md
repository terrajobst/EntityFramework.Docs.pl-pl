---
title: Kolumny obliczane — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993956"
---
# <a name="computed-columns"></a>Kolumny obliczane

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Kolumna obliczana jest kolumna, której wartość jest obliczana w bazie danych. Kolumna obliczana można użyć innych kolumn w tabeli można obliczyć jej wartość.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją kolumnach obliczanych nie są tworzone w modelu.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować przy użyciu adnotacji danych kolumn obliczanych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API służy do określenia, czy właściwość powinna być zamapowana z kolumną obliczaną.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
