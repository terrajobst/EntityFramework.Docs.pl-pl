---
title: Klucze podstawowe - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998071"
---
# <a name="primary-keys"></a><span data-ttu-id="f682a-102">Klucze podstawowe</span><span class="sxs-lookup"><span data-stu-id="f682a-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="f682a-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f682a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f682a-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="f682a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f682a-105">Wprowadzono ograniczenie klucza podstawowego klucza dla każdego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="f682a-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="f682a-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f682a-106">Conventions</span></span>

<span data-ttu-id="f682a-107">Zgodnie z Konwencją, klucz podstawowy w bazie danych będą miały nazwę nadaną `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="f682a-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f682a-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f682a-108">Data Annotations</span></span>

<span data-ttu-id="f682a-109">Konkretnych aspektów relacyjnej bazy danych klucza podstawowego można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="f682a-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f682a-110">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="f682a-110">Fluent API</span></span>

<span data-ttu-id="f682a-111">Interfejs Fluent API umożliwiają skonfigurowanie nazwę ograniczenia klucza podstawowego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f682a-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
