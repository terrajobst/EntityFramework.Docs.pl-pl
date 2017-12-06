---
title: "Domyślny schemat - EF Core"
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
---
# <a name="default-schema"></a><span data-ttu-id="7d2c3-102">Schemat domyślny</span><span class="sxs-lookup"><span data-stu-id="7d2c3-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="7d2c3-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="7d2c3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7d2c3-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="7d2c3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7d2c3-105">Schemat domyślny jest schemat bazy danych, które obiekty zostaną utworzone w przypadku schemat nie jest jawnie skonfigurowany dla tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="7d2c3-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="7d2c3-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7d2c3-106">Conventions</span></span>

<span data-ttu-id="7d2c3-107">Według Konwencji dostawcy bazy danych będzie wybierz najbardziej odpowiedni schemat domyślny.</span><span class="sxs-lookup"><span data-stu-id="7d2c3-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="7d2c3-108">Na przykład Microsoft SQL Server będzie używać `dbo` schematu i SQLite nie będzie używać schematu (ponieważ schematy nie są obsługiwane w SQLite).</span><span class="sxs-lookup"><span data-stu-id="7d2c3-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7d2c3-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="7d2c3-109">Data Annotations</span></span>

<span data-ttu-id="7d2c3-110">Nie można ustawić domyślnego schematu za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="7d2c3-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7d2c3-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="7d2c3-111">Fluent API</span></span>

<span data-ttu-id="7d2c3-112">Aby określić domyślny schemat, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7d2c3-112">You can use the Fluent API to specify a default schema.</span></span>

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
