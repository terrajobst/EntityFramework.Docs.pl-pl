---
title: Klucze podstawowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196947"
---
# <a name="primary-keys"></a><span data-ttu-id="91044-102">Klucze podstawowe</span><span class="sxs-lookup"><span data-stu-id="91044-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="91044-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="91044-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="91044-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="91044-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="91044-105">Dla klucza każdego typu jednostki wprowadzono ograniczenie PRIMARY KEY.</span><span class="sxs-lookup"><span data-stu-id="91044-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="91044-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="91044-106">Conventions</span></span>

<span data-ttu-id="91044-107">Zgodnie z Konwencją klucz podstawowy w bazie danych ma nazwę `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="91044-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="91044-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="91044-108">Data Annotations</span></span>

<span data-ttu-id="91044-109">Brak specyficznych dla relacyjnych baz danych klucza podstawowego można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="91044-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="91044-110">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="91044-110">Fluent API</span></span>

<span data-ttu-id="91044-111">Możesz użyć interfejsu API Fluent, aby skonfigurować nazwę ograniczenia PRIMARY KEY w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="91044-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
