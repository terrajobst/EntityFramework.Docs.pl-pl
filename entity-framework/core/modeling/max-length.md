---
title: Maksymalna długość numeru PIN — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996194"
---
# <a name="maximum-length"></a>Maksymalna długość

Konfigurowanie maksymalnej długości stanowi wskazówkę z magazynem danych o typie danych odpowiednich dla danej właściwości. Maksymalna długość ma zastosowanie tylko do tablicy typów danych, takich jak `string` i `byte[]`.

> [!NOTE]  
> Platformy Entity Framework wykonaj wszelkie weryfikacji o maksymalnej długości przed przekazaniem do dostawcy. Jest magazynu dostawcy lub danych, sprawdź, czy jest to odpowiednie. Na przykład gdy przeznaczonych dla programu SQL Server, która przekracza maksymalną długość spowodują wyjątek jako typ danych kolumny źródłowej nie zezwoli nadmiarowych danych mają być przechowywane.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją pozostało do dostawcy bazy danych, aby wybrać odpowiedni typ danych właściwości. Dla właściwości o długości dostawca bazy danych zazwyczaj wybierz typ danych, który umożliwia najdłuższy długość danych. Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować maksymalną długość dla właściwości. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie maksymalnej długości dla właściwości. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
