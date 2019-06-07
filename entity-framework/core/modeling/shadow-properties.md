---
title: Właściwości w tle — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749705"
---
# <a name="shadow-properties"></a>Właściwości w tle

Właściwości w tle są właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu platformy EF Core. Wartość i stan tych właściwości jest obsługiwane wyłącznie w śledzenie zmian.

Właściwości w tle są przydatne w przypadku danych w bazie danych, które nie powinny zostać ujawnione w typach zamapowanego jednostki. W większości przypadków są one używane dla właściwości klucza obcego, których relację między dwiema jednostkami jest reprezentowany przez wartość klucza obcego w bazie danych, lecz relację odbywa się na typy jednostek przy użyciu właściwości nawigacji między typami encji.

Wartości właściwości w tle, które mogą być uzyskane i zmienić za pomocą `ChangeTracker` interfejsu API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Właściwości w tle mogą być przywoływane w zapytaniach LINQ za pośrednictwem `EF.Property` metody statycznej.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konwencje

Właściwości w tle mogą być tworzone przez Konwencję, gdy relacji został odnaleziony, ale nie właściwość klucza obcego znajduje się w klasie jednostki zależne. W takim przypadku zostaną wprowadzone właściwości klucza obcego w tle. Właściwość klucza obcego w tle będą miały nazwę nadaną `<navigation property name><principal key property name>` (nawigacji na jednostki zależne wskazuje główną jednostki, używany na potrzeby nazywania). Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, a następnie po prostu nazwą będzie `<principal key property name>`. Jeśli nie ma właściwości nawigacji jednostki zależne, Nazwa Typ podmiotu zabezpieczeń jest używana w tym miejscu.

Na przykład w poniższym fragmencie kodu spowoduje `BlogId` właściwości w tle są wprowadzane do `Post` jednostki.

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

## <a name="data-annotations"></a>Adnotacje danych

Nie można utworzyć właściwości w tle przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie właściwości w tle. Gdy wywołujesz przeciążenie ciągu `Property` można połączyć w łańcuch dowolne wywołania konfiguracji, jak w przypadku innych właściwości.

Jeśli nazwa dostarczona do `Property` metody jest zgodna z nazwą istniejącej właściwości (właściwość w tle lub jeden zdefiniowany w klasie jednostek), a następnie kod służy do konfigurowania właściwości istniejących, zamiast wprowadzać nowe właściwości w tle.

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
