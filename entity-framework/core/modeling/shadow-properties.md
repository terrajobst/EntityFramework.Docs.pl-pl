---
title: Właściwości cienia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197705"
---
# <a name="shadow-properties"></a>Właściwości w tle

Właściwości cienia to właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu EF Core. Wartość i stan tych właściwości są utrzymywane wyłącznie w monitorze zmian.

Właściwości cienia są przydatne, gdy w bazie danych znajdują się dane, które nie powinny być uwidocznione w mapowanych typach obiektów. Są one najczęściej używane w przypadku właściwości klucza obcego, gdzie relacja między dwiema jednostkami jest reprezentowana przez wartość klucza obcego w bazie danych, ale relacja jest zarządzana w typach jednostek przy użyciu właściwości nawigacji między typami jednostek.

Wartości właściwości cienia można uzyskać i zmienić za pomocą `ChangeTracker` interfejsu API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Właściwości cienia można przywoływać w zapytaniach `EF.Property` LINQ za pomocą metody statycznej.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konwencje

Właściwości cienia można utworzyć według Konwencji, gdy zostanie wykryta relacja, ale w klasie jednostki zależnej nie znaleziono żadnej właściwości klucza obcego. W takim przypadku zostanie wprowadzona właściwość klucza obcego cienia. Właściwość klucza obcego cienia zostanie nazwana `<navigation property name><principal key property name>` (Nawigacja w jednostce zależnej, która wskazuje podmiotowi głównemu, jest używana do nazewnictwa). Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, nazwa zostanie `<principal key property name>`wydana. Jeśli w jednostce zależnej nie ma właściwości nawigacji, w jej miejscu zostanie użyta nazwa typu podmiotu zabezpieczeń.

Na przykład następująca lista kodu spowoduje, że właściwość `BlogId` Shadow zostanie wprowadzona `Post` do jednostki.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

Właściwości cienia nie mogą być tworzone za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Aby skonfigurować właściwości cienia, można użyć interfejsu API Fluent. Po wywołaniu przeciążania `Property` ciągu można utworzyć łańcuch dowolnego wywołania konfiguracji dla innych właściwości.

Jeśli nazwa przekazywana do `Property` metody jest zgodna z nazwą istniejącej właściwości (właściwości cienia lub jednej zdefiniowanej w klasie jednostki), wówczas kod skonfiguruje istniejącą właściwość zamiast wprowadzać nową właściwość Shadow.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
