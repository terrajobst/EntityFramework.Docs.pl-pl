---
title: Kolumny obliczane - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054071"
---
# <a name="computed-columns"></a><span data-ttu-id="a6ed9-102">Kolumny obliczane</span><span class="sxs-lookup"><span data-stu-id="a6ed9-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="a6ed9-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a6ed9-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="a6ed9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a6ed9-105">Kolumna obliczana jest kolumna, której wartość jest obliczana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="a6ed9-106">Kolumny obliczanej służy do obliczania wartości innych kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="a6ed9-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="a6ed9-107">Conventions</span></span>

<span data-ttu-id="a6ed9-108">Konwencja kolumnach obliczanych nie są tworzone w modelu.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a6ed9-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="a6ed9-109">Data Annotations</span></span>

<span data-ttu-id="a6ed9-110">Nie można skonfigurować kolumny obliczane przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a6ed9-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="a6ed9-111">Fluent API</span></span>

<span data-ttu-id="a6ed9-112">Aby określić właściwości powinny być mapowane na kolumny obliczanej, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a6ed9-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
