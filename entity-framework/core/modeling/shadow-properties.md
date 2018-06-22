---
title: Właściwości — podstawowe EF w tle
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054563"
---
# <a name="shadow-properties"></a>Właściwości w tle

Właściwości cienia są właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu podstawowej EF. Wartość i stan tych właściwości są obsługiwane wyłącznie w śledzenie zmian.

Właściwości w tle są przydatne, gdy brak danych w bazie danych, które nie powinny zostać ujawnione w typach jednostek mapowane. Są one najczęściej używane dla właściwości klucza obcego, gdzie relację między dwiema jednostkami jest reprezentowany przez wartość klucza obcego w bazie danych, ale relacji jest zarządzana w przypadku typów jednostek przy użyciu właściwości nawigacji między typami jednostek.

Wartości właściwości w tle mogą być uzyskane i zmieniać za pomocą `ChangeTracker` interfejsu API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Właściwości w tle może być przywoływany w zapytań LINQ za pośrednictwem `EF.Property` metody statycznej.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konwencje

Właściwości w tle można tworzyć przez Konwencję, gdy zostanie wykryta relacji, ale ma właściwości klucza obcego znajduje się w klasie jednostki zależnej. W takim przypadku zostaną wprowadzone tle właściwości kluczy obcych. Właściwość klucza obcego cień będzie miała nazwę `<navigation property name><principal key property name>` (nawigacji na jednostki zależnej wskazuje główną jednostki, jest używany dla nazwy). Jeśli nazwa główna właściwości klucza zawierają nazwę właściwości nawigacji, a następnie nazwę będzie działać `<principal key property name>`. Jeśli nie ma właściwości nawigacji na jednostki zależnej, nazwa typu podmiotu zabezpieczeń jest używany w jego miejscu.

Na przykład w poniższym fragmencie kodu spowoduje `BlogId` wprowadzeniem do właściwości tle `Post` jednostki.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Adnotacji danych

Nie można utworzyć właściwości tle przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Można skonfigurować właściwości w tle, można użyć interfejsu API Fluent. Po wywołaniu przeciążenia ciąg `Property` można łańcucha żadnego z tych wywołań konfiguracji jak dla innych właściwości.

Jeśli nazwa dostarczona do `Property` metoda zgodna z nazwą istniejącej właściwości (właściwość tle lub zdefiniowanych w klasie jednostek), a następnie kod służy do konfigurowania tego istniejącej właściwości, zamiast wprowadzać nową właściwość w tle.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
