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
# <a name="computed-columns"></a><span data-ttu-id="7d898-102">Kolumny obliczane</span><span class="sxs-lookup"><span data-stu-id="7d898-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="7d898-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="7d898-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7d898-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="7d898-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7d898-105">Kolumna obliczana jest kolumna, której wartość jest obliczana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7d898-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="7d898-106">Kolumna obliczana można użyć innych kolumn w tabeli można obliczyć jej wartość.</span><span class="sxs-lookup"><span data-stu-id="7d898-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="7d898-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7d898-107">Conventions</span></span>

<span data-ttu-id="7d898-108">Zgodnie z Konwencją kolumnach obliczanych nie są tworzone w modelu.</span><span class="sxs-lookup"><span data-stu-id="7d898-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7d898-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="7d898-109">Data Annotations</span></span>

<span data-ttu-id="7d898-110">Nie można skonfigurować przy użyciu adnotacji danych kolumn obliczanych.</span><span class="sxs-lookup"><span data-stu-id="7d898-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7d898-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="7d898-111">Fluent API</span></span>

<span data-ttu-id="7d898-112">Interfejs Fluent API służy do określenia, czy właściwość powinna być zamapowana z kolumną obliczaną.</span><span class="sxs-lookup"><span data-stu-id="7d898-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
