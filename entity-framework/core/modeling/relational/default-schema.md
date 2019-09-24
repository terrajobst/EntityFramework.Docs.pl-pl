---
title: Domyślny schemat — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197133"
---
# <a name="default-schema"></a><span data-ttu-id="74a8b-102">Schemat domyślny</span><span class="sxs-lookup"><span data-stu-id="74a8b-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="74a8b-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="74a8b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="74a8b-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="74a8b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="74a8b-105">Domyślny schemat to schemat bazy danych, w którym obiekty zostaną utworzone w przypadku, gdy schemat nie jest jawnie skonfigurowany dla tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="74a8b-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="74a8b-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="74a8b-106">Conventions</span></span>

<span data-ttu-id="74a8b-107">Według Konwencji dostawca bazy danych wybierze najbardziej odpowiedni domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="74a8b-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="74a8b-108">Na przykład Microsoft SQL Server będzie używać `dbo` schematu, a program SQLite nie użyje schematu (ponieważ schematy nie są obsługiwane w programie SQLite).</span><span class="sxs-lookup"><span data-stu-id="74a8b-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="74a8b-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="74a8b-109">Data Annotations</span></span>

<span data-ttu-id="74a8b-110">Nie można ustawić domyślnego schematu przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="74a8b-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="74a8b-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="74a8b-111">Fluent API</span></span>

<span data-ttu-id="74a8b-112">Możesz użyć interfejsu API Fluent, aby określić domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="74a8b-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
