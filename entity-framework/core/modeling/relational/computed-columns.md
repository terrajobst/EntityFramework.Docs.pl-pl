---
title: Kolumny obliczane — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197235"
---
# <a name="computed-columns"></a><span data-ttu-id="c33ed-102">Kolumny obliczane</span><span class="sxs-lookup"><span data-stu-id="c33ed-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="c33ed-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="c33ed-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c33ed-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="c33ed-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c33ed-105">Kolumna obliczana to kolumna, której wartość jest obliczana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c33ed-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="c33ed-106">Kolumna obliczana może użyć innych kolumn w tabeli do obliczenia jej wartości.</span><span class="sxs-lookup"><span data-stu-id="c33ed-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="c33ed-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c33ed-107">Conventions</span></span>

<span data-ttu-id="c33ed-108">Według Konwencji kolumny obliczane nie są tworzone w modelu.</span><span class="sxs-lookup"><span data-stu-id="c33ed-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c33ed-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="c33ed-109">Data Annotations</span></span>

<span data-ttu-id="c33ed-110">Kolumn obliczanych nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="c33ed-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c33ed-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="c33ed-111">Fluent API</span></span>

<span data-ttu-id="c33ed-112">Możesz użyć interfejsu API Fluent, aby określić, że właściwość powinna być mapowana na kolumnę obliczaną.</span><span class="sxs-lookup"><span data-stu-id="c33ed-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
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
