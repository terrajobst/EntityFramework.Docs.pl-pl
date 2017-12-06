---
title: "Indeksy — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="9f7f1-102">Indeksy</span><span class="sxs-lookup"><span data-stu-id="9f7f1-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="9f7f1-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9f7f1-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="9f7f1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9f7f1-105">Mapuje indeksu relacyjnej bazy danych do tego samego pojęcia jako indeks w podstawowe programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="9f7f1-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="9f7f1-106">Conventions</span></span>

<span data-ttu-id="9f7f1-107">Konwencja, indeksy są nazywane `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="9f7f1-108">Złożone indeksów `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9f7f1-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="9f7f1-109">Data Annotations</span></span>

<span data-ttu-id="9f7f1-110">Indeksy nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9f7f1-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="9f7f1-111">Fluent API</span></span>

<span data-ttu-id="9f7f1-112">Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="9f7f1-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
