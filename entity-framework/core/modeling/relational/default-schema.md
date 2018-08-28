---
title: Schemat domyślny - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995369"
---
# <a name="default-schema"></a><span data-ttu-id="ee42d-102">Schemat domyślny</span><span class="sxs-lookup"><span data-stu-id="ee42d-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="ee42d-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ee42d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ee42d-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="ee42d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ee42d-105">Schemat domyślny jest schemat bazy danych, które obiekty zostaną utworzone w, jeśli schemat nie jest jawnie skonfigurowany dla tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="ee42d-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="ee42d-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ee42d-106">Conventions</span></span>

<span data-ttu-id="ee42d-107">Zgodnie z Konwencją dostawcy bazy danych będzie wybrać najbardziej odpowiedni schemat domyślny.</span><span class="sxs-lookup"><span data-stu-id="ee42d-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="ee42d-108">Na przykład Microsoft SQL Server będzie używać `dbo` bazy danych SQLite i schematu nie będzie używać schematu (ponieważ schematy nie są obsługiwane w SQLite).</span><span class="sxs-lookup"><span data-stu-id="ee42d-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ee42d-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="ee42d-109">Data Annotations</span></span>

<span data-ttu-id="ee42d-110">Nie można ustawić domyślny schemat przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="ee42d-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ee42d-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="ee42d-111">Fluent API</span></span>

<span data-ttu-id="ee42d-112">Aby określić domyślny schemat, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ee42d-112">You can use the Fluent API to specify a default schema.</span></span>

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
